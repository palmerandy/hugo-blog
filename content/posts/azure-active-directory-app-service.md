---
title: "OAuth with Azure Active Directory to a Function App"
date: 2019-01-31T10:07:11+11:00
draft: false
tags: ["azure", "functions", "OAuth", "authentication and authorization", "EasyAuth"]
---

This article is about adding Azure Active Directory to a Function App.  The same principles could be applied to an Azure Web App as this concept is for any [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/overview-authentication-authorization). 

In addition to securing a Function App via OAuth, I have the requirement that the secured Function App be called from another Function App.  This means the calling Function App will need to run as an application identity, instead of as a user's identity.

### Why aren't you simply using Authorization Keys?

According to [Microsoft's docs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook#authorization-keys):

> While keys may help obfuscate your HTTP endpoints during development, they are not intended as a way to secure an HTTP trigger in production. To learn more, see [Secure an HTTP endpoint in production](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook#secure-an-http-endpoint-in-production).

Specifically, this article details the first option in the [Secure an HTTP endpoint in production](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook#secure-an-http-endpoint-in-production) link.  This blog post attempts to simplify some of the documentation Microsoft has provided.

### Set up in the Azure Portal

1. In the [Azure Portal](https://portal.azure.com/) go to the 'Platform Features' tab of your Function App. 

![Alt text](/images/azure-active-directory-auth-1-app-service-auth.png "'Authentication / Authorization' link")

2. Click the 'Authentication / Authorization' link (in the 'Networking' section)

    a. 'App Service Authentication': 'On'

    b. 'Action to take when request is not authenticated': 'Log in with Azure Active Directory' 

    c. 'Authentication Provides': 'Azure Active Directory'

        i. 'Management Mode': 'Express'

        ii. Leave the defaults and click 'OK'

![Alt text](/images/azure-active-directory-auth-2-auth.png "'Authentication / Authorization'")

![Alt text](/images/azure-active-directory-auth-2-express.png "Express")

3. In the portal go the 'App Registractions (Preview)' tab of 'Azure active Directory'
4. From the 'Own applications' tab select the application that you just created - this should have the same name as the App Service.
5. Click the 'Certificates & Secrets' tab
6. Click 'New client Secret' button:

    a. Give the 'Description' a meaningful name

    b. Choose your expiry.  

    c. Click Add

    d. Note down the new client secret value, as it is only shown until you leave this blade in the Portal.  I recommend story this directly into Azure KeyVault for future use.

![Alt text](/images/azure-active-directory-auth-6-secret.png "Secret")

### Testing with a User Identiy

In your browser, when you navigate to the Function App's URL* you should be re-directed to login via Azure Active Directory.  After authenticating should be re-directed to the homepage which tells you the Function App is up and running.

*You can get your Function App url from the Azure portal, by going to your Function App and clicking the URL link.  

### Testing with the Client Secret

We can now peform a FormUrlEncoded HTTP Post to https://login.microsoftonline.com/{AzureActiveDirectoryTenantId}/oauth2/token to get a bearer token:
{{< gist palmerandy aa19504cec56a215564a41ea66b733e6 >}}

From here you can wrap the bearer token in a AuthenticationHeaderValue and add it to the Authorization property of your HttpClient's DefaultRequestHeaders:
```
var token = 
await _azureAppServiceAuthenticator.GetBearerToken();            
var header = new AuthenticationHeaderValue("Bearer", token);
HttpClient.DefaultRequestHeaders.Authorization = header;
```