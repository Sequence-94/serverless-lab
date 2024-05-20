![serverless crud](https://github.com/Sequence-94/serverless-lab/assets/53806574/d9d7bccd-65af-40e7-8559-ba7c780d2972)

Step1:
First I created an IAM Role(execution role) that will allow my lambda function access to aws resources such as dynambodb and cloudwatch. This policy will allow me to take action on the dynamodb table such as getitem,deleteitem...etc. This policy also allows me to upload logs onto cloudwatch incase i want analyse the logs later on. Below is the policy for the execution role.
![execpolicy](https://github.com/Sequence-94/serverless-lab/assets/53806574/49216a30-b50d-42e3-a9c8-76c9cd9882d9)

Step2:
I created a lambda function and chose to "Author from Scratch", with the name "LambdaFunctionOverHttps", with a runtime "Python 3.8". I then applied the existing execution role I had created in step 1 and left everything else as default. Below is a screenshot of the lambda function that was created.
![lambda func](https://github.com/Sequence-94/serverless-lab/assets/53806574/d27bee44-1a06-4998-af90-4fc907cc38c5)

Step3:
We have a script written in python code that imports boto3 which is AWS SDK for python so we're able to interact with aws resources via python code. We also import json for JSON data as our inputs will be in JSON format. We have a main handler fucntion so this will be invoked everytime this lambda function is triggered. What this does is that the Lambda function processes an event to perform some operation(get,scan,delete...)on a DynamoDB table. It dynamically handles different CRUD operations based on the input. If the operation is not recognized, it raises an error. Below is the python script that will be deployed as my source code.
![pyscript](https://github.com/Sequence-94/serverless-lab/assets/53806574/2411b693-7c16-4d8f-962d-6242feb4589f)

Error: One of the error that I faced was that the script that was getting invoked was the default script because I had forgot to click on "Deploy" after having updated it so that the correct source code could be executed when the lambda function was triggered.This was partly due to not having tested the function before actually creating the dynamodb table.

Step4: Test the lambda function. This is a simple test that will run the echo part of the python script because we have yet to create the dynamodb table.
Before Test:
![beforetest](https://github.com/Sequence-94/serverless-lab/assets/53806574/b6f5095c-8ee1-4c2c-8a94-761cc55ad195)
After Test:
![aftertest](https://github.com/Sequence-94/serverless-lab/assets/53806574/060b7814-f96c-49ff-9289-85170e9c49ef)

Step5:
Create Dynamodb table. 
This is a NoSQL database, which is inherently highly available. This means that all data is automatically replicated across multiple availability zones. Also, it is capable of horizontal scaling on its own when faced with high traffic.
Using the aws console to create a dynamodb is very simple , I gave it the name "lambda-apigateway" with the partition key "id" as a "string" and choosing "Default settings" and therafter click "create table".
![dynamodb](https://github.com/Sequence-94/serverless-lab/assets/53806574/c1204433-8153-495b-9c34-770dc4e36fb7)

Step6:
Create API gateway in particular a REST API so we have complete control of requests and responses and also so that we can expose our Lambda function to the internet.
Doing this via aws consle is farely straightforward as well. I gave it a name "DynamoDBOperations" and chose "New API" and left eveyrthing thing as is.
![api](https://github.com/Sequence-94/serverless-lab/assets/53806574/cbb0df66-a8fd-47d3-bf98-c20eadfa6724)
Each API is collection of resources and methods that are integrated with backend HTTP endpoints or AWS services.

Step7:Create a resource for the API this will contain all of our methods such as post,get...etc
![resrc](https://github.com/Sequence-94/serverless-lab/assets/53806574/a5dd4f32-cf6a-453f-9baf-077feac679aa)

Step8: Create POST method.
For method type i picked "POST", so that we can update the table with some data since it is empty.
The intergation type will be lambda function since we are planning to use API -> Lambda -> dynamodb. 
AWS will scan for all our lambda functions that exist in the region we have selected in this case i only created one function in the us-east-1 so i seletced that one.
example of my Our API-Lambda integration below
![method](https://github.com/Sequence-94/serverless-lab/assets/53806574/031375c9-775e-446d-a479-53eb367bd5e5)

Step9: Deploy the API and copy the Invoke URL
I Selected "New Stage" for Deployment stage. Gave "Prod" as Stage name. Clicked "Deploy"
![deploy](https://github.com/Sequence-94/serverless-lab/assets/53806574/e350948c-e8dd-49a0-b07d-3792ea89fa8c)

Copy the Invoke URL:
![url](https://github.com/Sequence-94/serverless-lab/assets/53806574/3d0d9d67-8108-4472-b73a-4e117220806e)

Step10: Running the solution and Validate solution
To run the solution I installed a lightweight Rest API Client Extension from Visual Studio Code called Thunder client.
![vscode](https://github.com/Sequence-94/serverless-lab/assets/53806574/5c227051-a2ae-4062-ae9f-70caa4bae6ab)

Validating from vscode(I have inserted a couple of more data as well to make it visible)
![vscode validate](https://github.com/Sequence-94/serverless-lab/assets/53806574/abec99e8-89c7-433d-bb9e-67cb51b006e0)

Validating from Dynamodb AWS Console
![aws console](https://github.com/Sequence-94/serverless-lab/assets/53806574/7a0b1c1c-2e44-4252-b911-1926d7e25baa)

DONE!
