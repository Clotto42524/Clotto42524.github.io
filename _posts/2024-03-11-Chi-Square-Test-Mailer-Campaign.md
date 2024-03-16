---
layout: post
title: Chi Square Test for Mailer Campaign
image: "/posts/ab-testing-title-img.png"
tags: [Python, Chi Square]
---

This is a small project using the Chi Square Test to determine the effectiveness of two Mailer campaigns for a fictional grocery chain

---

First we start by importing our various necessary packages

```python
import pandas as pd
from scipy.stats import chi2_contingency, chi2
```

We'll then import our data into a DataFrame, which is stored as an excel file in the sheet named "campaign_data"

```python
campaign_data = pd.read_excel("grocery_database.xlsx", sheet_name = "campaign_data")
```

Taking a quick look at our data, we can tell using **list** that our data has various columns. With **value_counts** we can also see that the column *"mailer_type"* has three possible values: "Mailer1", "Mailer2", and "Control"

```python
list(campaign_data)
>>>
['customer_id', 'campaign_name', 'campaign_date', 'mailer_type', 'signup_flag']

campaign_data["mailer_type"].value_counts()
>>>
mailer_type
Mailer1    375
Mailer2    336
Control    159
Name: count, dtype: int64
```

Since we're only interested in comparing Mailer1 and Mailer2 in this instance, we will overwrite **campaign_data** to drop rows where *"mailer_type"* is "Control"

```python
campaign_data = campaign_data.loc[campaign_data["mailer_type"] != "Control"]
```

We want to create a matrix which contains the data of the outcomes (mailer type 1 or 2 and signup yes or no). We use **crosstab** and pass our columns of interest *"mailer_type"* and *"signup_flag"* and save that as **observed_values**. We want our data to be an array, because this is what we need to use **chi2_contingency**, so we use the **values** method to give us that.
Then we can use the output of **observed_values** to manually calculate the signup rates of each mailer variation.

```python
observed_values = pd.crosstab(campaign_data["mailer_type"], campaign_data["signup_flag"]).values
>>>
array([[140,  19],
       [252, 123],
       [209, 127]])
mailer1_signup_rate = 123 / (252 + 123)
mailer2_signup_rate = 127 / (209 + 127)
```

In preparation for running our test, we can create our null hypothesis, our alternative hypothesis, and set our acceptance criteria of .05.
While this isn't strictly needed for our test, it'll be nice to have it to state the conclusion at the end.

```python
null_hypothesis = "There is no relationship between mailer type and signup rate. They are independent."
alt_hypothesis = "There is a relationship between mailer type and signup rate. They are not independent."
acceptance_criteria = .05
```

Now that we have all our data ready, we can run our test.
Using **chi2_contingency** and passing in our **observed_values**, we're able to calculate the chi square statistic, p-value, degrees of freedom, and expected values for our scenario.
We can also get the critical value by using **chi2.ppf** and passing in our acceptance criteria and degrees of freedom.

```python
chi2_statistic, p_value, dof, expected_values = chi2_contingency(observed_values, correction = False)
critical_value = chi2.ppf(1 - acceptance_criteria, dof)
```

After getting the output of our test, we can prepare two ways of presenting our conclusions: either using the chi square statistic and critical value, or p-value and acceptance criteria.

```python
# Chi Square Statistic
if chi2_statistic >= critical_value:
    print(f"As our chi-square statistic of {chi2_statistic} is larger than the critical value of {critical_value}, we reject the null hypothesis and conclude that {alt_hypothesis}")
else:
    print(f"As our chi-square statistic of {chi2_statistic} is smaller than the critical value of {critical_value}, we retain the null hypothesis and conclude that {null_hypothesis}")

# p-value
if p_value <= acceptance_criteria:
    print(f"As our p-value of {p_value} is smaller than our acceptance criteria of {acceptance_criteria}, we reject the null hypothesis and conclude that {alt_hypothesis}")
else:
    print(f"As our p-value of {p_value} is larger than our acceptance criteria of {acceptance_criteria}, we retain the null hypothesis and conclude that {null_hypothesis}")
 ```

In this particular case, running our conditional statement on the output values from our test gives us

```python
# Chi Square Statistic
As our chi-square statistic of 1.9414468614812481 is smaller than the critical value of 3.841458820694124, we retain the null hypothesis and conclude that There is no relationship between mailer type and signup rate. They are independent.

# p-value
As our p-value of 0.16351152223398197 is larger than our acceptance criteria of 0.05, we retain the null hypothesis and conclude that There is no relationship between mailer type and signup rate. They are independent.
```
    
