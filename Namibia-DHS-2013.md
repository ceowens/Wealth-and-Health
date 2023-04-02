---
title: "Namibia DHS"
author: "Caroline Owens"
date: "2023-03-23"
output: 
  html_document: 
    keep_md: yes
---



### DHS Namibia— Assessing wealth, status, and mental well-being using Namibia DHS data













##### MCA



##### Assess Wealth Dimensions against the DHS Wealth Index (HV271)

```r
#################################
### Anchor and Check Results ####
#################################

#### Here, we check our created dimension 1 against the pre-created DHS wealth index
plot(hhnamibia_wealthdim$Dim.1, hhnamibia_wealthdim$HV271, xlab="Dim 1 Original", ylab="DHS Wealth Index")
```

![](Namibia-DHS-2013_files/figure-html/anchor-1.png)<!-- -->

```r
#reverse Dim 1
hhnamibia_wealthdim$Dim.1 <- -1*hhnamibia_wealthdim$Dim.1
plot(hhnamibia_wealthdim$Dim.1, hhnamibia_wealthdim$HV271, xlab="Dim 1", ylab="DHS Wealth Index")
```

![](Namibia-DHS-2013_files/figure-html/anchor-2.png)<!-- -->

```r
plot(hhnamibia_wealthdim$Dim.2, hhnamibia_wealthdim$HV271, xlab="Dim 2", ylab="DHS Wealth Index")
```

![](Namibia-DHS-2013_files/figure-html/anchor-3.png)<!-- -->






```
## Warning: `qplot()` was deprecated in ggplot2 3.4.0.
```

![](Namibia-DHS-2013_files/figure-html/create vars-1.png)<!-- -->



##### Assess means of key variables and dimensions 1 and 2 to affirm anchoring and examine differences in assets across the landscape of wealth in Namibia. 

```
## # A tibble: 3 × 4
##   AnyCows meandim1 meandim2  nobs
##     <dbl>    <dbl>    <dbl> <int>
## 1       0   0.0404   -0.108  6757
## 2       1  -0.108     0.256  2951
## 3      NA   0.241     0.290     4
```

```
## # A tibble: 3 × 4
##   AnyChickens meandim1 meandim2  nobs
##         <dbl>    <dbl>    <dbl> <int>
## 1           0   0.0915  -0.0997  6031
## 2           1  -0.162    0.170   3676
## 3          NA   0.114    0.324      5
```

```
## # A tibble: 2 × 4
##   AnyLand meandim1 meandim2  nobs
##     <dbl>    <dbl>    <dbl> <int>
## 1       0   0.0905  -0.0904  5734
## 2       1  -0.142    0.136   3978
```

```
## # A tibble: 2 × 4
##   Urban meandim1 meandim2  nobs
##   <dbl>    <dbl>    <dbl> <int>
## 1     0   -0.176   0.0549  5020
## 2     1    0.179  -0.0535  4692
```

```
## # A tibble: 2 × 4
##   BankAccount meandim1 meandim2  nobs
##         <dbl>    <dbl>    <dbl> <int>
## 1           0  -0.215   -0.0725  2561
## 2           1   0.0707   0.0294  7151
```

```
## # A tibble: 2 × 4
##   Computer meandim1 meandim2  nobs
##      <dbl>    <dbl>    <dbl> <int>
## 1        0  -0.0897  -0.0138  7893
## 2        1   0.364    0.0732  1819
```

```
## # A tibble: 2 × 4
##      TV meandim1 meandim2  nobs
##   <dbl>    <dbl>    <dbl> <int>
## 1     0   -0.200  -0.0185  5484
## 2     1    0.248   0.0297  4228
```


##### Expanding on assessment of means we will now assess correlations between dimensions and these variables. 

```
## [1] -0.3988543
```

```
## [1] 0.519735
```

```
## [1] -0.2385174
```

```
## [1] 0.7801348
```

```
## [1] -0.4302005
```

```
## [1] 0.6089328
```

```
## [1] 0.6183913
```

```
## [1] -0.2523585
```

```
## [1] 0.4394747
```

```
## [1] 0.2091677
```

```
## [1] 0.6186953
```

```
## [1] 0.1580558
```

```
## [1] 0.7757229
```

```
## [1] 0.1112578
```




##### Merge and clean data- note that, among those who report days of low energy and little energy, peak around 1 week (cognitive ease of 1 week?). Distributions are relatively similar across the sexes.

```r
###########################
### Merge into womens file ####
###########################

vars_to_keep_households <- c("HV000", "HV001", "HV002", "Dim.1", "Dim.2", "HV271")
hh_wealth <- hhnamibia_wealthdim[vars_to_keep_households]




# Mental health variables are as follows: S1010S = Times you see or hear things, S1010T = Felt worthless, S1010U = Felt little interest, S1010V = Felt low energy/sad
# For two of the variables there are also number of days variables: S1010UN = # of days felt little interest in past two weeks, S1010VN = # of days felt low energy/sad in past two weeks


# Explore the mental health variables for women
womennamibia %>% count(S1010S) #See or hear things
```

```
##   S1010S    n
## 1     No 8589
## 2    Yes 1422
## 3   <NA>    7
```

```r
womennamibia %>% count(S1010T) # felt worthless
```

```
##   S1010T    n
## 1     No 8659
## 2    Yes 1347
## 3   <NA>   12
```

```r
womennamibia %>% count(S1010U) #felt little interest
```

```
##               S1010U    n
## 1                 No 7804
## 2                Yes 2157
## 3 Dont know/not sure   52
## 4               <NA>    5
```

```r
womennamibia %>% count(S1010UN) # of days felt little interest
```

```
##    S1010UN    n
## 1        1  518
## 2        2  706
## 3        3  379
## 4        4  110
## 5        5   82
## 6        6   13
## 7        7  254
## 8        8    8
## 9        9    4
## 10      10   14
## 11      12    8
## 12      13    1
## 13      14   55
## 14      NA 7866
```

```r
# Plot distribution
#### Density plot/histogram # of Days Little Interest
womennamibia %>%
  ggplot(aes(x=S1010UN)) +
  geom_histogram(aes(y=..density..), fill = "lightblue") +
  geom_density(alpha = .2) +
  geom_vline(aes(xintercept=mean(S1010UN, na.rm = T)),
             color="black") +
  xlab("Number of Days of Little Interest") + ggtitle("Women") +
  theme_minimal()
```

```
## Warning: The dot-dot notation (`..density..`) was deprecated in ggplot2 3.4.0.
## ℹ Please use `after_stat(density)` instead.
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 7866 rows containing non-finite values (`stat_bin()`).
```

```
## Warning: Removed 7866 rows containing non-finite values (`stat_density()`).
```

![](Namibia-DHS-2013_files/figure-html/mh vars-1.png)<!-- -->

```r
womennamibia %>% count(S1010V) #felt low energy & sad
```

```
##               S1010V    n
## 1                 No 7679
## 2                Yes 2299
## 3 Dont know/not sure   36
## 4               <NA>    4
```

```r
womennamibia %>% count(S1010VN) # of days felt low energy/")
```

```
##    S1010VN    n
## 1        1  616
## 2        2  654
## 3        3  379
## 4        4  156
## 5        5   74
## 6        6   17
## 7        7  284
## 8        8   11
## 9        9    3
## 10      10   14
## 11      11    1
## 12      12   11
## 13      13    2
## 14      14   72
## 15      NA 7724
```

```r
#### Density plot/histogram # of Days Low Energy
womennamibia %>%
  ggplot(aes(x=S1010VN)) +
  geom_histogram(aes(y=..density..), fill = "lightblue") +
  geom_density(alpha = .2) +
  geom_vline(aes(xintercept=mean(S1010VN, na.rm = T)),
             color="black") +
  xlab("Number of Days of Low Energy") + ggtitle("Women") +
  theme_minimal()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 7724 rows containing non-finite values (`stat_bin()`).
```

```
## Warning: Removed 7724 rows containing non-finite values (`stat_density()`).
```

![](Namibia-DHS-2013_files/figure-html/mh vars-2.png)<!-- -->

```r
table(womennamibia$S1010U, womennamibia$S1010UN)
```

```
##                     
##                        1   2   3   4   5   6   7   8   9  10  12  13  14
##   No                   0   0   0   0   0   0   0   0   0   0   0   0   0
##   Yes                518 706 379 110  82  13 254   8   4  14   8   1  55
##   Dont know/not sure   0   0   0   0   0   0   0   0   0   0   0   0   0
```

```r
#explore mental health for men SM813Q", "SM813R", "SM813S", "SM813SN",  "SM813T", "SM813TN"

#namibia_all_men
mennamibia %>%  count(SM813Q) #See or hear things
```

```
##   SM813Q    n
## 1     No 3985
## 2    Yes  491
## 3   <NA>    5
```

```r
mennamibia %>%  count(SM813R)  #felt worthless
```

```
##   SM813R    n
## 1     No 4074
## 2    Yes  331
## 3   <NA>   76
```

```r
mennamibia %>%  count(SM813S) #felt little interest
```

```
##               SM813S    n
## 1                 No 3888
## 2                Yes  553
## 3 Dont know/not sure   35
## 4               <NA>    5
```

```r
mennamibia %>%  count(SM813SN) # of days felt little interest
```

```
##    SM813SN    n
## 1        1  148
## 2        2  186
## 3        3   83
## 4        4   40
## 5        5   18
## 6        6    9
## 7        7   47
## 8        8    3
## 9        9    1
## 10      10    4
## 11      11    1
## 12      13    1
## 13      14   12
## 14      NA 3928
```

