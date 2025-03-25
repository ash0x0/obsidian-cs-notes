A load balancer evenly distributes incoming traffic among web servers that are defined in a load-balanced set. 

![[Pasted image 20250305215000.png]]
# Operations

- Users connect to the public IP of the load balancer directly.
- Web servers are unreachable directly by clients anymore. 
- For better security, private IPs are used for communication between servers. 
	- A private IP is an IP address reachable only between servers in the same network; however, it is unreachable over the internet. 
- The load balancer communicates with web servers through private IPs.

# Fail-over

- If server 1 goes offline
	- All the traffic will be routed to server 2.
	- This prevents the website from going offline.
	- We will also add a new healthy web server to the server pool to balance the load.
- If the website traffic grows rapidly, and two servers are not enough to handle the traffic
	- Add more servers to the web server pool.
	- Load balancer automatically starts to send requests to them. 


]]