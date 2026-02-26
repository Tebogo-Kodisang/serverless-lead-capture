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

## Design Decisions

### Component: Hosting Static Website

Selected solution:

I decided to use Amazon S3 for static website hosting, CloudFront as a CDN, and AWS Certificate Manager for website security.

Why This Approach:

- Static website hosting is one of S3's features. The service is scalable, highly available, and fully managed. 
- CloudFront will help improve the speed for users by caching content at edge locations, and also it enables HTTPS using AWS Certificate Manager.
- Serverless architecture will minimize costs because there will be no need to manage servers or to maintain them.
- AWS Certificate Manager will develop trust with clients because the connection will be perceived as safe and customer data will be protected. 

Implementation Details:

1. Prepare website files and code (HTML, FONTS, CSS, JavaScript, images)
2. Create an S3 bucket for static website hosting.
3. Enable the static website feature on S3 bucket.
4. Configure public access and added a bucket policy that grants users "GetObject" permissions.
5. Upload an error document for when users are unable to access the site.
6. Upload all website files to the S3 bucket.
7. Create a CloudFront distribution for the website using the S3 bucket as the origin type.
8. Request a public TLS certificate in ACM for theepicbooks.com.
9. Route traffic to the CloudFront distribution on Route 53. 

### Component: Configuring the “Contact Us” section of the lead capture form

Selected solution:

I used Amazon API Gateway to securely expose the backend, AWS Lambda to process contact form submissions, Amazon SES to send email alerts, IAM to grant secure permissions, and CloudWatch to monitor and log.

Why This Approach:
- API Gateway provides a secure public URL for the website, allowing messages to be securely passed to the backend.
- AWS Lambda automates form submissions, scales with demand, and eliminates the need to manage servers.
- When a user submits a form, Amazon SES sends emails that are both reliable and cost-effective.
- IAM Roles and Policies ensure that Lambda only has the necessary permissions (sending emails, writing logs), thereby improving security.
- CloudWatch will act as our eyes and ears on the backend. It allows us to view what's going on, detect errors (including SES and Lambda issues), and track performance across the entire serverless system.
- This serverless architecture eliminates operational efforts, reduces cost, and enables your frontend to communicate safely with the backend.

Implementation Details:
1. Set up the SES service and create two identities (sender and receiver email addresses).
2. Create an IAM role to attach to the Lambda function, providing permissions to send emails using Amazon SES and write logs to Amazon CloudWatch.
3. Create a Lambda function that invokes the Amazon SES API to send emails.
4. Test the Lambda function. 
5. Create a REST API in API Gateway to take form submissions from the website and send them to the Lambda function for processing.
6. Enabled CORS on the API Gateway to allow the website to access the API.
7. Deploy the API to a stage so it becomes live and usable by the website.
8. Simulate preflight check (optional) – Use CURL on the command line to mimic the browser’s preflight request and verify that the API allows requests from the website.
9. Run actual POST request – Use CURL on the command line to send the real form data to the API and verify that Lambda processes it correctly.
10. After running the CURL command for the actual POST request, you should be able to see the form submission details in CloudWatch logs and on your third-party email receiver.
11. Because CloudFront does not allow POST requests by default, edit the website code to point the form directly to the API Gateway endpoint, and include any necessary JavaScript or other code so the form can successfully send data to the Lambda function.
12. Invalidate CloudFront cache to ensure the updated website in S3 is served to users instead of the old cached version.

### Component: Saving leads and data management

Selected solution: 

I used DynamoDB to store contact form submissions in a scalable and reliable manner. This ensures that all leads are carefully stored for future usage.

Why This Approach:
- Amazon DynamoDB stores form submissions reliably and at scale, integrates with Lambda, and eliminates the need to manage servers, allowing the backend to access and process leads efficiently.

Implementation Details:
1. Create a table in DynamoDB.
2. Create an inline policy for the Lambda function to allow it to securely write data to DynamoDB.
3. Update the Lambda function code to store form submissions in the DynamoDB table.
4. Deploy the code and test.

How the customer will get their ebook:
The e-book is stored in Amazon S3, which provides secure, scalable, and cost-effective storage. When a visitor submits the form on the website, Lambda processes the submission, saves the lead data in DynamoDB, and triggers SES to send a confirmation email. This email includes a link to download the e-book, either as a public S3 object or a pre-signed URL generated by Lambda, allowing the user to access the e-book securely and immediately.

