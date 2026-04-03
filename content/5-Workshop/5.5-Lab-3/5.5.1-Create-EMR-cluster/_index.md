---
title : "3.1 Create EMR cluster"
date : 2026-03-25
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

If you are running this workshop in your own account you need to follow below steps to create the EMR Cluster.

1.  Navigate to the [Amazon EMR Console](https://console.aws.amazon.com/emr/home). In the left navigation pane, under **EMR on EC2**, choose **Clusters**.

---

2.  Click on **Create cluster** button in the top-right corner.

![click-create-cluster](/images/5-Workshops/5.5/5.5.1/1.png)

---

3.  On the **Create cluster** page, under **Name and applications**, enter a descriptive **Cluster name**.

![go-to-advance-option](/images/5-Workshops/5.5/5.5.1/3.png)

4.  Under **Amazon EMR release**, select `emr-7.5.0`. In the **Application bundle** section, choose **Custom** to manually select the following applications:
    
    1.  Hadoop
    2.  Hive
    3.  JupyterEnterpriseGateway
    4.  Spark
    5.  Livy

5.  Expand the **Software settings** section. From the **Edit software settings** dropdown, choose **Enter configuration** and paste the following JSON to enable Iceberg support:

```json
[{
  "Classification": "iceberg-defaults",
  "Properties": {
    "iceberg.enabled": "true"
  }
}]
```

> **Note:** This lab has been tested with `EMR 7.5.0`. EMR 7.x releases (7.5.0 and later) are also compatible. Using an earlier EMR version may result in unexpected behavior.

![Create-Cluster-Advanced-Options](/images/5-Workshops/5.5/5.5.1/2.png)

6.  Scroll down to the **Cluster termination** section. By default, **Automatically terminate cluster after idle time** is enabled. **Disable** this option to keep the cluster running for the duration of this workshop.

![Create-Cluster-Advanced-Options](/images/5-Workshops/5.5/5.5.1/4.png)

7.  Leave all other settings at their defaults. Click **Create cluster** to launch the EMR cluster. The cluster will pass through `Starting` → `Bootstrapping` → `Waiting` states. **Wait until the cluster status shows `Waiting`** (approximately 10–15 minutes) before proceeding to the next section.

---
