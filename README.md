# Microservices-With-Spring-Student-Files
Student Files for Microservices with Spring course

# Microservices-With-Spring-Student-Files
Student Files for Microservices with Spring course

Monolithic Challenges
Also thick about the challenges that we are facing at present in the industry
New Type of client applications
	- smart phones, tablets, game consoles and TVs and new we even have watches and wearables and who knows where we're going next
	- The controllers that were designed for our web interfaces were not designed for these other channels
	- think about differnt kinds of networks like mobile networks that might be more 
	- also have to think about different types of peristence and backend technologies
	- lot of specialized technologies that offer really superior peristence for specific use cases.
	- seasrch can get it out of relational DB and use someting like elastic seasrch
	
Microservices
	- services are small independently deployable applications and not part of a single codebase 
	- not only can they evolve and deploy separstely but they do not necessarily need to be written in the same language or framework.
	- How social-services are going to consume set-fusion services and how changes may effect them. Consumer-Driven contracts
	- without the need for the centralized governance, micro services are free to use the best technology for the service needs and various types of storage technology is so called polyglot peristence.
	- can roll out changes to individual services as soon as they are ready without waiting
	- 

Spring Cloud
	- The goal of spring cloud is to address the needs of cloud based applications. Cloud based applications are distributed applications running in an environment that is best characterized as volatile and as such there are some common patterns that are needed to address the reality of this platform. 
	
	Distributed/Versioned/Centralized Configuration Management
	Service Registrtion and Discovery 
	Load Balancing
	Service-to-Service calls
	Circuit Breakers
	Routing

AWS already had afforable globally distributed compute and storage capability. However AWS didn't really address the software component. No one did. So Netflix had to invent the technologies needed to deliver a world class cloud based business. 

The spring cloud is to address common patterns in distributed computing which are commonly encountered in cloud native environments when using micro services architectures. Spring cloud makes it easy to use the main Netflix libraries.
	
	
Spring Cloud Configuration
	- profiles
	- spring.profiles.active=<profile-name>
	- export SPRING_PROFILES_ACTIVE=development  
	
	- Spring Cloud Config is about centralized versioned configuration management for distributed/cloud-native applications. 
	- Spring cloud config is all about providing a centralized location for applications to get their configuration information. 
	- It's externalised from the applications 
	- It is easy-to-reach source of application configuration regardless of what language is used.
	
	
	Config Server
		- EnvirmentReposiroy
		- 2 implementations are available, git based and native (local flat files)
		- The spring cloud config server is recommended to be running in multiple instances behind a load balancer. 
		- Spring Cloud Config is an application that is designated as a centralized server that serves up configuration information
		- The configuration information itself is backed by some kind of source control like Git. where actual code itself uses.
		 
	Config Client
		- spring.cloud.config.failFast=true
		- The spring cloud config client will connect to the Config Server at startup time then contribute the discovered properties to the main application context.
		- config clients abtain the settings from config server, what the client application is actually going to make a HTTP call the server URL that is specified. It actually will use the client application name as key and it will add the active profile. If there is no active profile, it will use the default profile. 

Service Discovery
	- this is about concept called passive service discovery
	- what is service descovery
	- We can use the config server for Eureka server location configuration to eliminate the need for each client to be configured with the location of the 	Eureka server
	eureka:
	  client:
        serviceUrl:
          defaultZone: http://localhost:8010/eureka
		  
	
	- As micro services will have large numnber of services that need to be interrelated and they need to call each other and it is very challenging to configure. So how can one application easily find all of the other runtime dependencies while this can be configured manually (even using something like spring cloud config server)
	- Service Discovery does is provide a single lookup service that is in essence itself maintaining because the clients register themselves and in the process they discover the other registrants.
	- 
	
