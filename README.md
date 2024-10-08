﻿# Project: AI-Powered Review Query System

## Overview

This project aims to develop a chatbot system for a company that stores and manages customer reviews. The system allows users to ask both simple and complex questions regarding reviews in natural language, which are then converted into SQL queries to retrieve relevant insights from the database.

The chatbot leverages a large language model (LLM), such as Google PaLM 2 integrated within the Langchain framework, to translate natural language questions into SQL queries. For more complex queries, we will implement a spatial handling technique with few-shot learning to enhance the accuracy of SQL generation. This system will make querying the reviews database fast and easy for managers, reducing reliance on data analysts.

## Purpose
The goal is to provide a seamless way for company stakeholders, including managers and analysts, to query business reviews without needing technical expertise. This system ensures that managers can ask natural-language questions such as:

"How many reviews do we have with 5-star ratings?"
"What are the top categories with the most reviews?"
"Which location has the most positive reviews?"
The chatbot will convert these queries into SQL, access the relevant tables (business_categories, yelp_reviews_with_business), and return the results.

## Architecture

### Simple Queries

1. **Input**: User submits a natural language question.
2. **LLM (Google PaLM 2 within Langchain)**: The question is passed to the PaLM 2 model, which generates the corresponding SQL query.
3. **SQL Query Execution**: The SQL query is executed against the MySQL database containing business categories and Yelp reviews.
4. **Response**: The relevant answer is returned to the user.

### Complex Queries

For queries where the out-of-the-box (OOB) performance of the LLM struggles, we implement a custom solution using spatial handling and few-shot learning.

1. **Identify Failed Queries**: Queries where the LLM fails to generate the correct SQL query are identified.
2. **Few-Shot Learning with Training Dataset**: We create a training dataset of sample questions and their corresponding SQL queries.
3. **Embedding Vectors (Hugging Face)**: Convert both the questions and SQL queries into embedding vectors using a model from Hugging Face.
4. **Vector Database (ChromaDB)**: Store these embeddings in ChromaDB (an open-source vector database) for fast similarity matching.
5. **Enhanced Query Generation**: For complex queries, the system checks the vector database and generates SQL queries using a few-shot prompt template to improve accuracy. This works by providing the LLM with examples from the training dataset, enhancing the response.
6. **Response**: The correct SQL query is executed, and the results are returned to the user.

### User Interface

- **Streamlit UI**: A web-based interface built using Streamlit allows users to interact with the chatbot. The interface supports both simple and complex query handling, providing an easy way to retrieve business review insights.

## Database Structure

### Tables:

1. **business_categories**

   - `business_id` (VARCHAR): Unique identifier for each business.
   - `category` (VARCHAR): Categories associated with the business (e.g., "Restaurants", "Doctors").

2. **yelp_reviews_with_business**
   - `business_id` (VARCHAR): Foreign key linking to `business_categories`.
   - `name` (VARCHAR): Name of the business.
   - `address` (VARCHAR): Business address.
   - `city` (VARCHAR): City where the business is located.
   - `state` (VARCHAR): State where the business is located.
   - `postal_code` (VARCHAR): Postal code for the business.
   - `latitude` (DECIMAL): Latitude for geolocation.
   - `longitude` (DECIMAL): Longitude for geolocation.
   - `stars` (DECIMAL): Star rating for the business.
   - `review_count` (INT): Number of reviews for the business.
   - `categories` (TEXT): Categories of the business in a comma-separated format.
   - `hours` (TEXT): Operating hours of the business.
   - `review_id` (VARCHAR): Unique identifier for each review.
   - `user_id` (VARCHAR): User who submitted the review.
   - `review_stars` (DECIMAL): Star rating for the specific review.
   - `useful` (INT): Number of "useful" votes the review received.
   - `funny` (INT): Number of "funny" votes the review received.
   - `cool` (INT): Number of "cool" votes the review received.
   - `text` (TEXT): The content of the review.
   - `date` (DATETIME): Date and time the review was submitted.

## Technology Stack

- **LLM**: Google PaLM 2 (Langchain Framework)
- **Few-Shot Learning**: Using Hugging Face for embedding generation
- **Vector Database**: ChromaDB (open-source)
- **Database**: MySQL (stores business categories and Yelp reviews)
- **Frontend**: Streamlit (UI for users to interact with the system)
- **Backend**: API layer for handling queries and communicating with the database

## Conclusion

This project creates an intelligent chatbot capable of answering review-related questions by generating SQL queries from natural language input. It helps managers and analysts access insights quickly without technical expertise. The system can handle both simple and complex queries, enhancing productivity and efficiency in querying review data.
