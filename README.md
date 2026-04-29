🎬 Chat with Any YouTube Video | LangChain RAG Pipeline
A RAG (Retrieval-Augmented Generation) based system that lets you chat with any YouTube video using its transcript.
Just provide a YouTube Video ID and ask anything — the system retrieves relevant context and answers using an LLM

🚀 How It Works

YouTube Video ID
      ↓
Fetch Transcript (YouTubeTranscriptApi)
      ↓
Split into Chunks (RecursiveCharacterTextSplitter)
      ↓
Generate Embeddings (OpenAI text-embedding-3-small)
      ↓
Store in Vector Store (FAISS)
      ↓
User Question → Retrieve Relevant Chunks
      ↓
Augment Prompt → Generate Answer (GPT-4o-mini)

🛠️ Tech Stack
ToolPurpose🦜 LangChainRAG pipeline orchestration
🎬 YouTubeTranscriptApiFetch video transcripts
🗄️ FAISSVector store for embeddings
🤖 OpenAI GPT-4o-miniAnswer generation
📐 OpenAI Embeddingstext-embedding-3-small
🐍 PythonProgramming language

📦 Installation
1. Clone the Repository
bashgit clone https://github.com/waseemmalik82/YouTube-RAG-Pipeline-LangChain.git
cd YouTube-RAG-Pipeline-LangChain
2. Create Virtual Environment
bashpython -m venv venv
3. Activate Virtual Environment
bash# Windows (Command Prompt)
venv\Scripts\activate.bat

# Mac/Linux
source venv/bin/activate
4. Install Dependencies
bashpython -m pip install langchain langchain-openai langchain-community faiss-cpu youtube-transcript-api openai tiktoken python-dotenv
5. Set Up API Key
Create a .env file in the root directory:
OPENAI_API_KEY=your_openai_api_key_here

🎯 Usage
Step 1 — Set Your YouTube Video ID
Open the notebook and set the video ID (not the full URL):
pythonvideo_id = "Gfr50f6ZBvo"  # Example: https://youtube.com/watch?v=Gfr50f6ZBvo
Step 2 — Run All Cells
The pipeline will automatically:

Fetch the transcript
Split into chunks
Generate and store embeddings
Set up the retriever and LLM chain

Step 3 — Ask Questions
pythonmain_chain.invoke("Can you summarize the video?")
main_chain.invoke("What is discussed about nuclear fusion?")
main_chain.invoke("Who is Demis Hassabis?")

🏗️ RAG Pipeline Breakdown
Step 1 — Indexing (Document Ingestion)
pythontranscript_list = YouTubeTranscriptApi.get_transcript(video_id, languages=["en"])
transcript = " ".join(chunk["text"] for chunk in transcript_list)
Step 2 — Text Splitting
pythonsplitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.create_documents([transcript])
Step 3 — Embedding & Vector Store
pythonembeddings = OpenAIEmbeddings(model="text-embedding-3-small")
vector_store = FAISS.from_documents(chunks, embeddings)
Step 4 — Retrieval
pythonretriever = vector_store.as_retriever(search_type="similarity", search_kwargs={"k": 4})
Step 5 — Generation (LangChain Chain)
pythonmain_chain = parallel_chain | prompt | llm | parser
main_chain.invoke("Your question here")

🗂️ Project Structure
YouTube-RAG-Pipeline-LangChain/
│
├── YouTube_RAG_Pipeline.ipynb   # Main notebook
├── .env                          # API keys (not pushed to GitHub)
├── .gitignore                    # Ignored files
└── README.md                     # This file

⚠️ Important Notes

Video must have English captions/subtitles available
Never push your .env file or API key to GitHub
venv/ folder should be in .gitignore
OpenAI API key is required for embeddings and generation


💡 Example Questions to Ask
pythonmain_chain.invoke("Can you summarize the video?")
main_chain.invoke("What are the main topics discussed?")
main_chain.invoke("Explain [specific concept] mentioned in the video")
main_chain.invoke("What did the speaker say about [topic]?")

👨‍💻 Author
Waseem Malik
GitHub: @waseemmalik82

📄 License
This project is open source and available under the MIT License.
