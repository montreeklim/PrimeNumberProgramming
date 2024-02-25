# Prime Number Programming
This project is based on the paper **...** ... a preprint of which is available at **...**

We provide code for a branch and bound based algorithm to solve a prime program (PP). We then provide code to verify whether the objective function value of a perturbed PP is at least that of the original problem; i.e., a sensitivity analysis on the PP. We use a depth-first-search procedure for solving the PP. The branching variable is the one that is farthest from its closest prime number. For details, see Section 3 of the preprint.

### Formulation for a PP:
```math
\begin{align}
  z^* = \min_{x} \quad & cx \\
  \text{s.t.} \quad & Ax \geq b, \\
  & x_i \in [2,M] \cap \mathbb{P} \quad \forall i.
\end{align}
```

### Formulation for a perturbed problem:
```math
\begin{align}
  z = \min_{x} \quad & (c+ c_\delta)x \\
  \text{s.t.} \quad & (A+A_\delta)x \geq b+b_\delta, \\
  & x_i \in [2,M] \cap \mathbb{P} \quad \forall i.
\end{align}
```
Given $\Delta \geq 0$, the sensitivity analysis assesses whether $z \geq z^* - \Delta$ is inferrable.

### Example of a PP code implementation:
Consider the following problem:
```math
\begin{align}
  \min_{x_1,x_2,x_3} \quad &-x_1 - x_2 - 2x_3 \\
  \text{s.t.} \quad & 3x_1 + 5x_2 + 10x_3 && \geq 10000, \\
  & 3x_1 + 8x_2 + x_3 && \geq 10000, \\
  & 3x_1 + 2x_2 + x_3 && \geq 10000, \\
  & x_1, x_2, x_3 \in [2,997] \cap \mathbb{P}.
\end{align}
```

To solve this problem we first assign:
```
c = np.array([-1, -1, -2])
A = np.array([[3,5,10],[3,8,1], [3,2,1]])
b = np.array([10000,10000,10000])
lower_bounds=np.array([2,2,2])
upper_bounds = np.array([997, 997, 997])
```
We run the code via the command
```
node_count, global_ub, incumbent, computation_time = branch_and_bound_prime(c, A, b, lower_bounds, upper_bounds)
```

