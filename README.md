# DataCentreOptimisation

### Problem Statement

We are managing two data centres. We aim to buy, sell, and transfer servers, in order to meet customer demand of different types, and maximise profits.

One data centre is low-latency, the other high-latency. There are two types of server, CPU and GPU. Thus, there are four types of demand (low versus high latency) x (CPU versus GPU). Of course, high-latency demand can be satisfied by the low-latency data centre, but not vice versa. 

Demand is in integers, and changes every time-step. Every unit of demand that we satisfy gives us some amount of profit. This amount is different for the four types of demand. A unit of demand that is not satisfied gives us zero profit, of course.

Demand is dynamic, ie changes every time-step, but it is known in advance. Between time-steps we can purchase servers, sell servers, and even transfer servers from one data centre to the other. We assume this is instantaneous. Each server type has a different purchase cost (which costs us money), selling price (which gives money to us), and transfer cost (which costs us money). If data centre 0 chooses to transfer a server to data centre 1, then data centre 1 receives it - it does not need to choose to receive it. 

At the start, we have some number of each server type in each data centre. After the final time-step, we will buy and/or sell servers in order to return to the same number of each that we had at the start. The objective function will implement this automatically, ie the proposed solution doesn't need to do this. As a result, eg for $n=100$ the sequence will look like this:

    [initial servers give profit for day 0] 
    (commands 0 give income and expense) 
    [servers give profit for day 1] 
    (commands 1 give income and expense) 
    ... 
    (commands 99 give income and expense)
    [servers give profit for day 100]
    (automatically generated commands 100 give income and expense)
    [final servers, day 101, no contribution to profit]

And so for $n=100$ we require 100 sets of commands (commands 0-99 inclusive), and the history of how many servers we have will contain 102 items when including the initial day 0 and final day 101.

Our goal is to maximise the total profits.

At each time-step, we have four numerical variables representing the number of servers of each type, and four variables giving the demand at that time-step.

Our task is to solve this problem, comparing multiple algorithms / algorithmic ideas, and variants, and multiple hyperparameter values where appropriate, and report briefly on the results. 

### Data

The data is given in two files, `demand.npy` and `data.txt`.

Notice the values in `data.txt`. These values create a consistent economic model where:

* Low-latency servers are more valuable than high-latency;
* GPU servers are more valuable, more expensive to buy or sell, and more expensive to transfer, than CPU;
* There's a cost associated with moving servers.


### Representation

In the supplied code, a solution is a sequence of buy/sell/transfer commands, for each time-step, for each data centre, for each server type. A data centre can execute multiple commands in the same time-step. For a single time-step we need to represent (2 x 2 x 3) commands. Eg consider these commands for a single time-step:

    1, 2, 3,    4, 5, 6,      7, 8, 9,    10, 11, 12

This means: at this time-step, data centre 0 will:

    Buy 1 CPUs
    Sell 2 CPUs
    Transfer 3 CPUs to dc 1
    Buy 4 GPUs
    Sell 5 GPUs
    Transfer 6 GPUs to dc 1

Data centre 1 will:

    Buy 7 CPUs
    Sell 8 CPUs
    Transfer 9 CPUs to dc 0
    Buy 10 GPUs
    Sell 11 GPUs
    Transfer 12 GPUs to dc 0

We will have n of these rows, 1 per time-step. So, one possible solution representation for a complete solution is an array of (n x 2 x 2 x 3) integers. This is the representation used by the objective function code below.


### Experiments

You are free to use any metaheuristic algorithms, including HC, SA, GA, and/or any variant. You can implement your own, use code from class, or import libraries. You are free to use any encoding, which could be a bitstring encoding, integer encoding, or something else; a genotype-phenotype mapping, a repair mechanism, or similar; and/or custom `init`, `nbr`, and/or `crossover` operators. You are free to use a different objective function **during the run**, but remember that you must use my objective function for the **final evaluation**, for fair comparison of all methods. You are free to use ideas which you or others have suggested in class to improve the algorithms.

For each setup, you should run the setup 5 times with random seeds 0, 1, 2, 3, 4, and compare the mean performance.

If you wish, you can show curves to demonstrate behaviour, such as objective versus iterations, or diversity versus iterations, or specific characteristics of solutions, like total transport costs versus iterations. Some plotting code is supplied below.