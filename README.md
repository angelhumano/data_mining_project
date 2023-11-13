## Predicting and determining the factors that lead to employee attrition

Team: PurrAnalytics
<br>
Members: Gabriel Fernandez, Jessica Tappi, Olympia Lala, and Tenzin Bajracharya
Fall 2022 (Updated: Sept. 2023)

## Project bites :chocolate_bar: 

**Brief**: This project focuses on predicting employee attrition and understanding its key drivers, involving data collection, preprocessing, model development, and providing actionable recommendations for HR departments and prospective employees.

**Tools**: R, RStudio, R Notebook, and R Markdown.

**Concepts**: Classification, Machine Learning, Logistic Regression, KNN, Decision Trees, Bagging, Random Forest, Performance Measures, and  Model Interpretability.



## Business problem
Employee retention is essential to the company's success, and the attrition rate is the metric that provides insight into how well the company is retaining its employees. Companies can identify and address the problems causing voluntary attrition by measuring and analyzing the attrition rate [1]. That is important because the costs associated with losing valuable employees can be staggering. For example, the cost to hire and train a new employee, when one employee voluntarily departs, can be one-half to two times that employee's annual salary [2].

Furthermore, company profits can be negatively affected when knowledgeable, experienced employees leave and productivity suffers as well. Also, high turnover has negative intangible costs, such as affecting the company culture and employee engagement.
Our project aims to build machine learning models to predict whether an employee will leave the company or not, at a mid-sized IT company, and find out the key drivers of employee attrition. Understanding why employees leave can help the human resource department develop relevant, effective retention strategies to reduce attrition and can offer valuable insights to prospective employees before they join a company.

## Target audience

- Human resource departments
- Prospective employees

## Data collection

The dataset used in this project is a fictional dataset created by IBM data scientists [3]. Dataset description:
- Kaggle usability score: 8.82
- License: Open Database License (ODbL)
- Number of observations: 1470
- Number of columns: 35
- Response variable: attrition {Yes = 1 (employee leaves); No = 0 (employee stays)}
- Variables dictionary [4]

## Data preprocessing

**Exploratory data analysis**

Duplicates: none
Missing values: none
Outliers: some predictors had outliers. Refer to Table 1 for more details
Dataset response variable is imbalanced (237 cases of “Yes” and 1233 cases of “No”). See Figure 1a
Some predictors are linearly dependent. Refer to Table 1

## Data transformation

After EDA and preprocessing, we created two datasets. One with dummy variables for logistic regression and KNN, and another one without dummies decision trees, bagging, and random forest. All transformations and data reductions are documented in Table 1. Since the data was imbalanced, we used the SMOTE technique to balance the training dataset. Refer to Figure 1b.


## Model development 

We split both dataset versions into training (80% of observations) and testing (20% of observations) using the same seed for the sample() function. Then, we applied oversampling with the SMOTE() function to the training dataset. Since predicting attrition is a classification problem, we developed five classification models using the following algorithms: 

- Logistic regression: trained a model with all variables, dropped linearly dependent variables, computed VIF(), dropped variables with VIF() > 10, and trained a final logistic regression model with the remaining predictors.
- K-nearest neighbors: dropped linearly dependent variables, normalized all predictors, split the training dataset into training and cross-validation to find optimal k (1-10) with 5-fold cross-validation, and trained a final KNN with k= 6.
- Decision trees: fitted a tree model to the training dataset, selected the best subtree size (lowest dev)  and fitted a prune model.
- Bagging: trained a bagging model with mtry = 30 (all predictors) and obtained the model’s feature importance.
- Random forest: trained a random forest model with mtry = 6 (square root of 30 is 5.4) and obtained the model’s feature importance.

**Measures**

For model evaluation, we used the confusion matrix and the following measures: F1-score, recall, precision, and accuracy. Table 2 shows that logistic regression performed better than the other four models in all these measures.  Refer to the R Notebook attached to this report to see the confusion matrix for each model and to Table 2 to see a comparison of the models by measures.  

For our business problem, recall is more important than precision because the cost associated with false negatives outweighs the cost associated with false positives. For instance, the false negative cost is when the employee is predicted not to leave but leaves the company. In this case, the company incurs a heavy loss in terms of the cost of hiring a new employee, training, and productivity issues. Research by SHRM suggests replacement costs can be as high as 50%-60%, with overall costs ranging anywhere from 90%-200% [5].

On the other hand, false positive cost is the case when we predict that an employee will leave but does not leave. Here the company incurs a little cost in terms of salary hike, better stock level option, and increase in pay as per overtime.

## Interpretation of the Models

Besides being the top performer by measures, logistic regression is the most interpretable of our models. Our logistic model helps predict attrition and make inferences about the predictors that are associated with attrition. To determine the top 10 most important predictors for attrition, we plotted the top odd ratios with their confidence interval (see Figure 2).

Regarding the other model’s interpretability, KNN is not interpretable, bagging and random forest have feature importance graphs (Figure 3 and Figure 4). However, we do not consider these for our recommendations because logistic regression performed significantly better than the tree-based models for recall (see Table 2).


## Conclusion and implications 

Based on the top ten contributing factors to attrition by odd ratios. We recommend that human resource department:

- Reach out to employees that travel extensively, to offer more time to work from their main location. This may reduce any strain that the employee(s) may experience when traveling frequently for work.

- Build morale by checking in with colleagues and employees to encourage engagement.

- Host team-building events throughout the year to help facilitate a collaborative work environment.

- Involve employees in the strategy and big picture of their role at the company to combat low job satisfaction.

- Prospective employees can inquire about the top 10 predictors of attrition before they join a company.  


## Ethical considerations

Models trained with employee attrition data can be challenging to implement and interpret. For example, a company may give the wrong employee or employees raises based on their analysis. Other nuance factors in employee attrition data may also lead to incorrect conclusions, such as health issues, or personal circumstances, all of which are very sensitive topics. 

Additionally, employment law or using health data, especially in the district of New York, can be costly by way of lawsuits and settlements. Any action plans based on these interpretations of the employee data should be consulted with the company’s legal counsel before implementation.



## Reference


- [1] [How to Evaluate the Effectiveness of Your Talent Acquisition Strategy](https://www.skeeled.com/blog/how-to-evaluate-the-effectiveness-of-your-talent-acquisition-strategy)
- [2] [The Real Value of Getting an Exit Interview Right](https://www.gallup.com/workplace/236051/real-value-getting-exit-interview-right.aspx)
- [3] [IBM HR Analytics Employee Attrition & Performance | Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
- [4] [Data dictionary for our dataset](Data_variable_dictionary.pdf)
- [5] [Retaining Talent: A Guide to Analyzing and Managing Employee Turnover (page 3)](https://www.shrm.org/hr-today/trends-and-forecasting/special-reports-and-expert-views/documents/retaining-talent.pdf)

