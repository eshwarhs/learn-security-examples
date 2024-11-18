# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
  
- The vulnerability in the insecure.ts file arises from the lack of authentication and logging mechanisms. This allows malicious users to send messages and retrieve them without any verification of their identity. As a result, users can repudiate their actions, claiming they did not send certain messages, as there is no record of who sent what. This can lead to misuse of the service and potential abuse, as anyone can interact with the endpoints without accountability.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.

-  The secure.ts file addresses the vulnerability by implementing user authentication and logging mechanisms. It requires users to be authenticated before they can send or retrieve messages, which helps ensure that only authorized users can perform these actions. Additionally, it logs all requests and actions taken by users, creating an audit trail that can be referenced in case of disputes. This way, users cannot easily repudiate their actions, as there is a record of their interactions with the server.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

- The secure version employs the Middleware design pattern. Middleware functions are used to process requests before they reach the route handlers. In secure.ts, middleware is utilized for logging requests and handling errors, as well as for implementing authentication checks. This pattern allows for separation of concerns, where different functionalities (like logging, authentication, and error handling) can be managed independently and applied to various routes without cluttering the main application logic. By using middleware, the application can maintain a clean structure while ensuring that security measures are consistently enforced across all routes.