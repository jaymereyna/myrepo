Project 2 - Chick Weight by Feed Type
================
Jayme R. Reyna
4/18/2021

## Introduction

My report for Project 2 is entitled Chick Weight by Feed Type. I plan to use the same combined dataset that I created in Project 1 to analyze and model. The two original datasets are called 'ChickWeight' and 'chickwts', and were acquired through R using the following code:

``` r
#lists all datasets in R
data(package = .packages(all.available = TRUE))
```

The dataset entitled 'ChickWeight' contains data from an experiment conducted to measure the effect of diet on early growth of chicks, and contains a total of 578 observations across 4 variables, including 'weight', 'Time', 'Chick', and 'Diet'. The variable 'weight' is a numeric vector giving the body weight of the chick in grams, 'Time' is a numeric vector giving the number of days since birth when the measurement was made, 'Chick' is an ordered factor with levels giving a unique identifier for the chick, and 'Diet' is a factor with levels 1-4 indicating which experimental diet the chick received. The dataset entitled 'chickwts' contains data from an experiment conducted to measure and compare the effectiveness of various feed supplements on the growth rate of chickens, and contains a total of 71 observations across 2 variables, including 'weight' and 'feed'. The variable 'weight' is a numeric variable giving the chick weight in grams and 'feed' is a factor giving the feed type. The two datasets share the variable 'weight', and are similar in that they both concern chicks and their growth rate measured by a number of variables. These datasets were particularly interesting to me because I eventually plan on studying animal science more extensively. I merged these datasets using the following code:

``` r
#merges two datasets
Chicks <- merge(ChickWeight, chickwts, by = "weight")
```

After combining the two datasets, the resulting dataset entitled 'Chicks' contains 53 observations across 5 variables.

Although the datasets are relatively simple, I expect to see associations among them, including a relationship between 'weight' and 'feed' type, as well as a relationship between 'weight' and 'Time'.

Because the data obtained came directly from R, both of the original datasets are tidy and resulted in a combined tidy dataset. They each contain one row per observation and one column for every variable. Therefore, there is no need to tidy the data any further.

## Exploratory Data Analysis

