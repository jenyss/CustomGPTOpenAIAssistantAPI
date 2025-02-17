# Custom GPT using OpenAI Assistant API

This Assistant processes documents and answers user queries with relevant context. It ingests PDFs and text files, breaks them into chunks, and stores them in a vector database for efficient retrieval. When a user asks a question, it searches for the most relevant document snippets and provides a response with citations.

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

3️⃣ **OpenAI Assistant Handles User Queries**
* A thread is created (```client.beta.threads.create()```).
* The user’s message is added to the thread.
* The assistant **automatically calls** ```search_docs``` **using function calling**.
* The polling loop **detects** ```requires_action``` **status**, retrieves documents, and submits tool outputs.
* The assistant receives the retrieved document snippets and **generates a response**.

## Intallation

<b>Prerequisites</b>

* Access to <b>JupyterLab, Google Colab</b>, or another interactive computing environment to run this Jupyter Notebook.

### Step 1: Clone the Repository

Clone this repository to your local machine:
```
git clone <REPOSITORY_URL>
cd <PROJECT_FOLDER>
```

### Step 2: Open Jupyter Notebook in JupyterLab

Ensure that ```<PROJECT_FOLDER>``` is accessible in JupyterLab by setting it as your working directory in JupyterLab.
 * In JupyterLab, use the "Open from Path" option to load ```CustomGPTOpenAIAssistantAPI.ipynb```.
 * Similarly, load ```.env``` and populate the variable keys with appropriate values.
 * The first cell in the Notebook installs the required libraries: **TODO**

### Step 3: Run the Jupyter Notebook

To execute the notebook, select each cell and press ```Shift + Enter```.

