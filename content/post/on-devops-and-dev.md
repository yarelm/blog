#+++
title = "On DevOps and Dev"
description = ""
date = 2021-03-22T02:13:50Z
author = "Yarel Maman"
+++
# Should Developers do DevOps?

DevOps is an interesting term. If you search Wikipedia for it, you will find that it is defined as
> **DevOps** is a set of practices that combines [software development](https://en.wikipedia.org/wiki/Software_development "Software development") (_Dev_) and [IT operations](https://en.wikipedia.org/wiki/IT_operations "IT operations") (_Ops_)

Well that already answer our question, no? Maybe. Let's explore.

Back in the days (when we were young and foolish), the engineering force of most small-mid sized tech companies was something like:
- Software Developers/Engineers
- QA Engineers
- IT/SysAdmins

Software developers did, well, software development.
QA mostly worked alongside the developers, writing automations or doing manual testing.
IT guys dealt with managing the servers and infra (that's mostly pre-cloud when we were mainly on-prem) and personal equipnment.

Nowadays, The structure may look as something of this sort:
- Software Developers/Engineers (some divide to Full Stack, Backend, Frontend) 
- DevOps Engineers
- QA/Automation Engineers
- IT/SysAdmins

The interesting part is that there seems to be a sort of trend regarding the growth in demand of each of these titles:
Title | Demand Growth
----- | ------
Software Developers | +
DevOps Engineers | +
QA Engineers | -
IT | -


You probably know what I mean. I can't back it up with actual data but this is what i'm experiencing in recent years.

My opinionated reasons for the reduced growth:
**QA Engineers** - nowadays more and more developers write tests, as their responisbility scope and their awareness is much bigger than in the past. Developer nowadays need to "own" their code, and take it through the commit step to the production step. Involving an additional person (QA) in the context can make this process longer and rigid. 
In addition, many companies prefer to allocate QA's time for external testing - much more End-To-End, acceptance testing, and testing which mimics the client and it's use cases, while being "disconnected" from the internal development bias.

**IT** - As more and more companies move their workloads to the cloud, the traditional IT role is losing its value and purpose. Less servers and on-prem infrastructure are needed to maintain.

# There is a Trend
So, as demand is reduced, people are scared of losing their job and naturally, change is occuring.
What I have witnessed, is the following trend:

QAs are becoming Automation Developers (or in some case shift to software developers)
ITs are becoming DevOps Engineers.

Now, there is nothing wrong with career shifts. Many of us have done that, and it's basically how it's being done for years, not just in tech. Technology is evolving and careers change with it. Some people chose it because they really like the modern concepts such as Cloud and Automation. Others just realized that if they will not act, they will stay out of job.

However, I feel this "massive" career shifts has also caused the utter confusion around the term DevOps.

DevOps, as I see it, is really a culture and a set of best practices for reliability, quality, and rapid delivery of software.

Many people like to categorize it to a job title. But there is an ambiguity here.

Some see DevOps as a rebranding of a traditional SysAdmin - In that sense DevOps is about operations such as managing servers, OS, configuration, manual work and some automation scripts. 

Others, myself included, see it as an extension of the development team. It is like the ambassador representing the developers in the operational world of hazards. Google call it SRE (Site Reliability Engineer), and that's their definition: 
> SRE is what happens when you ask a software engineer to design an operations team

In that sense I highly encourage you to read the [SRE book by Google](https://sre.google/sre-book/introduction/) (It's free!)

> 50–60% are Google Software Engineers, or more precisely, people who have been hired via the standard procedure for Google Software Engineers. The other 40–50% are candidates who were very close to the Google Software Engineering qualifications (i.e., 85–99% of the skill set required), and who _in addition_ had a set of technical skills that is useful to SRE but is rare for most software engineers. By far, UNIX system internals and networking (Layer 1 to Layer 3) expertise are the two most common types of alternate technical skills we seek.


So in Google's eyes, an SRE is a software engineer. The focus seems to be more on the operational and reliability aspect than on the product aspect.

I think it's understandable why. In order to create useful mechanisms for DevOps, you have to be well aware of the software development lifecycle and of building reliability.
For example, Does my application handles termination signals properly? Is there a possible memory leak? Should a circuit breaker be used here? Is this API idempotent? Can I safely retry it?

## So, Should I encourage my developers to be involved with DevOps?
Yes! 

That being said it really depends on how big is your organization. If you are a small startup with a few developers - by all means, encourage them to be involved in the process. As you are currently small, several fresh and very important aspects of the development work is being defined as you go. Things such as: CI/CD, Infrastructure-As-Code, Microservices, Kubernetes (or not), will evolve as your company evolves. Software developers can have a big impact on these, even if you include them just for their opinions.

If you are a mid-large organization with a well established operations team, then you can still benefit a lot from involving your developers. Let them write Infrastructure-as-code, let them have a say in the CI/CD definition, and hear them out constantly. The amount of overlap that exists between operations and development is huge. Don't assume the operations team knows best. Instead, ask for the developers opinion.

Remember that DevOps is more of a culture and a set of practices than a role. 
When you ask someone what is their role and they answer "DevOps", 
