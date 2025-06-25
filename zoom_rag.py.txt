import os
import tempfile
import gradio as gr
from dotenv import load_dotenv
from faster_whisper import WhisperModel

from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_community.vectorstores import Chroma
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain.docstore.document import Document

# Load environment variables from .env file
load_dotenv()
api_key = os.getenv("OPENAI_API_KEY")


# 1. Transcribe Zoom Recording using faster-whisper
def transcribe_audio(file_path):
    model = WhisperModel("base", device="cpu", compute_type="int8")  # Use "cuda" if you have GPU
    segments, _ = model.transcribe(file_path)
    transcript = " ".join([seg.text for seg in segments])
    return transcript


# 2. Chunk Transcript and Create Vector Store
def create_vector_store(transcript):
    text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
    docs = text_splitter.create_documents([transcript])
    embeddings = OpenAIEmbeddings(openai_api_key=os.getenv("OPENAI_API_KEY"))

    # Create and save Chroma vector store
    db = Chroma.from_documents(docs, embeddings, persist_directory="./chroma_db")
    db.persist()


# 3. Load Vector Store and Answer Queries
def answer_query(query):
    embeddings = OpenAIEmbeddings(openai_api_key=os.getenv("OPENAI_API_KEY"))
    db = Chroma(persist_directory="./chroma_db", embedding_function=embeddings)

    retriever = db.as_retriever(search_type="similarity", k=5)

    llm = ChatOpenAI(
        model_name="gpt-3.5-turbo",
        openai_api_key=os.getenv("OPENAI_API_KEY")
    )

    qa = RetrievalQA.from_chain_type(
        llm=llm,
        chain_type="stuff",
        retriever=retriever
    )

    return qa.run(query)



# 4. Upload File ➝ Transcribe ➝ Embed ➝ Store
def upload_and_process(audio_bytes):
    with tempfile.NamedTemporaryFile(delete=False, suffix=".mp3") as tmp:
        tmp.write(audio_bytes)
        tmp_path = tmp.name
    transcript = transcribe_audio(tmp_path)
    create_vector_store(transcript)
    return "✅ Transcription and vector store complete. You can now ask questions!"


# Gradio UI
upload_interface = gr.Interface(
    fn=upload_and_process,
    inputs=gr.File(type="binary", label="Upload Zoom Audio/Video"),
    outputs="text",
    title="Zoom Meeting Transcriber + Q&A (faster-whisper + LangChain)",
    description="Upload a Zoom recording. It will be transcribed and stored. Then ask questions about the session."
)

qa_interface = gr.Interface(
    fn=answer_query,
    inputs=gr.Textbox(lines=2, placeholder="Ask a question about the Zoom session..."),
    outputs="text"
)

app = gr.TabbedInterface(
    [upload_interface, qa_interface],
    ["Upload & Transcribe", "Ask a Question"]
)

app.launch(share=True)
