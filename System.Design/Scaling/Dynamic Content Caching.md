Dynamic Content Caching involves storing frequently accessed, personalized, or computed content to reduce load on backend systems and improve response times.

# Overview

Unlike static content (images, CSS, JS), dynamic content is personalized or generated based on:
- User data
- Real-time information
- Database queries
- Computational results

# Challenges with Dynamic Content

1. **Personalization**: Content differs per user
2. **Freshness**: Data must be up-to-date
3. **Variability**: Many possible variations
4. **Cache Invalidation**: Knowing when to update

# Caching Strategies

## 1. Application-Level Caching

Cache computed results in application memory.

```python
# Using Redis
def get_user_feed(user_id):
    cache_key = f"feed:{user_id}"
    
    # Try cache first
    cached_feed = redis.get(cache_key)
    if cached_feed:
        return cached_feed
    
    # Generate feed if not cached
    feed = generate_feed(user_id)
    
    # Cache for 5 minutes
    redis.setex(cache_key, 300, feed)
    return feed
```

## 2. Database Query Result Caching

Cache database query results.

```python
@cache.memoize(timeout=300)
def get_user_profile(user_id):
    return db.query("SELECT * FROM users WHERE id = ?", user_id)
```

## 3. Page Fragment Caching

Cache parts of a page that don't change often.

```html
<!-- Cache user profile (changes rarely) -->
<div>{% cache 3600 user_profile user.id %}
    {{ user.profile }}
{% endcache %}</div>

<!-- Don't cache feed (changes frequently) -->
<div>{{ user.feed }}</div>
```

## 4. Edge Caching with Personalization

Use CDN with dynamic capabilities.

```
User Request → CDN Edge
  ↓
Check cache with user-specific key
  ↓
If miss: Forward to origin
  ↓
Cache response with user key
  ↓
Return to user
```

## 5. Session-Based Caching

Cache based on session ID.

```javascript
const sessionCache = new Map();

function getUserData(sessionId) {
    if (sessionCache.has(sessionId)) {
        return sessionCache.get(sessionId);
    }
    
    const data = fetchUserData(sessionId);
    sessionCache.set(sessionId, data);
    
    // Expire after 15 minutes
    setTimeout(() => sessionCache.delete(sessionId), 900000);
    
    return data;
}
```

# Cache Invalidation Strategies

## 1. Time-Based (TTL)

Simplest approach - cache expires after time period.

```python
cache.set(key, value, ttl=300)  # 5 minutes
```

**Pros**: Simple, predictable
**Cons**: May serve stale data

## 2. Event-Based

Invalidate when underlying data changes.

```python
def update_user(user_id, new_data):
    db.update(user_id, new_data)
    # Invalidate cache
    cache.delete(f"user:{user_id}")
    cache.delete(f"feed:{user_id}")
```

**Pros**: Always fresh data
**Cons**: More complex, requires careful tracking

## 3. Write-Through

Update cache and database simultaneously.

```python
def save_user(user_id, data):
    db.save(user_id, data)
    cache.set(f"user:{user_id}", data)
```

## 4. Write-Behind (Write-Back)

Update cache immediately, database asynchronously.

```python
def save_user(user_id, data):
    cache.set(f"user:{user_id}", data)
    queue.enqueue(lambda: db.save(user_id, data))
```

## 5. Cache Stampede Prevention

Prevent multiple processes from regenerating cache simultaneously.

```python
def get_cached_value(key):
    value = cache.get(key)
    if value is not None:
        return value
    
    # Acquire lock
    with cache.lock(f"lock:{key}", timeout=10):
        # Double-check after acquiring lock
        value = cache.get(key)
        if value is not None:
            return value
        
        # Generate and cache
        value = expensive_operation()
        cache.set(key, value, ttl=300)
        return value
```

# Cache Key Design

## User-Specific Keys

```
user:123:profile
user:123:settings
user:123:feed:page:1
```

## Query-Based Keys

```
query:hash(SELECT * FROM products WHERE category='electronics')
```

## Composite Keys

```
product:456:user:123:recommendations
search:laptops:page:2:sort:price
```

# Partial Caching

Cache expensive computations, combine with fresh data.

```python
def get_product_page(product_id):
    # Cache expensive parts
    product_details = cache.get_or_compute(
        f"product:{product_id}:details",
        lambda: fetch_product_details(product_id),
        ttl=3600
    )
    
    # Get fresh data
    current_price = get_current_price(product_id)
    inventory = get_inventory(product_id)
    
    return {
        "details": product_details,
        "price": current_price,
        "inventory": inventory
    }
```

# Technologies

## In-Memory Caches

- **Redis**: Key-value store, pub/sub, Lua scripting
- **Memcached**: Simple key-value store
- **Hazelcast**: Distributed in-memory data grid

## Application Frameworks

- **Spring Cache**: Java caching abstraction
- **Django Cache**: Python web framework caching
- **Node Cache**: Node.js in-memory caching

## CDN with Dynamic Capabilities

- **Cloudflare Workers**: Edge computing
- **AWS CloudFront**: Lambda@Edge
- **Fastly**: VCL for dynamic logic

# Best Practices

1. **Cache Selectively**
   - Cache expensive operations
   - Don't cache everything

2. **Use Appropriate TTLs**
   - Shorter TTL for frequently changing data
   - Longer TTL for stable data

3. **Monitor Cache Hit Rates**
   - Aim for 80%+ hit rate
   - Adjust strategies based on metrics

4. **Handle Cache Failures**
   - Gracefully degrade if cache unavailable
   - Don't let cache failures break application

5. **Version Cache Keys**
   - Include version in key for schema changes
   - `user:v2:123:profile`

6. **Compress Large Values**
   - Save memory and network bandwidth
   - Trade CPU for memory/network

7. **Use Cache Hierarchies**
   - L1: Application memory
   - L2: Redis/Memcached
   - L3: Database query cache

# Example: News Feed

```python
class FeedCache:
    def __init__(self, redis_client):
        self.redis = redis_client
        self.ttl = 300  # 5 minutes
    
    def get_feed(self, user_id, page=1):
        cache_key = f"feed:{user_id}:page:{page}"
        
        # Try cache
        cached = self.redis.get(cache_key)
        if cached:
            return json.loads(cached)
        
        # Generate feed
        feed = self._generate_feed(user_id, page)
        
        # Cache with sliding window
        self.redis.setex(
            cache_key,
            self.ttl,
            json.dumps(feed)
        )
        
        return feed
    
    def invalidate_feed(self, user_id):
        # Clear all pages
        pattern = f"feed:{user_id}:page:*"
        keys = self.redis.keys(pattern)
        if keys:
            self.redis.delete(*keys)
    
    def _generate_feed(self, user_id, page):
        # Expensive operation
        return fetch_and_rank_posts(user_id, page)
```

# Metrics to Monitor

- Cache hit rate
- Cache miss rate
- Average response time (cached vs uncached)
- Cache memory usage
- Eviction rate
- Cache invalidation frequency

# Related Topics

- [[Caching Strategies]]
- [[Redis]]
- [[Content Delivery Network (CDN)]]
- [[Scalability]]
- [[Database Optimization]]
