Availability refers to the percentage of time a system is operational and accessible when required. It's a critical metric for system reliability and user satisfaction.

# Definition

Availability = (Uptime / (Uptime + Downtime)) × 100%

# Availability Levels (Nines)

| Level | Uptime % | Downtime per Year | Downtime per Month | Common Use Case |
|-------|----------|-------------------|-------------------|-----------------|
| 90% ("one nine") | 90% | 36.5 days | 3 days | Non-critical systems |
| 99% ("two nines") | 99% | 3.65 days | 7.2 hours | Internal tools |
| 99.9% ("three nines") | 99.9% | 8.76 hours | 43.8 minutes | Standard services |
| 99.99% ("four nines") | 99.99% | 52.6 minutes | 4.38 minutes | Business-critical |
| 99.999% ("five nines") | 99.999% | 5.26 minutes | 26 seconds | Mission-critical |
| 99.9999% ("six nines") | 99.9999% | 31.5 seconds | 2.6 seconds | Life-critical systems |

# Strategies for High Availability

## 1. Redundancy

- **Active-Active**: Multiple systems handle requests simultaneously
- **Active-Passive**: Backup system takes over when primary fails
- **N+1 Redundancy**: N systems needed, N+1 deployed
- **2N Redundancy**: Double the required capacity

## 2. Load Balancing

```
Users → Load Balancer → [Server 1, Server 2, Server 3, ...]
```

Distributes traffic across multiple servers to prevent single point of failure.

## 3. Failover Mechanisms

- **Automatic Failover**: System switches to backup automatically
- **Manual Failover**: Requires human intervention
- **Graceful Degradation**: System continues with reduced functionality

## 4. Geographic Distribution

- Multi-region deployment
- Disaster recovery sites
- Content Delivery Networks (CDN)

## 5. Monitoring and Alerting

- Health checks
- Real-time monitoring
- Automated alerts
- Incident response

## 6. Database Strategies

- **Replication**: Master-slave or multi-master
- **Clustering**: Multiple database nodes
- **Backup and Recovery**: Regular backups, point-in-time recovery

# Trade-offs

## Availability vs Consistency

According to the CAP theorem, in distributed systems during network partitions, you must choose between:
- **Consistency**: All nodes see the same data
- **Availability**: Every request receives a response

Most systems choose availability for better user experience.

## Availability vs Cost

- Higher availability requires more resources
- Redundancy increases infrastructure costs
- Complex failover mechanisms need more engineering effort

## Availability vs Performance

- Multiple hops through load balancers add latency
- Synchronous replication slows writes
- Health checks consume resources

# Measuring Availability

## Service Level Agreement (SLA)

Contract specifying guaranteed uptime percentage and penalties for violations.

## Service Level Objective (SLO)

Internal target for availability, typically stricter than SLA.

## Service Level Indicator (SLI)

Actual measured availability metric.

# Common Causes of Downtime

1. **Hardware failures**: Server crashes, disk failures, network issues
2. **Software bugs**: Application errors, memory leaks
3. **Human error**: Configuration mistakes, accidental deletions
4. **Network issues**: DDoS attacks, ISP problems
5. **Dependency failures**: Third-party service outages
6. **Planned maintenance**: Updates, migrations

# Best Practices

1. **Eliminate Single Points of Failure (SPOF)**
   - Redundant servers, databases, load balancers
   - Multiple availability zones/regions

2. **Implement Health Checks**
   - Application-level checks
   - Infrastructure monitoring
   - Dependency checks

3. **Automate Recovery**
   - Auto-scaling
   - Self-healing systems
   - Circuit breakers

4. **Design for Failure**
   - Assume components will fail
   - Graceful degradation
   - Timeout and retry logic

5. **Test Disaster Recovery**
   - Regular failover drills
   - Chaos engineering
   - Load testing

6. **Monitor Everything**
   - Infrastructure metrics
   - Application metrics
   - Business metrics
   - User experience monitoring

# Example: Highly Available Web Application

```
            Internet
                |
        [Global Load Balancer]
        /                    \
    [Region 1]            [Region 2]
        |                      |
  [Load Balancer]        [Load Balancer]
   /    |    \            /    |    \
[Web] [Web] [Web]      [Web] [Web] [Web]
   \    |    /            \    |    /
  [App] [App] [App]      [App] [App] [App]
   \    |    /            \    |    /
  [DB Master]              [DB Replica]
        \___________________/
           (Replication)
```

# Availability Calculation Example

If you have:
- 3 servers, each with 99% availability
- Configured in active-active mode

Combined availability: 1 - (0.01)³ = 1 - 0.000001 = 99.9999%

# Related Topics

- [[Scalability]]
- [[Reliability]]
- [[Fault Tolerance]]
- [[Disaster Recovery]]
- [[Load Balancing]]
- [[Replication]]

# Tools and Technologies

- Load Balancers: Nginx, HAProxy, AWS ELB
- Monitoring: Prometheus, Grafana, DataDog, New Relic
- Service Mesh: Istio, Linkerd
- Database: Multi-master replication, read replicas
- Cloud: Multi-AZ/Multi-region deployments
