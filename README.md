# Exercise 9 - Scheduling

## Properties

### Task 1:
 1. Why do we assign priorities to tasks? 
	Some tasks are more important than others.
 2. What features must a scheduler have for it to be usable for real-time systems?
	The scheduler needs to be predictable so we can analyze the system. 

 

## Inversion and inheritance


| Task | Priority   | Execution sequence | Release time |
|------|------------|--------------------|--------------|
| a    | 3          | `E Q V E`          | 4            |
| b    | 2          | `E V V E E E`      | 2            |
| c    | 1 (lowest) | `E Q Q Q E`        | 0            |

 - `E` : Executing
 - `Q` : Executing with resource Q locked
 - `V` : Executing with resource V locked


### Task 2: Draw Gantt charts to show how the former task set:
 1. Without priority inheritance

Task	0 1 2 3 4 5 6 7 8 9 10 11 12 13 14
------------------------------------------------------------
a    |          E               Q  V  E 
b    |      E V   V E E E 
c    |  E Q               Q Q            E


 2. With priority inheritance
Task	0 1 2 3 4 5 6 7 8 9 10 11 12 13 14
------------------------------------------------------------
a    |          E     Q   V E
b    |      E V         V      E  E  E 
c    |  E Q       Q Q                   E


### Task 3: Explain:
 1. What is priority inversion? What is unbounded priority inversion?
	Priority inversion is when a task of lower priority is executed instead of a task with higher priority and is then (unintentionally) stealing all the resources. 
		Priority gets inversed (high is low and visa versa)
	Unbounded priority inversion is when lower priority tasks execute their own tasks and share the rescourses with eachother, so there is not certain that the higher priority tasks will get executed. 
		The lower priorty tasks blocks the higher.
 3. Does priority inheritance avoid deadlocks?
	No 


## Utilization and response time

### Task set 2:

| Task | Period (T) | Exec. Time (C) |
|------|------------|----------------|
| a    | 50         | 15             |
| b    | 30         | 10             |
| c    | 20         | 5              |

### Task 4:
 1. There are a number of assumptions/conditions that must be true for the utilization and response time tests to be usable (The "simple task model"). What are these assumptions? Comment on how realistic they are.
	* Fixed set of tasks (Not optimal with sporadix tasks)
	* Periodic tasks (Realistic)
	* Independant tasks (Realistic)
	* No overhead (Depends - switching times can be ignored)
 2. Perform the utilization test for the task set. Is the task set schedulable?
	U = 15/50 + 10/30 + 5/20 = 0.8833
	(2^(1/3)-1) = 0.7798
		0.8833 =/< 0.7798 - test fails so not sure if the task is schedulable
 3. Perform response-time analysis for the task set. Is the task set schedulable? If you got different results than in 2), explain why.
	Task c) 	w_0 = 5													R_c = 5 <= 20 
	Task b) 	w_0 = 10 	w_1 = 10 + (10/20 *5) = 15			w_2 = 10 + (15/20 *5) = 15		R_b = 15 <= 30
	Task a)		w_0 = 15	w_1 = 15 + (15/30 *10) + (15/20 *5) = 30 	w_2 = 35 w_3 = 45 w_4 = 50 w_5 = 50	R_a = 50 <= 50
		The tasks sets are schedulable. The difference between this and task 2 is that utilization test is only sufficient, not necessary. Respons time analysis is both necessary and sufficient  
	
 4. (Optional) Draw a Gantt chart to show how the task set executes using rate monotonic priority assignment, and verify that your conclusions are correct.

## Formulas

Utilization:  
![U = \sum_{i=1}^{n} \frac{C_i}{T_i} \leq n(2^{\frac{1}{n}}-1)](eqn-utilization.png)

Response-time:  
![w_{i}^{n+1} = C_i + \sum_{j \in hp(i)} \bigg \lceil {\frac{w_i^n}{T_j}} \bigg \rceil C_j](eqn-responsetime.png)


