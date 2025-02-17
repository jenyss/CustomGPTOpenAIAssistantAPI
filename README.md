# Custom GPT using OpenAI Assistant API

## How it works

1️⃣ **Document Ingestion & Embedding (One-Time Process)**
* Reads **PDF and text** files (```extract_text_from_pdf```, ```load_text_file```).
* Splits them into **manageable chunks** using ```RecursiveCharacterTextSplitter```.
* Converts them into ```Document``` objects with **metadata (source, page number)**.
* Generates **unique IDs for each chunk**.
* Stores the embedded document chunks in ChromaDB.<br><br>

2️⃣ **Query Processing (Every Time a User Asks a Question)**
* When a user asks something, the ```search_docs(query)``` function:
* Re-initializes the **Chroma vector store**. ❗❗❗ This is done because I am testing the embedding of different types of documents and also the chunking and I need to start with a clean database. It will be removed for the final version.
* Converts the query into an embedding.
* Performs similarity search.
* Retrieves **top-k most relevant document chunks**.
* Formats the response, including:
  * Relevant text snippets.
  * Citations with source and page number.<br><br>

3️⃣ OpenAI Assistant Handles User Queries
A thread is created (client.beta.threads.create()).
The user’s message is added to the thread.
The assistant automatically calls search_docs using function calling.
Your polling loop detects requires_action status, retrieves documents, and submits tool outputs.
The assistant receives the retrieved document snippets and generates a response.
