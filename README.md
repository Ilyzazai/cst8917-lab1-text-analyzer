# CST8917 Lab 1 - Azure Function Text Analyzer with Cosmos DB

## Student Information

| Item | Details |
|---|---|
| Course | CST8917 - Serverless Applications |
| Lab | Lab 1 - Create and Deploy Your First Azure Function |
| Student | Ilyas Zazai |
| Technology Used | Azure Functions, Python, Azure Cosmos DB, Azurite, Visual Studio Code |

---

---

## Demo Video

https://www.youtube.com/watch?v=goEXbON81lo

---

## Project Overview

This lab demonstrates a serverless text analysis application built with Azure Functions and Python.

The application exposes an HTTP-triggered API endpoint that receives text, analyzes it, returns text statistics, and stores the result in Azure Cosmos DB.

A second HTTP endpoint retrieves previous analysis results from Cosmos DB, allowing users to view the analysis history.

---

## What This Project Does

The project contains two main Azure Function endpoints:

| Endpoint | Method | Purpose |
|---|---|---|
| `/api/TextAnalyzer` | GET / POST | Analyzes text and saves the result to Cosmos DB |
| `/api/GetAnalysisHistory` | GET | Retrieves previous text analysis records |

---

## Features

The Text Analyzer function calculates:

- Word count
- Character count
- Character count without spaces
- Sentence count
- Paragraph count
- Average word length
- Longest word
- Estimated reading time
- Analysis timestamp
- Text preview

Each result is saved in Azure Cosmos DB with a unique ID.

---

## Architecture

```text
User / Browser / API Client
        |
        v
Azure Function HTTP Trigger
        |
        v
TextAnalyzer Function
        |
        v
Text analysis logic
        |
        v
Azure Cosmos DB for NoSQL
        |
        v
GetAnalysisHistory Function
        |
        v
Return stored analysis records
```

---

## Azure Resources Used

| Resource | Purpose |
|---|---|
| Azure Function App | Hosts the serverless Python functions |
| Azure Storage Account | Required by the Azure Functions runtime |
| Azure Cosmos DB for NoSQL | Stores text analysis results as JSON documents |
| Azurite | Local Azure Storage emulator for local function testing |

---

## Database Information

The application uses Azure Cosmos DB for NoSQL.

| Item | Value |
|---|---|
| Database name | `TextAnalyzerDB` |
| Container name | `AnalysisResults` |
| Partition key | `/id` |

Cosmos DB was selected because the analysis results are stored as JSON documents, which fits naturally with a NoSQL document database.

---

## Local Development Setup

### Required Tools

- Visual Studio Code
- Azure Functions extension for VS Code
- Python 3.12
- Azure Functions Core Tools
- Azurite extension
- Azure account

---

## Environment Variables

The following environment variables are required.

For local development, they are stored in `local.settings.json`.

For Azure deployment, they are stored in the Function App environment variables.

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "COSMOS_CONNECTION_STRING": "your-cosmos-db-connection-string",
    "COSMOS_DATABASE_NAME": "TextAnalyzerDB",
    "COSMOS_CONTAINER_NAME": "AnalysisResults"
  }
}
```

> Important: `local.settings.json` is not included in GitHub because it contains secrets.

---

## Python Dependencies

The project uses the following main Python packages:

- `azure-functions`
- `azure-cosmos`

Install dependencies with:

```bash
pip install -r requirements.txt
```

---

## Running the Project Locally

Start Azurite first:

```text
F1 > Azurite: Start
```

Then start the Azure Function locally:

```bash
func start
```

After the function starts, the local endpoints are available at:

```text
http://localhost:7071/api/TextAnalyzer
http://localhost:7071/api/GetAnalysisHistory
```

---

## Local Test - Text Analyzer

Example browser test:

```text
http://localhost:7071/api/TextAnalyzer?text=Serverless%20computing%20stores%20data%20in%20Cosmos%20DB.
```

Expected result:

```json
{
  "id": "generated-record-id",
  "analysis": {
    "wordCount": 7,
    "characterCount": 46,
    "characterCountNoSpaces": 40,
    "sentenceCount": 1,
    "paragraphCount": 1,
    "averageWordLength": 5.7,
    "longestWord": "Serverless",
    "readingTimeMinutes": 0.0
  },
  "metadata": {
    "analyzedAt": "timestamp",
    "textPreview": "Serverless computing stores data in Cosmos DB."
  },
  "message": "Analysis saved successfully."
}
```

---

## Local Test - Analysis History

Example browser test:

```text
http://localhost:7071/api/GetAnalysisHistory
```

Example response:

```json
{
  "count": 1,
  "results": [
    {
      "id": "generated-record-id",
      "analysis": {
        "wordCount": 7,
        "characterCount": 46,
        "characterCountNoSpaces": 40,
        "sentenceCount": 1,
        "paragraphCount": 1,
        "averageWordLength": 5.7,
        "longestWord": "Serverless",
        "readingTimeMinutes": 0
      },
      "metadata": {
        "analyzedAt": "timestamp",
        "textPreview": "Serverless computing stores data in Cosmos DB."
      }
    }
  ]
}
```

The history endpoint also supports a `limit` parameter:

```text
http://localhost:7071/api/GetAnalysisHistory?limit=5
```

---

## Azure Deployment

The function was deployed to Azure using Visual Studio Code:

```text
F1 > Azure Functions: Deploy to Function App
```

The deployed Azure Function App uses the same environment variable names as the local project.

---

## Azure Test Endpoints

Text Analyzer endpoint:

```text
https://func-cst8917-lab1-ilyas-c3gnhdb0cxcccpf5.eastus-01.azurewebsites.net/api/TextAnalyzer?text=Azure%20Function%20is%20saving%20data%20to%20Cosmos%20DB.
```

History endpoint:

```text
https://func-cst8917-lab1-ilyas-c3gnhdb0cxcccpf5.eastus-01.azurewebsites.net/api/GetAnalysisHistory
```

---

## Security Notes

- The Cosmos DB connection string is not committed to GitHub.
- `local.settings.json` is excluded from source control.
- Azure Function App environment variables are used for production configuration.
- The function uses anonymous HTTP access because this is a student lab project.

---

## Demo Video

The demo video link is available in:

```text
DEMO.md
```

---

## Files Included

| File | Purpose |
|---|---|
| `function_app.py` | Main Azure Functions code |
| `requirements.txt` | Python dependencies |
| `DATABASE_CHOICE.md` | Database selection explanation |
| `DEMO.md` | YouTube demo video link |
| `host.json` | Azure Functions host configuration |
| `.gitignore` | Prevents local and secret files from being committed |
| `.funcignore` | Prevents unnecessary files from being deployed |

---

## Conclusion

This lab helped me understand the full serverless workflow: creating an Azure Function locally, testing it with Azurite, deploying it to Azure, connecting it to Cosmos DB, and retrieving stored results through a second API endpoint.



---

*AI is used for Documentation*