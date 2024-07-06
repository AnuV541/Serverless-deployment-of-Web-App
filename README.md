# Serverless Deployment of Web App

In today’s decentralized world, web apps are crucial. They provide remote workers with unified access to software and are platform agnostic, running through internet browsers without needing installation.

However, developing web apps can be complex. They need a responsive UI, backend hosting, and a suitable tech stack. AWS simplifies these challenges by managing much of the logistics. Here’s a concise guide on creating your first web app and connecting it to a database using AWS (Serverless).

### Create a Web App

To begin, we'll use AWS Amplify to set up the static resources for our web app, including HTML, CSS, JavaScript, images, and other files. AWS Amplify simplifies the process of hosting static websites, making it straightforward and efficient. Once set up, it provides an accessible URL for your web app.

1. **Download the Required Files:**
   - First, download the provided HTML, CSS, and JavaScript files in a zip file called `index.zip`.

2. **Go to AWS Console:**
   - Navigate to the AWS Amplify service.

   ![AWS Amplify Console](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/104a3583-a37c-4933-8918-72c233fbaefd)

3. **Start Hosting:**
   - Select "Get Started" under "Host your web app."

4. **Deploy without Git Provider:**
   - Choose the option to deploy without a Git provider and click "Continue."

5. **Name Your App:**
   - Name your app "SimonGame" and set the environment name to "dev."

6. **Upload Files:**
   - Select the "Drag and drop" method, upload `index.zip`, and then click "Save and deploy."

### Create a Serverless Function

Next, let's add interactivity to our web app by writing some code in Python and JavaScript. We'll convert this code into an AWS Lambda function.

1. **Open AWS Lambda Console:**
   - Navigate to the AWS Lambda console. Ensure that you select the same region as your web app.

2. **Create Lambda Function:**
   - Go to AWS Console -> AWS Lambda.

   ![AWS Lambda Console](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/90251c1d-6710-4261-a32c-7fa41eaa08ec)

3. **Provide Details and Create Function:**
   - Provide the necessary details to create the function.

4. **Add Code to Lambda:**
   - Scroll down and add the following code:

     ```python
     import json

     def lambda_handler(event, context):
         name = event['Name']
         email = event['mail']
         return {
             'statusCode': 200,
             'body': json.dumps('Hello from Lambda, ' + name),
             'email': json.dumps('email: ' + email)
         }
     ```

   ![Lambda Code](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/947e0a87-a68e-4ec9-a9fe-ad2839b1da4e)

5. **Deploy and Test:**
   - After deploying, test the function with the following test event:

     ```json
     {
       "Name": "Myra",
       "mail": "Myra@gmail.com"
     }
     ```

   ![Test Lambda](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/922c5638-816b-44f3-a536-4b46b526fcb2)

   - The execution result should be:

     ```json
     {
       "statusCode": 200,
       "body": "\"Hello from Lambda, Myra\"",
       "email": "\"email: Myra@gmail.com\""
     }
     ```

### Create API to Connect the Lambda Function to the Web App

1. **Go to API Gateway:**
   - Navigate to AWS Console -> API Gateway -> REST API.

   ![API Gateway Step 1](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/27de3b67-c263-468b-9ef8-d377c6766a9e)

   ![API Gateway Step 2](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/c1370ff9-ee37-4c08-beb7-578a8314a111)

2. **Create and Configure API:**
   - Create a new API and configure it to connect to the Lambda function.

   ![API Gateway Step 3](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/e31a07f0-c181-4d58-b0ab-a8f2971b6567)

   ![API Gateway Step 4](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/7e512d79-ae1d-46e6-8fdf-36bf26bb80d4)

3. **Save the API URL:**
   - Save the generated API URL for later testing. It should look something like this: `https://lyjf2b904d.execute-api.ap-southeast-1.amazonaws.com/dev`.

   ![API URL](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/018ab9dd-1588-46c1-a29d-ab0faac5b358)

4. **Test the API:**
   - When you test it, you should get a response similar to this:

   ![API Test Result](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/6cf0226c-eb9c-48f5-969e-d419d81ccb03)

### Create DynamoDB Table

1. **Go to DynamoDB Console:**
   - Navigate to AWS Console -> DynamoDB.

2. **Create Table:**
   - Create a table with two columns: `name` and `email`.

   ![Create DynamoDB Table](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/e427586f-f1b6-4de3-bcc8-b7cbe2ca3c3e)

3. **Add Table Policy:**
   - Create a new policy and add the following permissions:

     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "VisualEditor0",
           "Effect": "Allow",
           "Principal": {
             "AWS": "arn:aws:iam::050016925652:root"
           },
           "Action": [
             "dynamodb:PutItem",
             "dynamodb:DeleteItem",
             "dynamodb:GetItem",
             "dynamodb:Scan",
             "dynamodb:Query",
             "dynamodb:UpdateItem"
           ],
           "Resource": "arn:aws:dynamodb:ap-southeast-1:050016925652:table/Simon_Database"
         }
       ]
     }
     ```

### Testing API with DynamoDB Integration

1. **Test API in API Gateway:**
   - Go to the API Gateway console and test your API.

   ![Test API](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/23f5b849-1e4c-46e6-bcf1-9221397304b9)

   ![API Gateway Console](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/75db5047-edef-44a5-bc6e-cb741686d19d)

   ![Test API Permissions](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/d4791161-56ae-49a5-8fd1-4a284615cc8b)

2. **Add Lambda Code for DynamoDB:**
   - Update your Lambda function code to interact with DynamoDB:

     ```python
     # import the json utility package since we will be working with a JSON object
     import json
     # import the AWS SDK (for Python the package name is boto3)
     import boto3
     # import two packages to help us with dates and date formatting
     from time import gmtime, strftime

     # create a DynamoDB object using the AWS SDK
     dynamodb = boto3.resource('dynamodb')
     # use the DynamoDB object to select our table
     table = dynamodb.Table('Simon_Database')
     # store the current time in a human readable format in a variable
     now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

     # define the handler function that the Lambda service will use as an entry point
     def lambda_handler(event, context):
         name = event['Name']
         email = event['mail']
         response = table.put_item(
             Item={
                 'Name': name,
                 'Email': email,
                 'LatestGreetingTime': now
             })
         return {
             'statusCode': 200,
             'body': json.dumps('Hello from Lambda, ' + name)
         }
     ```
3. **Test Lambda and DynamoDB Integration:**
   - When you test it, you'll see an output like this:

   ![Lambda Test Result](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/43221909-576b-4709-8ba5-3c7b9b314bc8)

4. **Verify DynamoDB Table:**
   - Check the DynamoDB table to ensure the values from Lambda are added correctly.

   ![DynamoDB Table Verification](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/f43e97b1-8d3e-4012-9416-9925caa55d12)

### Update Web App with API Integration

1. **Update Amplify with New HTML:**
   - Go back to AWS Amplify and upload your updated HTML document with the API access URL.

2. **Run and Verify:**
   - Run your function and view the output in the web app.

   ![Updated Web App](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/92fb8d84-d39f-4091-bcb7-3ece3e31afb5)

   ![Web App Output](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/064547fb-1279-42f5-a29a-ccbeac3943d0)

This guide should help you deploy a serverless web app using AWS services efficiently and effectively.
