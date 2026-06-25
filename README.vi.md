# AWS-KnowledgeZone

Bộ sưu tập toàn diện các MCP (Model Context Protocol) Servers của AWS được cấu hình sẵn, sẵn sàng sử dụng với các AI coding assistants như Kilo, Cursor, VS Code, và Cline.

Repo này chứa file `.vscode/mcp.json` với cấu hình cho **50+ AWS MCP Servers**, bao gồm các server core (API, Documentation, Knowledge, IAM), infrastructure & deployment (IaC, Cloud Control API, EKS, ECS, Serverless), data & analytics (DynamoDB, DocumentDB, Neptune, Redshift, S3 Tables), AI & Machine Learning (Bedrock KB Retrieval, SageMaker AI, Bedrock AgentCore, Amazon Q), cost & operations (CloudWatch, Billing, Pricing, CloudTrail), integration & messaging (SNS/SQS, MQ, AppSync, Step Functions, Location), developer tools (OpenAPI, IAM, Kendra, Prometheus), và healthcare (HealthOmics, HealthLake).

---

## Mục lục

- [Giới thiệu về MCP](#gi%E1%BB%9Bi-thi%E1%BB%87u-v%E1%BB%81-mcp)
- [Hướng dẫn sử dụng](#h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-s%E1%BB%AD-d%E1%BB%A5ng)
- [Essential & Core (Cốt lõi)](#essential--core-c%E1%BB%91t-l%C3%B5i)
- [Documentation (Tài liệu)](#documentation-t%C3%A0i-li%E1%BB%87u)
- [Infrastructure & Deployment (Hạ tầng & Triển khai)](#infrastructure--deployment-h%E1%BA%A1-t%E1%BA%A7ng--tri%E1%BB%83n-khai)
- [AI & Machine Learning](#ai--machine-learning)
- [Data & Analytics (Dữ liệu & Phân tích)](#data--analytics-d%E1%BB%AF-li%E1%BB%87u--ph%C3%A2n-t%C3%ADch)
- [Developer Tools & Support (Công cụ phát triển)](#developer-tools--support-c%C3%B4ng-c%E1%BB%A5-ph%C3%A1t-tri%E1%BB%83n)
- [Integration & Messaging (Tích hợp & Nhắn tin)](#integration--messaging-t%C3%ADch-h%E1%BB%A3p--nh%E1%BA%AFn-tin)
- [Cost & Operations (Chi phí & Vận hành)](#cost--operations-chi-ph%C3%AD--v%E1%BA%ADn-h%C3%A0nh)
- [Healthcare & Lifesciences (Y tế & Khoa học sức khỏe)](#healthcare--lifesciences-y-t%E1%BA%BF--khoa-h%E1%BB%8Dc-s%E1%BB%A9c-kh%E1%BB%8Fe)

---

## Giới thiệu về MCP

**Model Context Protocol (MCP)** là một giao thức mã nguồn mở do Anthropic phát triển, cho phép tích hợp liền mạch giữa các ứng dụng AI (LLM) với các nguồn dữ liệu và công cụ bên ngoài. Một MCP Server là một chương trình nhẹ chạy cục bộ hoặc từ xa, cung cấp các khả năng cụ thể thông qua giao thức MCP chuẩn hóa.

**AWS MCP Servers** mở rộng khả năng của AI coding assistants để tương tác trực tiếp với AWS, bao gồm:
- Truy xuất tài liệu AWS mới nhất
- Quản lý tài nguyên AWS (EC2, S3, Lambda,...)
- Triển khai infrastructure as code
- Phân tích logs, metrics, chi phí
- Và nhiều hơn nữa

---

## Hướng dẫn sử dụng

Repo này sử dụng **uvx** (từ [Astral uv](https://docs.astral.sh/uv/)) để chạy các MCP servers. Cài đặt uv:

```bash
# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Sao chép file `.vscode/mcp.json` vào cấu hình MCP của IDE bạn:
- **VS Code**: Dùng trực tiếp file `.vscode/mcp.json` trong project
- **Kilo (global)**: `~/.kilo/settings/mcp.json`
- **Kilo (project)**: `.kilo/settings/mcp.json`
- **Cursor**: `.cursor/mcp.json`
- **Cline**: `cline_mcp_settings.json`

> **Lưu ý**: Hầu hết các server yêu cầu cấu hình AWS credentials (`AWS_PROFILE`, `AWS_REGION`). Cập nhật các giá trị `your-aws-profile`, `your-aws-region`,... tương ứng trong file cấu hình trước khi sử dụng.

---

## Essential & Core (Cốt lõi)

### 1. AWS API MCP Server

Cho phép AI assistants tương tác với AWS services thông qua AWS CLI commands. Hỗ trợ tất cả dịch vụ AWS mới nhất.

| Tính năng | Mô tả |
|-----------|-------|
| **Loại** | Core / stdio |
| **Cấu hình** | `"command": "uvx", "args": ["awslabs.aws-api-mcp-server@latest"]` |
| **Env chính** | `AWS_REGION: "us-east-1"` |
| **MCP Tools** | `call_aws`, `suggest_aws_commands`, `get_execution_plan` |
| **Công dụng** | Liệt kê EC2 instances, tạo S3 buckets, quản lý tài nguyên AWS |
| **Bảo mật** | Hỗ trợ read-only mode (`READ_OPERATIONS_ONLY`), IAM-based |

### 2. AWS Knowledge MCP Server

Server từ xa được AWS quản lý, cung cấp tài liệu AWS, code samples, agent skills, và thông tin regional availability.

| Tính năng | Mô tả |
|-----------|-------|
| **Loại** | Remote / HTTP |
| **URL** | `https://knowledge-mcp.global.api.aws` |
| **MCP Tools** | `search_documentation`, `read_documentation`, `recommend`, `list_regions`, `get_regional_availability`, `retrieve_skill` |
| **Công dụng** | Tra cứu tài liệu AWS real-time, API references, blog posts, Well-Architected guidance, CDK/CloudFormation docs, Strands Agents SDK docs |

---

## Documentation (Tài liệu)

### 3. AWS Documentation MCP Server

Truy xuất tài liệu AWS documentation real-time, tìm kiếm và đọc nội dung.

| Tính năng | Mô tả |
|-----------|-------|
| **Loại** | Documentation / stdio |
| **Cấu hình** | `"command": "uvx", "args": ["awslabs.aws-documentation-mcp-server@latest"]` |
| **Env chính** | `FASTMCP_LOG_LEVEL: "ERROR"`, `AWS_DOCUMENTATION_PARTITION: "aws"` |
| **MCP Tools** | `read_documentation`, `search_documentation`, `read_sections`, `recommend` |
| **Workflows** | Vibe Coding & Development, Conversational Assistants |

---

## Infrastructure & Deployment (Hạ tầng & Triển khai)

### 4. Infrastructure as Code MCP Server

Tạo và troubleshooting CloudFormation/CDK templates.

| Tính năng | Mô tả |
|-----------|-------|
| **Loại** | Infrastructure / stdio |
| **Cấu hình** | `"command": "uvx", "args": ["awslabs.aws-iac-mcp-server@latest"]` |
| **Env chính** | `AWS_PROFILE`, `FASTMCP_LOG_LEVEL` |
| **MCP Tools** | `validate_cloudformation_template`, `check_cloudformation_template_compliance`, `troubleshoot_cloudformation_deployment`, `search_cdk_documentation`, `search_cloudformation_documentation`, `cdk_best_practices` |

### 5. AWS Cloud Control API MCP Server

> ⚠️ **DEPRECATED**: Server này không còn được cập nhật. Migrate lên [AWS IaC MCP Server](#4-infrastructure-as-code-mcp-server).

Quản lý 1,100+ AWS resources qua Cloud Control API với bảo mật nhiều lớp (Checkov scanning, token workflow).

| MCP Tools | `check_environment_variables`, `get_aws_session_info`, `generate_infrastructure_code`, `explain`, `run_checkov`, CRUDL resources |
|-----------|-------------------------------------------------------------------------------------------------------------------------------|
| **Công dụng** | Tạo/Sửa/Xóa tài nguyên AWS bằng ngôn ngữ tự nhiên |
| **Bảo mật** | Checkov scanning, double confirmation cho deletion, policy restrictions |

### 6. Amazon EKS MCP Server

Quản lý EKS clusters và Kubernetes resources.

| Tính năng | Mô tả |
|-----------|-------|
| **Loại** | Infrastructure / stdio |
| **Args** | `--allow-write --allow-sensitive-data-access` |
| **MCP Tools** | `manage_eks_stacks`, `manage_k8s_resource`, `apply_yaml`, `list_k8s_resources`, `get_pod_logs`, `get_k8s_events`, `get_cloudwatch_logs/metrics`, `search_eks_troubleshoot_guide` |
| **Công dụng** | Tạo cluster, deploy ứng dụng, troubleshoot EKS issues |
| **Auth modes** | IAM (default) và kubeconfig/OIDC |

### 7. Amazon ECS MCP Server

Containerize và deploy ứng dụng lên Amazon ECS.

| Tính năng | Mô tả |
|-----------|-------|
| **Loại** | Infrastructure / stdio |
| **Args** | `"--from", "awslabs-ecs-mcp-server", "ecs-mcp-server"` |
| **MCP Tools** | `containerize_app`, `build_and_push_image_to_ecr`, `ecs_resource_management`, `ecs_troubleshooting_tool`, `wait_for_service_ready`, `delete_app` |
| **Công dụng** | Deploy Express Mode, build/push Docker images, ECS resource management |

### 8. Finch MCP Server

Build và push container images qua Finch CLI với ECR integration.

| MCP Tools | `finch_build_container_image`, `finch_push_image`, `finch_create_ecr_repo` |
|-----------|--------------------------------------------------------------------------|
| **Env** | `AWS_PROFILE: "default"`, `AWS_REGION: "us-west-2"` |

### 9. AWS Lambda Tool MCP Server

Bridge để expose Lambda functions như MCP tools.

| MCP Tools | Invoke Lambda functions được filter bởi prefix/tag |
|-----------|---------------------------------------------------|
| **Env** | `FUNCTION_PREFIX`, `FUNCTION_LIST`, `FUNCTION_TAG_KEY/VALUE`, `FUNCTION_INPUT_SCHEMA_ARN_TAG_KEY` |
| **Công dụng** | Truy cập private resources (databases, internal APIs) qua Lambda |

### 10. AWS Step Functions Tool MCP Server

Bridge để expose Step Functions state machines như MCP tools.

| MCP Tools | Invoke Standard/Express workflows |
|-----------|-----------------------------------|
| **Env** | `STATE_MACHINE_PREFIX`, `STATE_MACHINE_LIST`, `STATE_MACHINE_TAG_KEY/VALUE` |
| **Schema** | EventBridge Schema Registry integration |

### 11. AWS Serverless MCP Server

Serverless application lifecycle management với SAM CLI.

| Tính năng | Mô tả |
|-----------|-------|
| **Args** | `--allow-write --allow-sensitive-data-access` |
| **MCP Tools** | `sam_init`, `sam_build`, `sam_deploy`, `sam_logs`, `sam_local_invoke`, `deploy_webapp`, `configure_domain`, `get_lambda_guidance`, `get_iac_guidance`, ESM tools |
| **Công dụng** | Tạo SAM projects, deploy web apps (Lambda Web Adapter), troubleshoot ESM |

### 12. AWS Support MCP Server

Tạo và quản lý AWS Support cases.

| MCP Tools | `create_support_case`, `describe_support_cases`, `describe_communications`, `add_communication_to_case`, `resolve_support_case`, `describe_services/severity_levels` |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Yêu cầu** | Business/Enterprise Support plan |

---

## AI & Machine Learning

### 13. Amazon Bedrock Knowledge Bases Retrieval MCP Server

Query knowledge bases với citation support và reranking.

| MCP Tools | `query_knowledge_base`, list/describe KBs, filter by data source |
|-----------|-----------------------------------------------------------------|
| **Env** | `KB_INCLUSION_TAG_KEY`, `BEDROCK_KB_RERANKING_ENABLED` |

### 14. Amazon Q Index MCP Server

Search qua enterprise Q index.

| Env | `QINDEX_ID`, `AWS_REGION: "us-east-1"` |

### 15. Amazon Q Business Anonymous MCP Server

AI assistant dựa trên knowledgebase với anonymous access.

| Env | `QBUSINESS_APP_ID`, `QBUSINESS_USER_ID` |

### 16. Bedrock AgentCore MCP Server

**122 tools** cho AgentCore platform: Runtime, Memory, Identity, Gateway, Policy, Browser (cloud-based), Code Interpreter.

| MCP Tools | `search_agentcore_docs`, `fetch_agentcore_doc`, runtime/memory/identity/gateway/policy management tools |
|-----------|--------------------------------------------------------------------------------------------------------|

### 17. SageMaker AI MCP Server

Quản lý SageMaker HyperPod clusters (EKS/Slurm orchestration).

| Args | `--allow-write --allow-sensitive-data-access` |
|------|----------------------------------------------|

### 18. Amazon Kendra Index MCP Server

Enterprise search và RAG enhancement.

| Env | `KEND_INDEX_ID`, `KEND_ROLE_ARN` |

---

## Data & Analytics (Dữ liệu & Phân tích)

### 19. Amazon DynamoDB MCP Server

Data modeling, validation, code generation, cost analysis cho DynamoDB.

| MCP Tools | `dynamodb_data_modeling`, `dynamodb_data_model_validation`, `source_db_analyzer`, `dynamodb_data_model_schema_converter`, `generate_data_access_layer`, `compute_performances_and_costs` |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Công dụng** | Thiết kế single-table design, migrate từ SQL, generate type-safe Python code |

### 20. Amazon DocumentDB MCP Server

MongoDB-compatible document database operations.

### 21. Amazon Neptune MCP Server

Graph database queries với openCypher và Gremlin.

### 22. Amazon Keyspaces MCP Server

Apache Cassandra-compatible operations.

### 23. Amazon ElastiCache MCP Server

Complete ElastiCache operations (Redis, Memcached).

### 24. Amazon ElastiCache / MemoryDB for Valkey MCP Server

Advanced data structures và caching với Valkey.

| Env | `VALKEY_HOST: "127.0.0.1"`, `VALKEY_PORT: "6379"` |

### 25. Amazon ElastiCache for Memcached MCP Server

High-speed caching operations.

### 26. Amazon Timestream for InfluxDB MCP Server

InfluxDB-compatible time-series operations.

### 27. AWS Data Processing MCP Server

Data processing tools cho AWS Glue, Amazon EMR, Amazon Athena.

### 28. Amazon Redshift MCP Server

Query và explore Redshift clusters & serverless workgroups.

### 29. AWS S3 Tables MCP Server

Manage, query, và ingest S3-based tables với SQL support.

### 30. AWS AppSync MCP Server

Backend API management và operations execution.

### 31. AWS IoT SiteWise MCP Server

Industrial IoT asset management, data ingestion, monitoring.

### 32. Aurora DSQL MCP Server

Distributed SQL với PostgreSQL compatibility.

### 33. PostgreSQL MCP Server (Aurora PostgreSQL)

PostgreSQL database operations via RDS Data API.

### 34. SageMaker Unified Studio - Spark Troubleshooting

Apache Spark troubleshooting cho Glue và EMR deployment models.

### 35. SageMaker Unified Studio - Code Recommendation

Apache Spark code recommendation tool.

### 36. SageMaker Unified Studio - Spark Upgrade

Apache Spark upgrade tools cho cluster migration.

---

## Developer Tools & Support (Công cụ phát triển)

### 37. OpenAPI MCP Server

Dynamic tool generation từ OpenAPI specifications. Hỗ trợ multi-spec composition, authentication (Basic, Bearer, API Key, Cognito), tag filtering.

| MCP Tools | Dynamic tools từ OpenAPI endpoints |
|-----------|-----------------------------------|
| **Công dụng** | Tích hợp bất kỳ REST API nào vào AI assistant |

### 38. Prometheus MCP Server

Prometheus-compatible operations cho Amazon Managed Service for Prometheus.

### 39. AWS IAM MCP Server

Comprehensive IAM management: users, roles, groups, policies, access keys, policy simulation.

| MCP Tools | `list_users`, `get_user`, `create_user`, `delete_user`, `list_roles`, `create_role`, `list_groups`, `create_group`, `add_user_to_group`, policy management, `simulate_principal_policy` |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Bảo mật** | Read-only mode (`--readonly`), force delete với cleanup |

### 40. AWS Location MCP Server

Place search, geocoding, route optimization.

---

## Integration & Messaging (Tích hợp & Nhắn tin)

### 41. Amazon MQ MCP Server

Message broker management cho RabbitMQ và ActiveMQ.

### 42. Amazon SNS/SQS MCP Server

Event-driven messaging và queue management.

---

## Cost & Operations (Chi phí & Vận hành)

### 43. CloudWatch MCP Server

AI-powered observability: metrics, logs, alarms, PromQL queries, Logs Insights.

| MCP Tools | `get_metric_data`, `get_metric_metadata`, `get_active_alarms`, `get_alarm_history`, `analyze_log_group`, `execute_log_insights_query`, `execute_cwl_insights_batch`, `describe_log_groups`, PromQL tools |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Công dụng** | Root cause analysis, alarm troubleshooting, log analysis, cross-account/cross-region queries |

### 44. CloudWatch Application Signals MCP Server

Application monitoring và performance insights.

### 45. AWS Well-Architected Security Assessment Tool MCP Server

Assess AWS environments theo Well-Architected Framework Security Pillar.

### 46. CloudTrail MCP Server

AWS API activity, user/resource analysis qua CloudTrail logs.

### 47. AWS Billing and Cost Management MCP Server

Cost analysis, optimization, Savings Plans, Reserved Instances, S3 Storage Lens, Billing Conductor.

| MCP Tools | Cost Explorer, budgets, free tier, Compute Optimizer recommendations, Pricing Calculator, Storage Lens |
|-----------|-------------------------------------------------------------------------------------------------------|
| **Env** | `STORAGE_LENS_MANIFEST_LOCATION` (optional cho Storage Lens) |

### 48. AWS Pricing MCP Server

Pre-deployment cost estimation và optimization.

---

## Healthcare & Lifesciences (Y tế & Khoa học sức khỏe)

### 49. AWS HealthOmics MCP Server

Generate, run, debug, optimize lifescience workflows.

### 50. AWS HealthLake MCP Server

FHIR interactions và quản lý HealthLake datastores.

---

## Tài liệu tham khảo

- [AWS MCP Servers Documentation](https://awslabs.github.io/mcp/)
- [AWS API MCP Server (GitHub)](https://github.com/awslabs/mcp)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/introduction)
- [AWS Blog: Introducing AWS MCP Servers](https://aws.amazon.com/blogs/machine-learning/introducing-aws-mcp-servers-for-code-assistants-part-1/)

---

## License

Cấu hình trong repo này được cung cấp dưới dạng tham khảo. Các MCP Servers thuộc sở hữu của Amazon Web Services và được cấp phép theo Apache License 2.0.
