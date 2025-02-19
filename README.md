# Churn Analysis For Telecommunications Industry Using Power BI  

![Alt Text](https://github.com/01-vivek-kumar/Churn-Analysis/blob/main/Project1.png))


## 1. Introduction  

Customer churn is a major challenge for businesses, especially in competitive industries like telecommunications. In this project, I focused on **Churn Analysis** to understand why customers leave and what factors contribute to their decisions. My goal was to uncover key trends and patterns that could help in designing better marketing campaigns and improving customer retention strategies.  

To achieve this, I **visualized and analyzed customer data** at different levels:  
- **Demographics** – Age, gender, customer type.  
- **Geographics** – Region, city, and customer location.  
- **Payment & Account Information** – Billing method, tenure, contract type.  
- **Services Used** – Internet, phone, and additional services.  

Through this analysis, I studied the **churner profile** and identified areas where businesses can take action to reduce churn and retain customers.  

This project is focused on the **telecom industry**, but the techniques and insights can be applied across various sectors like **retail, finance, and healthcare** to enhance customer engagement and retention.  

### Why Power BI?  
I chose **Power BI** for this project because of its powerful capabilities in:  
- 🟢 **Data visualization** – Creating interactive dashboards for better insights.  
- 🟢 **Ease of use** – Handling complex data relationships with **DAX**.  
- 🟢 **Real-time dashboards** – Enabling dynamic analysis for decision-making.  

The dataset I used was sourced from **PivotalStats**, a platform that provides rich business datasets for analytics.  


## 2. Dataset Overview  

For this **Churn Analysis project**, I used a dataset sourced from **PivotalStats**. The dataset consists of **6,418 rows and 32 columns**, containing detailed customer information such as demographics, account details, and service usage.  

### **Key Features in the Dataset**  
The dataset includes the following important features:  
- **Customer Information:** `Customer_ID`, `Gender`, `Age`, `Married`, `State`, `Number_of_Referrals`  
- **Account Details:** `Tenure_in_Months`, `Contract`, `Payment_Method`, `Monthly_Charge`, `Total_Charges`  
- **Services Used:** `Phone_Service`, `Multiple_Lines`, `Internet_Service`, `Online_Security`, `Streaming_TV`, `Streaming_Movies`
  
- **Churn Identification:**  
  - `Churn_Category` – Initially contained **"Yes" and "No"** values.  
  - To make it more useful for analysis, I **converted** this into a **numerical format** (`1` for churned customers and `0` for retained customers).  
  - `Churn_Reason` – Provides additional details on why a customer churned.  


 ## 3. Data Preprocessing & Cleaning  

Before analyzing and visualizing the data, I performed **data preprocessing** to ensure accuracy and consistency. The key steps included handling missing values, transforming features, and cleaning inconsistencies.  

### **3.1 Handling Missing Values**  
Since the number of missing values was relatively small, I replaced them using appropriate methods instead of dropping them.  
- **Categorical columns** (e.g., `Multiple_Lines`, `Internet_Type`, `Online_Security`): Missing values were filled with the most frequent (mode) value.  
- **Numerical columns**: Filled using median or mean based on the distribution.  

### **3.2 Feature Transformation**  
To improve visualization and interpretation, I transformed some columns using **Power Query M language** in Power BI.  

#### **Churn Mapping**  
- Converted `Churn_Category` from `"Yes"/"No"` to `1/0` for numerical analysis.  

#### **Tenure Grouping**  
Since `Tenure_in_Months` had many discrete values, I grouped customers into tenure segments for better visualization:  

```m
= Table.AddColumn(#"Removed Duplicates", "Tenure Group", 
    each if [Tenure_in_Months] < 6 then "< 6 Months" 
    else if [Tenure_in_Months] < 12 then "6-12 Months" 
    else if [Tenure_in_Months] < 18 then "12-18 Months" 
    else if [Tenure_in_Months] < 24 then "18-24 Months" 
    else ">= 24 Months")
```
#### **Age Grouping**  

I categorized customers into different age brackets to make insights clearer:
```powerbi 
= Table.AddColumn(#"Removed Other Columns", "Age Group", 
    each if [Age] < 20 then "< 20" 
    else if [Age] < 36 then "20 – 35" 
    else if [Age] < 51 then "36 – 50" 
    else "> 50")
```


### **3.3 Data Consistency & Cleaning**   

Removed Duplicates to avoid redundant data affecting insights.
Checked for Inconsistent Values (e.g., ensuring no negative tenure, incorrect payment details).

 ## 4. Metrics & Measures  

To analyze customer churn effectively, I created key **DAX measures** in Power BI to track customer trends.  

### **4.1 Key Metrics**  
- **Total Customers** → The total number of active customers in the dataset.  
- **Total Churn & Churn Rate** → The absolute number of churned customers and their percentage.  
- **New Joiners** → Customers who recently joined the service.  

### **4.2 DAX Measures Used**  
I created the following **DAX measures** to calculate these metrics dynamically:  

#### **Total Customers**  
Counts the total number of unique customers.  
```DAX
Total Customers = COUNT(prod_Churn[Customer_ID])

```
#### **Total Churns**  

Summing the Churn Status column where churn is mapped as 1 (Yes) and 0 (No).
```
Total Churns = SUM(prod_Churn[Churn Status])
```
#### **New Joiners**  

Calculates the number of new customers who recently joined.

```
New Joiners = CALCULATE(
    COUNT(prod_Churn[Customer_ID]), 
    prod_Churn[Customer_Status] = "Joined"
)

```
#### **Churn Rate (%)**  

Calculates the percentage of churned customers.

```
Churn Rate = [Total Churns] / [Total Customers] * 100
```

4.3 Importance of These Metrics
These KPIs helped me track:
✅ Customer Growth – Monitoring total customers & new joiners.
✅ Churn Behavior – Understanding how many customers are leaving.
✅ Retention Strategies – Identifying key areas to reduce churn.

With these core metrics in place, I proceeded to Exploratory Data Analysis (EDA) to gain deeper insights into churn patterns. 

## 5. Exploratory Data Analysis (EDA)  

To understand customer churn better, I analyzed key factors such as **demographics, geography, and services used.**  

### **5.1 Demographic Analysis**  
- **Gender-Based Churn**  
  - **Female churn rate:** **64.15%**  
  - **Male churn rate:** **35.85%**  
  - Across all age groups, the **female churn rate was consistently higher than male churn.**  

### **5.2 Geographic Analysis**  
- **Highest Churn Region:** **Jammu & Kashmir** with a **57% churn rate**  
- **Top 5 states with the highest churn:**  
  - **Jammu & Kashmir** → **57%**  
  - **Assam**  
  - **Jharkhand**  
  - **Chhattisgarh**  
  - **Delhi**  

### **5.3 Service-Based Churn**  
- Certain services had a significant impact on churn:  
  - **Internet Service Subscribers** → **93.71% churn**  
  - **Phone Service Users** → **90.59% churn**  
  - **Unlimited Data Users** → **80.08% churn**  

### **5.4 Overall Churn Patterns & Key Insights**  
- **Top Churn Reasons by Category:**  
  - **Competitor Influence** was the **#1 reason for churn.**  
  - Surprisingly, the **second major churn factor** was related to **attitude issues**:  
    - **Attitude of service provider**  
    - **Attitude of service personnel**  
  - Initially, I expected dissatisfaction due to price, but customer experience played a more critical role.  

These insights helped in identifying key areas where **customer retention strategies** could be applied.  

With a clear understanding of **who churns and why**, the next step was **building dashboards** in Power BI to visualize and track these trends effectively. 🚀  

## 6. Power BI Dashboard & Visualizations  

![Alt Text](https://github.com/01-vivek-kumar/Churn-Analysis/blob/main/Dashboard.png)


### **6.1 Dashboard Overview**  
I created an interactive **Power BI dashboard** with the following key pages:  

1. **Summary Page**  
   - Provides a **high-level view** of churn trends.  
   - Displays **Total Customers, New Joiners, Total Churn, and Churn Rate** using **Top Cards** for quick insights.  
   - Includes a **bar chart showing churn by category** with a **tooltip for detailed churn reasons.**  

2. **Churn Reason Analysis**  
   - A deep dive into **why customers churn.**  
   - Uses a **tooltip-enabled visual** to display **churn reasons** within the Summary Page.  

---

### **6.2 Key Metrics Tracked**  
📌 **Top Cards (Summary Page)**  
- **Total Customers**  
- **New Joiners**  
- **Total Churn**  
- **Churn Rate**  

📌 **Demographics Analysis**  
- **Gender-based churn rate**  
- **Age group-wise total customers & churn rate**  

📌 **Account Information**  
- **Churn rate by Payment Method**  
- **Churn rate by Contract Type**  
- **Tenure Group-wise Total Customers & Churn Rate**  

📌 **Geographic Distribution**  
- **Top 5 states with the highest churn rate**  

📌 **Churn Distribution**  
- **Churn Category vs. Total Churn (Bar Chart)**  
- **Churn Reason Breakdown (Tooltip enabled)**  

📌 **Service Usage Impact on Churn**  
- **Internet Type vs. Churn Rate**  
- **Service Subscription Status & Churn Rate**  

---

### **6.3 Key Visuals Used**  
To make churn analysis **clear and actionable**, I used:  
✅ **Bar Chart** → For comparing churn across categories (e.g., tenure, churn reasons)  
✅ **Donut Chart** → For churn rate distribution across demographics  
✅ **Clustered Bar Chart** → To compare service-based churn rates  
✅ **Tables** → To display detailed churn metrics  

---

### **6.4 Interactive Features & Filters**  
To enhance user interactivity, I added:  
🔹 **Slicers** for:  
- **Monthly Charges Range**  
- **Marital Status**  

🔹 **Tooltips** for deep insights into **churn reasons** when hovering over churn categories in the Summary Page.  

## 7. Business Impact & Recommendations  

### **7.1 Business Impact of Churn**  
Customer churn has a **direct impact on revenue, profitability, and customer acquisition costs.** Key business impacts identified in this analysis include:  

🔹 **Revenue Loss** → High churn rate leads to **recurring revenue loss**, impacting overall business growth.  
🔹 **Increased Customer Acquisition Cost (CAC)** → Losing customers means **higher expenses** to acquire new ones.  
🔹 **Brand Reputation** → Negative customer experiences (e.g., service issues) may **hurt brand loyalty** and affect future sales.  
🔹 **Operational Costs** → Churn due to **service dissatisfaction** suggests a need for **improved customer support & retention efforts.**  

---

### **7.2 Key Insights from Churn Analysis**  
🔹 **Competitor Influence is the #1 Reason for Churn**  
   - A **large portion of customers** switched due to competitors offering better deals or services.  

🔹 **Attitude & Service Quality Matter**  
   - **Attitude-related churn factors** (service provider & support experience) were unexpectedly high, indicating dissatisfaction.  
   - Customers are not only leaving due to pricing but also due to **poor customer service interactions.**  

🔹 **Demographics & Geography Play a Role**  
   - **Female customers** showed a **higher churn rate** across all age groups.  
   - **States like Jammu & Kashmir, Assam, and Jharkhand** had the highest churn percentages.  

🔹 **Service Usage & Contract Type Impact Churn**  
   - Customers using **internet services had a 93.71% churn association**, highlighting possible **network reliability concerns.**  
   - **Shorter contracts** correlated with **higher churn rates**, showing the need for **long-term engagement incentives.**  

---

### **7.3 Recommendations for Reducing Churn**  
✔️ **Competitor Analysis & Pricing Strategy**  
   - Conduct a detailed **benchmarking analysis** against competitors.  
   - Introduce **customer loyalty programs** and personalized discounts.  

✔️ **Improve Customer Support & Engagement**  
   - Invest in **customer service training** to improve agent interactions.  
   - Implement **automated chatbots & support solutions** to address common issues quickly.  

✔️ **Targeted Retention Campaigns**  
   - Focus on **high-risk churn segments (e.g., females, short-tenure customers).**  
   - Launch **personalized offers** to retain customers likely to churn.  

✔️ **Long-Term Contract Incentives**  
   - Offer **discounts on annual contracts** to **reduce short-term churn.**  
   - Educate customers on the **value of staying longer with better deals.**  

✔️ **Network & Service Quality Enhancements**  
   - Improve **internet reliability** in high-churn regions.  
   - Offer proactive **service guarantees** for frequent complaint areas.  

---

### **7.4 Marketing & Retention Strategies**  
🎯 **Re-Engagement Campaigns**  
   - Use **targeted email & SMS campaigns** for customers at high risk of churn.  
   - Offer **personalized discounts or better service plans.**  

🎯 **Proactive Customer Outreach**  
   - Reach out to **customers in their first 6 months** (high churn risk).  
   - Provide **educational resources, onboarding benefits, and check-in calls.**  

🎯 **Localized Offers for High-Churn Regions**  
   - Create special **regional offers** for **Jammu & Kashmir, Assam, and Jharkhand** to retain customers.  

🎯 **Value-Based Communication**  
   - Highlight **network improvements** and **customer service enhancements** in marketing messages.  
   - Run campaigns showcasing **existing customer success stories.**  


 ## 📌 8. Final Project Summary  

### **Project Overview**  
This Power BI project focused on **Churn Analysis for a Telecom Firm**, identifying key churn drivers and providing **actionable insights** for customer retention. The project involved **data cleaning, transformation, visualization, and deployment** to create an interactive dashboard.  

 ### **Project Impact**  
📊 **Data-Driven Decisions:** Helps stakeholders identify churn patterns.  
📈 **Marketing Optimization:** Enables **targeted campaigns** to reduce churn.  
⚡ **Improved Customer Retention Strategies:** Data-backed insights to enhance **service quality & customer satisfaction.**  


## 📜 9. Final Thoughts  
This project was an insightful deep dive into **churn analytics**, leveraging **Power BI’s visualization capabilities** to translate raw data into meaningful business insights. The dashboard is a **real-time decision-making tool**, enabling proactive churn reduction strategies.  

📌 **Thanks for exploring my Power BI Churn Analysis project!** Feel free to connect or provide feedback.  














