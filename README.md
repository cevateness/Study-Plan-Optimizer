# Study Plan Optimization with Gurobi

This repository contains a Python implementation of a study plan optimization model using the Gurobi optimizer. The model aims to create an efficient study schedule by minimizing both total study hours and workload deviations across days, while respecting constraints like due dates, earliest start dates, and available study time.

## Problem Description

The goal is to develop a study plan for a PhD semester where there are various activities (projects, exams, presentations, etc.) spread over a number of weeks. Each activity requires a certain number of hours to be completed, and some activities can only start after a specific week. 

We solve this problem by optimizing the allocation of study hours to activities while balancing the workload across the week.

## Mathematical Model

### Decision Variables
- Let `x_{i, w, d}` represent the number of study hours allocated to activity `i` during week `w` on day `d`.

### Objective Function
We have a multi-objective function that balances two goals:
1. Minimize the total study hours.
2. Minimize the deviation of workload across days.

The objective function is a weighted sum:

Where:
- `w_total_hours` is the weight assigned to minimizing total hours.
- `w_workload` is the weight assigned to minimizing workload deviation.

### Constraints

#### 1. Workload Ratio
For each day `d` in week `w`, the study workload ratio is constrained by the total number of study hours divided by the available hours on that day:


#### 2. Required Study Hours
Each activity `i` must receive the required number of study hours over the semester:


#### 3. Earliest Start Date
Activities cannot start before their defined earliest start week. For each activity `i`, if the current week `w` is less than the earliest start week `w_start^i`, then no study time is allocated:


#### 4. Daily Study Time Limit
The total study hours on any given day must not exceed the available time for that day:


#### 5. Due Date
Each activity must be completed by its due week `w_due^i`. Thus, study time after the due date is not allowed:


## Usage

### Prerequisites
- Python 3.x
- Gurobi Optimizer
- Pandas

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/study-plan-optimization.git
   cd study-plan-optimization

### Input
You need to provide the input data in an Excel file named `study_plan.xlsx`. This file contains:

Activities sheet with the following columns:

- Activity: Name of the activity (e.g., "Project1", "Midterm").
- Required Hours: The total number of hours required for the activity.
- Due Week: The week number when the activity is due.
- Due Day: The specific day of the week when the activity is due.
- Earliest Start Week: The earliest week when the activity can start.
- Week Availability sheet that contains available study hours for each day of the week, for each week.

### Output
The script generates an optimized study plan and exports it to an Excel file `optimized_study_plan.xlsx`. The output contains:

- Week number
- Day of the week
- Activity to study
- Number of hours to study for that activity
