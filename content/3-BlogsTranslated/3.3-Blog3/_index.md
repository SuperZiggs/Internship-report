---
title: "Blog 3"
date: 2026-03-13
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Twenty years of Amazon S3 and building what’s next

Amazon Simple Storage Service (Amazon S3) marks its 20th anniversary as one of the most foundational services in AWS. Originally launched on **March 14, 2006**, Amazon S3 began with a very simple value proposition: provide storage for the internet through a web service interface that allows developers to store and retrieve any amount of data, at any time, from anywhere on the web. At launch, the service was introduced with very little fanfare, but over time it became one of the most important building blocks in cloud computing.

The early vision behind Amazon S3 was not just to provide object storage, but to offer developers a simple and reliable primitive that removes operational burden. At its core, S3 introduced straightforward operations such as **PUT** for storing objects and **GET** for retrieving them later. However, the deeper innovation was the philosophy of abstracting away undifferentiated heavy lifting so builders could focus on creating applications and services instead of managing storage infrastructure.

From the very beginning, Amazon S3 was guided by five fundamental principles that still define the service today: **security, durability, availability, performance, and elasticity**. Security ensures data is protected by default. Durability is designed for **99.999999999% (11 nines)**. Availability is built into every layer of the system with the assumption that failures will occur and must be handled gracefully. Performance is designed so the service can store extremely large amounts of data without degradation, while elasticity allows S3 to automatically scale as data volumes increase or decrease without manual intervention. These principles have helped make S3 a service that “just works” for millions of customers.

---

## The early days of Amazon S3

When Amazon S3 first launched, it operated at a scale that was already significant for its time. The service initially offered about **one petabyte** of total storage capacity distributed across approximately **400 storage nodes**, **15 racks**, and **three data centers**, with around **15 Gbps** of total bandwidth. The system was designed to store **tens of billions of objects**, and the maximum object size at launch was **5 GB**. Pricing at that time was **$0.15 per gigabyte**.

Although those numbers were impressive in 2006, they are modest compared with the scale Amazon S3 supports today. Still, the original design choices laid the foundation for a service that could evolve dramatically over two decades without breaking backward compatibility for users. One of the most remarkable aspects of S3 is that code written for it in 2006 can still function today. Even though the infrastructure, storage hardware, and request-handling code have gone through multiple generations of change, AWS has maintained compatibility while continuing to innovate internally. 

---

## Amazon S3 today: scale beyond imagination

Twenty years later, Amazon S3 has expanded far beyond its original scale. Today, it stores **more than 500 trillion objects** and serves **more than 200 million requests per second** globally. It operates across **hundreds of exabytes of data** in **123 Availability Zones** spanning **39 AWS Regions**, serving **millions of customers** worldwide. The maximum object size has also increased dramatically, from **5 GB** at launch to **50 TB**, representing a **10,000-fold increase**. 

At the same time, the economics of storage have improved significantly. The price has fallen from **$0.15 per gigabyte** in 2006 to just over **$0.02 per gigabyte** today, which represents an approximate **85% price reduction**. AWS has also expanded storage cost optimization through multiple storage tiers. For example, customers using **Amazon S3 Intelligent-Tiering** have collectively saved more than **$6 billion** in storage costs compared with using **Amazon S3 Standard**. 

Another major milestone is the influence of the **S3 API** itself. Over the years, the API has become an industry reference point for object storage. Many vendors now provide S3-compatible storage systems and tools, meaning that knowledge, tooling, and practices developed around Amazon S3 often translate into broader storage environments as well. This widespread adoption reflects the long-term stability and influence of the service. 

---

## The engineering behind S3 at massive scale

The ability of Amazon S3 to operate at such enormous scale depends on continuous engineering innovation. A key part of S3 durability comes from a distributed system of microservices that continuously inspect every byte across the storage fleet. These auditor services detect signs of degradation and automatically trigger repair systems whenever necessary. AWS describes S3 as being designed to be **lossless**, with the 11 nines durability target reflecting how replication and repair systems are sized and operated. 

AWS also applies **formal methods and automated reasoning** to parts of the S3 production environment. For example, when engineers update code in the indexing subsystem, automated proofs are used to verify that consistency guarantees remain correct. Similar approaches are used in areas such as **cross-Region replication** and **access policies**, helping AWS mathematically validate critical behavior in a service where correctness is essential. 

Over the past **eight years**, AWS has also been progressively rewriting performance-critical components in the S3 request path using **Rust**. This includes blob movement and disk storage systems, with more work continuing across additional components. Rust provides not only strong runtime performance, but also compile-time memory safety and type guarantees that eliminate entire classes of bugs. At the scale of Amazon S3, these reliability and correctness benefits are especially valuable. 

Another design philosophy highlighted in the blog is that **scale itself becomes an advantage**. AWS engineers design S3 so that growth improves the overall system. As workloads increase and diversify, they become more de-correlated, which can improve reliability and robustness for all customers. This mindset is a major part of how S3 has continued to evolve while maintaining stability for two decades. 

---

## Looking forward: the future of Amazon S3

AWS now positions Amazon S3 as more than an object storage service. The long-term vision is for S3 to become the **universal foundation for all data and AI workloads**. The idea is that customers should be able to store any kind of data one time in S3 and then work with it directly, without moving that data into multiple specialized systems. This approach can reduce complexity, lower cost, and eliminate duplicate copies of data across environments. 

Recent launches show how AWS is extending S3 in that direction:

- **S3 Tables** provide fully managed **Apache Iceberg** tables with automated maintenance to improve query efficiency and reduce storage costs over time.
- **S3 Vectors** add native vector storage capabilities for **semantic search** and **retrieval-augmented generation (RAG)** workloads, supporting up to **2 billion vectors per index** with **sub-100 ms query latency**.
- **S3 Metadata** introduces centralized metadata capabilities for instant data discovery, reducing the need to recursively list very large buckets and improving time-to-insight for data lake scenarios. 

The blog also highlights strong adoption momentum for these newer capabilities. In just five months, from **July to December 2025**, customers created more than **250,000 indices**, ingested more than **40 billion vectors**, and executed more than **1 billion queries** with S3 Vectors. These capabilities are intended to extend S3’s reach into analytics and AI while preserving the service’s familiar cost structure and operational simplicity. 

---

## Conclusion

The 20-year history of Amazon S3 reflects both continuity and transformation. What began as a simple storage service for developers has grown into a global platform supporting hundreds of exabytes of data, hundreds of millions of requests per second, and a broad ecosystem of applications, analytics systems, and AI workloads. Despite this extraordinary growth, the core principles of **security, durability, availability, performance, and elasticity** remain unchanged. 

Amazon S3’s story is also notable because AWS has preserved backward compatibility while continuously redesigning the system internally. Customers benefit from new capabilities and improved economics without having to rewrite old applications. Looking ahead, AWS sees S3 not merely as storage, but as the common foundation on which future data-intensive and AI-driven systems can be built. 

---

## About the Author

**Sébastien Stormacq** is the author of this AWS News Blog post. He has been writing code since first using a Commodore 64 in the mid-1980s. At AWS, he focuses on inspiring builders to unlock value from the AWS Cloud through a combination of passion, customer advocacy, curiosity, and creativity. His areas of interest include **software architecture, developer tools, and mobile computing**. 