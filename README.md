 # **Summary**

**Introduction** <br>
The data comes from Purdue University [here](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks) and a data dictionary is available at this [article](https://www.sciencedirect.com/science/article/pii/S2352340918307182) under Table 1. Variable descriptions.

This dataset contains information about power outages in the US from 2000 to 2016. Each outage contains regional information like the climate region and state that each outage was in. It also has information about the causes of the outages and the extent of the damage they caused. There is also other information about the economics and power consumption of the surrounding area. The question I want to investigate is whether there are certain regions in the US that experience major power outages more frequently than others. By combining the geographical data with the effects of each outage I will explore the distribution of major power outages across the US.

**Cleaning and Exploration** <br>
The first step in cleaning the data was to load the correct rows and columns. I used load excel function in pandas with the correct parameters for rows and columns to read the correct data. The next step in cleaning was combining the outage start date and time as well as the end date and time using datetime and string manipulation.

After that I started to explore the data. I found the columns of customers affected and power lost to best represent the effect of the outage. Customers affected had a right skewed distribution with a mean of 143,456 customers affected per outage. Power lost had a right skewed distribution with a mean of 536 MW lost per outage. The scatterplot between these variables shows that there is a positive association between power lost and customers affected which makes sense because losing more power affects more customers.

After that I wanted to determine which outages were considered major by the definition of affecting at least 50,000 customers and losing at least 300 MW. About 14% of the outages in the dataset ended up being major outages. When comparing the proportion of outages in each region which were major, the southeast region seemed to have a higher proportion than most with 38.8%, which is over 20% more than the next highest. In contrast, there was a much smaller difference in proportions for climates which had differences less than 3%. Since the difference is proportions of major outages was different for each region, I wanted to test whether the distribution of major outages in the US was significantly different from random chance.

**Assessment of Missingness** <br>
Since I wanted to analyze the effects of power outages, I decided to analyze the missingness of demand loss because it directly shows the severity of the outage. The missingness of this column usually happens for severe weather or attacks in which case the power station likely turns off the power themselves rather than it failing. In this case it is possible that the power loss is not automatically recorded whereas it would be in other cases. In addition, the number of people affected varies a lot when demand loss is missing so it is likely not missing in a certain range of values. Therefore the missingness is not NMAR because other factors have the most influence.

I set up the first missingness test by first plotting a histogram of customers affected for when demand loss is missing and not missing. Since they have close to the same mean, the KS test statistic is needed to get better differences between samples. I set an alpha level of 0.05 and ran the KS test which gave a p-value of 2.93-40. Since this is less than alpha I reject the null hypothesis. There is sufficient evidence to show that the distribution of people affected is different for missing and not missing values of demand loss. Because of this, it is possible that some outages with missing data could have been considered major when they actually weren't leading to a misrepresentation of the distribution of major outages across America.

I set up the second missingness test for Industrial Sector Sales the same way as the first and used the KS test again. I set an alpha level of 0.05 and ran the KS test which gave a p-value of 0.18. Since this is more than alpha I don't reject the null hypothesis. There is not sufficient evidence to show that the distribution of Industrial Sector sales is different for missing and not missing values of demand loss. Because of this, states with different amounts of industrializations won't have this affect major outages which improves the accuracy of the hypothesis test.

**Hypothesis Test** <br>
I ran a hypothesis test to find out if major outages are uniformly distributed across regions in the United States.

Null Hypothesis: Major outages are uniformly distributed across regions
Alternative Hypothesis: Major outages are not uniformly distributed across regions

Since the region is a categorical variable, the best test statistic is the TVD of the distribution compared to a uniform distribution. I ran a hypothesis test with 1,000,000 randomly generated distributions of outages with 0.05 as the alpha level. The test resulted in a p-value of 0 which is less than alpha. Therefore, I reject the null hypothesis because there is sufficient evidence that major outages are not uniformly distributed across regions.

This result shows that one way to improve outages in the US is to only focus on certain regions first because they have a higher proportion of major outages in general. However, the shortcoming of this approach is that it doesn't determine the reason for the outages so it doesn't show exactly how to prevent outages. A way to improve this test is to run a hypothesis test of different causes of outages and see if there is a significant result to determine exactly what to improve.
