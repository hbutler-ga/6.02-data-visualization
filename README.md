# Module 12: Data Visualization with Matplotlib and Seaborn

## Overview

In this module, students learn how to create clear, readable visualizations using **Matplotlib** and **Seaborn**.

The goal is not to memorize every plotting option. The goal is to learn how to choose an appropriate chart type, write the basic plotting syntax, and improve a chart so another person can understand it.

We will use Seaborn's built-in `mpg` dataset throughout the lesson.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

mpg = sns.load_dataset("mpg")
mpg.head()
```

---

## Learning Objectives

By the end of this module, students will be able to:

- Choose an appropriate visualization based on the type of question being asked.
- Create basic plots using Matplotlib and Seaborn.
- Use histograms, count plots, scatterplots, boxplots, line plots, subplots, and heatmaps.
- Add titles, axis labels, figure sizes, and other readability improvements.
- Use `hue` to add a grouping variable to a Seaborn plot.
- Export visualizations as `.png` or `.jpg` files.

---

## Dataset

This lesson uses the `mpg` dataset from Seaborn.

```python
mpg = sns.load_dataset("mpg")
mpg.head()
```

Useful columns include:

| Column | Type | Description |
|---|---|---|
| `mpg` | Numeric | Miles per gallon / fuel efficiency |
| `cylinders` | Numeric / categorical | Number of engine cylinders |
| `displacement` | Numeric | Engine displacement |
| `horsepower` | Numeric | Engine horsepower |
| `weight` | Numeric | Car weight |
| `acceleration` | Numeric | Acceleration value |
| `model_year` | Ordered numeric | Model year |
| `origin` | Categorical | Country/region of origin |
| `name` | Categorical | Car name |

Before plotting, inspect the data:

```python
mpg.shape
```

```python
mpg.info()
```

```python
mpg.describe()
```

---

# Section 1: Visualization Answers Questions

## Core Idea

Data visualization is not just about making charts. A visualization should help answer a question.

A useful workflow is:

1. Identify the question.
2. Identify the variables needed.
3. Identify the variable types.
4. Choose the chart type.
5. Write the plotting code.
6. Improve readability.

## Chart Selection Guide

| Question | Variable Type | Recommended Chart |
|---|---|---|
| What does one numeric variable look like? | One numeric column | Histogram |
| How many observations are in each category? | One categorical column | Count plot / bar chart |
| Are two numeric variables related? | Two numeric columns | Scatterplot |
| How does a numeric variable differ across categories? | Numeric + categorical | Boxplot |
| How does a value change across ordered values? | Ordered x-axis + numeric y-axis | Line plot |
| How do several numeric variables relate? | Multiple numeric columns | Heatmap |
| Does a pattern differ by group? | Extra categorical variable | `hue` / subplots |

## Examples with `mpg`

| Question | Columns | Chart Type |
|---|---|---|
| What does fuel efficiency look like? | `mpg` | Histogram |
| How many cars are from each origin? | `origin` | Count plot |
| Are heavier cars less fuel efficient? | `weight`, `mpg` | Scatterplot |
| Does fuel efficiency differ by origin? | `origin`, `mpg` | Boxplot |
| How did average MPG change over model years? | `model_year`, `mpg` | Line plot |

## Mini Exercise

Complete the table.

| Question | Columns Needed | Chart Type |
|---|---|---|
| What does car weight look like? |  |  |
| Are horsepower and MPG related? |  |  |
| How many cars are from each origin? |  |  |
| Does MPG differ by cylinder count? |  |  |
| How did average weight change by model year? |  |  |

Possible answers:

| Question | Columns Needed | Chart Type |
|---|---|---|
| What does car weight look like? | `weight` | Histogram |
| Are horsepower and MPG related? | `horsepower`, `mpg` | Scatterplot |
| How many cars are from each origin? | `origin` | Count plot |
| Does MPG differ by cylinder count? | `cylinders`, `mpg` | Boxplot |
| How did average weight change by model year? | `model_year`, `weight` | Line plot |

---

# Section 2: Anatomy of a Matplotlib Plot

## Core Idea

A plot has several parts:

| Plot Part | Meaning |
|---|---|
| Figure | The overall canvas |
| Plot type | Histogram, scatterplot, bar chart, etc. |
| Title | Explains what the chart shows |
| X-axis label | Describes the horizontal axis |
| Y-axis label | Describes the vertical axis |
| Figure size | Controls how large the chart appears |

## Basic Histogram

```python
plt.hist(mpg["mpg"])
plt.show()
```

This works, but it is not very readable yet.

## Add Figure Size, Title, and Labels

```python
plt.figure(figsize=(10, 6))

