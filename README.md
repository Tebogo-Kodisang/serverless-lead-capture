# Serverless Lead Capture on AWS: The Epic Books

## Project Overview
Epic Books is a mock company created for this project. The business needed a simple way to collect customer names and email addresses from its website.

To solve this, I built a serverless solution on AWS. This removes the need to manage servers, which also reduces infrastructure costs and eliminates the need to hire staff to maintain them. The solution automatically scales as website visitors increase and improves security by using managed AWS services instead of self-hosted systems.

## Business Goals
* **Global Reach**: Ensure the website is highly responsive for users regardless of their location.
* **Security & Privacy**: Protect customer data using industry-standard encryption and secure storage.
* **Scalability**: Build a system that automatically handles spikes in traffic as the business grows.
* **Operational Efficiency**: Eliminate the need for manual server management and reduce staffing overhead.
* **Lead/Customer Capture Form**: Provide a simple, user-friendly interface to collect names and email addresses.

## Well-Architected Framework Considerations

This solution was designed using the AWS Well-Architected Framework as a guiding principle. All six pillars were considered during the design process, and the AWS Well-Architected Tool was used to review architectural decisions, evaluate trade-offs, and identify potential improvements.

Based on the business goals of The Epic Books, the applicable AWS Well-Architected Framework pillars are:

- Business Goal: Global Reach (Performance Efficiency Pillar)  
- Business Goal: Security & Privacy (Security Pillar)  
- Business Goal: Scalability (Reliability & Performance Efficiency Pillars)  
- Business Goal: Operational Efficiency (Operational Excellence & Cost Optimization Pillars)  
- Business Goal: Lead/Customer Capture Form (Security & Operational Excellence Pillars)  

Having considered these goals, I decided to prioritize these particluar pillars below,  in order of importance:

1. Security  
2. Cost Optimization  
3. Performance Efficiency

These pillars helped drive my architectural decisions for the serverless solution: data is secured with managed AWS services and HTTPS, serverless components save good money by scaling automatically and eliminating idle resources, and performance is optimized with fast and swift global content delivery. The implementation specifics are detailed in the "How It Works" section.