## Trade-offs and Alternatives considered:

In this section, we'll look at other viable AWS alternatives.

### Component: Hosting Static Website

1. Direct access to S3 with no CloudFront
Rather than using CloudFront, users access the website using the web address that belongs to the S3 bucket.

Pros:
- Removes the responsibility of managing CloudFront distributions and invalidation. When you make changes to your website, users will see them immediately, without having to wait for a cache to clear.

Cons:
- S3 buckets are limited to a particular region, causing delays for customers outside that region. This goes against the company's purpose of providing a quick and responsive website for worldwide consumers.
- In order to get HTTPS, you would need Amazon CloudFront in front of S3 so that you can attach an SSL certificate using Certificate Manager. This goes directly against our business requirement that all traffic must use HTTPS.
- It costs more to send data directly from S3 to the internet than to use CloudFront.
- With this type of architecture, you would need to make your S3 bucket public. That means anyone on the internet can access your bucket directly, which poses a security risk. You lose CloudFront's Origin Access Control feature, which allows CloudFront to securely access your private S3 bucket while other users are denied access. 

2. Using AWS Global Accelerator instead of CloudFront
With this type of architecture, traffic enters through the nearest AWS edge location and travels over AWS's private fiber to the S3 bucket and API Gateway. 

Pros:
- This type of setup is better at rerouting traffic when an AWS region becomes unhealthy.

Cons
- Unlike CloudFront, S3 does not cache content. As a result, it must handle every request. Each time a user requests a file, S3 counts it as a request, incurring a small cost that can add up quickly if your site receives a lot of traffic.
- You pay a fixed hourly fee for each accelerator you own, even if no traffic passes through it.
- In contrast to CloudFront, you pay for the public IPv4 addresses attached to your accelerators.

### Component: Configuring the “Contact Us” section of the lead capture form

1. Using an Application Load Balancer (ALB) instead of API Gateway
Rather than exposing the backend through API Gateway, we could route traffic through an ALB directly to Lambda.

Pros:
- It can be cheaper during periods of high traffic. 
- There are no built-in API management capabilities, such as request validation or throttle controls. 

Cons
- To safeguard public API exposure, more configuration is required than with API Gateway.

2. Instead of using API Gateway, we can expose the Lambda function via a Lambda Function URL.

Pros:
- Simpler setup.
- Lower cost.

Cons
- There are less security and traffic control options compared to API Gateway.
- There is no built-in throttling, API keys, or advanced request processing.

3. Using Amazon EC2 instead of Lambda

Pros:
- Ideal for long-running or complex workloads.

Cons:
- Requires server management, patching, scaling configuration, and monitoring.
- More expensive for low or unpredictable traffic.

## Reliability & Resilience

The architecture uses AWS managed services to reduce infrastructure-level single points of failure. This section looks at potential failure scenarios and their impact on system reliability.

1) S3 (Static Website)

Failure Scenario:
Bucket misconfiguration or object deletion.

Impact:
Website becomes unavailable or returns 403/404 errors.

Single Point of Failure Assessment:
Low — Amazon S3 automatically replicates data across various Availability Zones within a region. Risk is generally configuration-related.

Recovery strategy:
Enable versioning in your S3 bucket. This will help with accidentally deleted items.
CloudFront caching reduces impact.

2) CloudFront

Failure Scenario:
Distribution misconfiguration or certificate expiry.

Impact:
Users cannot access website via HTTPS.

Single Point of Failure Assessment:
Low — CloudFront is a globally distributed service with automatic edge redundancy. Failure risk is primarily related to configuration errors rather than infrastructure failure.

Recovery strategy:

ACM-managed certificate auto-renewal

Multi-edge redundancy (built-in)

Infrastructure-as-Code for consistent redeployment

3) API Gateway

Failure Scenario:
Incorrect route configuration, throttling limits exceeded, or service disruption.

Impact:
Form submissions fail and return 4XX or 5XX errors.

Single Point of Failure Assessment:
Low — API Gateway is regionally managed and highly available across multiple Availability Zones. Most risks stem from misconfiguration or quota limits.

Recovery strategy:

Enable throttling and usage monitoring

CloudWatch alarms for 4XX/5XX errors

Infrastructure-as-Code for redeployment

