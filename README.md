# AWS-Realtime-Serverless-GraphQL-API
First attempt build of my Realtime Serverless GraphQL API with Websockets on AWS build:

Our real-time serverless GraphQL is an API that uses GraphQL as the query language, allowing clients to request only the data they need. 

Showing my ability to create a realtime serverless app using AWS AppSync, which is natively supporting Websockets for real-time subscriptions. 

GraphQL in generality provides; efficient data fetching as clients request only the fields they need, reducing over-fetching and under-fetching, provides flexible queries as one endpoint can serve multiple use cases, and it provides built-in subscriptions which are basically just using WebSockets under the hood.

PROJECT STRUCTURE:
REALTIME-CHAT
|-- mapping-templates
   |-- ForwardResult.response.vtl
   |-- Message.request.vtl
|-- .env
|-- schema.graphql
|-- serverless.yml
|-- deployment.txt


[.env] file for transparency:
===========================================
# ==============================
# AWS Environment Variables
# ==============================
export AWS_ACCOUNT_ID=123456789  # Replace with your AWS Account ID

# Fetch the AppSync API ID dynamically using AWS CLI
export APPSYNC_API_ID=$(aws appsync list-graphql-apis \
    --query 'graphqlApis[?name==`realtimeChatAPI`].apiId' \
    --output text >/dev/null 2>&1)

# Fetch the API Key dynamically using AWS CLI
export APPSYNC_API_KEY=$(aws appsync list-api-keys \
    --api-id "$APPSYNC_API_ID" \
    --query 'apiKeys[0].id' \
    --output text >/dev/null 2>&1)
===========================================


Brief Schema/Timeline:

1) Define GraphQL Schema (schema.graphql)
> Specifies Queries, Mutations, and Subscriptions for real-time communication.
> Uses the @aws_subscribe directive to link real-time updates to the correct mutation.

2) Set Up Mapping Templates (mapping-templates/)
> Uses AWS AppSync resolvers with Velocity Template Language (VTL) to process GraphQL requests/responses.
> Request Template (Message.request.vtl) extracts user input and formats it.
> Response Template (ForwardResult.response.vtl) formats and returns the response.

3) Configure Serverless Framework (serverless.yml)
> Uses the serverless-appsync-plugin to automate API deployment.
> Defines the API authentication type, schema location, and mapping templates.
> Uses local resolvers (type: NONE) to relay messages in real-time without storing them in a database.

4) Set Up Environment Variables (.env)
> Dynamically retrieves the AppSync API ID and API Key using AWS CLI.
> Ensures secure and automated configuration for deployment.

5) Deploy API to AWS AppSync
> Installs dependencies with:
npm install --dev serverless-appsync-plugin
> Deploys the API with a single command:
. .env && sls deploy
> The API is now live on AWS AppSync and supports real-time messaging.

6) Client App
Furthermore, my showcase client app in vanilla javascript that is receiving updates via websockets to showoff the functionality of the API:
(work in progress, updating repo with finalized Application soon, stay tuned!)
