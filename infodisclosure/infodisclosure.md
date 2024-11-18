# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The insecure.ts file has a potential vulnerability related to NoSQL injection. Specifically, the vulnerability arises from the way user input is handled in the /userinfo route. The code directly uses the id query parameter to query the MongoDB database without any validation or sanitization. This allows an attacker to manipulate the query by injecting malicious query objects.

2. Briefly explain how a malicious attacker can exploit them.

- The code directly uses the id query parameter to query the MongoDB database without any validation or sanitization. This allows an attacker to manipulate the query by injecting malicious query objects.
- There is no validation to ensure that the id parameter is a valid MongoDB ObjectId. An attacker could exploit this by sending a specially crafted request that alters the intended query, potentially allowing them to access unauthorized data.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

- The code checks if the username query parameter is of type string. If it is not, the server responds with a 400 Bad Request status, indicating that the input format is invalid. This helps to ensure that only valid input is processed.
- The username input is sanitized by removing any non-alphanumeric characters using a regular expression (username.replace(/[^\w\s]/gi, '')). This step helps to prevent malicious input from being executed as part of a query, effectively mitigating the risk of NoSQL injection.
- The code includes a try-catch block around the database query. This ensures that any errors encountered during the query execution are caught and logged, and a 500 Internal Server Error response is sent to the client. 
- Instead of using a generic identifier like _id, the code queries the database using the username field, which is more appropriate for the context of user authentication. This reduces the risk of unintended data exposure.
