---
title: "What are Serverless? Why do you need them?"
datePublished: Wed Jul 21 2021 03:21:25 GMT+0000 (Coordinated Universal Time)
cuid: ckrcx5ju307cn91s18m8q7rfj
slug: what-are-serverless-why-do-you-need-them
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1626837656918/mrzhATJE8.jpeg

---

In recent years, Serverless adoption has started, with more and more individuals depending on Serverless technology to meet organizations’ specific needs. A survey conducted by Serverless Inc. showed in 2018 that half of the respondents used Serverless in their job, and the numbers are projected to rise further. In this post, we would get around the terms so that you can start your own Serverless journey 😉<br>
Let’s get started!

#### **What is a Server?**
<p>
If you have been on the internet even for a while, you have undoubtedly interacted with a server. Servers are a piece of specific hardware which ‘serves’ information or data to you. The specific hardware contains a lot of memory, speedy internet and generally hosted in massive data centres with other servers.</p>

####  **The Limitation** 
<p>
Most serverless providers won’t let your code execute for more than a few minutes, and when you spin up a function, it doesn’t retain any stateful data from previously run instances. A related problem is that serverless code can take as long as several seconds to spin up — not a problem for many use cases, but if your application requires low latency, be warned.</p><p>
You can always jump into a scalable cloud computing infrastructure.Although there are open source options available, the serverless market is dominated by the big commercial cloud providers, as we’ll discuss in a moment. That means developers often end up using tooling from their vendors, which makes it hard to switch if they grow dissatisfied. And because so much of serverless computing takes place, by definition, on the vendor’s infrastructure, it can be difficult to integrate serverless code into in-house development and testing pipelines.</p>

#### **What is Serverless?**
<p>
Serverless functions are event-driven, meaning the code is invoked only when triggered by a request. The provider charges only for compute time used by that execution, rather than a flat monthly fee for maintaining a physical or virtual server. These functions can be connected together to create a processing pipeline, or they can serve as components of a larger application, interacting with other code running in containers or on conventional servers.</p>
<p><img src="https://miro.medium.com/max/3600/1*PyPO0Tq1VC4ZfIGQusEw4w.jpeg" alt/text="Servers"></p>

### **How ServerLess Works?** 
<p>
Developers rely on serverless to execute specific functions. Because of this, cloud service providers offer Functions as a Service (FaaS). Below, you can see how functions are written and executed in a serverless way.</p><p>
1. **The developer writes a function.** This function often serves a specific purpose within the application code.
2. **The developer defines an event. **The event is what triggers the cloud service provider to execute the function. A common type of event is an HTTP request.
3. **The event is triggered.** If the defined event is an HTTP request, a user triggers the event with a click or similar action.
4. **The function is executed.** The cloud service provider checks if an instance of the function is already running. If not, it starts a new one for the function.
5. **The result is sent to the client.** The user sees the result of the executed function inside the application.</p>

### **Serverless vendors: AWS Lambda, Azure Functions, and Google Cloud Functions**<p>
The modern age of serverless computing began with the launch of AWS Lambda, a platform based on Amazon’s cloud service, in 2014. Microsoft followed suit with Azure Functions in 2016. Google Cloud Functions, which had been in beta since 2017, finally reached production status in July 2018. The three services have slightly different limitations, advantages, supported languages, and ways of doing things.
<img src="https://miro.medium.com/max/1050/1*9v5XVNWV1CPBW49l-yZMmA.png">
</p>