I will use dplyr functions to manipulate and explore my newly created dataset. First, I will use the filter() function to filter the data based on the 4 different diets and observe and compare the chick weights recorded for each diet.

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
#filters the data by chicks on diet 1
Chicks %>% filter(Diet == "1")
```

    ##    weight Time Chick Diet      feed
    ## 1     108   14     4    1 horsebean
    ## 2     124   10     6    1 horsebean
    ## 3     124   21    10    1 horsebean
    ## 4     136   16     4    1 horsebean
    ## 5     141   12     5    1   linseed
    ## 6     141   12     6    1   linseed
    ## 7     148   14     6    1   linseed
    ## 8     160   18     6    1 horsebean
    ## 9     160   20     6    1 horsebean
    ## 10    160   20     4    1 horsebean
    ## 11    168   12    11    1 horsebean
    ## 12    171   18     1    1   soybean
    ## 13    181   20    11    1   linseed
    ## 14    199   20     1    1   soybean
    ## 15    199   18     5    1   soybean
    ## 16    227   16    14    1 horsebean
    ## 17    248   18    14    1   soybean
    ## 18    248   18    14    1   soybean
    ## 19    250   18     7    1   soybean

``` r
#filters the data by chicks on diet 2
Chicks %>% filter(Diet == "2")
```

    ##    weight Time Chick Diet      feed
    ## 1     108   12    22    2 horsebean
    ## 2     124   10    25    2 horsebean
    ## 3     136   12    26    2 horsebean
    ## 4     143   16    30    2 horsebean
    ## 5     148   18    22    2   linseed
    ## 6     169   16    26    2   linseed
    ## 7     217   12    21    2 horsebean
    ## 8     230   18    29    2   soybean
    ## 9     309   21    29    2   linseed
    ## 10    318   20    21    2 sunflower
    ## 11    318   20    21    2    casein

``` r
#filters the data by chicks on diet 3
Chicks %>% filter(Diet == "3")
```

    ##    weight Time Chick Diet      feed
    ## 1     158   10    35    3   soybean
    ## 2     169   20    37    3   linseed
    ## 3     179   14    32    3 horsebean
    ## 4     227   18    36    3 horsebean
    ## 5     250   20    39    3   soybean
    ## 6     263   18    32    3  meatmeal
    ## 7     295   20    40    3 sunflower
    ## 8     327   20    34    3   soybean
    ## 9     332   18    35    3    casein
    ## 10    341   21    34    3 sunflower

``` r
#filters the data by chicks on diet 4
Chicks %>% filter(Diet == "4")
```

    ##    weight Time Chick Diet      feed
    ## 1     108    8    49    4 horsebean
    ## 2     124   10    41    4 horsebean
    ## 3     141   14    45    4   linseed
    ## 4     148   12    47    4   linseed
    ## 5     153   14    41    4  meatmeal
    ## 6     160   12    42    4 horsebean
    ## 7     168   16    47    4 horsebean
    ## 8     199   20    43    4   soybean
    ## 9     199   20    41    4   soybean
    ## 10    203   18    49    4   linseed
    ## 11    222   16    48    4    casein
    ## 12    303   20    48    4  meatmeal
    ## 13    322   21    48    4 sunflower

Based on the results of these filter() functions, it does not seem that there is an association between the chicks' diets and the chicks' weights. There is a distribution of weights in each diet group that is versatile and does not show a pattern based on diet. To further visualize this and confirm that there is no association between 'Diet' and 'weight', I am going to use the select() and arrange() functions:

``` r
#creates new dataset selecting only weight and Diet
SelectDietWeight <- Chicks %>% select(weight, Diet)
#arrange dataset by weight
arrange(SelectDietWeight, weight)
```

    ##    weight Diet
    ## 1     108    2
    ## 2     108    4
    ## 3     108    1
    ## 4     124    2
    ## 5     124    4
    ## 6     124    1
    ## 7     124    1
    ## 8     136    2
    ## 9     136    1
    ## 10    141    4
    ## 11    141    1
    ## 12    141    1
    ## 13    143    2
    ## 14    148    1
    ## 15    148    2
    ## 16    148    4
    ## 17    153    4
    ## 18    158    3
    ## 19    160    1
    ## 20    160    1
    ## 21    160    1
    ## 22    160    4
    ## 23    168    1
    ## 24    168    4
    ## 25    169    3
    ## 26    169    2
    ## 27    171    1
    ## 28    179    3
    ## 29    181    1
    ## 30    199    1
    ## 31    199    4
    ## 32    199    1
    ## 33    199    4
    ## 34    203    4
    ## 35    217    2
    ## 36    222    4
    ## 37    227    1
    ## 38    227    3
    ## 39    230    2
    ## 40    248    1
    ## 41    248    1
    ## 42    250    3
    ## 43    250    1
    ## 44    263    3
    ## 45    295    3
    ## 46    303    4
    ## 47    309    2
    ## 48    318    2
    ## 49    318    2
    ## 50    322    4
    ## 51    327    3
    ## 52    332    3
    ## 53    341    3

``` r
#arrange dataset by Diet
arrange(SelectDietWeight, Diet)
```

    ##    weight Diet
    ## 1     108    1
    ## 2     124    1
    ## 3     124    1
    ## 4     136    1
    ## 5     141    1
    ## 6     141    1
    ## 7     148    1
    ## 8     160    1
    ## 9     160    1
    ## 10    160    1
    ## 11    168    1
    ## 12    171    1
    ## 13    181    1
    ## 14    199    1
    ## 15    199    1
    ## 16    227    1
    ## 17    248    1
    ## 18    248    1
    ## 19    250    1
    ## 20    108    2
    ## 21    124    2
    ## 22    136    2
    ## 23    143    2
    ## 24    148    2
    ## 25    169    2
    ## 26    217    2
    ## 27    230    2
    ## 28    309    2
    ## 29    318    2
    ## 30    318    2
    ## 31    158    3
    ## 32    169    3
    ## 33    179    3
    ## 34    227    3
    ## 35    250    3
    ## 36    263    3
    ## 37    295    3
    ## 38    327    3
    ## 39    332    3
    ## 40    341    3
    ## 41    108    4
    ## 42    124    4
    ## 43    141    4
    ## 44    148    4
    ## 45    153    4
    ## 46    160    4
    ## 47    168    4
    ## 48    199    4
    ## 49    199    4
    ## 50    203    4
    ## 51    222    4
    ## 52    303    4
    ## 53    322    4

These results further confirm that there does not seem to be an association between 'weight' and 'Diet' in the 'Chicks' dataset. Next, I want to explore whether there is an association between 'weight' and 'feed'.

``` r
#filters the data by feed as horsebean
Chicks %>% filter(feed == "horsebean")
```

    ##    weight Time Chick Diet      feed
    ## 1     108   12    22    2 horsebean
    ## 2     108    8    49    4 horsebean
    ## 3     108   14     4    1 horsebean
    ## 4     124   10    25    2 horsebean
    ## 5     124   10    41    4 horsebean
    ## 6     124   10     6    1 horsebean
    ## 7     124   21    10    1 horsebean
    ## 8     136   12    26    2 horsebean
    ## 9     136   16     4    1 horsebean
    ## 10    143   16    30    2 horsebean
    ## 11    160   18     6    1 horsebean
    ## 12    160   20     6    1 horsebean
    ## 13    160   20     4    1 horsebean
    ## 14    160   12    42    4 horsebean
    ## 15    168   12    11    1 horsebean
    ## 16    168   16    47    4 horsebean
    ## 17    179   14    32    3 horsebean
    ## 18    217   12    21    2 horsebean
    ## 19    227   16    14    1 horsebean
    ## 20    227   18    36    3 horsebean

``` r
#filters the data by feed as linseed
Chicks %>% filter(feed == "linseed")
```

    ##    weight Time Chick Diet    feed
    ## 1     141   14    45    4 linseed
    ## 2     141   12     5    1 linseed
    ## 3     141   12     6    1 linseed
    ## 4     148   14     6    1 linseed
    ## 5     148   18    22    2 linseed
    ## 6     148   12    47    4 linseed
    ## 7     169   20    37    3 linseed
    ## 8     169   16    26    2 linseed
    ## 9     181   20    11    1 linseed
    ## 10    203   18    49    4 linseed
    ## 11    309   21    29    2 linseed

``` r
#filters the data by feed as meatmeal
Chicks %>% filter(feed == "meatmeal")
```

    ##   weight Time Chick Diet     feed
    ## 1    153   14    41    4 meatmeal
    ## 2    263   18    32    3 meatmeal
    ## 3    303   20    48    4 meatmeal

``` r
#filters the data by feed as soybean
Chicks %>% filter(feed == "soybean")
```

    ##    weight Time Chick Diet    feed
    ## 1     158   10    35    3 soybean
    ## 2     171   18     1    1 soybean
    ## 3     199   20     1    1 soybean
    ## 4     199   20    43    4 soybean
    ## 5     199   18     5    1 soybean
    ## 6     199   20    41    4 soybean
    ## 7     230   18    29    2 soybean
    ## 8     248   18    14    1 soybean
    ## 9     248   18    14    1 soybean
    ## 10    250   20    39    3 soybean
    ## 11    250   18     7    1 soybean
    ## 12    327   20    34    3 soybean

``` r
#filters the data by feed as casein
Chicks %>% filter(feed == "casein")
```

    ##   weight Time Chick Diet   feed
    ## 1    222   16    48    4 casein
    ## 2    318   20    21    2 casein
    ## 3    332   18    35    3 casein

``` r
#filters the data by feed as sunflower
Chicks %>% filter(feed == "sunflower")
```

    ##   weight Time Chick Diet      feed
    ## 1    295   20    40    3 sunflower
    ## 2    318   20    21    2 sunflower
    ## 3    322   21    48    4 sunflower
    ## 4    341   21    34    3 sunflower

Based on these results, it is evident that there is an association between 'weight' and 'feed' in horsebean, as well as 'weight' and 'feed' in linseed. There seems to be a consistently lower weight associated with chicks being fed horsebeans and linseeds, with their weights mostly staying betwwen 100 grams and 200 grams. It also appears that there is an association between 'weight' and 'feed' in casein, and 'weight' and 'feed' in sunflower. There seems to be a consistently higher weight associated with chicks being fed caseins and sunflowers, with their weights mostly staying in the high 200s and 300s (grams).

I then computed a correlation matrix for only the numerical variables in the data. To do this, I first needed to create a new dataset which only included the numeric variables:

``` r
#creates new dataset with only numerical variables
num <- Chicks %>% select(weight, Time)
```

Then, I used cor() to compute a correlation matrix:

``` r
#creates correlation matrix between numeric variables
corrmatrix <- round(cor(num),2)
head(corrmatrix)
```

    ##        weight Time
    ## weight   1.00 0.65
    ## Time     0.65 1.00

Next, I made a correlation heatmap of only the numeric variables. In order to do this, I first had to melt the correlation matrix created above:

``` r
library(reshape2)
#melts the correlation matrix
melted_corrmatrix <- melt(corrmatrix)
head(melted_corrmatrix)
```

    ##     Var1   Var2 value
    ## 1 weight weight  1.00
    ## 2   Time weight  0.65
    ## 3 weight   Time  0.65
    ## 4   Time   Time  1.00

Fianlly, I used the following code to create the correlation heatmap between the two numeric variables:

``` r
library(ggplot2)
#creates correlation heatmap
ggplot(data = melted_corrmatrix, aes(x=Var1, y=Var2, fill=value)) + 
  geom_tile()
