Hello everyone,

This project involved analyzing insurance claim amount data. After exploring the data, I fit several distributions. From an insurance perspective, it's crucial to have a **good fit at the tails** of the distribution.

Initially, I analyzed the data using a **scatter plot** but found it uninformative. Applying a log transformation revealed an outlier.

To assess central tendencies and interquartile range, I plotted a **box plot**. This confirmed the presence of an outlier.

I then fit various distributions to the data and discovered that the generalized Pareto distribution provided the best overall fit, based on **AIC, BIC, KS Statistics, KS P-Value, KL Divergence, Sum of Squared error, PP and QQ** plot analysis.

To capture the different behaviors at the tails, I divided the data into low-value and high-value claims. I found that the **beta distribution best fits low-value claims**, while the **generalized Pareto distribution is a good fit for high-value claims**.

Thanks for reading!

- Monu Kumar Pathak