API Gateway
	- it handles all of the complexity of talking to the individual services which it does on the server side where it's much more efficient. It can also handle thinks like caching, authentication, protocol translation and etc.
	
	- API Gateway provides server side backend and that is custom made for the needs of the client applications. It simplifies their access needs by providing a single point of contact for calling the backend services. 
	- API Gateway provides and API customized specially for what the client needs. 
	- It provides a single place in the architecture where security can be applied at least for authentication purposes.
	- It reduces the number of trips to the backend if gateway consolidate a number of individual backend calls into one call.
	- We have ability to implement caching in the API Gateway
	
	- API Gateway is built to address the remote client needs and it serves as facade to the real API. 
	- It reduces the number of calls and it can reduces the data carried back in the calls to the client
	- In simple cases it simply routes to the real backend services using Zuul from Netflix. The routing is quick and easy in adrresing some of our API needs without too much work. This simply means that the client can make a request that goes to the API Gateway and the gateway immediately sends the requests to the real server that's respomsible for it. 
	- API Gateway is a reverse proxy and calls to the various backend services
	
	- On startup API Gateway (Zuul Proxy) server, it first register with Service Discovery(Eureka) Server and learns of the client IDs to find the routing mappings. All URL paths configured in the API Gateway properties are mapped to a special controller called Zuul Controller which is provided by the framework and calls the actual Eureka identified service although it is load balanced and within a circuit breaker.
	
	- Once Zuul proxy is enabled, it registers itself with Eureka and it collects all client IDs reported by Eureka and the client IDs are used to build URIs to the actual backend services. The URL mappings are configurable in config server. Any traffic sent to those URI mappings would be routed to the real service and since Ribbon is involved internally, all the calls are automatically load balanced whenever there is more than one backend service instance available. Also since Hystrix involved, each call is made within a Hystrix command to get a simple circuit breaker behaviour. 
	
	- A request for /model path would be routed to wherever the model server service is running. A request for /entity path would be routed to wherever the entity server service is running through custom Zuul routing filter for identifing the pluggable entity service dynamically. 

	Caching
	
	- API Gateway can also optimize the calls to the backend API 
	- Caching is fairly critical when large number of microservices serving up largely static data. 
	- ability to setup caching between the clients and API Gateway entry point. In fact if the client is browser, the catching capability built into the client however the HTTP based caching capability doesn't work automatically with just clinet capabilities. It's upto the server to send the correct HTTP headers back to the client what it can cache and how long. These headers like expires, ETags etc. are well known in web development world. 
	- ability to setup caching between the API Gateway and the backend services
	
	- API Gateway addresses the protocol translation for different protocols like JMS, AMQP, SOAP etc. 
	

Load Breaker - Client Side
	- Traditionally load balancer are server-side components 
	- Traditionally a load balancer is a component that sits in front of an application usually web server and it has the purpose of distributing load.
	- The load balancer simply dispatches the incoming request to one of the real servers usually using simple algorithm like roundrobin. 
	- They may be implemented via software or often organizations will purchase special hardware applications to make a load balancing superfast and reliable.
	-
	
	Client-Side Load Balancer
	- a client side load balancer is part of the client application software and its purpose it to select which server to call. 
	- There may be and probably should be multiple copies of the server load balancer application that that the client can call.
	- The client side load balancer makes itself aware of what servers are available and makes its own choice about which one it should call at any given time. It make some decision based on some criteria, Perhaps something as simple a round robin or perhaps keeping track of which server is giving it the fastest response.
	- 
	
	- now you may be wondering, why this is necessary since we have server side load balancers and they work great.
	- You may not need client-side load balancing but this capability does allow you to a level of resilience and adaptability that is often needed in a microservice architecture. Not all server are same, servers can crash unexpectedly, load balamcer itself unavaialable.
	- Some servers unexpectedly slow down 
	- I expect that the application can make call to the server in the same region because it's fast. But what if it is somehow unavaialable. If my client side load balancer is aware of othe regions, it can route the calls to the other region even though the call  may be bit slower the application stays running. 
	
	Spring Cloud Netflix Ribbon
	- It is a client side load balancer and it has automatic integration with Eureka service discovery.
	- It also has support for the failure resiliency which we will cover later when we get to hystrix and it even supports like caching and batching
	- Caching is when you let the client avoid the remote call entirely because it has saved the results of similar recent call
	- Batching is when several requests are grouped into one call which is generally more effect
	- Now one interesting thing about ribbon is that it supports multiple protocols not just http. http, tcp, udp
	
	key ribbon concepts
	- List of Servers
		- This list can be obtained from the static source like populated via configuration or dynamically from a service discovery
	- Filtered List of Servers
		- A filtered list attempts to select a favorite subset of servers for use by the load balancer. In ribbon there are a number of stategies can be used. In spring cloud the default strategy is to include only servers in the same zone.
	- Ping
		- Used to test if the server is up or down. In spring cloud the default stategy is to delegate to Eureka to determine if server is up or down.
		- When client goes down and Eureka has not discovered this yet because there is a lit bit of delay between the time that Eureka stops receiving a ping and has decided to exclude that client from its list. These defaults can be overriden.
	- Load Balancer
		- It is the actual component that routes the calls to the servers in the filtered list.
		- In spring cloud the default load balancer is something called a ZoneAwareLoadBalancer which is bringigng the concept of zone. 
	- Rule
		- The rule is the single module responsible for making the server selection on whether to call or not
		- There are several implementations available, the default is something called ZoneAvoidanceRule

