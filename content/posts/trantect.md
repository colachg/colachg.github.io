+++ 
draft = false
date = 2019-10-25T17:56:24+08:00
title = "Trantect"
slug = "/trantect" 
tags = []
categories = []
+++

[Trantect](http://www.trantect.com/) (Chinese: 川陀大匠) is a software development startup in Nanjing, China. Inspired by FSM and the great computer scientists, engineers and hackers of last few decades, Trantect is looking forward to building a software development team of leading technologies, agile processes, and meaningful styles domestically and internationally.

## Agile software development for the future world

### Plan

**Story** comes from users, our supports help user to find and write down their requirements. Story has point to show the value and priority. (like: "[8 points] As a user I want to show my paid orders so that I can see can check my money.")  
**Tasks** are what developers will do spilt by a story. Every task has points to evaluate its difficult or complexity.We use [plan poker](http://poker.trantect.com/) to decide points of tasks.  
**Sprint** is the basic unit of development in Scrum. We often use either one week or two weeks for each sprint. We deploy valuable functions for users within per sprint continuously.

**Trello** is used widely here to trace and backlog stories and tasks.

### Develop

We use CI/CD to speed up our daily work. When developers finish their task, they will commit and push to self-hosted Gitlab-CE. Then the pipeline will be triggered to run code lint and unit tests. After all jobs of pipeline are green, leaders will review the code if fine it will be merged into master branch and trigger another pipelines.

We often work **in pairs** to make efficient and use open source tools in our works such as: Gitlab-CE, Gitlab-runner, Jenkins, GOCD, Nexus, terraform, ansible etc. Ubuntu Desktop is our default work environment. **Thanks to the open source community**.

Recently, we add a special event on Friday noon that developers show us their results of work. So we know what they are doing exactly and developers can make the sense of accomplishment. I think it is double win.