plt.hist(mpg["mpg"], bins=20, edgecolor="black")

plt.title("Distribution of Miles Per Gallon")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

## Explanation

```python
plt.figure(figsize=(10, 6))
```

sets the size of the figure.

```python
plt.hist(mpg["mpg"], bins=20, edgecolor="black")
```

creates a histogram using the `mpg` column.

```python
plt.title("Distribution of Miles Per Gallon")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")
```

adds context so the viewer understands the chart.

## Practice

Create a histogram showing the distribution of `weight`.

```python
plt.figure(figsize=(10, 6))

plt.hist(mpg["weight"], bins=20, edgecolor="black")

plt.title("Distribution of Car Weight")
plt.xlabel("Weight")
plt.ylabel("Number of Cars")

plt.show()
```

---

# Section 3: Seaborn as a Higher-Level Plotting Tool

## Core Idea

Seaborn is built on top of Matplotlib. Seaborn makes many common statistical plots easier to create, especially when working with pandas DataFrames.

A useful beginner framing:

> Use Seaborn to create common plots quickly. Use Matplotlib to size, label, and display the chart.

## Matplotlib Histogram

```python
plt.figure(figsize=(10, 6))

plt.hist(mpg["mpg"], bins=20, edgecolor="black")

plt.title("Distribution of Miles Per Gallon")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

## Seaborn Histogram

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="mpg", bins=20)

plt.title("Distribution of Miles Per Gallon")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

## Syntax Comparison

| Library | Common Style |
|---|---|
| Matplotlib | `plt.hist(mpg["mpg"])` |
| Seaborn | `sns.histplot(data=mpg, x="mpg")` |

## Count Plot

A count plot is useful for one categorical variable.

Question:

> How many cars are from each origin?

```python
plt.figure(figsize=(8, 5))

sns.countplot(data=mpg, x="origin")

plt.title("Number of Cars by Origin")
plt.xlabel("Origin")
plt.ylabel("Number of Cars")

plt.show()
```

This chart visualizes the same information as:

```python
mpg["origin"].value_counts()
```

## Ordering Categories

```python
origin_order = mpg["origin"].value_counts().index

plt.figure(figsize=(8, 5))

sns.countplot(data=mpg, x="origin", order=origin_order)

plt.title("Number of Cars by Origin")
plt.xlabel("Origin")
plt.ylabel("Number of Cars")

plt.show()
```

## Preview: Adding `hue`

```python
plt.figure(figsize=(10, 6))

sns.countplot(data=mpg, x="origin", hue="cylinders")

plt.title("Number of Cars by Origin and Cylinder Count")
plt.xlabel("Origin")
plt.ylabel("Number of Cars")

plt.show()
```

`hue` adds another variable using color groups.

## Practice

Create a Seaborn histogram of the `weight` column.

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="weight", bins=20)

plt.title("Distribution of Car Weight")
plt.xlabel("Weight")
plt.ylabel("Number of Cars")

plt.show()
```

Create a count plot showing the number of cars for each cylinder count.

