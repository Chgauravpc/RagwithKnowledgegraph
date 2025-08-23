# Knowledge Graph with RAG

This Jupyter Notebook (`KnowledgegraphwithRAG.ipynb`) demonstrates how to build a simple knowledge graph using LangChain, Neo4j, and Wikipedia data. It loads information about "Elizabeth I" from Wikipedia, processes it, and integrates it into a Neo4j graph database. This setup can be used as a foundation for Retrieval-Augmented Generation (RAG) applications, where the graph serves as a structured knowledge base for querying and enhancing AI responses.

## Overview

The notebook covers:
- Installing required packages (LangChain ecosystem, Tiktoken, Neo4j, Wikipedia loader).
- Setting up connections to Neo4j and Groq API.
- Loading and processing Wikipedia documents about "Elizabeth I".
- Creating a Neo4j graph instance for storing entities and relationships.

## Prerequisites

- **Python**: Version 3.12 or higher.
- **Jupyter Notebook**: Install via `pip install notebook` if needed.
- **Neo4j Database**: A running Neo4j instance (e.g., Neo4j Aura free tier or local setup). You will need:
  - `NEO4J_URI` (e.g., `bolt://localhost:7687`)
  - `NEO4J_USERNAME` (e.g., `neo4j`)
  - `NEO4J_PASSWORD`
- **Groq API Key**: Required for LangChain integrations with Grok models (sign up at [groq.com](https://groq.com)).
- Internet access for package installations and Wikipedia API calls.

## Installation

To set up the environment, run the following commands in your terminal or execute the installation cells in the notebook:

```bash
# Install core LangChain packages
pip install --upgrade --quiet langchain langchain-community langchain-groq langchain-experimental

# Install Tiktoken for tokenization
pip install --upgrade --quiet tiktoken

# Install Neo4j integration packages
pip install --upgrade langchain-neo4j neo4j

# Install Wikipedia loader
pip install --upgrade wikipedia
```

**Note**: The notebook output indicates potential dependency conflicts (e.g., `tenacity` or `fsspec`). If errors occur, consider resolving them by aligning package versions (e.g., `pip install tenacity==8.5.0 fsspec==2025.3.0`).

## Usage

1. **Clone or Download the Notebook**:
   - Save `KnowledgegraphwithRAG.ipynb` to your local machine.

2. **Configure Credentials**:
   - Open the notebook in Jupyter and update the following placeholders with your credentials:
     ```python
     NEO4J_URI = "your_neo4j_uri"  
     NEO4J_USERNAME = "your_username"  
     NEO4J_PASSWORD = "your_password"
     groq_api_key = "your_groq_api_key"
     ```

3. **Run the Notebook**:
   - Start Jupyter: `jupyter notebook`
   - Open `KnowledgegraphwithRAG.ipynb`.
   - Execute cells sequentially (Shift + Enter).
   - The notebook will:
     - Install required packages.
     - Establish a connection to your Neo4j database.
     - Load Wikipedia data for the query "Elizabeth I".
     - Initialize a Neo4j graph instance.
     - (Note: The provided notebook snippet is truncated; additional code is needed for full graph population and RAG queries.)

4. **Expected Output**:
   - Installation logs for packages.
   - Loaded Wikipedia documents, including summaries and content about Elizabeth I and related entities (e.g., Elizabeth of Russia, House of Tudor).
   - A connected Neo4j graph instance ready for further entity extraction and relationship mapping.

## Extending the Notebook

To make the notebook fully functional for a knowledge graph with RAG, consider the following enhancements:

- **Entity and Relationship Extraction**:
  - Use LangChain's graph transformers or an LLM chain to extract entities (e.g., people, places, events) and relationships from the Wikipedia documents.
  - Store these in the Neo4j graph using Cypher queries. For example:
    ```python
    from langchain_experimental.graph_transformers import LLMGraphTransformer
    llm_transformer = LLMGraphTransformer(llm=llm)
    graph_documents = llm_transformer.convert_to_graph_documents(raw_documents)
    graph.add_graph_documents(graph_documents)
    ```

- **RAG Integration**:
  - Implement a retrieval mechanism to query the Neo4j graph and augment prompts for a Grok model.
  - Example query to retrieve relationships:
    ```python
    query = "MATCH (p:Person)-[r]->(n) WHERE p.name = 'Elizabeth I' RETURN p, r, n"
    results = graph.query(query)
    ```

- **Visualization**:
  - Use libraries like `yfiles-jupyter-graphs` (as seen in the notebook) to visualize the knowledge graph.
  - Extend the visualization to include more nodes and relationships as the graph grows.

- **Query Expansion**:
  - Add more Wikipedia queries or external data sources to enrich the knowledge graph.
  - Example: Load additional historical figures or events related to the Elizabethan era.

## Notes

- **Deprecation Warning**: The notebook uses `langchain_community.graphs.Neo4jGraph`, which is deprecated in LangChain 0.3.8. Update to `langchain_neo4j.Neo4jGraph`:
  ```python
  from langchain_neo4j import Neo4jGraph
  ```
- **Wikipedia Parser Warning**: The Wikipedia loader may raise a `GuessedAtParserWarning`. To resolve, explicitly specify the parser:
  ```python
  from langchain.document_loaders import WikipediaLoader
  raw_documents = WikipediaLoader(query="Elizabeth I", features="lxml").load()
  ```
- **Truncated Notebook**: The provided notebook snippet ends prematurely. To complete the workflow, add cells for processing documents, extracting graph data, and querying the graph for RAG purposes.

## Troubleshooting

- **Dependency Conflicts**: If you encounter errors related to `tenacity` or `fsspec`, pin compatible versions as noted above.
- **Neo4j Connection Issues**: Ensure your Neo4j instance is running and credentials are correct. Test connectivity with:
  ```python
  graph.query("MATCH (n) RETURN n LIMIT 1")
  ```
- **API Key Errors**: Verify your Groq API key is valid and has sufficient quota. 
- **Wikipedia API Limits**: If Wikipedia requests fail, reduce the query load or add delays between requests.

## License

This project is for educational purposes and does not include a specific license. Ensure compliance with the terms of use for Wikipedia, Neo4j, and Groq APIs when deploying or sharing.
