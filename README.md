# Medical_Assitant

Medical Assistant — RAG-based Healthcare Q&A Chatbot

    A Retrieval-Augmented Generation (RAG) application that answers medical questions using a curated medical textbook as its knowledge source. Built with LangChain, Pinecone, HuggingFace Embeddings, and OpenRouter LLM.

Live Demo

    URL: https://medical-assitant.onrender.com

Project Structure
Medical_Assitant/
|── app.py # Flask web application entry point
|── store_index.py # One-time script to embed PDF and store in Pinecone
|── src/
| ├── helper.py # PDF loader, text splitter, embeddings downloader
| └── prompt.py # System prompt for the LLM
|── templates/
| └── index.html # Frontend chat UI
|── static/
| └── style.css # Stylesheet
|── Data/
| └── Medical_book.pdf # Source medical knowledge base
|── requirements.txt
|── Procfile # For Render deployment
|── runtime.txt # Python version for Render
|── setup.py

Setup and Run Instructions

    Python 3.10
    Conda (recommended) or virtualenv
    Pinecone account
    OpenRouter account
    OpenAI API key

Create and activate conda environment
conda create -n medibot python=3.10 -y
conda activate medibot

Install dependencies
pip install -r requirements.txt

Set up environment variables
PINECONE_API_KEY=your_pinecone_api_key
OPENAI_API_KEY=your_openai_api_key
OPENROUTER_API_KEY=your_openrouter_api_key

Run the application
python app.py

Architecture
User Question
│
Flask Web App (app.py)
│
HuggingFace Embeddings
│
Pinecone Vector Store ──► Top 3 relevant chunks
│
LangChain RAG Chain
│
OpenRouter LLM
│
Answer returned to User

Components

Web Framework - Flask 3.1.3
LLM - OpenRouter
Embeddings - HuggingFace all-MiniLM
Vector Database - Pinecone
RAG Framework - LangChain
PDF Parsing - PyPDF via LangChain DirectoryLoader

Agent / Tool Workflow

    This project uses a RAG (Retrieval-Augmented Generation) chain, not an agent. The workflow is:


        User sends a question via POST to /get
        Question is embedded using HuggingFace embeddings
        Top-3 similar chunks are retrieved from Pinecone
        Retrieved chunks + question are passed to the LLM via create_retrieval_chain
        LLM generates a grounded answer
        Answer is returned as plain text to the frontend


    Limitations and Future Improvements

    Current Limitations:


        Only one PDF as knowledge source — limited medical coverage
        No conversation memory — each question is independent
        Free tier Render instance sleeps after 15 minutes of inactivity
        No source citation shown to the user
        No fallback if Pinecone index is empty


    Future Improvements:


        Add conversation history for multi-turn dialogue
        Support multiple PDFs and larger medical datasets
        Add source document citations in responses
        Implement user authentication
        Add evaluation metrics (faithfulness, answer relevance)
        Stream responses for better UX
        Add Docker support for easier deployment
