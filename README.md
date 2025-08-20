<!--
> [!NOTE]
> The content presented here serves as an example intended solely for educational objectives and should not be implemented in a live production environment without proper modifications and rigorous testing.
-->

# Guidance for Cost Analysis and Optimization with Amazon Bedrock Agents 


## Table of Contents
- 📋 [Overview](#overview)
- 🏗️ [Guidance overview](#guidance-overview)
- ✅ [Prerequisites](#prerequisites)
- ☁️ [AWS services in this Guidance](#aws-services-in-this-guidance)
- 💰 [Cost](#cost)
- 🔒 [Security](#security)
- 🌎 [Supported AWS Regions](#supported-aws-regions)
- 🚀 [Deploy the Guidance](#deploy-the-guidance)
<!--
- 💻 [Deploy the Amplify application](#deploy-the-amplify-application)
- 🔐 [Amazon Cognito for user authentication](#amazon-cognito-for-user-authentication)
- 🤖 [Amazon Bedrock Agents with multi-agent capability](#amazon-bedrock-agents-with-multi-agent-capability)
- ⚡ [Lambda functions for Amazon Bedrock action groups](#lambda-functions-for-amazon-bedrock-action-groups)
- 🌐 [Amplify for frontend](#amplify-for-frontend)
- 🔄 [Multi-agent and application walkthrough](#multi-agent-and-application-walkthrough)
- 🧹 [Clean up](#clean-up)
-->
- 💡 [Considerations](#considerations)
- 📝 [Conclusion](#conclusion)
- 📚 [Additional resources](#additional-resources)
- ➡️ [Next Steps](#next-steps)
- ❓ [FAQ, Known Issues, Additional Considerations, and Limitations](#faq-known-issues-additional-considerations-and-limitations)
- 📋 [Revisions](#revisions)
- ⚠️ [Notices](#notices)
- 👥 [Authors](#authors)
- 📜 [License](#license)

## Overview

AI agents are revolutionizing how businesses enhance their operational capabilities and enterprise applications. By enabling natural language interactions, these agents provide customers with a streamlined, personalized experience. [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) uses the capabilities of foundation models (FMs), combining them with APIs and data to process user requests, gather information, and execute specific tasks effectively. The introduction of now enables organizations to orchestrate multiple specialized AI agents working together to tackle complex, multi-step challenges that require diverse expertise. 

[Amazon Bedrock](https://aws.amazon.com/bedrock/) offers a diverse selection of FMs, allowing you to choose the one that best fits your specific use case. Among these offerings, [Amazon Nova](https://aws.amazon.com/ai/generative-ai/nova/) stands out as AWS's next-generation FM, delivering breakthrough intelligence and industry-leading performance at exceptional value.

The Amazon Nova family comprises two types of models:

* Understanding models – Available in Micro, Lite, and Pro variants
* Content generation models – Featuring Canvas and Reel

These models are specifically optimized for enterprise and business applications, excelling in the following capabilities:

* Text generation
* Summarization
* Complex reasoning tasks
* Content creation

This makes Amazon Nova ideal for sophisticated use cases like our FinOps solution.

A key advantage of the Amazon Nova model family is its [industry-leading price-performance ratio](https://aws.amazon.com/blogs/aws/introducing-amazon-nova-frontier-intelligence-and-industry-leading-price-performance/). Compared to other enterprise-grade AI models, Amazon Nova offers comparable or superior capabilities at a more competitive price point. This cost-effectiveness, combined with its versatility and performance, makes Amazon Nova an attractive choice for businesses looking to implement advanced AI solutions.

In this guidance, we use the [multi-agent](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-multi-agent-collaboration.html) feature of Amazon Bedrock to demonstrate a powerful and innovative approach to [AWS cost management](). By using the advanced capabilities of Amazon Nova FMs, we've developed a solution that showcases how AI-driven agents can revolutionize the way organizations analyze, optimize, and manage their AWS costs.

## Guidance overview

Our innovative AWS cost management guidance uses the power of AI and multi-agent collaboration to provide comprehensive cost analysis and optimization recommendations. The core of the system is built around three key components:

* FinOps supervisor agent – Acts as the central coordinator, managing user queries and orchestrating the activities of specialized subordinate agents
* Cost analysis agent – Uses [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) to gather and analyze cost data for specified time ranges
* Cost optimization agent – Uses the [AWS Trusted Advisor Cost Optimization Pillar](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/) to provide actionable cost-saving recommendations

The guidance integrates the multi-agent collaboration capabilities of Amazon Bedrock with Amazon Nova to create an intelligent, interactive, cost management AI assistant. This integration enables seamless communication between specialized agents, each focusing on different aspects of AWS cost management. Key features of the guidance include:

* User authentication through [Amazon Cognito](https://aws.amazon.com/cognito/) with [role-based access control](https://docs.aws.amazon.com/cognito/latest/developerguide/role-based-access-control.html)
* Frontend application hosted on [AWS Amplify](https://aws.amazon.com/amplify/)
* Real-time cost insights and historical analysis
* Actionable cost optimization recommendations
* Parallel processing of tasks for improved efficiency

By combining AI-driven analysis with AWS cost management tools, this guidance offers finance teams and cloud administrators a powerful, user-friendly interface to gain deep insights into AWS spending patterns and identify cost-saving opportunities.

The architecture displayed in the following diagram uses several AWS services, including [AWS Lambda](https://aws.amazon.com/lambda/) functions, to create a scalable, secure, and efficient system. This approach demonstrates the potential of AI-driven multi-agent systems to assist with cloud financial management and solve a wide range of cloud management challenges.

![cost_optimization_reference_architecture_final](assets/cost_optimization_reference_architecture_final.jpg)
*Figure 1. Reference Architecture of Cost Analysis and Optimization with Amazon Bedrock Agents*

<u>Architecture Workflow</u>:

1. The Administrator User deploys the guidance to AWS Account and Region using an AWS CloudFormation Template.
The Base AWS CloudFormation stack will deploy and create all of the AWS resources needed to host the guidance. This includes Amazon Cognito User group and user, Amazon Bedrock Agents, AWS Lambda Functions, AWS Identity and Access Management (IAM) roles and AWS STS token.
2. The user navigates to the Secure Chat UI URL
3. Secure Chat  application is hosted on AWS Amplify
4. The web page is returned with HTML, CSS, JavaScript.  User is now able to input the configuration details for Amazon Cognito and Amazon Bedrock Agents
5. Upon configuration completion, the user is prompted to authenticate using Amazon Cognito with a username and password configured for them in the user pool
6. After successful authentication, Cognito Identity Pool will negotiate temporary credentials from AWS Simple Token Service (STS)
7. Cognito Identity Pool passes temporary AWS credentials to the Secure Chat UI
8. Once authenticated, the user now sees the Secure Chat UI chat prompt to interact with the Amazon Bedrock Agent that is configured
9. The FinOps Supervisor Agent evaluates each User's question and directs it to one of two specialized sub-agents: the Cost Analysis Agent or the Cost Optimization Agent
10. Each specialized agent (Cost Analysis or Cost Optimization) reviews its predefined set of actions to identify the correct procedure for answering the user's question
11. The action groups execute their respective AWS  Lambda functions to fetch data, whether that's accessing the AWS Cost Explorer API or pulling recommendations from Trusted Advisor's Cost Optimization pillar
12. The FinOps Supervisor Agent compiles all the gathered data into a final answer and sends it back to the Secure Chat UI visible to the User

In the following sections, we dive deeper into the architecture of our guidance, explore the capabilities of each agent, and discuss the potential impact of this approach on AWS cost management strategies.

### AWS services in this Guidance

| AWS service | Description |
|-------------|-------------|
| [Amazon Bedrock](https://aws.amazon.com/bedrock/) | Core. Provides foundation models and agent capabilities for natural language processing and multi-agent orchestration. |
| [Amazon Nova](https://aws.amazon.com/ai/generative-ai/nova/) | Core. AWS's next-generation foundation model that delivers breakthrough intelligence and industry-leading performance. |
| [AWS Lambda](https://aws.amazon.com/lambda/) | Core. Executes code for Amazon Bedrock action groups, enabling agents to interact with AWS services. |
| [Amazon Cognito](https://aws.amazon.com/cognito/) | Core. Provides user authentication and role-based access control for the application. |
| [AWS Amplify](https://aws.amazon.com/amplify/) | Core. Hosts the frontend application for user interaction with the FinOps agents. |
| [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) | Core. Provides cost data and analysis capabilities for the Cost Analysis Agent. |
| [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/) | Core. Delivers cost optimization recommendations for the Cost Optimization Agent. |
| [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) | Supporting. Manages permissions and access control for AWS services used in the guidance. |
| [AWS CloudFormation](http://aws.amazon.com/cloudformation) | Supporting. Deploys and configures the guidance resources in a consistent and repeatable manner. |

## Cost
This estimate assumes a relatively simple usage pattern and minimal data storage. The majority of the cost comes from Amazon Bedrock usage. Costs could increase if:

Requests to Bedrock involve larger token counts.
Lambda functions run for longer durations.
More data is stored or transferred through Amplify.
Advanced features of Trusted Advisor are used.

| AWS Service | Usage Estimate | Monthly Cost (USD) |
|-------------|----------------|---------------------|
| Amazon Bedrock (Nova) | 3,000 requests * 1,000 tokens/request | $30.00 |
| Amazon Cognito | 100 MAU | $0.00 (within free tier) |
| AWS Lambda | 3,000 invocations * 5 functions * 1s avg. duration | $0.00 (within free tier) |
| AWS Amplify | 1 GB storage, 5 GB data transfer | $0.23 |
| Amazon CloudWatch | Basic monitoring + 1 GB logs | $0.50 |
| AWS IAM | N/A | $0.00 |
| AWS Cost Explorer | 3,000 API requests | $0.00 (within free tier) |
| AWS Trusted Advisor | Basic checks | $0.00 |
| **Total Estimated Monthly Cost** | | **$30.73** |

## Security

When you build systems on AWS infrastructure, security responsibilities are shared between you and AWS. This [shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/) reduces your operational burden because AWS operates, manages, and controls the components including the host operating system, the virtualization layer, and the physical security of the facilities in which the services operate. For more information about AWS security, visit [AWS Cloud Security](http://aws.amazon.com/security/).

This Guidance implements the following security features:

- **Amazon Cognito user authentication** - Secure user authentication with user pools and identity pools
- **Role-based access control** - Ensures that only authorized users can access specific functionality
- **IAM roles and policies** - Provides least-privilege permissions for Lambda functions and other AWS services
- **Secure API communication** - All communication between components uses HTTPS encryption

### Supported AWS Regions

"Guidance for Cost Analysis and Optimization with Amazon Bedrock Agents" is supported in the following AWS Regions (as of July 2025):

| **Region Name**  | | 
|-----------|------------|
|US East (Ohio) | Asia Pacific (Seoul) |
|US East (N. Virginia) | Europe (Paris) |
|US West (Northern California) | Middle East (Bahrain) |
|US West (Oregon) | AWS GovCloud (US-West) |
|Africa (Cape Town)  | Asia Pacific (Seoul) |

## Deploy the Guidance

### Prerequisites

You must have the following in place to complete the guidance in this post:

* An [AWS account](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fportal.aws.amazon.com%2Fbilling%2Fsignup%2Fresume&client_id=signup)
* FM [access](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html) in Amazon Bedrock for Amazon Nova Pro and Micro in the same [AWS Region](https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html#region) where you will deploy this guidance
* The accompanying [AWS CloudFormation template](http://aws.amazon.com/cloudformation) downloaded from the [aws-samples GitHub repo](https://github.com/aws-samples/sample-finops-bedrock-multiagent-nova)

### Deployment Instructions

Please refer to [Implementation Guide](https://aws-solutions-library-samples.github.io/compute/cost-analysis-and-optimization-with-amazon-bedrock-agents.html) for detailed instructions for guidance deployment options.


## Considerations 

For optimal visibility across your organization, deploy this guidance in your AWS payer account to access cost details for your linked accounts through Cost Explorer.

Trusted Advisor cost optimization visibility is limited to the account where you deploy this guidance. To expand its scope, [enable](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html) Trusted Advisor at the AWS organization level and modify this guidance accordingly.

Before deploying to production, enhance security by implementing additional safeguards. You can do this by [associating guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-guardrail.html) with your agent in Amazon Bedrock.

## Conclusion

The integration of the multi-agent capability of Amazon Bedrock with Amazon Nova demonstrates the transformative potential of AI in AWS cost management. Our FinOps agent guidance showcases how specialized AI agents can work together to deliver comprehensive cost analysis, forecasting, and optimization recommendations in a secure and user-friendly environment. This implementation not only addresses immediate cost management challenges, but also adapts to evolving cloud financial operations. As AI technologies advance, this approach sets a foundation for more intelligent and proactive cloud management strategies across various business operations.

## Additional resources

To learn more about Amazon Bedrock, refer to the following resources:

* [Introducing multi-agent collaboration capability for Amazon Bedrock](https://aws.amazon.com/blogs/aws/introducing-multi-agent-collaboration-capability-for-amazon-bedrock/)
* [Unlocking complex problem-solving with multi-agent collaboration on Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/unlocking-complex-problem-solving-with-multi-agent-collaboration-on-amazon-bedrock/)
* [Introducing Amazon Nova foundation models: Frontier intelligence and industry leading price performance](https://aws.amazon.com/blogs/aws/introducing-amazon-nova-frontier-intelligence-and-industry-leading-price-performance/)



## Next Steps

To further enhance the guidance's capabilities, you can develop additional specialized agents that address more complex inquiries. These advanced agents can leverage the rich data available in AWS Cost and Usage Reports, enabling deeper analysis and comprehensive insights into your AWS spending patterns and resource utilization.

## FAQ, Known Issues, Additional Considerations, and Limitations

Additional Considerations
- Amazon Bedrock requests are charged per token

## Revisions

- **v1.0.0** – Initial release with Amazon Bedrock Agents (multi-agent) integration with AWS Cost Explorer API and AWS Trusted Advisor Cost Optimization Pillar API

## Notices

Customers are responsible for making their own independent assessment of the information in this Guidance.  

This Guidance:  
(a) is for informational purposes only,  
(b) represents AWS current product offerings and practices, which are subject to change without notice, and  
(c) does not create any commitments or assurances from AWS and its affiliates, suppliers, or licensors.  

AWS products or services are provided “as is” without warranties, representations, or conditions of any kind, whether express or implied.  
AWS responsibilities and liabilities to its customers are controlled by AWS agreements, and this Guidance is not part of, nor does it modify, any agreement between AWS and its customers.

## Authors
- Sergio Barraza, Sr TAM
- Salman Ahmed, Sr TAM
- Ravi Kumar, Sr TAM
- Ankush Goyal, ESL TAM
- Daniel Zilberman, Sr Solutions Architect, Techical Solutions 


## License

This library is licensed under the MIT-0 License. See the [LICENSE](./LICENSE) file.

