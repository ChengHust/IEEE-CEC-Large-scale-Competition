Large-scale Multiobjective Optimization for Status Assessment of Measuring Equipment

**Please download the PlatEMO platform for competition at [PlatEMO4.4](https://github.com/ChengHust/IEEE-CEC-2024-Competition/tree/master) using git lfs.**
**If you are not familiar with the LFS, please download the file via [Google Drive](https://drive.google.com/file/d/1hfX8ejpZy4Dgx0daFZ3lItINfd_BpT1F/view?usp=drive_link)**

## Overview & Aim:

---

Evolutionary algorithms (EAs) have been a popular optimization tool for decades, showing promising performance in solving various benchmark optimization problems.
Nevertheless, using EAs on multiobjective optimization with over 100 decision variables (large-scale multiobjective optimization problems, LSMOPs) remains challenging due to the "curse of dimensionality".
This phenomenon is more significant for LSMOPs in complex man-made systems, e.g., railway systems, social networks, and power systems.
Specifically, EAs suffer from difficulties in dealing with enormous search space, irregularity in variable interactions and objective functions, and the existence of massive local optima for LSMOPs in emerging and critical applications.
Existing optimization algorithms may cost unbearable function evaluations (FEs) and computation time (time complexity) to obtain acceptably converged/diverse results.
Unfortunately, this phenomenon is more serious when the number of decision variables increases to large scale (>100), where the limitation in storage memory rises due to the increased space complexity.
Both time and memory efficiency and search effectiveness should be considered when dealing with large-scale multiobjective optimization problems (LSMOPs)  to fill the gap between complex real-world optimization and advanced optimization algorithms.

<img src="https://github.com/ChengHust/IEEE-CEC-2024-Competition/blob/main/CEC0_IEEE_30_nodes.png" />
Fig. 1 The IEEE 30-node standard topology for OSA-IT problems.

In this competition, we carefully format three LSMOPs from one interesting real-world application: online status assessment of instrument transformers in wide-area power systems (ETT problems).
The IEEE 30-node standard topology, refer to Figure 1, with a time-varying workload, is modelled and simulated to obtain measured data of the system, aiming to assess the status of instrument transformers.
Generally, the time-varying voltages, currents, and system-level parameters are formatted as decision variables, and the differences between the estimated results and physical rules of the system are modelled as the objectives.
Three ETT problems with 10,000, 50,000, and 100,000 decision variables are given in this competition.
Complicated variable interactions (e.g., partial separability and overlapping interactions) and variable properties (e.g., system parameters and measurement values) are involved, making the ETT problems challenging and representative.
As an extension of the TREE test suite, this competition is expected to promote research in smart grids and advanced optimization algorithms and explore potential research directions for super large-scale optimization, especially for the computational intelligence community.

Participants are encouraged to develop the algorithm to solve this optimization problem, not just a specific one.
Participants may propose a new optimization algorithm or utilize a hybrid of previously proposed algorithms.
Remarkably, it is not restricted to evolutionary computing, and commercial optimization software is allowed.
Participants must submit their source codes, a brief description of the optimization algorithm, and a brief code instruction.
Organizers will evaluate your proposed algorithm's performance in all three problems to guarantee its fairness.
With the same computational budget, the best solution for each problem obtained by running your algorithm one time will be compared directly.

## Platform & Parameter settings

Participants are encouraged to develop the algorithm to solve these ETT problems, not just a specific one.
Participants may propose a new optimization algorithm or utilize a hybrid of previously proposed algorithms.
Notably, it is unrestricted in the field of evolutionary computing.
Participants must submit their source codes, a brief description of the optimization algorithm, a brief code instruction, and the data generated by the platform.
Organizers will assess the quality of your submitted data in all three problems to guarantee its fairness.
With the same computational budget, the best solution for each problem obtained by randomly running your algorithm one time will be compared directly.

The PlatEMO v4.4 will be used as the competition platform for fair comparisons, with population size (N=50), number of independent runs (run=1), and number of results (1). The code of an example is given as

```
platemo('problem',@ETT1,'algorithm',@NSGAII,'N',50,'maxFE',1e7,'save',1)
```

* The test problems are

| Problem | Number of Objectives | Number of Decision Variables | Maximum Number of Function Evaluations |
| :-----: | :------------------: | :--------------------------: | :------------------------------------: |
|  ETT1  |         M=3         |          D = 10,000          |                FE = 1E7                |
|  ETT2  |         M=3         |         D = 50,000         |                FE = 1E7                |
|  ETT3  |         M=3         |         D = 100,000         |                FE = 1E7                |

<img src="https://github.com/ChengHust/IEEE-CEC-2024-Competition/blob/main/CEC2024Competition_Settings.png" />
* It is remarkable that an individual with a high-dimensional problem will be memory-costly in PlatEMO. Please revise the corresponding code to satisfy the memory requirements.

To avoid memory overflow in Matlab, it is suggested that matrix operations related to decision variables should be re-write in the "for-end" loop style, e.g.,

```
Offspring  = OperatorGA(Problem,Population);
```

should be re-written as

```
for i = 1 : N/2
  Offspring(i*2-1:i*2)  = OperatorGA(Problem,Population(i*2-1:i*2));
end
```

Also, a smaller population size, e.g., 10 or 20, could be set to satisfy the memory requirement.

## Important Dates:

For participants planning to submit a paper to the 2024 IEEE Congress on Evolutionary Computation:

### Paper submission: 13 Jan 2024

- Deadline: January 15, 2025
- Notification: March 15, 2025
- Congress: June 8 - 12, 2025, Hangzhou, China

**Participants for competition**

- Results submission deadline: 1st May 2025
- You can submit your related documents and results to Dr. He (chenghe_seee@hust.edu.cn)/ Dr. Wang (hdwang@xidian.edu.cn)/ Dr. Tian (field910921@gmail.com).

## Awards

### - Each winner will get an IEEE certificate!!!

## Competition Organizers:

* ***Cheng He***
  School of Electrical and Electronic Engineering, Huazhong University of Science and Technology, Wuhan 430074, China.
  chenghe_seee@hust.edu.cn
* ***Ye Tian***
  Institutes of Physical Science and Information Technology, Anhui University, Hefei 230601, China
  field910921@gmail.com
* ***Handing Wang***
  School of Artificial Intelligence, Xidian University, Xi'an 710071, China.
  hdwang@xidian.edu.cn
* ***Hongbin Li***
  School of Electrical and Electronic Engineering, Huazhong University of Science and Technology, Wuhan 430074, China.
  lihongbin@hust.edu.cn