```python
plt.figure(figsize=(8, 5))

sns.countplot(data=mpg, x="cylinders")

plt.title("Number of Cars by Cylinder Count")
plt.xlabel("Cylinders")
plt.ylabel("Number of Cars")

plt.show()
```

---

# Section 4: One Numeric Variable

## Core Idea

When we have one numeric variable, we often want to understand its distribution.

A distribution helps answer:

- What values are common?
- What values are rare?
- Is the data spread out?
- Is the data skewed?
- Are there unusual values?

For one numeric variable, start with a histogram.

## Preview the Variable

```python
mpg["mpg"].describe()
```

## Histogram of MPG

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="mpg", bins=20)

plt.title("Distribution of Miles Per Gallon")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

## Interpret the Chart

Ask:

> What do you notice about the distribution of miles per gallon?

Possible observations:

- Most cars fall within a middle range of MPG values.
- There are fewer cars with very high MPG.
- The distribution is not perfectly symmetric.
- There may be clusters or unusual values.

## Compare Different Bin Counts

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="mpg", bins=10)

plt.title("Distribution of MPG with 10 Bins")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="mpg", bins=40)

plt.title("Distribution of MPG with 40 Bins")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

Teaching point:

- Too few bins can hide patterns.
- Too many bins can make the chart noisy.

## Optional: Add a KDE Curve

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="mpg", bins=20, kde=True)

plt.title("Distribution of Miles Per Gallon")
plt.xlabel("Miles Per Gallon")
plt.ylabel("Number of Cars")

plt.show()
```

A KDE curve is a smoothed version of the histogram shape.

## Practice

Create a histogram of `horsepower`.

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="horsepower", bins=20)

plt.title("Distribution of Horsepower")
plt.xlabel("Horsepower")
plt.ylabel("Number of Cars")

plt.show()
```

Create a histogram of `acceleration`.

```python
plt.figure(figsize=(10, 6))

sns.histplot(data=mpg, x="acceleration", bins=20)

plt.title("Distribution of Acceleration")
plt.xlabel("Acceleration")
plt.ylabel("Number of Cars")

plt.show()
```

## Key Takeaway

A histogram answers:

> What does this numeric variable look like?

---

# Section 5: One Categorical Variable

## Core Idea

When we have one categorical variable, we often want to count how many observations belong to each category.

For one categorical variable, use a count plot or bar chart.

## Preview the Variable

```python
mpg["origin"].value_counts()
```

## Count Plot of Origin

```python
plt.figure(figsize=(8, 5))

sns.countplot(data=mpg, x="origin")

plt.title("Number of Cars by Origin")
plt.xlabel("Origin")
plt.ylabel("Number of Cars")

plt.show()
```

## Interpret the Chart

Possible observations:

- This dataset contains more cars from the USA than Japan or Europe.
- Japan and Europe have fewer observations than the USA.

Use careful wording:

> This dataset contains more cars from the USA.

Avoid overgeneralizing:

> USA made the most cars.

## Order Categories by Frequency

```python
origin_order = mpg["origin"].value_counts().index

plt.figure(figsize=(8, 5))

sns.countplot(data=mpg, x="origin", order=origin_order)

plt.title("Number of Cars by Origin")
plt.xlabel("Origin")
plt.ylabel("Number of Cars")

plt.show()
```

## Count Plot of Cylinders

```python
plt.figure(figsize=(8, 5))

sns.countplot(data=mpg, x="cylinders")

plt.title("Number of Cars by Cylinder Count")
plt.xlabel("Cylinders")
plt.ylabel("Number of Cars")

plt.show()
```

Teaching note:

`cylinders` is numeric, but it behaves like a category here because it contains a small number of repeated values.

## Horizontal Bar Chart for Long Labels

If category labels are long, a horizontal bar chart may be easier to read.

```python
top_names = mpg["name"].value_counts().head(10)

plt.figure(figsize=(10, 6))

plt.barh(top_names.index, top_names.values)

plt.title("Top 10 Most Common Car Names")
plt.xlabel("Number of Cars")
plt.ylabel("Car Name")

plt.show()
```

