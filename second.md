```python
from aguaclara.core.units import unit_registry as u
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from scipy import stats
#The data file path is the raw data url on github. Happily python can read directly from a web page.
data_file_path = "https://github.com/IanStarnes/First-Repository/second.md"

#Now we create a pandas dataframe with the data in the file
df = pd.read_csv('/Users/Ian/Documents/Heights_and_weights.csv')
  #if you want to see what is in the dataframe you can print it!
print(df)
# The column headers can be access by using the list command
list(df)
columns = df.columns
print(columns)
  #Below are three equally fine methods of extracting a column of data from the pandas dataframe.

  # 1) We can select a column by using the column header. Here we use the column header by selecting one array element from the list command.
x = df[list(df)[0]].values * u.cm
x
  # 2) We can use the loc command to select all of the rows (: command) and the column with the label given by list(df)[0].
x = df.loc[:, list(df)[0]].values * u.cm
x
  # 3) We can use the iloc command and select all of the rows in column 0.
x = df.iloc[:,0].values * u.cm
x
  #The iloc method is simple and efficient, so I'll use that to get the y values.
y = df.iloc[:,1].values * u.kg
y
  # We will use the stats package to do the linear regression.
  # It is important to note that the units are stripped from the x and y arrays when processed by the stats package.
slope, intercept, r_value, p_value, std_err = stats.linregress(x,y)

  #We can add the units to intercept by giving it the same units as the y values.
intercept = intercept * y.units
  # Note that slope is dimensionless for this case, but not in general!
  # For the general case we can attach the correct units to slope.
slope = slope * y.units/x.units

  # Now create a figure and plot the data and the line from the linear regression.
fig, ax = plt.subplots()
  # plot the data as red circles
ax.plot(x, y, 'ro', )

  #plot the linear regression as a black line
ax.plot(x, slope * x + intercept, 'k-', )

  # Add axis labels using the column labels from the dataframe
ax.set(xlabel=list(df)[0])
ax.set(ylabel=list(df)[1])
ax.legend(['Measured', 'Linear regression'])
ax.grid(True)

  # Here I save the file to my local harddrive. You will need to change this to work on your computer.
  # We don't need the file type (png) here.
plt.savefig('/Users/Ian/Documents/linear.jpeg')
plt.show()
![photo](/Users/Ian/Documents/linear.jpeg)
The formula $$ slope * height + intercept = weight $$
