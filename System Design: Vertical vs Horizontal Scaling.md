# Vertical vs Horizontal Scaling

## Vertical Scaling (Scaling Up)
- Vertical scaling, also known as "scaling up" involves boosting the power of an existing machine within your system to handle increased loads.
- This can mean upgrading the CPU, RAM, Storage, or other hardware components to boost the server's capacity.

### Boosting server's capacity by upgrading following components:
- **Upgrading CPU:** Replacing your server's processor with a more powerful one.
- **Increasing RAM:** Adding more memory to handle larger datasets and reduce reliance on slower storage.
- **Enhancing Storage:** Switching to faster storage (like SSDs) or increasing overall storage capacity.

### Pros of Vertical Scaling
- Simplicity
- Low Latency
- Reduces Software costs
- No Major Code Change

### Cons of Vertical Scaling
- Limited Scalability
- Single Point of Failure
- Downtime
- Higher Costs in Longer Run

## Horizontal Scaling (Scaling Out)
- Horizontal scaling, or scaling out, involves adding more servers or nodes to the system to distribute the load across multiple machines.
- Each server runs a copy of the application, and the load is balanced among them often using a load balancer.

### Pros of Horizontal Scaling
- Near Limitless Scalability
- Improved Fault Tolerance
- Cost Effective

### Cons of Horizntal Scaling
- Complexity
- Increased Latency
- Cost
- Application Compatibility

# When to Choose Vertical vs Horizontal scaling
Things to consider to decide between vertical and horizontal scaling:

- **Cost:** Analyze initial hardware costs vs. long-term operational expenses.
- **Workload:** Is your application CPU bound, memory bound, or does it lend itself to distribution?
- **Architectural Complexity:** Can your application code handle distributed workloads?
- **Future Growth:** How much scaling do you realistically anticipate?
