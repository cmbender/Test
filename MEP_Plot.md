Middle Ear Pressure Probability Density Function By Age
================
E Bryan Crenshaw III with Cody Bender
March 31, 2017

**Objective**: Plot the probability density function of middle ear pressure in three age groups (young = age &gt;= 0.5 & age &lt; 3; middle = age &gt;= 3 & age &lt;= 15; old = (age &gt;= 15 & age &lt;= 21 ).

**Approach**: Loaded 'nodupall', a dataset of patients from AudGenDB with no tympnaostomy CPT codes checked against a list of tympanostomy patients. Removed duplicate entries with the same patient alias. Patients with missing values for ear canal volume, middle ear pressure, static compliance, or age were also removed. Patients were then divided into 3 age groups: young (0.65-3 years), middle (3-15 years), and older (15-21 years). Quantiles were calculated for each group and probability density functions were plotted.

``` r
#Data divided into 3 age groups
load(file = "nodupall.Rdata")
nodupall_yng <- subset(nodupall, age >= 0.5 & age < 3,)
nodupall_mid <- subset(nodupall, age >= 3 & age < 15,)
nodupall_old <- subset(nodupall, age >= 15 & age < 21,)
```

**Basic Statistical Analyses:**

``` r
nodupallm_yng.describe <- describe(nodupall_yng$mep)
print("Basic Statistics on Middle Ear Pressure of Children 6 months to <3 years Old :"); nodupallm_yng.describe
```

    ## [1] "Basic Statistics on Middle Ear Pressure of Children 6 months to <3 years Old :"

    ##    vars  n   mean   sd median trimmed   mad  min max range  skew kurtosis
    ## X1    1 57 -37.46 57.9    -20  -31.17 44.48 -190  65   255 -0.91     0.22
    ##      se
    ## X1 7.67

``` r
nodupallm_mid.describe <- describe(nodupall_mid$mep)
print("Basic Statistics on Middle Ear Pressure of Children 6 months to <3 years Old :"); nodupallm_mid.describe
```

    ## [1] "Basic Statistics on Middle Ear Pressure of Children 6 months to <3 years Old :"

    ##    vars    n   mean    sd median trimmed   mad  min max range  skew
    ## X1    1 1556 -33.02 63.13    -10   -21.8 37.06 -385 105   490 -1.96
    ##    kurtosis  se
    ## X1     4.55 1.6

``` r
nodupallm_old.describe <- describe(nodupall_old$mep)
print("Basic Statistics on Middle Ear Pressure of Children 6 months to <3 years Old :"); nodupallm_old.describe
```

    ## [1] "Basic Statistics on Middle Ear Pressure of Children 6 months to <3 years Old :"

    ##    vars   n   mean    sd median trimmed   mad  min max range  skew
    ## X1    1 189 -12.81 42.78     -5   -4.68 14.83 -235  40   275 -3.21
    ##    kurtosis   se
    ## X1    11.77 3.11

**Quantile Analyses:**

``` r
# Show quantiles of percents in the probs vector below
mep_yng <- quantile(nodupall_yng$mep,  probs = c(2.5, 5, 10, 50, 90, 95, 97.5, NA)/100, na.rm = TRUE)
print("Quantiles of Younger Children:"); print(mep_yng);
```

    ## [1] "Quantiles of Younger Children:"

    ##  2.5%    5%   10%   50%   90%   95% 97.5%       
    ##  -180  -157  -114   -20    20    30    30    NA

``` r
mep_mid <- quantile(nodupall_mid$mep,  probs = c(2.5, 5, 10, 50, 90, 95, 97.5, NA)/100, na.rm = TRUE)
print("Quantiles of Middle Children:"); print(mep_mid)
```

    ## [1] "Quantiles of Middle Children:"

    ##    2.5%      5%     10%     50%     90%     95%   97.5%         
    ## -210.00 -171.25 -115.00  -10.00   20.00   25.00   30.00      NA

``` r
mep_old <- quantile(nodupall_old$mep,  probs = c(2.5, 5, 10, 50, 90, 95, 97.5, NA)/100, na.rm = TRUE)
print("Quantiles of Older Children:"); print(mep_old)
```

    ## [1] "Quantiles of Older Children:"

    ##   2.5%     5%    10%    50%    90%    95%  97.5%        
    ## -154.0  -78.0  -41.0   -5.0   15.0   25.0   26.5     NA

``` r
# Plot the mep of the 3 age ranges with outliers removed
plot(density(nodupall_yng$mep, na.rm = TRUE), main = "Middle Ear Pressure of Children with PTA <=15 dB & No Pathology", xlab = "Middle Ear Pressure (dPa)", xlim = c(-250,100), ylim = c(0,0.025))
lines(density(nodupall_mid$mep, na.rm = TRUE), col =2)
lines(density(nodupall_old$mep, na.rm = TRUE), col = 3)
legend("topleft", c("Young (0.5-<3yo)", "Middle (3-15yo)","Older (15-21yo)"), cex=1.1, col=c("black","red","green"), pch=21:23, lty=1:3)
```

![](MEP_Plot_files/figure-markdown_github/plot-1.png)

**Observations**: The dataset contained 1802 patients after additional tympanograms performed for each patient after the first were removed. The vast majority of patients fell into the Middle subset between ages 3-15.

**Comparisons By Age**:

The Younger subset contained 57 patients and had a 90% range of middle ear pressure from -157 to 30.

The Middle subset contained 1556 patients and had a 90% range of ear canal volume from -171.25 to 25.

The Older subset contained 189 patients and had a 90% range of ear canal volume from -78 to 25.
