---
title: "Blog 4"
date: 2026-03-12
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Secure AI agents with Policy in Amazon Bedrock AgentCore

Deploying AI agents safely in regulated industries is a major challenge because these systems can access sensitive data, call tools, and take actions with real-world impact. Unlike traditional software, an AI agent does not simply follow a fixed sequence of instructions. Instead, it can decide which tools to call, what data to retrieve, and how to respond based on user input and environmental context. This flexibility makes agents powerful, but it also introduces new security risks such as unauthorized data access, unintended transactions, prompt injection, and misuse of connected systems. In the AWS blog post *Secure AI agents with Policy in Amazon Bedrock AgentCore*, the authors explain how **Policy in Amazon Bedrock AgentCore** provides a deterministic enforcement layer that secures AI agents independently of the agent’s own reasoning. 

The post uses a **healthcare appointment scheduling agent** as a concrete example. Healthcare is a highly relevant domain because agents in this environment may handle protected health information (PHI), appointment scheduling, and access to patient records. In such a setting, it is not enough to rely on the model itself to behave safely. Instead, organizations need a mechanism that consistently enforces who can access what, under which conditions, and with what restrictions. Policy in Amazon Bedrock AgentCore is designed to provide exactly that kind of control by operating at runtime through the gateway layer. 

---

## Why AI agents need external policy enforcement

A key argument in the blog is that securing AI agents is more difficult than securing conventional applications. Traditional software usually has clearly defined code paths, while agents rely on large language models (LLMs) that can reason in open-ended ways. This means the same model might behave differently depending on prompts, tool results, surrounding context, or even adversarial input. Since LLMs can hallucinate and do not inherently separate trusted instructions from untrusted text, agents are vulnerable to prompt injection and related attacks. 

One common way to reduce risk is to wrap agents in application code that limits which tools can be called and under what circumstances. However, this approach has drawbacks. Security logic becomes scattered across the codebase, making it harder to review, audit, and maintain. It also means that the correctness of the wrapper code itself becomes part of the security boundary. If there is a bug in that code, the agent may still perform unsafe operations. The AWS post proposes a different model: move policy enforcement completely outside the agent and enforce it before any tool invocation takes place. 

This separation is important because it means the policy remains in effect regardless of how the agent is prompted, what the agent “believes,” or whether the application logic contains flaws. The gateway evaluates every request against defined policies before allowing access to tools. As a result, security rules become explicit, auditable, and independent of the LLM’s behavior. 

---

## Cedar as the foundation for deterministic policies

To make external policy enforcement practical, Amazon Bedrock AgentCore uses **Cedar**, an authorization language designed to be both machine-efficient and understandable by humans. Cedar policies describe three core elements: the **principal** (who is making the request), the **action** (what operation is being attempted), and the **resource** (what is being accessed). Conditions can then be added in a `when` clause to evaluate runtime context. 

The AWS blog highlights several reasons Cedar is a strong fit for agent security. First, it has a **default-deny** posture, which means any request is denied unless there is an explicit rule allowing it. Second, **forbid rules override permit rules**, which gives security teams a powerful way to hard-stop dangerous patterns even when broader permissions exist. Third, Cedar is designed to be side-effect free and loop-free, which makes evaluation fast, predictable, and easier to analyze formally. These properties support deterministic enforcement, something that is especially valuable when working with highly dynamic AI agents.

The blog also explains that policies in AgentCore can be authored in multiple ways. Developers and security teams can write Cedar directly for fine-grained control, generate Cedar from natural-language policy descriptions, or use form-based tools to define the required rules. This flexibility lowers the barrier to adoption while still preserving precise enforcement semantics.

---

## Policy in Amazon Bedrock AgentCore

Policy in Amazon Bedrock AgentCore works by evaluating every agent-to-tool request against a defined **policy engine**. This engine is a collection of Cedar policies attached to an **AgentCore Gateway**. At runtime, the gateway intercepts requests and decides whether they should be allowed or denied. This means the gateway becomes the enforcement point between the agent and the tools it wants to call. 

According to the post, the service supports not only direct Cedar authoring, but also generation of Cedar from plain English policy statements. The generated policies are checked for syntactic correctness, validated against the gateway schema, and analyzed for potential problems such as being overly permissive or overly restrictive. This capability helps teams convert business rules into enforceable controls while reducing the chance of misconfiguration. 

A practical benefit of this model is that organizations can first attach a policy engine in **LOG_ONLY** mode. In that mode, they can observe how policies would behave without actually blocking production traffic. Once they confirm the policies produce the intended behavior, they can switch to enforcement mode and actively govern runtime access. This staged rollout is useful for enterprises that want to introduce strict controls without disrupting critical systems. 

---

## Healthcare appointment scheduling agent example

The blog demonstrates these ideas with a healthcare appointment scheduling agent. This agent supports several tools, including:

- `getPatient` to retrieve a patient record
- `searchImmunization` to query immunization records
- `bookAppointment` to schedule an appointment
- `getSlots` to retrieve available appointment slots

Because these tools expose sensitive capabilities, the agent must be governed carefully. The authors show how multiple policy patterns can be combined into a secure and auditable policy set.

### Identity-based policies