```

![](Project-2_files/figure-markdown_github/unnamed-chunk-9-1.png)

Based on the correlation heatmap that was just created, I concluded that there is a moderate to strong correlation between the two numeric variables. The correlation coefficients were 0.65, which indicates a positive linear relationship between 'weight' and 'Time'.

I also chose to create a histogram of 'weight' faceted by 'feed' using the following code:

``` r
#creates histogram of 'weight' faceted by 'feed'
ggplot(Chicks, aes(x=weight)) + geom_histogram() + facet_wrap(~feed) + ggtitle("Weight Faceted by Feed")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Project-2_files/figure-markdown_github/unnamed-chunk-10-1.png)

These visuals highlight my findings from earlier that showed that on average, chicks that were fed sunflower and casein were heavier and chicks that were fed horsebean and linseed were lighter. You can see this by their distributions on the graphs (horsebean data and linseed data congregates to the left and casein data and sunflower data congregates to the right).

## MANOVA

We will be performing a MANOVA to test whether my numeric variables 'weight' and 'Time' show a mean difference across levels of my categorical variable 'feed'. I did this using the following code:

``` r
#compute manova
res.man <- manova(cbind(weight, Time) ~ feed, data = Chicks)
summary(res.man)
```

    ##           Df  Pillai approx F num Df den Df    Pr(>F)    
    ## feed       5 0.67179   4.7544     10     94 1.667e-05 ***
    ## Residuals 47                                             
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
summary.aov(res.man)
```

    ##  Response weight :
    ##             Df Sum Sq Mean Sq F value    Pr(>F)    
    ## feed         5 144669 28933.9  14.461 1.413e-08 ***
    ## Residuals   47  94037  2000.8                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ##  Response Time :
    ##             Df Sum Sq Mean Sq F value   Pr(>F)   
    ## feed         5 200.08  40.015  3.7405 0.006225 **
    ## Residuals   47 502.79  10.698                    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
#compute anova
```