```r
mennamibia %>%  count(SM813T)  #felt low energy & sad
```

```
##               SM813T    n
## 1                 No 3882
## 2                Yes  558
## 3 Dont know/not sure   35
## 4               <NA>    6
```

```r
mennamibia %>%  count(SM813TN) ## of days felt low energy/")
```

```
##    SM813TN    n
## 1        1  188
## 2        2  146
## 3        3   95
## 4        4   38
## 5        5   12
## 6        6    5
## 7        7   46
## 8        8    2
## 9        9    1
## 10      10    8
## 11      11    1
## 12      12    1
## 13      13    1
## 14      14   14
## 15      NA 3923
```

```r
#### Density plots for men to compare
mennamibia %>%
  ggplot(aes(x=SM813SN)) +
  geom_histogram(aes(y=..density..), fill = "orange") +
  geom_density(alpha = .2) +
  geom_vline(aes(xintercept=mean(SM813SN, na.rm = T)),
             color="black") +
  xlab("Number of Days of Little Interest") + ggtitle("Men") +
  theme_minimal()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 3928 rows containing non-finite values (`stat_bin()`).
```

```
## Warning: Removed 3928 rows containing non-finite values (`stat_density()`).
```

![](Namibia-DHS-2013_files/figure-html/mh vars-3.png)<!-- -->

```r
mennamibia %>%
  ggplot(aes(x=SM813TN)) +
  geom_histogram(aes(y=..density..), fill = "orange") +
  geom_density(alpha = .2) +
  geom_vline(aes(xintercept=mean(SM813TN, na.rm = T)),
             color="black") +
  xlab("Number of Days of Low Energy") + ggtitle("Men") +
  theme_minimal()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 3923 rows containing non-finite values (`stat_bin()`).
```

```
## Warning: Removed 3923 rows containing non-finite values (`stat_density()`).
```

![](Namibia-DHS-2013_files/figure-html/mh vars-4.png)<!-- -->





#### Note: women, on average, report higher frequency of days of low energy and little interest than men. Those in urban areas report higher frequency of days of low energy and little interest, on average, than those in rural areas. When grouped by both strata (sex/gender) and place of residence, women in urban areas report the highest frequency days, followed by women in rural areas, men in rural areas, and men in urban areas. In other words, there are interesting interactions between gender and place of residence on a descriptive level.

```r
#### Merge data—create new mental health variables that account for reported frequencies

namibia_men_women <- namibia_men_women %>%
  mutate(nointerest_days = ifelse(little_interst == "No", 0, days_litte_interest),
         lowenergy_days = ifelse(lowenergy_sad == "No", 0, days_low_energy)
         )

namibia_men_women %>% head()
```

```
##   V000 V001 V002   V005 V012  V025 V021           V022         V106    V191
## 1  NM6    1    7 531957   44 Urban    1  Erongo, urban    Secondary  151934
## 2  NM6    2    2 564917   25 Rural    2 Caprivi, rural No education -120322
## 3  NM6    2    4 564917   31 Rural    2 Caprivi, rural      Primary -134413
## 4  NM6    2    5      0   61 Rural    2 Caprivi, rural No education -126248
## 5  NM6    2    5 564917   35 Rural    2 Caprivi, rural      Primary -126248
## 6  NM6    2    5 564917   16 Rural    2 Caprivi, rural    Secondary -126248
##   gender see_or_hear felt_worthless little_interst lowenergy_sad
## 1 female          No            Yes            Yes            No
## 2 female          No             No             No            No
## 3 female          No            Yes            Yes           Yes
## 4 female          No             No             No            No
## 5 female          No            Yes            Yes           Yes
## 6 female          No             No             No            No
##   days_litte_interest days_low_energy V744A V744B V744C V744D V744E abuse
## 1                   1              NA    No    No    No    No    No     0
## 2                  NA              NA   Yes   Yes   Yes   Yes   Yes     4
## 3                   7               4   Yes    No    No    No    No     1
## 4                  NA              NA    No   Yes   Yes   Yes   Yes     3
## 5                   7               3    No   Yes   Yes    No   Yes     2
## 6                  NA              NA    No    No    No    No    No     0
##   nointerest_days lowenergy_days
## 1               1              0
## 2               0              0
## 3               7              4
## 4               0              0
## 5               7              3
## 6               0              0
```

```r
#### Group by gender
namibia_men_women %>%
#  filter(felt_worthless != 3) %>%
  group_by(gender) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## # A tibble: 2 × 4
##   gender mean_lowenergy mean_nointerest_days n_obs
##   <chr>           <dbl>                <dbl> <int>
## 1 female          0.760                0.692 10018
## 2 male            0.375                0.375  4481
```

```r
#### Group by urban/rural
namibia_men_women %>%
  #  filter(felt_worthless != 3) %>%
  group_by(V025) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## # A tibble: 2 × 4
##   V025  mean_lowenergy mean_nointerest_days n_obs
##   <fct>          <dbl>                <dbl> <int>
## 1 Urban          0.706                0.635  7387
## 2 Rural          0.574                0.552  7112
```

```r
#### Group by both 
namibia_men_women %>%
  group_by(gender, V025) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## `summarise()` has grouped output by 'gender'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 5
## # Groups:   gender [2]
##   gender V025  mean_lowenergy mean_nointerest_days n_obs
##   <chr>  <fct>          <dbl>                <dbl> <int>
## 1 female Urban          0.856                0.754  5163
## 2 female Rural          0.658                0.627  4855
## 3 male   Urban          0.357                0.359  2224
## 4 male   Rural          0.393                0.392  2257
```

```r
# Look at women's violence attitudes
namibia_women %>% count(V744A)
```

```
##        V744A    n
## 1         No 8375
## 2        Yes 1356
## 3 Don't know  283
## 4       <NA>    4
```

```r
namibia_men_women %>%
  group_by(gender) %>%
  count(V025)
```

```
## # A tibble: 4 × 3
## # Groups:   gender [2]
##   gender V025      n
##   <chr>  <fct> <int>
## 1 female Urban  5163
## 2 female Rural  4855
## 3 male   Urban  2224
## 4 male   Rural  2257
```

```r
namibia_men_women %>%
  group_by(gender) %>%
  count(little_interst)
```

```
## # A tibble: 8 × 3
## # Groups:   gender [2]
##   gender little_interst         n
##   <chr>  <fct>              <int>
## 1 female No                  7804
## 2 female Yes                 2157
## 3 female Dont know/not sure    52
## 4 female <NA>                   5
## 5 male   No                  3888
## 6 male   Yes                  553
## 7 male   Dont know/not sure    35
## 8 male   <NA>                   5
```

```r
namibia_men_women %>%
  group_by(gender) %>%
  count(lowenergy_sad)
```

```
## # A tibble: 8 × 3
## # Groups:   gender [2]
##   gender lowenergy_sad          n
##   <chr>  <fct>              <int>
## 1 female No                  7679
## 2 female Yes                 2299
## 3 female Dont know/not sure    36
## 4 female <NA>                   4
## 5 male   No                  3882
## 6 male   Yes                  558
## 7 male   Dont know/not sure    35
## 8 male   <NA>                   6
```

```r
namibia_men_women %>%
  group_by(gender) %>%
  count(felt_worthless)
```

```
## # A tibble: 6 × 3
## # Groups:   gender [2]
##   gender felt_worthless     n
##   <chr>  <fct>          <int>
## 1 female No              8659
## 2 female Yes             1347
## 3 female <NA>              12
## 4 male   No              4074
## 5 male   Yes              331
## 6 male   <NA>              76
```




```r
# merge namibia_men_women into hhwealth

namibia_hh_men_woman <- merge(namibia_men_women,hh_wealth, by.x =c("V000", "V001", "V002"), by.y = c("HV000", "HV001", "HV002"))

namibia_hh_men_woman %>%
  group_by(gender) %>%
  count(felt_worthless)
```

```
## # A tibble: 6 × 3
## # Groups:   gender [2]
##   gender felt_worthless     n
##   <chr>  <fct>          <int>
## 1 female No              8539
## 2 female Yes             1325
## 3 female <NA>              12
## 4 male   No              4002
## 5 male   Yes              325
## 6 male   <NA>              72
```

```r
#check results match
namibia_hh_men_woman %>%
  #  filter(felt_worthless != 3) %>%
  group_by(gender) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## # A tibble: 2 × 4
##   gender mean_lowenergy mean_nointerest_days n_obs
##   <chr>           <dbl>                <dbl> <int>
## 1 female          0.757                0.686  9876
## 2 male            0.372                0.374  4399
```

```r
namibia_hh_men_woman %>%
  #  filter(felt_worthless != 3) %>%
  group_by(V025) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## # A tibble: 2 × 4
##   V025  mean_lowenergy mean_nointerest_days n_obs
##   <fct>          <dbl>                <dbl> <int>
## 1 Urban          0.702                0.633  7271
## 2 Rural          0.573                0.546  7004
```

```r
namibia_hh_men_woman %>%
   filter(!is.na(lowenergy_days) & !is.na(nointerest_days)) %>%
  group_by(gender) %>%
  summarise(mean_lowenergy = mean(lowenergy_days),
            mean_nointerest_days = mean(nointerest_days),
            n_obs = length(lowenergy_days))
```

