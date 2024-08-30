# System Design: What is Availability?
- Availability refers to the proportion of time a system is operational and accessible when required.
- The formal definition of availability is:
> **_Availability = Uptime + (Uptime + Downtime)_**
- **Uptime:** The period during which a system is functional and acessible
- **Downtime:** The period during which a system is unavailable due to failures, maintenance, or other issues.

## Availablity Tiers
- Availability is often expressed in "nines". The higher the availability, the less downtime there is.
  
  ![image](https://github.com/user-attachments/assets/28c1a32a-d457-4a7f-ba40-f18bc35a8c44)

- Each additional "nine" represents an order of magnitude improvement in availability.

> Example: 99.99% availability represents a 10-fold improvement in uptime compared to 99.9%.

## Stategies for Improving Availability
1. Redundency
2. Load Balancing
3. Failover Mechanisms
4. Data Replication
5. Monitoring and Alerts

## Best Practices for High Availability
1. Design for Failure
2. Implement Health Checks
3. Use Multiple Availability Zones
4. Practice Chaos Engineering
5. Implement Circuit Breakers
6. Use Caching Wisely
7. Plan for Capacity
