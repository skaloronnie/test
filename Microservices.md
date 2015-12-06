http://highscalability.com/blog/2015/12/1/deep-lessons-from-google-and-ebay-on-building-ecosystems-of.html

Modern large scale systems compose services in a graph of relationships, not a hierarchy or set of tiers.

These best performing systems are more the product of evolution than intelligent design. At Google, for example, there never has been a centralized top down design of the system. It has evolved and grown over time in a very organic way.

develop from the bottom up. Clean design can be an emergent property rather than a product of top down design.

eBay of 2004. There was an architecture review board, which had to approve all large-scale projects.
A better way for eBay to have handled the situation was to encode the knowledge of the smart experienced people in the review board and put it into something that’s reusable by individual teams. Encode that experience into a library or a service or even a set of guidelines that people can use on their own rather only coming into the process at the last moment.

The parts of communication that are usually standardized:

Network protocols. Google uses a proprietary protocol called Stubby. eBay uses REST.

Data formats. Google uses Protocol Buffers. eBay tends to use JSON.

Interface schema standard. Google uses Protocol Buffers. For JSON there’s JSON schema.

The easiest way to encourage best practices is through actual code. It’s not about top down review, or up front design, it’s about someone producing code that makes it easy to get the job done.


The team, typically a small teams, owns the service from design, through development, and deployment, all the way through to retirement.

Teams have the freedom to make their own technology choices, methodologies, and working environment.

There’s no need to understand all the other services in the ecosystem.

A team needs to understand their service in depth and the services they depend on.

Conway's law: organizations which design systems ... are constrained to produce designs which are copies of the communicationstructures of these organizations

Think about relationships between services as vendor-customer relationships, even though you are at the same company.


Charging for a service aligns economic incentives. It motivates both sides to be extremely efficient about the use of resources.

When things are free we tend not to value them and tend not to optimize them.


Maintain full backward / forward compatibility of interfaces.

Never break client code.

This means maintaining multiple interface versions. In some nasty situations it means maintaining multiple deployments, one for the new version and others for older versions.

Usually because of the small incremental change model interfaces are not changed.

Have an explicit deprecation policy. Then the service provider is strongly incented to moved all clients off version N and over to version N+1.


Services at scale are very exposed to variability in performance.

Predictability in performance is much more important than average performance.

Low latency with inconsistent performance is actually not low latency at all.


eBay made use of feature flags to decouple code deployment from feature deployment.


A service that does too much is a just another monolith. It’s hard to reason about, it’s hard to scale, it’s hard to change, and it also creates more upstream and downstream dependencies than you want.



In the tiered model services are put in the application tier and the persistence layer is provided as a common service to the applications.

They did this at eBay and it didn’t work. It breaks the encapsulation of the service. Applications can back-door into your service by updating the database. It ends up reintroducing coupling of services. Shared databases don't allow for loosely coupled services.

