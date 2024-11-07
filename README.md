# Backend

Moving from the frontend, the backend runs on AWS through a variety of different services to create an API that counts visitors to the website. Every time a visitor goes to the website, the index.html file will reference the Javascript file held within S3, which is then turned into an HTTP request to the API Gateway. It will then request the Lambda API with Python code, which increments the visitor count in DynamoDB, and returns the value. The value is then displayed as the visitor # at the top of the website, which is being displayed through the index.html file.

This repository seeks to serve as a restrospective on the creation of the process, and will break down each individual aspect that goes into the backend API.

### Javascript Code

The API Gateway handles requests for the Javascript, and helps communicate with the other tools such as Lambda and DynamoDB that are being used for the API. The Javascript is included in this repository under [visitor-counter.js](https://github.com/Uyu2/Backend/blob/3a33cee3994e22ebe4e1d77a2889e8935672de3c/visitor-counter.js). The [index.html](https://github.com/Uyu2/Backend/blob/3a33cee3994e22ebe4e1d77a2889e8935672de3c/index.html) file is also included in this repository, as the Javascript file is referenced in it, which is what triggers the API to function when a visitor clicks on the website. 

The Javascript itself operates on the frontend, requesting the number of visitors who have clicked on the page and then returning the value. It is then assigned to the website under "Visitor #: "

### Database

The database used to hold the visitor count is DynamoDB. This is a serverless and NoSQL database ensuring fluidity in terms of schema, and is also a tool within AWS. This is the backbone of the visitor counter, and is where the value is stored and incremented.
<br><br>
<img width="728" alt="Screenshot 2024-11-06 at 2 40 22 PM" src="https://github.com/user-attachments/assets/90d83c39-592f-4ab1-b064-f388e06efabe">
<br><br>
The data type shown above being path (String), also shows the ease of setup and simplicity in usage. The current table being visitor_count will have a single value tied to the count in DynamoDB resulting in fast response time. It also serves as a unique value for this specific count we're tracking, so there is low likelihood for issues arising between multiple columns, repeat values, or key issues like with alternative databases.

### API & Python

Rather than communicating directly with the database, the API is built on AWS Lambda and the API Gateway communicating with the database through requests from the web app. The way this is achieved is having the Javascript run upon a visitor visiting the site in the index.html file. This then triggers a request to the API through Lambda and API Gateway aiding communication, receiving the response for the visitor count from DynamoDB, and returning the correct count.
<br><br>
<img width="1068" alt="Screenshot 2024-11-06 at 3 14 07 PM" src="https://github.com/user-attachments/assets/bc9762c1-237a-4c4e-982a-d1d75e15fe7b">
<br><br>
This is impossible to achieve if the API Gateway and Lambda function are not integrated. As shown above, once they are integrated, they can communicate with each other. The Javascript file in this repository is the function that will cause the frontend code and backend logic to interact, through the API Gateway endpoint. The [Python file](https://github.com/Uyu2/Backend/blob/3a33cee3994e22ebe4e1d77a2889e8935672de3c/visitorCount.py) is the code within the Lambda function, and when triggered is the function that increments the visitor count and returns the value.

Going through the Python code more in-depth, the table is referenced from DynamoDB when the Lambda function activates. It increments the current number, then gets the visitor_count specifically from its 'path' and saves the response as visitor_count. At the end, it then returns this value, where it is then put as the value for the current amount of visitors.