```
## # A tibble: 2 × 4
##   gender mean_lowenergy mean_nointerest_days n_obs
##   <chr>           <dbl>                <dbl> <int>
## 1 female          0.753                0.684  9791
## 2 male            0.373                0.375  4341
```

```r
#filter(!is.na(worthless))

namibia_hh_men_woman %>%
  filter(!is.na(lowenergy_days) & !is.na(nointerest_days)) %>%
  group_by(V025) %>%
  summarize(counts = sum(lowenergy_days), n = length(lowenergy_days))
```

```
## # A tibble: 2 × 3
##   V025  counts     n
##   <fct>  <dbl> <int>
## 1 Urban   5046  7182
## 2 Rural   3951  6950
```

```r
namibia_hh_men_woman %>%
  group_by(gender) %>%
  summarize(mean = mean(days_low_energy, na.rm = T),
            n = length(days_low_energy))
```

```
## # A tibble: 2 × 3
##   gender  mean     n
##   <chr>  <dbl> <int>
## 1 female  3.30  9876
## 2 male    3.00  4399
```

```r
namibia_hh_men_woman %>%
  #filter(lowenergy_days != 0) %>%
  group_by(gender, V025) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## `summarise()` has grouped output by 'gender'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 5
## # Groups:   gender [2]
##   gender V025  mean_lowenergy mean_nointerest_days n_obs
##   <chr>  <fct>          <dbl>                <dbl> <int>
## 1 female Urban          0.851                0.750  5091
## 2 female Rural          0.657                0.619  4785
## 3 male   Urban          0.352                0.360  2180
## 4 male   Rural          0.391                0.388  2219
```

```r
namibia_hh_men_woman %>%
  #  filter(felt_worthless != 3) %>%
  group_by(gender) %>%
  summarise(mean_lowenergy = mean(lowenergy_days, na.rm = T),
            mean_nointerest_days = mean(nointerest_days, na.rm = T),
            n_obs = length(lowenergy_days))
```

```
## # A tibble: 2 × 4
##   gender mean_lowenergy mean_nointerest_days n_obs
##   <chr>           <dbl>                <dbl> <int>
## 1 female          0.757                0.686  9876
## 2 male            0.372                0.374  4399
```

```r
# Three key variables felt_worthless, lowenergy_days, nointerest_days
```




```
## 
## Attaching package: 'table1'
```

```
## The following objects are masked from 'package:base':
## 
##     units, units<-
```

```{=html}
<div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Urban<br><span class='stratn'>(N=7271)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Rural<br><span class='stratn'>(N=7004)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=14275)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'>gender</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>female</td>
<td>5091 (70.0%)</td>
<td>4785 (68.3%)</td>
<td>9876 (69.2%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>male</td>
<td class='lastrow'>2180 (30.0%)</td>
<td class='lastrow'>2219 (31.7%)</td>
<td class='lastrow'>4399 (30.8%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>V012</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>31.3 (11.5)</td>
<td>31.8 (13.0)</td>
<td>31.5 (12.2)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>29.0 [15.0, 64.0]</td>
<td class='lastrow'>29.0 [15.0, 64.0]</td>
<td class='lastrow'>29.0 [15.0, 64.0]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>V106</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No education</td>
<td>360 (5.0%)</td>
<td>841 (12.0%)</td>
<td>1201 (8.4%)</td>
</tr>
<tr>
<td class='rowlabel'>Primary</td>
<td>1166 (16.0%)</td>
<td>2256 (32.2%)</td>
<td>3422 (24.0%)</td>
</tr>
<tr>
<td class='rowlabel'>Secondary</td>
<td>4928 (67.8%)</td>
<td>3656 (52.2%)</td>
<td>8584 (60.1%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Higher</td>
<td class='lastrow'>817 (11.2%)</td>
<td class='lastrow'>251 (3.6%)</td>
<td class='lastrow'>1068 (7.5%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>abuse</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.418 (0.931)</td>
<td>0.753 (1.20)</td>
<td>0.583 (1.08)</td>
</tr>
<tr>
<td class='rowlabel'>Median [Min, Max]</td>
<td>0 [0, 4.00]</td>
<td>0 [0, 4.00]</td>
<td>0 [0, 4.00]</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>8 (0.1%)</td>
<td class='lastrow'>5 (0.1%)</td>
<td class='lastrow'>13 (0.1%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Dim.1</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.183 (0.234)</td>
<td>-0.182 (0.204)</td>
<td>0.00385 (0.286)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>0.235 [-0.422, 0.596]</td>
<td class='lastrow'>-0.238 [-0.516, 0.575]</td>
<td class='lastrow'>-0.0477 [-0.516, 0.596]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Dim.2</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>-0.0406 (0.202)</td>
<td>0.0827 (0.220)</td>
<td>0.0199 (0.220)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>-0.0796 [-0.332, 0.766]</td>
<td class='lastrow'>0.0711 [-0.333, 0.721]</td>
<td class='lastrow'>-0.0388 [-0.333, 0.766]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>felt_worthless</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>6214 (85.5%)</td>
<td>6327 (90.3%)</td>
<td>12541 (87.9%)</td>
</tr>
<tr>
<td class='rowlabel'>Yes</td>
<td>1015 (14.0%)</td>
<td>635 (9.1%)</td>
<td>1650 (11.6%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>42 (0.6%)</td>
<td class='lastrow'>42 (0.6%)</td>
<td class='lastrow'>84 (0.6%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>nointerest_days</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.633 (1.80)</td>
<td>0.546 (1.57)</td>
<td>0.590 (1.69)</td>
</tr>
<tr>
<td class='rowlabel'>Median [Min, Max]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>59 (0.8%)</td>
<td class='lastrow'>41 (0.6%)</td>
<td class='lastrow'>100 (0.7%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>lowenergy_days</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.702 (1.95)</td>
<td>0.573 (1.65)</td>
<td>0.639 (1.81)</td>
</tr>
<tr>
<td class='rowlabel'>Median [Min, Max]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>54 (0.7%)</td>
<td class='lastrow'>31 (0.4%)</td>
<td class='lastrow'>85 (0.6%)</td>
</tr>
</tbody>
</table>
</div>
```

```{=html}
<div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Urban<br><span class='stratn'>(N=7387)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Rural<br><span class='stratn'>(N=7112)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=14499)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'>felt_worthless</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>6310 (85.4%)</td>
<td>6423 (90.3%)</td>
<td>12733 (87.8%)</td>
</tr>
<tr>
<td class='rowlabel'>Yes</td>
<td>1033 (14.0%)</td>
<td>645 (9.1%)</td>
<td>1678 (11.6%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>44 (0.6%)</td>
<td class='lastrow'>44 (0.6%)</td>
<td class='lastrow'>88 (0.6%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>nointerest_days</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.635 (1.80)</td>
<td>0.552 (1.58)</td>
<td>0.594 (1.70)</td>
</tr>
<tr>
<td class='rowlabel'>Median [Min, Max]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>61 (0.8%)</td>
<td class='lastrow'>41 (0.6%)</td>
<td class='lastrow'>102 (0.7%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>lowenergy_days</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.706 (1.95)</td>
<td>0.574 (1.65)</td>
<td>0.641 (1.81)</td>
</tr>
<tr>
<td class='rowlabel'>Median [Min, Max]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
<td>0 [0, 14.0]</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>55 (0.7%)</td>
<td class='lastrow'>31 (0.4%)</td>
<td class='lastrow'>86 (0.6%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>little_interst</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>5892 (79.8%)</td>
<td>5800 (81.6%)</td>
<td>11692 (80.6%)</td>
</tr>
<tr>
<td class='rowlabel'>Yes</td>
<td>1438 (19.5%)</td>
<td>1272 (17.9%)</td>
<td>2710 (18.7%)</td>
</tr>
<tr>
<td class='rowlabel'>Dont know/not sure</td>
<td>49 (0.7%)</td>
<td>38 (0.5%)</td>
<td>87 (0.6%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>8 (0.1%)</td>
<td class='lastrow'>2 (0.0%)</td>
<td class='lastrow'>10 (0.1%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>lowenergy_sad</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>5793 (78.4%)</td>
<td>5768 (81.1%)</td>
<td>11561 (79.7%)</td>
</tr>
<tr>
<td class='rowlabel'>Yes</td>
<td>1542 (20.9%)</td>
<td>1315 (18.5%)</td>
<td>2857 (19.7%)</td>
</tr>
<tr>
<td class='rowlabel'>Dont know/not sure</td>
<td>43 (0.6%)</td>
<td>28 (0.4%)</td>
<td>71 (0.5%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>9 (0.1%)</td>
<td class='lastrow'>1 (0.0%)</td>
<td class='lastrow'>10 (0.1%)</td>
</tr>
</tbody>
</table>
</div>
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  namibia_hh_men_woman$felt_worthless and namibia_hh_men_woman$V025
## X-squared = 83.539, df = 1, p-value < 2.2e-16
```

```
## 
## 	Two Sample t-test
## 
## data:  namibia_hh_men_woman$nointerest_days by namibia_hh_men_woman$V025
## t = 3.0561, df = 14173, p-value = 0.002246
## alternative hypothesis: true difference in means between group Urban and group Rural is not equal to 0
## 95 percent confidence interval:
##  0.03107699 0.14223617
## sample estimates:
## mean in group Urban mean in group Rural 
##           0.6329728           0.5463162
```

