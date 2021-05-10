Chick Weight
================
Jayme R. Reyna
3/10/2021

# Introduction

My report is entitled Chick Weight by Feed Type. I plan to combine two separate datasets that share a common variable and perform exploratory data analysis on the newly created dataset. The two datasets are called 'ChickWeight' and 'chickwts', and were acquired through R using the following code:

``` r
#lists all datasets in R
data(package = .packages(all.available = TRUE))
```

The above code listed all datasets in R by packages, from which I was able to choose two. The dataset entitled 'ChickWeight' contains data from an experiment conducted to measure the effect of diet on early growth of chicks, and contains a total of 578 observations across 4 variables, including 'weight', 'Time', 'Chick', and 'Diet'. The variable 'weight' is a numeric vector giving the body weight of the chick in grams, 'Time' is a numeric vector giving the number of days since birth when the measurement was made, 'Chick' is an ordered factor with levels giving a unique identifier for the chick, and 'Diet' is a factor with levels 1-4 indicating which experimental diet the chick received. The dataset entitled 'chickwts' contains data from an experiment conducted to measure and compare the effectiveness of various feed supplements on the growth rate of chickens, and contains a total of 71 observations across 2 variables, including 'weight' and 'feed'. The variable 'weight' is a numeric variable giving the chick weight in grams and 'feed' is a factor giving the feed type. The two datasets share the variable 'weight', and are similar in that they both concern chicks and their growth rate measured by a number of variables. These datasets were particularly interesting to me because I eventually plan on studying animal science more extensively. Although the datasets are relatively simple, I expect to see associations among them.

# Tidy Data

Because the data obtained came directly from R, both of the datasets are tidy. They each contain one row per observation and one column for every variable. Therefore, there is no need to tidy the data any further.

# Join/Merge

In order to successfully join the two datasets, I used the merge() function:

``` r
#merges the datasets using inner join
Chicks <- merge(ChickWeight, chickwts, by = "weight")
```

I chose to perform an inner join because it produced the least amount of 'NA's in the newly created dataset. Inner joins select all rows from both tables where the value of the common field is the same. The resulting combined dataset called 'Chicks' contains 53 observations across 5 variables. Two of these variables are numeric ('weight', 'Time'). The remaining three variables are factors ('Chick', 'Diet', 'feed'). The cases that were dropped were those in which the values for 'weight' did not match in both of the original datasets. One potential problem is that performing an inner join significantly reduced the amount of observations in 'Chicks'. It would be justified to assume that the summary statistics may not be as accurate or extensive as in the original datasets because the range of values is significantly reduced.

# Summary Statistics

I used all six core dplyr functions to manipulate and explore my newly created dataset. First, I will use the filter() function to filter the data based on the 4 different diets and observe and compare the chick weights recorded for each diet.

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

Next, I used the mutate() function to create a categorical variable in the dataset:

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

First, it was necessary to define the categorical variable 'newborn'. Any chick that was measured and recorded in the dataset between 0 and 14 days since birth is considered a newborn chick, and is denoted with a "Yes". Any chick that was measured and recorded in the dataset between 15 and 21 days since birth is considered not a newborn chick, and is denoted with a "No".

Next, I used the function group\_by() to group my dataset by the new categorical variable that I just created, 'newborn':

``` r
#create new dataset grouped by categorical variable 'newborn'
group_by_newborn <- NewChicks %>% group_by(newborn)
```

I then ran summary statistics on this new dataset grouped by the categorical variable 'newborn':

``` r
#gives summary statistics of variables
summary(group_by_newborn)
```

    ##      weight           Time           Chick    Diet          feed   
    ##  Min.   :108.0   Min.   : 8.00   6      : 5   1:19   casein   : 3  
    ##  1st Qu.:148.0   1st Qu.:14.00   4      : 3   2:11   horsebean:20  
    ##  Median :171.0   Median :18.00   14     : 3   3:10   linseed  :11  
    ##  Mean   :198.2   Mean   :16.42   21     : 3   4:13   meatmeal : 3  
    ##  3rd Qu.:248.0   3rd Qu.:20.00   41     : 3          soybean  :12  
    ##  Max.   :341.0   Max.   :21.00   48     : 3          sunflower: 4  
    ##                                  (Other):33                        
    ##    newborn         
    ##  Length:53         
    ##  Class :character  
    ##  Mode  :character  
    ##                    
    ##                    
    ##                    
    ## 

We see that for our two numerical variables ('weight', 'Time'), we were given Min., 1st Qu., Median, Mean, 3rd Qu., and Max.

I then ran summary statistics on the original dataset 'Chicks' to compare to the summary statistics above.

``` r
#gives summary statistics of variables
summary(Chicks)
```

    ##      weight           Time           Chick    Diet          feed   
    ##  Min.   :108.0   Min.   : 8.00   6      : 5   1:19   casein   : 3  
    ##  1st Qu.:148.0   1st Qu.:14.00   4      : 3   2:11   horsebean:20  
    ##  Median :171.0   Median :18.00   14     : 3   3:10   linseed  :11  
    ##  Mean   :198.2   Mean   :16.42   21     : 3   4:13   meatmeal : 3  
    ##  3rd Qu.:248.0   3rd Qu.:20.00   41     : 3          soybean  :12  
    ##  Max.   :341.0   Max.   :21.00   48     : 3          sunflower: 4  
    ##                                  (Other):33