## Count Plot vs. Bar Chart

| Situation | Use |
|---|---|
| Raw categorical column | `sns.countplot()` |
| Already summarized values | `plt.bar()` or `sns.barplot()` |

## Practice

Create a count plot for `model_year`.

```python
plt.figure(figsize=(10, 5))

sns.countplot(data=mpg, x="model_year")

plt.title("Number of Cars by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Number of Cars")

plt.show()
```

## Key Takeaway

A count plot answers:

> How many observations are in each group?

---

# Section 6: Numeric vs. Numeric

## Core Idea

When we have two numeric variables, we often want to understand whether they have a relationship.

For two numeric variables, use a scatterplot.

## Guiding Question

> Are heavier cars generally less fuel efficient?

Columns:

- `weight`
- `mpg`

## Scatterplot with Matplotlib

```python
plt.figure(figsize=(10, 6))

plt.scatter(mpg["weight"], mpg["mpg"])

plt.title("Car Weight vs. Miles Per Gallon")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## Scatterplot with Seaborn

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg")

plt.title("Car Weight vs. Miles Per Gallon")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## Add Transparency

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", alpha=0.7)

plt.title("Car Weight vs. Miles Per Gallon")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.show()
```

`alpha` controls transparency. This can help when points overlap.

## Optional: Add a Trend Line

```python
plt.figure(figsize=(10, 6))

sns.regplot(data=mpg, x="weight", y="mpg", scatter_kws={"alpha": 0.7})

plt.title("Car Weight vs. Miles Per Gallon")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.show()
```

Instructor note:

Keep this visual. Do not turn it into a regression lesson.

## Careful Interpretation

Better:

> In this dataset, heavier cars are associated with lower MPG.

Avoid:

> Weight causes MPG to decrease.

A scatterplot shows association, not proof of causation.

## Practice

Create a scatterplot of `displacement` vs. `mpg`.

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="displacement", y="mpg", alpha=0.7)

plt.title("Engine Displacement vs. Miles Per Gallon")
plt.xlabel("Displacement")
plt.ylabel("Miles Per Gallon")

plt.show()
```

Create a scatterplot of `horsepower` vs. `weight`.

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="horsepower", alpha=0.7)

plt.title("Car Weight vs. Horsepower")
plt.xlabel("Weight")
plt.ylabel("Horsepower")

plt.show()
```

## Key Takeaway

A scatterplot answers:

> How do two numeric variables relate to each other?

---

# Section 7: Numeric vs. Categorical

## Core Idea

When we have one numeric variable and one categorical variable, we often want to compare the numeric variable across groups.

For this, use a boxplot.

## Guiding Question

> Does fuel efficiency differ by car origin?

Columns:

- `origin`
- `mpg`

## Preview the Summary

```python
mpg.groupby("origin")["mpg"].describe()
```

## Basic Boxplot

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="origin", y="mpg")

plt.title("Miles Per Gallon by Origin")
plt.xlabel("Origin")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## How to Read a Boxplot

| Boxplot Part | Meaning |
|---|---|
| Line inside the box | Median |
| Bottom of the box | 25th percentile |
| Top of the box | 75th percentile |
| Height of the box | Middle 50% of values |
| Whiskers | Typical low/high range |
| Dots beyond whiskers | Possible outliers |

Beginner questions to ask:

- Where is the middle?
- How spread out are the values?
- Are there unusual values?
- Do the groups look different?

## Compare MPG by Cylinder Count

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="cylinders", y="mpg")

plt.title("Miles Per Gallon by Cylinder Count")
plt.xlabel("Cylinders")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## Boxplot vs. Bar Chart

A bar chart can show the average value per group:

