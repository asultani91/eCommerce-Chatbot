
# 🤖 Agentic AI: Product Recommendation Chatbot with Amazon Bedrock Agents

A hands-on workshop project demonstrating how to build an intelligent, serverless e-commerce chatbot using **Amazon Bedrock Agents**. The chatbot autonomously makes decisions, calls APIs, and takes actions to help users find and purchase gifts through natural language interaction.

> **Target Audience:** Solution Architects, Software Designers, and Developers
> **Duration:** ~1 hour
> **Region:** US West (Oregon) — `us-west-2`

---

## 📐 Architecture

The solution uses a fully **serverless architecture**:

| Service | Role |
|---|---|
| Amazon Bedrock Agents | Orchestrates the conversational AI |
| AWS Lambda | Executes business logic and API calls |
| Amazon DynamoDB | Stores product and cart data |
| Amazon S3 | Hosts knowledge base documents |
| Knowledge Bases for Amazon Bedrock | Provides RAG (Retrieval Augmented Generation) capabilities |
| Amazon Personalize | ML-based product recommendations (simulated) |

---

## 🚀 Workshop Sections

### 1. 🛍️ Product Recommendation Agent

**Components Created:**
- DynamoDB table: `producttableandapi-ws-Products-XXXX`
  - Attributes: `product_name` (partition key), `category`, `gender`, `occasion`
  - Contains 100 sample products
- Lambda: `GetProductDetailsFunction` — retrieves filtered product data
- Lambda: `PopulateProductsTableFunction` — generates sample data

**Agent Configuration:**
- **Name:** `product-recommendation-agent`
- **Model:** Anthropic Claude Sonnet 4.5
- **Action Group:** `get-product-recommendations` (OpenAPI schema)
- **User Input:** Enabled — agent asks clarifying questions

**Result:** Agent asks about recipient, occasion, and preferences, then recommends relevant products.

---

### 2. 🛒 Cart Actions

**New Components:**
- DynamoDB table: `producttableandapi-ws-Cart-XXXXX` (`user_id`, `product_name`)
- Lambda: `GetCartFunction` — retrieves cart items
- Lambda: `AddToCartFunction` — adds products to cart

**New Action Groups:**
1. `get-cart` — retrieves cart for a user ID
2. `add-item-to-cart` — adds products to user's cart

**Result:** Agent can add products to cart, track user ID across conversation, and display cart contents.

---

### 3. 🎯 Amazon Personalize Integration

**Component:**
- Lambda: `GetPersonalizeRecommendationFunction` — simulates *"Customers who bought X also bought Y"* recommendations

**Action Group:** `get-amazon-personalize-recommendation`

**Result:** Agent proactively suggests related products with social proof to increase average cart size.

---

### 4. 🎁 Gift Wrapping Knowledge Base (RAG)

**Components:**
- S3 bucket with `Gift-wrapping.txt`
- Knowledge Base: `GiftWrappingKnowledgeBase`
- Data Source: `GiftWrappingDataSource`

**Process:**
1. Synchronize data source in Knowledge Bases console
2. Add knowledge base to agent
3. Agent queries knowledge base based on cart contents

**Result:** Agent suggests creative, context-aware gift wrapping ideas using Retrieval Augmented Generation (RAG).

---

### 5. 🤝 Multi-Agent Collaboration

Implements a **supervisor-collaborator pattern** with specialized agents.

#### Specialized Agents:

**A. Product Details Agent**
- **Name:** `product-details-agent`
- **Focus:** Product exploration and recommendations
- **Action Group:** `get-product-recommendations`
- **Alias:** `get-product-alias`

**B. Cart Management Agent**
- **Name:** `cart-management-agent`
- **Focus:** Cart operations (add/retrieve)
- **Action Groups:** `add-item-to-cart`, `get-cart`
- **Alias:** `cart-agent-alias`

#### Supervisor Agent:
- **Name:** `shopping-supervisor-agent`
- **Multi-agent collaboration:** ENABLED
- **Role:** Intelligently routes requests to the appropriate specialized agent

**Collaborators:**
| Collaborator | Agent | When Invoked |
|---|---|---|
| `product-recommender` | product-details-agent | User needs product recommendations (called first) |
| `cart-manager` | cart-management-agent | User wants to add items or view cart |

**Conversation History:** Enabled on both collaborators to maintain context, prevent redundant questions, and ensure smooth transitions.

---

## 🧠 Key Concepts Demonstrated

| Concept | Description |
|---|---|
| Agentic AI | Autonomous, goal-driven AI systems |
| Action Groups | APIs that agents can invoke |
| Knowledge Bases | RAG for enterprise/domain data |
| Multi-Agent Collaboration | Specialized agents working together |
| Conversation History | Context sharing between agents |
| OpenAPI Schemas | Defining agent capabilities |
| Orchestration | Agent decision-making and reasoning |

---

## 🛠️ Technologies Used

- **Amazon Bedrock Agents**
- **Anthropic Claude Sonnet 4.5**
- **AWS Lambda**
- **Amazon DynamoDB**
- **Amazon S3**
- **Knowledge Bases for Amazon Bedrock**
- **Amazon Personalize**
- **OpenAPI Schemas**
- **AWS IAM Roles**

---

## 📸 Screenshots

> Screenshots from the AWS Console are included in the `/images` folder, covering:
> - Model selection (Claude Sonnet 4.5)
> - Agent details configuration
> - Action group creation
> - OpenAPI schema editor
> - Test agent conversation window
> - Trace/debugging interface
> - DynamoDB table contents
> - Multi-agent collaboration settings
> - Conversation history toggles

---

## 📋 Summary

This project progressively builds a production-ready conversational AI system:

1. ✅ Basic product recommendations via Bedrock Agent
2. ✅ Cart management with DynamoDB
3. ✅ ML-based personalization with Amazon Personalize
4. ✅ Knowledge base integration for gift wrapping ideas (RAG)
5. ✅ Multi-agent collaboration with supervisor pattern

---

## 👤 Author

**Ahmad Sultani**
- 🔗 [LinkedIn](https://linkedin.com/in/asultani91)
- ☁️ AWS Partner: Generative AI Essentials (December 2025)
- 🎯 AWS Solutions Architect Associate (SAA-C03) — In Progress
