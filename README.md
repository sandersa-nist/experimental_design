# experimental_design
Tools to create and manage design of experiments (DOE) or statistical design of experiments.
# Installation
```shell
pip install git+https://github.com/sandersa-nist/experimental_design.git
```
# Use Case
When exploring an experimental space one or more parameters it is useful to use an [experimental design](https://en.wikipedia.org/wiki/Design_of_experiments). A simple experimental design would be one in which you vary one or more factors by setting them at one or more different levels and measure a single response. For example, to see if temperature and/or humidity affect a particular current reading you could pick a hot, cold, dry and humid condition. A systematic approach would be to use a [2x2 fully factorial design](https://en.wikipedia.org/wiki/Factorial_experiment), in which every combination of temperature and humidity are tried in some order [(hot,humid),(hot,dry),(cold,humid), (cold,dry)]. This package allows one to create this type of table for any number of factors with any number of settings. In addition, it provides the benefit of having repeatable randomization and adding in a default (or control) value for factors. The above example is created by
```python
import experimental_design 
import pandas as pd
design = {}
design["temperature"] = {"-":"cold","+":"hot"}
design["humidity"] = {"-":"dry","+":"humid"}
test_conditions = experimental_design.fully_factorial(design)
test_conditions_df = pd.DataFrame(test_conditions)
test_conditions_df
```
Now you can imagine that when you have 6000 factors with different number of levels this can be a daunting thing to do by hand. Additionally, randomizing the order, which is best practice, becomes quite cumbersome. And finally, making sure the experiment is working as planned by going to a default or control state periodically adds even more complexity.
With this package making a design of 6,000 factors with a high and low state that are randomized and default values is just:
```python
import experimental_design 
import pandas as pd
n_factors = 3
design = {f"F{i}":{"-":-1,"+":1} for i in range(1,n_factors+1)}
default = {f"F{i}":{0:"Default"} for i in range(1,n_factors+1)}
default_design = experimental_design.fully_factorial_default(design_dictionary=design,default_state=default,
                                                              randomized= True,random_seed= 42,run_values="values")
dd_df = pd.DataFrame(default_design)
dd_df
```
# Workflow
1. create a dictionary that has the factor names as keys and a dictionary of settings with names or level indicators as keys and specific test points as values 
```python
design = {"temperature":{"-":"cold","+":"hot"},"humidity":{"-":"dry","+":"humid"}}
```
2. Decide if you want or need a default (control test) and how frequently it would be tested
```python
default = {"temperature":{0:"cold},"humidity:{0,"dry"}}  
```
3.  if you
# [Jupyter Example](./examples/experimental_designs_example.ipynb)