```python
avg_mpg_by_origin = mpg.groupby("origin")["mpg"].mean().reset_index()

plt.figure(figsize=(8, 5))

sns.barplot(data=avg_mpg_by_origin, x="origin", y="mpg")

plt.title("Average Miles Per Gallon by Origin")
plt.xlabel("Origin")
plt.ylabel("Average Miles Per Gallon")

plt.show()
```

But a boxplot shows more of the distribution:

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="origin", y="mpg")

plt.title("Miles Per Gallon by Origin")
plt.xlabel("Origin")
plt.ylabel("Miles Per Gallon")

plt.show()
```

Use a bar chart when comparing one summary number per group.

Use a boxplot when comparing distributions across groups.

## Optional: Strip Plot

A strip plot shows individual observations.

```python
plt.figure(figsize=(8, 5))

sns.stripplot(data=mpg, x="origin", y="mpg", alpha=0.5)

plt.title("Individual MPG Values by Origin")
plt.xlabel("Origin")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## Optional: Boxplot Plus Strip Plot

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="origin", y="mpg")
sns.stripplot(data=mpg, x="origin", y="mpg", alpha=0.4)

plt.title("Miles Per Gallon by Origin")
plt.xlabel("Origin")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## Optional: Violin Plot

A violin plot shows the shape of the distribution for each group.

```python
plt.figure(figsize=(8, 5))

sns.violinplot(data=mpg, x="origin", y="mpg")

plt.title("Miles Per Gallon by Origin")
plt.xlabel("Origin")
plt.ylabel("Miles Per Gallon")

plt.show()
```

Simple explanation:

> Wider parts of the violin mean more observations are concentrated around that value.

## Instructor Note

Boxplots are the main chart type for this section. Strip plots and violin plots are optional extensions. If students are struggling with quartiles, medians, and whiskers, keep the focus on the boxplot and skip the optional plots during live instruction.

## Practice

Create a boxplot comparing `horsepower` across `origin`.

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="origin", y="horsepower")

plt.title("Horsepower by Origin")
plt.xlabel("Origin")
plt.ylabel("Horsepower")

plt.show()
```

Create a boxplot comparing `weight` across `cylinders`.

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="cylinders", y="weight")

plt.title("Car Weight by Cylinder Count")
plt.xlabel("Cylinders")
plt.ylabel("Weight")

plt.show()
```

## Key Takeaway

A boxplot answers:

> How does a numeric variable differ across groups?

---

# Section 8: Trends Over Ordered Values

## Core Idea

When the x-axis has a meaningful order, we can use a line plot to show how a value changes across that order.

Line plots are useful for:

- time
- years
- months
- ordered categories
- sequences

In this dataset, `model_year` is ordered.

## Guiding Question

> How did average MPG change over model years?

Columns:

- `model_year`
- `mpg`

## Group First with pandas

```python
avg_mpg_by_year = mpg.groupby("model_year")["mpg"].mean().reset_index()

avg_mpg_by_year
```

This calculates one average MPG value per model year.

## Line Plot

```python
plt.figure(figsize=(10, 6))

sns.lineplot(data=avg_mpg_by_year, x="model_year", y="mpg", marker="o")

plt.title("Average MPG by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Average Miles Per Gallon")

plt.show()
```

## Interpret the Chart

Possible observations:

- Average MPG generally increases over time.
- There is some year-to-year variation.
- Later model years tend to have higher average MPG than earlier model years.

Use careful wording:

> In this dataset, average MPG tends to increase across model years.

## Scatterplot vs. Line Plot

A scatterplot shows individual observations.

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="model_year", y="mpg", alpha=0.6)

plt.title("Individual Car MPG by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Miles Per Gallon")

plt.show()
```

A line plot shows the summarized trend.

```python
plt.figure(figsize=(10, 6))

sns.lineplot(data=avg_mpg_by_year, x="model_year", y="mpg", marker="o")

plt.title("Average MPG by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Average Miles Per Gallon")

