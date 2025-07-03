---
sources:
  - https://www.youtube.com/watch?v=gNQ9-kgyHfo
---
The goal is to deliver a design in production with considerations of **deployment, scaling, failures, availability, and performance**. Be prepared to discuss **latency and concurrency**.
# Preparation

- Software components and their best design
- Datastores and their design concepts
- Decision making and business logic (APIs or processes)
- Distributed systems
- Service-Oriented Architecture (SOA)
- N-tiered software architecture
- Practice drawing your system design by hand and be prepared to whiteboard
# Evaluation Criteria

## Ask clarifying questions to scope-down and define requirements

- As a designer you should start with the (primary) customer and work backwards:
    - Who are you designing the system for and why?
    - What expectations do they have in terms of functionality?
    - What things would a customer just assume will be in the system but they may not think about in the forefront of their minds? (e.g. it'll be fast and secure)
    - What happens if we become hyper-popular with customers? What does 2x growth look like? Or 10x? And how would that influence the design? 
- Understand first what problem your system is supposed to solve.
- See the interviewer as the customer, requirements might be intentionally vague, and she/he can give you clarifications. 
- Write/raise the requirements or assumptions you are making, and base your design on them.
## Create a design for a system that fulfills captured requirements (constraints, scalability, maintenance)

- Non-functional requirements (load, load distribution, security), ways of interacting with the system (user access, scheduled processes, synchronous/asynchronous communication), and data flow.
- Spend majority of time on the Critical Requirements identified and on the core functionality.
- Design the solution for scale, i.e. would more transactions seamlessly work.
- Explicitly mention all the assumptions that are being made.
- Justify the design choices that are made.
## Design for operational performance and plans for failure and can measure the results (e.g., metrics)

- Think about operations and resilience in your design.
	- What are the key business and technical metrics for the system? How will someone use that to identify problems?
	- What are the potential bottlenecks or pain points?
	- What failure points exist?
	- What redundancy can we build in to reduce single points of failure?
	- How does someone get logs and debug the system?
## Identify potential shortcomings and tradeoffs with different designs (performance, fault, tolerance, dependencies)

- Describe the trade-offs of different approaches, choose one and explain why.
- We are looking for a high level understanding of how to design a system and deep knowledge into at least one area. 
- **If the interviewer asks you to dive deep into something you are unfamiliar with, tell them, and suggest some other area you are familiar with and dive into that.**
## Secondary Criteria

### Scaling

- [[Cache]]
- [[Load Balancing]]
- [[Non-Relational Database]]
- [[Microservices]]
- [[Sharding]]
## Fault tolerance

This translates to the Customer Obsession Leadership Principle.

Ensure that when dependencies are failing the system still works. Frugal redundancy is also a key criteria.
### Additional Criteria

- Designs for operational performance, plans for failure, and measures the results (e.g., metrics)
- Considers both technical and business metrics, as well as data quality
- Thinks about how to monitor for performance and catch potential problems
- Considers resiliency and making things easier for operators

# Framework

1. Data
	1. What are the types or categories of data?
	2. How long do we keep that data?
	3. What kind of processing do we need to do on the data?
	4. Are there any special requirements to store the data?
	5. How is the data added? who adds it?
	6. What are the necessary properties of this data and its categories?
	7. What are the properties we display to the users?
	8. What are the main entities we have in the datastore?
		1. What objects do we want to capture?
		2. What are the properties of each?
		3. What actions can users take?
		4. What transactions can be made?
2. Scale
	1. How many users?
	2. How many transactions daily? per second?
3. Performance
	1. What criteria do we have for latency?
	2. Are the users sensitive to delays or not?
	3. Are user queries repeated often or highly custom?
4. Distribution
	1. What are the regions we're serving? one or more?
# Tips

1. Don't jump into solving the problem
	1. Ask clarifying questions about each component
2. Make this a conversation
	1. Tell the interviewer where you're going with each part and each step
3. Start where you're strongest. If you're more comfortable with frontend, start with it. If you're more comfortable with database, start with that.
4. Create the overally big picture black boxes them go into the details of each. Make it a dive layer-by-layer instead of having it be a linear deep dive from the first go through.