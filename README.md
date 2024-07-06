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
This means that the lambda functio is working.

