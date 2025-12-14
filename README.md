# 🤖 AI-Powered Adidas Customer Support Agent on WhatsApp

<div align="center">

![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Twilio](https://img.shields.io/badge/Twilio-F22F46?style=for-the-badge&logo=twilio&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-F55036?style=for-the-badge&logo=ai&logoColor=white)
![Airtable](https://img.shields.io/badge/Airtable-18BFFF?style=for-the-badge&logo=airtable&logoColor=white)

**An intelligent AI customer support chatbot that handles real-time customer queries via WhatsApp, powered by Groq AI and automated with n8n workflows.**

[Features](#-features) • [Architecture](#-architecture) • [Setup](#-setup) • [Usage](#-usage) • [Demo](#-demo)

</div>

---

## 📋 Overview

This project demonstrates a production-ready AI customer support system for Adidas that operates entirely on WhatsApp. Built using **n8n workflow automation**, **Twilio WhatsApp API**, and **Groq's LLM**, the chatbot provides instant 24/7 support for:

- ❓ Answering FAQs (return policies, shipping, store hours)
- 📦 Order tracking with real-time status updates
- 🛍️ Product availability and inventory queries
- 🎫 Automated ticket creation for complex issues
- 💬 Natural, conversational AI responses

---

## ✨ Features

### 🧠 AI-Powered Conversations
- **Context-aware responses** using Groq's `gpt-oss-120b` model
- Professional, friendly tone matching brand voice
- Handles multiple conversation flows simultaneously

### 📱 WhatsApp Integration
- Real-time message processing via Twilio
- Seamless two-way communication
- Supports text, emojis, and formatted responses

### 🔧 Smart Automation
- **Order Tracking**: Automatically fetches order status from Airtable
- **Product Queries**: Searches inventory database for pricing and availability
- **Ticket Management**: Creates support tickets with auto-assignment
- **Knowledge Base**: Instant access to FAQs and policies

### 🗄️ Airtable Database Integration
- **Orders Table**: Track order IDs, status, and delivery information
- **Inventory Table**: Product catalog with pricing and stock levels
- **Tickets Table**: Support request management with assignee tracking

---

## 🏗️ Architecture

### Workflow Components

![Workflow Screenshot](./screenshots/workflow.png)

```
┌─────────────────┐
│  WhatsApp User  │
└────────┬────────┘
         │ Message
         ▼
┌─────────────────┐
│  Twilio Trigger │ ◄── Webhook receives incoming messages
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Edit Fields    │ ◄── Format/prepare data
└────────┬────────┘
         │
         ▼
┌──────────────────────────────────────────┐
│           AI Agent (LangChain)          │
│  ┌───────────────────────────────────┐  │
│  │  System Prompt:                   │  │
│  │  - Customer support assistant     │  │
│  │  - Adidas brand context           │  │
│  │  - Conversational guidelines      │  │
│  └───────────────────────────────────┘  │
│                                          │
│  Connected Tools:                        │
│  ├─ 🗃️ order_tracking (Airtable)        │
│  ├─ 📋 order_records (Airtable)         │
│  └─ 🎫 create_tickets (Airtable)        │
│                                          │
│  Powered by:                             │
│  └─ ⚡ Groq Chat Model (GPT-OSS-120B)   │
└──────────────────┬───────────────────────┘
                   │
                   ▼
         ┌─────────────────┐
         │  Send WhatsApp  │ ◄── Reply to user
         │    Message      │
         └─────────────────┘
```

### Tech Stack

| Component | Technology | Purpose |
|-----------|------------|----------|
| **Automation Platform** | n8n | Workflow orchestration and execution |
| **Messaging** | Twilio WhatsApp API | WhatsApp Business integration |
| **AI Model** | Groq (gpt-oss-120b) | Natural language understanding & generation |
| **Database** | Airtable | Order management, inventory, tickets |
| **Framework** | LangChain | AI agent with tool-calling capabilities |

---

## 🚀 Setup

### Prerequisites

- n8n instance (self-hosted or cloud)
- Twilio account with WhatsApp sandbox or approved number
- Groq API key (get from [console.groq.com](https://console.groq.com))
- Airtable account with Personal Access Token

### Step 1: Clone the Repository

```bash
git clone https://github.com/Rahul4112002/AI-Adidas-Customer-Support-WhatsApp-Twilio-n8n.git
cd AI-Adidas-Customer-Support-WhatsApp-Twilio-n8n
```

### Step 2: Setup Airtable Database

Create an Airtable base with three tables:

#### **Orders Table**
- `order_id` (Text)
- `customer_name` (Text)
- `status` (Single Select: Shipped, Delivered, Processing)
- `delivery_date` (Date)
- `tracking_number` (Text)

#### **Inventory Table**
- `product_name` (Text)
- `product_price` (Currency)
- `stock_quantity` (Number)
- `category` (Single Select)

#### **Tickets Table**
- `Name` (Text) - Customer name
- `Notes` (Long Text) - Issue description
- `Assignee` (Text) - Support team member
- `Status` (Single Select: Todo, In Progress, Done)

### Step 3: Configure Credentials in n8n

1. **Twilio API**
   - Go to n8n Settings → Credentials → Add Credential
   - Select "Twilio API"
   - Enter Account SID and Auth Token from Twilio Console

2. **Groq API**
   - Add "Groq API" credential
   - Enter API key from Groq console

3. **Airtable**
   - Add "Airtable Personal Access Token"
   - Generate token with `data.records:read` and `data.records:write` scopes
   - Enter your Base ID

### Step 4: Import Workflow

1. Open n8n dashboard
2. Click "Import from File"
3. Select `workflow.json`
4. Update credential references to your newly created credentials
5. Update Twilio phone numbers in the "Send WhatsApp Message" node

### Step 5: Configure Twilio Webhook

1. Activate the workflow in n8n
2. Copy the webhook URL from the "Twilio Trigger" node
3. In Twilio Console → WhatsApp Sandbox Settings:
   - Set "When a message comes in" to your webhook URL
   - Save configuration

### Step 6: Customize the AI Agent

Edit the **System Message** in the AI Agent node to customize:
- Brand voice and tone
- Business hours
- Available products/services
- FAQs and policies
- Default assignee for tickets

---

## 💬 Usage

### Starting a Conversation

1. Send a WhatsApp message to your Twilio number
2. The AI agent (Alex) will greet you
3. Ask questions naturally

**Example Interactions:**

![WhatsApp Demo](./screenshots/whatsapp-chat.png)

```
User: Hi, I need support
Bot: Hi Rahul! How can I assist you today? If you need to track an 
     order, just share the order ID. If you have another question or 
     issue, let me know the details and I'll take care of it for you.

User: What is the return policy?
Bot: Return Policy: Returns are accepted within 5 days of purchase.

User: Which product is very expensive?
Bot: Sure! Are you looking for a particular type of shoe (e.g., running, 
     basketball, lifestyle) or a price range you have in mind? Let me 
     know so I can point you to our premium options.
```

### Supported Commands

- **Order Tracking**: "Track my order #12345"
- **Product Queries**: "Show me expensive shoes", "What's available in running shoes?"
- **FAQs**: "Return policy", "Shipping time", "Store hours"
- **Create Ticket**: "I want to speak to someone", "My order is damaged"

---

## 📸 Demo

### WhatsApp Conversation Flow
Real customer interaction showing AI-powered responses and order tracking.

### n8n Workflow Dashboard
Complete automation workflow with all connected nodes and tools.

---

## 🎯 Key Capabilities

### 1. **Intelligent Order Tracking**
```
User: Track order #ORD123
→ AI queries Airtable Orders table
→ Returns: "Your order is Shipped! Expected delivery: Dec 18, 2025"
```

### 2. **Dynamic Product Search**
```
User: Show me shoes under $100
→ AI filters Inventory table by price
→ Returns: List of matching products with prices
```

### 3. **Automated Ticket Creation**
```
User: I received the wrong size
→ AI asks for: Name, Issue details
→ Creates Airtable ticket with auto-assignment
→ Confirms: "Thanks! I've submitted this to our team."
```

### 4. **Context-Aware Responses**
- Maintains conversation context
- Asks clarifying questions
- Handles multiple topics in one session

---

## 🔧 Customization

### Modify AI Behavior

Edit the system prompt in the **AI Agent** node:

```javascript
You are an AI WhatsApp assistant for [YOUR BRAND]. 
You help customers with [YOUR SERVICES]...
```

### Add More Tools

1. Create new Airtable tool nodes in n8n
2. Connect to the AI Agent node
3. Update system prompt with tool usage instructions

### Change AI Model

Replace Groq Chat Model with:
- OpenAI GPT-4
- Anthropic Claude
- Google Gemini
- Any n8n-supported LLM

---

## 📊 Workflow Nodes Breakdown

| Node | Type | Purpose |
|------|------|----------|
| **Twilio Trigger** | Trigger | Listens for incoming WhatsApp messages |
| **Edit Fields** | Data | Formats message data for AI processing |
| **AI Agent** | LangChain | Core conversational AI with tool-calling |
| **Groq Chat Model** | LLM | Provides language understanding |
| **create_tickets** | Airtable Tool | Creates support tickets |
| **order_records** | Airtable Tool | Searches product inventory |
| **order_tracking** | Airtable Tool | Retrieves order status |
| **Send WhatsApp Message** | Twilio | Sends AI response back to user |

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

- Report bugs
- Suggest features
- Submit pull requests
- Share improvements

---

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

---

## 👨‍💻 Author

**Rahul Chauhan**
- GitHub: [@Rahul4112002](https://github.com/Rahul4112002)
- Twitter: [@R4hulChauhan](https://twitter.com/R4hulChauhan)
- Portfolio: [rahul4112.me](https://rahul4112.me)

---

## 🙏 Acknowledgments

- [n8n](https://n8n.io) - Workflow automation platform
- [Twilio](https://twilio.com) - WhatsApp API integration
- [Groq](https://groq.com) - Lightning-fast LLM inference
- [Airtable](https://airtable.com) - Flexible database solution

---

<div align="center">

**⭐ Star this repo if you found it helpful!**

</div>