# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

- The application directly uses the id parameter from the query without any validation or sanitization. This makes it susceptible to NoSQL injection attacks, where an attacker can manipulate the query to execute unintended commands or queries against the database.
- If an attacker sends a specially crafted request that causes the database to perform extensive operations (e.g., querying for a large number of records or executing complex queries), it can lead to high resource consumption. This can exhaust server resources, leading to slowdowns or crashes.
  
2. Briefly explain how a malicious attacker can exploit them.

- An attacker could send a large number of requests with complex queries that require significant processing power or memory. This could involve using a loop or automated script to repeatedly hit the /userinfo endpoint with various id values, causing the database to perform extensive operations. This can lead to high CPU and memory usage, ultimately crashing the server or making it unresponsive.
- By sending malformed or unexpected data, an attacker could induce errors in the application. If these errors are not handled properly, they could lead to unhandled exceptions that crash the server, making it unavailable.
- Without rate limiting, an attacker can flood the server with requests in a short period. This could be done using tools like curl or automated scripts that continuously send requests to the /userinfo endpoint. The server may become overwhelmed, leading to denial of service for legitimate users.
  
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

- The application uses the express-rate-limit middleware to limit the number of requests that can be made to the /userinfo endpoint from a single IP address within a specified time window. In this case, it allows only 1 request every 5 seconds. This helps mitigate the risk of flooding the server with excessive requests, ensuring that legitimate users can still access the service.
- The code includes a try-catch block around the database query. This ensures that if an error occurs during the query execution, it is caught and handled gracefully. Instead of crashing the server, the application logs the error and responds with a 500 Internal Server Error status, maintaining the application's stability.