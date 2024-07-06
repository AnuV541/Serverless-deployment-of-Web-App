# Serverless-deployment-of-Web-App
In today’s decentralized world, web apps are crucial. They provide remote workers with unified access to software and are platform agnostic, running through internet browsers without needing installation.

However, developing web apps can be complex. They need a responsive UI, backend hosting, and a suitable tech stack. AWS simplifies these challenges by managing much of the logistics. Here’s a concise guide on creating your first web app and connecting it to a database using AWS (Serverless).
### Create a Web App

To begin, we'll use AWS Amplify to set up the static resources for our web app, including HTML, CSS, JavaScript, images, and other files. AWS Amplify simplifies the process of hosting static websites, making it straightforward and efficient. Once set up, it provides an accessible URL for your web app.

Before starting first, download the given html,css and Js files in a zip file called index.
Now go to AWS Console-> AWS Amplify and follow the given steps.
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/104a3583-a37c-4933-8918-72c233fbaefd)
Then, select Get Started. Then click the orange Get Started button beneath Host your web app. Then choose to Deploy without Git provider.
Click Continue.

Name your app SimonGame.

In the field for Environment name, input dev.

Select the Drag and drop method. Then drag index.zip and then click Save and deploy.

Create a Serverless Function
Next, let's add interactivity to our web app by writing some code in Python and JavaScript. We'll convert this code into an AWS Lambda function.

Open AWS Lambda Console:

Navigate to the AWS Lambda console. Ensure that you select the same region as your web app.
Create Lambda Function:

Go to AWS Console -> AWS Lambda
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/90251c1d-6710-4261-a32c-7fa41eaa08ec)
Provide the given details and create the function.
After creating go down and and add the given code.
Put this is lambda code:
import json

def lambda_handler(event, context):
    name = event['Name']  # Removed the comma at the end of this line
    email = event['mail']  # Changed 'mail' to 'email' to match the key in your event
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda, ' + name),  # This line is fine
        'email': json.dumps('email: ' + email)  # Changed this line to include 'email' key in the dictionary
}
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/947e0a87-a68e-4ec9-a9fe-ad2839b1da4e)
After this press deploy and then test. In the testing environment add the following code:
Testing environment:
{
"Name": "Myra",
"mail": "Myra@gmail.com"
}

![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/922c5638-816b-44f3-a536-4b46b526fcb2)

You will be able to see that the execution result for this will be:
{
  "statusCode": 200,
  "body": "\"Hello from Lambda, Myra\"",
  "email": "\"email: Myra@gmail.com\""
}
This means that the lambda function is working.

### Create API to connect the lambda function to the web app
Follow the given steps:
Go to AWS Consolve -> API -> Rest API
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/27de3b67-c263-468b-9ef8-d377c6766a9e)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/c1370ff9-ee37-4c08-beb7-578a8314a111)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/e31a07f0-c181-4d58-b0ab-a8f2971b6567)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/7e512d79-ae1d-46e6-8fdf-36bf26bb80d4)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/7cce4f0d-9b8e-427e-948d-a94ffaa6c01d)
Now save the given url to later check if the api is working or not. It should look something like this: https://lyjf2b904d.execute-api.ap-southeast-1.amazonaws.com/dev
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/018ab9dd-1588-46c1-a29d-ab0faac5b358)
Now when you test it, it should give an answer like this:
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/6cf0226c-eb9c-48f5-969e-d419d81ccb03)

### Creating Table

Go to AWS ConsolE -> DynamoDB 
While creating the table give it two columns- name and email
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/e427586f-f1b6-4de3-bcc8-b7cbe2ca3c3e)
In the policy section, create a new policy and add this:
Table policy:
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

### Testing API
To test the API, go to the API Gateway console
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/23f5b849-1e4c-46e6-bcf1-9221397304b9)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/75db5047-edef-44a5-bc6e-cb741686d19d)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/d4791161-56ae-49a5-8fd1-4a284615cc8b)
Add the following code to sepecify the permession:
Lambda code:
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
    email= event['mail'] variable
    response = table.put_item(
        Item={
            'Name': name,
            'Email': email,
            'LatestGreetingTime':now
            })
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda, ' + name)
    }

When you test it, you'll be able to see this as the output:
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/43221909-576b-4709-8ba5-3c7b9b314bc8)

You will be able to see the same output in the url, which was saved before.

Now , the same value from the lambda is added to th etable through the API:
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/f43e97b1-8d3e-4012-9416-9925caa55d12)
Now go to the same amplify and add your new updated html document with the api access given from the url copied.
Now run your function and view the output
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/92fb8d84-d39f-4091-bcb7-3ece3e31afb5)
![image](https://github.com/AnuV541/Serverless-deployment-of-Web-App/assets/110184106/064547fb-1279-42f5-a29a-ccbeac3943d0)

