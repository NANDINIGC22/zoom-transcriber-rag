README.md (Markdown for GitHub)
markdown
Copy
Edit
<p align="center">
  <img src="https://img.icons8.com/ios-filled/100/000000/zoom.png" width="70" />
  <img src="https://img.icons8.com/ios-filled/100/000000/transcription.png" width="70"/>
  <img src="https://img.icons8.com/ios-filled/100/000000/artificial-intelligence.png" width="70"/>
</p>

<h1 align="center">Zoom Meeting Transcriber & AI Q&A Assistant 🎙️📄🤖</h1>

<p align="center">
  Upload Zoom recordings, get transcripts, and ask smart questions using AI.<br/>
  Powered by Faster-Whisper · ChromaDB · LangChain · OpenAI · Gradio
</p>

---

## 📌 Features

✅ Upload Zoom audio/video  
✅ Transcribe using faster-whisper  
✅ Store as vector embeddings using OpenAI + ChromaDB  
✅ Ask intelligent questions using LangChain + GPT  
✅ Simple UI with Gradio  

---

## 🧰 Tech Stack

| Feature             | Tool/Library            |
|---------------------|-------------------------|
| Transcription       | faster-whisper          |
| Embeddings          | OpenAI                  |
| Vector DB           | ChromaDB                |
| Retrieval/QA Logic  | LangChain               |
| UI Interface        | Gradio                  |
| Deployment Ready    | Python, .env            |

---

## 🚀 Quick Start

### 1. Clone the Repo

```bash
git clone https://github.com/NANDINIGC22/zoom-transcriber-rag.git
cd zoom-transcriber-rag
2. Set OpenAI API Key
Create a file called .env in your project folder:

bash
Copy
Edit
touch .env
Paste the following inside:

env
Copy
Edit
OPENAI_API_KEY=your_openai_api_key_here
🧪 You can get your key from https://platform.openai.com/account/api-keys

3. Install Dependencies
bash
Copy
Edit
pip install -r requirements.txt
4. Run the App
bash
Copy
Edit
python zoom_rag.py
This will open a Gradio app in your browser.

📸 Screenshots
(Upload your screenshots in a folder named assets/ and update these links)

🟢 Upload Interface

🔍 Ask Meeting Questions

🧠 How It Works
Upload Zoom recording (audio/video)

Faster-Whisper converts speech to text

LangChain splits text and creates embeddings

Stores vectors in ChromaDB

On query: uses similarity search (top k) for context

GPT model answers using RAG pipeline

🗂️ Project Structure
bash
Copy
Edit
zoom-transcriber-rag/
│
├── zoom_rag.py            # Main app logic
├── requirements.txt       # All required packages
├── .env                   # (Your OpenAI key goes here)
├── assets/                # (Optional: screenshots for README)
└── README.md              # This file
📜 License
MIT License © 2025
Created with 💙 by Nandini GC

markdown
Copy
Edit
