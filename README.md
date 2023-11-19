# Dyte-Assignment

## Description

This project is a Log Ingestor and Query Interface designed to efficiently handle vast volumes of log data. The system provides routes & functions for ingesting logs and querying log data using full-text search or specific field filters. The log ingestor is built using Node.js, JavaScript, and Elastic Search database for log storage. Passport.js is used for user authentication and authorization.

## Getting Started


Log Ingestor and Query Interface
Overview
This project is a Log Ingestor and Query Interface designed to efficiently handle vast volumes of log data. The system provides a simple interface for querying log data using full-text search or specific field filters. The log ingestor is built using Node.js, JavaScript, and Elastic Search database for log storage. Passport.js is used for user authentication and authorization.

Getting Started

Clone the repository:
git clone https://github.com/CHIRAG-GUPTA-987/Dyte-Assignment.git
cd repository-directory

Install dependencies:
npm install

Configure Elastic Search:
Ensure Elastic Search is installed and running.
Update the Elastic Search connection details in the configuration file if needed.

Run the application:
node app.js
The log ingestor will be running on http://localhost:3000.


### Log Ingestor/Indexing logs

The indexLogs function is responsible for indexing multiple logs into the specified Elasticsearch index. It uses the Elasticsearch bulk API for improved efficiency in handling log data arrays.


### Searching Logs

The searchLogs function performs a search query on the Elasticsearch index based on specified filters such as timestamp, level, message, etc. It constructs a query using the Elasticsearch Query DSL and returns the search results.

### User Authentication and Authorization

- The user can Register/Login himself.
- The user can register himself by accessing this API.

  URL:** `POST https://pwassg.onrender.com/auth/register`
  {
    "email": "rohangoyal991gmail.com", 
    "username": "rohan987", 
    "password": "abcdefgh"
  }
  This will return an accessToken through which we can authorize a user.


- Basic authentication is implemented with a dummy user (username and password). You can use these credentials for authentication. We have used passportJs for authentication.

  Example :
  
  URL:** `POST https://pwassg.onrender.com/auth/login`
  {
      "username": "rohan987", 
      "password": "abcdefgh"
  }
  This will return an accessToken through which we can authorize a user.

  {
    "authToken":         "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7ImlkIjoiNjUzOTcwODVhZmM5YTBjNGU0YjVlN2ZlIn0sImlhdCI6MTY5ODI5MTI5OX0.F73s__86EeWePYAbL2LL-xIt44fHxYEP2Ivsv2flp2A"
  }
  
- Authorization is based on token authentication. Obtain a token after successful authentication to access protected endpoints.

## Logs Routes

The logs routes define the API endpoints for indexing and searching logs. These routes are protected, requiring user authentication and admin privileges.

In all APIs, you have to pass the accessToken in the header of the request you got while logging in for authorization.

1. **Ingest a New Log:**

   - **URL:** `POST http://localhost:3000/postlogs`
   - **Description:** Add a new Log to the dataset.
   - **Sample Request:**
        POST http://localhost:3000/postlogs

      Example :
     
      Sample Input Body
     {
        "indexName": "meet_logs",
        "logData": {
            "timestamp": "2023-09-15T08:00:00Z",
            "level": "error",
            "message": "Failed to complete the project",
            "resourceId": "server-1235",
            "traceId": "abc-xyz-124",
            "spanId": "span-987",
            "commit": "5e5342a",
            "metadata": {
                "parentResourceId": "server-0987"
            }
        }
    }


      Returned Object :
       {
          "message": "Record added successfully",
          "record": {
              "name": "Rohan",
              "salary": 5000,
              "currency": "INR",
              "department": "Engineering",
              "sub_department": "Platform2",
              "on_contract": false,
              "_id": "6539df122b9a8ccf572693a5",
              "__v": 0
          }
      }
    
     

2. **Search a Record:**

   - **URL:** `GET http://localhost:3000/logs/search`
   - **Description:** Search a/multiple record from the dataset.
   - **Sample Request:**

     `GET http://localhost:3000/logs/search`

    Sample Input Body if we only want to search with a field
     {
          "indexName": "sunday_logs",
          "fieldsToSearch": [
              { "fieldName": "level", "fieldValue": "error" }
          ]
      }

    Sample Input Body if we only want to search with the combination of fields and Timestamp
     {
        "indexName": "sunday_logs",
        "fieldsToSearch": [
            { "fieldName": "level", "fieldValue": "error" }
        ]
        "timestampFilter": {
             "startTime": "2023-09-15T09:00:00Z",
             "endTime": "2023-09-15T10:00:00Z"
         }
    }
   
     Returned Object :
     {
        "message": "Record deleted successfully",
        "record": {
            "_id": "6539df122b9a8ccf572693a5",
            "name": "Rohan",
            "salary": 5000,
            "currency": "INR",
            "department": "Engineering",
            "sub_department": "Platform2",
            "on_contract": false,
            "__v": 0
        }
    }