plt.show()
```

## Let Seaborn Calculate the Mean Automatically

Seaborn can calculate the average directly when there are multiple rows with the same x-value.

```python
plt.figure(figsize=(10, 6))

sns.lineplot(data=mpg, x="model_year", y="mpg", marker="o", errorbar=None)

plt.title("Average MPG by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Average Miles Per Gallon")

plt.show()
```

`errorbar=None` removes the uncertainty band.

Teaching recommendation:

Show the manual `groupby()` version first so students understand what is being summarized. Then show the shorter Seaborn version as a convenience.

## Common Mistake: Line Plot for Unordered Categories

A line plot implies order. Avoid using line plots for unordered categories like `origin`.

Better chart for `origin` and `mpg`:

```python
plt.figure(figsize=(8, 5))

sns.boxplot(data=mpg, x="origin", y="mpg")

plt.title("MPG by Origin")
plt.xlabel("Origin")
plt.ylabel("Miles Per Gallon")

plt.show()
```

## Practice

Create a line plot showing average horsepower by model year.

```python
avg_hp_by_year = mpg.groupby("model_year")["horsepower"].mean().reset_index()

plt.figure(figsize=(10, 6))

sns.lineplot(data=avg_hp_by_year, x="model_year", y="horsepower", marker="o")

plt.title("Average Horsepower by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Average Horsepower")

plt.show()
```

Create a line plot showing average displacement by model year.

```python
avg_displacement_by_year = mpg.groupby("model_year")["displacement"].mean().reset_index()

plt.figure(figsize=(10, 6))

sns.lineplot(data=avg_displacement_by_year, x="model_year", y="displacement", marker="o")

plt.title("Average Displacement by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Average Displacement")

plt.show()
```

## Key Takeaway

A line plot answers:

> How does a value change across an ordered variable?

---

# Section 9: Multiple Variables

## Core Idea

Once students understand basic chart types, they can add more variables to a plot.

Three common techniques are:

| Technique | Purpose |
|---|---|
| `hue` | Adds color groups |
| Subplots | Shows multiple related plots together |
| Heatmap | Shows relationships among several numeric variables |

This section should be treated as a controlled preview, not a deep dive.

## Add `hue` to a Scatterplot

Guiding question:

> Does the relationship between weight and MPG look different by origin?

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", hue="origin", alpha=0.7)

plt.title("Car Weight vs. MPG by Origin")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.show()
```

`hue="origin"` uses color to split the points by `origin`.

## Interpret the Plot

Possible observations:

- USA cars appear more common among heavier vehicles.
- Japanese and European cars appear more common among lighter vehicles.
- The overall relationship between weight and MPG is still negative.

## Use `hue` with a Boxplot

```python
plt.figure(figsize=(10, 6))

sns.boxplot(data=mpg, x="cylinders", y="mpg", hue="origin")

plt.title("MPG by Cylinder Count and Origin")
plt.xlabel("Cylinders")
plt.ylabel("Miles Per Gallon")

plt.show()
```

Teaching caution:

Adding more variables can be powerful, but every extra variable increases complexity.

## Bad Use of `hue`

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", hue="name", alpha=0.7)

plt.title("Car Weight vs. MPG by Car Name")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.show()
```

This is usually a bad chart because `name` has too many unique values.

Rule:

> Use `hue` when the grouping variable has a small number of meaningful categories.

Good examples:

- `origin`
- `cylinders`

Poor example:

- `name`

## Subplots

Subplots let us put multiple charts in one figure.

Guiding question:

> What do the distributions of MPG, weight, and horsepower look like?

```python
plt.figure(figsize=(16, 4))

plt.subplot(1, 3, 1)
sns.histplot(data=mpg, x="mpg", bins=20)
plt.title("Distribution of MPG")
plt.xlabel("MPG")
plt.ylabel("Number of Cars")

plt.subplot(1, 3, 2)
sns.histplot(data=mpg, x="weight", bins=20)
plt.title("Distribution of Weight")
plt.xlabel("Weight")
plt.ylabel("Number of Cars")

