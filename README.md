# 🏥 Medi_AI_Bot — AI Medical Assistant

> **Powered by LangChain · Pinecone · Groq LLaMA3 · RAG Pipeline**

---

## 📋 Table of Contents

- [About the Project](#-about-the-project)
- [Demo](#-demo)
- [Tech Stack](#-tech-stack)
- [Features](#-features)
- [Architecture & RAG Pipeline](#-architecture--rag-pipeline)
- [Knowledge Base](#-knowledge-base)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Installation & Setup](#-installation--setup)
- [Environment Variables](#-environment-variables)
- [Running the Application](#-running-the-application)
- [Usage Guide](#-usage-guide)
- [Quick Topics Supported](#-quick-topics-supported)
- [Medical Disclaimer](#-medical-disclaimer)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🩺 About the Project

**Med AI Bot** is an AI-powered medical chatbot built using a **Retrieval-Augmented Generation (RAG)** pipeline. It provides accurate, context-aware general health information by combining a curated medical knowledge base with the power of large language models.

Med AI Bot is designed to serve as a first point of reference for common medical queries — helping users understand symptoms, treatments, and general health guidelines — while always emphasizing that professional medical consultation is essential for diagnosis and treatment.

The chatbot is built entirely inside a **Jupyter Notebook** and deployed through a **Gradio** web interface, making it accessible and easy to run both locally and in the cloud (e.g., Google Colab).

---

## 🖼️ Demo
### 🌐 Live Demo
👉 **Public URL:** [Click here to open the app](https://d8c8a01a838bda9532.gradio.live/)

### 💬 Diabetes Query — Symptoms & Treatment

![Diabetes Medical Chatbot](https://github.com/AbhishekDhawan07/Medi_AI_Bot/blob/main/Medical%20AI%20Chatbot%20using%20LangChain%20%2B%20Pinecone%20%2B%20Groq%20LLM/Diabetes%20Medical%20Chatbot.png?raw=true)

*MedBot answering a question about diabetes symptoms and available treatments, using context retrieved from the medical knowledge base.*

---

### 🌡️ Fever Query — When to Worry

![Fever Medical Chatbot](https://github.com/AbhishekDhawan07/Medi_AI_Bot/blob/main/Medical%20AI%20Chatbot%20using%20LangChain%20%2B%20Pinecone%20%2B%20Groq%20LLM/Fever%20Medical%20chatbot.png?raw=true)

*MedBot providing guidance on when fever becomes a medical emergency, including information on lymphangitis and antimalarial drug interactions.*

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **LLM** | Groq — LLaMA 3.3 70B Versatile |
| **Orchestration** | LangChain (ConversationalRetrievalChain) |
| **Vector Database** | Pinecone |
| **Embeddings** | HuggingFace — `sentence-transformers/all-MiniLM-L6-v2` |
| **Frontend / UI** | Gradio Blocks |
| **Document Loader** | LangChain PyPDFLoader |
| **Text Splitter** | RecursiveCharacterTextSplitter |
| **Memory** | ConversationBufferWindowMemory (last 5 turns) |
| **Runtime** | Jupyter Notebook / Google Colab |
| **Environment** | Python-dotenv |

---

## ✨ Features

- 🤖 **Conversational AI** — Multi-turn medical Q&A powered by LLaMA 3.3 70B via Groq
- 🔍 **RAG Pipeline** — Retrieves relevant context from a vector database before generating answers
- 📚 **Dual Knowledge Source** — Works with both a medical PDF book (if provided) and a built-in curated knowledge base
- 🧠 **Conversation Memory** — Remembers the last 5 exchanges for contextual continuity
- ⚡ **Quick Topic Buttons** — One-click access to 12 pre-loaded medical topics
- 🌐 **Gradio Web Interface** — Clean, browser-based UI with shareable public link
- 🔒 **Safe by Design** — Always reminds users to consult a qualified doctor; never gives standalone dosage recommendations
- 📄 **PDF Support** — Optionally load any medical PDF to extend the knowledge base
- 🧩 **Modular Notebook** — Each cell is a self-contained step: load → embed → retrieve → generate → serve

---

## 🏗️ Architecture & RAG Pipeline
```
User Question
     │
     ▼
┌─────────────────────────────┐
│    Gradio Web Interface     │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│  ConversationalRetrievalChain│
│  (LangChain Orchestration)  │
└────┬──────────────────┬─────┘
     │                  │
     ▼                  ▼
┌──────────┐     ┌──────────────────┐
│  Groq    │     │   Pinecone       │
│ LLaMA3   │     │  Vector Store    │
│  (LLM)   │     │  (Retriever k=4) │
└──────────┘     └──────────────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │  HuggingFace     │
                 │  Embeddings      │
                 │ (MiniLM-L6-v2)  │
                 └──────────────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │  Medical PDF or  │
                 │  Built-in Texts  │
                 └──────────────────┘
```

**How it works:**

1. **Ingestion** — Medical texts (PDF or built-in) are loaded and split into 500-token chunks with 50-token overlap.
2. **Embedding** — Each chunk is embedded using `all-MiniLM-L6-v2` and stored in Pinecone.
3. **Retrieval** — On every user query, the top-4 most similar chunks are retrieved from Pinecone.
4. **Generation** — Retrieved context + conversation history is passed to LLaMA 3.3 via Groq to generate a grounded, empathetic response.
5. **Memory** — The last 5 turns of conversation are maintained for contextual follow-ups.

---

## 📖 Knowledge Base

MedBot ships with a built-in knowledge base covering **10 core medical topics**. If a `Medical_book.pdf` is found in the working directory, it is used instead for richer context.

| # | Topic | Key Content |
|---|---|---|
| 1 | **Diabetes Mellitus** | Type 1 & 2, symptoms, treatments, HbA1c targets, complications |
| 2 | **Hypertension** | BP thresholds, DASH diet, medications, complications |
| 3 | **Fever** | Causes, treatment (paracetamol/ibuprofen), emergency signs |
| 4 | **Common Cold** | Rhinovirus, symptomatic treatment, antibiotic warning |
| 5 | **Asthma** | Triggers, inhalers (salbutamol, ICS), Asthma Action Plan |
| 6 | **Migraine** | Aura, triggers, triptans, NSAIDs, red flags |
| 7 | **COVID-19** | Symptoms, high-risk groups, Long COVID, prevention |
| 8 | **CPR & First Aid** | 30:2 ratio, AED, Heimlich manoeuvre, burns treatment |
| 9 | **Mental Health** | Depression, anxiety, SSRIs, CBT, India crisis helplines |
| 10 | **Nutrition** | Diet guidelines, BMI categories, exercise recommendations |

---

## 📁 Project Structure
```
Medi_AI_Bot/
├── 📄 README.md 
├── Medical AI Chatbot using LangChain + Pinecone + Groq LLM/
│   ├── Medical_AI_Chatbot.ipynb          # Main Jupyter Notebook (all-in-one)
│   ├── Diabetes Medical Chatbot.png      # Screenshot 1
│   └── Fever Medical chatbot.png         # Screenshot 2
│   └── Medical_book.pdf                  # custom medical PD
├── .env                                  # API Keys(not commited to git)

              
```

> **Note:** The entire application — data ingestion, embedding, RAG chain setup, and Gradio UI — lives inside a single Jupyter Notebook for portability.

---

## ✅ Prerequisites

- Python 3.9 or higher
- A [Groq API Key](https://console.groq.com/) (free tier available)
- A [Pinecone API Key](https://www.pinecone.io/) (free tier available)


---

## 🚀 Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/AbhishekDhawan07/Medi_AI_Bot.git
cd Medi_AI_Bot
```

### 2. Install Dependencies
```bash
pip install langchain langchain-community langchain-groq langchain-pinecone \
            pinecone-client sentence-transformers gradio pypdf python-dotenv
```

Or in a Jupyter / Colab cell:
```python
!pip install langchain langchain-community langchain-groq langchain-pinecone \
             pinecone-client sentence-transformers gradio pypdf python-dotenv
```


## 🔑 Environment Variables

Create a `.env` file in the project root:
```env
GROQ_API_KEY=your_groq_api_key_here
PINECONE_API_KEY=your_pinecone_api_key_here
```

Alternatively, use the built-in helper function in the notebook:
```python
save_api_keys(
    groq_api_key="gsk_...",
    pinecone_api_key="pcsk_..."
)
```

---

## ▶️ Running the Application

Open `Medical_AI_Chatbot.ipynb` and run the cells in order:

| Cell | Purpose |
|---|---|
| **Cell 1** | Import libraries |
| **Cell 2** | Save API keys to `.env` |
| **Cell 3** | Load environment variables |
| **Cell 4** | Load medical texts / PDF and create chunks |
| **Cell 5** | Embed chunks and push to Pinecone vector store |
| **Cell 6** | Connect to Pinecone, build LLM + RAG chain, run test query |
| **Cell 7** | Launch Gradio UI (generates public share link) |

> After running the final cell, Gradio prints a **public URL** (e.g., `https://xxxx.gradio.live`) valid for 72 hours — share it with anyone!

---

## 📱 Usage Guide

1. **Open the Gradio URL** in any browser.
2. **Type a question** in the "Your Question" text box and press **Send** or hit Enter.
3. **Use Quick Topic buttons** on the left sidebar to instantly ask pre-loaded questions about common conditions.
4. **Continue the conversation** — MedBot remembers context for follow-up questions.
5. Press **Clear** to reset the conversation and start fresh.

---

## ⚡ Quick Topics Supported

Click any button in the sidebar to instantly query MedBot:

| Button | Query Sent |
|---|---|
| Diabetes | What are diabetes symptoms and treatment? |
| Hypertension | How to manage high blood pressure? |
| Common Cold | Remedies for common cold? |
| Asthma | Asthma symptoms, triggers, treatment? |
| Headache | Migraine causes and treatment? |
| COVID-19 | COVID-19 symptoms and precautions? |
| Mental Health | Tell me about depression and anxiety. |
| Nutrition | Healthy diet and nutrition tips. |
| First Aid CPR | How to perform CPR? |
| Fever | When to worry about fever? |
| Medications | Common medications and uses. |
| Sleep | How much sleep do adults need? |

---

## ⚠️ Medical Disclaimer

> **DISCLAIMER:** MedBot provides **general health information only**. It is **NOT** a substitute for professional medical diagnosis, advice, or treatment. Always consult a qualified and licensed healthcare professional for any medical concerns. Never disregard professional medical advice or delay seeking it because of information provided by this chatbot.

For mental health emergencies in India:
- **iCall:** 9152987821
- **Vandrevala Foundation:** 1860-2662-345
- **AASRA:** 9820466627

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

<div align="center">
  Built with ❤️ using LangChain · Pinecone · Groq · Gradio
</div>
