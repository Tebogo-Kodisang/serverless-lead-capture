# Serverless Lead Capture on AWS: The Epic Books

## Project Overview
Epic Books is a mock company that needs a simple way to collect customer names and email addresses  through their website.Traditional setups rely on fixed servers that are expensive to maintain and slow for global users. These systems can't handle sudden traffic spikes, leading to crashes and lost customers. Furthermore, manual security patching introduces risks that could compromise user data or result in total site downtime.

To solve this, I built a serverless solution on AWS. This eliminates the need to manage servers, reducing infrastructure costs and eliminating the need to hire staff for maintenance (patching). The solution automatically scales as website visitors increase and improves security by using managed AWS services instead of vulnerable self-hosted systems.


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

My solution was created with the AWS Well-Architected Framework in mind. While all six pillars were evaluated to guarantee architectural competence, design selections were generally guided by the pillars that were most relevant to the project's specific business needs. The AWS Well-Architected Tool was used to assess trade-offs and identify potential improvements.

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

## Design Decisions & Trade-offs

### Component: Hosting Static Website

Services Chosen:

I decided to use Amazon S3 for static website hosting, CloudFront as a CDN, and AWS Certificate Manager for website security.

Why This Approach:

- Static website hosting is one of S3's features. The service is scalable, highly available, and fully managed. 
- CloudFront will help improve the speed for users by caching content at edge locations, and also it enables HTTPS using AWS Certificate Manager.
- Serverless architecture will minimize costs because there will be no need to manage servers or to maintain them.
- AWS Certificate Manager will develop trust with clients because the connection will be perceived as safe and customer data will be protected. 


