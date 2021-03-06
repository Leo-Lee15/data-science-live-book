

# Considerations involving time


## What is this about?

<img src="Heraklit.jpeg" width="200px" alt="Heraclitus doing machine learning">

> Everything changes and nothing stands still. - Heraclitus, (535 – 475 BC), pre-Socratic Greek philosopher.

So do variables. 

As time goes by, variables may change their values, making the time-analysis crucial in order to create a predictive model. Avoiding the use of **effects** as **causes**.

<br>

**What are we going to review in this chapter?**

* Concepts of filtering information before the event to predict.
* How to analyze and prepare variables that increase -or decrease- their value to infinity (and beyond).

<br>

### Don't use information from the future

<img src="back_to_the_future.png" width='250px' alt="Data Preparation - Don't use variables from the future"> 

Using a variable which contains information **after** the event it's being predicted, is a common mistake when starting a new predictive model project, like playing the lottery today using the tomorrow's newspaper.

Imagine we need to build a predictive model to know what users are likely to adquire full subscription in a web application, and this software has a ficticious feature called it `feature_A`:



```
##    user_id feature_A full_subscription
## 1        1       yes               yes
## 2        2       yes               yes
## 3        3       yes               yes
## 4        4        no                no
## 5        5       yes               yes
## 6        6        no                no
## 7        7        no                no
## 8        8        no                no
## 9        9        no                no
## 10      10        no                no
```


We build the predictive model, we got a perfect accuracy, and an inspection throws the following: _"100% of users that have a full subscription, uses Feature A"_. Some predictive algorithms report variable importance; thus `feature_A` will be at the top.

**The problem is:** `feature_A` is only available **after the user goes for full subscription**. Therefore it cannot be used.

**The key message is**: Don't trust in perfect variables, nor perfect models. 

<br>

### Play fair with data, let it develop their behavior

Like in nature, things have a minimum and maximum time to start showing certain behavior. This time oscillates from 0 to infinity. In practice it's recommended to study what is the best period to analyze, in other words, we may exclude all the behavior before and after this observation period. To establish ranges in variables, it's not straight forward since it may be kind of subjective.

Imagine we've got a **numerical variable** which increases as time moves. We may need to define a **observation time window** to filter the data and feed the predictive model. 

* Setting the **minimum** time: How much time is it need to start seeing the behavior? 
* Setting the **maximum** time: How much time is it required to see the end of the behavior? 

The easiest solution is: setting minimum since begin and the maximum as the whole history.


**Case study:** 

Two people, `Ouro` and `Borus`, are users of a web application which has a certain functionality called `feature_A`, and we need to build a predictive model which forecast based on `feature_A` usage  -measured in 'clicks" if the person is going to acquire `full_subscription`.

The current data says: Borus has `full_subscription`, while Ouro doesn't. 

<img src="figure/data_preparation_time_variable-1.png" title="plot of chunk data_preparation_time_variable" alt="plot of chunk data_preparation_time_variable" width="600px" />


User `Borus` starts using `feature_A` from day 3, and after 5 days she has more use -15 vs. 12 clicks- on this feature than `Ouro` who started using it from day 0. 

If `Borus` acquire `full subscription` and `Ouro` doesn't, _what would the model will learn?_ 

When modeling with full history -`days_since_signup = all`-, the higher the `days_since_signup` the higher the likelihood, since `Borus` has the highest number.

However, if we keep only with the user history corresponding to their first 5 days since signup, the conclusion is the opposite. 

_Why to keep first 5 days of history?_

The behavior in this kick-off period may be more relevant -regarding prediction accuracy- than analyzing the whole history. It depends on each case as we said before. 


<br> 

### Fighting the infinite

<img src="infinity.png" alt="Cutting the inifinity" width="150px">

The number of examples on this topic is vast. Let's keep the essence of this chapter in **how data changes across time**. Sometimes it's straightforward, as a variable reaching its minimum (or maximum) after a fixed time length. This case is easily achievable.

On the other hand, it requires the human being to fight the infinite.

Consider the following example. _How much hours are needed to reach the 0 value?_

How about 100 hours?

