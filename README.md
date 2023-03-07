# Telecom Churn Prediction for 5GBoost
This project aims to predict which customers are likely to churn in 5GBoost, a company currently facing a high monthly churn rate of 14.5%. By identifying these customers, the company can adjust its approach to increase retention and minimize the financial impact of churn.

The project utilizes supervised machine learning models to solve a classification problem. Model interpretability is important in this content because we not only want to know the likelihood of customer churn, we also want to know the reason for the prediction and learn more about the problem, the data, and the reasons why the model might fail.

## Table of Contents
- [ Dataset ](#dataset)
- [ Data Preprocessing ](#prepro)
- [ Model Training ](#model)
- [ Business Interpretation ](#interpr)

<a name="dataset"></a>
## Dataset

The dataset used in this project contains information on 10,000 customers of a telecom company 5GBoost. The dataset contains 3335 rows and 14 columns related to customer usage and services of a telecom company, including Account length, International plan, Voice mail plan, Number vmail messages, Total day calls, Total day charge, Total eve calls, Total eve charge, Total night calls, Total night charge, Total intl calls, Total intl charge, Customer service calls, and Churn.

<a name="prepro"></a>
## Data Preprocessing

To prepare the dataset for analysis, we first loaded it into a Python Jupyter Notebook and performed several data preprocessing steps. The data was cleaned by checking strange distribution, missing values, outliers, suspicious or corrupted values, irrevelanvt columns, etc. We also checked target leakage and correlation, as well as performed feature scaling and one-hot encoding to ensure that all features were on the same scale and in a suitable format for the machine learning algorithm.

<a name="model"></a>
## Model Training

In the training-test split, we chose 80% of the training data along with 20% of the test data, allowing neither variance to be too high. Also, in this setting, the share of Churned customer are similar in both datasets (14.29% in train and 15.25% in test).

- Feature engineering: Created "Total Charge" and "Total Calls", which are the sum of total day, evening and night minutes and calls. The reason why these 2 new features are created because we can use this to know the overall frequency of consumers' calls and minutes. Knowing the actual time interval does not help us in our prediction
- Feature selection: Removed “Total day/eve/night charge” and “Total day/eve/night calls” as they are already included in the new features. Also, “Number vmail messages” were excluded since its feature importance was only 0.34%
<br />
The model is trained using BIGML with classification algorithms such as Decision Tree, Random Forest, and 5-fold cross-validation, and their performance was evaluated using metrics such as accuracy, precision, and F1 score, with a focus on recall and Confusion Matrix. The model was then fine-tuned by switching from Smart pruning to Active statistical pruning, and was checked for overfitting.

#### Decision Tree
![image](https://user-images.githubusercontent.com/123428884/223501073-bbdf858c-0388-420c-88b1-86f7b12c0248.png) <br />
According to the Decision Tree Model and its metrics, we can tell that 378 out of 383 instances are predicted, which is a fairly high rate. In addition, we can tell that Total day charge, Total eve charge, and Customer service calls are the three most important features. From the evaluation of the trained dataset, we can tell that the model discovered 72% customers that actually churned, whereas also 72% of predicted customers are those who churned in reality.

#### Decision Tree (with Feature Engineering and Selection)
![image](https://user-images.githubusercontent.com/123428884/223500076-230b4ddb-3ed2-4f15-bf74-dce5c1832a88.png)<br />
Comparing with the old version, the precision increased by 15.21% to 87.21%, while recall also rose to 75%. Besides, we can see from the decision tree that “Total charge” has now became the most important feature

#### Random Forest
![image](https://user-images.githubusercontent.com/123428884/223500181-75392e6c-74be-4bf3-b485-92d61e6c27d6.png)<br />
Comparing with Decision Tree Model, Random Forest Model has an even higher Precision, setting at 95.0%. Accuracy and Recall also increased by 1.2% and 1% respectively. In addition, to find out the most customers that would possibly churn, Recall would be the most important metric as it reflects this rate

#### Cross Validation
![image](https://user-images.githubusercontent.com/123428884/223500252-d6a51b9a-4f5a-40ed-9c53-5261afa55165.png)<br />
The average accuracy and average precision of cross-validation are in between the two previous models, while the average recall is the highest of all models. The difference between K-fold cross-validation and the other two models is caused because the method that was utilized.

#### Fine Tune
![image](https://user-images.githubusercontent.com/123428884/223502620-8c6c7dcd-a6f4-41cf-be77-6399d8504e3b.png)

#### Check Overfitting
![image](https://user-images.githubusercontent.com/123428884/223502712-f6e73222-ec87-45b1-9e37-48b6f9a79442.png)

<a name="interpr"></a>
## Business Interpretation

Based on the the confusion matrix of each model, we are able to assign a value to each possible output using the assumption about 5GBoost to choose the model that maximizes the profit of the company.

### Assumption
- Number of customers: 100,000
- Monthly plan per customer: $60
- Marketing effort to retain one customer: $20
- Cost of Acquisition: $70
- 70% of customers who want to churn will stay because of targeted marketing plan
- Company will have to acquire new customers to replace those churned

With the assumption, we can get
- True Positive: $22 (70% * $60 – 100% * $20)
- True Negative: $60
- False Positive: $40 ($60 - $20)
- False Negative: -$70

### How much do you estimate that the profit of 5GBoost would change the month after the model is deployed?
Net Profit of Each Model
- Decision Tree<br />
![image](https://user-images.githubusercontent.com/123428884/223506156-95db14b4-a78b-4bf1-9933-0215c8498234.png)
- Decision Tree with Feature Engineering<br />
![image](https://user-images.githubusercontent.com/123428884/223506010-45922b9b-62f0-43e0-ba92-ad154f313356.png)
- Random Forest<br />
![image](https://user-images.githubusercontent.com/123428884/223506220-e27eba51-a374-4944-ae5c-5aaaf272d7a7.png)<br />
<br />
We can tell that Decision Tree Model with Feature Engineering has the highest net profit. <br />
<br />
The net profit for this month would be $4,115,000 (100,000*85.5%*$60 -100,000*14.5%*$70 ). Therefore, we can estimate that the profit of 5GBoost would increase for $937,473.76.
