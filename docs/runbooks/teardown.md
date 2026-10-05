# Infrastructure Teardown Runbook

## Overview
This runbook details the safe and deterministic procedure for decommissioning the serverless RAG stack components (OpenSearch Serverless collection, Bedrock Knowledge Base, Lambda workers, and S3 ingestion buckets).

## Execution Steps
1. **Disable Event Sources:** Pause SQS ingestion queues and EventBridge triggers.
2. **Drain In-Flight Workloads:** Allow active chunking/embedding tasks to complete.
3. **Delete Vector Indices:** Destroy OpenSearch Serverless collections.
4. **Terraform Destroy:** Run `terraform destroy -target=module.rag_stack`.
