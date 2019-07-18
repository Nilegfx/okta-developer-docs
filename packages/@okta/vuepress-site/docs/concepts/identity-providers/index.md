---
title: External Identity Providers
---
# External Identity Providers
As a developer building a custom application, you want to give your users the freedom to choose which Identity Provider that they use to sign in to your application. But first you should understand how various Identity Providers connect to Okta. 

What is an Identity Provider? It is a service that creates and maintains identity information and then provides authentication services to your applications. Identity Providers can significantly reduce sign-in and registration friction, which allows your users to access applications with one click and without needing to create new passwords or remember usernames. 

## The big picture
Okta manages connections to other Identity Providers for your application and sits between your application and the Identity Provider that authenticates your users. 

* When you use Okta as the user store for your applications, users can sign in with their email and password by default. See our guides for how to sign in users to your [web](/docs/guides/sign-into-web-app/), [mobile](/docs/guides/sign-into-mobile-app/), and [single-page](/docs/guides/sign-into-spa/) apps.

* You can add connections to Identity Providers like Twitter or Facebook. This is called social login or social authentication. It allows your users to sign in to your app using credentials from their existing accounts with those Identity Providers. After users authenticate, you sync their accounts into your Okta Universal Directory while continuing to use that Identity Provider for user authentication. This eliminates the need to store an additional username and password for that user. 

* You can add connections to Identity Providers that you build in-house that support OpenID Connect or SAML protocols, also referred to as inbound federation or inbound SAML. The SAML flow is initiated with the service provider (in this case, Okta) that redirects the user to the Identity Provider for authentication. After authentication, a user is created inside Okta, and the user is redirected back to your application along with an ID token. This allows you to use Okta to proxy between SAML-only Identity Providers and OIDC-only applications that normally are incompatible. 

> Note: Social and OpenID Connect Identity Providers store access tokens that allow subsequent calls to Identity Providers after the user is authorized. For example, the token may contain the permission to add events to a user's Google calendar. After the user is authorized, the token can be used on additional calls to add events to a user's Google calendar on the user's behalf. 

* You can also configure federation between Okta orgs using OpenID Connect or SAML. 

Adding any of these Identity Providers allows users to sign in to your application using their credentials from a specific Identity Provider. 

## Benefits of using Okta to manage Identity Providers
You could connect your application directly to an Identity Provider (for example, using an SDK to add a button). However, using Okta as the user store for your application and letting Okta manage the Identity Provider connections has some benefits:

* You don't have to write any custom code. Your application only needs to talk to Okta, and Okta does the rest. 

* There's one protocol. Your application uses OpenID Connect to talk to Okta. Okta handles whatever protocols the other Identity Providers use, and this is transparent to your application.

* All users are stored in Okta (a single user store). You can capture the profile attributes from an Identity Provider user and store those attributes in Okta's Universal Directory.

* If a user updates their profile at the Identity Provider, those changes can be reflected inside Okta the next time that they use the provider to sign in. This is called profile sync.

* Users can use multiple Identity Providers to sign in, and Okta links those profiles to a single Okta user, which is called account linking.

* Okta supports group sync between Okta and enterprise SAML Identity Providers. This allows you to specify the groups that users should be added to. 

## How Okta connects to external Identity Providers
Okta sits between your application and the external provider. Your application only needs to talk to Okta, and Okta does the rest.

### Sign-in process
The sign in process starts at the `/authorize` endpoint, and then goes out to the provider and back:

1. In your application, the user clicks a button similar to: **Sign in with (Identity Provider)**.
2. Your application redirects the browser to Okta.
3. Okta redirects the browser to the Identity Provider.
4. The user is prompted to sign in at the Identity Provider (if they aren't already) and to accept the permissions required by your app.
5. The Identity Provider redirects the browser back to Okta.
6. Okta processes the sign-in request and adds the user to your Okta organization's Universal Directory.
7. Okta redirects the browser back to your application, just like any other sign-in request.

![Social Login Flow width:](/img/social_login_flow.png "Social Login Flow width:")

## Account Linking and Just-in-Time Provisioning
When you allow your users to sign in to your app using their choice of Identity Providers, you can use Account Linking to help create a unified view of your users within your org. Additionally, you can use Just in Time (JIT) provisioning to create a seamless experience for users that sign-in to your application for the first time using their credentials from another Identity Provider.

Users can use multiple Identity Providers to sign in, and Okta can link all of those profiles to a single Okta user. This is called Account Linking. If, for example, a user signs in to your app using a different Identity Provider than they used for registration, Account Linking can establish that the user owns both identities, allowing the user to sign in from either account.

If a user signs in to your application for the first time using another Identity Provider, you can implement Just in Time (JIT) provisioning to automatically create an Okta account for them. JIT account creation and activation only works for end users who aren't already Okta users. (JIT does update the accounts of existing users during full imports.)

Additionally, you can apply granular control over Account Linking and JIT by defining a policy and then rules for the policy. You can base a policy on a variety of factors, such as location, group definitions, and authentication type. A specific policy rule can then be created for groups that have been assigned to your application. You can create multiple policies with more or less restrictive rules and apply them to different groups.

You can also set up group sync to specify the groups that users should be added to and enable profile mastering so that a user's profile gets updated each time that the user signs in. This allows you to always have the most recent data about your users. 

## See also
See the following guides for information on how to set up various IdPs:

* [Facebook](/docs/guides/sign-in-with-facebook/)
* [Google](/docs/guides/sign-in-with-google/)
* [LinkedIn](/docs/guides/sign-in-with-linkedin/)
* [Microsoft](/docs/guides/sign-in-with-microsoft/)
* [Generic OIDC Identity Provider](/docs/guides/federate-with-oidc/overview/)
* [SAML Identity Provider](/docs/guides/add-saml-idp/)