plt.subplot(1, 3, 3)
sns.histplot(data=mpg, x="horsepower", bins=20)
plt.title("Distribution of Horsepower")
plt.xlabel("Horsepower")
plt.ylabel("Number of Cars")

plt.tight_layout()
plt.show()
```

Explanation:

```python
plt.subplot(1, 3, 1)
```

means 1 row, 3 columns, first plot.

```python
plt.subplot(1, 3, 2)
```

means 1 row, 3 columns, second plot.

```python
plt.subplot(1, 3, 3)
```

means 1 row, 3 columns, third plot.

Use `plt.tight_layout()` to reduce overlapping labels and titles.

## Heatmap

Guiding question:

> Which numeric variables are most strongly related to each other?

```python
numeric_cols = [
    "mpg",
    "cylinders",
    "displacement",
    "horsepower",
    "weight",
    "acceleration",
    "model_year"
]

mpg_corr = mpg[numeric_cols].corr()
mpg_corr
```

```python
plt.figure(figsize=(10, 6))

sns.heatmap(mpg_corr, annot=True)

plt.title("Correlation Heatmap of Numeric MPG Variables")

plt.show()
```

## Heatmap with a Diverging Palette

```python
plt.figure(figsize=(10, 6))

sns.heatmap(mpg_corr, annot=True, cmap="coolwarm", center=0)

plt.title("Correlation Heatmap of Numeric MPG Variables")

plt.show()
```

Teaching point:

For correlations, negative and positive values matter, so a diverging color palette is useful.

## Common Mistake: Too Much Information

More variables do not always make a better chart.

A chart can become hard to read if it has:

- too many colors
- too many categories
- too many subplots
- too many labels
- too many overlapping points

A good visualization makes the important pattern easier to see.

## Practice

Create a scatterplot of `horsepower` vs. `mpg`, colored by `origin`.

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="horsepower", y="mpg", hue="origin", alpha=0.7)

plt.title("Horsepower vs. MPG by Origin")
plt.xlabel("Horsepower")
plt.ylabel("Miles Per Gallon")

plt.show()
```

Create three histograms as subplots.

```python
plt.figure(figsize=(16, 4))

plt.subplot(1, 3, 1)
sns.histplot(data=mpg, x="mpg", bins=20)
plt.title("Distribution of MPG")
plt.xlabel("MPG")
plt.ylabel("Number of Cars")

plt.subplot(1, 3, 2)
sns.histplot(data=mpg, x="weight", bins=20)
plt.title("Distribution of Weight")
plt.xlabel("Weight")
plt.ylabel("Number of Cars")

plt.subplot(1, 3, 3)
sns.histplot(data=mpg, x="acceleration", bins=20)
plt.title("Distribution of Acceleration")
plt.xlabel("Acceleration")
plt.ylabel("Number of Cars")

plt.tight_layout()
plt.show()
```

Create a correlation heatmap.

```python
numeric_cols = [
    "mpg",
    "cylinders",
    "displacement",
    "horsepower",
    "weight",
    "acceleration",
    "model_year"
]

mpg_corr = mpg[numeric_cols].corr()

plt.figure(figsize=(10, 6))

sns.heatmap(mpg_corr, annot=True, cmap="coolwarm", center=0)

plt.title("Correlation Heatmap of Numeric MPG Variables")

plt.show()
```

## Key Takeaway

Multiple-variable plots are powerful, but they should still make the data easier to understand.

---

# Section 10: Polishing and Exporting Visualizations

## Core Idea

A plot is not finished just because the code runs.

A finished visualization should be:

- readable
- clearly titled
- labeled
- sized appropriately
- not cluttered
- exportable as an image file

## Figure Size

```python
plt.figure(figsize=(10, 6))
```

`figsize` controls the width and height of the chart.

## Titles and Labels