For both of the response variables, we see that our results are statistically significant. This means that the numeric variables 'Time' and 'weight' do show a mean difference across levels of 'feed'. The p-value for 'weight' by 'feed' is 1.413e-08, and the p-value for 'Time' by 'feed' is 0.006225.

Next, I will perform univariate ANOVAs:

``` r
#compute anova
res.aov <- aov(Time ~ feed, data = Chicks)
summary(res.aov)
```

    ##             Df Sum Sq Mean Sq F value  Pr(>F)   
    ## feed         5  200.1   40.02   3.741 0.00622 **
    ## Residuals   47  502.8   10.70                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
#compute anova
res.aov <- aov(weight ~ feed, data = Chicks)
summary(res.aov)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## feed         5 144669   28934   14.46 1.41e-08 ***
    ## Residuals   47  94037    2001                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Lastly, I will perform post-hoc t tests using the following code:

``` r
#post-hoc t test for ANOVA
TukeyHSD(res.aov)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = weight ~ feed, data = Chicks)
    ## 
    ## $feed
    ##                           diff         lwr       upr     p adj
    ## horsebean-casein    -137.61667 -219.881352 -55.35198 0.0001305
    ## linseed-casein      -118.12121 -204.664262 -31.57816 0.0024481
    ## meatmeal-casein      -51.00000 -159.487454  57.48745 0.7289029
    ## soybean-casein       -67.50000 -153.266863  18.26686 0.1998022
    ## sunflower-casein      28.33333  -73.147388 129.81405 0.9605227
    ## linseed-horsebean     19.49545  -30.380894  69.37180 0.8527180
    ## meatmeal-horsebean    86.61667    4.351981 168.88135 0.0337193
    ## soybean-horsebean     70.11667   21.599602 118.63373 0.0011686
    ## sunflower-horsebean  165.95000   93.174403 238.72560 0.0000003
    ## meatmeal-linseed      67.12121  -19.421838 153.66426 0.2130091
    ## soybean-linseed       50.62121   -4.841627 106.08405 0.0919652
    ## sunflower-linseed    146.45455   68.875499 224.03359 0.0000151
    ## soybean-meatmeal     -16.50000 -102.266863  69.26686 0.9924294
    ## sunflower-meatmeal    79.33333  -22.147388 180.81405 0.2058013
    ## sunflower-soybean     95.83333   19.121119 172.54555 0.0068154

Based on these results, we see that 'sunflower-horsebean' has the lowest p adjustment. I will be using these two subcategories within the 'feed' variable to compare means.

## Randomization Test

We will be comparing the difference in 'weight' means between the 'sunflower' feed type and the 'horsebean' feed type. Based on our EDA, we know that there is a relationship between 'weight' and 'feed', with chicks fed the 'horsebean' diet measuring lighter on average, and chicks fed the 'sunflower' diet measuring heavier on average. First, we will create a new dataset by filtering the data to only include values associated with the 'sunflower' and 'horsebean' feed types:

``` r
#filters data by 'horsebean' and 'sunflower'
Chicks_2 <- filter(Chicks, feed == "horsebean" | feed == "sunflower")
```

When performing a randomization test, we must first state the null and alternative hypotheses.

Null Hypothesis: There is no difference in means between the 'sunflower' weight values and the 'horsebean' weight values.

Alternative Hypothesis: There is a difference in means between the 'sunflower' weight values and the 'horsebean' weight values.

Next, we will compute the mean difference between the 'horsebean' feed type and the 'sunflower' feed type on this scrambled dataset:

``` r
#computes mean difference
Chicks_2 %>% 
  mutate(weight = sample(weight)) %>%
  group_by(feed) %>%
  summarize(means = mean(weight)) %>%
  summarize(diff_means = diff(means)) %>%
  pull
