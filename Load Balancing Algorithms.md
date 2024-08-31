# Load Balancing Algorithms
- Load balancing is the process of distributing incoming network traffic across multiple servers to ensure that no single server is overwhelmed.
- By evenly spreading the workload, load balancing aims to prevent overload on a single server, enhance performance by reducing response times and improve availability by rerouting traffic in case of server failures.
- There are several algorithms to achieve load balancing, each with its pros and cons.

## Algorithms
**1. Round Robin:** Simple and even distribution, best for homogeneous servers.

**2. Weighted Round Robin:** Distributes based on server capacity, good for heterogeneous environments.

**3. Least Connections:** Dynamically balances based on load, ideal for varying workloads.

**4. Least Response Time:** Optimizes for fastest response, best for environments with varying server performance.

**5. IP Hash:** Ensures session persistence, useful for stateful applications.

Choosing the right load balancing algorithm depends on the specific needs and characteristics of your system, including server capabilities, workload distribution, and performance requirements.

### Algorithm 1: Round Robin

**How it Works:**
- A request is sent to the first server in the list.
- The next request is sent to the second server, and so on.
- After the last server in the list, the algorithm loops back to the first server.

**When to Use:**
- When all servers have similar processing capabilities and are equally capable of handling requests.
- When simplicity and even distribution of load is more critical.

**Benefits:**
- Simple to implement and understand.
- Ensures even distribution of traffic.

**Drawbacks:**
- Does not consider server load or response time.
- Can lead to inefficiencies if servers have different processing capabilities.

```py
class RoundRobin:
    def __init__(self, servers):
        self.servers = servers
        self.current_index = -1

    def get_next_server(self):
        self.current_index = (self.current_index + 1) % len(self.servers)
        return self.servers[self.current_index]

# Example usage
servers = ["Server1", "Server2", "Server3"]
load_balancer = RoundRobin(servers)

for i in range(6):
    server = load_balancer.get_next_server()
    print(f"Request {i + 1} -> {server}")
```

  
