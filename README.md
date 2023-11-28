# Prime Number Programming
This project is based on the paper **...**.
We provide an algorithm to solve a prime program (PP) and a code to check if the sensitivity analysis works with the perturbed problem or not. There, using a proposed branch-and-bound algorithm with depth-first-search to solve a given PP. The branching variable is a non-prime relaxation solution which is the furthest from its closest prime number.

### Example of a PP
Consider the following problem:

```math
\begin{align}
  \min \quad -&x_1 - x_2 - 2x_3 \\
  \text{s.t.} \quad & 3x_1 + 5x_2 + 10x_3 && \leq 10000, \\
  & 3x_1 + 8x_2 + x_3 && \leq 10000, \\
  & 3x_1 + 2x_2 + x_3 && \leq 10000, \\
  & x_1, x_2, x_3 \in [2,997] \cap \mathbb{P}.
\end{align}
```

To solve this problem we assign:
c = np.array([-1, -1, -2]),
A = np.array([[3,5,10],[3,8,1], [3,2,1]]),
b = np.array([10000,10000,10000]),
lower_bounds=np.array([2,2,2]),
upper_bounds = np.array([997, 997, 997]).
The function is called as:
node_count, global_ub, incumbent = branch_and_bound_prime(c, A, b, lower_bounds, upper_bounds).

Then, the relaxation solution at the root node is x = [997, 841.08, 280.36].
Since $\bar{p}_{841.08} = 853$ and $\underline{p}_{841.08}$ = 839, the distance between its closest prime is 2.08 and $\bar{p}_{280.36} = 281$ and $\underline{p}_{280.36} = 277$, the distance between its closest prime is 0.64. We choose $x_2$ to branch on because 2.08 > 0.64. The enumeration tree of this problem has 237 nodes with the objective function value of -2398.0 and the optimal solution of $[x_1, x_2, x_3]=[997.0, 839.0, 281.0]$.

## Repository content
The repository contains the following content:
- `PP_branch_and_bound.ipynb` a jupyter notebook file to run the branch-and-bound algorithm for a PP. The function returns number of nodes in the enumeration tree, the objective function value, and an optimal soultion.
- `Sensitivity_Analysis_PP.ipynb` a jupyter notebook file to run the sensitivity analysis for a PP and its perturbed problem. The function returns True if the objective function value of the perturbed problem is at least z* - delta_z, and False if it cannot be implied by the sensitivity analysis.
## Requirements to run code
The code uses the optimization solver Gurobi and sympy package which are therefore required to run it.