The first pattern is **identity-scoped access**. In a healthcare system, a patient should only be able to access their own records. For example, when the agent calls `getPatient`, the `patient_id` supplied in the request must match the authenticated user’s patient ID. A similar rule applies to immunization searches. This ensures that users cannot ask the agent to access someone else’s medical records simply by changing an input value. 

The blog explains that such rules can be written directly in Cedar or generated from natural-language descriptions. In both cases, the final policy compares the authenticated identity with the request parameters and only permits the action if they match.

### Read versus write separation

The second pattern is **scope-based separation of read and write access**. In many healthcare systems, read operations are broader than write operations. A user may be allowed to view certain information if they have a scope such as `fhir:read`, while booking appointments or modifying data may require a separate scope such as `appointment:write`. By separating permissions in this way, organizations can minimize the risk of unauthorized changes while still supporting necessary access to information. 

The post shows how a form-based policy creation flow can be used to create such rules by specifying the effect, principal, resource, action, and conditions. This is useful for teams that want structured policy authoring without writing Cedar manually from scratch.

### Risk controls on scheduling

The third pattern is the use of **explicit risk controls** to block dangerous or abusive requests. The blog gives an example policy that restricts access to appointment slots to a specific time range, such as between **9 AM and 9 PM UTC**. Requests made outside that window are denied. This kind of rule is not just about authorization; it also encodes business constraints and abuse prevention directly into the runtime enforcement layer.

This is where Cedar’s **forbid-wins** model becomes especially useful. Even if a more general permit rule exists, a targeted forbid rule can still block high-risk operations. That makes it easier to create layered protection that is both readable for auditors and enforceable at runtime.

---

## Testing the enforcement model

The AWS authors also walk through policy testing scenarios to show how the system behaves in practice. In one test, the authenticated user is `adult-patient-001` and asks the agent to retrieve information for the same patient ID. Because the request matches the authenticated identity, the policy decision is **ALLOW** and the patient record is returned.

In the next test, the same authenticated user requests patient information for `pediatric-patient-001`. The agent, model, and tool are unchanged, but the request parameter no longer matches the user’s identity. As a result, the gateway denies the request because no matching permit policy exists. This demonstrates that the security boundary is enforced consistently at the gateway, independent of the agent’s reasoning.

A second pair of tests examines time-based scheduling control. When a user requests appointment slots during permitted hours, such as **2 PM UTC**, the relevant forbid condition does not match, so the request is allowed if a permit rule applies. But when the same request is made at **3 AM UTC**, the forbid rule matches and the request is denied. These tests illustrate how identity-based permits and time-based forbids can work together to create deterministic security outcomes.

---

## Implementation and operational considerations

To try the example, the blog instructs readers to clone the **amazon-bedrock-agentcore-samples** repository and navigate to the healthcare appointment agent sample. The sample includes setup and deployment instructions for configuring the AWS environment, deploying the stack, and invoking the agent end to end.

The post also lists prerequisites, including an active AWS account, use of a Region where Policy in AgentCore is available, and appropriate IAM permissions for creating and managing policy engines, Cedar policies, policy generation resources, and gateway associations. AWS recommends scoping these permissions to specific resources in production rather than using broad wildcards.

Operationally, the recommended sequence is to create a policy engine, author policies using one of the supported methods, attach the engine to a gateway in LOG_ONLY mode, observe behavior through logs, and then move to full enforcement once the behavior is verified. This process helps teams adopt policy-based enforcement safely and systematically.

---

## Conclusion

The main message of the AWS blog is that AI agents are only as trustworthy as the boundaries surrounding them. Because agents can reason flexibly and invoke powerful tools, security controls must be enforced outside the model itself. Policy in Amazon Bedrock AgentCore addresses this challenge by placing a deterministic, auditable policy layer at the gateway, where every tool call is evaluated before execution.

By combining Cedar policies, default-deny behavior, forbid-overrides semantics, and runtime gateway enforcement, organizations can build agentic systems that are safer, more transparent, and easier to audit. The healthcare scheduling example shows how identity-based rules, scope separation, and business-risk controls can work together to protect sensitive systems. For enterprises operating in regulated domains, this separation between agent capability and security enforcement is a strong foundation for production-grade AI agents.

---

## About the Authors

**Bharathi Srinivasan** is a Generative AI Data Scientist at the AWS Worldwide Specialist Organization. She develops Responsible AI solutions with a focus on algorithmic fairness, large language model veracity, explainability, and governance of agents. She also advises internal AWS teams and customers on their Responsible AI journey and has presented her work at multiple machine learning conferences. 

**Anil Nadiminti** is a Senior Solutions Architect at AWS focused on the FinTech segment. He works with enterprise financial institutions and blockchain-native companies to deliver technical and strategic solutions across Web3 and decentralized finance. He is also passionate about Agentic AI and contributes to the AWS community through conferences, workshops, and open-source projects.

**Pushpinder Dua** is a Software Development Manager at AWS Agentic AI. He leads initiatives related to governance of agentic systems and the evolution of agentic protocols that aim to improve safety, reliability, and interoperability across agentic ecosystems.

**Jean-Baptiste Tristan** is a Principal Applied Scientist at AWS Agentic AI. His work focuses on agentic safety and security, combining automated reasoning with generative AI.