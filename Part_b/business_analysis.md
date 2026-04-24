# Part B: Business Case Analysis
## Scenario: Promotion Effectiveness at a Fashion Retail Chain
### B1. Problem Formulation
(a) The target variable is Items_Sold, which represents the total number of units sold for a given store, month, promotion, and product category. This clearly defines sales volume without ambiguity. The input features include Location_Type, Month, Promotion_Type, Store_Size, Local_Competition, Event_Flag, Event_Type, Avg_Customer_Age, Gender_Distribution, Avg_Income_Level, Customer_Segment, and Product_Category.

This is a Supervised Learning regression problem, as we are predicting a continuous value. This approach is justified because the business goal is to maximise sales by selecting the best promotion, which requires predicting the number of items sold under different conditions. Features like product category, seasonality, events, and customer demographics help capture real-world demand patterns.

(b) Items sold (sales volume) is a more reliable target because it directly reflects customer demand, while price and discount act as influencing factors captured in the input features. Revenue can be misleading due to price fluctuations, but items sold clearly shows how demand changes across different promotions. This also illustrates that the target variable should be closely aligned with the business objective, which in this case is selecting the most effective promotion for each store. Choosing items sold ensures the model directly supports this decision by identifying which promotion leads to higher sales, whereas a poorly chosen target could result in incorrect promotion strategies despite good model performance.

(c) A single global model may not perform well because stores in different locations can respond differently to the same promotion. An alternative approach is to group similar stores based on factors like location, customer demographics, and sales patterns using business logic or clustering.
Then, separate regression models can be trained for each group. For example, a flat discount may work better in urban stores, while a buy-one-get-one (BOGO) offer may be more effective in rural stores for the same product. This approach captures group-specific behaviour and leads to more accurate predictions and better promotion decisions.

### B2. Data and EDA Strategy
(a) The tables can be joined using Store_ID and Date, where transactions are linked with store attributes using Store_ID, and with promotion and calendar data using Date.
The grain of the dataset is one row per store, month, promotion, and product category. For example, one row can represent Store A, January, Flat Discount, TV category with total items sold.
Before modelling, transaction data should be aggregated to calculate total items sold and summary features like average customer metrics and event indicators for each row.

(b) Before building the model, I would perform EDA to understand data quality, patterns, and relationships. I would check missing values, outliers, and consistency in the dataset.
1.	Promotion Type vs Items Sold (Boxplot or Bar Chart): We check which promotions lead to higher sales and how consistent they are. This helps in selecting important features and possibly creating interaction features between promotion and location.
2.	Month vs Items Sold (Line Plot): We look for seasonality patterns such as higher sales in certain months. This helps in creating time-based features or seasonal indicators.
3.	Location Type vs Items Sold (Boxplot): We analyze how sales differ across urban, semi-urban, and rural stores. This can justify grouping stores or adding interaction features with promotion type.
4.	Product Category vs Items Sold (Bar Chart): We examine which categories sell more and how they respond to promotions. This helps in creating category-level features and improving model accuracy.

(c) Since 80% of the transactions have no promotion, the model may become biased and mostly learn patterns for non-promotion cases, ignoring how promotions actually impact sales.
To handle this, we can use resampling techniques. For example, if we have 800 no-promotion and 200 promotion records, we can either reduce no-promotion data to 200 (under sampling) or increase promotion data to 800 (oversampling) to balance the dataset. This helps the model learn the true effect of promotions more accurately.

### B3. Model Evaluation and Deployment
 (a) Since the data is time-based (monthly over three years), I would use a time-based split by training the model on earlier periods and testing on later periods (for example, train on the first 2.5 years and test on the last 6 months). A random split is inappropriate because it can mix past and future data, leading to data leakage.
 
I would use evaluation metrics like RMSE and MAE to measure prediction accuracy. RMSE helps penalize large errors, while MAE provides an easily interpretable average error. I would also consider MAPE or weighted MAE to reflect business impact, especially for high-sales stores or key product categories, ensuring the model supports better promotion decisions.

(b) Feature importance can show that during high-demand periods (like festive seasons), factors such as Month and Event_Type dominate, indicating strong natural demand across customer segments. In such cases, promotions like loyalty bonuses are preferred because customers are already willing to spend, and the goal is to retain them and encourage repeat purchases rather than reduce prices.

In contrast, during low-demand periods, features like Avg_Income_Level and Customer_Segment become more important, showing higher price sensitivity among certain groups. This leads to recommendations like flat discounts to stimulate demand. I would explain to the marketing team that loyalty bonuses are suggested during peak periods to maximize long-term customer value, while discounts are used during slower periods to drive immediate sales.

(c) The trained model needs to generate promotion recommendations at the start of every month for all 50 stores without being retrained each time. After training once, the model is saved (for example as a .pkl file) so it can be reused.

Each month, new data for all stores is prepared in the same format as the training data, including features like location, product category, promotion options (such as discount, BOGO, or loyalty points), events, and customer details. This new data is not used for training but is passed as input to the saved model to generate predictions (items sold) for different promotion options.

For each store, the model predicts the number of items sold for each possible promotion. These predictions are then compared, and the promotion that gives the highest predicted sales is selected for that specific store and month. The model’s performance is monitored over time using metrics like RMSE or MAE, and it is retrained only when performance drops due to changes in trends or customer behavior.











