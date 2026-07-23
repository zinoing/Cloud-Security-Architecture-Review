![Architecture diagram that shows a simple enterprise integration.](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/images/simple-enterprise-integration.svg)	



#### Data flow

The following data flow corresponds to the previous diagram:

1. The application is a client that calls the API gateway after it authenticates with Microsoft Entra ID. The application can be a web app, mobile app, or any other client that makes HTTP requests.
2. Microsoft Entra ID authenticates the client application. The client application obtains an access token from Microsoft Entra ID and includes it in the request to the API gateway.
3. API Management consists of two related components:

   - The API gateway accepts HTTP calls from the client application, validates the token from Microsoft Entra ID, and forwards the request to the back-end service. The API gateway can also transform requests and responses, and cache responses.
   - Developers use the developer portal to discover and interact with the APIs. You can customize the developer portal to match your organization's branding.
4. Logic apps orchestrate the calls to the back-end services. Various events can trigger logic apps, and logic apps can call various services. In this architecture, Logic Apps calls the back-end services and provides connectivity through connectors, which reduces the need for custom code.
5. The back-end services can be any service or line-of-business (LOB) application, like a database, web service, or SaaS application. They can be hosted in Azure or on-premises.
