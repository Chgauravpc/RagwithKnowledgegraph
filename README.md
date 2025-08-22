Knowledge Graph with RAG (Retrieval-Augmented Generation)
This repository contains a Jupyter notebook (KnowledgegraphwithRAG.ipynb) that demonstrates how to build a knowledge graph from textual data (e.g., Wikipedia articles) using LangChain, integrate it with Neo4j for storage and querying, and implement a Retrieval-Augmented Generation (RAG) system for question answering. The example focuses on historical figures like Elizabeth I and Mary Queen of Scots, extracting entities, relationships, and enabling hybrid search (vector + graph-based).
Features

Data Loading: 
Fetch and process Wikipedia articles using WikipediaLoader.
Text Splitting: Chunk documents for efficient processing.
Graph Construction: Use LLMGraphTransformer with Groq's Gemma2 model to extract entities and relationships.
Neo4j Integration: Store the knowledge graph in Neo4j, add embeddings, and create indexes for full-text and vector search.
Visualization: Render the graph using yfiles-jupyter-graphs.
RAG Pipeline: Build a retriever for structured (graph) and unstructured (vector) data, and chain it with an LLM for natural language QA.
Entity Extraction: Identify persons, organizations, etc., using structured output from the LLM.
Conversational QA: Handle chat history for follow-up questions.

Requirements

Python 3.10+
Jupyter Notebook or Google Colab
Neo4j AuraDB instance (free tier available) â�� replace credentials in the notebook.
API Keys: Groq API key for LLM access (replace in code).
Dependencies (install via pip):
textlangchain
langchain-community
langchain-groq
langchain-experimental
langchain-huggingface
neo4j
yfiles-jupyter-graphs
wikipedia
sentence-transformers
huggingface-hub


Install all at once:
textpip install langchain langchain-community langchain-groq langchain-experimental langchain-huggingface neo4j yfiles-jupyter-graphs wikipedia sentence-transformers huggingface-hub
Setup

Clone the Repository:
textgit clone https://github.com/Chgauravpc/knowledge-graph-rag.git
cd knowledge-graph-rag

Neo4j Setup:

Create a free Neo4j AuraDB instance at neo4j.com/cloud/aura-db.
Update the notebook with your Neo4j URI, username, and password (e.g., NEO4J_URI = "neo4j+s://your-instance.databases.neo4j.io").


API Keys:

Get a Groq API key from groq.com and replace groq_api_key in the notebook.
Hugging Face token if needed for embeddings (optional, as it uses public models).


Run the Notebook:

Open KnowledgegraphwithRAG.ipynb in Jupyter or Colab.
Execute cells sequentially.
The graph visualization will appear after running showGraph().



Usage

Load Data: The notebook loads Wikipedia pages for "Elizabeth I" and "Mary Queen of Scots". Modify query in WikipediaLoader for other topics.
Build Graph: Convert documents to graph entities/relationships and add to Neo4j.
Query the System:

Use the chain.invoke() function for QA, e.g.:
textchain.invoke({"question": "Which house did Elizabeth I belong to?"})

Supports conversational context with chat history:
textchain.invoke({
    "question": "Tell me more about her reign.",
    "chat_history": [("Which house did Elizabeth I belong to?", "House of Tudor")]
})



Customization:

Change LLM model (e.g., to other Groq models).
Adjust chunk sizes or embedding models for better performance.



Example Outputs

Graph Visualization: Displays nodes (e.g., "Elizabeth I") and edges (e.g., "PARENT" to "Henry VIII").
QA Example:

Input: "Who was Elizabeth I?"
Output: A concise summary based on retrieved graph and text data.



Limitations

Requires internet for Wikipedia loading and API calls.
Neo4j free tier has limits on database size â�� suitable for small demos.
Entity extraction relies on LLM accuracy; fine-tune prompts for better results.

Contributing
Feel free to open issues or PRs for improvements, such as adding more data sources or optimizing the graph schema.
License
