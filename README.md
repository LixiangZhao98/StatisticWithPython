# StatisticWithPython

## Read CSV

```python
import os
path_directory= r'/Study Data'  
for subdir, dirs, files in os.walk(path_directory):   #read all the .csv files one by one under path_directory
  for file in files:
    if file.endswith(".csv"):
      path_file = os.path.join(subdir,file)
      with open(path_file, encoding='utf-8')as f:
        all_lines= f.readlines()


        #Examples: Get elements
        taskID=(int)(all_lines[2].split(",")[3]) #Get the task id (int) on row 2, column 3
        datasetID=(int)(all_lines[-2].split(",")[4])  #Get the dataset id (int) on row n-2, column 4 (n is the total row number)
```
## Mean and Confidence Interval
```python
import numpy as np
import scipy.stats

def mean_confidence_interval(data, confidence=0.95):
    a = 1.0 * np.array(data)
    n = len(a)
    m, se = np.mean(a), scipy.stats.sem(a)
    h = se * scipy.stats.t.ppf((1 + confidence) / 2., n-1)
    return m, m-h, m+h
```

## P-Value from a t-Score
#### P-value for a One-sample T-test
```python

#Let’s consider a scenario where we have a sample of exam scores from a group of students, and we want to test whether the average exam score is significantly different from a population mean. The average exam score for a population of students is known to be 75 in a sample of 250 students.

import numpy as np
from scipy.stats import t


def one_sample_t_test(sample, population_mean, alpha=0.05, tail="two"):
	# Step 1: Calculate T-score
	sample_mean = np.mean(sample)
	sample_std = np.std(sample, ddof=1)
	sample_size = len(sample)

	t_score = (sample_mean - population_mean) / \
		(sample_std / np.sqrt(sample_size))

	# Step 2: Determine degrees of freedom
	df = sample_size - 1

	# Step 3: Identify the appropriate t-distribution
	# No need to explicitly specify degrees of freedom for one-sample t-test in scipy.stats.t

	# Step 4: Find the p-value
	if tail == "two":
		p_value = t.sf(np.abs(t_score), df) * 2 # for two-tailed test
	elif tail == "left":
		p_value = t.sf(t_score, df) # for left-tailed test
	elif tail == "right":
		p_value = t.sf(-t_score, df) # for right-tailed test
	else:
		raise ValueError(
			"Invalid tail argument. Use 'two', 'left', or 'right'.")

	# Step 5: Interpret the p-value
	print("P-value:", p_value)

	if p_value < alpha:
		print(
			"Reject the null hypothesis. There is a statistically significant difference.")
	else:
		print("Fail to reject the null hypothesis. There is no statistically significant difference.")


# Let's generate a sample for experiment
np.random.seed(42)
# Generating a sample
sample_data = np.random.normal(loc=77, scale=10, size=250)
population_mean = 75

# Example for a two-tailed test
one_sample_t_test(sample_data, population_mean, tail="two")
```
