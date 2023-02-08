# Our approach to microservices

Most people today think the ideology of splitting software into smaller pieces came from the microservices architecture, however, this is a good practice established and followed in software development and strictly followed by the engineers at xgeeks.

A perfect example is the “Unix philosophy”, which is not only a few good practices written in a paper but something proved and tested in Unix itself. There are some of the main rules they followed:

* Write simple parts connected by clean interfaces
* Design programs to be connected to other programs
* Separate policy from mechanism; separate interfaces from engines

No, they have not described microservices.

These principles were defined back in the 70s and they are referring to the good practices we should adopt in code design.

The boost of the microservices was caused by the growth of cloud-native tools in the last years that turn it easy to apply it in every software solution we see today. Most of the cloud solutions prioritize automation, virtualization and containerization, leading us to different kinds of infrastructure we were used to, mainly because it turned easy to implement a distributed system in form of microservices. Some of the tools that contributed to this boost were:

* Docker: which popularized containerization;
* Kubernetes: that allowed to orchestrate of the containers and all the infrastructure around them;
* CI/CD tools: such as GitHub Actions or Flux CD, contribute mainly to simplifying the deployments of new features, integrating them easily and quickly in a production environment.

## Implementing microservices is easier nowadays

Let’s go deeper into a common microservice architecture and how it’s easier to implement a distributed system without the complexity that we faced back in the old days.

[microservices](microservices.png)

The previous diagram illustrates a very simple architecture of how microservices can be implemented. Each of the represented services has its own code repository and they work independently of each other.

Since they have different code repositories, different teams can work in parallel without affecting the workflow of the other application.

Another advantage here, it’s the possibility to use containers to deploy our application instead of using only virtualization and running the application directly on it. So, Docker has an important role here since it allows us to recreate and start the application faster and easier.

In terms of infrastructure, we commonly see Kubernetes in most of the cloud-native solutions. Kubernetes working together with Docker offer the developer an easier way to define some points of the application infrastructure without the need to implement mechanisms to do so. One of the best examples is the Deployment resource of Kubernetes. There we can define the number of replicas, the type of deployment and define other containers to run together with the application on the same pod.

If you are not familiar with Kubernetes, Pod is the smallest deployable unit which can contain one or more containers. The replicas are defined at the Pod level.

So, we just need to define a couple of YAML files representing the resources we want to create in the Kubernetes cluster and apply it. Note that this is a simple example and the complexity of the problem affects consequently the way we need to build the solution.

Since we can define the infrastructure in a declarative way, we can put these files in our application repository as well and use the code versioning tool to manage the changes and the upgrades made on it.

At this point, we have our repository with all we need to deploy the application, so that’s the time we can bring to the table the CI/CD tools. If you are using GitHub, you may know about GitHub Actions.

With this tool, we can define our pipelines affecting the specific repository with all the steps until the deployment in production. This includes the common steps we should have on the pipeline, like the build step, the testing phase, the deployment on a test environment and finally the deployment in production.

Of course, you may need to adapt this CI/CD pipeline according to your needs and the application's complexity.

## Implementing this back in the early 21st century

Let’s go to a little exercise and go back in time to see how difficult it is to implement the same architecture.

Imagine a world without Kubernetes and Docker. It’s painful, I know, but we have already been there.

Without Docker or any other solution for containerization, we probably would need to use virtual machines, and each VM represents a replica of our service. Otherwise, you may choose to use different instances in the same VM to replicate the application, but you are not really doing the same as we do with Kubernetes, assuming you have the pod replicas in different nodes.

Following this approach, one VM for each replica of your application, can you imagine the cost of it? The virtual machines can take a lot of storage space from their host and the capacity of regeneration as we see with containers is quite slow because we need to rebuild the image with all the systems installed on it.

Then you probably would have an ESB working as a middleware to establish a common place to manage the communication between the different services, providing also load balancing and service discovery.

Are you imagine presenting this solution back then? Yeah, probably not. At this point, you are remembering why microservices weren’t a solution or even an idea in our heads in terms of architectural design.

Now we can understand how some tools, such as Kubernetes and Docker, launched microservices into the spotlight and turn it into tech trends in the last few years.

## It’s a trend, we need to use it everywhere

Since it was easier to implement and became somehow a trend in software architecture, its use of it became the first option independent of this case we are working on.

We are all aware of the advantages of microservices, such as:

* Scalability
* Reusability
* Simpler to deploy
* Improve team management
* Fault tolerance

But, as with everything in life, it has its downsides and problems if it is not applied correctly and for the right purpose.

One of the common errors that people make is to start using it at the beginning of a new project. This is highly not recommended since it can lead to costs you are not thinking about at this point. This happens because we start believing that we already have the infrastructure and the automation mechanisms ready, but this is part of the work before you even think about building microservices.

We usually say that microservices reduce the complexity of the application, but we are just passing the complexity to other levels. You might need to think about logging centralization, monitoring and other points in the infrastructure you might not be used to thinking about.

The testing is more manageable if you are talking about the unit tests because they are restricted to the microservice you are testing. However, integration tests become a nightmare. In this architecture, you can’t test all the microservices involved on a specific functionality locally, even to integrate them in your pipeline, you will need to build an independent test environment and use an external tool to run the tests. Resuming, more complexity and more costs to your pocket since we need more resources to implement the test environment.

The communication between the different microservices starts to use the network and with that, all the problems associated with it start to appear. Instead of the flow passing through the same application we need to define interfaces to allow this communication, mainly using REST APIs. This can not be a problem, however, if we start dealing with huge payloads in this communication through the network, you probably will see some latency.

At xgeeks we believe good engineering is understanding the problem and tackling it with the best tool, not the trendy tool and this post aims to show how we look at cloud-native plus microservices and demystify its hype.

