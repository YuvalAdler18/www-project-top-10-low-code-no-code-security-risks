---

layout: col-sidebar
title: "LCNC-SEC-02: Authorization Misuse"

---

## Risk Rating [*](https://owasp.org/www-project-top-ten/2017/Note_About_Risks)

| Prevalence | Detectability | Exploitability | Technical Impact |
| --- | --- | --- | --- |
| 3 | 3 | 3 | 3 |

## The Gist

Applications and connections are first-class objects in most no-code/low-code platforms. Applications can be embedded with a developer account which is used implicity by any application user. Applications can also be shared with users who should not have access to their underlying data or even with the entire organization. This creates a direct path towards Privilege Escalation, allows an attacker to hide behind another user's identity, and circumvents traditional security controls.

## Description

No-code/low-code applications aren't built in silos. Their impact is based on integrations across the organization's stack through on-premise, SaaS, PaaS, and cloud platforms. Most no-code/low-code platforms come built-in with a large set of connectors, i.e., wrappers around APIs, which allow quick and easy connectivity. Connectors and user credentials form connections are first-class objects in most no-code/low-code platforms. This means that connections can be shared among applications, with other users, or with entire organizations. This creates a direct path to privilege escalation, where application users can gain access levels they should not typically have.

Many no-code/low-code platforms abuse OAuth authorization flows by querying and storing user refresh tokens and re-using them at will to increase productivity and reduce time-to-deliver. 
This allows business users to quickly set up connections without thinking about secrets or permissions; at the same time, No-code/low-code applications can take advantage of embedded user accounts rather than having their own application identity. Embedded identities can belong to the application creator, or they could be a common identity shared by teams, such as database credentials. They could also be service accounts or shared identities. 

An authorization scope controls the access to resources and assets of an organization. No-code/low-code platform application developers prefer broad authorization scope for their applications to make them as generic as possible. For organizations, this allows quick and easy setup of applications, which they can later use for other use cases without needing another application. The broad authorization scope does come at the cost of unnecessary risk of authorization misuse.

## Example Attack Scenarios

### Scenario #1

A developer creates a connection to their corporate email account.
They inadvertently click the "share with everyone" option, granting either usage permissions or full ownership.
Every user in the organization, including contractors and vendors, gains access to their corporate email account.
A malicious user triggers a "forgot password" flow and uses the connection to follow through with the process and gain control over the account.

### Scenario #2

A developer creates a simple application to view records from a database.
The application is configured to ensure each user can only view related records.
However, the application is configured in such a way that the underlying database connection is implicitly shared with its user.
An application user can use the database connection directly, gaining full access to all records.

### Scenario #3

Admin connects an application to their source code management system (e.g., Bitbucket) using a service or application account.
The provisioned service or application account has unrestricted access to all repositories to enable seamless integration.
Any internal user can abuse this connection to access restricted repositories they usually don't have access to.

### Scenario #4

A developer creates a simple application to submit forms from one platform to another.
The application, however, is configured to require authorization to edit and delete form submissions when just creating form submissions should have been enough.

## How to Prevent

- Disable or monitor the use of implicitly shared connections.
- Adhere to the principle of least privilege when providing access to environments that can contain shared connections to databases/services/SaaS.
- Monitor no-code/low-code platforms for over-shared connections.
- Educate business users on the risks of connection sharing and its relation to credential sharing.
- Explicitly refresh OAuth tokens on a regular basis by re-authenticating connections.
- Carefully review the scope an application requires and adhere to the principle of least privilege.
- Ensure applications use a single consistent identity across all their connections, rather than a different identity for each. Use a dedicated service or application account for those connections.
- Ensure a proper audit trail is maintained to identify the actor behind actions performed through the shared connection, whether those connections are shared by virtue of users using the application or by granting users access to that connection directly.

## References

- [Credential Sharing as a Service: The Hidden Risk of Low-Code/No-Code](https://www.darkreading.com/dr-tech/credential-sharing-as-a-service-hidden-risk-of-low-code-no-code)
- [Low-Code Platforms Are the New Holy Grail for Hackers](https://www.zenity.io/blog/why-are-low-code-platforms-becoming-the-new-holy-grail-of-cyberattackers/)
