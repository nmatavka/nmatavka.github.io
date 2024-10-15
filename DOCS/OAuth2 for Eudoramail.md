# OAuth2 for Eudoramail

## Getting Started

### Introduction

As the creators of a modern e-Mail product for the desktop computer, we offer some support for the Open Authorisation protocol version 2 (better known as OAuth2). While we are not entirely content with how we've had to implement it for the time being, recent events have forced our hand, and certain problems demand alpha-quality solutions rather than none at all. We say "alpha-quality" because the *pro tem* solution we've had to put in place is *markedly* user-hostile and might as well be a Platonic archetype for the concept of a spit-and-baling-twine "bodge job". To help you understand why, we'll have to explain two very difficult concepts: *authentication* and *authorisation*.

In the past, *authentication*—the process of proving your identity to a server—was handled in Eudora and Eudoramail using SSL/TLS encryption (this is a technology that got renamed partway through its development), alongside a system known as *Basic Auth* (Basic Authentication). This method requires you to send your password with each transaction (such as sending or checking email) within a secure, encrypted tunnel. The server verifies the password, then either processes your request or denies it. If someone attempts a man-in-the-middle attack, all they see is the encrypted data passing through.

However, Basic Auth doesn’t address *authorization*—verifying whether you're permitted to perform certain actions. The assumption is that if you have the password, you are automatically authorized to perform any action. While this may be acceptable for a small, dedicated server, it becomes problematic for large *federated* (single sign-on) systems like Gmail or Microsoft, which handle billions of users and services beyond just email.

For example, consider an email client that only needs permission to send and receive messages but, using Basic Auth, has access to additional services like cloud storage or calendars. If this client were malicious, it could abuse that access. Or imagine a phishing website that tricks users into entering their passwords (for a federated service, naturally) under the guise of sports betting. With Basic Auth, it would have full access to users' accounts, regardless of encryption.

To solve this, modern systems rely on a trusted directory server that manages passwords and controls what services an application can access. The email client never sends the password directly to the service it's trying to use. Instead, the directory "hashes" the password with other data (like the requested permissions and user credentials) to generate a token that can be passed to the service. This token acts as proof of authorization, making the password itself irrelevant in regular communications between the client and the server.

Unfortunately, Eudora was discontinued before this system became widespread, and major email providers now require OAuth2 starting September 2024. OAuth2 is complex, and writing code to implement it is a considerable effort. Fortunately, there is an existing solution: a *local proxy*. In Eudoramail 8, we’ve integrated this proxy, which effectively enables OAuth2 support for Gmail and Microsoft services.

Here’s how it works: when you want to authorise Eudoramail to send or receive emails, the proxy opens a Chrome window where you can complete the authorisation process directly with Google or Microsoft. You’ll receive a long alphanumeric code that you’ll need to copy and paste. The proxy acts as the intermediary between Eudoramail and the email service, handling the authentication. Neither Eudoramail nor the proxy ever stores or sees your password.

### Microsoft Customers

**NOTE:** This section requires Microsoft Azure. The confusingly-named "free" access to Azure is nothing more than a 30-day free trial; although "pay-as-you-go" access is not **technically** free, it is free **in practice**, because app registration is zero-rated.

