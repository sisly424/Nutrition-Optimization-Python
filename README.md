# Nutrition Optimization

A mathematical optimization project that develops affordable and nutritionally balanced meal plans for a school food assistance program.

Using Python and Gurobi, this project formulates and solves linear and mixed-integer optimization models to minimize food costs while satisfying daily nutritional requirements. The initial model is progressively refined to improve food-group balance, practical usability, and variety across a seven-day meal plan.

---

## Project Overview

This project is based on the fictional **Feeding Hope** initiative in the town of Starlight.

Following the closure of a local factory, many families in the community experienced financial hardship, and some children began facing inadequate nutrition. The local school sought a data-driven way to provide nutritionally complete meals while operating under a limited budget.

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

The food dataset contains nutritional and cost information for a range of common food items.

For each food item, the dataset includes:

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

The Excel workbook is stored as:

```text
food_data.xlsx
```

It contains the nutrient requirements and the nutritional characteristics and costs of the available food items.

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

## Project Workflow

1. Load nutrient requirements and food data from Excel.
2. Define food-serving decision variables.
3. Minimize total daily food cost.
4. Add minimum and maximum nutrient constraints.
5. Add calorie and protein contribution constraints.
6. Solve the initial daily optimization model.
7. Evaluate the practicality of the initial result.
8. Categorize foods and add food-group requirements.
9. Solve the refined daily model.
10. Extend the model to a seven-day planning horizon.
11. Add binary food-selection and weekly variety constraints.
12. Compare the costs and practicality of the resulting plans.

---

## Mathematical Formulation

### Sets

- `I`: set of available food items
- `N`: set of nutrients

### Decision Variable

```text
x[i] = number of servings of food item i consumed per day
```

### Parameters

```text
c[i]   = cost per serving of food item i

a[n,i] = amount of nutrient n in one serving of food item i

L[n]   = minimum required daily intake of nutrient n

U[n]   = maximum permitted daily intake of nutrient n
```

### Objective Function

The objective is to minimize the total daily food cost:

```text
Minimize:

Σ c[i] × x[i]
i ∈ I
```

### Nutritional Constraints

For every nutrient `n`:

```text
L[n] ≤ Σ a[n,i] × x[i] ≤ U[n]
       i ∈ I
```

This ensures that the total daily intake of every nutrient remains between its required minimum and maximum levels.

### Calorie-Contribution Constraint

For every food item `i`:

```text
Calories[i] × x[i]
≤ 0.30 × total daily calories
```

Equivalent form:

```text
Calories[i] × x[i]
≤ 0.30 × Σ Calories[j] × x[j]
         j ∈ I
```

This prevents one food item from supplying more than 30% of total daily calories.

### Protein-Contribution Constraint

For every food item `i`:

```text
Protein[i] × x[i]
≤ 0.30 × total daily protein
```

Equivalent form:

```text
Protein[i] × x[i]
≤ 0.30 × Σ Protein[j] × x[j]
         j ∈ I
```

This prevents one food item from supplying more than 30% of total daily protein.

### Non-Negativity Constraint

```text
x[i] ≥ 0 for every food item i
```

---

## Part 1: Initial Daily Optimization

The initial linear programming model minimized daily food cost while satisfying:

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

The result satisfied the mathematical requirements at a very low cost. However, it relied heavily on selected grains, fruits, and dairy products and did not include a sufficiently balanced range of food groups.

For example, the plan contained more than four servings of oranges but did not include a clear serving of common vegetables such as broccoli or carrots.

This showed that the mathematically cheapest solution was not necessarily the most practical or balanced solution.

---

## Part 2: Improving the Daily Meal Plan

To make the recommendation more realistic, the food items were classified into five categories:

- Vegetables
- Protein
- Fruit
- Grain/Pasta
- Dairy

The refined model required at least one serving from each food category.

### Food-Category Constraint

For every required food category `k`:

```text
Σ x[i] ≥ 1
i ∈ category k
```

