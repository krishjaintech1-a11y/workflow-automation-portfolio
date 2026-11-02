# AI-Powered Lead Intake & Validation Automation

## Overview

An AI-powered lead intake and data processing workflow built using n8n, OpenAI, and Supabase.
The workflow receives customer enquiries through a webhook, extracts structured lead information using an AI agent, validates the submitted data, and stores customer and enquiry records in a Supabase database.

## Problem Statement

Businesses often receive customer enquiries containing unstructured product requirements, contact details, and budget information.
Manually extracting this information, checking its accuracy, and maintaining customer records can be time-consuming and prone to errors.

## Solution

Developed an automated lead processing pipeline that uses AI for information extraction and JavaScript for deterministic data validation.
Valid submissions are processed and stored in Supabase, while incomplete or invalid submissions receive an error response identifying the information that needs correction.

## Workflow Architecture

### 1. Webhook Lead Capture
- Receives POST requests through an n8n webhook.
- Accepts customer enquiry data from external forms or applications.

### 2. AI-Powered Data Extraction
- Uses an OpenAI GPT-4o-mini model.
- Extracts customer name, email, company, phone number, products, quantities, and budget.
- Uses a Structured Output Parser to enforce a defined JSON schema.
- Handles missing information using actual JSON null values instead of invented data.

### 3. Data Validation
A JavaScript Code node validates:
- Required customer name and company.
- Email format.
- Phone number format.
- Product names and quantities.
- Positive numerical budget values.

The workflow generates a validation status and a list of errors for invalid submissions.

### 4. Conditional Processing
- Invalid submissions receive an HTTP 400 response with validation errors.
- Valid submissions proceed to customer lookup in Supabase.
- Existing customers are associated with new enquiry records.
- New customers are inserted into the customer table before their enquiry is recorded.

### 5. Database Management
Uses Supabase to maintain two sets of records:

- **Customer records:** Email, mobile number, and company name.
- **Enquiry records:** Customer ID, requested products, quantities, and budget.

Customer lookup is performed using the submitted email address.

## Technologies Used

- n8n – Workflow automation and orchestration
- OpenAI GPT-4o-mini – AI-powered information extraction
- Structured Output Parser – Structured JSON generation
- JavaScript – Data validation and conditional logic
- Supabase – Database and customer record management
- PostgreSQL – Relational data storage
- Webhooks – HTTP-based data ingestion

## Key Features

- Webhook-based lead capture
- AI-powered extraction of unstructured enquiries
- Structured JSON output
- Null-value handling
- Email and phone validation
- Product and quantity validation
- Conditional error handling
- Existing customer identification
- Automated customer and enquiry record creation
- Centralised data storage in Supabase

## Setup Requirements

To reproduce this workflow, you will need:

- An n8n instance
- OpenAI API credentials
- A Supabase project
- Customer and Enquiries tables configured in Supabase
- A configured webhook endpoint

Import the workflow JSON into n8n, configure your credentials, verify the Supabase table and column names, and test using sample webhook requests.
