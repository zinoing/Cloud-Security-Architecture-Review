![Diagram that shows a basic App Service architecture.](https://learn.microsoft.com/en-us/azure/architecture/web-apps/app-service/_images/basic-app-service-architecture-flow.svg)



### Workflow

The following workflow corresponds to the preceding diagram.

1. A user issues an HTTPS request to the App Service default domain on `azurewebsites.net`. This domain automatically points to the built-in public IP address of your App Service application. The transport layer security (TLS) connection is established from the client directly to App Service. Azure fully manages the certificate.
2. Easy Auth, which is a feature of App Service, ensures that the user who accesses the site is authenticated by using Microsoft Entra ID.
3. Your application code deployed to App Service handles the request. For example, that code might connect to an Azure SQL Database instance by using a connection string that's configured in App Service as an app setting.
4. The information about the original request to App Service and the call to SQL Database is logged in Application Insights.
