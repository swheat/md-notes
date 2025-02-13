# Mapping a REST API to AWS Lambda for a Chatbot Web Service

For a serverless chatbot web service, API Gateway routes requests to AWS Lambda functions, which handle message processing, session management, and user interactions.

## REST API to AWS Lambda Mapping Table

| REST API Endpoint              | HTTP Method | Lambda Function           | Description                              |
|--------------------------------|-------------|---------------------------|------------------------------------------|
| /chats                         | POST        | createChatSessionLambda   | Create a new chat session                |
| /chats/{chat_id}/messages      | POST        | sendMessageLambda         | Send a message to the chatbot and receive a response |
| /chats/{chat_id}/messages      | GET         | getChatHistoryLambda      | Retrieve all messages in a chat session  |
| /chats/{chat_id}               | DELETE      | deleteChatSessionLambda   | End and delete a chat session            |
| /users/{user_id}/chats         | GET         | listUserChatsLambda       | Retrieve a list of all chat sessions for a user |
| /bots/{bot_id}/status          | GET         | getBotStatusLambda        | Get the current status of the chatbot    |
| /bots/{bot_id}/settings        | PUT         | updateBotSettingsLambda   | Update chatbot settings                  |

## How AWS Lambda Integrates with API Gateway

1. Amazon API Gateway receives HTTP requests from the chatbot frontend.
2. API Gateway routes the request to the appropriate AWS Lambda function.
3. Lambda function processes the request, calls external AI services (e.g., Amazon Lex, OpenAI API), and returns a response.
4. API Gateway sends the response back to the chatbot client.

## Example AWS Lambda Handlers for Chatbot Service

### 1️⃣ Create a New Chat Session (createChatSessionLambda)

```python
import json
import uuid

def lambda_handler(event, context):
    chat_id = str(uuid.uuid4())  # Generate unique chat session ID
    return {
        "statusCode": 201,
        "body": json.dumps({"message": "Chat session created", "chat_id": chat_id})
    }
```

### 2️⃣ Send a Message to the Chatbot (sendMessageLambda)

```python
import json

def lambda_handler(event, context):
    chat_id = event['pathParameters']['chat_id']
    body = json.loads(event['body'])
    user_message = body['message']

    # Simulate chatbot response (this can call Amazon Lex or an LLM)
    bot_response = f"Echo: {user_message}"

    return {
        "statusCode": 200,
        "body": json.dumps({
            "chat_id": chat_id,
            "user_message": user_message,
            "bot_response": bot_response
        })
    }
```

### 3️⃣ Retrieve Chat History (getChatHistoryLambda)

```python
def lambda_handler(event, context):
    chat_id = event['pathParameters']['chat_id']
    
    # Simulated response (Replace with DB query for real use)
    chat_history = [
        {"sender": "user", "message": "Hello"},
        {"sender": "bot", "message": "Hi! How can I help?"}
    ]
    
    return {
        "statusCode": 200,
        "body": json.dumps({"chat_id": chat_id, "messages": chat_history})
    }
```

## 🚀 Summary

✔ Each API route maps to a Lambda function for chatbot processing.
✔ Amazon API Gateway handles HTTP requests and forwards them to Lambda.
✔ Lambda functions interact with AI models (e.g., Amazon Lex, OpenAI API) to generate chatbot responses.
✔ Chat history can be stored in DynamoDB, S3, or an external database.
