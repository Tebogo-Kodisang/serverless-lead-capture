# Serverless Lead Capture on AWS: The Epic Books

## Project Overview
Epic Books is a mock company created for this project. The business needed a simple way to collect customer names and email addresses from its website.

To solve this, I built a serverless solution on AWS. This removes the need to manage servers, which also reduces infrastructure costs and eliminates the need to hire staff to maintain them. The solution automatically scales as website visitors increase and improves security by using managed AWS services instead of self-hosted systems.

## Business Requirements

The Epic Books provided the following requirements:

- The website will be built using HTML, CSS, and JavaScript.
- Customers must be able to download an ebook.
- Customers must receive an email notification after downloading the ebook.
- Customer details (name and email) must be stored in a database after download.
- The website must serve global users and remain fast and responsive.
- The website has 500 active users with 10–15 ebook downloads per day.
- All requests must use HTTPS.
- The domain name must be theepicbooks.com.


## Well-Architected Framework Considerations

My solution was designed using the AWS Well-Architected Framework as a guiding principle. All six pillars were considered during the design process, and the AWS Well-Architected Tool was used to review architectural decisions, evaluate trade-offs, and identify potential  areas of improvement.

Based on the stated business requirements of The Epic Books, the applicable AWS Well-Architected Framework pillars are:

- Global website performance (Performance Efficiency Pillar)
- HTTPS and data protection (Security Pillar)
- Store customer details in a database (Security & Reliability Pillars)
- Email notification after download (Operational Excellence Pillar)
- Support 500 active users and 10–15 daily downloads (Cost Optimization & Performance Efficiency Pillars)

Having considered these requirements, I decided to prioritize the following pillars in order of importance:

1. Security
2. Cost Optimization
3. Performance Efficiency

These pillars helped drive my architectural decisions for the serverless solution. Data is secured using managed AWS services and HTTPS, serverless components reduce costs by automatically scaling and eliminating idle resources, and performance is optimized to support global users efficiently. The architecture details are explained in the “How It Works” section. It's also worth mentioning that these priorities might be switched and modified based on business requirements. 

