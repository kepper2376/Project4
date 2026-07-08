# REPORT.docx Outline

Grace & Mercy Lutheran Ministries should deploy the 13B language model using INT4 quantization because it provides a strong balance between performance, hardware cost, and storage requirements. The organization's existing 48 GB GPU is capable of serving the model while leaving room for runtime memory, making it the most cost-effective option. The larger 70B model would require significantly more GPU memory and a much larger infrastructure investment. Storage should be divided across hot, warm, cold, and archive tiers so that high-performance assets remain readily available while infrequently accessed data is stored at lower cost. Sensitive ministry information, including counseling records and donor information, should remain on-premises and follow defined retention and deletion policies.

## Executive Summary

The AI estate consists of model weights, a vector database, source documents, fine-tuning datasets, application logs, temporary scratch storage, and backups. The 13B model stored with INT4 quantization requires approximately 6.5 GB of weight storage, while runtime overhead such as the KV cache and framework memory increases total GPU memory usage to roughly 20 GB. The 70B model requires approximately 35 GB of weights and nearly 45 GB of deployed GPU memory, making it impractical for the ministry's current hardware.

The vector database stores five million embeddings with 1,536 dimensions using half precision. This results in approximately 15.36 GB of raw vector storage, plus approximately 1.54 GB for the HNSW index, producing an estimated total of 16.9 GB. The RAG corpus is estimated at approximately 100,000 ministry documents averaging 500 KB each, resulting in about 50 GB of source data. A fine-tuning dataset containing approximately 250,000 question-and-answer pairs averaging 2 KB each requires roughly 500 MB.

Application logs are expected to generate approximately 500 MB per day, while temporary scratch space used during embedding generation, indexing, and fine-tuning is estimated at approximately 50 GB. Backup storage includes copies of critical datasets, model configurations, and vector indexes to ensure recovery from hardware failures while minimizing downtime.

## Estate Sizing Narrative

The pgvector database was initialized using the provided SQL script after starting PostgreSQL with the pgvector extension. After loading the sample embeddings and building the HNSW index, the database size query was executed to measure the storage used by the vector table and index. The measured size should closely match the calculated estimate of approximately 16 to 17 GB for a five-million-vector workload when using half precision. Minor differences are expected because PostgreSQL stores metadata, pages, and index structures in addition to the raw vector data. The measured SQL output or screenshot from the lab environment should be inserted here as evidence.

## pgvector Proof

The pgvector database was initialized using the provided SQL script after starting PostgreSQL with the pgvector extension. After loading the sample embeddings and building the HNSW index, the database size query was executed to measure the storage used by the vector table and index. The measured size should closely match the calculated estimate of approximately 16 to 17 GB for a five-million-vector workload when using half precision. Minor differences are expected because PostgreSQL stores metadata, pages, and index structures in addition to the raw vector data. The measured SQL output or screenshot from the lab environment should be inserted here as evidence.

## Governance Policy Summary

The governance policy classifies ministry assets according to four sensitivity levels: Public, Internal, Confidential, and Restricted. Public information includes published ministry materials and documentation. Internal information includes operational documentation and reusable model files. Confidential information includes embeddings, vector indexes, and fine-tuning datasets that contain ministry knowledge but not highly sensitive personal information. Restricted information includes donor records, counseling documents, financial records, authentication credentials, and any personally identifiable information.

Retention periods vary according to the value and sensitivity of each asset. Model weights may be retained indefinitely because they are reproducible. Runtime cache and temporary scratch space are deleted immediately after use. Application logs are retained for one year to support troubleshooting and auditing. Fine-tuning datasets and backups follow organizational retention schedules and are securely destroyed when no longer required.

## Hard-Tier Policy Memo

The most irreplaceable assets are the RAG source corpus, donor information, counseling records, financial documents, and fine-tuning datasets because recreating them would require significant time or would be impossible. These assets must remain on secure on-premises storage with regular encrypted backups.

Model weights, vector embeddings, HNSW indexes, and application software are reproducible because they can be regenerated or downloaded from trusted sources. Although valuable, these assets do not require the same long-term protection as ministry records.

Application logs become a liability if retained longer than necessary because they may contain user prompts, operational information, or other sensitive metadata. Temporary scratch files also become unnecessary after processing is complete and should be securely deleted. Maintaining clear retention schedules reduces storage costs, limits security risks, and ensures compliance with organizational governance policies.

