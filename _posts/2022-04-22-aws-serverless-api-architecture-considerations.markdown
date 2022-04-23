---
layout: post
title: "AWS Serverless API Architecture Considerations"
date: 2022-04-22 12:09:27 -0700
categories: AWS
---

I will be discussing three limitations worthy of consideration when debating whether serverless is the correct design pattern for a given use case. There are a lot of convincing arguments for going serverless, thus this is not a roast, but rather things to keep in mind when designing a new system.

# **1) Lambda 6 MB Limit**

**Problem:** The max payload that can be sent to and from a lambda function is capped at 6 MB.

**Solution:** The solution to this problem is hacky. Assuming that requests are always under 6 MB in size and responses can be larger than 6 MB, then an asynchronous endpoint where responses are written to an S3 bucket can solve the problem. This will add complexity to the system. Let me paint the picture further from the client point of view:

1. Make a request
   - Receive response with request_id

2. Make a call to status endpoint with request_id
   - Receive response with job status

3. Status endpoint returns status: ‘complete’ with a signed URL
   - Make get request with signed URL to retrieve response

As we can see an asynchronous endpoint adds some burden on the client. This also introduces additional resources into the system such as an S3 Bucket and a database to track the status of requests. Furthermore, I would like to highlight that if client requests contain payloads larger than 6 MB then this design does not work. For requests larger than 6 MB the client will have to place the request payload in a S3 Bucket then make the request to the API. [More information here](https://sookocheff.com/post/api/uploading-large-payloads-through-api-gateway).

# **2) Environment Variables Limited to 4 KB**

**Problem:** Lambda functions have a 4 KB limit for environment variables.

**Solution:** The solution is rather straightforward and will require one of the following resources: AWS Secrets Manager or AWS Parameter Store. AWS Secrets Manger has more features than Parameter Store, so which resource to leverage is use case specific.

# **3) Lambda Max Runtine is 15 Minutes**

**Problem:** AWS Lambdas have a max runtime of 15 minutes. For a lot of use cases this is not a concern, but assuming a job takes longer than 15 minutes then this becomes a problem.

**Solution:** There are various ways to solve this problem, but an elegant way is to use recursion. Implementing this is more fun than it should be and took me back to the good ole CS days at UW-Madison. AWS Lambda has an asynchronous invocation method which is handy for this use case. When recursing, the state of the job will need saving either by passing it as the payload (limited to 256 KB in async invocations) or by writing it somewhere (ex: S3 Bucket) and passing a reference to it in the payload.

I hope this post has highlighted some limitations that aren’t obvious to take into account when designing a system.
