Project: Employee Attrition
================
Gabriel Fernandez, Jessica Tappi, Olympia Lala, and Tenzin Bajracharya
12/06/2022

``` r
# Set default options for code chunks
knitr::opts_chunk$set(
  echo = FALSE,          # Display R code and its output
  comment=NA,           # Suppress code comments in output
  include = FALSE,      # Suppress code outputs
  warning = FALSE,      # Suppress warning messages
  fig.align='center',   # Align figures in the center
  eval = TRUE           # Evaluate R code
)
```

# Dataset

[IBM HR Analytics Employee Attrition & Performance: IBM HR Analytics
Employee Attrition &
Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)

Below is the data description along with the link to variable
descriptions. Data description:

This is a fictional dataset created by IBM data scientists. - Kaggle
usability score: 8.82 - License: Open Database License (ODbL) (users can
freely share, modify, and use a database while maintaining this same
freedom for others.) - Number of observations: 1470 - Number of columns:
35 - [Link:
Data_Variable_Dictionary](https://docs.google.com/spreadsheets/d/13k0-IkYwHR9Em9-OyjJcwEblWxX_xvtT0LHyk4QZSFM/edit)

Which factors lead to employee attrition? How many employees are more
likely to leave their jobs? Our target variable is attrition

Employee attrition prediction is a classification problem. We plan to
use classification algorithms and compare the performance of the
following models to predict attrition.

- Logistic regression
- KNN
- Decision tree
- Bagging
- Random forest

# Exploratory data analysis

### Columns names

    Age
    Attrition
    BusinessTravel
    DailyRate
    Department
    DistanceFromHome
    Education
    EducationField
    EmployeeCount
    EmployeeNumber
    EnvironmentSatisfaction
    Gender
    HourlyRate
    JobInvolvement
    JobLevel
    JobRole
    JobSatisfaction
    MaritalStatus
    MonthlyIncome
    MonthlyRate
    NumCompaniesWorked
    Over18
    OverTime
    PercentSalaryHike
    PerformanceRating
    RelationshipSatisfaction
    StandardHours
    StockOptionLevel
    TotalWorkingYears
    TrainingTimesLastYear
    WorkLifeBalance
    YearsAtCompany
    YearsInCurrentRole
    YearsSinceLastPromotion
    YearsWithCurrManager

## Univariate analysis

### Numerical variables

### Observations

- Remove “EmployeeCount.” All observations have the same value (1)

- Remove “StandardHours.” All observations have the same value (80)

- Remove “EmployeeNumber” because is a unique Id that will not add any
  additional information

#### After exploring the histogram for the following variables, we realized that they are factors, not numerical.

- Education
- EnvironmentSatisfaction
- JobInvolvement
- JobSatisfaction
- PerformanceRating
- RelationshipSatisfaction
- WorkLifeBalance
- StockOptionLevel

The factor levels were provided within the original source for all
variables except for “StockOptionLevel.” From the dataset link, we can
find the name of the levels. [Check dataset
source](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)

For “StockOptionLevel”, we only converted the variable to a factor with
the unique levels as the following factors(0, 1, 2, 3). It seems these
are the level names that IBM uses for its option levels. [Stock Option
Levels](https://www.levels.fyi/companies/ibm/salaries/recruiter)

### Categorical variables

### Observations

- The response variable is unbalanced: 237 observations have “Yes” and
  1233 have “No”

- “Yes” is coded as 1 and “No” as 0

- “Over18” has only 1 value. All the employees are older than 18.
  Therefore, we have to drop the redundant column.

## Bivariate analysis

### Correlation between numerical variables

<img src="PurrAnalytics_project_final_code_revised_Fall_2023_files/figure-gfm/unnamed-chunk-19-1.png" style="display: block; margin: auto;" />

    Department =  6 
    DistanceFromHome =  5 

### Observations

- “Department” and “MonthlyIncome” are highly correlated. Some
  departments may be paid more than the others

- “DistanceFromHome” and “JobLevel” are highly correlated

### Numerical variables vs response

### Observations:

- Four out of fifteen variables do not show obvious differences between
  the median attrition, regardless of the condition: (HourlyRate,
  TrainingTimesLastYear, YearsSinceLastPromotion, PercentSalaryHike)

- For the rest of the numerical variables, the median attrition differs
  between the classes

### Factor variables vs. response

### Observations:

- Most factors show an imbalance between the classes

- Employees with Bachelor’s degree as their highest education level are
  more likely to leave the company, whereas employees with Doctorate
  degree are less likely to leave the company

- Single employees are more likely to leave the company

## Check for null values

                         Age                Attrition           BusinessTravel 
                           0                        0                        0 
                   DailyRate               Department         DistanceFromHome 
                           0                        0                        0 
                   Education           EducationField  EnvironmentSatisfaction 
                           0                        0                        0 
                      Gender               HourlyRate           JobInvolvement 
                           0                        0                        0 
                    JobLevel                  JobRole          JobSatisfaction 
                           0                        0                        0 
               MaritalStatus            MonthlyIncome              MonthlyRate 
                           0                        0                        0 
          NumCompaniesWorked                 OverTime        PercentSalaryHike 
                           0                        0                        0 
           PerformanceRating RelationshipSatisfaction         StockOptionLevel 
                           0                        0                        0 
           TotalWorkingYears    TrainingTimesLastYear          WorkLifeBalance 
                           0                        0                        0 
              YearsAtCompany       YearsInCurrentRole  YearsSinceLastPromotion 
                           0                        0                        0 
        YearsWithCurrManager 
                           0 

## Check for duplicates

    Duplicate values: 0

### Observations

- There are no missing values.
- There are no duplicates.

## Check for outliers

The following variables have outliers:

     MonthlyIncome   NumCompaniesWorked TotalWorkingYears TrainingTimesLastYear
     Min.   : 1009   Min.   :0.000      Min.   : 0.00     Min.   :0.000        
     1st Qu.: 2911   1st Qu.:1.000      1st Qu.: 6.00     1st Qu.:2.000        
     Median : 4919   Median :2.000      Median :10.00     Median :3.000        
     Mean   : 6503   Mean   :2.693      Mean   :11.28     Mean   :2.799        
     3rd Qu.: 8379   3rd Qu.:4.000      3rd Qu.:15.00     3rd Qu.:3.000        
     Max.   :19999   Max.   :9.000      Max.   :40.00     Max.   :6.000        
     YearsAtCompany   YearsInCurrentRole YearsSinceLastPromotion
     Min.   : 0.000   Min.   : 0.000     Min.   : 0.000         
     1st Qu.: 3.000   1st Qu.: 2.000     1st Qu.: 0.000         
     Median : 5.000   Median : 3.000     Median : 1.000         
     Mean   : 7.008   Mean   : 4.229     Mean   : 2.188         
     3rd Qu.: 9.000   3rd Qu.: 7.000     3rd Qu.: 3.000         
     Max.   :40.000   Max.   :18.000     Max.   :15.000         
     YearsWithCurrManager
     Min.   : 0.000      
     1st Qu.: 2.000      
     Median : 3.000      
     Mean   : 4.123      
     3rd Qu.: 7.000      
     Max.   :17.000      

### Observations

- Outliers do not look unreasonable

## Check if the data is balanced or imbalanced

- Here, the data is unbalanced: 237 (\~16%) employees leave the company,
  whereas 1233 (\~84%) stay at their workplace

## Data preprocessing for logistic regression and KNN

# Datasets after EDA and preprocessing

# Final dataset after EDA. Use this dataset for decision trees, bagging, and random forest.

- Multicollinearity does not affect tree-based models.

## Using oversampling to balance the training data for dataset without the dummy variables

# Final dataset for logistic regression and KNN. For this dataset, we created dummy variables for the factor variables.

### - Train and test the hold-out dataset with the dataset containing dummy variables used for logistic regression and KNN

## Using oversampling to balance the training data for the dataset containing dummy variables

# Model 1: Logistic Regression

\###- Fitting all the data to the logistic model to find variables with
high VIF.

\###- We need to drop the linearly dependent variables below and refit
the model:

     [1] "department_sales"                    "education_doctor"                   
     [3] "environment_satisfaction_very_high"  "job_involvement_very_high"          
     [5] "job_satisfaction_very_high"          "performance_rating_low"             
     [7] "performance_rating_good"             "performance_rating_outstanding"     
     [9] "relationship_satisfaction_very_high" "stock_option_level_3"               
    [11] "work_life_balance_best"             

### - Fit the entire dataset to the logistic model after dropping high linearly dependent variables

### - These variables have a VIF \> 5

### - These variables have a VIF \> 10

- We chose a VIF = 10 as a threshold to avoid removing too many
  significant variables from the dataset

<!-- -->

    Dataset for logistic regression and KNN shape = ( 1822 , 60 )

### - Fit final logistic regression

### - Train and test the hold-out dataset after removing all the high VIF variables


    Call:
    glm(formula = attrition_yes ~ ., family = binomial, data = final_EA_data_log)

    Deviance Residuals: 
        Min       1Q   Median       3Q      Max  
    -3.0082  -0.3647   0.1070   0.3933   3.8303  

    Coefficients:
                                         Estimate Std. Error z value Pr(>|z|)    
    (Intercept)                        -6.685e+00  1.525e+00  -4.383 1.17e-05 ***
    business_travel_travel_frequently   3.563e+00  4.570e-01   7.797 6.33e-15 ***
    business_travel_travel_rarely       2.011e+00  4.232e-01   4.752 2.01e-06 ***
    education_below_college             1.029e-01  3.118e-01   0.330 0.741456    
    education_college                   6.530e-01  2.396e-01   2.726 0.006415 ** 
    education_master                    4.294e-01  2.347e-01   1.830 0.067267 .  
    education_field_human_resources    -5.380e-01  7.110e-01  -0.757 0.449246    
    education_field_life_sciences      -4.587e-02  4.116e-01  -0.111 0.911269    
    education_field_marketing           1.130e+00  4.802e-01   2.354 0.018572 *  
    education_field_medical            -5.493e-01  4.247e-01  -1.293 0.195899    
    education_field_technical_degree    1.043e+00  4.758e-01   2.192 0.028398 *  
    environment_satisfaction_low        2.615e+00  2.578e-01  10.146  < 2e-16 ***
    environment_satisfaction_medium     7.972e-01  2.705e-01   2.947 0.003205 ** 
    environment_satisfaction_high       9.673e-01  2.329e-01   4.154 3.27e-05 ***
    gender_male                         8.632e-01  1.865e-01   4.629 3.68e-06 ***
    job_involvement_low                 2.992e+00  5.476e-01   5.464 4.67e-08 ***
    job_involvement_medium              1.687e+00  3.980e-01   4.239 2.24e-05 ***
    job_involvement_high                6.710e-01  3.869e-01   1.734 0.082883 .  
    job_role_healthcare_representative -1.615e+00  5.077e-01  -3.180 0.001471 ** 
    job_role_manager                    5.774e-02  5.022e-01   0.115 0.908468    
    job_role_research_director         -2.154e+00  7.434e-01  -2.897 0.003765 ** 
    job_role_sales_executive           -4.924e-01  2.440e-01  -2.018 0.043602 *  
    job_satisfaction_low                2.505e+00  2.640e-01   9.490  < 2e-16 ***
    job_satisfaction_medium             1.234e+00  2.940e-01   4.199 2.69e-05 ***
    job_satisfaction_high               1.294e+00  2.530e-01   5.117 3.10e-07 ***
    marital_status_married              6.029e-01  2.687e-01   2.244 0.024862 *  
    marital_status_single               1.396e+00  3.835e-01   3.640 0.000272 ***
    over_time_yes                       1.839e+00  1.991e-01   9.234  < 2e-16 ***
    performance_rating_excellent        2.445e-01  4.213e-01   0.580 0.561754    
    relationship_satisfaction_low       1.397e+00  2.506e-01   5.577 2.44e-08 ***
    relationship_satisfaction_medium    5.334e-01  2.754e-01   1.937 0.052749 .  
    relationship_satisfaction_high      5.090e-01  2.521e-01   2.019 0.043465 *  
    stock_option_level_0               -3.765e-01  5.875e-01  -0.641 0.521680    
    stock_option_level_1               -1.218e+00  5.499e-01  -2.215 0.026776 *  
    stock_option_level_2               -9.558e-01  5.864e-01  -1.630 0.103091    
    work_life_balance_bad               1.249e+00  4.777e-01   2.615 0.008912 ** 
    work_life_balance_good              7.376e-01  3.648e-01   2.022 0.043174 *  
    work_life_balance_better            3.496e-01  3.319e-01   1.053 0.292137    
    age                                -2.839e-02  1.330e-02  -2.135 0.032743 *  
    daily_rate                          3.547e-05  2.308e-04   0.154 0.877871    
    distance_from_home                  3.671e-02  1.139e-02   3.224 0.001262 ** 
    hourly_rate                        -2.768e-03  4.488e-03  -0.617 0.537490    
    monthly_rate                       -1.752e-06  1.249e-05  -0.140 0.888413    
    num_companies_worked                3.282e-01  3.830e-02   8.570  < 2e-16 ***
    percent_salary_hike                 1.135e-02  4.196e-02   0.271 0.786742    
    total_working_years                -1.601e-01  2.818e-02  -5.682 1.33e-08 ***
    training_times_last_year           -3.060e-01  7.238e-02  -4.228 2.36e-05 ***
    years_at_company                    1.591e-01  3.840e-02   4.142 3.44e-05 ***
    years_in_current_role              -2.103e-01  4.657e-02  -4.517 6.29e-06 ***
    years_since_last_promotion          1.933e-01  4.092e-02   4.724 2.31e-06 ***
    years_with_curr_manager            -2.004e-01  4.696e-02  -4.266 1.99e-05 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    (Dispersion parameter for binomial family taken to be 1)

        Null deviance: 2525.0  on 1821  degrees of freedom
    Residual deviance: 1070.5  on 1771  degrees of freedom
    AIC: 1172.5

    Number of Fisher Scoring iterations: 6

                           (Intercept)  business_travel_travel_frequently 
                         -6.684683e+00                       3.563210e+00 
         business_travel_travel_rarely            education_below_college 
                          2.011231e+00                       1.028563e-01 
                     education_college                   education_master 
                          6.530100e-01                       4.294306e-01 
       education_field_human_resources      education_field_life_sciences 
                         -5.379702e-01                      -4.587031e-02 
             education_field_marketing            education_field_medical 
                          1.130279e+00                      -5.493119e-01 
      education_field_technical_degree       environment_satisfaction_low 
                          1.042726e+00                       2.615430e+00 
       environment_satisfaction_medium      environment_satisfaction_high 
                          7.971965e-01                       9.673211e-01 
                           gender_male                job_involvement_low 
                          8.632012e-01                       2.991869e+00 
                job_involvement_medium               job_involvement_high 
                          1.687339e+00                       6.710023e-01 
    job_role_healthcare_representative                   job_role_manager 
                         -1.614711e+00                       5.774343e-02 
            job_role_research_director           job_role_sales_executive 
                         -2.153696e+00                      -4.923717e-01 
                  job_satisfaction_low            job_satisfaction_medium 
                          2.505497e+00                       1.234267e+00 
                 job_satisfaction_high             marital_status_married 
                          1.294446e+00                       6.029451e-01 
                 marital_status_single                      over_time_yes 
                          1.395939e+00                       1.838887e+00 
          performance_rating_excellent      relationship_satisfaction_low 
                          2.444680e-01                       1.397381e+00 
      relationship_satisfaction_medium     relationship_satisfaction_high 
                          5.334066e-01                       5.090297e-01 
                  stock_option_level_0               stock_option_level_1 
                         -3.764745e-01                      -1.217846e+00 
                  stock_option_level_2              work_life_balance_bad 
                         -9.558026e-01                       1.249473e+00 
                work_life_balance_good           work_life_balance_better 
                          7.376215e-01                       3.496093e-01 
                                   age                         daily_rate 
                         -2.839142e-02                       3.546967e-05 
                    distance_from_home                        hourly_rate 
                          3.671433e-02                      -2.767657e-03 
                          monthly_rate               num_companies_worked 
                         -1.752420e-06                       3.282186e-01 
                   percent_salary_hike                total_working_years 
                          1.135179e-02                      -1.601199e-01 
              training_times_last_year                   years_at_company 
                         -3.059935e-01                       1.590560e-01 
                 years_in_current_role         years_since_last_promotion 
                         -2.103153e-01                       1.933119e-01 
               years_with_curr_manager 
                         -2.003529e-01 

\###- Here “0” means “No”, the employee does not leave the company and
“1” mean “Yes”, the employee leaves the company.

## Measures for Model 1: Logistic regression

# Model 2: K-Nearest Neighbors (KNN)

\###- Use 5-fold cross-validation to find optimal k

### - Refit the KNN model with K = 6

### Drop linear dependent variables in the testing dataset

## Measures for Model 2: K-Nearest Neighbors (KNN)

# Model 3: Classification Decision Trees

# Reminder: for Model 3-5, we will use EA_data1, which is our clean dataset, before creating dummy variables.

### - Fit a Tree-model to EA_data1training data.

    18

### - The best subtree size is 18 because it has the lowest dev.

### Observations:

- The unpruned tree has 18 terminal nodes, and the pruned tree has 18
  terminal nodes.

## Measures for Model 3: Decision Trees

# Model 4: Bagging

## Measures for Model 4: Bagging

The results indicate that across all of the trees considered in the
bagging model, the following ten are the most important variables, with
Job level ranking is the highest.

# Model 5: Random Forest

## Measures for Model 5: Random Forest

The results indicate that across all of the trees considered in the
random forest model, the following ten are the most important variables.

# Compare measures between all models

# Reference

- 1.  Plotting histograms with
      ggplot2:[1.0](https://appsilon.com/ggplot2-histograms/),
      [1.1](https://community.rstudio.com/t/different-titles-for-histograms-using-for-loop/143297/9)

- 2.  Using ggtitle :
      [2.0](https://www.datanovia.com/en/blog/ggplot-title-subtitle-and-caption/)

- 3.  Box plots:
      [3.0](https://ggplot2.tidyverse.org/reference/geom_boxplot.html)

- 4.  Hide axis in ggplot2:
      [4.0](https://www.statology.org/remove-axis-labels-ggplot2/)

- 5.  Bar charts with ggplot2:
      [5.0](http://www.cookbook-r.com/Graphs/Bar_and_line_graphs_(ggplot2)/),
      [5.1](https://r-graph-gallery.com/48-grouped-barplot-with-ggplot2)

- 6.  Adding text to low-level plotting functions:
      [6.0](https://bookdown.org/ndphillips/YaRrr/low-level-plotting-functions.html)

- 7.  Adding Labels to a Bar Graph:
      [7.0](https://r-graphics.org/recipe-bar-graph-labels),
      [7.1](https://stats.stackexchange.com/a/68152/314078),
      [7.2](https://www.tutorialspoint.com/how-to-create-a-bar-plot-using-ggplot2-with-percentage-on-y-axis-in-r)

- 8.  Drop linearly dependent variables references:
      [8.0](https://www.statology.org/r-aliased-coefficients-in-the-model/),
      [8.1](https://stackoverflow.com/a/32791536/15333580)

- 9.  Plotting odd ratios (forest plot):
      [9.0](https://www.mihiretukebede.com/posts/2020-09-30-2020-09-30-plotting-model-coefficients-in-a-forest-plot/)

- 10. Standardize all variables in a data frame:
      [10](https://www.statology.org/standardize-data-in-r/)

- 11. Create a table from a data frame for a report:
      [11.0](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html#Column__Row_Specification),
      [11.1](https://rstudio.github.io/distill/tables.html)
