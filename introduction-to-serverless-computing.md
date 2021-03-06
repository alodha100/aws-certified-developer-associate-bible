# Introduction to Serverless Computing

### Lambda

- What is Lambda?
  - Data Centres
  - Hardware
  - Assembly Code/Protocols
  - High Level Languages
  - Operating Systems
  - Application Layer/AWS APIs
  - AWS Lambda
    - A compute service where you can upload your code and create a Lambda function. AWS Lambda takes care of provisioning and managing the servers that you use to run the code. You don't have to worry about operating systems, patching, scaling, etc. You can use Lambda in the following ways
    - As an event-driven compute service where AWS Lambda runs your code in response to events. These events could be changes to data in an Amazon S3 bucket or an Amazon DynamoDB table
    - As a compute service to run your code in response to HTTP requests using Amazon API Gateway or API calls made using AWS SDKs

- What Languages?
  - Node.js
  - Java
  - Python
  - C#
  - Go

 - How is Lambda Priced?
  - Number of requests
    - First 1 million requests are free. $0.20 per 1 million request thereafter.
  - Duration
    - Duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB-second used.

- Why is Lambda Cool?
  - NO SERVERS
  - Continuous Scaling
  - Super super super cheap

- Exam Tips
  - Lambda scales out (not up) automatically
  - Lambda functions are independent, 1 event = 1 function
  - Lambda is Serverless
  - Know what services are serverless
  - Lambda functions can trigger other lambda functions, 1 event can = x functions if functions trigger other functions
  - Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening
  - Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets etc
  - Know your trigger

### API Gateway 101

- What is an API
  - An API is an Application Programming Interface

- Types of APIs
  - REST APIs (REpresentational State Transfer)
    - Uses JSON
  - SOAP APIs (Simple Object Access Protocol)
    - Uses XML

- What is API Gateway?
  - Amazon API Gateway is a fully managed service that makes it easy for developers to publish, maintain, monitor, and secure APIs at any scale. With a few clicks in the AWS Management Console, you can create an API that acts as a 'front door' for applications to access data, business logic, or functionality from your back-end services, such as applications running on Amazon Elastic Compute Cloud (Amazon EC2), code running on AWS Lambda, or any web application

- What can API Gateway Do?
  - Expose HTTPS endpoints to define a RESTful APIs
  - Serverless-ly connect to services like Lambda & DynamoDB
  - Send each API endpoint to a different target
  - Run efficiently with low cost
  - Scale effortlessly
  - Track and control usage by API key
  - Throttle requests to prevent attacks
  - Connect to CloudWatch to log all requests for monitoring
  - Maintain multiple versions of your API

- How do I configure API Gateway
  - Define an API (container)
  - Define Resources and nested Resources (URL paths)
  - For each Resource:
    - Select supported HTTP methods (verbs)
    - Set security
    - Choose target (such as EC2, Lambda, DynamoDB, etc)
    - Set request and response transformations
  - Deploy API to a Stage
    - Uses API Gateway domain, by default
    - Can use custom domain
    - Now supports AWS Certificate Manager: free SSL/TLS certs

  - What is API Caching?
    - You can enable API caching in Amazon API Gateway to cache your endpoint's response. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of the requests to your API. When you enable caching for a stage, API Gateway caches responses from your endpoint for a specified time-to-live (TTL) period, in seconds. API Gateway then responds to the request by looking up the endpoint response from the cache instead of making a request to your endpoint

  - Same Origin Policy
    - In computing, the same-origin policy is an important concept in the web application security model. Under the policy, a web browser permits scripts contained in a first web page to access data in a second web page, but only if both web pages have the same origin
    - This is done to prevent Cross-Site Scripting (XSS) attacks
    - Enforced by web browsers
    - Ignored by tools like PostMan and curl

  - Cross-Origin Resource Sharing (CORS)
    - Cross-Origin Resource Sharing (CORS) is one way the server at the other end (not the client code in the browser) can relax the same-origin Policy
    - Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the first resource was served
    - Browser makes an HTTP OPTIONS call for a URL
      - OPTIONS is an HTTP method like GET, PUT and POST
    - Server returns a response that says
      - These other domains are approved to GET this URL
    - Error - 'Origin policy cannot be read at the remote resource?'
      - You need to enable CORS on API Gateway

  - Exam Tips
    - Remember what API Gateway is at a high Level
    - API Gateway has caching capabilities to increase performance
    - API Gateway is low cost and scales automatically
    - You can throttle API Gateway to prevent attacks
    - You can log results to CloudWatch
    - If you are using Javascript/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
    - CORS is enforced by the client  

### Version Control With Lambda

