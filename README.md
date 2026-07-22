# Nutrition Optimization

A mathematical optimization project that develops affordable and nutritionally balanced meal plans for a school food assistance program.

Using Python and Gurobi, this project formulates and solves linear and mixed-integer optimization models to minimize food costs while satisfying daily nutritional requirements. The initial model is progressively refined to improve food-group balance, practical usability, and variety across a seven-day meal plan.

---

## Project Overview

This project is based on the fictional **Feeding Hope** initiative in the town of Starlight.

Following the closure of a local factory, many families in the community experienced financial hardship, and some children began facing inadequate nutrition. The local school sought a data-driven way to provide nutritionally complete meals while operating under a limited food budget.

The project applies mathematical optimization to answer the following question:

> How can a school provide nutritionally balanced and varied meals for children at the lowest possible cost?

The analysis begins with a minimum-cost daily diet model and then introduces additional constraints to improve realism and weekly variety.

---

## Project Objectives

The project was completed in four stages:

1. Formulate an algebraic optimization model for a minimum-cost daily meal plan.
2. Solve the model using Python and Gurobi.
3. Evaluate whether the initial result is realistic and refine the model to improve food-group balance.
4. Extend the model to generate a varied seven-day meal plan.

---

## Business Problem

School nutrition programs must balance several competing goals:

- Satisfy minimum nutritional requirements
- Avoid excessive nutrient intake
- Maintain a limited food budget
- Include a balanced mix of food groups
- Reduce repetition across consecutive days
- Create meal plans that can be implemented at scale

A solution based only on cost minimization may satisfy all mathematical constraints while still producing repetitive or impractical food combinations. Therefore, this project evaluates both mathematical optimality and real-world usability.

---

## Dataset

The food dataset contains 30 available food items.

For each item, the dataset includes:

- Food name
- Serving description
- Calories
- Fat
- Sodium
- Carbohydrates
- Fiber
- Protein
- Vitamin A
- Vitamin C
- Calcium
- Iron
- Cost per serving

The workbook also contains the minimum and maximum permitted daily intake for each nutrient.

---

## Daily Nutritional Requirements

Every daily meal plan must satisfy the following nutritional ranges:

| Nutrient | Minimum | Maximum |
|---|---:|---:|
| Calories | 1,800 kcal | 2,400 kcal |
| Fat | 60 g | 95 g |
| Sodium | 1,200 mg | 2,200 mg |
| Carbohydrates | 240 g | 400 g |
| Fiber | 30 g | 35 g |
| Protein | 40 g | 55 g |
| Vitamin A | 2,000 IU | 6,000 IU |
| Vitamin C | 45 mg | 1,200 mg |
| Calcium | 1,300 mg | 3,000 mg |
| Iron | 8 mg | 40 mg |

The original model also required that:

- No single food item contribute more than 30% of total daily calories.
- No single food item contribute more than 30% of total daily protein.
- All serving quantities remain non-negative.

---

## Tools and Technologies

- Python
- Gurobi Optimizer
- gurobipy
- Pandas
- Jupyter Notebook
- Linear Programming
- Mixed-Integer Programming
- Mathematical Optimization
- Operations Research

---

## Mathematical Formulation

### Sets

Let:

- \(I\) represent the set of available food items.
- \(N\) represent the set of nutrients.

### Decision Variable

\[
x_i = \text{number of servings of food item } i
\]

### Parameters

\[
c_i = \text{cost per serving of food item } i
\]

\[
a_{ni} = \text{amount of nutrient } n \text{ in one serving of food } i
\]

\[
L_n = \text{minimum required daily intake of nutrient } n
\]

\[
U_n = \text{maximum permitted daily intake of nutrient } n
\]

### Objective Function

The objective is to minimize the total daily cost:

\[
\min \sum_{i \in I} c_i x_i
\]

### Nutritional Constraints

For every nutrient \(n\):

\[
L_n \leq \sum_{i \in I} a_{ni}x_i \leq U_n
\]

### Calorie-Contribution Constraint

For every food item \(i\):

\[
Calories_i x_i
\leq
0.30\sum_{j \in I} Calories_j x_j
\]

### Protein-Contribution Constraint

