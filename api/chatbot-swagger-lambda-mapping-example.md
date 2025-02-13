```swagger
openapi: 3.0.1
info:
  title: Chatbot API
  description: RESTful API for a serverless chatbot service powered by AWS Lambda
  version: 1.0.0
servers:
  - url: https://api.chatbotplatform.com/v1

paths:
  /chats:
    post:
      summary: Create a new chat session
      description: Starts a new chat session with the chatbot
      operationId: createChatSession
      responses:
        "201":
          description: Chat session successfully created
          content:
            application/json:
              schema:
                type: object
                properties:
                  chat_id:
                    type: string
                    example: "12345"
      x-amazon-apigateway-integration:
        type: aws_proxy
        httpMethod: POST
        uri: arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:createChatSessionLambda/invocations

  /chats/{chat_id}/messages:
    post:
      summary: Send a message to the chatbot
      description: Sends a user message to the chatbot and gets a response
      operationId: sendMessage
      parameters:
        - name: chat_id
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                message:
                  type: string
                  example: "Hello, what are your features?"
      responses:
        "200":
          description: Chatbot response
          content:
            application/json:
              schema:
                type: object
                properties:
                  chat_id:
                    type: string
                    example: "12345"
                  user_message:
                    type: string
                    example: "Hello, what are your features?"
                  bot_response:
                    type: string
                    example: "I can assist with FAQs, bookings, and troubleshooting!"
      x-amazon-apigateway-integration:
        type: aws_proxy
        httpMethod: POST
        uri: arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:sendMessageLambda/invocations

    get:
      summary: Retrieve chat history
      description: Gets all messages from a specific chat session
      operationId: getChatHistory
      parameters:
        - name: chat_id
          in: path
          required: true
          schema:
            type: string
      responses:
        "200":
          description: Chat history retrieved successfully
          content:
            application/json:
              schema:
                type: object
                properties:
                  chat_id:
                    type: string
                    example: "12345"
                  messages:
                    type: array
                    items:
                      type: object
                      properties:
                        sender:
                          type: string
                          example: "user"
                        message:
                          type: string
                          example: "Hello"
      x-amazon-apigateway-integration:
        type: aws_proxy
        httpMethod: GET
        uri: arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:getChatHistoryLambda/invocations

  /chats/{chat_id}:
    delete:
      summary: End and delete a chat session
      description: Ends a chat session and deletes it from the system
      operationId: deleteChatSession
      parameters:
        - name: chat_id
          in: path
          required: true
          schema:
            type: string
      responses:
        "204":
          description: Chat session successfully deleted
      x-amazon-apigateway-integration:
        type: aws_proxy
        httpMethod: DELETE
        uri: arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:deleteChatSessionLambda/invocations

  /bots/{bot_id}/status:
    get:
      summary: Get chatbot status
      description: Retrieves the operational status of the chatbot
      operationId: getBotStatus
      parameters:
        - name: bot_id
          in: path
          required: true
          schema:
            type: string
      responses:
        "200":
          description: Chatbot status retrieved successfully
          content:
            application/json:
              schema:
                type: object
                properties:
                  bot_id:
                    type: string
                    example: "98765"
                  status:
                    type: string
                    example: "Online"
      x-amazon-apigateway-integration:
        type: aws_proxy
        httpMethod: GET
        uri: arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:getBotStatusLambda/invocations
```