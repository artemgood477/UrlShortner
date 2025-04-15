## Section 1 - Project Description  

## Project  

**Project Name:** URL Shortener Service  

## Description  

The **URL Shortener Service** is a web-based application designed to convert long URLs into short, easily shareable links.  
This service allows users to enter a long URL and receive a shortened version that redirects to the original URL when accessed.  

### Key Features:  
- Shorten long URLs into compact links  
- Store shortened URLs in **DynamoDB** for persistence  
- Retrieve and redirect users when accessing a short URL  
- Deployable via **AWS Lambda, API Gateway, and S3** for scalability and cost efficiency  
- Simple **frontend UI** hosted on **S3** for easy access  

### Target Audience:  
- Developers needing short links for API integrations  
- General users who want easy-to-share URLs  
- Marketing professionals tracking link engagement  

<br><br>

## Section 2 - Overview

### Purpose  
This project focuses on providing URL shortening functionality as part of the overall project. It is designed for users who require a simple and efficient way to generate short URLs from long URLs, improving shareability and accessibility.

### Scope  
This project implements a RESTful API for URL shortening, storing mappings in a DynamoDB table, and serving shortened links via API Gateway and Lambda.  
The frontend is deployed via **S3 Static Website Hosting**, allowing users to generate and use shortened URLs from a clean web interface.

### Requirements  

#### Functional Requirements  

- **R1:** The system shall allow users to submit a long URL and receive a shortened URL.  
- **R2:** The system shall retrieve and redirect users to the original URL when they access a shortened URL.  
- **R3:** The system shall store shortened URLs in a DynamoDB table.  
- **R4:** The system shall prevent duplicate URL entries and reuse existing short links if applicable.  
- **R5:** The system shall return appropriate error messages for invalid or expired URLs.  

#### Non-Functional Requirements  

- **Performance:** The system shall process URL shortening requests in less than **500ms**.  
- **Scalability:** The system shall support up to **10,000 URL shortening requests per day**.  
- **Reliability:** The system shall have an uptime of **99.9%**, ensuring minimal downtime.  

#### Technical Requirements  

- **Backend:** AWS Lambda (Python)  
- **Storage:** DynamoDB (NoSQL) for mapping short URLs to original URLs  
- **API Management:** AWS API Gateway  
- **Frontend:** Static website hosted on AWS S3  

#### Security Requirements  

- **Rate Limiting:** API shall impose rate limits to prevent spam and excessive usage.  
- **Data Encryption:** All stored URLs shall be encrypted at rest in DynamoDB.  

#### Estimates  

| # | Description | Estimated Hours |  
|---|------------|----------------|  
| 1 | Setup AWS Lambda, API Gateway, and DynamoDB | 6 hrs |  
| 2 | Develop URL shortening logic in Lambda | 4 hrs |  
| 3 | Implement redirection and retrieval logic | 3 hrs |  
| 4 | Deploy frontend via S3 | 3 hrs |  
| 5 | Integrate custom domain & SSL | 4 hrs |  
| 6 | Implement security measures (Rate Limiting, Encryption) | 5 hrs |  
| **TOTAL** | | **25 hrs** |  

<br><br>

## Section 3 - System Architecture

### Overview  
The system architecture consists of a serverless backend built using AWS Lambda, API Gateway, and DynamoDB, along with a static frontend hosted on S3.  
Users interact with the frontend to submit long URLs, which are processed by the backend and stored in the database.  
When a shortened URL is accessed, the backend retrieves and redirects the user accordingly.

![Architecture Diagram](https://github.com/user-attachments/assets/54c69454-8b2e-489d-8059-5b54d583bb7a)

<br><br>

## Section 4 - Data Dictionary  

Below is a description of the key database table used in this module:

| Field      | Notes                                      | Type      |
|------------|--------------------------------------------|-----------|
| ID         | Unique Identifier for the shortened URL    | STRING    |
| LongURL    | The original URL before shortening         | STRING    |
| ShortCode  | The generated short identifier             | STRING    |
| CreatedAt  | Timestamp of when the URL was created      | TIMESTAMP |

<br><br>

## Section 5 - Data Design  

### Persistent/Static Data  
The system uses a **DynamoDB** table to store URL mappings.  

### Dataset  

**Entities:**  
- **ShortenedURL**  
  - Attributes: `ID (PK)`, `LongURL`, `ShortCode`, `CreatedAt`  
  - Relationships: Standalone, no foreign key dependencies.  

<br><br>

## Section 6 - User Interface Design  

### User Interface Design Overview  
The UI consists of a simple page where users can enter a long URL and receive a short link. It includes:  
- Input field for the long URL  
- Button to generate a shortened URL  
- Display area for the shortened link  

### User Interface Navigation Flow  
1. User enters a URL and clicks "Shorten".  
2. The system processes and generates a short link.  
3. The short link is displayed for the user to copy and use.  
4. When someone accesses the short link, they are redirected to the original URL.  

### Use Cases / User Function Description  

| Use Case     | Description                                             |
|--------------|---------------------------------------------------------|
| Shorten URL  | User enters a long URL, and the system provides a short link. |
| Redirect     | User accesses the short link, and the system redirects to the original URL. |

<br><br>

## Section 7 - Testing  

### Test Plan Creation  
The testing plan includes **unit testing, integration testing, and user acceptance testing (UAT)**.  

**Test Environment:**  

| Test Case           | Input                             | Expected Output              | Actual Output |
|---------------------|-----------------------------------|------------------------------|----------------|
| Shorten URL         | `https://www.youtube.com/`    | Short URL is generated       | ✅             |
| Redirect            | `https://6wpltrt8ob.execute-api.us-east-1.amazonaws.com/dev/b9051d`         | Redirects to original URL    | ✅             |
| Invalid Short URL   | `https://6wpltrt8ob.execute-api.us-east-1.amazonaws.com/dev/xyz999`         | 404 Error                    | ✅             |

<br><br>

## Section 8 - Monitoring  

To ensure system health, we monitor the following metrics:  

- **Performance Metrics**: Response time, API latency, server load  
- **Error Metrics**: Rate of failed requests, API Gateway logs  
- **Availability Metrics**: Uptime monitoring (99.9% target)  
- **User Metrics**: Number of active users, usage trends  

AWS CloudWatch and API Gateway logs are used for monitoring.  

<br><br>

## Section 9 - Other Interfaces  

### External Interfaces  
- **API Gateway**: Routes requests to Lambda functions  
- **DynamoDB**: Stores URL mappings  
- **S3**: Hosts the frontend UI  

<br><br>

## Section 10 - Extra Design Features / Outstanding Issues  

- Potential **custom domain** for shortened links  
- **Expiration feature** for temporary shortened URLs  
- **Analytics dashboard** for tracking URL usage (future enhancement)  

<br><br>

## Section 11 - Deployment Info  

| Component  | Location/URL |
|------------|--------------|
| Frontend   | [http://url-shortener-frontend-host.s3-website-us-east-1.amazonaws.com/]http://url-shortener-frontend-host.s3-website-us-east-1.amazonaws.com/) |
| API        | https://6wpltrt8ob.execute-api.us-east-1.amazonaws.com/dev/shorten |
| Backend    | AWS Lambda Function |
| Database   | DynamoDB Table: `ShortURLs` |

<br><br>

## Section 12 - Glossary  

| Term        | Definition                                      |
|-------------|-------------------------------------------------|
| API Gateway | AWS service for managing API endpoints          |
| Lambda      | Serverless compute service for backend logic    |
| DynamoDB    | NoSQL database for storing URL mappings         |
| S3          | AWS service for hosting static frontend assets  |