For every food item \(i\):

\[
Protein_i x_i
\leq
0.30\sum_{j \in I} Protein_j x_j
\]

### Non-Negativity

\[
x_i \geq 0 \qquad \forall i \in I
\]

---

## Part 1: Initial Daily Optimization

The initial linear programming model minimized daily cost while satisfying:

- All ten nutritional ranges
- The 30% calorie-contribution restriction
- The 30% protein-contribution restriction
- Non-negative serving quantities

### Initial Optimized Meal Plan

| Food | Servings |
|---|---:|
| Potatoes, Baked | 3.15 |
| Spaghetti with Sauce | 0.61 |
| Banana | 0.09 |
| Oranges | 4.54 |
| 2% Low-Fat Milk | 2.04 |
| Skim Milk | 1.00 |
| White Rice | 0.16 |
| Pork | 0.57 |

### Initial Daily Cost

```text
$2.43 per child
```

The result satisfied the mathematical requirements at a very low cost. However, it relied heavily on selected grains, fruit, and dairy products and did not include a sufficiently balanced range of food groups.

For example, the plan contained more than four servings of oranges but no clear serving of common vegetables such as broccoli or carrots.

---

## Part 2: Improving the Daily Meal Plan

To make the recommendation more practical, the food items were classified into five categories:

- Vegetables
- Protein
- Fruit
- Grain/Pasta
- Dairy

The refined model required at least one serving from each category:

\[
\sum_{i \in I_k} x_i \geq 1
\qquad \forall k \in K
\]

where \(I_k\) is the set of food items belonging to category \(k\).

### Model Changes

The updated model introduced two important changes:

1. At least one serving from every food category was required.
2. The original 30% calorie and protein contribution constraints were removed to provide greater flexibility when selecting a balanced combination of food groups.

Removing the 30% rule in this stage was a deliberate model refinement. The initial rule promoted balance at the individual-food level, but the updated category requirements provided a more direct way to enforce variety across vegetables, protein, fruit, grains, and dairy.

### Refined Daily Meal Plan

| Food | Servings |
|---|---:|
| Lettuce, Iceberg, Raw | 1.00 |
| Potatoes, Baked | 3.19 |
| Spaghetti with Sauce | 0.42 |
| Oranges | 5.43 |
| 2% Low-Fat Milk | 2.94 |
| Skim Milk | 0.78 |
| Beef | 0.55 |
| White Rice | 0.12 |
| Pork | 0.45 |

### Refined Daily Cost

```text
$2.55 per child
```

The cost increased by only $0.12 compared with the initial solution, while the meal plan represented a broader range of food categories.

This demonstrates an important optimization trade-off:

> The mathematically cheapest solution is not always the most practical solution.

A small increase in cost can produce a meal plan that is more balanced and easier to justify from a human-centered perspective.

---

## Part 3: Seven-Day Meal Plan

The model was extended from a one-day linear program to a seven-day mixed-integer optimization model.

For each day \(d\) and food item \(i\):

\[
s_{di} =
\text{number of servings of food } i \text{ on day } d
\]

A binary variable was introduced:

\[
y_{di} =
\begin{cases}
1, & \text{if food } i \text{ is used on day } d \\
0, & \text{otherwise}
\end{cases}
\]

### Weekly Objective

\[
\min
\sum_{d=1}^{7}
\sum_{i \in I}
c_i s_{di}
\]

### Weekly Model Requirements

The seven-day model:

- Satisfies all minimum and maximum nutrient requirements each day.
- Includes food from all five categories each day.
- Uses binary variables to connect food selection with serving quantities.
- Requires daily food selections to differ across consecutive days.
- Limits each non-dairy food item to no more than four appearances per week.
- Allows additional flexibility for dairy because the dataset contains only two dairy products.

The four-use weekly limit was selected so that one food could not dominate more than half of the week while still leaving the model enough flexibility to find a feasible solution.

---

## Optimized Seven-Day Meal Plan

### Day 1

| Food | Servings |
|---|---:|
| Lettuce, Iceberg, Raw | 1.00 |
| Potatoes, Baked | 1.28 |
| Apple, Raw, with Skin | 8.16 |
| Grapes | 1.93 |
| 2% Low-Fat Milk | 3.89 |
| Beef | 2.90 |

