# Mapping a REST API to AWS Lambda: Example Table

When designing a serverless REST API using AWS Lambda, each API route maps to a specific Lambda function, often managed through Amazon API Gateway.

Here’s an example mapping table for a User Management API:

| REST API Endpoint              | HTTP Method | Lambda Function       | Description                  |
|--------------------------------|-------------|-----------------------|------------------------------|
| /users                         | POST        | createUserLambda      | Create a new user            |
| /users/{user_id}               | GET         | getUserLambda         | Retrieve user details        |
| /users/{user_id}               | PUT         | updateUserLambda      | Update user information      |
| /users/{user_id}               | DELETE      | deleteUserLambda      | Delete a user                |
| /users                         | GET         | listUsersLambda       | List all users               |
| /users/{user_id}/orders        | GET         | getUserOrdersLambda   | Get orders for a user        |
| /users/{user_id}/orders/{order_id} | GET     | getOrderDetailsLambda | Get details of a specific order |

## How AWS Lambda Integrates with API Gateway

1. Amazon API Gateway receives the HTTP request.
2. API Gateway routes the request to the corresponding AWS Lambda function.
3. Lambda function processes the request and returns a response in JSON format.
4. API Gateway sends the response back to the client.

## Example AWS Lambda Handler (Python)

### 1️⃣ Lambda for Creating a User (createUserLambda)

```python
import json

def lambda_handler(event, context):
    body = json.loads(event['body'])
    user_id = "12345"  # Example: Generate user ID
    return {
        "statusCode": 201,
        "body": json.dumps({"message": "User created", "user_id": user_id})
    }
```

### 2️⃣ Lambda for Fetching a User (getUserLambda)

```python
def lambda_handler(event, context):
    user_id = event['pathParameters']['user_id']
    return {
        "statusCode": 200,
        "body": json.dumps({"user_id": user_id, "name": "John Doe"})
    }
```

## 🚀 Summary

- Each API route maps to a specific AWS Lambda function.
- Amazon API Gateway handles HTTP requests and routes them to Lambdas.
- Lambda functions return structured JSON responses.