```python
plt.title("Car Weight vs. Miles Per Gallon")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")
```

A viewer should understand what the chart shows without needing to inspect the code.

## Rotate X-Axis Labels

Useful when labels are crowded.

```python
plt.figure(figsize=(10, 6))

sns.countplot(data=mpg, x="model_year")

plt.title("Number of Cars by Model Year")
plt.xlabel("Model Year")
plt.ylabel("Number of Cars")
plt.xticks(rotation=45)

plt.show()
```

## Use `plt.tight_layout()`

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", hue="origin", alpha=0.7)

plt.title("Car Weight vs. MPG by Origin")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.tight_layout()
plt.show()
```

`plt.tight_layout()` helps prevent labels and titles from overlapping.

## Export as PNG

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", hue="origin", alpha=0.7)

plt.title("Car Weight vs. MPG by Origin")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.tight_layout()
plt.savefig("weight_vs_mpg.png", dpi=300)

plt.show()
```

This line saves the plot:

```python
plt.savefig("weight_vs_mpg.png", dpi=300)
```

| Argument | Meaning |
|---|---|
| `"weight_vs_mpg.png"` | File name |
| `dpi=300` | Image resolution |
| `.png` | File type |

## Export as JPG

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", hue="origin", alpha=0.7)

plt.title("Car Weight vs. MPG by Origin")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.tight_layout()
plt.savefig("weight_vs_mpg.jpg", dpi=300)

plt.show()
```

## Important Ordering Note

Call `savefig()` before `plt.show()`.

Preferred:

```python
plt.savefig("my_plot.png", dpi=300)
plt.show()
```

Avoid:

```python
plt.show()
plt.savefig("my_plot.png", dpi=300)
```

In some environments, calling `plt.show()` first can clear or finalize the figure before it gets saved.

## Optional: Remove Extra Whitespace

```python
plt.savefig("weight_vs_mpg.png", dpi=300, bbox_inches="tight")
```

Full example:

```python
plt.figure(figsize=(10, 6))

sns.scatterplot(data=mpg, x="weight", y="mpg", hue="origin", alpha=0.7)

plt.title("Car Weight vs. MPG by Origin")
plt.xlabel("Weight")
plt.ylabel("Miles Per Gallon")

plt.tight_layout()
plt.savefig("weight_vs_mpg.png", dpi=300, bbox_inches="tight")

plt.show()
```

---

# Module Review

## Chart Type Summary

| Question | Chart Type | Example |
|---|---|---|
| What does one numeric variable look like? | Histogram | `sns.histplot(data=mpg, x="mpg")` |
| How many observations are in each category? | Count plot | `sns.countplot(data=mpg, x="origin")` |
| Are two numeric variables related? | Scatterplot | `sns.scatterplot(data=mpg, x="weight", y="mpg")` |
| How does a numeric variable differ across groups? | Boxplot | `sns.boxplot(data=mpg, x="origin", y="mpg")` |
| How does a value change across ordered values? | Line plot | `sns.lineplot(data=summary, x="model_year", y="mpg")` |
| How do several numeric variables relate? | Heatmap | `sns.heatmap(corr, annot=True)` |
| Does a pattern differ by group? | Hue | `sns.scatterplot(..., hue="origin")` |

## Big Takeaways

- Start with the question, not the chart.
- Match the chart type to the variable types.
- Use Seaborn for quick DataFrame-friendly plotting.
- Use Matplotlib for sizing, titles, labels, layout, and exporting.
- Keep charts readable.
- More variables do not automatically make a better chart.
- A finished chart should be understandable and exportable.

---

# Recommended Closing Discussion

Ask students:

1. What chart type would you use for one numeric variable?
2. What chart type would you use for one categorical variable?
3. What chart type would you use for two numeric variables?
4. Why should we avoid using line plots for unordered categories?
5. What does `hue` do in Seaborn?
6. Why should we call `plt.savefig()` before `plt.show()`?