**Daily cost: $4.35**

### Day 2

| Food | Servings |
|---|---:|
| Lettuce, Iceberg, Raw | 1.00 |
| Potatoes, Baked | 1.00 |
| Apple, Raw, with Skin | 7.44 |
| Banana | 1.14 |
| Oranges | 0.29 |
| 2% Low-Fat Milk | 3.88 |
| Beef | 2.91 |

**Daily cost: $3.76**

### Day 3

| Food | Servings |
|---|---:|
| Lettuce, Iceberg, Raw | 1.00 |
| Potatoes, Baked | 3.16 |
| Spaghetti with Sauce | 0.52 |
| Oranges | 5.18 |
| 2% Low-Fat Milk | 2.93 |
| Turkey | 0.50 |
| White Rice | 0.11 |
| Pork | 0.50 |

**Daily cost: $2.56**

### Day 4

| Food | Servings |
|---|---:|
| Carrots, Raw | 0.10 |
| Lettuce, Iceberg, Raw | 0.90 |
| Potatoes, Baked | 0.90 |
| Apple, Raw, with Skin | 7.34 |
| Banana | 1.33 |
| Oranges | 0.27 |
| White Bread | 0.10 |
| 2% Low-Fat Milk | 3.88 |
| Beef | 2.90 |

**Daily cost: $3.76**

### Day 5

| Food | Servings |
|---|---:|
| Carrots, Raw | 0.17 |
| Corn | 0.13 |
| Tomato, Red, Ripe, Raw | 0.70 |
| Apple, Raw, with Skin | 2.34 |
| Oranges | 5.05 |
| White Bread | 6.33 |
| 2% Low-Fat Milk | 2.66 |
| Turkey | 0.29 |
| White Rice | 0.10 |
| Pork | 0.71 |

**Daily cost: $3.16**

### Day 6

| Food | Servings |
|---|---:|
| Carrots, Raw | 0.13 |
| Corn | 0.87 |
| Spaghetti with Sauce | 0.45 |
| Banana | 5.59 |
| Wheat Bread | 0.10 |
| White Bread | 0.10 |
| 2% Low-Fat Milk | 3.85 |
| Skim Milk | 0.10 |
| Turkey | 0.41 |
| White Rice | 0.56 |
| Pork | 0.59 |

**Daily cost: $2.85**

### Day 7

| Food | Servings |
|---|---:|
| Carrots, Raw | 0.14 |
| Corn | 0.86 |
| Spaghetti with Sauce | 0.38 |
| Banana | 5.79 |
| Wheat Bread | 0.10 |
| White Bread | 0.10 |
| 2% Low-Fat Milk | 3.89 |
| Skim Milk | 0.10 |
| Turkey | 0.10 |
| Beef | 0.36 |
| White Rice | 0.58 |
| Pork | 0.54 |

**Daily cost: $2.84**

---

## Weekly Cost Summary

| Day | Cost |
|---|---:|
| Day 1 | $4.35 |
| Day 2 | $3.76 |
| Day 3 | $2.56 |
| Day 4 | $3.76 |
| Day 5 | $3.16 |
| Day 6 | $2.85 |
| Day 7 | $2.84 |
| **Total** | **$23.28** |

The average cost is approximately:

```text
$3.33 per child per day
```

---

## Key Findings

- The initial minimum-cost meal plan cost only **$2.43 per child per day**.
- Cost minimization alone produced a solution with limited food-group diversity.
- Requiring all five food categories increased the daily cost to **$2.55**, an increase of only **$0.12**.
- The seven-day model generated a more varied plan for **$23.28 per child per week**.
- Daily costs in the weekly plan ranged from **$2.56 to $4.35**.
- Adding realism and variety increased cost, but the increase was relatively small compared with the improvement in meal balance.
- Optimization is most useful when practical constraints are included alongside financial objectives.

---

## Practical Interpretation

The optimized serving quantities include fractional values, which may appear difficult to implement for one child.

However, this model is intended for a school feeding program. When serving quantities are multiplied across hundreds or thousands of students, fractional per-child quantities become manageable bulk purchasing and preparation amounts.