<img src="figure/data_preparation_time_variable_1-1.png" title="plot of chunk data_preparation_time_variable_1" alt="plot of chunk data_preparation_time_variable_1" width="600px" />

Hmm let's check the minimum value.


```
## [1] "Min value after 100 hours: 0.22"
```

It's close to zero, but _what if we wait 1000 hours?_

<img src="figure/data_preparation_time_variable_3-1.png" title="plot of chunk data_preparation_time_variable_3" alt="plot of chunk data_preparation_time_variable_3" width="600px" />

```
## [1] "Min value after 1,000 hours: 0.14"
```

Hurra! We are approaching it! From `0.21` to `0.14` But what if we wait 10 times more? (10,000 hours)

<img src="figure/data_preparation_time_variable_4-1.png" title="plot of chunk data_preparation_time_variable_4" alt="plot of chunk data_preparation_time_variable_4" width="600px" />

```
## [1] "Min value after 10,000 hours: 0.11"
```

_Still no zero! How much time do I need?!_ 😱


As you may notice, it will probably reach the zero value in the infinity... We are in the presence of an <a href="https://en.wikipedia.org/wiki/Asymptote" target="blank">Asymptote</a>.

_What do we have to do?_ Time to go to the next section.

<br>

### Befriend the infinite

The last example can be seen in the analysis of customer age in the company. _This value can be infinite._

When selecting customers that have been with the company for at least 3 months, and a maximum of 12.

For example, if the project goal is to predict a binary outcome, like `buy`/`don't buy`, one useful analysis is to calculate the `buy` rate according to age's user. Coming to conclusions like: _In average, a customer needs around 6 months to buy this product_.

This answer may come by the joint work of the data scientist and the domain expert. 

In this case, a zero can be considered the same as the value which has the 95% of the population. In statistics terms it is the `0.95` **percentile**. This book extensively covers this topic in <a href="http://livebook.datascienceheroes.com/exploratory_data_analysis/annex_1_profiling_percentiles.html" target="blank">Annex 1: The magic of percentiles</a>. This is a key topic in exploratory data analysis.

A related case is **dealing with outliers**, when we can apply this cutting percentile criteria, as we saw in <a href="http://livebook.datascienceheroes.com/data_preparation/outliers_treatment.html" target="blank">treating outliers chapter</a>.


<br>

### Examples in other areas

It's really common to find this kind of variables in many data sets or projects.

**In medicine**, the <a href="https://en.wikipedia.org/wiki/Survival_analysis" target="blank">survival analysis projects</a>, the doctors usually define a threshold of, for example, 3 years to consider that one patient _survive_ the treatment.

**In marketing projects**, if a user decreases its activity under a certain threshold, let's say:
* 10-clicks in company's web page during last month
* Not opening an email after 1-week
* If she (or he) don't buy after 30-days

Can be defined as a churned or lost opportunity customer.

**In customer support**, an issue can be marked as solved after the person doesn't complain during 1-week.

**In brain signal analysis**, if these signals come from the visual cortex in a project that, for example, we need to predict what type of image the patient is looking at, then the first 40ms of values are useless because it is time the brain need to start processing the signal.

But this also happens in _"real life"_, like the case when we write a data science book suitable for all ages, how much time is it required to end it? An infinite amount? Probably not 😄.


<br>

### Final thoughts

Defining a time frame to create a training and validation set **is not a free-lunch** when the data is dynamic, as well as deciding how to handle variables that change over time. That's why the **Exploratory Data Analysis** is important in order to get in touch with the data we're analyzing. 

Topics are inter-connected. Now, it's the time to mention the relationship of this chapter with the assessing model performance (<a href="http://livebook.datascienceheroes.com/model_performance/out_of_time_validation.html" target="blank">out-of-time validation</a>). When we predict events in the future, we have to analyze how time is it needed for the target variable to change.

The key concept here: **how to handle time in predictive modeling**. It's a good opportunity to ask: _How would it be possible to address this time issues with automatic systems?_ 

Human knowledge is crucial in these contexts to define thresholds based on experience, intuition and some calculations.

<br>


