# AI-Powered Company Knowledge Assistant (RAG)

## Overview

An AI-powered internal knowledge assistant built using n8n, OpenAI, Supabase, and Slack. The assistant allows employees to ask questions about company policies, product information, technical documentation, and internal procedures directly through Slack.
It uses Retrieval-Augmented Generation (RAG) to retrieve relevant information from company documents before generating responses.

## Problem Statement

Employees often need to search through multiple documents to find information about company policies, product pricing, technical troubleshooting, onboarding procedures, and HR guidelines.
Manually searching these documents can be time-consuming and inefficient.

## Solution

Built a Slack-integrated AI knowledge assistant that retrieves relevant information from a centralised company knowledge base and uses it to answer employee questions.
The system uses vector embeddings and semantic search to find relevant document content rather than relying only on keyword matching.

## Knowledge Base

The demonstration knowledge base for NexaFlow Technologies includes:

- Product pricing and information
- Technical troubleshooting documentation
- Employee onboarding SOPs
- Company security guidelines
- HR policies

## Workflow Architecture

### 1. Document Ingestion

- Retrieves the company knowledge document from Google Drive.
- Loads the document using n8n's Default Data Loader.
- Converts document content into embeddings using OpenAI.
- Stores the embeddings in Supabase using pgvector.

### 2. Question Answering

- Listens for Slack app mentions.
- Passes employee questions to an AI Agent.
- Uses GPT-4o-mini to process questions and generate responses.
- Retrieves relevant context from Supabase through a vector search tool.
- Uses Postgres chat memory to maintain conversational context.
- Sends the generated response back to the Slack channel.

## Technologies Used

- n8n – Workflow orchestration
- OpenAI GPT-4o-mini – AI language model
- OpenAI Embeddings – Text vectorisation
- Supabase – Vector database
- PostgreSQL / pgvector – Semantic similarity search
- Google Drive – Knowledge document storage
- Slack API – Employee interaction interface
- Retrieval-Augmented Generation (RAG) – Context-based AI responses

## Key Features

- Slack-based conversational interface
- Document-based question answering
- Semantic search using vector embeddings
- Centralised knowledge retrieval
- Conversational memory
- Responses grounded in the company knowledge base
- Avoids answering questions outside the available knowledge base

## Setup Requirements

To reproduce this workflow, you will need:

- An n8n instance
- OpenAI API credentials
- Supabase project with pgvector enabled
- Google Drive credentials
- Slack app with appropriate permissions and event subscriptions

Configure the required credentials and environment-specific settings in n8n before executing the workflows.

## Disclaimer

NexaFlow Technologies is a fictional demonstration company created for this project. The knowledge base contains demonstration information and does not represent a real company's internal documentation.
