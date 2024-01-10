# Prime Number Programming
This project is based on the paper **...** ... a preprint of which is available at **...**

We provide code for a branch and bound based algorithm to solve a prime program (PP). We then provide code to verify whether the objective function value of a perturbed PP is at least that of the original problem; i.e., a sensitivity analysis on the PP. We use a depth-first-search procuedure for solving the PP. The branching variable is the one that is farthest from its closest prime number. For details, see Section 3 of the preprint.

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
node_count, global_ub, incumbent = branch_and_bound_prime(c, A, b, lower_bounds, upper_bounds)
```
**** Can you show snippets of the output so that someone running the code can verify he is doing it correctly? what would be relevant to include for the output? perhaps the root node output as you say below and the end result of the code***

The optimal solution of the relaxation at the root node is $x = [997, 841.08, 280.36]$. 997 is prime. Then, $\bar{p}(841.08) = 853$ and $\underline{p}(841.08) = 839$, and the distance to the closest prime is 2.08; while, $\bar{p}(280.36) = 281$ and $\underline{p}(280.36) = 277$, and the distance to the closest prime is $0.64$. We choose $x_2$ to branch on because $2.08 > 0.64$. *** I've not gone through your new edits on the paper, but is $\underline{p}(841.08) the notation you use now?*** 
The display in the Git page $\bar{p}_{280.36}$ or $\underline{p}_{280.36}$ look weird. Continuing, the enumeration tree of this problem has $237$ nodes with the objective function value of $-2398.0$ and the optimal solution of $[x_1, x_2, x_3]=[997.0, 839.0, 281.0]$. The code displays the following message following completion is xxxx seconds:



## Repository content
The repository contains the following content:
- `PP_branch_and_bound.ipynb` a Jupyter notebook file to run the branch-and-bound algorithm for a PP. The function (**which function?***) returns the number of nodes in the enumeration tree, the objective function value, and an optimal soultion.
- `Sensitivity_Analysis_PP.ipynb` a Jupyter notebook file to run the sensitivity analysis for a PP and its perturbed problem. The function (**which function?**) returns True if the objective function value of the perturbed problem is at least $z^* - \Delta$, and False if this implication cannot be made through the sensitivity analysis.

***Above are these files containing one function? Maybe sensible to write the input and output?***

## Requirements to run code
The code uses the optimization solver Gurobi and sympy package required to run it.  
