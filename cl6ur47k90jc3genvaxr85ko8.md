---
title: "API Automation :- Testing"
seoTitle: "Keploy vs Pynt"
seoDescription: "Keploy is a no-code testing platform that generates tests from API calls. Pynt is an API Security testing solution built on top of Newman  & Postman."
datePublished: Mon Aug 15 2022 12:47:51 GMT+0000 (Coordinated Universal Time)
cuid: cl6ur47k90jc3genvaxr85ko8
slug: api-automation-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660356441025/-NnXTGAbs.jpg
tags: apis, testing, testing-tool

---

API automated testing is critical for product quality and CI/CD processes. Unlike GUI tests, API tests can cope with short release cycles and frequent changes — *without breaking the test outputs*.

# What is API testing?

In software application development, **API** is the middle layer between the *UI and the database layer*. They enable the communication and data exchange from one software system to another.

API testing is a software testing practice that tests the APIs directly — from their functionality, reliability, performance, to security. Part of integration testing, API testing effectively validates the logic of the build architecture within a short amount of time.

## Types of API Testing

There are 6 main types of API testing that must be done and to ensure that an API is working properly and there is no breakage from either the Server or Client Side, these are as follows :-

1. Validation Testing
2. Functional testing
3. Security testing
4. Load testing
5. Runtime and error detection
6. Penetration testing

# How Keploy comes in the Play ?

[Keploy](https://github.com/keploy/keploy) is a no-code testing platform that generates tests from API calls. It converts API calls into testcases and Mock test cases are automatically generated with the actual request/responses.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660355385013/FLZjr_OuU.png align="left")

## Key Features

- **Convert API calls from anywhere to Test-Case**

- **Automatically mocks network/external dependencies for all CRUD operations with correct responses.**

- **It identifies noisy fields in the responses accurately like (timestamps, random values) to ensure high quality tests.**

- **It has native integrations with popular testing libraries like go-test. Code coverage will be reported with existing and Keploy recorded test cases and can also be integrated in existing CI pipelines easily.**

- **Keploy has Instrumentation/Integration framework to easily add the new libraries/drivers within ~100 lines of code.**

# What is Pynt ?

[Pynt](https://github.com/pynt-io/pynt) is an API security solution for developers and testers. It generates and runs security tests automatically from existing Postman and Newman collections. 

While working with Pynt, One thing that is really important is to make sure that the API has more extensive functional tests are, the more the security tests will cover. For example, more APIs, more users, more requests, and full use of the parameters will trigger broader and richer dynamic security tests.

We can provide any valid functional Postman test collection for API/s you have. The product generates security tests from it, runs them, and provides you with the results.

![image.png](https://user-images.githubusercontent.com/107360829/181883204-fed73a15-8c9a-4087-b28b-22f53884ed44.gif align="left")

---

Personally, I liked Keploy usage more as Pynt is still in development phase and Keploy has provided me with better Developer Experience that in now-a-days many product lacks, which is very important point of focus as majority of user for such tools and libraries are Developers themselves.