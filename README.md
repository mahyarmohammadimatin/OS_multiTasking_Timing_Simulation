<img src="https://github.com/mahyarmohammadimatin/OS_multiTasking_Timing_Simulation/blob/main/52.jpg">

## Overview
I have chosen the Multi-Tasking algorithm (concurrency of processes) from among the operating system algorithms. Previously, I found the idea of the operating system handling tasks concurrently fascinating, so this time, I decided to implement this algorithm myself. In this project, both parallelization and concurrency are implemented. Parallelization means that one CPU core switches between tasks so quickly that we think they are all running simultaneously. However, concurrency means assigning multiple CPU cores to handle tasks concurrently.

Additionally, I have manually written an algorithm for generating random numbers, which utilizes the system clock to assist in generating these numbers. There is also a validity test included in the code to ensure that our random numbers are truly random!

In this project, we consider a task as a set of processes, each of which requires a certain amount of memory and a specific execution time. When a process is in progress, it resides in RAM and its interactions with the CPU are calculated. It's important to note that RAM capacity is limited and provided by the user as input. If there is not enough space in RAM for processes, the operating system uses virtual RAM, which is essentially on the hard drive and has a relatively slower speed. This is because transferring data from the hard drive to RAM takes some time. The user can also manually set this time period. Additionally, we will proving Amdahl's Law in this project.


## Project Structure (Classes)

### Process
This class represents an input process to the operating system. As previously mentioned, it occupies a certain amount of memory and
requires a specific execution time, both of which need to be set. Additionally, it has the capability to be locked, which assists us in the
implementation of the concurrency algorithm.

Another crucial variable is **need_prev**. When set to true, it signifies that this process cannot be executed unless all preceding processes within 
the same task are completed. This value is probabilistically determined based on user input. This parameter plays a pivotal role in establishing 
dependencies among processes. The more interdependent processes are, the less effective concurrency will be. We will validate this through a controlled 
experiment.

### Task
Each task consists of multiple processes, and the user specifies the number of processes as well as the average memory and time requirements
for each process within the task.

In the constructor of this class, as it is evident, a user-defined number of random processes are generated and placed in the queue specific to this task.

Furthermore, all processes are subject to a dependency probability, **prev_prob**. The higher this probability, 
the less our ability to execute commands concurrently.


### Core
It can be considered as the "brain" of our operating system. Multiple instances of these cores are placed within the CPU, 
ensuring efficient handling of processes.

The core's execution process involves parallel processing of tasks. Upon encountering a task, it checks if it's completed; if so, it proceeds to the next task. Otherwise, it tackles the first unfinished process. If a process is locked, it moves to the next in the task. It considers process dependencies based on the need_prev parameter, skipping locked processes. When an unlocked process is found, it locks, sleeps, and unlocks after execution.

Once a process within a task is executed, the core doesn't continue with that task, adhering to the parallelism rule, and moves on to other tasks.

### CPU
In this class, we initially create cores based on the user-provided input and set the values for RAM and virtual RAM. In the run function, we execute all CPU cores in parallel. We also calculate the start and stop times to measure the duration it takes to complete all tasks.




## Usage
Import the necessary libraries.

Create a CPU instance with the desired number of cores, RAM capacity, and virtual RAM capacity.

Define tasks, processes, and their attributes.

Run the CPU to execute the tasks.

Monitor task progress and execution times.

## Testing
Through the code written above, we get the average time needed to solve fixed tasks by the CPU under different conditions. Once we set the CPUs in terms of the number of cores equal to the numbers 1, 2, 5, 10, 20 and 50. 
Once we change the degree of dependence of the processes on each other in the range of 0 to 1.

<img src="https://github.com/mahyarmohammadimatin/OS_multiTasking_Timing_Simulation/blob/main/50.jpg">

As evident, the more processes are interdependent, the less effective adding more cores becomes. This is because the potential for parallelization
diminishes, and some cores remain idle while waiting for tasks to become available. Although it seems that the execution time for processes takes
10 seconds when handled by a single core, it appears that using more than 5 cores doesn't significantly improve performance,
especially when process dependencies are high.

## Proving Amdahl's Law

Amdahl's Law is a principle in computer science that tells us the following:

When you try to make a program run faster by using multiple processors or cores, the maximum speedup you can achieve is limited by the portion of the program that cannot be parallelized.

It defines a formula to calculate the maximum speedup, taking into account the fraction of the program that can be parallelized (P) and the number of processors (N).

Amdahl's Law emphasizes that no matter how many processors you add, if a significant portion of the program cannot be parallelized (i.e., is inherently sequential), the overall speedup will be limited.

<img src="https://github.com/mahyarmohammadimatin/OS_multiTasking_Timing_Simulation/blob/main/51.jpg">

This diagram analogous to Amdahl's Law, which states that simply doubling the number of CPU cores does not necessarily lead to a 50% increase in performance. Instead, it depends on the value of "S," which represents the degree of interdependence among processes.
