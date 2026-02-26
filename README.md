# Fitting Poisson  distribution
### Developed by : **AMSAVARADHAN M**
### Reg No : **212225230014**
# Aim : 

To fit poisson distribution for the arrival of objects per minute from the feeder

# Software required :  

Python and Visual component tool

# Theory:

The Poisson distribution is the discrete probability distribution of the number of events occurring in a given time period, given the average number of times the event occurs over that time period.

![image](https://user-images.githubusercontent.com/104613195/166248326-fd042076-8b0b-40c4-8b11-1d8e8fcb74db.png)

 Conditions for Poisson Distribution:

1. An event can occur any number of times during a time period.
2. Events occur independently. I
3. The rate of occurrence is constant.
4. The probability of an event occurring is proportional to the length of the time period. 
 
# Procedure :

![image](https://user-images.githubusercontent.com/104613195/166251988-d0c53205-6080-4f7b-ae4c-398178586637.png)

# Experiment :

![image](https://user-images.githubusercontent.com/103921593/230282876-f4a5afbf-cac1-4648-a1b0-c78840638a8e.png)

# Program :
```python
import numpy as np
import math
from scipy.stats import chi2

# Step 1: Input data
data = list(map(int, input("Enter space-separated frequency values: ").split()))

# Step 2: Basic statistics
N = len(data)
max_val = max(data)

# Step 3: Frequency distribution (Observed)
observed_freq = [data.count(i) for i in range(max_val + 1)]
X = list(range(max_val + 1))  # X values
total_freq = sum(observed_freq)

# Step 4: Probability for each X based on observed data
prob_obs = [f / total_freq for f in observed_freq]

# Step 5: Compute mean (λ for Poisson) using dot product
mean = np.inner(X, prob_obs)

# Step 6: Compute expected probabilities & expected frequencies under Poisson
expected_probs = []
expected_freq = []
chi_sq_components = []

print("\nX\tP(X=x)\tObs.Freq\tExp.Freq\tChi^2")
print("-" * 50)

for x in X:
    # Poisson probability formula: P(X = x) = e^(-λ) * λ^x / x!
    poisson_prob = math.exp(-mean) * (mean ** x) / math.factorial(x)
    exp_freq = poisson_prob * total_freq
    chi_sq = ((observed_freq[x] - exp_freq) ** 2) / exp_freq if exp_freq > 0 else 0

    expected_probs.append(poisson_prob)
    expected_freq.append(exp_freq)
    chi_sq_components.append(chi_sq)

    print(f"{x}\t{poisson_prob:.4f}\t{observed_freq[x]:>8}\t{exp_freq:>9.2f}\t{chi_sq:>7.2f}")

# Step 7: Calculate total Chi-square value
calculated_chi_sq = sum(chi_sq_components)
degrees_of_freedom = max_val  # df = k - 1 - 1 (mean estimated → one parameter)
table_chi_sq = chi2.ppf(1 - 0.01, df=degrees_of_freedom)

# Step 8: Hypothesis Testing
print("-" * 50)
print(f"Calculated Chi-square value: {calculated_chi_sq:.4f}")
print(f"Critical Chi-square value (1% LOS, df={degrees_of_freedom}): {table_chi_sq:.4f}")

if calculated_chi_sq < table_chi_sq:
    print("✅ The data *fits* the Poisson distribution at 1% level of significance.")
else:
    print("❌ The data *does not fit* the Poisson distribution at 1% level of significance.")
```
 

# Output : 

```
X	P(X=x)	Obs.Freq	Exp.Freq	Chi^2
--------------------------------------------------
0	0.0213	       1	     0.43	   0.78
1	0.0819	       2	     1.64	   0.08
2	0.1577	       2	     3.15	   0.42
3	0.2024	       5	     4.05	   0.22
4	0.1948	       1	     3.90	   2.15
5	0.1500	       5	     3.00	   1.33
6	0.0962	       1	     1.92	   0.44
7	0.0529	       3	     1.06	   3.56
--------------------------------------------------
Calculated Chi-square value: 8.9913  
Critical Chi-square value (1% LOS, df=7): 18.4753  
The data *fits* the Poisson distribution at 1% level of significance.
```

# Results

The Poisson distribution is fitted for the objects arrived from feeder per minute and the data is tested using Chi-square test. 
 
