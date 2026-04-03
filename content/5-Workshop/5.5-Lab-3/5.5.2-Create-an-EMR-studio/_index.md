---
title : "3.2 Create an EMR Studio and attach it to the EMR cluster"
date : 2026-03-25
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

**A. Get your EMR cluster's VPC and Subnet ID**

1.  Go to the [Amazon EMR Console](https://console.aws.amazon.com/emr/home). In the left navigation pane, under **EMR on EC2**, choose **Clusters**.

2.  Click on your cluster's **Cluster ID** to open its details page. On the cluster details page, go to the **Summary** tab and scroll down to the **Network and security** section. Copy both the **VPC ID** and **Subnet ID** — you will need them when creating the Studio.

---

**B. Create the EMR Studio and Workspace**

1.  In the left navigation pane, still under **EMR on EC2**, choose **Studios**. Then click **Create Studio**.

    ![EMRStudioPage](/images/5-Workshops/5.5/5.5.2/1.png)
    ![EMRStudioPage2](/images/5-Workshops/5.5/5.5.2/2.png)

2.  On the **Create a Studio** page:
    - Select **Custom setup**.
    - Click **Browse S3** to select your S3 bucket as the storage location for notebooks.
    - Under **Service role**, choose **EMR\_Iceberg\_Notebook\_Role**.

    ![Create Studio Bucket/Role](/images/5-Workshops/5.5/5.5.2/3.png)

3.  Scroll down to the **Networking and security** section. Select the **VPC ID** and **Subnet ID** you copied from Step A.2. Then click **Create Studio**.

    ![Create Studio Bucket/Role](/images/5-Workshops/5.5/5.5.2/4.png)
    ![Create Studio Bucket/Role2](/images/5-Workshops/5.5/5.5.2/5.png)   

4.  After the Studio is created, click **Launch Studio** to open it in a new browser tab. You will be presented with the JupyterLab interface.

    > **Note:** If a new tab does not open automatically, check your browser's popup blocker and allow popups for the AWS Console domain.

5.  Inside the JupyterLab interface, click the **Cluster** icon (or **Compute** icon) in the left sidebar. Under **Compute type**, select **EMR on EC2 cluster**, then choose your cluster from the dropdown list. Click **Attach** at the bottom of the panel to connect the cluster to your workspace.

    ![Create Studio Bucket/Role](https://static.us-east-1.prod.workshops.aws/public/ebc00129-fa83-4c1d-931e-1f301fc04542/static/AttachCluster.png)


**Congratulations! You have completed the EMR Studio/Workspace setup, now it's time to explore Iceberg features on EMR.**