### Model Changes

The updated model introduced two important changes:

1. At least one serving from every food category was required.
2. The original 30% calorie and protein contribution constraints were removed.

Removing the 30% constraints gave the model more flexibility to select a lower-cost combination while the category requirements directly promoted balance across vegetables, protein, fruit, grains, and dairy.

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

The refined plan cost only `$0.12` more than the initial minimum-cost solution while representing a broader mix of food groups.

This demonstrates an important optimization trade-off:

> The mathematically cheapest solution is not always the most practical solution.

A small increase in cost can produce a meal plan that is more balanced and easier to justify from a human-centered perspective.

---

## Part 3: Seven-Day Optimization Model

The model was extended from a one-day linear program to a seven-day mixed-integer optimization model.

### Additional Sets

- `D`: seven days in the planning period
- `K`: required food categories

### Weekly Decision Variables

```text
s[d,i] = number of servings of food item i on day d
```

A binary selection variable was also introduced:

```text
y[d,i] = 1 if food item i is selected on day d
         0 otherwise
```

### Weekly Objective Function

The objective is to minimize total food cost across all seven days:

```text
Minimize:

Σ Σ c[i] × s[d,i]
d i
```

### Daily Nutritional Constraints

For every day `d` and nutrient `n`:

```text
L[n] ≤ Σ a[n,i] × s[d,i] ≤ U[n]
       i
```

Each individual day must satisfy all minimum and maximum nutritional requirements.

### Food Selection Link

The binary and continuous variables are connected using:

```text
s[d,i] ≤ M × y[d,i]
```

where `M` is a sufficiently large upper bound.

When `y[d,i] = 0`, the food item cannot have a positive serving quantity on that day.

### Daily Food-Category Constraints

Each daily plan must include food from every required category:

```text
Σ s[d,i] ≥ 1
i ∈ category k
```

This requirement applies to:

- Vegetables
- Protein
- Fruit
- Grain/Pasta
- Dairy

### Consecutive-Day Variety Constraint

Binary variables were used to ensure that at least one selected food item differed between consecutive days.

This prevented two adjacent days from containing exactly the same set of foods.

### Weekly Frequency Constraint

Each non-dairy food item was limited to appearing on no more than four days during the week:

```text
Σ y[d,i] ≤ 4
d ∈ D
```

Dairy products were excluded from this restriction because the dataset contains limited dairy choices.

The four-day limit was chosen to prevent one food from dominating most of the week while preserving sufficient flexibility for the model to remain feasible.

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

Average daily cost:

```text
$23.28 ÷ 7 ≈ $3.33 per child per day
```

---

## Key Findings

- The initial minimum-cost meal plan cost **$2.43 per child per day**.
- Cost minimization alone produced a solution with limited food-group diversity.
- Requiring all five food categories increased the daily cost to **$2.55**.
- Improving food-group balance increased cost by only **$0.12 per child per day**.
- The seven-day model generated a more varied weekly plan for **$23.28 per child**.
- Daily costs in the weekly plan ranged from **$2.56 to $4.35**.
- The average cost of the weekly plan was approximately **$3.33 per child per day**.
- Binary variables enabled the model to control food selection and weekly repetition.
- Practical constraints improved the usefulness of the model but increased total cost.
- Optimization is most effective when human-centered requirements are considered alongside cost.

---

## Practical Interpretation

The optimized results include fractional serving quantities, which may appear difficult to implement for one child.

However, the model is intended for a school feeding program where meals are prepared for hundreds or thousands of students. Fractional per-child quantities can therefore be converted into manageable bulk purchasing and preparation amounts.

For example:

```text
0.10 servings per child × 500 children
= 50 full servings
```

This makes fractional solutions more practical at the school-program level.

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

The optimization uses continuous serving variables, so some recommended quantities are very small or fractional.

Although this is manageable when preparing food at scale, additional serving-size constraints may be required for smaller programs.

### Large Quantities of Individual Foods