Based on these results, the summary statistics on the original dataset 'Chicks' do not differ from the summary statistics when the data was grouped by 'newborn'.

Finally, I computed a correlation matrix for only the numerical variables in the data. To do this, I first needed to create a new dataset which only included the numeric variables:

``` r
#creates new dataset with only numerical variables
num <- NewChicks %>% select(weight, Time)
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

# Visualizations

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

![](Project-Report_files/figure-markdown_github/unnamed-chunk-13-1.png)

Based on the correlation heatmap that was just created, I concluded that there is a moderate to strong correlation between the two numeric variables. The correlation coefficients were 0.65, which indicates a positive linear relationship between 'weight' and 'Time'.

I also chose to creat a scatterplot of the data using the following code:

``` r
#creates scatterplot
ggplot(data=NewChicks,aes(x=Time, y=weight, color=newborn))+
  geom_point()+ geom_smooth(method="lm") + ggtitle("Time vs. Weight")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Project-Report_files/figure-markdown_github/unnamed-chunk-14-1.png)

I plotted 'Time' on the x-axis, 'weight' on the y-axis, and colored by a third variable 'newborn'. From the scatterplot we can see that there is a clear positive linear relationship between 'Time' and 'weight'. As 'Time' increases, 'weight' of the chicks increases as well. Knowing how we defined 'newborn' earlier, the coloring of the scatterplot is also consistent with these findings.

I also chose to create a histogram of 'weight' faceted by 'feed' using the following code:

``` r
#creates histogram of 'weight' faceted by 'feed'
ggplot(NewChicks, aes(x=weight)) + geom_histogram() + facet_wrap(~feed) + ggtitle("Weight Faceted by Feed")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Project-Report_files/figure-markdown_github/unnamed-chunk-15-1.png)

These visuals highlight my findings from earlier that showed that on average, chicks that were fed sunflower and casein were heavier and chicks that were fed horsebean and linseed were lighter. You can see this by their distributions on the graphs (horsebean data and linseed data congregates to the left and casein data and sunflower data congregates to the right).

Lastly, I chose to create a histogram of 'weight' faceted by 'Diet', as well as a scatterplot of 'Diet' and 'weight' colored by 'newborn' using the following code:

``` r
#creates histogram of 'weight' faceted by 'Diet'
ggplot(NewChicks, aes(x=weight)) + geom_histogram() + facet_wrap(~Diet) + ggtitle("Weight Faceted by Diet")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Project-Report_files/figure-markdown_github/unnamed-chunk-16-1.png)

``` r
#creates scatterplot
ggplot(data=NewChicks,aes(x=Diet, y=weight, color=newborn))+ 
  geom_point()+ geom_smooth(method="lm") + ggtitle("Diet vs. Weight")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Project-Report_files/figure-markdown_github/unnamed-chunk-16-2.png)

These visuals highlight my findings from earlier that showed no correlation between 'weight' and 'Diet'.

# PAM Clustering

First, I needed to determine how many clusters to use for PAM. I did this using the following code:

``` r
library(cluster)
library(factoextra)
```

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

``` r
#determines optimal number of clusters
fviz_nbclust(num, FUNcluster = pam, method = "s")
```

![](Project-Report_files/figure-markdown_github/unnamed-chunk-17-1.png)

From this plot, we saw that the optimal number of clusters for PAM is 3.

Next, I ran PAM using the optimal number of clusters:

``` r
#creates new dataframe while running PAM 
Chicks_PAM <- pam(num, k=3)
#prints PAM
print(Chicks_PAM)
```

    ## Medoids:
    ##      ID weight Time
    ## [1,] 14    148   14
    ## [2,] 38    227   18
    ## [3,] 48    318   20
    ## Clustering vector:
    ##  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2
    ## [39] 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3
    ## Objective function:
    ##    build     swap 
    ## 21.45843 16.86597 
    ## 
    ## Available components:
    ##  [1] "medoids"    "id.med"     "clustering" "objective"  "isolation" 
    ##  [6] "clusinfo"   "silinfo"    "diss"       "call"       "data"

Next, I ran this code to produce a plot which shows the best projection of the six-dimentional data in 2D:

``` r
#projection of six-dimensional data in 2D
plot(Chicks_PAM, which=1)
```

![](Project-Report_files/figure-markdown_github/unnamed-chunk-19-1.png)

Finally, I determined the Principal Components by performing Principal Component Analysis:

``` r
#scales numeric variables 
Chicks_pca <- num %>%
  scale %>%
  prcomp
```

``` r
#visualize how the variables contribute to the principal components
fviz_pca_var(Chicks_pca, col.var = "black")
```

![](Project-Report_files/figure-markdown_github/unnamed-chunk-21-1.png)

``` r
# Computes the percentage of variance explained
percent <- 100* (Chicks_pca$sdev^2 / sum(Chicks_pca$sdev^2))
# Scree plot with percentages as text
fviz_screeplot(Chicks_pca) + geom_text(aes(label = round(percent, 2)), size = 4, vjust = -0.5)
```

![](Project-Report_files/figure-markdown_github/unnamed-chunk-22-1.png)

Based on this information, we know that we only need to keep one Principal Component. We know this because PC1 accounts for more than 80% of the variance (82.47%).
