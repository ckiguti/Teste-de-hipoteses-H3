ANCOVA test for `pos.score`\~`pre.score`+`scenario`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “pos.score”](#ancova-plots-for-the-dependent-variable-pos.score)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for pos.score
    [../data/table-for-pos.score.csv](../data/table-for-pos.score.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | variable  |   n |  mean | median | min | max |    sd |    se |    ci | iqr | symmetry | skewness | kurtosis |
|:-------------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|----:|:---------|---------:|---------:|
| gamified     | pos.score |  16 | 8.938 |      9 |   7 |  10 | 0.929 | 0.232 | 0.495 |   2 | YES      |   -0.357 |   -1.021 |
| non.gamified | pos.score |  16 | 7.000 |      7 |   5 |   8 | 0.966 | 0.242 | 0.515 |   2 | YES      |   -0.416 |   -1.135 |
| NA           | pos.score |  32 | 7.969 |      8 |   5 |  10 | 1.356 | 0.240 | 0.489 |   2 | YES      |   -0.171 |   -0.826 |

![](/home/rstudio/report/ancova/6c96a7f0184c1825/results/ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(
"pos.score" = c("P28","P30","P10","P11")
)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|           | var       |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----------|:----------|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| pos.score | pos.score |  28 |    0.361 |   -1.098 | YES      |     0.933 | Shapiro-Wilk | 0.073 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|:----------|:-------------|----------:|------:|:---------|
| gamified     | pos.score |  14 | 8.786 |      9 |   7 |  10 | 0.893 | 0.239 | 0.515 | 1.00 | YES       | Shapiro-Wilk |     0.889 | 0.079 | ns       |
| non.gamified | pos.score |  14 | 6.857 |      7 |   5 |   8 | 0.949 | 0.254 | 0.548 | 1.75 | YES       | Shapiro-Wilk |     0.882 | 0.063 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["pos.score"]], x=covar, y="pos.score", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](/home/rstudio/report/ancova/6c96a7f0184c1825/results/ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|             | var       | method         | formula            |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------------|:----------|:---------------|:-------------------|----:|--------:|--------:|----------:|------:|:---------|
| pos.score.1 | pos.score | Levene’s test  | `.res`\~`scenario` |  28 |       1 |      26 |     0.281 | 0.600 | ns       |
| pos.score.2 | pos.score | Anova’s slopes | `.res`\~`scenario` |  28 |       1 |      24 |     0.032 | 0.859 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|             | scenario     | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr |
|:------------|:-------------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|
| pos.score.1 | gamified     | pos.score |  14 | 8.786 |      9 |   7 |  10 | 0.893 | 0.239 | 0.515 | 1.00 |
| pos.score.2 | non.gamified | pos.score |  14 | 6.857 |      7 |   5 |   8 | 0.949 | 0.254 | 0.548 | 1.75 |

![](/home/rstudio/report/ancova/6c96a7f0184c1825/results/ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var       | Effect    | DFn | DFd |    SSn |   SSd |      F |   p |   ges | p.signif |
|:----------|:----------|----:|----:|-------:|------:|-------:|----:|------:|:---------|
| pos.score | pre.score |   1 |  25 | 13.083 | 8.989 | 36.385 |   0 | 0.593 | \*\*\*\* |
| pos.score | scenario  |   1 |  25 | 19.516 | 8.989 | 54.279 |   0 | 0.685 | \*\*\*\* |

### Pairwise comparison

| var       | scenario | group1   | group2       | estimate | conf.low | conf.high |   se | statistic |   p | p.adj | p.adj.signif |
|:----------|:---------|:---------|:-------------|---------:|---------:|----------:|-----:|----------:|----:|------:|:-------------|
| pos.score | NA       | gamified | non.gamified |    1.694 |    1.221 |     2.168 | 0.23 |     7.367 |   0 |     0 | \*\*\*\*     |

### Descriptive Statistic of Estimated Marginal Means

| var       | scenario     |   n | emmean |  mean | conf.low | conf.high |    sd | sd.emms | se.emms |
|:----------|:-------------|----:|-------:|------:|---------:|----------:|------:|--------:|--------:|
| pos.score | gamified     |  14 |  8.668 | 8.786 |    8.336 |     9.001 | 0.893 |   0.604 |   0.161 |
| pos.score | non.gamified |  14 |  6.974 | 6.857 |    6.642 |     7.307 | 0.949 |   0.604 |   0.161 |

### Ancova plots for the dependent variable “pos.score”

``` r
plots <- oneWayAncovaPlots(sdat[["pos.score"]], "pos.score", between
, aov[["pos.score"]], pwc[["pos.score"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `pos.score` \~ `scenario`

``` r
plots[["scenario"]]
```

![](/home/rstudio/report/ancova/6c96a7f0184c1825/results/ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “pre.score”, ANCOVA tests
with independent between-subjects variables “scenario” (gamified,
non.gamified) were performed to determine statistically significant
difference on the dependent varibles “pos.score”. For the dependent
variable “pos.score”, there was statistically significant effects in the
factor “pre.score” with F(1,25)=36.385, p&lt;0.001 and ges=0.593 (effect
size) and in the factor “scenario” with F(1,25)=54.279, p&lt;0.001 and
ges=0.685 (effect size).

Pairwise comparisons using the Estimated Marginal Means (EMMs) were
computed to find statistically significant diferences among the groups
defined by the independent variables, and with the p-values ajusted by
the method “bonferroni”. For the dependent variable “pos.score”, the
mean in the scenario=“gamified” (adj M=8.668 and SD=0.893) was
significantly different than the mean in the scenario=“non.gamified”
(adj M=6.974 and SD=0.949) with p-adj&lt;0.001.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.