```
## 
## 	Two Sample t-test
## 
## data:  namibia_hh_men_woman$lowenergy_days by namibia_hh_men_woman$V025
## t = 4.2402, df = 14188, p-value = 2.248e-05
## alternative hypothesis: true difference in means between group Urban and group Rural is not equal to 0
## 95 percent confidence interval:
##  0.06923058 0.18826465
## sample estimates:
## mean in group Urban mean in group Rural 
##           0.7018152           0.5730675
```




```
## # A tibble: 4 × 2
##   little_interst       mean
##   <fct>               <dbl>
## 1 No                   0   
## 2 Yes                  3.15
## 3 Dont know/not sure NaN   
## 4 <NA>               NaN
```

```
## # A tibble: 4 × 2
##   little_interst        sd
##   <fct>              <dbl>
## 1 No                  0   
## 2 Yes                 2.67
## 3 Dont know/not sure NA   
## 4 <NA>               NA
```

```
## # A tibble: 4 × 2
##   lowenergy_sad        mean
##   <fct>               <dbl>
## 1 No                   0   
## 2 Yes                  3.24
## 3 Dont know/not sure NaN   
## 4 <NA>               NaN
```

```
## # A tibble: 4 × 2
##   lowenergy_sad         sd
##   <fct>              <dbl>
## 1 No                  0   
## 2 Yes                 2.86
## 3 Dont know/not sure NA   
## 4 <NA>               NA
```




```r
ggplot(namibia_hh_men_woman, aes(x = Dim.1, y = Dim.2, col = gender)) + 
  geom_point(alpha = 0.5) + 
  scale_color_brewer(palette = "Dark2") +
  xlab("Cash Economy Assets") +
  ylab("Agricultural Economy Assets") +
  theme_minimal() 
```

![](Namibia-DHS-2013_files/figure-html/visuals-1.png)<!-- -->

```r
ggplot(namibia_hh_men_woman, aes(x = Dim.1, y = Dim.2, col = V025)) + 
  geom_point(alpha = 0.5) + 
  xlab("Cash Economy Assets") +
  ylab("Agricultural Economy Assets") +
  scale_colour_manual(values = c("#00BFC4", "#F8766D")) + 
  theme_classic() +
    geom_smooth(method=lm) #add linear trend line
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](Namibia-DHS-2013_files/figure-html/visuals-2.png)<!-- -->

```r
ggsave("Wealth Dims by Place.png")
```

```
## Saving 7 x 5 in image
## `geom_smooth()` using formula = 'y ~ x'
```

```r
p1 <- 
  ggplot(namibia_hh_men_woman, aes(x = lowenergy_sad, y = Dim.1, fill = gender)) +  
  geom_violin(trim = FALSE) + 
  theme_minimal() + 
    scale_fill_brewer(palette = "Dark2") + 
  xlab("Low Energy or Sad") + ylab("Cash Economy Wealth")

p1 + stat_summary(fun.y=mean, geom="point", shape=23, size=2)
```

```
## Warning: The `fun.y` argument of `stat_summary()` is deprecated as of ggplot2 3.3.0.
## ℹ Please use the `fun` argument instead.
```

![](Namibia-DHS-2013_files/figure-html/visuals-3.png)<!-- -->

```r
p2 <- 
  ggplot(namibia_hh_men_woman, aes(x = lowenergy_sad, y = Dim.2, fill = gender)) +  
  geom_violin(trim = FALSE) + 
  theme_minimal() + 
    scale_fill_brewer(palette = "Dark2") + 
  xlab("Low Energy or Sad") + ylab("Ag Economy Wealth")

p2 + stat_summary(fun.y=mean, geom="point", shape=23, size=2)
```

![](Namibia-DHS-2013_files/figure-html/visuals-4.png)<!-- -->

```r
ggplot(namibia_hh_men_woman, aes(x = lowenergy_sad, fill = V025)) +
    geom_bar(position = "fill") + 
    scale_fill_brewer(palette = "Dark2") + 
  xlab("Low Energy or Sad") +
  theme_minimal()
```

![](Namibia-DHS-2013_files/figure-html/visuals-5.png)<!-- -->

```r
ggplot(namibia_hh_men_woman, aes(x = felt_worthless, fill = V025)) +
    geom_bar(position = "fill") + 
    scale_fill_brewer(palette = "Dark2") + 
  xlab("Feelings of Worthlessness") +
  theme_minimal()
```

![](Namibia-DHS-2013_files/figure-html/visuals-6.png)<!-- -->

```r
ggplot(namibia_hh_men_woman, aes(x = little_interst, fill = V025)) +
    geom_bar(position = "fill") + 
    scale_fill_brewer(palette = "Dark2") + 
  xlab("Lack of Interest") +
  theme_minimal()
```

![](Namibia-DHS-2013_files/figure-html/visuals-7.png)<!-- -->

##### Analysis approaches to consider: Poisson regression can be used for count data (for instance, count of days of little interest or low energy, which we model here). However, if those data are overdispersed (conditional variance exceeds conditional mean), negative binomial regression may be more appropriate. Regardless of approach, wealth dimensions exhibit similar effects on days of little interest, with greater wealth in either dimensions associated with fewer days reported; however, the magnitude of effect is relatively greater of Dim2. As may be expected given this effect, rural residence is also associated with fewer days reported. Education is positively associated with days of no interest, with each successive education category associated with more days reported. The variable with the highest magnitude of effect in the model is sex. Sex-differentiation is also evident when considering low energy; however, the other effects are more mixed. In that model, Dim 1 and Dim2 differ in direction of effect, as they do with feeling worthless.

```r
#### Fit models
# install.packages("tidymodels")
# install.packages("poissonreg")

library(tidymodels)
```

```
## ── Attaching packages ────────────────────────────────────── tidymodels 0.2.0 ──
```

```
## ✔ broom        1.0.2     ✔ rsample      0.1.1
## ✔ dials        0.1.1     ✔ tune         0.2.0
## ✔ infer        1.0.0     ✔ workflows    0.2.6
## ✔ modeldata    0.1.1     ✔ workflowsets 0.2.1
## ✔ parsnip      0.2.1     ✔ yardstick    0.0.9
## ✔ recipes      1.0.4
```

```
## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
## ✖ scales::discard()        masks purrr::discard()
## ✖ dplyr::filter()          masks stats::filter()
## ✖ recipes::fixed()         masks stringr::fixed()
## ✖ yardstick::get_weights() masks jtools::get_weights()
## ✖ dplyr::lag()             masks stats::lag()
## ✖ yardstick::spec()        masks readr::spec()
## ✖ recipes::step()          masks stats::step()
## • Learn how to get started at https://www.tidymodels.org/start/
```

```r
library(poissonreg)

#### Model for feeling worthless

# Note: V012 = current age
log_spec <- logistic_reg() %>%
  set_engine("glm") %>%
  set_mode("classification") %>%
  fit(felt_worthless ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2 + gender*abuse , data = namibia_hh_men_woman)

#log linear first
log_lin_spec<-poisson_reg()

log_spec
```

```
## parsnip model object
## 
## 
## Call:  stats::glm(formula = felt_worthless ~ gender + V012 + V025 + 
##     V106 + gender + Dim.1 + Dim.2 + gender * abuse, family = stats::binomial, 
##     data = data)
## 
## Coefficients:
##      (Intercept)        gendermale              V012         V025Rural  
##        -1.853436         -0.619282         -0.004035         -0.236726  
##      V106Primary     V106Secondary        V106Higher             Dim.1  
##         0.208827          0.199269          0.132526          0.484345  
##            Dim.2             abuse  gendermale:abuse  
##        -0.638174          0.037372          0.003603  
## 
## Degrees of Freedom: 14182 Total (i.e. Null);  14172 Residual
##   (92 observations deleted due to missingness)
## Null Deviance:	    10200 
## Residual Deviance: 9968 	AIC: 9990
```

```r
class(log_spec)
```

```
## [1] "_glm"      "model_fit"
```

```r
tidy(log_spec)
```

```
## # A tibble: 11 × 5
##    term             estimate std.error statistic  p.value
##    <chr>               <dbl>     <dbl>     <dbl>    <dbl>
##  1 (Intercept)      -1.85      0.149    -12.4    1.61e-35
##  2 gendermale       -0.619     0.0728    -8.50   1.87e-17
##  3 V012             -0.00404   0.00234   -1.73   8.41e- 2
##  4 V025Rural        -0.237     0.0742    -3.19   1.43e- 3
##  5 V106Primary       0.209     0.121      1.73   8.37e- 2
##  6 V106Secondary     0.199     0.117      1.70   8.98e- 2
##  7 V106Higher        0.133     0.153      0.868  3.85e- 1
##  8 Dim.1             0.484     0.133      3.64   2.77e- 4
##  9 Dim.2            -0.638     0.138     -4.61   4.05e- 6
## 10 abuse             0.0374    0.0270     1.39   1.66e- 1
## 11 gendermale:abuse  0.00360   0.0676     0.0533 9.57e- 1
```

```r
library(sjPlot)
tab_model(log_spec)
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Dependent variable</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Odds Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.16</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.12&nbsp;&ndash;&nbsp;0.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.47&nbsp;&ndash;&nbsp;0.62</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.084</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.79</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.68&nbsp;&ndash;&nbsp;0.91</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.23</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.97&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.084</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.22</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.97&nbsp;&ndash;&nbsp;1.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.090</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.14</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85&nbsp;&ndash;&nbsp;1.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.385</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.62</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.25&nbsp;&ndash;&nbsp;2.11</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.53</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.40&nbsp;&ndash;&nbsp;0.69</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.04</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.166</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.88&nbsp;&ndash;&nbsp;1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.957</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14183</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Tjur</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.016</td>
</tr>

