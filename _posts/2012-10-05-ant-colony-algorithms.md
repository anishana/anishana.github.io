---
title: Ant Colony Algorithms
date: 2012-10-05 04:14:48 -07:00
permalink: "/posts/2012/10/05/ant-colony-algorithms/"
categories:
- Artificial Intelligence
tags:
- Education
- m tim jones
- programming
- Projects
- Research
- technology
author: ameya005
comments: true
layout: post
wordpress_id: 78
---

Hey guys, Thanks for the great response on my previous article.

Today, I will be writing about a brilliant algorithm called Ant Colony Algorithm. Ant Colony Algorithm or Ant Algorithm as it is popularly known as, is basically an optimization algorithm based on [swarm intelligence](http://en.wikipedia.org/wiki/Swarm_intelligence). It mimics the behavior of an ant colony foraging for food. This algorithm was first proposed by [Marco Dorigo](http://en.wikipedia.org/wiki/Marco_Dorigo) in 1992 as his PhD. thesis. It is a part of the amazing world of swarm intelligence wherein a swarm of low ability agents are used to solve complex problems.

Ants are perhaps one of the most amazing creatures in this world. They are nearly blind but still mange to navigate through difficult terrain and find their way to food and back. Not only that, they can lift up to10 times their body weight and perhaps one of the sturdiest creatures of the insect world. The major reason of their success in the battle for survival is the fact that they co=operate with each other. The survival of the ant colony is foremost rather than survival of a single ant. It is this co-operation and communication that we attempt to model through ant algorithms.



Now, ants have an amazing method of navigation. They use pheromones to communicate the optimal path to any point to other ants. This works in the following way.



	
  1. An ant finds a food source.

	
  2. It lays down a trail of pheromones which can be detected by other ants upon the path from the food source to the storage area

	
  3. Other ants also reach the food source through various paths.

	
  4. Now, based on the time taken to complete each trip to and fro, the shortest path will have the most pheromone as it has been traversed the most.

	
  5. As the amount of pheromone on each path dictates the probability of that path being taken, more and more ants will be traveling on the optimal path. Thus, using several ants, the optimal path to a food source is obtained.


We attempt to mimic this behavior by using agents that we call ants. To solve a problem using Ant algorithms, we model it as a fully connected bidirectional graph. You can read up more on graphs and graph theory [here](http://en.wikipedia.org/wiki/Graph_theory). On each node, we place an ant and then ask it to traverse the other nodes on the graph thus completing a trip. The constraint on the ant is that it must visit each node only once. As it travels, we calculate the pheromone levels on that path based on its length. Upon the completion of each trip( known as a **Tour**), we replace the ant on a node and send it on its way again. But each time, it reaches a node, the probability of it taking the next edge is given by the amount of pheromone on that edge. Thus, in the end, through traversing the graph, we find out the most optimal path.

For example, lets take a situation with 2 ants and a food source. Now Ant1,  who will call Han takes a path with 2L distance to the food source whereas Ant2, Luke, takes a path with distance L. Now both lay down small amounts of pheromone as they pass. Now Luke completes the trip to the food and back in time T, while Han takes 2T, assuming both travel at the same speed. He takes the food and returns back. And then proceeds on his second trip. While he starts of his second lap, Han is still just at the food. So, when Han returns back, Luke will have completed two trips. Thus, there is approximately twice the mount of pheromone on the Luke's path as compared to Han's path. So, next time, Han will be more probable to taking Luke's path based on simply the amount of pheromones.
So, how do we calculate the probability of an ant taking the path? Before that, we also add another factor to the situation- that the pheromone evaporates as time goes on. Thus, paths which are poor are discarded more easily. Now, we need to describe the probability equation governing the movement of ants.

While an ant has not completed a tour i.e. it has not visited all the nodes, the probability of the next edge to take is given by the following equation:


[![](http://ameyajoshi005.files.wordpress.com/2012/10/ant1.jpg)](http://ameyajoshi005.files.wordpress.com/2012/10/ant1.jpg)




Here $latex \tau(r,u)$ is the intensity of pheromone on the edge between the nodes r and u. $latex \tau(r,u)$ is actually a heuristic function which represents the inverse of the distance between the nodes, $latex \alpha $ is the weight for the pheromone, and $latex \beta $ is the weight for the distance of the edge which can be modeled after visibility. $latex k $ is the no. of edges not traversed yet. This formula can be said to be the ratio of the weight of an edge to the sum of the weights of all untraveled edges. Thus, this will give us a probability based on the amount of pheromone deposited and the distance of the edge. Both of these values will help us in converging to an optimal value.




Now, we calculate the pheromone left on each edge only at the end of each ant tour. We do this using the following formula:




[![](http://ameyajoshi005.files.wordpress.com/2012/10/ant21.jpg)](http://ameyajoshi005.files.wordpress.com/2012/10/ant21.jpg)




Here $latex Q$ is a constant and $latex L^{k}(t)$ is the total length of the tour. We then use this value to calculate the total pheromone value due to a single ant after a tour.




[![](http://ameyajoshi005.files.wordpress.com/2012/10/ant3.jpg)](http://ameyajoshi005.files.wordpress.com/2012/10/ant3.jpg)




We have to note that this operation is carried out only after the tour is completed as the pheromone levels are inversely proportional to the length of the tour. Constant $latex \rho$ is a constant between 0 and 1.




As I have mentioned before, we consider pheromone evaporation as a way to discard poor paths. The evaporation is modeled by a simple equation




[![](http://ameyajoshi005.files.wordpress.com/2012/10/ant4.jpg)](http://ameyajoshi005.files.wordpress.com/2012/10/ant4.jpg)Here, we are using the inverse coefficient of the weight of the change in pheromone levels on a n edge to model evaporation. This proves to be an efficient way to discard edges which are not optimal.




As each ant completes it's tour, we update the edges based on the equations above. Then, we restart the entire process with the ants being placed on nodes once more and then allowed to move according to the updates edges. hence, we can see that more and more ants will converge to a single path. The no. of iterations can be controlled so as the algorithm ends when there is no change in the value of the shortest length for a few iterations.




[![](http://ameyajoshi005.files.wordpress.com/2012/10/aco.jpg)](http://ameyajoshi005.files.wordpress.com/2012/10/aco.jpg)




Thus, we see how a complex natural system is modeled and used to solve problems. A few applications of Ant Algorithms include






	
  * Network Routing

	
  * Graph Coloring

	
  * Vehicle Routing

	
  * Path Optimization

	
  * Swarm Robotics


This concludes this post. In the next post, we will use the discussed Ant algorithm to solve a famous computational problem - The Traveling Salesman Problem.
