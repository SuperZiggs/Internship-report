---
title: "Blog 1"
date: 2025-12-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Introducing Amazon Nova Forge: Build Your Own Frontier Models Using Nova

Organizations are increasingly adopting generative AI in many parts of their operations, from internal productivity tools to advanced domain-specific applications. As this adoption grows, the limitations of general-purpose foundation models become more visible. Many enterprise scenarios require models that can understand private knowledge, internal terminology, specialized workflows, and business-specific requirements at a much deeper level than standard prompting can provide.

Common customization approaches such as **prompt engineering** and **Retrieval-Augmented Generation (RAG)** are useful for many applications, but they still do not fundamentally reshape the model’s internal representation of knowledge. They mainly help the model respond better at inference time rather than altering what the model has learned in a deeper sense. Other approaches like **supervised fine-tuning** and **reinforcement learning** also help customize a model, but these methods are often applied after the model has already been fully trained. At that stage, the model is harder to steer toward very specific domains of interest.

For organizations that want stronger customization, **Continued Pre-Training (CPT)** may appear to be a natural solution. However, if CPT is done only on proprietary enterprise data, the model can experience **catastrophic forgetting**, where it becomes better at the new domain but loses foundational capabilities learned earlier. On top of that, training a frontier model from the beginning remains too expensive and resource-intensive for most organizations because it requires very large datasets, substantial computing infrastructure, and advanced training expertise. AWS introduced **Amazon Nova Forge** as a way to close this gap. With Nova Forge, customers can start from early Amazon Nova checkpoints, combine their own datasets with Amazon Nova-curated training data, and build custom frontier models hosted securely on AWS. AWS positions this as an easier and more cost-effective way to build domain-specific frontier models. 

---

## Use Cases and Applications

Amazon Nova Forge is designed for organizations that already have access to proprietary data, industry-specific knowledge, or specialized operational information and want to turn those assets into stronger AI capabilities. Rather than relying only on a generic model, these organizations can create models that are more aligned with the reality of their domain.

AWS highlights several example scenarios. In **manufacturing and automation**, organizations can build models that better understand specialized industrial processes, machine data, operational procedures, and equipment-related workflows. In **research and development**, companies can create models trained on internal research materials, private data collections, and domain-specific expertise that may not be captured by public datasets. In **content and media**, teams can develop models that align with a company’s brand voice, internal content standards, and moderation requirements. In **specialized industries** more broadly, Nova Forge can be used to train models that understand sector-specific language, regulations, best practices, and technical conventions.

Depending on the business objective, Nova Forge can help organizations create differentiated model capabilities, improve task-specific performance, reduce latency in production settings, and lower overall cost compared with approaches that require more extensive retraining or external model adaptation pipelines. 

---

## How Nova Forge Works

Amazon Nova Forge is designed to address the limitations of traditional model customization by allowing organizations to start development from **early checkpoints** in the model lifecycle. Instead of only working with a fully completed model, customers can begin from **pre-training**, **mid-training**, or **post-training** checkpoints. This provides much more flexibility in how a model is adapted to a specialized domain.

A core capability of Nova Forge is **data blending**. Organizations can combine their proprietary datasets with **Amazon Nova-curated data** across all training phases. AWS states that this training can be run using proven recipes on fully managed infrastructure in **Amazon SageMaker AI**. This means that customers do not need to assemble the entire training stack from scratch. Instead, they can rely on AWS-managed tooling and workflows while still shaping the model using their own data.

This data-mixing strategy is especially important because it helps reduce catastrophic forgetting compared with training only on raw proprietary data. According to AWS, blending curated data with organization-specific data helps preserve foundational skills such as core intelligence, general instruction-following ability, and built-in safety characteristics, while still enabling the model to absorb specialized enterprise knowledge. In practical terms, Nova Forge tries to balance two goals at once: retaining the broad usefulness of a powerful foundation model and adapting it more deeply to a specific domain. 

---

## Reinforcement Learning in Proprietary Environments

Nova Forge also supports **reinforcement learning (RL)** using reward functions defined in an organization’s own environment. This allows models to learn from feedback generated under conditions that match real use cases more closely than generic benchmark settings.

This capability is useful in situations where success depends on sequential decisions, environment-specific feedback, or business-defined reward logic. AWS explains that customers can use their own orchestrator to manage **multi-turn rollouts**, which makes it possible to support more advanced RL workflows, including complex agent behavior and sequential decision-making tasks.

AWS gives examples such as using chemistry tools to evaluate molecular designs or robotics simulations that reward efficient task completion while penalizing unsafe behavior like collisions. These examples show that Nova Forge is not limited to text-only adaptation. Instead, it can be used in settings where model improvement depends on interaction with specialized tools, simulators, or internal evaluation environments. That makes it relevant for research-heavy industries and advanced operational AI systems that need more than standard fine-tuning. 

---

## Responsible AI and Safety Configuration

