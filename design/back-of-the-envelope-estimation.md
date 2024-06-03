## Back-of-the-Envelope Estimation in System Design Interviews

### Importance of Back-of-the-Envelope Calculations
- **Definition**: Quick estimates using thought experiments and common performance numbers.
- **Purpose**: To assess if a design will meet requirements.

### Scalability Basics for Effective Estimation
- **Power of Two**: Fundamental understanding of data volume units.
  - **Byte**: 8 bits.
  - **ASCII character**: 1 byte (8 bits).
- **Latency Numbers**: Awareness of typical computer operation durations.
  - **Nanosecond (ns)**: \(10^{-9}\) seconds.
  - **Microsecond (μs)**: \(10^{-6}\) seconds = 1,000 ns.
  - **Millisecond (ms)**: \(10^{-3}\) seconds = 1,000 μs = 1,000,000 ns.
  - **2020 Visualization**: Modern latency numbers provide insights on operation speeds.

### Key Latency Insights
- **Memory vs. Disk**: Memory is significantly faster than disk operations.
- **Disk Seeks**: Avoid if possible due to high latency.
- **Compression**: Simple algorithms are fast; compress data before transmission.
- **Data Centers**: Inter-region data transfer is time-consuming.

### Availability Numbers
- **High Availability**: Continuous operation for long periods.
  - **Measurement**: Percentage uptime, with 100% indicating zero downtime.
  - **SLA (Service Level Agreement)**: Formal uptime agreement between service provider and customer.
  - **Uptime Measurement**: Typically measured in "nines".
    - **Example**: 99.9% uptime is standard for major cloud providers (Amazon, Google, Microsoft).

### Example: Estimating Twitter QPS and Storage Requirements
- **Assumptions**:
  - 300 million monthly active users.
  - 50% daily active users.
  - 2 tweets per day per user on average.
  - 10% of tweets contain media.
  - Data retention for 5 years.
- **QPS Calculation**:
  - **Daily Active Users (DAU)**: \(300 \text{ million} \times 50\% = 150 \text{ million}\)
  - **Tweets QPS**: \(\frac{150 \text{ million} \times 2}{24 \text{ hours} \times 3600 \text{ seconds}} \approx 3500\)
  - **Peak QPS**: \(2 \times 3500 \approx 7000\)
- **Media Storage Calculation**:
  - **Average Tweet Size**: \( \text{tweet\_id} = 64 \text{ bytes}, \text{ text} = 140 \text{ bytes}, \text{ media} = 1 \text{ MB}\)
  - **Daily Media Storage**: \(150 \text{ million} \times 2 \times 10\% \times 1 \text{ MB} = 30 \text{ TB}\)
  - **5-Year Media Storage**: \(30 \text{ TB} \times 365 \times 5 \approx 55 \text{ PB}\)

### Tips for Back-of-the-Envelope Estimation
- **Rounding and Approximation**: Simplify complicated math for quick calculations.
  - Example: Simplify \(99987 / 9.1\) to \(100,000 / 10\).
- **Write Down Assumptions**: Clearly state assumptions for reference.
- **Label Units**: Specify units to avoid confusion.
  - Example: Use "5 MB" instead of just "5".
- **Common Estimations**:
  - Query per second (QPS).
  - Peak QPS.
  - Storage requirements.
  - Cache size.
  - Number of servers.
- **Practice**: Regular practice helps improve accuracy and speed.