```

    ## [1] 34.25

We will then repeat the randomization 1000 times, save the result in a vector, and visualize the null sampling distribution using a histogram:

``` r
#set seed
set.seed(348)
#create empty vector
diff_means_random <- vector()
#run randomization 
for(i in 1:1000){
  temp <- Chicks_2 %>% 
    mutate(weight = sample(weight))
  
  diff_means_random[i] <- temp %>% 
    group_by(feed) %>%
    summarize(means = mean(weight)) %>%
    summarize(diff_means_random = -diff(means)) %>%
    pull
}
#create histogram 
hist(diff_means_random)
```

![](Project-2_files/figure-markdown_github/unnamed-chunk-16-1.png)

Based on these results, it seems that we can reject our null hypothesis and conclude that there is a difference in means between the 'sunflower' weight values and the 'horsebean' weight values.

To confirm this even further, we can perform a two sample t-test:

``` r
t.test(weight ~ feed, data = Chicks_2, var.equal = FALSE)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  weight by feed
    ## t = -13.166, df = 8.6855, p-value = 4.902e-07
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -194.6212 -137.2788
    ## sample estimates:
    ## mean in group horsebean mean in group sunflower 
    ##                  153.05                  319.00

With a p-value of 4.902e-07 (less than our significance value of 0.05), we confirm that we can reject the null hypothesis.