Client-Side Load Balancing augments regular server side load balancing by allowing the client to select a server based on some criteria. Spring Cloud Ribbon is an easy-to-use implementation of client side load balancing. 

Feign
-------
	- It allows to calls to RESTful service using declarative style which results in no actual implementation code. 
	- Here we are talking about the client side code that actually makes calls to some server side that's written in whatever type of technology
	- Alternative to RestTemplate and Feign declarative client is much easier.
	
	--- Pending notes
	
	- Ribbon is going to automatically load balance between the clients. Eureka gives all application Clients for the given Client ID so Ribbon automatically applies the load balancing. 

Feign provides a declarative way to call RESTful services and it integrates with Ribbon for load balcing and Eureka for list of Servers of the given Client ID.

 
Hystrix - Circuit Breaker pattern
---------------------------------
	- how software circuit breakers patterns protect against cascade failure 
	- will explore how spring cloud Netflix Hystrix annotations to implement circuit breakers
	- also will learn how to establish simple monitopting of circuit breakers using Hystrix Dashboard and Turbine
	
	Will explore the cascade failure scenario and how circuit breaker pattern can resolve this. How Spring Cloud Netflix Hystrix can easily add the circuit breakers to the system. 

	Cascade Failure
		- Whenever you have a large number of interacting services, there is always a possibility that one of them could be failure state for any number of reasons. The issue is the affect that the individual failures have on the other services. MSA is very vulnerable to this type of cascade failure. And without mitigating this, microservices are a recipe of certain disaster.
		
		

 In MSA, usually will have larger number of interacting service with dependencies,there is always a possibility that one of them could be failure state for any number of reasons that individual failures will have effect on other services which could lead to a 'cascading failures'. MSA is very vulnerable to this type of cascade failure. And without mitigating this, microservices are a recipe of certain disaster.
	
Distributed systema are more vulnerable for failures, fallacies of Distributed Computing.
In addition to the software failures, we could have a slow network, a node could drop out or unreachable, security policy cloud change, could suddenly not able to reach a node, etc. 

We have to take action to isolate failures to prevent cascade failures from resulting in significant outages for large percentage of time. And this leads us to the circuit breaker pattern.

The main point of circuit breaker is to detect a failure condition and to isolate and it stops it from becoming much more worse. In doing so it prevents a cascade failure.

Spring cloud Hystrix simply as a lightwight easy to use wrapper. just like physical circuit breaker, Hystrix detects failure conditions. When it start to get repeated failures from a remote service in short period of time, hystrix will respond by opening the circuit, it means that it's not going to allow further calls to take place. The default behaviour in hystrix is 20 failures over any five second window of time but that's adjustable. 

We can define what we want to do when the primary service call is not available. We could define the "fallback" method that gets executed in case of a service dependecy failure and the fallbacks can be chained. Hystrix is designed to reclose itself after an interval to see if the situations have improved. After all sometimes in cloud environment the failure is just that the given service is simply temporarily overwhelmed with too many requests by default. Hystrix will reclose the circuit after 5 seconds. 

When everything is your House is operating normally, We say that all of the circuit breakers are closed and when a failure occurs the breaker trips and we say that the breaker is open. Hystrix circuit breaker uses the same terminology. closed is good, open is bad. Hystrix failure definion is more flexible - essentially for exception thrown or timeouts occuring within a time interval. When hystrix command detects a failure, we have the option of determing a fallback behaviour. Automatically re-close itself and is configurable.  

