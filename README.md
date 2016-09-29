# Serverless Architecture

Progress toward full utilization...

## What Is It

- Ephemeral computing
    - you don't provision servers
    - your code runs on demand
- Service coordination
    - your application is an assembly of services summing to more than the parts

Serverless means you focus on writing your code and assembling services.
You don't operate physical or virtual hardware.

## Why Do It

A primary reason to consider a Serverless implementation is cost. Most
applications don't come near to fully-utilizing their hardware. They often
have dark periods where no requests come in. Yet, if you provision a VM in the
cloud, you pay for it every second it's up, whether someon'e using it or not.

With an ephemeral implementation, you only pay for the requests and compute
time you use.

*Ephemeral Cost Example*

```
An Amazon Lambda Function costs $0.0000002 per request plus computing time. Say
you settle on a 256MB runtime environment, then your compute time costs
0.000000417 per 100ms. If your average request response time is 200ms, then your
total cost per request is $0.000001034.

An EC2 micro instannce costs $0.013 per hour. So your break-even is 12750 requests
per hour or 3.5 requests per second every second of every day.
```

Another big advantage is nearly-automatic scaling. You don't have to think in
terms of bringing compute units up and down.  Rather, you are only concerned with
any vendor throttling. If you keep an eye on that, it is easy to scale with
no sweat.

Another advantage is that you leverage the expertise of many other people in
your application. Most every service company now offers network API integration
that you can use easily in your code. An application today can simply be a static
web page, delivered by a CDN, that uses Javascript in the browser to simply
orchestrate 3rd-party services.


Finally, by not provisioning servers, you don't need much server expertise on your
team.  Your team is focused  on developing and depoying your application.

## Designing Ephemeral Applications

For ephemeral computing code, statelessness is key. Your code can be disposed of
at any time so you can't count on computations to last between runs.

Startup time is important because your application starts up many many times over
its lifetime.  Startup time is included both in our application latency and in
your compute costs.

Think of your code as a cooperating system of event handlers.

## Designing Service Coordinators

The service offerings will dictate your design.  Sometimes you will have to compromise
because you don't want to pay multiple vendors for the best solutions and instead
will work around a single vendor's offerings.

If you are designing a browser-based application, you will need a way to coordinate
signon.  You may need an ephemeral component to coordinate secrets for service invocation.

## Leverage a Devops Mindset

If you are already doing PaaS, or even IaaS with repeatable setups, you have the right
mindset for serverless. Make your setups repeatable. Make your setups repeatable.
Never, ever configure your app through a web application.


## Drawbacks

You have to care about bloated library code. If you run a Java container, you don't
really care because you only start it up once in a while. But if you are implementing
an ephemeral computation, you don't want 60 seconds of startup to run 300ms of compute.

Ephemeral operating environments don't offer many choices for implementation
technology. AWS Lambda offers Node.js, Java, and Python. That's it. Technically, in Lambda,
you can launch a native command by bundling it and shelling to it from your code but that's
not an easy-to-maintain solution.


Security is also a concern. With serverless, you often greatly increase the surface you
need to defend.  Some services, such as AWS Lambda, allow you to push your endpoints
behind VPCs but you still have 3rd-party services that need to be secured.

Some claim vendor lock-in as a disadvantage but I tend to disagree. The lock-in is largely
around provisioning and that occurs just as much in PaaS and IaaS environments.


## Things That Still Need To Be Worked Out

- Secret Delivery
- Fine-grained Throttling
- Service Discovery