## Most of the time you can solve it with a monolith

One of the biggest advantages of micro-services is that you can work on a particular service, and if no changes are done to its interface then no other service is affected. But one of the concepts that are often overlooked is the Modular Monolith, in reality, a modular monolith is a monolith done right. One thing that can be said for sure is that if you’re not able to properly design a monolith then you sure won't be able to design a microservice architecture.

Here we argue that most of the benefits of microservices can be achieved with a well-thought-out monolith, that way the hardest parts of a fully distributed system are not present, and it might actually be the best option for the project you are working on.

If the separation of domains is not clear and concise, then you end up with a distributed monolith, which is one of the biggest pitfalls of microservices. So if you implement a truly modular monolith you are much closer to migrating to microservices if needed.

The ability to deploy a single part of your system is one of the hallmarks of microservices. Let’s say you update a feature and you want to do some beta testing in production, you are now able to deploy the affected service and only route 10% of your requests. Again monoliths can achieve the same, if code is structured in a way that a new feature doesn’t touch multiple contexts, then from the programmer’s point of view it's just the same. The caveat is that you have to deploy the whole system, which incurs more costs, something we’ll look into in more detail further ahead.

Reliability is another big topic, but reliability is more about using cloud-native tools and the way you use them. It's not because Netflix uses a microservice architecture that their application has close to zero downtime. It’s the strategies and methodologies they put in place that allow them to keep operating as usual when a whole AWS region becomes unstable. (lint to chaos monkey article here)

Just like with microservices, with monoliths, you can still apply various design patterns like DDD. One of the benefits of having a full microservice architecture is that data is bounded to its own domain, and each microservice has its own data storage. That guarantees that some other piece of code(or service) won’t do unexpected changes to that data.
Again monoliths can achieve this! Even if the database server is shared by all modules, if well implemented a great deal of domain separation can be achieved with proper design.

A good modularized architecture with clear domain separation, won’t get in the way of your organization and the way you structure teams. You can still build teams around certain contexts. At xgeeks we keep that in mind, where a team is tasked with developing a finite number of modules (or microservices) that encapsulate a set of features, that way we allow our engineers to be specialists in their own domain, with full ownership.
Git is an amazing tool and when you’re able to implement a new feature without making changes across multiple domains (unless required by the nature of the feature), integrating new code is not much more complicated than managing it across multiple repositories, in some scenarios it might even be the best option. Managing a mono repository has its complications, but it has the advantage that your CI/CD pipelines are all in one place.

The bottom line, modularity comes more from domain separation than from pieces of code running in multiple containers.

Just like in microservices, you can make use of another design pattern to implement your modular monolith. DDD is a good example of it, you can start with somewhat big modules that encapsulate a bounded context, that you can further break apart into multiple modules each with fewer aggregates.

One thing you cannot do with a monolith is used multiple languages, but in the real world, rare is the case that there is a real need to use specific technologies for certain tasks and to be able to develop and support a heterogeneous system, you need a mature and robust organization. Otherwise, you are just going to end up with bad code in different languages, or with whole parts of the system only owned by a very small group of very expensive specialists.

When we talk about modularity, two big concepts arise, coupling and cohesion. Here Sam Newman said it best in his book, Monolith to Microservices:

`"Cohesion and coupling are concerns regarding modular software, and what is microservice architecture other than modules that communicate via networks and can be independently deployed?"`

Just to reiterate, modularity is one of the biggest concerns of microservices, but that is not inherent to them that comes from a project that has been designed and structured to have the highest standard of software design.

## It’s not “if”, but “when”

As we have discussed so far, the point is not if we should use microservices, but when this architecture fits better.

In fact, choosing to follow this architecture in the early stages of a project, without having the support and the expertise on the team side, will create an impact on the solution and more precisely on the business.

So, the question you need to ask is “When should I use a microservices architecture?”

There are some guidelines that can help you understand when you should use this approach:

* You need to migrate a monolithic application in order to improve scalability.
* Common services that work independently in the system where it is implemented, for example, authentication services.
* You require that your services be highly scalable and easier to implement continuous delivery in terms of new features.
* You have already an infrastructure and a team capable to give support to the system.

Apart from the microservices, in xgeeks we follow some principles to ensure that distributed teams can contribute to the system independently by following three main principles:

1. Code design
As we have said before, microservices can lead us to make the teams more independent and autonomous, however, this can be the opposite in some cases. One example is when you start to create microservice tightly coupled to each other and run them in separate deployments. This will not make your software modular, you are just creating a distributed monolith without taking any benefit from it.

As we said before, Modularity is the point we want to reach, being the right way to design code, and consequently, “microservices ready” is with domain-driven design most of the time. By understanding the boundaries of each Bounded Context we will be able to understand the granularity of each service and how we can optimize for it to be as independent as possible from other services. Having clearly defined Bounded.

2. Team topology
Taking the previous topic, in xgeeks we believe the best way to organize teams is to take into account the code design itself. If the code design applied is based on the business domains and as independent as possible from each other, the teams will be able to add more value to the overall system by clearly separating their responsibilities by domain.

3. Independent release pipeline
Since we have our code design properly defined and it has the domain's context well defined, we not only have the teams split by domain, but also we can define different and independent releases pipelines.

## Final thoughts

As with everything in software engineering, in xgeeks, we take all the information about one solution and verify if it resolves properly our problem, without generating other ones more complex.

Microservices are really good when we have all the conditions discussed in this article joined together, otherwise, you must have the notion that you don’t need to apply it in everything you work on just because someone else is using it.

**Prioritize the code design in order to achieve modularity** so you don’t need to think about microservices in the first place.
