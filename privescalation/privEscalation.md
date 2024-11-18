# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The insecure.ts file contains several potential vulnerabilities related to privilege escalation. The code simulates user authentication by simply finding a user in the users array based on the provided userId.

2. Briefly explain how a malicious attacker can exploit them.

- The authorization check only verifies if the user's role is 'admin' before allowing a role update. This means that any user with a valid userId can potentially change their own role or the role of others if they can manipulate the request, leading to privilege escalation.
- The code does not validate the newRole input, which could allow an attacker to set arbitrary roles, including potentially harmful ones, if they can bypass the authorization check.
  
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

- The application uses express-session to manage user sessions. This allows the server to track authenticated users and ensures that only logged-in users can perform sensitive actions, such as updating user roles.
- The code checks if the userId provided in the request corresponds to a valid user in the system. If the user does not exist, it returns a 404 error, which helps prevent attempts to update roles for non-existent users.
- The session middleware is configured with secure cookie settings (httpOnly and sameSite: 'strict'), which help mitigate risks such as cross-site scripting (XSS) and cross-site request forgery (CSRF) attacks.