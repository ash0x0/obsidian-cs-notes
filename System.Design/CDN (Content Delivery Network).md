- A CDN is a network of geographically dispersed servers used to deliver static content. 
- CDN servers cache images, videos, CSS, JavaScript files, etc.
- [[Dynamic Content Caching]] enables the caching of HTML pages based on request path, query strings, cookies, and request headers.

# Operation

- When a user visits a website, CDN server closest to the user will deliver static content.
- The URL’s domain for the specified asset is provided by the CDN provider. 
- If the CDN server does not have the assets in the cache, the CDN server requests the file from the origin, which can be a web server or online storage.
- The origin returns assets to the CDN, which includes optional HTTP header Time-to-Live (TTL), specifying how long the asset is cached.
- CDN caches the asset and returns it to User.
- The image remains cached in the CDN until the TTL expires. 
- Other users sending a new request to get the asset will get it from cache as long as the TTL has not expired.

![[Pasted image 20250305213421.png]]

![[Pasted image 20250305213500.png]]

# Considerations

## Cost

- CDNs are run by third-party providers, and you are charged for data transfers in and out of the CDN. 

## Cache Expiry

- For time-sensitive content, setting a cache expiry time is important.
- If it is too long, the content might no longer be fresh. 
- If it is too short, it can cause repeat reloading of content from origin servers to the CDN.
- Caching infrequently used assets provides no significant benefits so you should consider moving them out of the CDN. 

# Fallback

- If there is a temporary CDN outage, clients should be able to detect the problem and request resources from the origin.

## Asset Invalidation

- To remove an asset from CDN before it expires
	- Invalidate the CDN object using APIs provided by CDN vendors.
	- Use object versioning to serve a different version of the object. 
		- To version an object, you can add a parameter to the URL, such as a version number. 
		- For example, version number 2 is added to the query string: image.png?v=2.