- Versioning
  - When you use versioning in AWS Lambda, you can publish one or more versions of your Lambda function. As a result, you can work with different variations of your Lambda function in your development workflow, such as development, beta, and production
  - Each Lambda function version has a unique Amazon Resource Name (ARN). After you publish a version, it is immutable (that is, it can't be changed)
  - AWS Lambda maintains your latest function code in the $LATEST version. When you update your function code, AWS Lamda replaces the code in the $LATEST version of the Lambda function

- Qualified / Unqualified variations
  - You can refer to this function using its Amazon Resource Name (ARN). There are two ARNs associated with this initial version:
  - Qualified ARN - The function ARN with the version suffix.
    - arn:aws:lambda:aws-region:acct-id:function:helloworld:$LATEST
  - Unqualified ARN - The function ARN without the version suffix
    - arn:aws:lambda:aws-region:acct-id:function:helloworld
 - Alias
  - After initially creating a Lambda function (the $LATEST version), you can publish a version 1 of it. By creating an alias named PROD that points to version 1, you can now use the PROD alias to invoke version 1 of the Lambda function
  - Now you can update the code (the $LATEST version) with all of your improvements, and then publish another stable and improved version (version 2). You can promote version 2 to production by remapping the PROD alias so that it points to version 2. If you find something wrong, you can easily roll back the production version to version 1 by remapping the PROD alias so that it points to version 1

- Exam Tips
  - Can have multiple versions of lambda functions
  - Latest version will use $latest
  - Qualified version will use $latest, unqualified will not have it
  - Versions are immutable (Cannot be changed)
  - Can split traffic using aliases to different versions
  - Cannot split traffic with $latest, instead create an alias to latest

### Step functions
- Summary
  - allows you to visualize and test your serverless applications
  - provides a graphical console to arrange and visualize the components of your application as a series of steps. This makes it simple to build and run multistep applications
  - automatically triggers and tracks each step, and retries when there are errors, so your application executes in order and as expected.
  - logs the state of each steps, so when things do go wrong, you can diagnose and debug problems quickly

### X-ray
- What is X-Ray?
  - AWS X-Ray is a service that collects data about requests that your application servers, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization.
  - For any traced required to your application, you can see detailed information not only about the request and response, but also about calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs

- X-Ray SDKs
  - provides
    - interceptors to add to your code to trace incoming HTTP requests
    - client handlers to instrument AWS SDK clients that your application uses to call other AWS services
    - An HTTP client to use to instrument calls to other internal and external HTTP web services

- X-Ray Integration
  - Elastic Load Balancing
  - AWS Lambda
  - Amazon API Gateway
  - Amazon Elastic Compute Cloud
  - AWS Elastic Beanstalk

- X-Ray Languages
  - Java
  - Go
  - Node.js
  - Python
  - Ruby
  - .Net

- Exam Tips
  - Understand what does X-Ray SDKs provides
  - Understand what does X-Ray integrate with
  - What language is supported

### Advanced API Gateway
- Import APIs
  - You can use the API Gateway Import API feature to import an API from an external definition file into API Gateway. Currently, the import API feature supports Swagger v2.0 definition files
  - With the import API, you can either create a new API by submitting a POST request that includes a Swagger definition in the payload and endpoint configuration, or you can update an existing API by using a PUT request that contains a Swagger definition in the payload. You can update an API by overwriting it with a new definition, or merge a definition with an existing API. You specify the options using a mode query parameter in the request URL
- API Throttling
  - By default, API Gateway limits the steady-state request rate to 10,000 requests per second (rps)
  - The maximum concurrent requests is 5000 requests across all APIs within an AWS account
  - If you go over 10,000 requests per second or 5000 concurrent requests you will receive a 429 Too Many Request error response
  - If a caller submits 10,000 requests in a one second period evenly (for example, 10 requests every millisecond), API Gateway processes all requests without dropping any
  - If the caller sends 10,000 requests in the first millisecond, API Gateway serves 5,000 of those requests and throttles the rest in the one-second period
  - If the caller submits 5,000 requests in the first millisecond and then evenly spreads another 5,000 requests through the remaining 999 milliseconds (for example, about 5 requests every millisecond), API Gateway processes all 10,000 requests in the one-second period without returning 429 Too Many Requests error response

- SOAP Webservice Passthrough
  - You can configure API Gateway as a SOAP web service passthrough

- Exam Tips
  - Import API's using Swagger 2.0 definition files
  - API Gateway can be throttled
    - Default limits are 10,000 RPS or 5,000 concurrently
  - You can configure API Gateway as a SOAP web service passthrough

### Serverless Summary

- Lambda Exam Tips
  - Lambda scales out (not up) automatically
    - You can have many many different concurrent lambda functions running at once
  - Lambda functions are independent, 1 event = 1 function
  - Lambda is Serverless
  - Know what services are serverless
  - Lambda functions can trigger other lambda functions, 1 event can = x functions if functions trigger other functions
  - Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening  
  - Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets etc
  - Know your triggers

- API Gateway Exam Tips
  - Remember what API Gateway is at a high Level
  - API Gateway has caching capabilities to increase performance
  - API Gateway is low cost and scales automatically
  - You can throttle API Gateway to prevent attacks
  - You can log results to CloudWatch
  - If you are using Javascript/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
  - CORS is enforced by the client

- Version Control With Lambda Exam Tips
  - Can have multiple versions of lambda functions
  - Latest version will use $LATEST
  - Qualified version will use $latest, unqualified will not have it
  - Versions are immutable (Cannot be changed)
  - Can split traffic using aliases to different versions
    - Cannot split traffic with $latest, instead create an alias to latest

- Step Functions Exam Tips
  - Grate way to visualize your serverless application
  - Step Functions automatically triggers and tracks each step
  - Step Functions logs the state of each steps so if something goes wrong you can track what went wrong and where

- X-ray Exam Tips
  - The X-Ray SDKs provides
    - interceptors to add to your code to trace incoming HTTP requests
    - client handlers to instrument AWS SDK clients that your application uses to call other AWS services
    - An HTTP client to use to instrument calls to other internal and external HTTP web services
  - The X-Ray Integrates with the following AWS services:
    - Elastic Load Balancing
    - AWS Lambda
    - Amazon API Gateway
    - Amazon Elastic Compute Cloud
    - AWS Elastic Beanstalk
  - X-Ray integrates with the following languages:
    - Java
    - Go
    - Node.js
    - Python
    - Ruby
    - .Net
