
# News Research Tool 

This is a user-friendly news research tool designed for effortless information retrieval. Users can input article URLs and ask questions to receive relevant insights from the stock market and financial domain.

## Need for this tool
Copy and pasting article in ChatGPT is tedious
Need for an aggregate knowledge base: One might not know which article to pull answers from and there might even be need for multiple articles
ChatGPT has a 3000 word limit

We need to use semantic search to find relevant chunks of data in the articles provided, because there are token limits on OpenAI of 4097. To do this we do embeddings (allows to figure out relevant chunk) and vector databases (for faster checks). We merge split chunks to make them closer to the token limit (for efficiency) and then allow overlap between chunks to provide context (one chunk might need info frmo the previous chunk to make sense). Langchain has a TextSplitter
API (RecursiveCharacterTextSplitter) that does this for you. It works by splitting on one character, adn if a chunk is too big (bigger than specified), it tries the other characters you give it to split the chunk, and if it's too small, merges chunks.

On the chunks, we convert them to vectors using Huggingface sentence transformer. We then use FAISS Index to do fast similarity search with the vectors. Since the chunks combined size might be greater than the token limit, we use the MapReduce method, as opposed to the stuff method. We do 5 LLM calls (imagine we have 4 chunks) - 4 for each chunk to see the answer based on each individual chunk, then you combine the answers of the for LLM calls to one summary, and one final LLM call on the summary chunk to get the final answer

## Features

- Load URLs or upload text files containing URLs to fetch article content.
- Process article content through LangChain's UnstructuredURL Loader
- Construct an embedding vector using OpenAI's embeddings and leverage FAISS, a powerful similarity search library, to enable swift and effective retrieval of relevant information
- Interact with the LLM's (ChatGPT) by inputting queries and receiving answers along with source URLs.


## Installation

1.Clone this repository to your local machine using:

```bash
  git clone https://github.com/codebasics/langchain.git
```
2.Navigate to the project directory:

```bash
  cd 2_news_research_tool_project
```
3. Install the required dependencies using pip:

```bash
  pip install -r requirements.txt
```
4.Set up your OpenAI API key by creating a .env file in the project root and adding your API

```bash
  OPENAI_API_KEY=your_api_key_here
```
## Usage/Examples

1. Run the Streamlit app by executing:
```bash
streamlit run main.py

```

2.The web app will open in your browser.

- On the sidebar, you can input URLs directly.

- Initiate the data loading and processing by clicking "Process URLs."

- Observe the system as it performs text splitting, generates embedding vectors, and efficiently indexes them using FAISS.

- The embeddings will be stored and indexed using FAISS, enhancing retrieval speed.

- The FAISS index will be saved in a local file path in pickle format for future use.
- One can now ask a question and get the answer based on those news articles


## Project Structure

- main.py: The main Streamlit application script.
- requirements.txt: A list of required Python packages for the project.
- faiss_store_openai.pkl: A pickle file to store the FAISS index.
- .env: Configuration file for storing your OpenAI API key.
