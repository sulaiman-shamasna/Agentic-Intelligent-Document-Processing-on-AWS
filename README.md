# Agentic-Intelligent-Document-Processing-on-AWS
---

This architecture bellow implements an intelligent document processing system using Amazon Bedrock AgentCore, where multiple specialized agents collaborate through graph-based
workflows to automatically identify, extract, validate, and learn from business documents with minimal human intervention.

![architecture](architecture/architecture.svg)

1. Business documents including purchase orders, invoices, shipping tickets, and other transactional records arrive via **Amazon Simple Email Service (Amazon SES)** or direct upload to **Amazon Simple Storage Service (Amazon S3)**.

2. **Amazon EventBridge** invokes an **AWS Lambda** function when new documents arrive in **Amazon S3**.

3. The Lambda function creates a tracking job in **Amazon DynamoDB** and invokes **Amazon Bedrock AgentCore Runtime** to orchestrate document processing through Strands Agents.

4. **Amazon Bedrock AgentCore Runtime** queries **Amazon Bedrock AgentCore Identity** for machine-to-machine credentials, enabling secure access to AWS resources and third-party services. **Amazon Bedrock AgentCore Identity** retrieves a workload access token from **Amazon Cognito**.

5. Using this token, **Amazon Bedrock AgentCore Runtime** discovers and accesses tools from **Amazon Bedrock AgentCore Gateway**. The agent selects tools based on document type and processing requirements.

6. **Amazon Bedrock AgentCore Gateway** transforms existing APIs and Lambda functions into agent-compatible tools with minimal code. It provides a searchable Model Context Protocol (MCP) interface for AWS resources, external tools, and databases, enabling secure discovery and communication.

7. Agents extract text from documents using **Amazon Textract**, classify entities using vector embeddings in **Amazon S3**, and determine next processing steps.

8. Agents access Enterprise Data via MCP servers to validate extracted content against business rules.

9. Users monitor status and perform administrative actions through a chat- enabled dashboard hosted on **Amazon Elastic Container Service (Amazon ECS)**.

10. The dashboard invokes **Amazon Bedrock AgentCore Runtime**, enabling users to troubleshoot document processing issues in real- time.

11. **Amazon Bedrock AgentCore Observability** traces, debugs, and monitors agent performance by automatically logging telemetry data to **Amazon CloudWatch**. This provides detailed visualizations of each workflow step, enabling inspection of execution paths and identification of performance bottlenecks.