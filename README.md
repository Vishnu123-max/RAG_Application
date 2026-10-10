# PDF RAG Application

A notebook-based Retrieval-Augmented Generation (RAG) learning project. It demonstrates a document question-answering pipeline that loads PDF pages, splits them into chunks, embeds the text, retrieves relevant passages from a persistent vector store, and uses a hosted language model to generate an answer.

> **Project status:** Experimental prototype implemented in Jupyter notebooks. This is not a production-ready service or packaged application. The project archive and sample documents will be added separately.

## How it works

```text
PDF files → PyPDFLoader → text chunks → Sentence Transformers embeddings
          → persistent ChromaDB collection → top-k retrieval
          → retrieved context + question → Groq chat model → answer and source details
```

The PDF workflow uses LangChain's `PyPDFLoader` and `RecursiveCharacterTextSplitter` (1,000-character chunks with 200-character overlap). Text is embedded locally with `all-MiniLM-L6-v2` and stored in a persistent ChromaDB collection. The retrieval code returns matching text and metadata, including source and page information. The notebook then passes retrieved context to a Groq-hosted model; additional notebook functions demonstrate source previews, citation text, optional summaries, and in-memory query history.

A second notebook contains introductory LangChain document-loading examples for text files and PDFs.

## Technology

- Python and Jupyter notebooks
- LangChain and PyPDF
- Sentence Transformers (`all-MiniLM-L6-v2`)
- ChromaDB for persistent vector storage
- Groq chat model integration

## Repository contents

The project archive is intended to provide the notebooks and dependency list. The working project also uses a `data/` directory for user-provided documents and a local ChromaDB vector store. PDFs, generated vector data, virtual environments, and API keys should remain local and should not be committed.

## Setup and usage

1. Extract the project archive into this repository.
2. Create and activate a Python virtual environment.
3. Install dependencies from `requirements.txt`.
4. Place PDFs you are permitted to process in the project's `data/` directory.
5. Configure your Groq API key locally as `GROQ_API_KEY` (for example, in an untracked `.env` file); never put credentials in notebooks or commits.
6. Open and run the PDF RAG notebook from its expected working directory so its relative paths resolve. Run cells in order to load documents, split and embed them, populate ChromaDB, retrieve context, and generate answers.

Exact environment and notebook instructions may be updated when the project archive is added and verified.

## Current limitations

- Notebook-centric prototype; no web UI, API, automated ingestion service, or test suite.
- Re-running ingestion may add duplicate chunks unless the persistent collection is rebuilt or IDs are made deterministic.
- Retrieval thresholds and the distance-to-similarity calculation need validation against the configured ChromaDB distance metric; the reported score is not a calibrated answer-confidence measure.
- The notebook's `stream` option is a simulation, not actual token streaming.
- Query history exists only in memory for the running notebook session.
- The prompts and citation handling are basic and need stronger grounding/abstention behavior for reliable answers.

## Security and data

Do not commit `.env` files, API keys, private documents, copyrighted PDFs without redistribution permission, or generated vector-store data. Any API key previously placed in a notebook should be revoked and replaced. Review notebook outputs before sharing because they can contain extracted document text.

## Potential improvements

- Move reusable notebook code into tested Python modules and add a small command-line or web interface.
- Add deterministic chunk IDs, duplicate-safe indexing, and an explicit index rebuild command.
- Configure and test the ChromaDB distance metric and evaluate retrieval quality on a question/answer dataset.
- Add hybrid search, reranking, query rewriting, and context-aware prompt construction.
- Make answers abstain when evidence is insufficient and provide verifiable page-level citations.
- Pin dependencies and add input validation, structured logging, and robust error handling.

## License

No license has been added yet. Until one is chosen, all rights remain with the respective content owners; verify document licenses before distributing any source files or data.

## Repository structure

The committed files are at the repository root:

```text
RAG_Application/
|-- README.md
|-- document.ipynb
|-- pdf_loader.ipynb
`-- requirements.txt
```

The `data/` directory and persistent ChromaDB vector store are created locally when documents are added and indexed. They are not committed to this repository.
