# System Requirements

Before deploying and operating your XRPL EVM node, it’s crucial to ensure that your infrastructure meets or exceeds the recommended specifications. Adequate hardware and network resources will help maintain node performance, ensure reliable uptime, and support the network’s integrity.

## Recommended Specifications

| Resource         | Minimum Recommendation               | Notes                                                                                                                                                                                                                                                               |
| ---------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operating System | Linux (AMD64)                        | A recent Linux distribution (e.g., Ubuntu 20.04 LTS, CentOS 8, Debian 11) is strongly recommended for stability, security, and compatibility.                                                                                                                       |
| CPU              | 8 Physical Cores or More             | Multi-core processors provide enough parallel processing to efficiently handle validation, API requests, and block synchronization. Higher core counts can enhance performance under heavy load.                                                                    |
| Memory (RAM)     | 32GB                                 | Sufficient memory ensures smooth operation during peak network load, handling large state DB operations, and responding to complex smart contract queries. More memory may further improve performance.                                                             |
| Storage          | 1TB Cloud Storage (AWS), SSD or NVMe | A high-performance NVMe SSD provides fast random access and write speeds, which are essential for maintaining a responsive node database and quickly processing state updates. More storage capacity may be required for archive nodes or long-term data retention. |
| Network          | 100Mbps                              | A stable 100Mbps connection or faster ensures timely propagation of blocks, efficient peer discovery, and responsive API endpoints. Lower bandwidth may lead to delayed synchronization or performance bottlenecks.                                                 |

## Additional Considerations

1. **Reliability & Redundancy:**  
   Consider enterprise-grade hardware, redundant power supplies, and error-correcting memory (ECC) to minimize downtime. Ensuring a stable internet connection with fallback options can also help maintain high uptime.

2. **Security Measures:**  
   Keep your operating system, node software, and dependencies up to date with the latest security patches. Implement firewalls, SSH key-based authentication, and regular security audits to protect against unauthorized access and data breaches.

3. **Scalability & Future Growth:**  
   As the XRPL EVM network evolves, increased transaction loads, larger state sizes, and additional features may necessitate greater resources. Investing in hardware that can be easily upgraded will help your node stay performant over time.

4. **Monitoring & Alerting:**  
   Use monitoring tools and alerting systems (e.g., Prometheus, Grafana, or external services) to track resource usage, network health, and node performance. This helps you proactively identify and address bottlenecks before they impact node availability.

With the right hardware and infrastructure in place, you’ll be well-equipped to run a stable, high-performance XRPL EVM node and contribute to the network’s success.
