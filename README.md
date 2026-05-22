# 🤖 AI Chat Agent Workflow

AI-powered chat agent built with n8n — uses Google Gemini to answer FAQs, check inventory, and log orders via Google Sheets.

## 📦 Workflow JSON

```json
{
  "name": "AI Chat Agent Workflow",
  "nodes": [
    {
      "parameters": {},
      "id": "trigger-node",
      "name": "When chat message received",
      "type": "@n8n/n8n-nodes-langchain.chatTrigger",
      "typeVersion": 1,
      "position": [180, 300]
    },
    {
      "parameters": {
        "options": {}
      },
      "id": "ai-agent-node",
      "name": "AI Agent",
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 1,
      "position": [500, 300]
    },
    {
      "parameters": {
        "modelName": "models/gemini-1.5-pro",
        "options": {}
      },
      "id": "gemini-chat-model",
      "name": "Google Gemini Chat Model",
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [300, 500]
    },
    {
      "parameters": {},
      "id": "simple-memory-node",
      "name": "Simple Memory",
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1,
      "position": [500, 500]
    },
    {
      "parameters": {
        "operation": "read",
        "documentId": {
          "__rl": true,
          "value": "YOUR_FAQ_SHEET_ID",
          "mode": "id"
        },
        "sheetName": {
          "__rl": true,
          "value": "Sheet1",
          "mode": "name"
        },
        "options": {}
      },
      "id": "faq-sheet-node",
      "name": "FAQ",
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4,
      "position": [500, 700]
    },
    {
      "parameters": {
        "operation": "read",
        "documentId": {
          "__rl": true,
          "value": "YOUR_INVENTORY_SHEET_ID",
          "mode": "id"
        },
        "sheetName": {
          "__rl": true,
          "value": "Sheet1",
          "mode": "name"
        },
        "options": {}
      },
      "id": "inventory-sheet-node",
      "name": "Inventory",
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4,
      "position": [700, 550]
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "YOUR_ORDERS_SHEET_ID",
          "mode": "id"
        },
        "sheetName": {
          "__rl": true,
          "value": "Sheet1",
          "mode": "name"
        },
        "columns": {
          "mappingMode": "autoMapInputData",
          "value": {}
        },
        "options": {}
      },
      "id": "orders-sheet-node",
      "name": "Orders",
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4,
      "position": [800, 700]
    }
  ],
  "connections": {
    "When chat message received": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "FAQ": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Inventory": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 1
          }
        ]
      ]
    },
    "Orders": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 2
          }
        ]
      ]
    }
  },
  "pinData": {},
  "settings": {
    "executionOrder": "v1"
  },
  "staticData": null,
  "tags": [],
  "triggerCount": 0,
  "updatedAt": "2026-05-22T00:00:00.000Z",
  "versionId": "1"
}
```
