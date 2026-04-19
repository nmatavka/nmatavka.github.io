As the creators of a modern e-Mail product for the desktop computer, we offer some support for the Open Authorisation protocol version 2 (better known as OAuth2). While we are not entirely content with how we've had to implement it for the time being, recent events have forced our hand, and certain problems demand alpha-quality solutions rather than none at all. We say "alpha-quality" because the *pro tem* solution we've had to put in place is *markedly* user-hostile and might as well be a Platonic archetype for the concept of a spit-and-baling-twine "bodge job". To help you understand why, we'll have to explain two very difficult concepts: *authentication* and *authorisation*.

Imagine the data of a user’s account is divided up and physically stored in one of the rooms of their house. One room holds all their contact information, and another room has a box full of signed letterheads. Obviously, to show a guest what’s in their house, they’ll need to give them *some sort* of house key—in other words, the guest needs to be *authenticated*—and their *credentials* can be a password, door code, USB dongle, card, or a literal, physical key.

## Basic Auth

In the simplest and oldest scheme, known as *Basic Auth* (or Basic Authentication), *any* person who has the key will have full access to *all* the rooms of the house, with all the data.

There are situations where this is more than good enough, as long as the guest is safe from being mugged (this is called a *man-in-the-middle* attack) and is *known* to have good intentions. This is all the more true if the apartment is small and doesn't have a lot of rooms to snoop in, and if the number of guests is small.

In cyberspace, the equivalent would be an eMail-only server with a few thousand users; as long as the lock *itself* is secure and the user is safe from MitM, no harm can come from every user using their (unlimited scope) password.

The problems come when these basic assumptions are challenged. For example, a mansion with 32 rooms will have *very* different security needs: here, we are talking 

Basic Auth only requires a user’s credentials to gain access to their online account. The account user’s credentials are sent from the “every request” application. Although this process is straightforward, it can leave your credentials and, eventually, your online account vulnerable.

Here are a few disadvantages of basic auth:

- If your connection isn’t secured through transport layer security (TLS), your password may be compromised.
- If you don’t set up multi-factor authentication (MFA), typically used with Basic Auth, there are no additional layers of security to prevent people who now have your credentials from accessing your account whenever they want.
- Your credentials give access to all the resources in your account.
- Anyone can use your credentials at any time.



 OAuth is basically the industry standard for web apps to vouch for each other. You're basically asking, Twitter, Facebook, Github, whoever to keep track of it for you in a few steps:

1. Someone walks up to your website and asks to come in. You have no idea who they are and have very little idea how to check. You ask Facebook (or whoever) to check for you since they have a whole system in place to do that.
2. They tell Facebook who they are (usually through a reroute to a login page or through a pop up and they supply their Facebook credentials) and then Facebook turns around and tells you, 'Yeah, they're who they say they are' and hand you a signed piece of paper with their seal of approval on it (Rerouted back to your application with an auth code).
3. Now that Facebook said it was okay and gave you the thumbs up, you can now tell Facebook that you're cool with letting them in if they're cool with it. Facebook says 'okay' tells the person what information Facebook is going to tell you, and then gives them a temporary key (passing the auth code for a web token).

## What is OAuth?

OAuth is an open-standard authorization framework or protocol. It provides apps with the ability to secure designated access. For example, you can tell Instagram that it's OK for ESPN.com to access your Instagram profile or post updates on your timeline without having to give ESPN your Instagram password. This improves the security of your account significantly because if ESPN suffers a security breach, your Instagram password remains uncompromised.

Unlike Basic Auth, where you have to share your password with people who need to access your user account, OAuth doesn’t share password data. Instead, OAuth uses authorization tokens to verify an identity between consumers and service providers. OAuth is an authentication security solution that enables online users to approve one application interacting with another app on their behalf without the need to give away their passwords.

Your smart home devices, such as a thermostat, security systems, and toasters, use login data to sync with each other, allowing you to administer them from a client device or browser. These devices use what OAuth refers to as *confidential authorisation*. This means that those smart home devices hold on to secret key information; thus, you don’t have to log in every time you need to access them.

Typically, OAuth is more about authorization than authentication. Authorization involves asking for permission and access rights to do stuff. While authentication is all about proving you’re the correct person because you have the online account credentials. 

Again, unlike Basic Auth, OAuth doesn’t share authentication data between consumers and service providers and consumers, but it acts as an authorization protocol in some form.

An OAuth token is like the valet key. As an account user, you can tell consumers what they can use and what they can’t use from every service provider. This way, you can give each consumer a different key, so they never get to have the full key or any of the confidential data that may give them access to the full key.

## Final Thoughts

To ensure better protection of your online accounts, OAuth is the way to go because, unlike Basic Auth, it doesn’t give away your password. That’s because OAuth is more of an authorization framework. This keeps your credentials safe. 

Basic Auth, on the other hand, is an authentication protocol, which mainly focuses on proving that you're the correct person because you know things. This can leave your private information vulnerable, especially if your internet connection isn’t secured through TLS or you don’t set up MFA.

For further reading, check out our articles like [CIAM Security](https://squareball.co/blog/macro-trends-influencing-a-decision-for-ciam) and [Active Directory Account Management Best Practices](https://squareball.co/blog/active-directory-account-management-best-practices).