</table>

```r
namibia_hh_men_woman %>%
  ggplot(aes(x=nointerest_days)) + geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 100 rows containing non-finite values (`stat_bin()`).
```

![](Namibia-DHS-2013_files/figure-html/fit models-1.png)<!-- -->

```r
namibia_hh_men_woman %>%
  ggplot(aes(x=lowenergy_days)) + geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 85 rows containing non-finite values (`stat_bin()`).
```

![](Namibia-DHS-2013_files/figure-html/fit models-2.png)<!-- -->

```r
# Note: V012 = current age
#### Feelings of Worthlessness
log_spec_interact <- logistic_reg() %>%
  set_engine("glm") %>%
  set_mode("classification") %>%
  fit(felt_worthless ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2 + gender*abuse + gender*Dim.1 + gender*Dim.2, data = namibia_hh_men_woman)

tidy(log_spec_interact)
```

```
## # A tibble: 13 × 5
##    term             estimate std.error statistic  p.value
##    <chr>               <dbl>     <dbl>     <dbl>    <dbl>
##  1 (Intercept)      -1.88      0.150     -12.6   3.23e-36
##  2 gendermale       -0.593     0.0741     -8.01  1.19e-15
##  3 V012             -0.00383   0.00234    -1.64  1.01e- 1
##  4 V025Rural        -0.234     0.0742     -3.16  1.60e- 3
##  5 V106Primary       0.218     0.121       1.80  7.14e- 2
##  6 V106Secondary     0.212     0.118       1.80  7.15e- 2
##  7 V106Higher        0.142     0.153       0.932 3.51e- 1
##  8 Dim.1             0.594     0.142       4.18  2.93e- 5
##  9 Dim.2            -0.569     0.154      -3.70  2.14e- 4
## 10 abuse             0.0460    0.0272      1.69  9.12e- 2
## 11 gendermale:abuse -0.0163    0.0683     -0.238 8.12e- 1
## 12 gendermale:Dim.1 -0.510     0.236      -2.16  3.05e- 2
## 13 gendermale:Dim.2 -0.323     0.312      -1.04  3.00e- 1
```

```r
tab_model(log_spec_interact)
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Dependent variable</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Odds Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.11&nbsp;&ndash;&nbsp;0.20</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.55</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.48&nbsp;&ndash;&nbsp;0.64</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.101</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.79</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.68&nbsp;&ndash;&nbsp;0.92</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.002</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.24</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.57</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.071</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.24</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.071</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.351</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.81</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.37&nbsp;&ndash;&nbsp;2.39</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.57</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.42&nbsp;&ndash;&nbsp;0.76</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.05</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.091</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.86&nbsp;&ndash;&nbsp;1.12</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.812</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.38&nbsp;&ndash;&nbsp;0.95</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.030</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.39&nbsp;&ndash;&nbsp;1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.300</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14183</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Tjur</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.016</td>
</tr>

</table>

```r
#### Feelings of worthlessness with interactions
log_spec_interact2 <- logistic_reg() %>%
  set_engine("glm") %>%
  set_mode("classification") %>%
  fit(felt_worthless ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2 + gender*abuse +  gender*V025, data = namibia_hh_men_woman)

