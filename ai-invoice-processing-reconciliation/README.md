# AI-Powered Invoice Processing & Reconciliation System

## Overview

An automated invoice processing and purchase order (PO) reconciliation system built using n8n, AI-powered document extraction, JavaScript, Supabase, and Slack.
The workflow extracts structured information from incoming invoice PDFs, retrieves corresponding purchase order records, and automatically compares invoice details against the stored PO data.
Discrepancies, missing purchase orders, and duplicate invoices are identified and routed for review before invoice records are finalised.

## Problem Statement

Businesses process large volumes of supplier invoices that need to be checked against previously approved purchase orders.
Manual verification of supplier details, product quantities, unit prices, GST, and invoice totals is time-consuming and susceptible to human error.
Duplicate invoices and discrepancies can also result in incorrect payments and financial losses.

## Solution

Developed an automated invoice processing pipeline that combines AI-powered information extraction with deterministic JavaScript-based reconciliation.
The system extracts invoice data, retrieves the relevant purchase order from Supabase, compares individual line items and financial totals, and routes exceptions through Slack for human review.

## Workflow Architecture

### 1. Invoice Intake
- Receives incoming invoice documents through Gmail / PDF intake.
- Processes invoice documents for information extraction.
- Includes duplicate invoice detection before further processing.

### 2. AI-Powered Information Extraction
- Uses n8n's Information Extractor.
- Extracts structured invoice information into JSON.
- Identifies supplier name, invoice number, PO number, invoice date, and currency.
- Extracts individual product names, quantities, units, unit prices, and line totals.
- Extracts subtotal, GST, and grand total.
- Uses a defined output schema to maintain consistent data structure.

### 3. Purchase Order Retrieval
- Uses Supabase as the purchase order database.
- Retrieves the relevant PO using the extracted PO number.
- Fetches stored supplier details, product information, quantities, prices, and financial totals.
- Identifies missing purchase orders.

### 4. Invoice Reconciliation

A JavaScript Code node performs deterministic comparisons between the extracted invoice and the stored purchase order.

The reconciliation checks:

- Supplier name
- Product names
- Product quantities
- Units of measurement
- Unit prices
- Individual line totals
- Subtotal
- GST
- Grand total

Product names are normalised before comparison to handle differences in capitalisation and formatting.

The workflow also tracks matched PO item indexes to prevent the same purchase order item from being matched multiple times.

### 5. Exception Handling

The workflow identifies different processing outcomes:

- **Matched:** Invoice details agree with the purchase order.
- **Mismatch:** One or more invoice fields differ from the PO.
- **PO Not Found:** No corresponding purchase order is available.
- **Duplicate:** Invoice has already been received or processed.

Discrepancies are identified and made available for further review.

### 6. Human-in-the-Loop Approval

- Uses Slack's Send and Wait functionality for review.
- Routes invoices requiring attention to the relevant purchase department.
- Allows a human reviewer to inspect discrepancies and approve or reject processing.
- Invoice recording proceeds after the required approval.

## Technologies Used

- n8n – Workflow automation and orchestration
- AI Information Extractor – Structured invoice data extraction
- OpenAI – AI-powered document understanding
- JavaScript – Reconciliation and validation logic
- Supabase – Purchase order database and invoice records
- PostgreSQL – Relational data storage
- Gmail – Invoice intake
- PDF Processing – Invoice document extraction
- Slack – Notifications and human approval workflows

## Key Features

- Automated invoice intake
- AI-powered PDF information extraction
- Structured JSON output
- Duplicate invoice detection
- Purchase order lookup
- Supplier and product verification
- Line-item reconciliation
- Quantity and unit-price validation
- GST and total amount verification
- Missing PO identification
- Automated discrepancy detection
- Slack-based human approval
- Conditional invoice processing

## Reconciliation Logic

The system uses deterministic comparisons rather than relying on AI to decide whether financial values match.

Financial comparisons use a small tolerance to account for rounding differences.

The reconciliation output includes:
- Overall matching status
- Supplier matching status
- Individual item comparison results
- Subtotal, GST, and grand-total comparisons
- Unmatched purchase order items
- Details of detected discrepancies

## Setup Requirements

To reproduce this workflow, you will need:

- An n8n instance
- Gmail credentials
- AI model credentials
- A Supabase project with the required PO and invoice tables
- A Slack app with appropriate permissions
- Sample invoice PDFs and corresponding purchase order records

Import the workflow into n8n, configure the required credentials, map the database fields, and test using sample invoices and purchase orders.

## Current Scope & Limitations

- Demonstration prototype using INR-based invoice data.
- Performs invoice-to-PO reconciliation rather than full three-way matching.
- Uses normalised product-name matching; more advanced SKU and product-alias mapping can improve matching accuracy.
- Partial deliveries and partial PO fulfilment are not currently supported.
- Human review is required for exceptions.

## Disclaimer

This is a demonstration project developed for learning and portfolio purposes. All supplier, invoice, and purchase order data should be fictional or anonymised. Credentials and sensitive information are excluded from the public workflow export.