4) AWS Lambda

Failure Scenario:
Function timeout, runtime error, or concurrency limit reached.

Impact:
Lead data is not processed; users may see an error message.

Single Point of Failure Assessment:
Low — Lambda is multi-AZ and serverless. Risk lies in code-level failures or configuration limits rather than infrastructure failure.

Recovery strategy:

CloudWatch log monitoring and alarms

Dead-letter queue (optional) for failed invocations

Proper timeout and memory configuration

Reserved concurrency settings if necessary

5) DynamoDB

Failure Scenario:
Table misconfiguration, throttling due to capacity limits, or accidental deletion.

Impact:
Lead data cannot be stored, resulting in lost submissions.

Single Point of Failure Assessment:
Very Low — DynamoDB is multi-AZ by design. Risk primarily comes from misconfiguration or capacity planning errors.

Recovery strategy:

On-demand capacity mode to prevent throttling

Point-in-Time Recovery (PITR) enabled

IAM least-privilege policies to prevent accidental deletion

6) Amazon SES

Failure Scenario:
SES sandbox restrictions, sending quota exceeded, or domain verification issues.

Impact:
Confirmation emails are not delivered, though lead data remains stored in DynamoDB.

Single Point of Failure Assessment:
Low — SES is regionally managed and highly available. Email failure does not affect data persistence.

Recovery strategy:

Move out of SES sandbox

Monitor sending quotas

CloudWatch alarms for delivery failures

Retry logic in Lambda (if implemented)

7) CloudWatch

Failure Scenario:
Logging misconfiguration or insufficient monitoring rules.

Impact:
Reduced visibility into system errors and performance issues.

Single Point of Failure Assessment:
Very Low — CloudWatch does not affect application functionality; it impacts observability only.

Recovery strategy:

Predefined alarms for errors and throttling

Log retention policies

Overall Resilience Summary

Because the architecture uses fully managed, multi-AZ AWS services, there are no traditional infrastructure single points of failure. Most risks are configuration-based or application-level rather than hardware-level. Data persistence (DynamoDB) remains intact even if email delivery (SES) fails, ensuring core business functionality is preserved.

## Performance & Scalability

The serverless architecture is designed to scale automatically with user demand while maintaining fast and reliable responses.

**Website Performance**
CloudFront caches website assets at edge locations to deliver content quickly to users worldwide.
Cache invalidation ensures updated content is served immediately after deployments.

**Lambda Scaling**
Lambda functions automatically scale to handle concurrent requests.
Lightweight functions and managed concurrency support smooth operation during traffic spikes.

**DynamoDB Throughput**
On-demand capacity mode ensures DynamoDB scales automatically to handle incoming requests.
Multi-AZ replication supports low-latency access and high availability.

**Email Delivery (SES)**
Amazon SES scales automatically to handle email confirmations.
Monitoring ensures timely detection of delivery issues.

Overall Scalability

All components are fully managed AWS services, allowing the system to scale seamlessly without manual intervention while maintaining responsiveness and reliability.

## Security Considerations

The architecture uses AWS managed services to provide strong security while minimizing operational overhead. Key considerations include:

**Data Protection**: Customer information is stored securely in DynamoDB with IAM policies enforcing least-privilege access. Data in transit is encrypted using HTTPS.

**Email Security**: Amazon SES uses verified sender identities and secure protocols to ensure email integrity.

**Access Control**: API Gateway and Lambda functions are secured with IAM roles, granting only the permissions required to perform their tasks.

**Infrastructure Security**: S3 bucket policies and CloudFront Origin Access Control prevent unauthorized access to static assets.

**Monitoring & Alerts**: CloudWatch monitors logs and triggers alarms for security events or misconfigurations.

The architecture balances usability and security, ensuring customer data is protected while the system remains operational and scalable.

## Cost Considerations

The architecture is designed to minimize costs by leveraging serverless and managed AWS services:

**Serverless Components**: Lambda, API Gateway, and DynamoDB eliminate the need for always-on servers, reducing infrastructure expenses.

**Pay-as-You-Go**: Services scale automatically, so costs align with actual usage rather than fixed capacity.

**Content Delivery**: CloudFront caching reduces repeated requests to S3, lowering data transfer costs.

**Operational Efficiency**: Fully managed services reduce the need for maintenance staff and manual patching.




