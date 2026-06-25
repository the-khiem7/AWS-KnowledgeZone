# AWS-KnowledgeZone

A comprehensive collection of pre-configured AWS MCP (Model Context Protocol) Servers, ready to use with AI coding assistants such as Kilo, Cursor, VS Code, and Cline.

This repo contains the `.vscode/mcp.json` configuration file for **50+ AWS MCP Servers**, including core servers (API, Documentation, Knowledge, IAM), infrastructure & deployment (IaC, Cloud Control API, EKS, ECS, Serverless), data & analytics (DynamoDB, DocumentDB, Neptune, Redshift, S3 Tables), AI & Machine Learning (Bedrock KB Retrieval, SageMaker AI, Bedrock AgentCore, Amazon Q), cost & operations (CloudWatch, Billing, Pricing, CloudTrail), integration & messaging (SNS/SQS, MQ, AppSync, Step Functions, Location), developer tools (OpenAPI, IAM, Kendra, Prometheus), and healthcare (HealthOmics, HealthLake).

---

## Table of Contents

- [Introduction to MCP](#introduction-to-mcp)
- [Usage Guide](#usage-guide)
- [Essential & Core](#essential--core)
- [Documentation](#documentation)
- [Infrastructure & Deployment](#infrastructure--deployment)
- [AI & Machine Learning](#ai--machine-learning)
- [Data & Analytics](#data--analytics)
- [Developer Tools & Support](#developer-tools--support)
- [Integration & Messaging](#integration--messaging)
- [Cost & Operations](#cost--operations)
- [Healthcare & Lifesciences](#healthcare--lifesciences)

---

## Introduction to MCP

**Model Context Protocol (MCP)** is an open-source protocol developed by Anthropic that enables seamless integration between AI applications (LLMs) and external data sources and tools. An MCP Server is a lightweight program running locally or remotely that provides specific capabilities through the standardized MCP protocol.

**AWS MCP Servers** extend the capabilities of AI coding assistants to interact directly with AWS, including:
- Retrieving the latest AWS documentation
- Managing AWS resources (EC2, S3, Lambda, etc.)
- Deploying infrastructure as code
- Analyzing logs, metrics, and costs
- And much more

---

## Usage Guide

This repo uses **uvx** (from [Astral uv](https://docs.astral.sh/uv/)) to run MCP servers. Install uv:

```bash
# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Copy the `.vscode/mcp.json` file into your IDE's MCP configuration:
- **VS Code**: Use `.vscode/mcp.json` directly within the project
- **Kilo (global)**: `~/.kilo/settings/mcp.json`
- **Kilo (project)**: `.kilo/settings/mcp.json`
- **Cursor**: `.cursor/mcp.json`
- **Cline**: `cline_mcp_settings.json`

> **Note**: Most servers require AWS credentials configuration (`AWS_PROFILE`, `AWS_REGION`). Update the placeholder values such as `your-aws-profile`, `your-aws-region`, etc., in the configuration file before use.

---

## Essential & Core

### 1. AWS API MCP Server

Enables AI assistants to interact with AWS services via AWS CLI commands. Supports all latest AWS services.

| Feature | Description |
|---------|-------------|
| **Type** | Core / stdio |
| **Config** | `"command": "uvx", "args": ["awslabs.aws-api-mcp-server@latest"]` |
| **Env** | `AWS_REGION: "us-east-1"` |
| **MCP Tools** | `call_aws`, `suggest_aws_commands`, `get_execution_plan` |
| **Purpose** | List EC2 instances, create S3 buckets, manage AWS resources |
| **Security** | Read-only mode (`READ_OPERATIONS_ONLY`), IAM-based |

### 2. AWS Knowledge MCP Server

A remotely managed server providing AWS documentation, code samples, agent skills, and regional availability information.

| Feature | Description |
|---------|-------------|
| **Type** | Remote / HTTP |
| **URL** | `https://knowledge-mcp.global.api.aws` |
| **MCP Tools** | `search_documentation`, `read_documentation`, `recommend`, `list_regions`, `get_regional_availability`, `retrieve_skill` |
| **Purpose** | Real-time AWS documentation lookup, API references, blog posts, Well-Architected guidance, CDK/CloudFormation docs, Strands Agents SDK docs |

---

## Documentation

### 3. AWS Documentation MCP Server

Real-time AWS documentation retrieval, search, and content reading.

| Feature | Description |
|---------|-------------|
| **Type** | Documentation / stdio |
| **Config** | `"command": "uvx", "args": ["awslabs.aws-documentation-mcp-server@latest"]` |
| **Env** | `FASTMCP_LOG_LEVEL: "ERROR"`, `AWS_DOCUMENTATION_PARTITION: "aws"` |
| **MCP Tools** | `read_documentation`, `search_documentation`, `read_sections`, `recommend` |
| **Workflows** | Vibe Coding & Development, Conversational Assistants |

---

## Infrastructure & Deployment

### 4. Infrastructure as Code MCP Server

Create and troubleshoot CloudFormation/CDK templates.

| Feature | Description |
|---------|-------------|
| **Type** | Infrastructure / stdio |
| **Config** | `"command": "uvx", "args": ["awslabs.aws-iac-mcp-server@latest"]` |
| **Env** | `AWS_PROFILE`, `FASTMCP_LOG_LEVEL` |
| **MCP Tools** | `validate_cloudformation_template`, `check_cloudformation_template_compliance`, `troubleshoot_cloudformation_deployment`, `search_cdk_documentation`, `search_cloudformation_documentation`, `cdk_best_practices` |

### 5. AWS Cloud Control API MCP Server

> ⚠️ **DEPRECATED**: This server is no longer maintained. Migrate to [AWS IaC MCP Server](#4-infrastructure-as-code-mcp-server).

Manage 1,100+ AWS resources via Cloud Control API with multi-layer security (Checkov scanning, token workflow).

| MCP Tools | `check_environment_variables`, `get_aws_session_info`, `generate_infrastructure_code`, `explain`, `run_checkov`, CRUDL resources |
|-----------|---------------------------------------------------------------------------------------------------------------------------------|
| **Purpose** | Create/Update/Delete AWS resources using natural language |
| **Security** | Checkov scanning, double confirmation for deletion, policy restrictions |

### 6. Amazon EKS MCP Server

Manage EKS clusters and Kubernetes resources.

| Feature | Description |
|---------|-------------|
| **Type** | Infrastructure / stdio |
| **Args** | `--allow-write --allow-sensitive-data-access` |
| **MCP Tools** | `manage_eks_stacks`, `manage_k8s_resource`, `apply_yaml`, `list_k8s_resources`, `get_pod_logs`, `get_k8s_events`, `get_cloudwatch_logs/metrics`, `search_eks_troubleshoot_guide` |
| **Purpose** | Create clusters, deploy applications, troubleshoot EKS issues |
| **Auth modes** | IAM (default) and kubeconfig/OIDC |

### 7. Amazon ECS MCP Server

Containerize and deploy applications to Amazon ECS.

| Feature | Description |
|---------|-------------|
| **Type** | Infrastructure / stdio |
| **Args** | `"--from", "awslabs-ecs-mcp-server", "ecs-mcp-server"` |
| **MCP Tools** | `containerize_app`, `build_and_push_image_to_ecr`, `ecs_resource_management`, `ecs_troubleshooting_tool`, `wait_for_service_ready`, `delete_app` |
| **Purpose** | Express Mode deployment, build/push Docker images, ECS resource management |

### 8. Finch MCP Server

Build and push container images via Finch CLI with ECR integration.

| MCP Tools | `finch_build_container_image`, `finch_push_image`, `finch_create_ecr_repo` |
|-----------|----------------------------------------------------------------------------|
| **Env** | `AWS_PROFILE: "default"`, `AWS_REGION: "us-west-2"` |

### 9. AWS Lambda Tool MCP Server

Bridge to expose Lambda functions as MCP tools.

| MCP Tools | Invoke Lambda functions filtered by prefix/tag |
|-----------|------------------------------------------------|
| **Env** | `FUNCTION_PREFIX`, `FUNCTION_LIST`, `FUNCTION_TAG_KEY/VALUE`, `FUNCTION_INPUT_SCHEMA_ARN_TAG_KEY` |
| **Purpose** | Access private resources (databases, internal APIs) via Lambda |

### 10. AWS Step Functions Tool MCP Server

Bridge to expose Step Functions state machines as MCP tools.

| MCP Tools | Invoke Standard/Express workflows |
|-----------|-----------------------------------|
| **Env** | `STATE_MACHINE_PREFIX`, `STATE_MACHINE_LIST`, `STATE_MACHINE_TAG_KEY/VALUE` |
| **Schema** | EventBridge Schema Registry integration |

### 11. AWS Serverless MCP Server

Serverless application lifecycle management with SAM CLI.

| Feature | Description |
|---------|-------------|
| **Args** | `--allow-write --allow-sensitive-data-access` |
| **MCP Tools** | `sam_init`, `sam_build`, `sam_deploy`, `sam_logs`, `sam_local_invoke`, `deploy_webapp`, `configure_domain`, `get_lambda_guidance`, `get_iac_guidance`, ESM tools |
| **Purpose** | Create SAM projects, deploy web apps (Lambda Web Adapter), troubleshoot ESM |

### 12. AWS Support MCP Server

Create and manage AWS Support cases.

| MCP Tools | `create_support_case`, `describe_support_cases`, `describe_communications`, `add_communication_to_case`, `resolve_support_case`, `describe_services/severity_levels` |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Requirement** | Business/Enterprise Support plan |

---

## AI & Machine Learning

### 13. Amazon Bedrock Knowledge Bases Retrieval MCP Server

Query knowledge bases with citation support and reranking.

| MCP Tools | `query_knowledge_base`, list/describe KBs, filter by data source |
|-----------|-----------------------------------------------------------------|
| **Env** | `KB_INCLUSION_TAG_KEY`, `BEDROCK_KB_RERANKING_ENABLED` |

### 14. Amazon Q Index MCP Server

Search enterprise Q index.

| Env | `QINDEX_ID`, `AWS_REGION: "us-east-1"` |

### 15. Amazon Q Business Anonymous MCP Server

AI assistant based on knowledgebase with anonymous access.

| Env | `QBUSINESS_APP_ID`, `QBUSINESS_USER_ID` |

### 16. Bedrock AgentCore MCP Server

**122 tools** for the AgentCore platform: Runtime, Memory, Identity, Gateway, Policy, Browser (cloud-based), Code Interpreter.

| MCP Tools | `search_agentcore_docs`, `fetch_agentcore_doc`, runtime/memory/identity/gateway/policy management tools |
|-----------|----------------------------------------------------------------------------------------------------------|

### 17. SageMaker AI MCP Server

Manage SageMaker HyperPod clusters (EKS/Slurm orchestration).

| Args | `--allow-write --allow-sensitive-data-access` |
|------|----------------------------------------------|

### 18. Amazon Kendra Index MCP Server

Enterprise search and RAG enhancement.

| Env | `KEND_INDEX_ID`, `KEND_ROLE_ARN` |

---

## Data & Analytics

### 19. Amazon DynamoDB MCP Server

Data modeling, validation, code generation, cost analysis for DynamoDB.

| MCP Tools | `dynamodb_data_modeling`, `dynamodb_data_model_validation`, `source_db_analyzer`, `dynamodb_data_model_schema_converter`, `generate_data_access_layer`, `compute_performances_and_costs` |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Purpose** | Design single-table patterns, migrate from SQL, generate type-safe Python code |

### 20. Amazon DocumentDB MCP Server

MongoDB-compatible document database operations.

### 21. Amazon Neptune MCP Server

Graph database queries with openCypher and Gremlin.

### 22. Amazon Keyspaces MCP Server

Apache Cassandra-compatible operations.

### 23. Amazon ElastiCache MCP Server

Complete ElastiCache operations (Redis, Memcached).

### 24. Amazon ElastiCache / MemoryDB for Valkey MCP Server

Advanced data structures and caching with Valkey.

| Env | `VALKEY_HOST: "127.0.0.1"`, `VALKEY_PORT: "6379"` |

### 25. Amazon ElastiCache for Memcached MCP Server

High-speed caching operations.

### 26. Amazon Timestream for InfluxDB MCP Server

InfluxDB-compatible time-series operations.

### 27. AWS Data Processing MCP Server

Data processing tools for AWS Glue, Amazon EMR, Amazon Athena.

### 28. Amazon Redshift MCP Server

Query and explore Redshift clusters & serverless workgroups.

### 29. AWS S3 Tables MCP Server

Manage, query, and ingest S3-based tables with SQL support.

### 30. AWS AppSync MCP Server

Backend API management and operations execution.

### 31. AWS IoT SiteWise MCP Server

Industrial IoT asset management, data ingestion, monitoring.

### 32. Aurora DSQL MCP Server

Distributed SQL with PostgreSQL compatibility.

### 33. PostgreSQL MCP Server (Aurora PostgreSQL)

PostgreSQL database operations via RDS Data API.

### 34. SageMaker Unified Studio - Spark Troubleshooting

Apache Spark troubleshooting for Glue and EMR deployment models.

### 35. SageMaker Unified Studio - Code Recommendation

Apache Spark code recommendation tool.

### 36. SageMaker Unified Studio - Spark Upgrade

Apache Spark upgrade tools for cluster migration.

---

## Developer Tools & Support

### 37. OpenAPI MCP Server

Dynamic tool generation from OpenAPI specifications. Supports multi-spec composition, authentication (Basic, Bearer, API Key, Cognito), tag filtering.

| MCP Tools | Dynamic tools from OpenAPI endpoints |
|-----------|--------------------------------------|
| **Purpose** | Integrate any REST API into the AI assistant |

### 38. Prometheus MCP Server

Prometheus-compatible operations for Amazon Managed Service for Prometheus.

### 39. AWS IAM MCP Server

Comprehensive IAM management: users, roles, groups, policies, access keys, policy simulation.

| MCP Tools | `list_users`, `get_user`, `create_user`, `delete_user`, `list_roles`, `create_role`, `list_groups`, `create_group`, `add_user_to_group`, policy management, `simulate_principal_policy` |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security** | Read-only mode (`--readonly`), force delete with cleanup |

### 40. AWS Location MCP Server

Place search, geocoding, route optimization.

---

## Integration & Messaging

### 41. Amazon MQ MCP Server

Message broker management for RabbitMQ and ActiveMQ.

### 42. Amazon SNS/SQS MCP Server

Event-driven messaging and queue management.

---

## Cost & Operations

### 43. CloudWatch MCP Server

AI-powered observability: metrics, logs, alarms, PromQL queries, Logs Insights.

| MCP Tools | `get_metric_data`, `get_metric_metadata`, `get_active_alarms`, `get_alarm_history`, `analyze_log_group`, `execute_log_insights_query`, `execute_cwl_insights_batch`, `describe_log_groups`, PromQL tools |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Purpose** | Root cause analysis, alarm troubleshooting, log analysis, cross-account/cross-region queries |

### 44. CloudWatch Application Signals MCP Server

Application monitoring and performance insights.

### 45. AWS Well-Architected Security Assessment Tool MCP Server

Assess AWS environments according to the Well-Architected Framework Security Pillar.

### 46. CloudTrail MCP Server

AWS API activity, user/resource analysis via CloudTrail logs.

### 47. AWS Billing and Cost Management MCP Server

Cost analysis, optimization, Savings Plans, Reserved Instances, S3 Storage Lens, Billing Conductor.

| MCP Tools | Cost Explorer, budgets, free tier, Compute Optimizer recommendations, Pricing Calculator, Storage Lens |
|-----------|-------------------------------------------------------------------------------------------------------|
| **Env** | `STORAGE_LENS_MANIFEST_LOCATION` (optional for Storage Lens) |

### 48. AWS Pricing MCP Server

Pre-deployment cost estimation and optimization.

---

## Healthcare & Lifesciences

### 49. AWS HealthOmics MCP Server

Generate, run, debug, optimize lifescience workflows.

### 50. AWS HealthLake MCP Server

FHIR interactions and management of HealthLake datastores.

---

## References

- [AWS MCP Servers Documentation](https://awslabs.github.io/mcp/)
- [AWS API MCP Server (GitHub)](https://github.com/awslabs/mcp)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/introduction)
- [AWS Blog: Introducing AWS MCP Servers](https://aws.amazon.com/blogs/machine-learning/introducing-aws-mcp-servers-for-code-assistants-part-1/)

---

## License

The configuration in this repo is provided as a reference. The MCP Servers are owned by Amazon Web Services and are licensed under the Apache License 2.0.
