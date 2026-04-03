---
title : "1.1 View and download sample dataset"
date : 2026-03-25
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

We will be using a simulated version of the Amazon Product Reviews dataset for the Athena and EMR labs, spend some time to get familiarized with this dataset.

**Amazon Customer Reviews Dataset**

Amazon Customer Reviews (a.k.a. Product Reviews) is one of Amazon’s iconic products. In a period of over two decades since the first review in 1995, millions of Amazon customers have contributed over a hundred million reviews to express opinions and describe their experiences regarding products on the Amazon.com website. This makes Amazon Customer Reviews a rich source of information for academic researchers in the fields of Natural Language Processing (NLP), Information Retrieval (IR), and Machine Learning (ML), amongst others. Accordingly, we are releasing this data to further research in multiple disciplines related to understanding customer product experiences. Specifically, this dataset was constructed to represent a sample of customer evaluations and opinions, variation in the perception of a product across geographical regions, and promotional intent or bias in reviews.

NOTE: The dataset we will be using is simulated and contains random numbers, names, and words.

**Download the sample dataset to your local machine and upload to S3**

1.  Download the dataset from one of the links below
    
    -   [Sample Data - us-east-1](https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/e57af944-04b9-41f5-8285-90295ed32bdc/simulatedproductreviews.parquet)
        
    -   [Sample Data - us-west-2](https://ws-assets-prod-iad-r-pdx-f3b3f9f1a7d6a3d0.s3.us-west-2.amazonaws.com/e57af944-04b9-41f5-8285-90295ed32bdc/simulatedproductreviews.parquet)
        
2.  Navigate to your [AWS S3 console](https://s3.console.aws.amazon.com/s3/buckets) 
    
3.  Click the name of the S3 bucket created for you
    
4.  Click **Create folder**
    
![create folder](/images/5-Workshops/5.3/5.3.1/3.png)

5.  Enter **productreviews** as Folder name then click **Create folder** button
    
6.  Navigate into the new folder by selecting **productreviews** then click the **Upload** button
    
7.  Click **Add files**, select the dataset you downloaded from step 1 above, then click **Upload**

![upload file](/images/5-Workshops/5.3/5.3.1/4.png)