## Linear Regression Model

First, let's visualize the effects that the variables 'Time' and 'feed' have on 'weight':

``` r
ggplot(data=Chicks,aes(x=Time, y=weight, color=feed))+
  geom_point()+ geom_smooth(method="lm") + ggtitle("Time vs. Weight")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Project-2_files/figure-markdown_github/unnamed-chunk-18-1.png)

We can see here that as 'Time' increases, 'weight' also increases, and we are able to see graphically how the different 'feed' types affect 'weight' differently over 'Time'.

Before we perform our linear regeression model, we need to center all of the numeric variables involved in the model. This includes variables 'weight' and 'Time'. I did this by running the following code:

``` r
#finds mean of 'weight'
mean(Chicks$weight)
```

    ## [1] 198.1887

``` r
#creates new dataset with 'weight' centered
centChicks <- Chicks %>% mutate(weight = weight - 198.1887)
#finds mean of 'Time'
mean(Chicks$Time)
```

    ## [1] 16.41509

``` r
#creates new dataset with 'Time' centered
centerChicks <- centChicks %>% mutate(Time = Time - 16.41509 )
```

We will be regressing 'weight' (numeric response) on 'feed' and 'Time', as well as their interaction. I did this by running the following code:

``` r
#creates linear regression model
mymodel <- lm(weight ~ feed * Time, data = centerChicks)
summary(mymodel)        
```

    ## 
    ## Call:
    ## lm(formula = weight ~ feed * Time, data = centerChicks)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -51.138 -20.667  -4.286  12.861  89.861 
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)          54.440     30.133   1.807  0.07816 . 
    ## feedhorsebean       -92.719     31.626  -2.932  0.00549 **
    ## feedlinseed         -76.929     32.198  -2.389  0.02157 * 
    ## feedmeatmeal        -36.246     37.933  -0.956  0.34491   
    ## feedsoybean         -42.812     32.807  -1.305  0.19919   
    ## feedsunflower       -35.752    157.049  -0.228  0.82105   
    ## Time                 24.000     13.241   1.813  0.07724 . 
    ## feedhorsebean:Time  -20.678     13.436  -1.539  0.13147   
    ## feedlinseed:Time    -14.270     13.672  -1.044  0.30274   
    ## feedmeatmeal:Time     1.357     15.826   0.086  0.93208   
    ## feedsoybean:Time    -16.378     13.860  -1.182  0.24413   
    ## feedsunflower:Time    1.000     39.724   0.025  0.98004   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 37.45 on 41 degrees of freedom
    ## Multiple R-squared:  0.7591, Adjusted R-squared:  0.6944 
    ## F-statistic: 11.74 on 11 and 41 DF,  p-value: 1.763e-09

Based on the results of this regression model, two of the explanatory variables seem to have a statistically significant effect on 'weight', including 'feed' in horsebean and 'feed' in linseed. The coefficient estimate for horsebean is -92.719, which means that for chicks fed the horsebean 'feed', 'weight' significantly decreases by 92.719. The coefficient estimate for linseed is -76.929, which means that for chicks fed the linseed 'feed', 'weight' significantly decreases by 76.929.The coefficient estimate for the intercept is -141.333 and is partially significant. The coefficient estimates for meatmeal, soybean, and sunflower were -36.246, -42.812, and -35.752 respectively. We expected 'sunflower' to have the most positive coefficient and horsebean to have the most negative coefficient among 'feed' types because we saw from our EDA that chicks on the 'sunflower' feed type were overall heavier and chicks on the 'horsebean' feed type were overall lighter. The variable 'Time' is also partially significant and has a coefficient estimate of 24.000, which means it is positively correlated with 'weight'. We also expected this relationship from earlier analysis in EDA. When looking at the results for our interactions, none of them were statistically significant, with 'feedhorsebean:Time' having the most negative coefficient of all interactions and 'feedsunflower:Time' having the most positive coefficient of all interactions. We expected this from earlier analysis.

Now that we have created our linear regression model, let's check for assumptions. To start, I used the following code to check for the linearity assumption:

``` r
#checks for linearity
plot(mymodel, 1)
```

![](Project-2_files/figure-markdown_github/unnamed-chunk-21-1.png)

Since we don't observe any patterns in the residual plot, we can conclude that the model is linear.

Next, I used the following code to check for the assumption of normality:

``` r
#checks for normality
plot(mymodel, 2)
```

![](Project-2_files/figure-markdown_github/unnamed-chunk-22-1.png)

Since the data generally lines up along the linear line, we can conclude that the model is normal.

Lastly, I used the following code to check for the assumption of homogeneity of variance:

``` r
#checks for homogeneity of variance
plot(mymodel, 3)
```

![](Project-2_files/figure-markdown_github/unnamed-chunk-23-1.png)

Since the data is generally spread equally and the line is generally horizontal, we can conclude that the model has homogeneity of variance.

Next, I used the following code to calculate robust standard errors:

``` r
library("lmtest")
```

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

``` r
library("sandwich")
#robust t-test
coeftest(mymodel, vcov = vcovHC(mymodel, type = "HC0"))
```

    ## 
    ## t test of coefficients:
    ## 
    ##                    Estimate Std. Error t value  Pr(>|t|)    
    ## (Intercept)         54.4401    20.4658  2.6600 0.0110985 *  
    ## feedhorsebean      -92.7195    22.5059 -4.1198 0.0001794 ***
    ## feedlinseed        -76.9291    23.5100 -3.2722 0.0021699 ** 
    ## feedmeatmeal       -36.2462    20.6027 -1.7593 0.0859881 .  
    ## feedsoybean        -42.8118    22.0244 -1.9438 0.0587997 .  
    ## feedsunflower      -35.7516    48.8755 -0.7315 0.4686452    
    ## Time                24.0000     7.3068  3.2846 0.0020956 ** 
    ## feedhorsebean:Time -20.6784     7.5523 -2.7380 0.0091024 ** 
    ## feedlinseed:Time   -14.2699     8.3691 -1.7051 0.0957475 .  
    ## feedmeatmeal:Time    1.3571     7.3459  0.1847 0.8543381    
    ## feedsoybean:Time   -16.3785     7.7164 -2.1225 0.0398777 *  
    ## feedsunflower:Time   1.0000    12.8312  0.0779 0.9382587    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

When looking at these results, we see that there are much more statistically significant relationships between the predictors and the response, including the intercept, horsebean 'feed', linseed 'feed', 'Time', 'feedhorsebean:Time', and 'feedsoybean:Time'.

## Logistic Regression

First, I used the mutate() function to create a new binary categorical variable in the dataset:

``` r
#create new categorical variable
NewChicks <- mutate(Chicks, newborn = ifelse(Time %in% 1:14, "Yes", ifelse(Time %in% 15:21, "No", "Maybe")))
#previews the new dataset
head(NewChicks)
```

    ##   weight Time Chick Diet      feed newborn
    ## 1    108   12    22    2 horsebean     Yes
    ## 2    108    8    49    4 horsebean     Yes
    ## 3    108   14     4    1 horsebean     Yes
    ## 4    124   10    25    2 horsebean     Yes
    ## 5    124   10    41    4 horsebean     Yes
    ## 6    124   10     6    1 horsebean     Yes

It is necessary to define the categorical variable 'newborn'. Any chick that was measured and recorded in the dataset between 0 and 14 days since birth is considered a newborn chick, and is denoted with a "Yes". Any chick that was measured and recorded in the dataset between 15 and 21 days since birth is considered not a newborn chick, and is denoted with a "No".

Next, we will build a logistic regression model predicting 'newborn' from 'Time' and 'weight', with no interaction:

``` r
#convert 'newborn' from character to factor 
NewChicks$newborn <- as.factor(NewChicks$newborn)
#creates logistic regression model 
fit <- glm(newborn ~ Time + weight, data = NewChicks, family = "binomial")
```

    ## Warning: glm.fit: algorithm did not converge

    ## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

``` r
summary(fit)
```

    ## 
    ## Call:
    ## glm(formula = newborn ~ Time + weight, family = "binomial", data = NewChicks)
    ## 
    ## Deviance Residuals: 
    ##        Min          1Q      Median          3Q         Max  
    ## -1.550e-05  -2.110e-08  -2.110e-08   2.110e-08   1.545e-05  
    ## 
    ## Coefficients:
    ##               Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)  3.439e+02  2.666e+05   0.001    0.999
    ## Time        -2.290e+01  1.930e+04  -0.001    0.999
    ## weight      -2.495e-03  5.869e+02   0.000    1.000
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 6.7923e+01  on 52  degrees of freedom
    ## Residual deviance: 2.4052e-09  on 50  degrees of freedom
    ## AIC: 6
    ## 
    ## Number of Fisher Scoring iterations: 25

We see from the results that neither of the predictors are statistically significant in their relation to whether a chick is a 'newborn' or not. However, when looking at the coefficient estimates, we see that they are -2.290e+01 and -2.495e-03 for 'Time' and 'weight' respectively. This means that for chicks that are considered a 'newborn', its amount of 'Time' since birth has a negative correlation, and its 'weight' has a negative correlation. The intercept coefficient estimate is 3.439e+02 and is also not statistically significant.

Next, I generated an ROC curve using the following code:

``` r
library(plotROC)
#create new predicted and probability variables
NewChicks$prob <- predict(fit, newdata = NewChicks, type = "response")
NewChicks$predicted <- ifelse(NewChicks$prob > .5, 1, 0)
#ROC curve
ggplot(NewChicks) + 
  geom_roc(aes(d = newborn, m = prob), cutoffs.at = list(0.3, 0.5, 0.6, 0.8)) 
```

    ## Warning in verify_d(data$d): D not labeled 0/1, assuming No = 0 and Yes = 1!

![](Project-2_files/figure-markdown_github/unnamed-chunk-27-1.png)

Lastly, we will calculate the AUC using the following code:

``` r
#save ROC plot 
ROCplot <- ggplot(NewChicks) + 
  geom_roc(aes(d = newborn, m = prob), cutoffs.at = list(0.3, 0.5, 0.6, 0.8)) 
#calculate AUC
AUC <- calc_auc(ROCplot)$AUC
```

    ## Warning in verify_d(data$d): D not labeled 0/1, assuming No = 0 and Yes = 1!

``` r
AUC
```

    ## [1] 1

The area under the curve is 1. This means that our predictions are 100% right.
