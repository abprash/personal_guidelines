Microservices vs Monolithic architecture

1. Monolithic
2. Microservices

A
1. Its the entire application packaged into one format and deployed. Its just deploying one single file onto a server. Can be a WAR file or a single NodeJS directory.
2. It is basically different from monolith in the matter that its deployed as a set of processes on servers. Depending on the need we can either scale up or down of particular processes alone. They (the processes) can communicate via Interprocess communication.

B
1. They are tightly coupled. Makes it hard to replace entirely parts of the system without thorough testing. 
2. We can componentize them. In the manner that, We can independently replace/ upgrade individual components.


C
1. The tech is  focussed on infrastructure. Eg. UI -- Server -- DBA.
2. The tech is built focussed on business needs. So there are teams who own a particular component fully.(even though that component may be fully be enveloping ui, server and DB)

********** Most important difference between the two thinking

D
1. There is a smart app layer. The infrastructure is smart and manages both ends.
2. There are smart end points and dumb pipes. The pipes are there just for communication. 

***********


E
1.
2. Decentralization of the data layer. How does that even work? replication?? Never share the data directly. Each service has its own database. All data sharing has to go thro the services that WRAP the data.


F
1
2. Lot of infratsructure automation. CI and CD comes with it naturally.


G
1. 
2. Microservices should be able to handle a chaos (or Armageddon ) monkey kind of service disruption.




***************** the most important question being, Is microservice SOA?????
===> Well, it really also depends on what SOA actually means.
SOA is a super set, which also has a very specific flavour called - microservices. 	



********************* When to use microservice vs monolithic style???
Monolith:
	Pros:
		1. Simple and straightforward in most cases
		2. In terms of business, more people available who know this.
		3. 

	Cons:
		1. Its easy to break the modularity.
		2. Can be a challenge to scale horizontally.
		3. Can be a challenge if many instances are running using the same data source.

Microservices :
General rule of thumb - If you don't know if you want to use a microservice - stick with a monolith.

	Pros:
		1. Deployments is easier. Individual services can be deployed independently. (But however there are companies who still do effective deployments of huge monoliths)
		2. If done right, we can easily scale up for higher availability.
		3. We can potentially use many different software and database stacks for each service. (because some langs are great at some tasks. So a good balance would be to have a small subset of languages which work well together.)
		4. Modularity is enforced. Its hard to break the modularity.
	Cons:
		1. introduces asynchrony. Asynchronous messaging is always more complex.
		2. Consistency goes for a toss.
		3. If the service boundaries have been incorrectly set up, then its much harder to fix them later down the road - since the services are distributed. (whereas for monolithic archi - its much easier to fix them, because they are in the same executable and codebase)






************ I would stick with monolithic architecture. Only if there is a particular need for it I would go.


******* Do not go into microservices until you can provide the following capabilities.
  1. Basic monitoring should be up and ready. (we should react if any of our services go down.)
  2. Should be able to provision more machines in a matter of hours.
  3. Should have a CI/CD pipeline in place. (because this approach increases the complexity of ops a lot)
