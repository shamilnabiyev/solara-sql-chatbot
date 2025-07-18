# Solara SQL Chatbot 

## Table of Contents

- [Introduction](#introduction)
- [Install dependencies](#install-dependencies)
- [Environment variables](#environment-variables)
- [Start Docker containers](#start-docker-containers)
- [Create a test database](#create-a-test-database)
- [Run Vanna SQL Agent training](#run-vanna-sql-agent-training)
- [Start the Solara SQL Chatbot](#start-the-solara-sql-chatbot)

## Introduction

<div align="center">
    <img src="docs/imgs/chatbot-gui.png" alt="Chatbot GUI" width="500"/>
</div>

<div align="center">
    <span><i>Fig.1: Solara Chatbot GUI</i></span>
</div>

<br>

This project is a proof-of-concept demonstrating how to create a simple SQL Chatbot using Solara for GUI development, FastAPI as the backend framework, Azure OpenAI API as the language model, Qdrant Vector Database for managing embeddings, PostgreSQL for data storage, and Vanna AI SQL Agent for generating SQL queries from natural language prompts.

<div align="center">
    <img src="docs/imgs/components-diagram.png" alt="Chatbot GUI" width="700"/>
</div>

<div align="center">
    <span><i>Fig.2: Components Diagram</i></span>
</div>




## Install dependencies

Create a new python virtual environment (venv):

```bash
python -m venv .venv
```

Activate the venv:

```bash
# On Linux
source .venv/bin/activate
```

```bash
# On Windows using CMD
.venv\Scripts\activate
```

```bash
# On Windows using Git Bash
source .venv/Scripts/activate
```

Install python dependencies:

```bash
pip install -r requirements.txt
```

## Environment variables

Create a `.env` file and add the environment variables:

```ini
# PostgreSQL
POSTGRES_USER='postgres'
POSTGRES_PASSWORD='postgres'
POSTGRES_DB='sales_db'
POSTGRES_HOST='localhost'
POSTGRES_PORT='5432'

# Qdrant
QDRANT_API_URL='http://localhost:6333'
QDRANT__SERVICE__API_KEY='api-key-readwrite'
QDRANT__SERVICE__READ_ONLY_API_KEY='api-key-readonly'

# Azure OpenAI
AZURE_OPENAI_MODEL_DEPLOYMENT='model-deployment-name'
AZURE_OPENAI_ENDPOINT='openai-endpoint'
AZURE_OPENAI_API_VERSION='openai-api-version'
```

## Start Docker containers

Start `postgresql` and `qdrant` containers, and create `sales_db` PostgreSQL database:

```bash
docker-compose --env-file .env up -d
```

## Create a test database

The python script `utils/data_gen.py` will create `customer`and `purchase` tables in the `sales_db` database, and finally fill the tables with random data.

```bash
python utils/data_gen.py
```

## Run Vanna SQL Agent training

Vanna SQL Agent should be trained only once:

```bash
python utils/vanna_train.py
```

## Start the Solara SQL Chatbot

Embed the Solara GUI into a FastAPI app:

```bash
SOLARA_APP=gui/sol.py uvicorn app:app
```

The app will be available at `http://localhost:8000/solara/`
