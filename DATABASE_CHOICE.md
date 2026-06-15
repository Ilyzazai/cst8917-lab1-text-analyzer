# Database Choice

## Selected Database

For this lab, I selected **Azure Cosmos DB for NoSQL**.

## Justification

Azure Cosmos DB for NoSQL is a good choice for this Text Analyzer function because the analysis result is stored as a JSON document. The function does not need a fixed relational schema, so a NoSQL database is simpler than Azure SQL Database for this use case. Cosmos DB also fits well with serverless applications because it can scale and work with cloud-native applications. It has a Python SDK, so it can be integrated with the Azure Function easily.

## Alternatives Considered

### Azure Table Storage

Azure Table Storage is simple and low cost, but it is better for key-value or table-style data. My analysis result contains nested JSON fields such as analysis details and metadata, so Cosmos DB is a better fit.

### Azure SQL Database

Azure SQL Database is useful when the application needs relational tables, joins, and structured SQL queries. For this lab, the data is JSON and simple history records, so SQL would add unnecessary complexity.

### Azure Blob Storage

Azure Blob Storage can store JSON files, but it is not ideal as a database. It is better for storing files such as images, logs, documents, or backups. Querying history records would be harder compared to Cosmos DB.

## Cost Considerations

Cosmos DB is suitable for a student lab because it supports development and small workloads. Azure Cosmos DB has free-tier options, and Microsoft documentation says the free tier can include 1000 RU/s and 25 GB of storage when enabled on an account. For this lab, the workload is very small because only simple text analysis records are stored.