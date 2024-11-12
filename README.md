# StatisticWithPython

## Read CSV

```python
path_directory= r'/Study Data'  
for subdir, dirs, files in os.walk(path_directory):   #read all the .csv files one by one under path_directory
  for file in files:
    if file.endswith(".csv"):
      path_file = os.path.join(subdir,file)
      with open(path_file, encoding='utf-8')as f:
        all_lines= f.readlines()
```
## Confidence Interval
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