The following is a snippet result of the function `branch_and_bound_prime`:
![snippet_bb_prime_function](https://github.com/montreeklim/PrimeNumberProgramming/assets/65499015/9dc1a1b1-dbb0-48fd-92bf-9b2b867fbaca)

Let $\bar{p}(x)$ denote the smallest prime number greater than $x \in \mathbb{R}^+$ and $\underline{p}(x)$ denote the largest prime number lesser than $x \in \mathbb{R}^+$.
The optimal solution of the relaxation at the root node is $x = [997, 841.08, 280.36]$. $997$ is prime. Then, $\bar{p}(841.08) = 853$ and $\underline{p}(841.08) = 839$, and the distance to the closest prime is $2.08$; while, $\bar{p}(280.36) = 281$ and $\underline{p}(280.36) = 277$, and the distance to the closest prime is $0.64$. We choose $x_2$ to branch on because $2.08 > 0.64$. 
Continuing, the enumeration tree of this problem has $237$ nodes with the objective function value of $-2398.0$ and the optimal solution of $[x_1, x_2, x_3]=[997.0, 839.0, 281.0]$. The code displays the following message following completion is $0.0766$ seconds.


Consider the following problem:
```math
\begin{align}
  \min_{x_1,x_2} \quad -&x_1 \\
  \text{s.t.} \quad & x_1 + x_2 \geq 1000, \\
  -& x_1 - x_2 \geq -1000, \\
  & x_1, x_2 \in [2,100000] \cap \mathbb{P}.
\end{align}
```
and the perturbed problem:
```math
\begin{align}
  \min_{x_1,x_2} \quad -&x_1 \\
  \text{s.t.} \quad & x_1 + x_2 \geq 1000+4, \\
  -& x_1 - x_2 \geq -1000-4, \\
  & x_1, x_2 \in [2,100000] \cap \mathbb{P}.
\end{align}
```

To do the sensitivity analysis we first assign:
```
c = np.array([-1, 0])
A = np.array([[1, 1], [-1, -1]])
b = np.array([1000, -1000])
lower_bounds=np.array([2,2])
upper_bounds = np.array([100000,100000])

Delta = 0
delta_A = np.array([[0,0],[0,0]])
delta_c = np.array([0,0])
delta_b = np.array([4,-4])
```

We run the code via the command
```
SA_status = branch_and_bound_prime_SA(c=c, A=A, b=b, lower_bounds = lower_bounds, upper_bounds = upper_bounds,\
                                      Delta= Delta, delta_A=delta_A, delta_b=delta_b, delta_c=delta_c)
print(SA_status)
```

The following is a snippet result of the function `branch_and_bound_prime_SA`:
![SA_snippet](https://github.com/montreeklim/PrimeNumberProgramming/assets/65499015/468ce65d-e812-4a6f-8d21-c64f6c92e881)

To run the heuristic approach we first assign:
```
n = 12
M = 1000
TIME_LIMIT = 3000
solve_time_limit = 600
alpha = 0.2
branch_name = 'Modulo'
branch_arg = 11
fixing_name = 'ExcludeOne'
# Create fixing args
fix_arg_ExcludeOne = {}
for i in range(1,n+1):
    fix_arg_ExcludeOne[i] = [[i-2, i-1],[i-1]]
```

We run the code via the command
```
sol, ttime = main_algorithm(n=n, M=M, TIME_LIMIT=TIME_LIMIT, solve_time_limit=solve_time_limit, alpha=alpha, fixing_name=fixing_name, branching_name=branch_name, br_arg = branch_arg, fix_arg = fix_arg_ExcludeOne)
print(sol)
print(ttime)
```

## Repository content
The repository contains the following content:
- `PP_branch_and_bound.ipynb` a Jupyter notebook file to run the branch-and-bound algorithm for a PP. The function `branch_and_bound_prime`, included in this notebook,
  takes as input arrays c = $c$, A = $A$, b = $b$, lower_bounds = array of lower bounds of variables, upper_bounds = array of upper bounds of variables, used in the PP formulation and returns as output the number of nodes in the enumeration tree, the objective function value, an optimal solution, and computation time.
- `Sensitivity_Analysis_PP.ipynb` a Jupyter notebook file to run the sensitivity analysis for a PP and its perturbed problem. The function `branch_and_bound_prime_SA`, included in this notebook, takes as input arrays c = $c$, A = $A$, b = $b$, lower_bounds = 
   array of lower bounds of variables, upper_bounds = array of upper bounds of variables, Delta = $\Delta $, delta_A = $A_\delta $, delta_b = $b_\delta $, delta_c = $c_\delta$ used in the perturbed problem and returns True if the objective function value of the perturbed problem is at least $z^* - \Delta$, and False if this implication cannot be made through the sensitivity analysis.
- `Average_prime_heuristic.ipynb` a Jupyter notebook file to run the heuristic approach to find a sequence of primes such that the average of any two primes in the sequence is also a prime. The function `main_algorithm`, included in this notebook, takes as input n = number of primes in the sequence, M = initial upper bound for the corresponding PP, TIME_LIMIT = total time limit of the algorithm, solve_time_limit = time limit for each iteration, alpha = increasing ratio of the upper bound, fixing_name = {'Naive', 'SelectAll', 'SelectHalf', 'ExcludeOne', 'ExcludeTwo'} name of fixing strategy, branching_name = {'Naive', 'Modulo'} name of branching strategy, br_arg = remainder of prime modulo 12 if branching_name == 'Modulo', fix_arg = [p, l] where p is a list containing unassigned indices of solution in the current iteration and l is a list containing indices of previous solution to be excluded. The function returns solution and total time to obtain a sequence of $n$ primes or statement of infeasibility.

## Requirements to run code
The code uses the optimization solver Gurobi and sympy package required to run it.  
