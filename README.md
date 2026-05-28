# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
## Date: 28/05/2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
1)Import necessary libraries (NumPy, Matplotlib)

2)Load the dataset

3)Calculate the linear trend values using least square method

4)Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
LINEAR TREND ESTIMATION & POLYNOMIAL TREND ESTIMATION
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
data = pd.read_csv(
    'gold_price.csv',
    parse_dates=['Date'],      
    index_col='Date'
)
print(data.head())
resampled_data = data['GLD'].resample('YE').sum().to_frame()
print(resampled_data.head())
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Date': 'Year'}, inplace=True)
print(resampled_data.head())
years = resampled_data['Year'].tolist()
values = resampled_data['GLD'].tolist()
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, values)]
n = len(years)
b = (n * sum(xy) - sum(values) * sum(X)) / \
    (n * sum(x2) - (sum(X) ** 2))
a = (sum(values) - b * sum(X)) / n
linear_trend = [a + b * X[i] for i in range(n)]
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, values)]
coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]
Y = [sum(values), sum(xy), sum(x2y)]
A = np.array(coeff)
B = np.array(Y)
solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution
poly_trend = [
    a_poly + b_poly * X[i] + c_poly * (X[i] ** 2)
    for i in range(n)
]
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"\nPolynomial Trend: y = {a_poly:.2f} + "
      f"{b_poly:.2f}x + {c_poly:.2f}x²")
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend
resampled_data.set_index('Year', inplace=True)
plt.figure(figsize=(10, 6))
plt.plot(
    resampled_data.index,
    resampled_data['GLD'],
    color='blue',
    marker='o',
    label='Original Data'
)
plt.plot(
    resampled_data.index,
    resampled_data['Linear Trend'],
    color='black',
    linestyle='--',
    label='Linear Trend'
)
plt.plot(
    resampled_data.index,
    resampled_data['Polynomial Trend'],
    color='red',
    marker='o',
    label='Polynomial Trend'
)
plt.title('Trend Estimation')
plt.xlabel('Year')
plt.ylabel('GLD')
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT
LINEAR TREND ESTIMATION & POLYNOMIAL TREND ESTIMATION

<img width="723" height="445" alt="Screenshot 2026-05-28 152856" src="https://github.com/user-attachments/assets/dba78e66-eef2-4342-be6f-2c36f2ab045d" />

<img width="1143" height="752" alt="Screenshot 2026-05-28 152933" src="https://github.com/user-attachments/assets/cd20c258-46e1-4708-913d-7d1d356de453" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