Beyond training and customization, Nova Forge also includes a built-in **responsible AI toolkit**. This toolkit allows organizations to configure the **safety** and **content moderation** behavior of their custom models. AWS notes that customers can adjust these settings to match specific business needs in areas such as safety, security, and the handling of sensitive content.

This is a meaningful part of the platform because enterprise AI deployment is not only about model quality or benchmark performance. In many organizations, production readiness also depends on governance, moderation policies, and content risk controls. By including these options directly in Nova Forge, AWS provides a framework where organizations can work on model customization and model responsibility within the same broader environment. 

---

## Getting Started with Nova Forge

AWS explains that Nova Forge integrates with existing AWS AI workflows, especially those built around **Amazon SageMaker AI** and **Amazon Bedrock**. Customers can use familiar tools and infrastructure in SageMaker AI to run training jobs, then import the resulting custom Nova models into Amazon Bedrock as **private models**.

This integration matters because it allows organizations to continue using the same broader AWS environment for security, APIs, and operational consistency. Instead of building one system for training and another separate stack for serving, teams can move from model development to application deployment using services that are already connected within the AWS ecosystem. AWS specifically notes that this gives customers the same security model, consistent APIs, and broader integrations available to other models in Amazon Bedrock. 

In **Amazon SageMaker Studio**, users can now build frontier models using Amazon Nova. The high-level workflow described by AWS is straightforward. First, users choose which checkpoint they want to start from, such as a **pre-trained**, **mid-trained**, or **post-trained** checkpoint. They can then upload their own dataset or work with existing datasets already available in their workflow.

After selecting the starting checkpoint and data sources, users can blend their training data with curated datasets provided by Nova. AWS notes that these curated datasets are categorized by domain and are intended to help preserve general model performance while reducing the risk of overfitting or catastrophic forgetting. This stage is central to how Nova Forge balances specialization with retention of broad capability.

AWS also notes that users can optionally apply **Reinforcement Fine-Tuning (RFT)**. According to the blog, this can be used to improve factual accuracy and reduce hallucinations in specific domains. After training is complete, the resulting model can be imported into Amazon Bedrock and used in downstream applications. 

---

## Things to Know

At launch, **Amazon Nova Forge** is available in the **US East (N. Virginia)** AWS Region. AWS states that the offering includes access to multiple Nova model checkpoints, recipes for mixing proprietary data with Amazon Nova-curated training data, established training recipes, and integration with both Amazon SageMaker AI and Amazon Bedrock.

For users who want to explore the service further, AWS points to the **Amazon Nova User Guide** and the **Amazon SageMaker AI console** as primary starting points. AWS also notes that organizations looking for deeper or more specialized support can contact the **Generative AI Innovation Center** for assistance with model development initiatives. 

---

## Why This Launch Is Important

Amazon Nova Forge represents a significant development for organizations that want more control over model creation without taking on the full burden of frontier-model training from scratch. In many enterprise settings, the ideal solution is not a completely generic model and not a fully custom model built from zero. Instead, the ideal path is often a hybrid one: begin with a strong existing model, adapt it earlier and more deeply than standard fine-tuning allows, preserve its core capabilities, and then deploy it within a production-ready platform.

Nova Forge is designed around exactly that middle ground. It provides access to Amazon Nova checkpoints, curated data mixing, AWS-managed training infrastructure, reinforcement learning support in proprietary environments, configurable responsible AI settings, and deployment through Amazon Bedrock. Taken together, these capabilities make Nova Forge more than just a fine-tuning feature. It is positioned as a broader framework for building **domain-aware frontier models** that remain aligned with enterprise requirements for performance, safety, and operational integration. 

---

## Conclusion

As organizations continue to adopt generative AI, the need for models that deeply understand proprietary data and specialized domains will continue to grow. Standard customization methods remain useful, but they often do not go far enough for enterprises with complex internal knowledge, unique workflows, or high-stakes domain requirements.

Amazon Nova Forge addresses this need by allowing customers to start from early Nova checkpoints, blend their data with Amazon-curated datasets, train using managed SageMaker AI infrastructure, and deploy custom models through Amazon Bedrock. AWS presents this as an easier and more cost-effective way to build frontier models that combine broad foundational capability with deeper domain alignment.

For enterprises that want to move beyond lightweight adaptation and toward more serious model specialization, Nova Forge offers a practical path inside the AWS ecosystem. It reduces some of the biggest barriers traditionally associated with frontier-model development while still giving organizations meaningful control over how their models are trained, adapted, governed, and deployed.

## About the Author

**Danilo Poccia** works with startups and companies of different sizes to support innovation. In his role as **Chief Evangelist (EMEA) at Amazon Web Services**, he helps people bring ideas to life, with particular focus on **serverless architectures**, **event-driven programming**, and the technical and business impact of **machine learning** and **edge computing**. He is also the author of *AWS Lambda in Action*.