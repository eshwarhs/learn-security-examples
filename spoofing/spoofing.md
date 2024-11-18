# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

- The spoofing vulnerability in insecure.ts arises from the insecure handling of session cookies. Specifically, the server sets the httpOnly property of the session cookie to false, which allows client-side scripts to access the cookie. This makes it susceptible to Cross-Site Scripting (XSS) attacks, where an attacker can execute JavaScript in the context of the user's browser to steal session cookies. Additionally, the lack of CSRF protection allows attackers to perform unauthorized actions on behalf of the user, as demonstrated in the mal-csrf.html file, where a form submission can trigger sensitive operations without the user's consent.

2. Briefly explain different ways in which vulnerability can be exploited.

- An attacker can inject malicious JavaScript into a web page that is viewed by the victim. Since the session cookie is accessible due to the httpOnly flag being set to false, the attacker can use JavaScript to read the cookie and send it to their own server, effectively hijacking the user's session.
- An attacker can craft a link that, when clicked by the victim, redirects them to a malicious server (like mal.ts) that captures the session cookie. This can be done through social engineering tactics, such as phishing emails or misleading advertisements.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

- In secure.ts, the httpOnly property of the session cookie is set to true. This prevents client-side scripts from accessing the cookie, thereby mitigating the risk of Cross-Site Scripting (XSS) attacks that could steal session cookies.
- The sameSite attribute is set to true in secure.ts, which helps protect against Cross-Site Request Forgery (CSRF) attacks. This attribute restricts how cookies are sent with cross-origin requests, ensuring that the session cookie is not sent along with requests initiated by third-party sites.
- The session secret in secure.ts is passed as a command-line argument, which can enhance security by not hardcoding sensitive information directly in the source code. This practice helps prevent exposure of the secret in version control systems.