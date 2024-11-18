# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

- The application directly uses user input (the name field) in the response without any sanitization. This allows an attacker to inject malicious scripts into the application. For example, if a user submits a script tag in the name field, it will be executed in the browser when the page is rendered.
- While the session management appears to be implemented correctly, the lack of input validation and sanitization can lead to session hijacking or manipulation if an attacker can inject scripts that access session data.

2. Briefly explain how a malicious attacker can exploit them.

- By submitting a crafted input in the name field, such as <script>alert('Hacked!');</script>, the attacker can inject malicious JavaScript into the application. When the server responds and renders the page, this script will execute in the context of the user's browser, allowing the attacker to perform actions like displaying alerts, redirecting users, or stealing sensitive information.
-  If the attacker can inject scripts that access session data, they could potentially steal session cookies or tokens. This would allow them to impersonate the user, gaining unauthorized access to sensitive areas of the application or user accounts.
-  The attacker could use the injected script to create a fake login form or redirect users to a malicious site, tricking them into providing their credentials or other sensitive information.
  
3. Briefly explain why **secure.ts** does not have the same vulnerabilties

- In secure.ts, user input from the name field is sanitized using the escapeHTML function before being used in the response. This function escapes special HTML characters, preventing any injected scripts from being executed in the browser. For example, if a user submits <script>alert('Hacked!');</script>, it would be rendered as plain text rather than executed as a script.
- secure.ts follows best practices for web application security, such as using httpOnly and sameSite attributes for cookies, which help protect against cross-site request forgery (CSRF) and other attacks.