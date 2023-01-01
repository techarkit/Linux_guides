## What is monitoring?
What is monitoring exactly? If you're in Operations team you probably figure-out you already know but one of the keys to making monitoring successful is being logically meticulous. So, let's talk about core concepts. 

The dictionary definition of monitoring is to observe and check the progress or quality of something over a period of time; keep under systematic review. - In tech that something can be a service you provide, pieces of hardware and software that are part of the service, user activity, your bill process, or any other activity you can observe. We'll call these our monitoring targets. 

## But why do we monitor? 
The short answer is to better serve our business. 
These services are important to the business so we want to understand what they're doing to ensure that they are working as expected. 
There are three core monitoring objectives. First, to detect when there're problems. This is often goes hand in hand with alerting. And second, to troubleshoot active problems so we understand where exactly in our system an issue is and get leads on how to resolve it. And third to do strategic work. Reporting and projections like capacity planning and optimization and assisting in longer term performance engineering of our absent systems.

## What do we want to monitor? 
 Pretty much every important aspect of the services we deliver besides monitoring a variety of targets, there are many different types of metrics to capture for each one. And picking up the right metric is important. 
 
 So to find out the important metrics to monitor for a target often include demand. What is being asked of it? For example, the volume and type of incoming requests. Workload, what and how is the service doing. For example, performance, how slow it is. Errors, what is going wrong with it. And, availability, is the service working in the first place? And resources, this includes system resource metrics like CPU and software resource metrics like connection pool capacity. 
 We can also look for specific business metrics that complement these technical metrics. 
 
 
 One, your systems were going down and two, your rat's nest of monitoring tools. - We first met working at a place where we had many tools collecting tens of thousands of metrics and sending hundreds of alerts a week. - All we could do was bail water and replace our on-call phone every once in a while when it was destroyed by an irate on-call engineer. But even with all that monitoring, it was hard to tell when there was really a problem, what was causing it, and most importantly, how to fix it. - As a result, we did a lot of analysis to learn how to monitor better. What we ended up with was a monitoring system that did what we needed and also a theory of monitoring that helped us understand how everything fit together. - Just like in the other areas of DevOps, tools, without the wisdom of how to use them, leaves you with a big mess. - Recently, the #monitoringlove hashtag has started to eclipse #monitoringsucks as the DevOps world has poured thought into how to get valuable monitoring effectively. - Up next we'll talk about observability and a DevOps approach to monitoring.