tidy(log_spec_interact2)
```

```
## # A tibble: 12 × 5
##    term                  estimate std.error statistic  p.value
##    <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
##  1 (Intercept)          -1.85       0.149    -12.4    1.89e-35
##  2 gendermale           -0.655      0.0891    -7.35   2.00e-13
##  3 V012                 -0.00398    0.00234   -1.70   8.88e- 2
##  4 V025Rural            -0.257      0.0797    -3.22   1.27e- 3
##  5 V106Primary           0.211      0.121      1.75   8.09e- 2
##  6 V106Secondary         0.203      0.118      1.73   8.40e- 2
##  7 V106Higher            0.136      0.153      0.891  3.73e- 1
##  8 Dim.1                 0.483      0.133      3.62   2.93e- 4
##  9 Dim.2                -0.639      0.138     -4.62   3.87e- 6
## 10 abuse                 0.0391     0.0271     1.44   1.49e- 1
## 11 gendermale:abuse     -0.000989   0.0680    -0.0146 9.88e- 1
## 12 gendermale:V025Rural  0.0931     0.133      0.699  4.85e- 1
```

```r
tab_model(log_spec, log_spec_interact, log_spec_interact2, dv.labels = c("Worthlessness1", "Worthlessness2", "Worthlessness3"))
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Worthlessness1</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Worthlessness2</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Worthlessness3</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Odds Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Odds Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col7">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col8">Odds Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col9">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  0">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.16</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.12&nbsp;&ndash;&nbsp;0.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.11&nbsp;&ndash;&nbsp;0.20</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.16</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.12&nbsp;&ndash;&nbsp;0.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.47&nbsp;&ndash;&nbsp;0.62</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.55</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.48&nbsp;&ndash;&nbsp;0.64</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.52</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.44&nbsp;&ndash;&nbsp;0.62</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.084</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.101</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.089</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.79</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.68&nbsp;&ndash;&nbsp;0.91</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.79</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.68&nbsp;&ndash;&nbsp;0.92</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.002</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.77</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.66&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.23</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.97&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.084</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.24</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.57</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.071</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.23</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.97&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.081</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.22</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.97&nbsp;&ndash;&nbsp;1.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.090</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.24</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.071</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.23</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.97&nbsp;&ndash;&nbsp;1.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.084</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.14</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85&nbsp;&ndash;&nbsp;1.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.385</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85&nbsp;&ndash;&nbsp;1.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.351</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.85&nbsp;&ndash;&nbsp;1.55</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.373</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.62</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.25&nbsp;&ndash;&nbsp;2.11</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.81</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.37&nbsp;&ndash;&nbsp;2.39</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.62</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.25&nbsp;&ndash;&nbsp;2.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.53</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.40&nbsp;&ndash;&nbsp;0.69</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.57</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.42&nbsp;&ndash;&nbsp;0.76</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.53</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.40&nbsp;&ndash;&nbsp;0.69</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.04</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.166</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.05</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.091</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.04</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.99&nbsp;&ndash;&nbsp;1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.149</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.88&nbsp;&ndash;&nbsp;1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.957</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.86&nbsp;&ndash;&nbsp;1.12</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.812</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.87&nbsp;&ndash;&nbsp;1.14</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.988</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.38&nbsp;&ndash;&nbsp;0.95</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.030</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.39&nbsp;&ndash;&nbsp;1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.300</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * V025<br>[Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.85&nbsp;&ndash;&nbsp;1.42</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.485</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14183</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14183</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14183</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Tjur</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.016</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.016</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.016</td>
</tr>

</table>

```r
#### No interest - poisson
log_lin_fit_nointerest <-
  log_lin_spec %>%
  fit(nointerest_days ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2 + gender*abuse, data = namibia_hh_men_woman)
log_lin_fit_nointerest
```

```
## parsnip model object
## 
## 
## Call:  stats::glm(formula = nointerest_days ~ gender + V012 + V025 + 
##     V106 + gender + Dim.1 + Dim.2 + gender * abuse, family = stats::poisson, 
##     data = data)
## 
## Coefficients:
##      (Intercept)        gendermale              V012         V025Rural  
##        -0.517341         -0.654115         -0.003258         -0.162347  
##      V106Primary     V106Secondary        V106Higher             Dim.1  
##         0.281367          0.285898          0.386818         -0.119771  
##            Dim.2             abuse  gendermale:abuse  
##        -0.208838          0.067361          0.139961  
## 
## Degrees of Freedom: 14166 Total (i.e. Null);  14156 Residual
##   (108 observations deleted due to missingness)
## Null Deviance:	    32680 
## Residual Deviance: 31890 	AIC: 39350
```

```r
log_lin_fit_nointerest %>% tidy()
```

```
## # A tibble: 11 × 5
##    term             estimate std.error statistic  p.value
##    <chr>               <dbl>     <dbl>     <dbl>    <dbl>
##  1 (Intercept)      -0.517    0.0622       -8.32 8.59e-17
##  2 gendermale       -0.654    0.0320      -20.4  1.01e-92
##  3 V012             -0.00326  0.000966     -3.37 7.48e- 4
##  4 V025Rural        -0.162    0.0308       -5.27 1.40e- 7
##  5 V106Primary       0.281    0.0500        5.62 1.88e- 8
##  6 V106Secondary     0.286    0.0490        5.83 5.42e- 9
##  7 V106Higher        0.387    0.0639        6.06 1.38e- 9
##  8 Dim.1            -0.120    0.0558       -2.15 3.19e- 2
##  9 Dim.2            -0.209    0.0556       -3.75 1.74e- 4
## 10 abuse             0.0674   0.0105        6.43 1.26e-10
## 11 gendermale:abuse  0.140    0.0248        5.65 1.59e- 8
```

```r
tidy(log_lin_fit_nointerest, conf.int = T, conf.level = .9)
```

```
## # A tibble: 11 × 7
##    term             estimate std.error statistic  p.value conf.low conf.high
##    <chr>               <dbl>     <dbl>     <dbl>    <dbl>    <dbl>     <dbl>
##  1 (Intercept)      -0.517    0.0622       -8.32 8.59e-17 -0.620    -0.416  
##  2 gendermale       -0.654    0.0320      -20.4  1.01e-92 -0.707    -0.602  
##  3 V012             -0.00326  0.000966     -3.37 7.48e- 4 -0.00485  -0.00167
##  4 V025Rural        -0.162    0.0308       -5.27 1.40e- 7 -0.213    -0.112  
##  5 V106Primary       0.281    0.0500        5.62 1.88e- 8  0.200     0.364  
##  6 V106Secondary     0.286    0.0490        5.83 5.42e- 9  0.206     0.367  
##  7 V106Higher        0.387    0.0639        6.06 1.38e- 9  0.282     0.492  
##  8 Dim.1            -0.120    0.0558       -2.15 3.19e- 2 -0.212    -0.0280 
##  9 Dim.2            -0.209    0.0556       -3.75 1.74e- 4 -0.301    -0.118  
## 10 abuse             0.0674   0.0105        6.43 1.26e-10  0.0501    0.0845 
## 11 gendermale:abuse  0.140    0.0248        5.65 1.59e- 8  0.0989    0.180
```

```r
#### No interest with interactions
log_lin_fit_nointerest_interact <-
  log_lin_spec %>%
  fit(nointerest_days ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2 + gender*abuse + gender*Dim.1 + gender*Dim.2, data = namibia_hh_men_woman)

tidy(log_lin_fit_nointerest_interact, conf.int = T, conf.level = .9)
```

```
## # A tibble: 13 × 7
##    term             estimate std.error statistic  p.value conf.low conf.high
##    <chr>               <dbl>     <dbl>     <dbl>    <dbl>    <dbl>     <dbl>
##  1 (Intercept)      -0.552    0.0625      -8.84  9.75e-19 -0.655    -0.450  
##  2 gendermale       -0.645    0.0323     -20.0   1.14e-88 -0.698    -0.592  
##  3 V012             -0.00295  0.000968    -3.04  2.33e- 3 -0.00454  -0.00136
##  4 V025Rural        -0.161    0.0308      -5.21  1.90e- 7 -0.211    -0.110  
##  5 V106Primary       0.292    0.0501       5.83  5.43e- 9  0.210     0.375  
##  6 V106Secondary     0.302    0.0491       6.15  7.65e-10  0.222     0.383  
##  7 V106Higher        0.398    0.0639       6.22  4.83e-10  0.293     0.503  
##  8 Dim.1             0.0238   0.0590       0.403 6.87e- 1 -0.0733    0.121  
##  9 Dim.2            -0.142    0.0613      -2.32  2.03e- 2 -0.244    -0.0418 
## 10 abuse             0.0782   0.0106       7.40  1.38e-13  0.0608    0.0955 
## 11 gendermale:abuse  0.114    0.0251       4.52  6.15e- 6  0.0719    0.155  
## 12 gendermale:Dim.1 -0.748    0.103       -7.28  3.30e-13 -0.917    -0.579  
## 13 gendermale:Dim.2 -0.342    0.128       -2.68  7.31e- 3 -0.553    -0.133
```

```r
log_lin_fit_nointerest_interact2 <-
  log_lin_spec %>%
  fit(nointerest_days ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2 + gender*abuse + gender*V025, data = namibia_hh_men_woman)

tidy(log_lin_fit_nointerest_interact2, conf.int = T, conf.level = .9)
```

```
## # A tibble: 12 × 7
##    term                 estimate std.error statistic  p.value conf.low conf.high
##    <chr>                   <dbl>     <dbl>     <dbl>    <dbl>    <dbl>     <dbl>
##  1 (Intercept)          -0.512    0.0621       -8.24 1.77e-16 -0.615    -0.410  
##  2 gendermale           -0.778    0.0416      -18.7  4.71e-78 -0.846    -0.710  
##  3 V012                 -0.00306  0.000967     -3.17 1.55e- 3 -0.00465  -0.00147
##  4 V025Rural            -0.217    0.0329       -6.61 3.74e-11 -0.272    -0.163  
##  5 V106Primary           0.287    0.0501        5.74 9.52e- 9  0.206     0.370  
##  6 V106Secondary         0.298    0.0491        6.06 1.33e- 9  0.218     0.379  
##  7 V106Higher            0.397    0.0639        6.22 5.06e-10  0.292     0.503  
##  8 Dim.1                -0.124    0.0559       -2.23 2.59e- 2 -0.216    -0.0326 
##  9 Dim.2                -0.213    0.0556       -3.82 1.32e- 4 -0.304    -0.121  
## 10 abuse                 0.0720   0.0105        6.85 7.48e-12  0.0546    0.0892 
## 11 gendermale:abuse      0.127    0.0250        5.10 3.47e- 7  0.0858    0.168  
## 12 gendermale:V025Rural  0.273    0.0558        4.89 1.01e- 6  0.181     0.365
```

```r
tab_model(log_lin_fit_nointerest, log_lin_fit_nointerest_interact, log_lin_fit_nointerest_interact2, dv.labels = c("Low Interest1", "LowInterest2", "LowInterest3"))
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Low Interest1</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">LowInterest2</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">LowInterest3</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col7">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col8">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col9">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  0">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.53&nbsp;&ndash;&nbsp;0.67</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.58</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.51&nbsp;&ndash;&nbsp;0.65</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.53&nbsp;&ndash;&nbsp;0.68</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.52</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.49&nbsp;&ndash;&nbsp;0.55</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.52</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.49&nbsp;&ndash;&nbsp;0.56</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.46</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.42&nbsp;&ndash;&nbsp;0.50</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.002</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.00&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>0.002</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.80</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.75&nbsp;&ndash;&nbsp;0.86</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.32</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.20&nbsp;&ndash;&nbsp;1.46</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.34</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.21&nbsp;&ndash;&nbsp;1.48</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.21&nbsp;&ndash;&nbsp;1.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.21&nbsp;&ndash;&nbsp;1.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.35</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.23&nbsp;&ndash;&nbsp;1.49</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.35</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.22&nbsp;&ndash;&nbsp;1.48</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.30&nbsp;&ndash;&nbsp;1.67</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.49</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.31&nbsp;&ndash;&nbsp;1.69</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.49</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.31&nbsp;&ndash;&nbsp;1.69</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.89</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80&nbsp;&ndash;&nbsp;0.99</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.032</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.02</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.91&nbsp;&ndash;&nbsp;1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.687</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.88</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.79&nbsp;&ndash;&nbsp;0.99</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>0.026</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.81</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.73&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.87</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.77&nbsp;&ndash;&nbsp;0.98</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.020</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.81</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.72&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.07</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.05&nbsp;&ndash;&nbsp;1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.08</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.06&nbsp;&ndash;&nbsp;1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.07</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.05&nbsp;&ndash;&nbsp;1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.10&nbsp;&ndash;&nbsp;1.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.12</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.07&nbsp;&ndash;&nbsp;1.18</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.14</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.08&nbsp;&ndash;&nbsp;1.19</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.39&nbsp;&ndash;&nbsp;0.58</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.71</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.55&nbsp;&ndash;&nbsp;0.91</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.007</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * V025<br>[Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.31</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.18&nbsp;&ndash;&nbsp;1.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14167</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14167</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14167</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Nagelkerke</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.060</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.065</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.062</td>
</tr>

</table>

```r
##### Low energy- poisson
log_lin_fit_lowenergy <-
  log_lin_spec %>%
  fit(lowenergy_days ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2  + gender*abuse, data = namibia_hh_men_woman)
log_lin_fit_lowenergy
```

```
## parsnip model object
## 
## 
## Call:  stats::glm(formula = lowenergy_days ~ gender + V012 + V025 + 
##     V106 + gender + Dim.1 + Dim.2 + gender * abuse, family = stats::poisson, 
##     data = data)
## 
## Coefficients:
##      (Intercept)        gendermale              V012         V025Rural  
##       -0.5081989        -0.6828372         0.0013111        -0.1647963  
##      V106Primary     V106Secondary        V106Higher             Dim.1  
##        0.2864148         0.2118292         0.1756976         0.0595455  
##            Dim.2             abuse  gendermale:abuse  
##       -0.3234019         0.0758703         0.0006532  
## 
## Degrees of Freedom: 14181 Total (i.e. Null);  14171 Residual
##   (93 observations deleted due to missingness)
## Null Deviance:	    34920 
## Residual Deviance: 33910 	AIC: 41770
```

```r
log_lin_fit_lowenergy %>% tidy()
```

```
## # A tibble: 11 × 5
##    term              estimate std.error statistic   p.value
##    <chr>                <dbl>     <dbl>     <dbl>     <dbl>
##  1 (Intercept)      -0.508     0.0590     -8.61   7.00e- 18
##  2 gendermale       -0.683     0.0310    -22.0    3.00e-107
##  3 V012              0.00131   0.000913    1.44   1.51e-  1
##  4 V025Rural        -0.165     0.0295     -5.58   2.44e-  8
##  5 V106Primary       0.286     0.0469      6.11   9.93e- 10
##  6 V106Secondary     0.212     0.0462      4.58   4.60e-  6
##  7 V106Higher        0.176     0.0617      2.85   4.43e-  3
##  8 Dim.1             0.0595    0.0535      1.11   2.66e-  1
##  9 Dim.2            -0.323     0.0542     -5.97   2.35e-  9
## 10 abuse             0.0759    0.00990     7.66   1.83e- 14
## 11 gendermale:abuse  0.000653  0.0274      0.0238 9.81e-  1
```

```r
tidy(log_lin_fit_lowenergy, conf.int = T, conf.level = .9)
```

```
## # A tibble: 11 × 7
##    term              estimate std.error statistic   p.value  conf.low conf.high
##    <chr>                <dbl>     <dbl>     <dbl>     <dbl>     <dbl>     <dbl>
##  1 (Intercept)      -0.508     0.0590     -8.61   7.00e- 18 -0.606     -0.412  
##  2 gendermale       -0.683     0.0310    -22.0    3.00e-107 -0.734     -0.632  
##  3 V012              0.00131   0.000913    1.44   1.51e-  1 -0.000193   0.00281
##  4 V025Rural        -0.165     0.0295     -5.58   2.44e-  8 -0.213     -0.116  
##  5 V106Primary       0.286     0.0469      6.11   9.93e- 10  0.210      0.364  
##  6 V106Secondary     0.212     0.0462      4.58   4.60e-  6  0.136      0.289  
##  7 V106Higher        0.176     0.0617      2.85   4.43e-  3  0.0742     0.277  
##  8 Dim.1             0.0595    0.0535      1.11   2.66e-  1 -0.0285     0.148  
##  9 Dim.2            -0.323     0.0542     -5.97   2.35e-  9 -0.413     -0.235  
## 10 abuse             0.0759    0.00990     7.66   1.83e- 14  0.0595     0.0921 
## 11 gendermale:abuse  0.000653  0.0274      0.0238 9.81e-  1 -0.0450     0.0453
```

```r
#### Low energy interaction effects
log_lin_fit_lowenergy_interact <-
  log_lin_spec %>%
  fit(lowenergy_days ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2  + gender*abuse + gender*Dim.1 + gender*Dim.2, data = namibia_hh_men_woman)
log_lin_fit_lowenergy_interact
```

```
## parsnip model object
## 
## 
## Call:  stats::glm(formula = lowenergy_days ~ gender + V012 + V025 + 
##     V106 + gender + Dim.1 + Dim.2 + gender * abuse + gender * 
##     Dim.1 + gender * Dim.2, family = stats::poisson, data = data)
## 
## Coefficients:
##      (Intercept)        gendermale              V012         V025Rural  
##        -0.541194         -0.669776          0.001639         -0.163788  
##      V106Primary     V106Secondary        V106Higher             Dim.1  
##         0.295348          0.226678          0.188669          0.184081  
##            Dim.2             abuse  gendermale:abuse  gendermale:Dim.1  
##        -0.329222          0.085079         -0.024569         -0.703997  
## gendermale:Dim.2  
##         0.010987  
## 
## Degrees of Freedom: 14181 Total (i.e. Null);  14169 Residual
##   (93 observations deleted due to missingness)
## Null Deviance:	    34920 
## Residual Deviance: 33860 	AIC: 41730
```

```r
log_lin_fit_lowenergy_interact2 <-
  log_lin_spec %>%
  fit(lowenergy_days ~ gender + V012 + V025 + V106 +  gender + Dim.1 + Dim.2  + gender*abuse + gender*V025, data = namibia_hh_men_woman)
log_lin_fit_lowenergy_interact2
```

```
## parsnip model object
## 
## 
## Call:  stats::glm(formula = lowenergy_days ~ gender + V012 + V025 + 
##     V106 + gender + Dim.1 + Dim.2 + gender * abuse + gender * 
##     V025, family = stats::poisson, data = data)
## 
## Coefficients:
##          (Intercept)            gendermale                  V012  
##            -0.501538             -0.867560              0.001564  
##            V025Rural           V106Primary         V106Secondary  
##            -0.241095              0.295339              0.228438  
##           V106Higher                 Dim.1                 Dim.2  
##             0.191248              0.052161             -0.328368  
##                abuse      gendermale:abuse  gendermale:V025Rural  
##             0.082185             -0.018141              0.408300  
## 
## Degrees of Freedom: 14181 Total (i.e. Null);  14170 Residual
##   (93 observations deleted due to missingness)
## Null Deviance:	    34920 
## Residual Deviance: 33860 	AIC: 41720
```

```r
tab_model(log_lin_fit_lowenergy, log_lin_fit_lowenergy_interact, log_lin_fit_lowenergy_interact2, dv.labels = c("Low Energy1", "LowEnergy2", "LowEnergy3"))
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">Low Energy1</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">LowEnergy2</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">LowEnergy3</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col7">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col8">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col9">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  0">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.54&nbsp;&ndash;&nbsp;0.68</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.58</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.52&nbsp;&ndash;&nbsp;0.65</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.61</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.54&nbsp;&ndash;&nbsp;0.68</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.51</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.48&nbsp;&ndash;&nbsp;0.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.51</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.48&nbsp;&ndash;&nbsp;0.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.42</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.39&nbsp;&ndash;&nbsp;0.46</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.151</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.073</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.00&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.087</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80&nbsp;&ndash;&nbsp;0.90</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.79</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.74&nbsp;&ndash;&nbsp;0.84</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.21&nbsp;&ndash;&nbsp;1.46</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.34</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.23&nbsp;&ndash;&nbsp;1.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.34</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.23&nbsp;&ndash;&nbsp;1.47</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.24</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.13&nbsp;&ndash;&nbsp;1.35</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.25</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.15&nbsp;&ndash;&nbsp;1.37</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.26</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.15&nbsp;&ndash;&nbsp;1.38</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.19</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.06&nbsp;&ndash;&nbsp;1.35</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.004</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.07&nbsp;&ndash;&nbsp;1.36</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.002</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.07&nbsp;&ndash;&nbsp;1.37</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>0.002</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.06</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.96&nbsp;&ndash;&nbsp;1.18</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.266</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.20</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.08&nbsp;&ndash;&nbsp;1.34</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.05</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.95&nbsp;&ndash;&nbsp;1.17</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.330</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.65&nbsp;&ndash;&nbsp;0.80</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.64&nbsp;&ndash;&nbsp;0.81</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.65&nbsp;&ndash;&nbsp;0.80</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.08</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.06&nbsp;&ndash;&nbsp;1.10</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.07&nbsp;&ndash;&nbsp;1.11</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.06&nbsp;&ndash;&nbsp;1.11</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.95&nbsp;&ndash;&nbsp;1.06</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.981</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.92&nbsp;&ndash;&nbsp;1.03</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.377</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">0.98</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">0.93&nbsp;&ndash;&nbsp;1.04</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0">0.512</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.49</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.41&nbsp;&ndash;&nbsp;0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.01</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.79&nbsp;&ndash;&nbsp;1.29</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.930</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * V025<br>[Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col8">1.50</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col9">1.35&nbsp;&ndash;&nbsp;1.68</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  0"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14182</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14182</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14182</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Nagelkerke</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.075</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.079</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.079</td>
</tr>

</table>

```r
# Negative binomial
library(MASS)
```

```
## 
## Attaching package: 'MASS'
```

```
## The following object is masked from 'package:dplyr':
## 
##     select
```

```r
summary(m1 <- glm.nb(nointerest_days ~ gender + V012 + V025 + V106 +  gender*abuse + Dim.1 + Dim.2 , data = namibia_hh_men_woman))
```

```
## 
## Call:
## glm.nb(formula = nointerest_days ~ gender + V012 + V025 + V106 + 
##     gender * abuse + Dim.1 + Dim.2, data = namibia_hh_men_woman, 
##     init.theta = 0.122992938, link = log)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.7552  -0.6836  -0.6491  -0.5520   2.8462  
## 
## Coefficients:
##                   Estimate Std. Error z value Pr(>|z|)    
## (Intercept)      -0.507283   0.142024  -3.572 0.000355 ***
## gendermale       -0.684368   0.066959 -10.221  < 2e-16 ***
## V012             -0.004910   0.002294  -2.140 0.032344 *  
## V025Rural        -0.154771   0.074449  -2.079 0.037627 *  
## V106Primary       0.319616   0.110515   2.892 0.003827 ** 
## V106Secondary     0.338578   0.107817   3.140 0.001688 ** 
## V106Higher        0.419234   0.147966   2.833 0.004607 ** 
## abuse             0.067055   0.027976   2.397 0.016534 *  
## Dim.1            -0.181103   0.134992  -1.342 0.179732    
## Dim.2            -0.276512   0.131529  -2.102 0.035527 *  
## gendermale:abuse  0.163238   0.059738   2.733 0.006285 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(0.123) family taken to be 1)
## 
##     Null deviance: 6676.5  on 14166  degrees of freedom
## Residual deviance: 6520.3  on 14156  degrees of freedom
##   (108 observations deleted due to missingness)
## AIC: 24219
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  0.12299 
##           Std. Err.:  0.00346 
## 
##  2 x log-likelihood:  -24194.74300
```

```r
summary(m2 <- glm.nb(lowenergy_days ~ gender + V012 + V025 + V106 +  gender*abuse + Dim.1 + Dim.2 , data = namibia_hh_men_woman))
```

```
## 
## Call:
## glm.nb(formula = lowenergy_days ~ gender + V012 + V025 + V106 + 
##     gender * abuse + Dim.1 + Dim.2, data = namibia_hh_men_woman, 
##     init.theta = 0.1277318483, link = log)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.7745  -0.7062  -0.6629  -0.5662   3.3842  
## 
## Coefficients:
##                   Estimate Std. Error z value Pr(>|z|)    
## (Intercept)      -0.527989   0.138389  -3.815 0.000136 ***
## gendermale       -0.676975   0.065286 -10.369  < 2e-16 ***
## V012              0.001143   0.002232   0.512 0.608617    
## V025Rural        -0.139335   0.072703  -1.916 0.055302 .  
## V106Primary       0.282998   0.107260   2.638 0.008329 ** 
## V106Secondary     0.221871   0.104795   2.117 0.034244 *  
## V106Higher        0.260626   0.144124   1.808 0.070552 .  
## abuse             0.075195   0.027261   2.758 0.005810 ** 
## Dim.1             0.029114   0.131839   0.221 0.825222    
## Dim.2            -0.330979   0.128643  -2.573 0.010087 *  
## gendermale:abuse  0.011186   0.059867   0.187 0.851784    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(0.1277) family taken to be 1)
## 
##     Null deviance: 6959.3  on 14181  degrees of freedom
## Residual deviance: 6782.1  on 14171  degrees of freedom
##   (93 observations deleted due to missingness)
## AIC: 25269
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  0.12773 
##           Std. Err.:  0.00349 
## 
##  2 x log-likelihood:  -25244.86600
```

```r
summary(m3 <- glm.nb(lowenergy_days ~  V012 + V025 + V106 + abuse + Dim.1 + Dim.2 , data = namibia_hh_men_woman, subset = gender == "female"))
```

```
## 
## Call:
## glm.nb(formula = lowenergy_days ~ V012 + V025 + V106 + abuse + 
##     Dim.1 + Dim.2, data = namibia_hh_men_woman, subset = gender == 
##     "female", init.theta = 0.1476676387, link = log)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.8310  -0.7484  -0.7112  -0.6641   2.3974  
## 
## Coefficients:
##                Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -0.540544   0.157721  -3.427 0.000610 ***
## V012           0.003832   0.002527   1.517 0.129337    
## V025Rural     -0.227078   0.081197  -2.797 0.005164 ** 
## V106Primary    0.258803   0.125129   2.068 0.038613 *  
## V106Secondary  0.194546   0.122116   1.593 0.111132    
## V106Higher    -0.049408   0.165998  -0.298 0.765976    
## abuse          0.089224   0.026015   3.430 0.000604 ***
## Dim.1          0.206965   0.146851   1.409 0.158732    
## Dim.2         -0.279367   0.144710  -1.931 0.053542 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(0.1477) family taken to be 1)
## 
##     Null deviance: 5348.7  on 9826  degrees of freedom
## Residual deviance: 5301.1  on 9818  degrees of freedom
##   (49 observations deleted due to missingness)
## AIC: 19817
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  0.14767 
##           Std. Err.:  0.00449 
## 
##  2 x log-likelihood:  -19796.57000
```

```r
summary(m4 <- glm.nb(lowenergy_days ~  V012 + V025 + V106 + abuse + Dim.1 + Dim.2 , data = namibia_hh_men_woman, subset = gender == "male"))
```

```
## 
## Call:
## glm.nb(formula = lowenergy_days ~ V012 + V025 + V106 + abuse + 
##     Dim.1 + Dim.2, data = namibia_hh_men_woman, subset = gender == 
##     "male", init.theta = 0.07666594137, link = log)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.6373  -0.5330  -0.5101  -0.4789   3.0829  
## 
## Coefficients:
##                Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -1.240548   0.290561  -4.269 1.96e-05 ***
## V012          -0.005047   0.004977  -1.014  0.31064    
## V025Rural      0.171935   0.166234   1.034  0.30100    
## V106Primary    0.287735   0.221359   1.300  0.19365    
## V106Secondary  0.242857   0.217839   1.115  0.26492    
## V106Higher     0.982298   0.309021   3.179  0.00148 ** 
## abuse          0.052396   0.067359   0.778  0.43665    
## Dim.1         -0.407485   0.304307  -1.339  0.18055    
## Dim.2         -0.650633   0.290023  -2.243  0.02487 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(0.0767) family taken to be 1)
## 
##     Null deviance: 1422.9  on 4354  degrees of freedom
## Residual deviance: 1403.2  on 4346  degrees of freedom
##   (44 observations deleted due to missingness)
## AIC: 5337.2
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  0.07667 
##           Std. Err.:  0.00467 
## 
##  2 x log-likelihood:  -5317.21700
```

```r
tab_model(m3, m4)
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">lowenergy days</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">lowenergy days</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  col7">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.58</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.43&nbsp;&ndash;&nbsp;0.80</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.29</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.17&nbsp;&ndash;&nbsp;0.50</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00&nbsp;&ndash;&nbsp;1.01</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.129</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.311</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.68&nbsp;&ndash;&nbsp;0.93</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.005</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.19</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85&nbsp;&ndash;&nbsp;1.67</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.301</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.30</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.01&nbsp;&ndash;&nbsp;1.65</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.039</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.85&nbsp;&ndash;&nbsp;2.05</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.194</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.21</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.95&nbsp;&ndash;&nbsp;1.54</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.111</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.27</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.83&nbsp;&ndash;&nbsp;1.92</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.265</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.95</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.69&nbsp;&ndash;&nbsp;1.32</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.766</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">2.67</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.47&nbsp;&ndash;&nbsp;4.92</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.04&nbsp;&ndash;&nbsp;1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.001</strong></td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.05</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.92&nbsp;&ndash;&nbsp;1.22</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.437</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.23</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.92&nbsp;&ndash;&nbsp;1.64</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.159</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.67</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.37&nbsp;&ndash;&nbsp;1.19</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7">0.181</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.76</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.58&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.054</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.52</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.29&nbsp;&ndash;&nbsp;0.95</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  col7"><strong>0.025</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">9827</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">4355</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Nagelkerke</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.016</td>
</tr>

</table>

```r
tab_model(m1)
```

```
## Profiled confidence intervals may take longer time to compute. Use
##   'ci_method="wald"' for faster computation of CIs.
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">nointerest days</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.60</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.46&nbsp;&ndash;&nbsp;0.80</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.50</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.44&nbsp;&ndash;&nbsp;0.58</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.99&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.032</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.86</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.74&nbsp;&ndash;&nbsp;0.99</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.038</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.38</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.11&nbsp;&ndash;&nbsp;1.70</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.004</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.40</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.13&nbsp;&ndash;&nbsp;1.73</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.002</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.52</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.14&nbsp;&ndash;&nbsp;2.04</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.005</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.07</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.01&nbsp;&ndash;&nbsp;1.13</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.017</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.83</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.64&nbsp;&ndash;&nbsp;1.09</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.180</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.76</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.59&nbsp;&ndash;&nbsp;0.98</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.036</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.18</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.04&nbsp;&ndash;&nbsp;1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.006</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14167</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Nagelkerke</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.029</td>
</tr>

</table>

```r
tab_model(m2)
```

```
## Profiled confidence intervals may take longer time to compute. Use
##   'ci_method="wald"' for faster computation of CIs.
```

<table style="border-collapse:collapse; border:none;">
<tr>
<th style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm;  text-align:left; ">&nbsp;</th>
<th colspan="3" style="border-top: double; text-align:center; font-style:normal; font-weight:bold; padding:0.2cm; ">lowenergy days</th>
</tr>
<tr>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  text-align:left; ">Predictors</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">Incidence Rate Ratios</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">CI</td>
<td style=" text-align:center; border-bottom:1px solid; font-style:italic; font-weight:normal;  ">p</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">(Intercept)</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.59</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.45&nbsp;&ndash;&nbsp;0.77</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.51</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.45&nbsp;&ndash;&nbsp;0.58</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>&lt;0.001</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V012</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.00&nbsp;&ndash;&nbsp;1.01</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.609</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V025 [Rural]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.87</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.75&nbsp;&ndash;&nbsp;1.00</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.055</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Primary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.07&nbsp;&ndash;&nbsp;1.64</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.008</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Secondary]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.25</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.01&nbsp;&ndash;&nbsp;1.53</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.034</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">V106 [Higher]</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.30</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.98&nbsp;&ndash;&nbsp;1.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.071</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.08</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.02&nbsp;&ndash;&nbsp;1.14</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.006</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 1</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.03</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.80&nbsp;&ndash;&nbsp;1.33</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.825</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">Dim 2</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.72</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.56&nbsp;&ndash;&nbsp;0.92</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  "><strong>0.010</strong></td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; ">gender [male] * abuse</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">1.01</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.90&nbsp;&ndash;&nbsp;1.15</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:center;  ">0.852</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm; border-top:1px solid;">Observations</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left; border-top:1px solid;" colspan="3">14182</td>
</tr>
<tr>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; text-align:left; padding-top:0.1cm; padding-bottom:0.1cm;">R<sup>2</sup> Nagelkerke</td>
<td style=" padding:0.2cm; text-align:left; vertical-align:top; padding-top:0.1cm; padding-bottom:0.1cm; text-align:left;" colspan="3">0.032</td>
</tr>

</table>

```r
mean(namibia_hh_men_woman$nointerest_days, na.rm = T)
```

```
## [1] 0.5904056
```

```r
var(namibia_hh_men_woman$nointerest_days, na.rm = T)
```

```
## [1] 2.85
```

```r
sd(namibia_hh_men_woman$nointerest_days, na.rm = T)
```

```
## [1] 1.688194
```

```r
mean(namibia_hh_men_woman$lowenergy_days, na.rm = T)
```

```
## [1] 0.6385483
```

```r
var(namibia_hh_men_woman$lowenergy_days, na.rm = T)
```

```
## [1] 3.2736
```




#### Couples Data from DHS

