**Name:** RAPOLU SOWMYA  
**Company:** CODTECH IT SOLUTIONS  
**ID:** CT08DS3835  
**Domain:** Data Analytics  
**Duration:** July to August 2024  
**Mentor:** N.SANTOSH KUMAR


**Overview of the Project**

**Project: Customer Segmentation and Analysis using K-Means Clustering with Python**

**Objective:**  
The objective of this project is to perform customer segmentation and analysis using K-Means clustering with Python.Using clustering techniques, companies can identify the several segments of customers allowing them to target the potential user base.

**Key Activities:**

**A.Loading and Exploring Data:**       
1.The dataset is loaded into a Pandas DataFrame.       
2.Basic exploratory data analysis is performed, including examining the first five rows, checking the data shape, and reviewing data information for insights. 

**B.Data Preprocessing:**      
1.Missing values are checked and found to be absent.      
2.The relevant columns for clustering (Annual Income and Spending Score) are selected.   

**C.Determining Optimal Clusters:**   
1.The "Elbow Method" is employed to determine the optimal number of clusters.   
2.The Within-Cluster-Sum-of-Squares (WCSS) is plotted against the number of clusters.  

**D.Training K-Means Model:**      
1.The K-means clustering model is trained with the optimal number of clusters determined from the elbow method.     
2.Labels are assigned to each data point based on their respective clusters.      

**E.Visualization:**       
1.The clustered data points are visualized in a scatter plot.     
2.Each cluster is represented by a distinct color, making it easy to discern different customer groups.  
3.The plot illustrates the relationship between Annual Income and Spending Score for each customer segment.  

**Dataset:**
The dataset is aquired from kaggle  
1.The dataset consists of following five features of 200 customers  
2.CustomerID: Unique ID assigned to the customer  
3.Gender: Gender of the customer  
4.Age: Age of the customer   
5.Annual Income (k$): Annual Income of the customer  
6.Spending Score (1-100): Score assigned by the mall based on customer behavior and spending nature.  

**Libraries Used:**  
1.Pandas  
2.NumPy  
3.Matplotlib  
4.Seaborn  
5.Sklearn   

