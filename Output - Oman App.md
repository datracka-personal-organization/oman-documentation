# Oman App - Business and Functional Requirements Document

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Introduction](#2-introduction)
    - 2.1 [Purpose](#21-purpose)
    - 2.2 [Scope](#22-scope)
    - 2.3 [Definitions, Acronyms, and Abbreviations](#23-definitions-acronyms-and-abbreviations)
3. [Business Objectives](#3-business-objectives)
4. [Functional Requirements](#4-functional-requirements)
    - 4.1 [Authentication Module](#41-authentication-module)
    - 4.2 [Campaign Management Module](#42-campaign-management-module)
    - 4.3 [Leads Management Module](#43-leads-management-module)
    - 4.4 [Settings Module](#44-settings-module)
5. [Non-Functional Requirements](#5-non-functional-requirements)
    - 5.1 [Usability](#51-usability)
    - 5.2 [Reliability](#52-reliability)
    - 5.3 [Performance](#53-performance)
    - 5.4 [Supportability](#54-supportability)
    - 5.5 [Security](#55-security)
6. [Technical Architecture](#6-technical-architecture)
    - 6.1 [System Overview](#61-system-overview)
    - 6.2 [Microservices Architecture](#62-microservices-architecture)
7. [Data Model](#7-data-model)
8. [API Endpoints](#8-api-endpoints)
9. [User Interface Design](#9-user-interface-design)
10. [Use Cases](#10-use-cases)
11. [Constraints and Assumptions](#11-constraints-and-assumptions)
12. [Appendices](#12-appendices)

---

## 1. Executive Summary

The **Oman App** is a comprehensive marketing tool designed to streamline the entire process for marketers—from initial prospect research to executing targeted email campaigns aimed at potential deals. The app enables users to create and manage campaigns, crawl LinkedIn for prospects, enrich lead data, and send customized email campaigns. This document outlines the business and functional requirements for the Oman App, focusing on its core modules, technical architecture, data model, and interfaces.

---

## 2. Introduction

### 2.1 Purpose

The purpose of this document is to provide a detailed description of the Oman App's business and functional requirements. It serves as a guide for developers, stakeholders, and team members involved in the project to understand the system's functionalities, architecture, and design.

### 2.2 Scope

This document covers the Oman App, which includes the following components:

- **Oman Server App**: A RESTful API backend.
- **Oman Client App**: A web application frontend.

**Note**: The Oman App relies on external services for crawling LinkedIn, finding and verifying emails, AI categorization, and sending campaigns. These services are not covered in this document.

### 2.3 Definitions, Acronyms, and Abbreviations

- **API**: Application Programming Interface
- **CRUD**: Create, Read, Update, Delete
- **UI/UX**: User Interface/User Experience
- **BE**: Backend
- **FE**: Frontend
- **UUID**: Universally Unique Identifier

---

## 3. Business Objectives

The Oman App aims to provide marketers with an integrated platform that simplifies the process of prospecting and executing email campaigns. By automating tasks such as lead crawling, email finding, and AI-based categorization, the app reduces manual effort and increases efficiency.

**Key Business Objectives:**

- **Streamline Campaign Management**: Simplify the creation and management of marketing campaigns.
- **Automate Lead Generation**: Utilize LinkedIn crawling to automate prospect research.
- **Enhance Lead Data**: Enrich leads with verified emails and AI-based categorization.
- **Seamless Integration**: Integrate with email marketing platforms like Zoho Campaigns for effortless campaign execution.
- **User-Friendly Interface**: Provide an intuitive and efficient user interface for managing campaigns and leads.

---

## 4. Functional Requirements

### 4.1 Authentication Module

#### Description

The Authentication Module manages user registration, login, and password recovery, ensuring secure access to the Oman App.

#### Features

- **User Registration:**
    
    - Collect first name, last name, email, and password.
    - Allow third-party sign-up using providers like Google and Apple.
    - All fields are mandatory.
    - Require email verification before accessing the dashboard.
- **User Login:**
    
    - Login using email/password or third-party providers.
    - Synchronize data if a user registers via email and later logs in using a third-party provider with the same email.
    
- **Password Recovery:**
    
    - Allow users to request a password reset link via email.
    - Provide a form to set a new password.

#### Technical Considerations

- Implement server-side and client-side validation.
- Use JSON for successful responses (HTTP 200) and standard HTTP error codes for errors.
- Utilize Zoho Zepto transaction platform for sending emails.
- Implement WebSocket technology to notify the frontend upon email confirmation.

### 4.2 Campaign Management Module

#### Description

Allows users to create, manage, and execute marketing campaigns, including crawling LinkedIn for leads and sending email campaigns.

#### Features

- **Campaign CRUD Operations:**
    
    - Create, view, edit, and delete campaigns.
    - Implement soft delete to retain campaign data in the database.
- **Campaign Details:**
    
    - **Campaign Name**
    - **Email Subject**: Subject line for emails sent to leads.
    - **Email Body**: Content of the email.
    - **Crawling Status**: Indicates the status of crawling (queued, processing, finished).
    - **Campaign Type**: LinkedIn or Sales Navigator.
    - **Search URL**: URL used for crawling leads.
- **Actions:**
    
    - **Trigger Crawling**: Enabled when the Search URL is provided.
    - **Send Campaign**: Enabled when crawling is finished.
    - **Send as Test**: Sends emails to a specified email address for testing.

### 4.3 Leads Management Module

#### Description

Manages the leads generated from campaigns, allowing users to view, edit, and organize lead information.

#### Features

- **Lead Details:**
    
    - **First Name**
    - **Last Name**
    - **Email**
    - **Company**
    - **Company Description**
    
- **Lead Operations:**
    
    - View leads associated with a campaign.
    - Edit and delete leads directly from the list view.
    - Add new leads to a campaign.
    
- **Data Refresh:**
    
    - Replace existing leads when re-crawling a campaign.
    - Prompt user confirmation before overwriting leads.
    - Soft delete leads by setting a status flag (`is_deleted`).

### 4.4 Settings Module

#### Description

Allows users to configure essential parameters required for crawling and campaign execution.

#### Features

- **Configuration Parameters:**
    
    - **LinkedIn Cookie**: Required for crawling LinkedIn.
    - **DropContact API Key**: For email enrichment.
    - **Zoho Campaigns API Key**: For campaign execution.
    - **Email Bounce Address**
    - **Email Domain**
    - **Email Sender**
- **Validation:**
    
    - All settings fields are mandatory.
    - Users cannot access Campaigns or Leads modules without completing the settings.
    - Display a notification prompting users to complete settings if incomplete.

---

## 5. Non-Functional Requirements

### 5.1 Usability

- Provide an intuitive and user-friendly interface.
- Ensure responsive design for various devices and screen sizes.
- Maintain consistent UI/UX across all modules.

### 5.2 Reliability

- Ensure high availability and minimal downtime.
- Implement robust error handling and logging mechanisms.
- Provide real-time feedback to users on the status of operations.

### 5.3 Performance

- Optimize API responses for minimal latency.
- Efficiently handle bulk data operations, such as bulk lead insertion.

### 5.4 Supportability

- Use a modular architecture for ease of maintenance and updates.
- Provide comprehensive documentation for developers and users.

### 5.5 Security

- Secure user authentication and authorization processes.
- Protect sensitive data, including user credentials and API keys.
- Comply with data protection regulations (e.g., GDPR).

---

## 6. Technical Architecture

### 6.1 System Overview

The Oman App is designed with a microservices architecture, separating concerns across different services to enhance scalability and maintainability.

### 6.2 Microservices Architecture

- **Frontend Services:**
    
    - **Authentication Frontend Microservice**
    - **Campaign Management Frontend Microservice**
    - **Leads Management Frontend Microservice**
    - **Settings Frontend Microservice**
- **Backend Services:**
    
    - **Authentication Backend Microservice (API)**
    - **Campaign Management Backend Microservice (API)**
    - **Leads Management Backend Microservice (API)**
    - **Settings Backend Microservice (API)**
- **Database:**
    
    - Shared **PostgreSQL 16** database acting as the source of truth.

**Technologies Used:**

- **Frontend:**
    
    - **React 19** with **TypeScript**
    - **Material UI 6.1.1**
- **Backend:**
    
    - **Java** with **Spring Boot 3.3.4**
    - **OpenAPI 3.1** specification for API design
- **Communication:**
    
    - **RESTful APIs** using JSON
    - **WebSocket** for real-time notifications (e.g., email confirmation)
- **Deployment:**
    
    - **Docker** and **Kubernetes** for containerization and orchestration

**Logging:**

- Implement a logging system to track user activities, including login events and triggered actions.

---

## 7. Data Model

The data model defines the structure of the data stored in the PostgreSQL database.

### Key Entities

- **User**
    
    - `id` (UUID)
    - `first_name`
    - `last_name`
    - `email` (unique)
    - `password_hash`
    - `email_confirmed` (boolean)
    - `created_at`
    - `updated_at`
- **Campaign**
    
    - `id` (UUID)
    - `user_id` (foreign key to User)
    - `name`
    - `email_subject`
    - `email_body`
    - `crawling_status` (enum: queued, processing, finished)
    - `campaign_type` (enum: LinkedIn, Sales Navigator)
    - `search_url`
    - `is_deleted` (boolean)
    - `created_at`
    - `updated_at`
- **Lead**
    
    - `id` (UUID)
    - `campaign_id` (foreign key to Campaign)
    - `first_name`
    - `last_name`
    - `email`
    - `company`
    - `company_description`
    - `is_deleted` (boolean)
    - `created_at`
    - `updated_at`
- **Settings**
    
    - `id` (UUID)
    - `user_id` (foreign key to User)
    - `linkedin_cookie`
    - `dropcontact_api_key`
    - `zoho_campaigns_api_key`
    - `email_bounce_address`
    - `email_domain`
    - `email_sender`
    - `created_at`
    - `updated_at`
- **ActivityLog**
    
    - `id` (UUID)
    - `user_id` (foreign key to User)
    - `action` (e.g., login, create_campaign)
    - `timestamp`
    - `details` (JSON or text)

---

## 8. API Endpoints

APIs will follow the **OpenAPI 3.1.0** specification.

### Authentication API

- **POST** `/api/auth/register`
    
    - **Description**: Register a new user.
    - **Request Body**: `{ first_name, last_name, email, password }`
    - **Response**: Success message.
- **POST** `/api/auth/login`
    
    - **Description**: User login.
    - **Request Body**: `{ email, password }`
    - **Response**: Authentication token.
- **POST** `/api/auth/logout`
    
    - **Description**: User logout.
    - **Response**: Success message.
- **POST** `/api/auth/password-reset-request`
    
    - **Description**: Request password reset email.
    - **Request Body**: `{ email }`
    - **Response**: Success message.
- **POST** `/api/auth/password-reset`
    
    - **Description**: Reset password using token.
    - **Request Body**: `{ token, new_password }`
    - **Response**: Success message.
- **POST** `/api/auth/confirm-email`
    
    - **Description**: Confirm email address.
    - **Request Body**: `{ token }`
    - **Response**: Success message.

### Campaign Management API

- **GET** `/api/campaigns`
    
    - **Description**: Get list of campaigns for the authenticated user.
    - **Response**: List of campaigns.
- **POST** `/api/campaigns`
    
    - **Description**: Create a new campaign.
    - **Request Body**: Campaign details.
    - **Response**: Created campaign.
- **GET** `/api/campaigns/{campaignId}`
    
    - **Description**: Get details of a specific campaign.
    - **Response**: Campaign details.
- **PUT** `/api/campaigns/{campaignId}`
    
    - **Description**: Update a campaign.
    - **Request Body**: Updated campaign details.
    - **Response**: Updated campaign.
- **DELETE** `/api/campaigns/{campaignId}`
    
    - **Description**: Delete a campaign (soft delete).
    - **Response**: Success message.
- **POST** `/api/campaigns/{campaignId}/crawl`
    
    - **Description**: Trigger crawling for a campaign.
    - **Response**: Success message.
- **POST** `/api/campaigns/{campaignId}/send`
    
    - **Description**: Send campaign emails.
    - **Response**: Success message.
- **POST** `/api/campaigns/{campaignId}/send-test`
    
    - **Description**: Send test emails to specified address.
    - **Request Body**: `{ test_email }`
    - **Response**: Success message.

### Leads Management API

- **GET** `/api/campaigns/{campaignId}/leads`
    
    - **Description**: Get leads for a campaign.
    - **Response**: List of leads.
- **POST** `/api/campaigns/{campaignId}/leads`
    
    - **Description**: Add a new lead to a campaign.
    - **Request Body**: Lead details.
    - **Response**: Created lead.
- **PUT** `/api/leads/{leadId}`
    
    - **Description**: Update a lead.
    - **Request Body**: Updated lead details.
    - **Response**: Updated lead.
- **DELETE** `/api/leads/{leadId}`
    
    - **Description**: Delete a lead (soft delete).
    - **Response**: Success message.

### Settings API

- **GET** `/api/settings`
    
    - **Description**: Get user settings.
    - **Response**: Settings details.
- **PUT** `/api/settings`
    
    - **Description**: Update user settings.
    - **Request Body**: Updated settings.
    - **Response**: Success message.

---

## 9. User Interface Design

### Authentication Screens

#### Login Page

- **Fields:**
    - Email (mandatory, email format)
    - Password (mandatory)
- **Options:**
    - Third-party login (Google, Apple)
- **Links:**
    - Sign Up
    - Forgot Password

#### Sign Up Page

- **Fields:**
    - First Name (mandatory)
    - Last Name (mandatory)
    - Email (mandatory, email format)
    - Password (mandatory, min 6 characters)
- **Options:**
    - Third-party sign-up (Google, Apple)
- **Links:**
    - Sign In

#### Email Confirmation Page

- **Message:** Prompt to check inbox for confirmation email.
- **Real-Time Update:** Use WebSocket to notify when email is confirmed.

#### Password Recovery Pages

- **Request Reset:**
    - Email field to send reset link.
- **Reset Password:**
    - New password entry (mandatory, min 6 characters)

### Campaign Management Screens

#### Campaign List Page

- **Display:**
    - Paginated list of campaigns.
- **Columns:**
    - Campaign Name
    - Crawling Status
    - Campaign Type
    - Sent Status
    - Number of Leads
- **Actions:**
    - Edit
    - View Leads
    - Delete

#### Campaign Form Page

- **Fields:**
    - Campaign Name
    - Email Subject
    - Email Body
    - Campaign Type (LinkedIn, Sales Navigator)
    - Search URL
- **Buttons:**
    - Save
    - Trigger Crawl (enabled when Search URL is provided)
    - Send Campaign (enabled when crawling is finished)
    - Send as Test (enabled when crawling is finished)
- **Validation:**
    - Mandatory fields highlighted
    - Status indicators for actions

### Leads Management Screens

#### Leads List Page

- **Display:**
    - Paginated list of leads for a campaign.
- **Columns:**
    - First Name
    - Last Name
    - Email
    - Company
    - Company Description
- **Actions:**
    - Edit inline
    - Delete
    - Add New Lead

### Settings Screen

- **Fields:**
    - LinkedIn Cookie
    - DropContact API Key
    - Zoho Campaigns API Key
    - Email Bounce Address
    - Email Domain
    - Email Sender
- **Validation:**
    - All fields are mandatory.
    - Incomplete settings prompt notifications.

---

## 10. Use Cases

### User Registration and Email Confirmation

- **Actor:** New User
- **Preconditions:** None
- **Flow:**
    1. User navigates to the Sign Up page.
    2. Fills in mandatory fields and submits the form.
    3. System sends a confirmation email.
    4. User is prompted to check their email.
    5. Clicks the confirmation link in the email.
    6. Account is activated; user gains access to the dashboard.

### Creating and Managing a Campaign

- **Actor:** Authenticated User
- **Preconditions:** User has completed settings configuration.
- **Flow:**
    1. User navigates to the Campaigns page.
    2. Clicks "Create New Campaign."
    3. Fills in campaign details and saves.
    4. Triggers crawling if Search URL is provided.
    5. System updates crawling status.
    6. Upon completion, user can send the campaign or test emails.

### Managing Leads

- **Actor:** Authenticated User
- **Preconditions:** Campaign has completed crawling.
- **Flow:**
    1. Selects a campaign and views associated leads.
    2. Edits or deletes leads as needed.
    3. Adds new leads manually if required.

---

## 11. Constraints and Assumptions

- **API Keys and Cookies:** Users must provide valid API keys and cookies for third-party integrations.
- **External Services:** The system assumes external services for crawling and email sending are operational.
- **Email Confirmation:** Mandatory before accessing main functionalities.
- **Settings Completion:** All settings fields are mandatory and must be completed before using campaigns and leads modules.
- **Data Privacy:** The system must comply with data protection laws and regulations.

---

## 12. Appendices

### Appendix A: Glossary

- **Crawling:** Automated process of extracting data from websites.
- **Lead Enrichment:** Adding additional information to leads, such as verified emails.
- **Soft Delete:** Marking data as deleted without physically removing it from the database.

### Appendix B: API Specifications

- Detailed **OpenAPI 3.1** definitions for all endpoints (to be provided separately).

### Appendix C: Database Schema Diagrams

- ER diagrams illustrating the relationships between entities (to be provided separately).

### Appendix D: User Interface Mockups

- Visual representations of the UI screens (to be provided separately).