Some optimized solutions include high quantities of particular foods, such as apples, bananas, oranges, bread, or milk.

The plans meet the mathematical requirements but may not always reflect realistic eating behavior.

### Food Availability

The model assumes every food item is available throughout the planning period.

It does not account for:

- Seasonal availability
- Supplier capacity
- Minimum purchase quantities
- Ingredient shortages
- Storage limitations
- Kitchen preparation capacity

### Static Food Costs

Food prices are treated as fixed and do not change across the week.

### Food Waste

The model does not include food spoilage, leftovers, package sizes, or waste-related costs.

### Meal Structure

The model optimizes total daily intake but does not divide food into breakfast, lunch, dinner, and snacks.

### Food Acceptability

The model measures nutrient intake and cost but does not directly evaluate whether children would find the recommended combinations appealing.

---

## Recommendations

### Incorporate Preferences and Allergies

Future versions should include student-level dietary information, including:

- Food preferences
- Allergies
- Intolerances
- Cultural requirements
- Religious requirements

Food-specific exclusion constraints could prevent unsuitable items from appearing in a student's plan.

### Add Practical Serving Limits

Minimum and maximum serving quantities could be added for individual foods:

```text
minimum_serving[i] × y[d,i]
≤ s[d,i]
≤ maximum_serving[i] × y[d,i]
```

These constraints would prevent extremely small or unusually large serving quantities.

### Add Meal-Level Planning

The model could assign foods to:

- Breakfast
- Lunch
- Dinner
- Snacks

This would help ensure that the daily combination forms recognizable meals.

### Scale the Model to the School Level

Per-child serving quantities can be multiplied by total enrollment to calculate:

- Total purchasing quantities
- Weekly procurement costs
- Kitchen preparation requirements
- Supplier orders
- Staffing needs

### Include Seasonal Availability and Prices

Time-dependent availability and cost parameters would improve the realism of the weekly model.

### Include Food-Waste Considerations

A future objective could minimize a weighted combination of:

```text
Food cost
+ food waste
+ nutritional deviation
+ meal repetition
```

### Add Multiple Objectives

Instead of minimizing cost alone, the model could balance:

- Cost
- Variety
- Student satisfaction
- Waste
- Preparation complexity
- Environmental impact

---

## Repository Structure

```text
school-meal-plan-optimization/
├── README.md
├── Nutrition_Optimization.ipynb
└── food_data.xlsx
```

---

## Repository Contents

| File | Description |
|---|---|
| `Nutrition_Optimization.ipynb` | Complete Python and Gurobi workflow, including model formulation, daily optimization, model refinement, and seven-day meal planning |
| `food_data.xlsx` | Nutritional requirements, food attributes, serving descriptions, and cost-per-serving data |
| `README.md` | Project overview, formulation, optimization results, limitations, and recommendations |

The original project-option document and screenshots are not included because they contain assignment instructions rather than the team's analytical work.

---

## How to Run the Project

### Requirements

- Python 3
- Jupyter Notebook or JupyterLab
- Pandas
- Gurobi Optimizer
- A valid Gurobi license
- openpyxl

### Install Required Python Packages

```bash
pip install pandas openpyxl gurobipy jupyter
```

### Clone the Repository

```bash
git clone https://github.com/your-username/school-meal-plan-optimization.git
```

### Open the Project Folder

```bash
cd school-meal-plan-optimization
```

### Start Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
Nutrition_Optimization.ipynb
```

Ensure that the Excel workbook is located in the same folder as the notebook:

```text
food_data.xlsx
```

The Notebook reads the dataset using:

```python
file_path = "food_data.xlsx"
```

Run all Notebook cells in order.

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
- Weekly Planning
- Binary Decision Variables
- Model Refinement
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

Generative AI was used only to assist with selected Gurobi coding questions and grammatical corrections.

The mathematical formulations, constraints, objective functions, model design, analysis, and recommendations were independently developed by the project team.
