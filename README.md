# Prime Number Programming
This project is based on the paper **...**.
We provide an algorithm to solve a prime program (PP).

## Repository content
The repository contains the following content:
- `PP_branch_and_bound.ipynb` a jupyter notebook file to run the branch-and-bound algorithm for a PP. The function returns number of nodes in the enumeration tree, the objective function value, and an optimal soultion.
- `Sensitivity_Analysis_PP.ipynb` a jupyter notebook file to run the sensitivity analysis for a PP and its perturbed problem. The function returns True if the objective function value of the perturbed problem is at least z* - delta_z, and False if it cannot be implied by the sensitivity analysis.
## Requirements to run code
The code uses the optimization solver Gurobi and sympy package which are therefore required to run it.
