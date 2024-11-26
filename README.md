# Cloud Resume Challenge 

I recently completed the Cloud Resume Challenge, a hands-on cloud engineering project that required me to build, deploy, and maintain a fully functional resume website using AWS. This project gave me experience with a full suite of cloud tools, implementing infrastructure as code, and practice with continuous integration and continuous deployment. The process was a large learning curve and rewarding, allowing me to deepen my understanding of cloud technologies, automation, and modern software development workflows. In this blog post, I break down the process of setting up a static website on AWS by working through the Frontend, Backend, and Infrastructure to develop it. This summary isn’t simply about how to create a static site, but about how these key services on AWS interact with each other.

---

## Frontend: Hosting the Resume Website

I started by creating an S3 bucket to hold my `index.html` file that is styled in CSS. This is a standard language and style used for website design. S3 is a simple storage service that allows users to store and retrieve data from anywhere on the web. Using Namecheap to purchase my domain, `andietometsko.com`, Route 53 was the domain name service that I used to host my domain. When I created a hosted zone, I put in an alias that directed it to my S3 buckets, and later to CloudFront, ensuring that my site could access my `index.html` file. Wanting to increase security and privacy for users visiting my website, I wanted my site to use HTTPS instead of HTTP. I requested an SSL certification on AWS Certificate Manager and did a DNS validation to prove that I own the website name. Once I obtained my certificate, I set up the CloudFront validation. CloudFront is a content delivery network that distributes content across numerous data centers known as edge locations. Caching my content allowed people outside of the `us-east-1` region to access it quickly and cut down any latency time.

---

## Backend: Creating a Visitor Counter

Starting with the backend development, I wanted to create a visitor counter API that increments by one each time a user visits my website. First, I created a visitor counter table in DynamoDB. This is a non-relational, NoSQL database where the data is stored that my API is going to interact with. I then used Lambda, written in Python, to create a function that would interact and edit my DynamoDB table. This function essentially automated my infrastructure as it is scalable and adjustable in case any changes need to be made. I then created an HTTP API, using API Gateway. Anytime I got an HTTP request from my website, it would trigger the Lambda function, add the increment, and store it in my DynamoDB table. I then created JavaScript code that allowed my frontend interface to interact with my backend; this was stored in the S3 bucket. This code triggered the API Gateway to invoke the Lambda functions and access my DynamoDB.

---

## Testing and Validation

To ensure that all components functioned correctly, I conducted three types of tests:

### Unit Test:
- I created a virtual environment to test the Lambda function with a mock DynamoDB table.
- This ensured the function ran properly without affecting real data or breaking existing code in case of failures.

### System Test:
- Tested the integration of the entire system, focusing on the user experience.
- Verified that the visitor counter incremented by one as expected, using the actual DynamoDB table for this test.

### End-to-End Test:
- Automated testing using Playwright to simulate a real user interaction:
  - Opened a browser autonomously.
  - Verified the presence of my information and visitor counter on the website.
  - Reloaded the page to confirm that the visitor counter was incremented by one.
- This test ensured that the entire system worked seamlessly from the front end to the back end.

---

## Infrastructure: IaC, CI/CD

After those three tests passed, I worked on Infrastructure as Code to configure my S3 bucket into code using Terraform. Now, instead of working through the AWS console, I can edit through code, making it more efficient for modifications. I modified a tag to prove that this worked. Next, I moved on to Source Control, which entailed setting up three GitHub repositories. This is so I can store, track, and roll back on any past changes I have made to all my files for this project. I pushed all the files from Visual Studio Code from the frontend, backend, and infrastructure into my GitHub repos. Finally, I practiced CI/CD using GitHub Actions. I used it for the frontend so that whenever I push code to my repositories, it will not only update my GitHub but actually go into my AWS S3 bucket and edit it as well. I did this by retrieving secret keys from AWS and configuring them to push GitHub Actions to my S3 bucket.

---

## Challenges and Reflection

Completing the Cloud Resume Challenge was both demanding and fulfilling. Troubleshooting issues provided some of the most valuable learning experiences throughout the process. One major challenge was getting DNS validation approved for my SSL certification. Eventually, I decided to restart the process from scratch, which worked smoothly the second time and taught me the importance of knowing when to start over. Another issue arose after obtaining the certificate when my resume didn’t display on my website. I discovered that Route 53 was still routed to my S3 bucket instead of CloudFront. This experience deepened my understanding of how AWS services interact and how one misconfiguration can affect the entire system. Beyond technical skills, the challenge reinforced soft skills like problem-solving, persistence, and attention to detail. By working through these real-world issues, I gained confidence in designing scalable, secure, and maintainable cloud infrastructure while showcasing my technical and troubleshooting abilities.
