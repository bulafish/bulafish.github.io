---
title: Basic Concept of CI CD
author: bulafish
date: 2018-07-15 +0800
categories: [Study Note]
# tags: []
---

CI/CD focus on the `automation of steps` to release application

CI：
 - Build
 - Test, code analysis
   - Unit testing
   - Linting
   - Code coverage type testing
 - Feed back quality of code

{% include ads3.html %}

CD：
 - Provision (optional)
   - QA test environment
   - Stage environment  
 - Deploying application
   - Specific version
   - Configured & installed on provisioned Environment  
 - Acceptance test
   - Sanity tests

Deployment：
 - Deploying application to production environment

All of the above steps are automated with monitoring mechanism

Thoughts：
 - How to perform revision control