1. Sign in to the [Azure portal](https://azure.microsoft.com/en-in/get-started/azure-portal) and access the *Home* screen.

2. Click *App registrations*.

![img](./figs/fig29.png)

3. To register the Eudoramail application, click the **+** symbol next to *New Registration* on the *App registrations* screen.

![img](./figs/fig30.png)

4. On the *Register an application* screen, in the *Name* textbox, enter the name `Eudoramail` or some other memorable value.

![img](./figs/fig31.png)

5. Under *Supported account types*, select the option labelled *Accounts in any organizational directory (Any Microsoft Entra ID tenant – Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)*.

![img](./figs/fig32.png)

6. Click the *Register* button to save the changes. You will be redirected to a screen containing the *Application (client) ID*. **IMPORTANT:** Note the Client ID as it will be needed while setting up OAuth.

![img](./figs/fig33.png)

7. Navigate to *Manage* > *Certificates & secrets*. Click on the **+** symbol located next to *New client secret*.

![img](./figs/fig34.png)

8. In the *Add a client secret* window, enter a suitable description (for example, *Eudoramail proxy client secret*). Then, specify the client secret's expiration date according to taste (we, like Microsoft, recommend six months).

![img](./figs/fig35.png)

9. Click the *Add* button to save the changes.

10. Note the value associated with the *Secret Value*. **NOTE:** You will need this value later.

![img](./figs/fig36.png)

11. Navigate to *Manage* > *API permissions*. Click on the **+** symbol next to *Add a permission*.

![img](./figs/fig37.png)

12. On the *Request API permissions* screen, select the *Microsoft Graph API*.

![img](./figs/fig38.png)

13. Click on *Delegated permissions*.

14. On the *Request API permissions* screen, under *Select permissions*, search for `offline_access`. Click the checkbox next to *offline_access*.

![img](./figs/fig39.png)

15. On the *Request API permissions*, under *Select permissions*, search for `SMTP`. Click the checkbox next to *SMTP.Send*.

![img](./figs/fig40.png)

16. Click the *Add permissions* button to save the changes. All the permissions you selected are displayed on the screen.

![img](./figs/fig41.png)

17. Navigate to *Manage* > *Authentication*.

18. Click on the **+** symbol next to *Add a platform*.

![img](./figs/fig42.png)

19. On the *Configure platforms* screen, choose *Web*.

![img](./figs/fig43.png)

20. In the *Redirect URIs* textbox, input the redirect URI as http://localhost

![img](./figs/fig44.png)

21. Click the *Configure* button to save the changes.

22. Log in to the [**Microsoft 365 admin center**](https://admin.microsoft.com/). Navigate to *Users* > *Active Users*.

23. Select the mailbox from the list for which you have configured the *Client ID* and *Client Secret*.

![img](./figs/fig45.png)

24. Select *Mail* from the flyout menu. Click on *Manage email apps*.

![img](./figs/fig46.png)

25. On the *Manage email apps* screen, ensure the *Authenticated SMTP* setting is enabled.

![img](./figs/fig47.png)

26. Click the *Save changes* button.

### Google Customers (Gmail)

1. Sign in to the [Google Developers Console](https://console.cloud.google.com/cloud-resource-manager). Click the *Terms of Service* checkbox and then click the *Agree and Continue* button.

2. Click the *Select a project* drop-down menu on the left side of the page. A pop-up window will appear on the screen.

![img](./figs/fig48.png)

3. Click *New Project* on the right side of the pop-up window.

![img](./figs/fig49.png)

4. Fill in the *Project name* and click the *Create* button. For memorability, call it  **Eudoramail OAuth** or the like.

![img](./figs/fig50.png)

5. Once the project is created, click the menu icon on the left side of the page. Select the project you created.

![img](./figs/fig51.png)

6. Click the ≡ icon on the left side of the page. Select *APIs & Services* > *OAuth consent* screen.

![img](./figs/fig52.png)

7. Under *User Type*, select *External*. Click the *Create* button.

![img](./figs/fig53.png)

8. On the OAuth consent screen, provide the below inputs.
   1. **App name** – Provide the name of the app for which you are provisioning OAuth authentication. In this case, it will be Eudoramail.
   2. **User support eMail** — Provide an eMail where the users can contact you regarding their consent. If you are setting up OAuth for yourself only, this can be your own eMail.
   3. **Developer contact information** — Provide an eMail address for Google to contact you if there are any changes to the project.


Click the *Save and Continue* button to proceed to the next screen.

![img](./figs/fig54.png)

9. Click the *Add or Remove Scopes* button. A new window will appear on the right side of the screen.

![img](./figs/fig55.png)

10. Add [*https://mail.google.com*](https://mail.google.com/) under the *Manually add scopes* section. Click the *Add to Table* button.

![img](./figs/fig56.png)

11. The newly added entry is displayed on the *Filter* table. Click the *Update* button.

![img](./figs/fig57.png)

12. The newly added entry is under *Your restricted scopes* section. Click the *Save and Continue* button to proceed to the next screen.

![img](./figs/fig58.png)

13. Click the *Add users* button on the Test users screen. A pop-up window appears on the screen. Enter the email address you want to use to authenticate using the OAuth authentication. Click the *Add* button.

![img](./figs/fig59.png)

14. Click the *Back to Dashboard* button on the Summary page.

![img](./figs/fig60.png)

15. Click *Credentials* under the *APIs & Services* menu. Click the *Create Credentials* menu on the right side of the page and select *OAuth client ID*.

![img](./figs/fig61.png)

16. On the *Create OAuth client ID* page, select *Web application* in the *Application type* drop-down box**.**

![img](./figs/fig62.png)

17. In the *Name* textbox, input `Eudoramail`.

![img](./figs/fig63.png)

18. Click the *Add URI* button under the *Authorised Javascript origins* section. Input http://localhost.

![img](./figs/fig64.png)

19. Click the *Add URI* button under the *Authorised Redirect URIs* section. Input http://localhost.

![img](./figs/fig65.png)

20. Click the *Create* button. A pop-up window appears on the screen displaying the *Client ID* and *Client secret* code.

![img](./figs/fig66.png)

You need the *Client ID* and *Client secret* code while configuring the OAuth2 proxy in Eudoramail. Download the JSON and keep it handy.

### Yahoo! Customers (Ymail, Rocketmail)

**NOTE:** This section is under construction. Compatibility with Yahoo! isn't yet supported.

**NOTE:** At this point in time, Yahoo! does not *mandate* that OAuth2 be used. Plain Auth (username and password) is completely acceptable, *provided that* you (the user) trust the application in question.

1. Visit https://developer.yahoo.com/apps/create/. If necessary, sign in.
2. In the *Application Name* text box, enter something memorable (we suggest `Eudoramail`).

## Step 2: Proxy Setup

### All Customers

**NOTE:** The procedure to set up OAuth2 *in Eudoramail* is covered in a separate document, which is available [here](./OAuth2.pdf). All customers wishing to use OAuth2 **must** complete these steps after they have their access token. 