---
ms.localizationpriority: medium
---

<!-- markdownlint-disable MD041 -->

In this section you will extend the application from the previous exercise to support authentication with Azure AD. This is required to obtain the necessary OAuth access token to call the Microsoft Graph. In this step you will integrate the [Azure Identity client library for Python](https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/identity/azure-identity) into the application and configure authentication for the [Microsoft Graph Core Python Client Library (preview)](https://github.com/microsoftgraph/msgraph-sdk-python-core).

[!INCLUDE [preview-disclaimer](preview-disclaimer.md)]

The Azure Identity library provides a number of `TokenCredential` classes that implement OAuth2 token flows. The Microsoft Graph client library uses those classes to authenticate calls to Microsoft Graph. In this example, we'll use the following `TokenCredential` classes.

- `DeviceCodeCredential` implements the [device code flow](/azure/active-directory/develop/v2-oauth2-device-code) for user authentication.
- `ClientSecretCredential` implements the [client credentials flow](/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow) for app-only authentication. You will use this class in the optional app-only sections.

## Configure Graph client for user authentication

In this section you will use the `DeviceCodeCredential` class to request an access token by using the [device code flow](/azure/active-directory/develop/v2-oauth2-device-code).

1. Create a new file named **graph.py** and add the following code to that file.

    :::code language="python" source="./src/demo/graphtutorial/graph.py" id="UserAuthConfigSnippet":::

    This code declares two private properties, a `DeviceCodeCredential` object and a `GraphClient` object. The `initialize_graph_for_user_auth` function creates a new instance of `DeviceCodeCredential`, then uses that instance to create a new instance of `GraphClient`. Every time an API call is made to Microsoft Graph through the `user_client`, it will use the provided credential to get an access token.

1. Add the following function to **graph.py**.

    :::code language="python" source="./src/demo/graphtutorial/graph.py" id="GetUserTokenSnippet":::

1. Replace the empty `display_access_token` function in **main.py** with the following.

    :::code language="python" source="./src/demo/graphtutorial/main.py" id="DisplayAccessTokenSnippet":::

1. Build and run the app. Enter `1` when prompted for an option. The application displays a URL and device code.

    ```bash
    Python Graph Tutorial

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List my inbox
    3. Send mail
    4. List users (requires app-only)
    5. Make a Graph call
    1
    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and
    enter the code RB2RUD56D to authenticate.
    ```

1. Open a browser and browse to the URL displayed. Enter the provided code and sign in.

    [!INCLUDE [browser-auth-note](../shared/browser-auth-note.md)]

1. Once completed, return to the application to see the access token.

    [!INCLUDE [token-debug-tip](../shared/token-debug-tip.md)]