For example, a recommendation of 0.10 servings per child becomes 50 full servings when meals are prepared for 500 children.

---

## Limitations

### Uniform Food Preferences

The model assumes that all children have the same food preferences.

It does not account for:

- Allergies
- Food intolerances
- Religious dietary restrictions
- Cultural food preferences
- Vegetarian or vegan requirements

### Fractional Servings

The optimization uses continuous decision variables, so some recommended serving quantities are fractional.

Although this is manageable when cooking at scale, additional integer or portion-size constraints may be necessary for smaller programs.

### Food Availability

The model assumes that every food item is always available.

It does not include:

- Seasonal availability
- Supplier capacity
- Minimum purchase quantities
- Food waste
- Storage limits
- Preparation capacity

### Food Acceptability

The model measures nutrition and cost but does not directly measure whether children would find the recommended combinations appealing.

Several optimized quantities, such as multiple servings of one fruit or bread item, may require additional practical limits.

### Static Prices

Food costs are treated as fixed and do not change across the week.

---

## Recommendations

### Incorporate Student Preferences and Allergies

Future versions should include information about student preferences, dietary restrictions, and allergies.

Food-specific exclusion constraints could prevent unsuitable items from appearing in a student's plan.

### Add Practical Portion Limits

Minimum and maximum serving constraints could be introduced for individual foods:

\[
l_i y_{di}
\leq
s_{di}
\leq
u_i y_{di}
\]

This would prevent very small or unusually large serving quantities.

### Scale the Model to the School Level

The per-child recommendations can be multiplied by total student enrollment to generate:

- Total purchasing quantities
- Weekly procurement costs
- Ingredient preparation requirements
- Supplier orders

### Include Seasonal Availability and Prices

Time-dependent availability and cost parameters would make the weekly model more realistic.

### Include Food-Waste Considerations

Future models could minimize a weighted combination of:

- Food cost
- Food waste
- Nutritional deviation
- Meal repetition

---

## Repository Structure

```text
school-meal-plan-optimization/
├── README.md
├── Nutrition_Optimization.ipynb
└── Food_Nutrition_Data.xlsx
```

---

## Repository Contents

| File | Description |
|---|---|
| `Nutrition_Optimization.ipynb` | Complete Python and Gurobi workflow, including model formulation, daily optimization, model refinement, and seven-day meal planning |
| `Food_Nutrition_Data.xlsx` | Nutritional requirements, food attributes, serving descriptions, and cost-per-serving data |
| `README.md` | Project overview, mathematical formulation, results, limitations, and recommendations |

---

## How to Run the Project

### Requirements

- Python 3
- Jupyter Notebook or JupyterLab
- Pandas
- Gurobi Optimizer
- A valid Gurobi license
- openpyxl

### Install Python Packages

```bash
pip install pandas openpyxl gurobipy jupyter
```

### Run the Notebook

1. Clone this repository:

```bash
git clone https://github.com/your-username/school-meal-plan-optimization.git
```

2. Open the project directory:

```bash
cd school-meal-plan-optimization
```

3. Start Jupyter Notebook:

```bash
jupyter notebook
```

4. Open:

```text
Nutrition_Optimization.ipynb
```

5. Ensure `Food_Nutrition_Data.xlsx` is located in the same directory as the notebook.

6. Update the workbook filename inside the notebook if necessary:

```python
file_path = "Food_Nutrition_Data.xlsx"
```

7. Run all cells in order.

---

## Skills Demonstrated

- Linear Programming
- Mixed-Integer Programming
- Mathematical Optimization
- Operations Research
- Gurobi
- Python
- Pandas
- Constraint Modeling
- Decision Modeling
- Cost Optimization
- Nutrition Analytics
- Scenario Refinement
- Human-Centered Analytics
- Analytics for Social Impact

---

## Team

- **Sicheng (Sisly) Lyu**
- Haibin (Allen) Yu
- Wendy (Mengyuan) Liu
- Wenqiang (Roy) Yu

---

## AI Usage

Generative AI was used only to assist with selected Gurobi coding questions and grammatical refinement.

The mathematical formulations, objective functions, constraints, model design, analysis, and recommendations were developed by the project team.
