---
title: Simulated Annealing
date: 2012-09-28 02:41:31 -07:00
permalink: "/posts/2012/09/28/simulated-annealing/"
categories:
- Artificial Intelligence
tags:
- Education
- m tim jones
- programming
- Projects
- science
- technology
author: ameya005
comments: true
layout: post
wordpress_id: 36
---

Recently, I have been fascinated by algorithms generally used for AI. Simulated Annealing is one of the basic algorithms for finding solutions a constrained problem. I have been greatly helped along this path by a book : [Artificial Intelligence Application Programming(1st edition) by M. Tim Jones. ](http://http://www.flipkart.com/artificial-intelligence-application-programming-8177224271/p/itmdyufmmwhw849j?pid=9788177224276&ref=0dce851e-f9f2-4c4f-b499-3610d163ebfc)

Annealing is a process in which a metal is first heated to a high temperature and then slowly cooled down resulting in formation of large, ordered crystal structures. The output of this process is dependent on the time taken for the cooling. Faster cooling times result in formation of brittle, fragile structures while slow cooling results in the formation of large stable crystals.

Simulated annealing as the name suggests, is creating an algorithm which mimics this cooling process. Basically, several problems of varied nature can be solved by creating an analogy with the annealing process. Many problems can thus be equated to the annealing process. For example, given a minimization problem, we can equate the 'Energy' of the solution to the value of the objective function and then randomly tweak the values within the given constraints to find the minimum solution.

The algorithm for simulated annealing is as follows:

1. Create an initial solution.

2. Assess it's value

3. Randomly tweak the solution.

4. Assess the new solution

5.Check acceptance criteria

6. Reduce Temperature

7. Go to step 3

When the temperature reaches a certain predetermined value, the current solution which satisfies all criteria is outputted as the best solution.

To model this, let us assume that the lower the 'energy' of a solution, the better it is. Now for the algorithm to converge on a solution, we must propagate the solutions having lower values of energy. Thus we use the acceptance criteria for that. So, if the new solution is lower in energy than the current solution, we move on to reducing the temperature. But, if the working solution is worse, then we have an acceptance criteria which we can follow. In simulated annealing, we use an acceptance criteria based on the laws of thermodynamics.


[![](http://ameyajoshi005.files.wordpress.com/2012/09/mimetex-cgi.jpeg)](http://ameyajoshi005.files.wordpress.com/2012/09/mimetex-cgi.jpeg)


The following example is a basic example of simulated annealing.

N-Queens problem:

A very famous problem, it was first solved by Carl Friedrich Gauss in 1850 by trial and error. Since then, several methods have been used including divide-and-conquer, depth first, genetic algorithms etc. The problem in it's conception is very simple. We have an N x N chess board and we have N queens. Now, we have to place the queens such that there is no conflict between any two queens as per the familiar rules of chess. As a queen can move diagonally, vertically and horizontally, we have to ensure that for an acceptable solution, there must be only one queen in each column and row and it's diagonals must not have any other queen.

The formulation of this problem can be done as follows:

Lets take an 1 x N matrix. The index of the matrix is the row index of the position of the queen while the value at that index is the column index of the queen. Thus we have defined the position of each of the N - queens. Now, we randomly allocate each of the queens on the board such that no 2 queens occupy the same row or column. This can be easily ensured by checking the condition that the value at each index is not equal to any before it.

We define the energy of each solution as the no. of conflicts i.e. the no. of queens threatening each other. We also define several macros which which help us in the coding process

[sourcecode language="cpp"]

 #define MAX_LENGTH 30
 #define INIT_TEMP 30.0
 #define FIN_TEMP 0.5
 #define ALPHA 0.98
 #define STEPS 100

[/sourcecode]

MAX_LENGTH is the no. of queens i.e. N.

The temperature parameter basically defines the region in which the solution is being searched for. As the temperature decreases, the range of the solution space also decreases thus resulting in an optimum solution. The higher the temperature, the more time it will take for convergence but also there is a greater probability of finding a solution. Now, the final temperature cannot be zero as the value of the exponent then cannot be calculated. Thus, we try and take the value as close to zero as possible but at the cost of time. I tried several values and found out that at 0.5, the algorithm converges to a solution for even large N's in a fairly decent amount of time.

ALPHA is the value of the constant in the linear function


$latex T_{t+1} = \alpha T_{t} $


This function can be any decreasing function of T including non linear functions. I used the above one because it is the easiest to analyse. Now, at each temperature, we allow for 100 iterations for a best solution to emerge. This solution is then propagated to the next temperature. This can be imagined as moving up or down a hill until we find a ditch which represents over optimum value. Then we can start following that ditch until we reach the actual global minimum. To ensure, we stay within the constraints of the ditch, we decrease the temperature and keep decreasing it with every 100 steps until at last we find a solution.

Thus we can see that STEPS must be sufficiently large while ALPHA must be as close to 1 as possible so as to generate the optimal solution. But we must also keep in mind the timin constraints for the program to find a solution.

Now let us encode the problem:

We create a struct for each solution in which we create an array representing the solution and the energy associated with it.

[sourcecode language="cpp"]
typedef struct {
solutionType solution;
float energy;
} memberType;
[/sourcecode]

Now, we create several functions to help us with the program

[sourcecode language="cpp"]
void tweakSolution( memberType *member)
{
int temp, x, y;

x=rand() % MAX_LENGTH;

do
{
y=rand() % MAX_LENGTH;
}while(x == y );

temp=member->solution[x];
member->solution[x]=member->solution[y];
member->solution[y]=temp;

}
[/sourcecode]

This is a simple function which will help us create a randomly perturbed solution.

[sourcecode language="cpp"]

void initSolution( memberType *member)
{
int i;

for(i=0;i<MAX_LENGTH;i++)
{
member->solution[i] = i;
}

for(i=0;i<MAX_LENGTH;i++)
{
tweakSolution(member);
}

}

[/sourcecode]

This function initialises the first solution.

[sourcecode language="cpp"]
void computeEnergy(memberType *member)
{
int i,j,x,y,tempx,tempy;

char board[MAX_LENGTH][MAX_LENGTH];

int conflicts;

const int dx[4] = { -1,1,-1,1 };

const int dy[4] = { -1, 1, 1 , -1};

bzero((void*)board, MAX_LENGTH*MAX_LENGTH);

for(i=0;i<MAX_LENGTH; i++)
{
board[i][member->solution[i]] = 'Q';
}

conflicts = 0;
for(i=0;i<MAX_LENGTH;i++)
{
x=i;
y=member->solution[i];

for(j=0;j<4;j++)
{
tempx=x;
tempy=y;

while(1)
{
tempx+=dx[j];
tempy+=dy[j];

if(tempx<0 || tempx>=MAX_LENGTH || tempy<0 || tempy >= MAX_LENGTH)
{
break;
}

if(board[tempx][tempy] == 'Q' )
conflicts++;
}
}
}

member->energy = (float) conflicts;
}

[/sourcecode]

This function is used to compute the energy of each solution. We define energy as the no. of conflicts. Since, we have ensured that while tweaking, we avoid placing 2 queens on the same row or column, we can easily reduce the complexity of finding the no. of conflicts to checking the diagonals of each queen.

There are a few more functions for just simplifying the coding process and emitting the putput for better readbility and understanding

[sourcecode language="cpp"]
void copySolution( memberType *dest, memberType *src)
{
int i;

for(i=0;i<MAX_LENGTH;i++)
{
dest->solution[i] = src->solution[i];
}

dest->energy = src->energy;
}

void emitSolution( memberType *member)
{
char board[MAX_LENGTH][MAX_LENGTH];

int x,y;
bzero((void*)board, MAX_LENGTH*MAX_LENGTH);

for(x=0;x<MAX_LENGTH;x++)
{
board[x][member->solution[x]] = 'Q';
}

printf("board: \n");

for(y=0;y<MAX_LENGTH;y++)
{
for(x=0;x<MAX_LENGTH;x++)
{
if(board[x][y] == 'Q')
printf("Q ");
else
printf(". ");
}
printf("\n");

}
printf("\n\n");

}
[/sourcecode]

Now, we implement the actual simulated annealing algorithm in the main()

[sourcecode language="cpp"]
int main()
{
int timer = 0,step,solution=0, useNew, accepted;

float temp = INIT_TEMP;
memberType current,working,best;

FILE *fp;

fp=fopen("stats.txt", "w");

srand(time(NULL));

initSolution(&current);
computeEnergy(&current);

best.energy=100.0;

copySolution(&working, &current);

while( temp > FIN_TEMP )
{
printf("\n Temperature = %f", temp);

accepted = 0;

/* Monte Carlo step*/

for( step = 0; step < STEPS; step++);
{
useNew=0;

tweakSolution(&working);
computeEnergy(&working);

if(working.energy <= current.energy)
{
useNew = 1;
}
else
{
float test = rand() % 1;
float delta = working.energy - current.energy;
float calc = exp(-delta/temp);

if(calc > test)
{
accepted++;
useNew = 1;
}
}
}

if(useNew)
{
useNew = 0;
copySolution(&current, &working);

if(current.energy < best.energy)
{
copySolution(&best, &current);
solution = 1;
}

else
{
copySolution(&working, &current);
}

}

fprintf(fp, "%d %f %f %d \n", timer++, temp, best.energy, accepted);
printf("Best Energy = %f\n", best.energy);

temp *= ALPHA;
}

fclose(fp);

if(solution)
{
emitSolution(&best);
}

return 0;
}
[/sourcecode]

In the main program, we have created two loops. The outer loop deals with the temperature and reduces the temperature after every STEPS steps. The inner loop on the other hand is an implementation of a [Metropolis algorithm](http://en.wikipedia.org/wiki/Metropolis_algorithm). I will surely discuss the Metropolis algorithm in detail in some other post. But, for now, what it basically does is that it picks up a tweaked solution and computes it's energy. If the energy of the solution is lower than the energy of the current solution, then it replaces the current solution by the tweaked solution. But if the energy is higher,  it calculates the difference between the current and the tweaked energy, calculates it's probability as according to Formula 1.1 and then compares it to a randomly generated threshold value.

The reason for doing this is that if the algorithm encounters a local optimum, it may get stuck in the same place thus causing the algorithm to fail. Due to the Metropolis Algorithm, we avoid this by allowing the solution to take values which may not be the best for that iteration.

The solution that we have thus far obtained is then compared to the best solution we have so far. If it's energy is lower, we copy the new solution into the best one at the end of each temperature change.

The program will terminate when we reach our chosen final temperature thus giving us the optimal solution.

Now that we know 'Simulated Annealing', we can apply it to solve problems in various domains,



	
  * Path generation

	
  * Transportation problems

	
  * Image Reconstruction

	
  * Circuit layout


Here's a screen-shot of the output.

[![](http://ameyajoshi005.files.wordpress.com/2012/09/simannealing.png)](http://ameyajoshi005.files.wordpress.com/2012/09/simannealing.png)

Much of the source code is provided in the above mentioned book. The following post will proceed on with Ant Algorithms. Thanks for reading and please contact me for any questions.