If the remote service is in a failed state, the fallback will be run. hystrix allows to fine tuning regarding the failure detection and recovery behaviour.
The hystrix command gives lot of options for how the target logic to be invoked. The defaul behaviour is synchronous execution. 
The Hystrix command can be run asynchronouly or reactive.
The hystrix command can be invoked in asynchronouly to improve the throghput if I have multiple work items that do not necessarily need to be run in a sequencial order. 
 
Reactive is a asynchronous as well and even better because we don't need to query a future object to see if it's done. As we can have observable that will be executed as soon  as the work is done. The corresponding logic will be fired and therefore all we have to do is listen to the event. 
	 
	
Hystrix Dashboard
		- In any Microservices environment it is important to consider the monitoring of the various services. One of the items we should really keep an eye on is our circuit breaker and how often they are tripping and why. Hystrix Dashboard provides a built-in dashboard to showing the state of all the circuit breakers within an application. 
		- if we have large number of microservices, it isn't really practical to install and visit a dashboard on each one so the solution what we call turbine. turbine is simply is a single hystrix dashboard that listens to and aggregates the streams of all other hystrix enabled services and provides a consolidated view.
		
		
Summary
	- Software circuit breakers protect against cascade failure. 
	- Spring cloud Netflix Hystrix provides an easy way to add circuit breakers to the applications
	- You can use Hystrix Dashboard and Turbine to monitor your circuit breakers	
		
		
Building Microservices  by 
--------------------------		
HTTP and REST
	- GET retrieves a resource in an idempotent way, and POST creates a new resource.
	- We get to use HTTP caching proxies like Varnish and load balancers like mod_proxy, and many monitoring tools already have lots of support for HTTP out of the box. These building blocks allow us to handle large volumes of HTTP traffic and route them smartly, in a fairly transparent way.
	- 
		
HATEOS
	- Hypermedia is a concept whereby a piece of content contains links to various other pieces of content in a variety of formats (e.g., text, images, sounds).
	- The idea behind HATEOAS is that clients should perform interactions (potentially leading to state transitions) with the server via these links to other resources. It doesn’t need to know where exactly customers live on the server by knowing which URI to hit; instead, the client looks for and navigates links to find what it needs.
	- As long as these implicit contracts between the customer and the website are still met, changes don’t need to be breaking changes.
	- Personally, I am a fan of using links to allow consumers to navigate API endpoints. The benefits of progressive discovery of the API and reduced coupling can be significant. However, it is clear that not everyone is sold, as I don’t see it being used anywhere near as much as I would like.
		
		
Versioning
	- Use Semantic Versioning
	- With semantic versioning, each version number is in the form MAJOR.MINOR.PATCH. When the MAJOR number increments, it means that backward incompatible changes have been made. When MINOR increments, new functionality has been added that should be backward compatible. Finally, a change to PATCH states that bug fixes have been made to existing functionality.
	- /v1/customer/ or /v2/customer/
	- We expand the capabilities we offer, supporting both old and new ways of doing something.

Deployment
	CI
	- newly checked-in code properly integrates with existing code
	- CI server detects that the code has been committed, checks it out, and carries out some verification like making sure the code compiles and that tests pass.
	- Ideally, we want to build these artifacts once and once only, and use them for all deployments of that version of the code. This is in order to avoid doing the same thing over and over again, and so that we can confirm that the artifact we deployed is the one we tested.
	- It is a key practice that allows us to make changes quickly and easily, and without which the journey into microservices will be painful.
	- The approach I prefer is to have a single CI build per microservice, to allow us to quickly make and validate a change prior to deployment into production.
	- Each check-in triggers individual builds and each build produces a single artifact
	- 
		

How digital transformation in Microservices
	- modern devops style development processes where developers are not just writting and shipping code, and then somebody else runs it but they are actually operating services themselves and are being asked to responsible for the agility and the speed at which the services are upgraded and the liability of the services.
	- And when you put software developers on that role,hey automate. smaller team can move faster, deploy faster, they can ship at different rate from other teams and not dependent on others, they can decouple. that's how this is evolved and software development is moved from long cycle releases to almost daily and very quickly.
	- 
