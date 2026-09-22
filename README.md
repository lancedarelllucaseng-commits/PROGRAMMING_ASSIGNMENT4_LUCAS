# PROGRAMMING_ASSIGNMENT4_LUCAS
# 📊 ECE 2112 — Experiment 4: Data Wrangling and Data Visualization

## 📋 Table of Contents

- [Overview](#overview)
- [Function Summary](#function-summary)
- [Problem Specifications & Solutions](#problem-specifications--solutions)
  - [A. Visayas Communication DataFrame](#a-visayas-communication-dataframe)
  - [B. Visayas Female DataFrame](#b-visayas-female-dataframe)
  - [C. Category-Average Visualization](#c-category-average-visualization)
- [Project File Structure](#project-file-structure)
- [Prerequisites & Requirements](#prerequisites--requirements)
- [How to Run](#how-to-run)
- [Edge Cases Handled](#edge-cases-handled)
- [Complete Code](#complete-code)
- [Conclusion](#conclusion)

---

# Overview

This project contains the Python code for **ECE 2112: Advanced Computer Programming and Algorithms — Experiment 4: Data Wrangling and Data Visualization**.

The activity uses Pandas for filtering and organizing tabular data and Matplotlib for creating visualizations.

The program uses the **ECE Board Exam 2 dataset** and performs the following tasks:

- Creates a DataFrame containing Visayas students under the Communication track.
- Creates a DataFrame containing female students from the Visayas.
- Filters Visayas female students whose Average is at least 60.
- Calculates the mean Average for each Track.
- Calculates the mean Average for each Gender.
- Calculates the mean Average for each Hometown.
- Creates three bar charts in one figure.
- Identifies the category with the highest sample mean for Track, Gender, and Hometown.

The laboratory instructions require the data and plotted values to be derived from the dataset and the original DataFrame to remain unchanged. :contentReference[oaicite:0]{index=0}

---

# Function Summary

| Function / Code | Purpose |
|---|---|
| `pd.read_csv()` | Loads the CSV dataset into a Pandas DataFrame. |
| `.copy()` | Creates a copy of the original DataFrame. |
| `.loc[]` | Selects rows and columns using labels and conditions. |
| `.mean()` | Calculates the arithmetic mean. |
| `.groupby()` | Groups data according to a categorical column. |
| `.reset_index()` | Converts grouped results back into a DataFrame. |
| `.rename()` | Changes column names. |
| `.idxmax()` | Finds the index containing the highest value. |
| `.max()` | Returns the highest value. |
| `plt.subplots()` | Creates multiple plots within one figure. |
| `.bar()` | Creates a bar chart. |
| `.set_title()` | Adds a title to a graph. |
| `.set_xlabel()` | Labels the x-axis. |
| `.set_ylabel()` | Labels the y-axis. |
| `.set_ylim()` | Sets the y-axis limits. |
| `plt.tight_layout()` | Adjusts the spacing between plots. |
| `plt.show()` | Displays the figure. |
| `print()` | Displays text and values. |
| `display()` | Displays DataFrames in Jupyter Notebook. |

---

# Problem Specifications & Solutions

## A. Visayas Communication DataFrame

The first problem requires a DataFrame named `VisComm`.

The DataFrame contains students who satisfy both of the following conditions:

1. `Hometown` is `Visayas`
2. `Track` is `Communication`

Only the following columns are retained:

- `Name`
- `Gender`
- `Math`
- `Electronics`
- `Average`

```python
VisComm = df_with_average.loc[
    (df_with_average["Hometown"] == "Visayas") &
    (df_with_average["Track"] == "Communication"),
    ["Name", "Gender", "Math", "Electronics", "Average"]
]

display(VisComm)

print("Number of rows in VisComm:", len(VisComm))
```

The two filtering conditions are applied to the source dataset before selecting the required columns.

---

# B. Visayas Female DataFrame

The second problem requires a DataFrame named `VisFemale`.

The DataFrame contains students who satisfy both:

1. `Hometown` is `Visayas`
2. `Gender` is `Female`

The retained columns are:

- `Name`
- `Track`
- `GEAS`
- `Electronics`
- `Average`

```python
VisFemale = df_with_average.loc[
    (df_with_average["Hometown"] == "Visayas") &
    (df_with_average["Gender"] == "Female"),
    ["Name", "Track", "GEAS", "Electronics", "Average"]
]

display(VisFemale)
```

### B.1 Average Greater Than or Equal to 60

The `VisFemale` DataFrame is filtered to display only students whose `Average` is at least 60.

The original `VisFemale` DataFrame is not overwritten.

```python
VisFemale_60 = VisFemale.loc[
    VisFemale["Average"] >= 60
]

display(VisFemale_60)
```

---

# C. Category-Average Visualization

This section examines the recorded `Average` according to the following categorical features:

- Track
- Gender
- Hometown

The laboratory instructions require the mean Average for every category and three bar charts in one figure. :contentReference[oaicite:1]{index=1}

---

## C.a Mean Average by Track

```python
track_mean = (
    df_with_average
    .groupby("Track")["Average"]
    .mean()
    .reset_index()
)

track_mean = track_mean.rename(
    columns={"Average": "Mean Average"}
)

display(track_mean)
```

---

## C.a Mean Average by Gender

```python
gender_mean = (
    df_with_average
    .groupby("Gender")["Average"]
    .mean()
    .reset_index()
)

gender_mean = gender_mean.rename(
    columns={"Average": "Mean Average"}
)

display(gender_mean)
```

---

## C.a Mean Average by Hometown

```python
hometown_mean = (
    df_with_average
    .groupby("Hometown")["Average"]
    .mean()
    .reset_index()
)

hometown_mean = hometown_mean.rename(
    columns={"Average": "Mean Average"}
)

display(hometown_mean)
```

---

## C.b Display the Three Summary Tables

The three summary tables are displayed using:

```python
print("\n1. Mean Average by Track")
display(track_mean)

print("\n2. Mean Average by Gender")
display(gender_mean)

print("\n3. Mean Average by Hometown")
display(hometown_mean)
```

---

## C.c Create One Figure with Three Bar Charts

The following code creates one figure containing:

1. Mean Average by Track
2. Mean Average by Gender
3. Mean Average by Hometown

The three charts use a consistent y-axis scale.

```python
# Find the largest mean value so all three graphs
# can use the same y-axis scale.

maximum_mean = max(
    track_mean["Mean Average"].max(),
    gender_mean["Mean Average"].max(),
    hometown_mean["Mean Average"].max()
)

y_limit = maximum_mean * 1.10


fig, axes = plt.subplots(
    1,
    3,
    figsize=(18, 6)
)


# ------------------------------------------------------------
# BAR CHART 1: TRACK
# ------------------------------------------------------------

axes[0].bar(
    track_mean["Track"],
    track_mean["Mean Average"]
)

axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average")
axes[0].set_ylim(0, y_limit)

axes[0].tick_params(
    axis="x",
    rotation=45
)


# ------------------------------------------------------------
# BAR CHART 2: GENDER
# ------------------------------------------------------------

axes[1].bar(
    gender_mean["Gender"],
    gender_mean["Mean Average"]
)

axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average")
axes[1].set_ylim(0, y_limit)

axes[1].tick_params(
    axis="x",
    rotation=45
)


# ------------------------------------------------------------
# BAR CHART 3: HOMETOWN
# ------------------------------------------------------------

axes[2].bar(
    hometown_mean["Hometown"],
    hometown_mean["Mean Average"]
)

axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average")
axes[2].set_ylim(0, y_limit)

axes[2].tick_params(
    axis="x",
    rotation=45
)


# Prevent overlapping labels

plt.tight_layout()

# Display the figure

plt.show()
```

---

## C.d Three Interpretation Statements

The category with the highest sample mean is determined automatically using `.idxmax()`.

```python
highest_track = track_mean.loc[
    track_mean["Mean Average"].idxmax()
]

highest_gender = gender_mean.loc[
    gender_mean["Mean Average"].idxmax()
]

highest_hometown = hometown_mean.loc[
    hometown_mean["Mean Average"].idxmax()
]
```

The three statements are then displayed:

```python
print(
    f"1. Among the Track categories, "
    f"{highest_track['Track']} has the highest sample mean "
    f"Average of {highest_track['Mean Average']:.2f}."
)

print(
    f"2. Among the Gender categories, "
    f"{highest_gender['Gender']} has the highest sample mean "
    f"Average of {highest_gender['Mean Average']:.2f}."
)

print(
    f"3. Among the Hometown categories, "
    f"{highest_hometown['Hometown']} has the highest sample mean "
    f"Average of {highest_hometown['Mean Average']:.2f}."
)
```

These statements describe the observed dataset only. A difference in group means does not by itself establish that a categorical feature causes a higher board-exam score. :contentReference[oaicite:2]{index=2}

---

# Project File Structure

```text
Programming_Assignment4_LUCAS/
│
├── board2.csv
├── ECE2112_PA4_LUCAS.ipynb
└── README.md
```

### File Descriptions

| File | Description |
|---|---|
| `board2.csv` | ECE Board Exam 2 dataset used in the activity. |
| `ECE2112_PA4_LUCAS.ipynb` | Jupyter Notebook containing the Python code, outputs, tables, and graphs. |
| `README.md` | Documentation explaining the programming activity and code. |

---

# Prerequisites & Requirements

The following are required:

- Python 3.x
- Pandas
- Matplotlib
- Jupyter Notebook

### Install Pandas

```bash
pip install pandas
```

### Install Matplotlib

```bash
pip install matplotlib
```

### Install Jupyter Notebook

```bash
pip install notebook
```

---

# How to Run

## Using Jupyter Notebook

### Step 1 — Prepare the Files

Place these files in the same folder:

```text
board2.csv
ECE2112_PA4.ipynb
README.md
```

### Step 2 — Open Jupyter Notebook

Open Command Prompt or Terminal and run:

```bash
jupyter notebook
```

### Step 3 — Open the Notebook

Open:

```text
ECE2112_PA4.ipynb
```

### Step 4 — Run the Cells

Run the cells from beginning to end.

Make sure that:

- `VisComm` is displayed.
- The number of rows in `VisComm` is displayed.
- `VisFemale` is displayed.
- `VisFemale_60` is displayed.
- The three category-mean tables are displayed.
- The figure containing the three bar charts is displayed.
- The three interpretation statements are displayed.
- No errors occur.

The submission requirements specify that the notebook should contain the required DataFrames, three category-mean summaries, completed figure, and three interpretation statements, with all cells executed. :contentReference[oaicite:3]{index=3}

---

# Edge Cases Handled

## 1. Multiple Filtering Conditions

Both conditions are explicitly included in the filtering expressions.

For `VisComm`:

```python
(df_with_average["Hometown"] == "Visayas") &
(df_with_average["Track"] == "Communication")
```

For `VisFemale`:

```python
(df_with_average["Hometown"] == "Visayas") &
(df_with_average["Gender"] == "Female")
```

---

## 2. Original DataFrame Is Preserved

The original DataFrame is not modified directly.

Instead, a copy is created:

```python
df_with_average = df.copy()
```

The `Average` column is then added to the copy.

---

## 3. VisFemale Is Not Overwritten

The original `VisFemale` DataFrame remains unchanged.

A separate DataFrame is created for the `Average >= 60` condition:

```python
VisFemale_60 = VisFemale.loc[
    VisFemale["Average"] >= 60
]
```

---

## 4. Consistent Graph Scale

The maximum mean value among Track, Gender, and Hometown is obtained:

```python
maximum_mean = max(
    track_mean["Mean Average"].max(),
    gender_mean["Mean Average"].max(),
    hometown_mean["Mean Average"].max()
)
```

The same y-axis limit is then applied to all three graphs.

---

## 5. Readable Category Labels

The x-axis labels are rotated by 45 degrees:

```python
axes[0].tick_params(
    axis="x",
    rotation=45
)
```

The same setting is applied to the Gender and Hometown graphs.

---

# Complete Code

The complete code used in the Jupyter Notebook is provided below.

```python
import pandas as pd
import matplotlib.pyplot as plt



# LOAD DATASET

df = pd.read_csv("board2.csv")

print("Dataset loaded successfully.")
print("Number of rows:", df.shape[0])
print("Number of columns:", df.shape[1])

display(df.head())



# CREATE AVERAGE COLUMN

# The uploaded dataset contains:
# Math, GEAS, Electronics, and Communication.
#
# The Experiment 4 instructions require an Average column.
# Therefore, the average of the four subject scores is calculated.

df_with_average = df.copy()

df_with_average["Average"] = df_with_average[
    ["Math", "GEAS", "Electronics", "Communication"]
].mean(axis=1)

print("\nDataset with Average column:")
display(df_with_average)



# A. VISAYAS COMMUNICATION DATAFRAME


print("A. VISAYAS COMMUNICATION DATAFRAME")


# Students must satisfy BOTH conditions:
# 1. Hometown is Visayas
# 2. Track is Communication

VisComm = df_with_average.loc[
    (df_with_average["Hometown"] == "Visayas") &
    (df_with_average["Track"] == "Communication"),
    ["Name", "Gender", "Math", "Electronics", "Average"]
]

print("\nVisComm:")
display(VisComm)

print("Number of rows in VisComm:", len(VisComm))



# B. VISAYAS FEMALE DATAFRAME


print("B. VISAYAS FEMALE DATAFRAME")

# Students must satisfy BOTH conditions:
# 1. Hometown is Visayas
# 2. Gender is Female

VisFemale = df_with_average.loc[
    (df_with_average["Hometown"] == "Visayas") &
    (df_with_average["Gender"] == "Female"),
    ["Name", "Track", "GEAS", "Electronics", "Average"]
]

print("\nVisFemale:")
display(VisFemale)



# B.1 AVERAGE >= 60


print("\nVisFemale students with Average >= 60:")

VisFemale_60 = VisFemale.loc[
    VisFemale["Average"] >= 60
]

display(VisFemale_60)



# C. CATEGORY-AVERAGE VISUALIZATION


print("C. CATEGORY-AVERAGE VISUALIZATION")



# C.a MEAN AVERAGE BY TRACK

track_mean = (
    df_with_average
    .groupby("Track")["Average"]
    .mean()
    .reset_index()
)

track_mean = track_mean.rename(
    columns={"Average": "Mean Average"}
)

print("\nMean Average by Track:")
display(track_mean)



# C.a MEAN AVERAGE BY GENDER

gender_mean = (
    df_with_average
    .groupby("Gender")["Average"]
    .mean()
    .reset_index()
)

gender_mean = gender_mean.rename(
    columns={"Average": "Mean Average"}
)

print("\nMean Average by Gender:")
display(gender_mean)

# C.a MEAN AVERAGE BY HOMETOWN

hometown_mean = (
    df_with_average
    .groupby("Hometown")["Average"]
    .mean()
    .reset_index()
)

hometown_mean = hometown_mean.rename(
    columns={"Average": "Mean Average"}
)

print("\nMean Average by Hometown:")
display(hometown_mean)



# C.b DISPLAY THREE SUMMARY TABLES

print("SUMMARY TABLES")

print("\n1. Mean Average by Track")
display(track_mean)

print("\n2. Mean Average by Gender")
display(gender_mean)

print("\n3. Mean Average by Hometown")
display(hometown_mean)



# C.c CREATE ONE FIGURE WITH THREE BAR CHARTS


# Find the largest mean value so all three graphs
# can use the same y-axis scale.

maximum_mean = max(
    track_mean["Mean Average"].max(),
    gender_mean["Mean Average"].max(),
    hometown_mean["Mean Average"].max()
)

y_limit = maximum_mean * 1.10


fig, axes = plt.subplots(
    1,
    3,
    figsize=(18, 6)
)



# BAR CHART 1: TRACK

axes[0].bar(
    track_mean["Track"],
    track_mean["Mean Average"]
)

axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average")
axes[0].set_ylim(0, y_limit)

axes[0].tick_params(
    axis="x",
    rotation=45
)



# BAR CHART 2: GENDER

axes[1].bar(
    gender_mean["Gender"],
    gender_mean["Mean Average"]
)

axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average")
axes[1].set_ylim(0, y_limit)

axes[1].tick_params(
    axis="x",
    rotation=45
)


# BAR CHART 3: HOMETOWN

axes[2].bar(
    hometown_mean["Hometown"],
    hometown_mean["Mean Average"]
)

axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average")
axes[2].set_ylim(0, y_limit)

axes[2].tick_params(
    axis="x",
    rotation=45
)


# Prevent overlapping labels

plt.tight_layout()

# Display the figure

plt.show()



# C.d THREE INTERPRETATION STATEMENTS

highest_track = track_mean.loc[
    track_mean["Mean Average"].idxmax()
]

highest_gender = gender_mean.loc[
    gender_mean["Mean Average"].idxmax()
]

highest_hometown = hometown_mean.loc[
    hometown_mean["Mean Average"].idxmax()
]


print("INTERPRETATION")


# Statement 1: Track

print(
    f"1. Among the Track categories, "
    f"{highest_track['Track']} has the highest sample mean "
    f"Average of {highest_track['Mean Average']:.2f}."
)


# Statement 2: Gender

print(
    f"2. Among the Gender categories, "
    f"{highest_gender['Gender']} has the highest sample mean "
    f"Average of {highest_gender['Mean Average']:.2f}."
)


# Statement 3: Hometown

print(
    f"3. Among the Hometown categories, "
    f"{highest_hometown['Hometown']} has the highest sample mean "
    f"Average of {highest_hometown['Mean Average']:.2f}."
)



# FINAL CHECK

print("FINAL CHECK")

print("VisComm rows:", len(VisComm))
print("VisFemale rows:", len(VisFemale))
print("VisFemale Average >= 60 rows:", len(VisFemale_60))

```

---

# Conclusion

The program demonstrates data wrangling and visualization using Pandas and Matplotlib. It filters the ECE Board Exam 2 dataset according to multiple categorical conditions, creates the required `VisComm` and `VisFemale` DataFrames, and performs an additional numerical filter for students with an Average of at least 60. It also calculates the mean Average for each Track, Gender, and Hometown category and presents the results using three bar charts in a single figure. The interpretation identifies the categories with the highest observed sample means without treating the differences as evidence of causation.

---

**ECE 2112 — Advanced Computer Programming and Algorithms**

**Experiment 4: Data Wrangling and Data Visualization**

**Programming Assignment 4 — LUCAS**
