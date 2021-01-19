# Authenticating-to-and-querying-Microsoft-Graph-by-using-MSAL-and-.NET-SDKs
AZ-204_LAB06


Exercise 1: Create an Azure Active Directory (Azure AD) application registration
Task 1: Open the Azure portal
On the taskbar, select the Microsoft Edge icon.

In the open browser window, go to the Azure portal (https://portal.azure.com).

Enter the email address for your Microsoft account, and then select Next.

Enter the password for your Microsoft account, and then select Sign in.

Note: If this is your first time signing in to the Azure portal, you’ll be offered a tour of the portal. Select Get Started to skip the tour and begin using the portal.

Task 2: Create an application registration
In the Azure portal’s navigation pane, select All services.

From the All services blade, select Azure Active Directory.

From the Azure Active Directory blade, select App registrations in the Manage section.

In the App registrations section, select New registration.

In the Register an application section, perform the following actions:

In the Name text box, enter graphapp.

In the Supported account types list, select the Accounts in this organizational directory only (Default Directory only - Single tenant) check box.

In the Redirect URI drop-down list, select Public client/native (mobile & desktop).

In the Redirect URI text box, enter http://localhost.

Select Register.

Task 3: Enable the default client type
In the graphapp application registration blade, select Authentication in the Manage section.

In the Authentication section, perform the following actions:

In the Default client type subsection, select Yes.

Select Save.

Task 4: Record unique identifiers
On the graphapp application registration blade, select Overview.

In the Overview section, find and record the value of the Application (client) ID text box. You’ll use this value later in the lab.

In the Overview section, find and record the value of the Directory (tenant) ID text box. You’ll use this value later in the lab.

Review
In this exercise, you created a new application registration and recorded important values that you’ll need later in the lab.

Exercise 2: Obtain a token by using the MSAL.NET library
Task 1: Create a .NET project
On the Start screen, select the Visual Studio Code tile.

On the File menu, select Open Folder.

In the File Explorer window that opens, browse to Allfiles (F):\Allfiles\Labs\06\Starter\GraphClient, and then select Select Folder.

In the Visual Studio Code window, right-click or activate the shortcut menu for the Explorer pane, and then select Open in Terminal.

At the open command prompt, enter the following command, and then select Enter to create a new .NET project named GraphClient in the current folder:

Code
dotnet new console --name GraphClient --output .
Note: The dotnet new command will create a new console project in a folder with the same name as the project.

At the command prompt, enter the following command, and then select Enter to import version 4.7.1 of Microsoft.Identity.Client from NuGet:

Code
dotnet add package Microsoft.Identity.Client --version 4.7.1
Note: The dotnet add package command will add the Microsoft.Identity.Client package from NuGet. For more information, go to Microsoft.Identity.Client.

At the command prompt, enter the following command, and then select Enter to build the .NET web application:

Code
dotnet build
Select Kill Terminal or the Recycle Bin icon to close the currently open terminal and any associated processes.

Task 2: Modify the Program class
In the Explorer pane of the Visual Studio Code window, open the Program.cs file.

On the code editor tab for the Program.cs file, delete all the code in the existing file.

Add the following line of code to import the Microsoft.Identity.Client namespace from the Microsoft.Identity.Client package imported from NuGet:

Code
using Microsoft.Identity.Client;
Add the following lines of code to add using directives for the built-in namespaces that will be used in this file:

Code
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
Enter the following code to create a new Program class:

Code
public class Program
{
}
In the Program class, enter the following code to create a new asynchronous Main method:

Code
public static async Task Main(string[] args)
{
}
In the Program class, enter the following line of code to create a new string constant named _clientId:

Code
private const string _clientId = "";
Update the _clientId string constant by setting its value to the Application (client) ID that you recorded earlier in this lab.

In the Program class, enter the following line of code to create a new string constant named _tenantId:

Code
private const string _tenantId = "";
Update the _tenantId string constant by setting its value to the Directory (tenant) ID that you recorded earlier in this lab.

Observe the Program.cs file, which should now include:

Code
using Microsoft.Identity.Client;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

public class Program
{
    private const string _clientId = "<app-reg-client-id>";        
    private const string _tenantId = "<aad-tenant-id>";

    public static async Task Main(string[] args)
    {
    }
}
Task 3: Obtain a Microsoft Authentication Library (MSAL) token
In the Main method, add the following line of code to create a new variable named app of type IPublicClientApplication:

Code
IPublicClientApplication app;
In the Main method, perform the following actions to build a public client application instance by using the static PublicClientApplicationBuilder class, and then store it in the app variable:

Add the following line of code to access the static PublicClientApplicationBuilder class:

Code
PublicClientApplicationBuilder
Update the previous line of code by adding another line of code to use the Create() method of the PublicClientApplicationBuilder class, passing in the _clientId variable as a parameter:

Code
PublicClientApplicationBuilder
    .Create(_clientId)
Update the previous line of code by adding another line of code to use the WithAuthority() method of the base AbstractApplicationBuilder<> class, passing in the enumeration value AzureCloudInstance.AzurePublic and the _tenantId variable as parameters:

Code
PublicClientApplicationBuilder
    .Create(_clientId)
    .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
Update the previous line of code by adding another line of code to use the WithRedirectUri() method of the base AbstractApplicationBuilder<> class, passing in a string value of http://localhost:

Code
PublicClientApplicationBuilder
    .Create(_clientId)
    .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
    .WithRedirectUri("http://localhost")
Update the previous line of code by adding another line of code to use the Build() method of the PublicClientApplication class:

Code
PublicClientApplicationBuilder
    .Create(_clientId)
    .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
    .WithRedirectUri("http://localhost")
    .Build();
Update the previous line of code by adding more code to store the result of the expression in the app variable:

Code
app = PublicClientApplicationBuilder
    .Create(_clientId)
    .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
    .WithRedirectUri("http://localhost")
    .Build();
In the Main method, add the following line of code to create a new generic string List<> with a single value of user.read:

Code
List<string> scopes = new List<string> 
{ 
    "user.read" 
};
In the Main method, add the following line of code to create a new variable named result of type AuthenticationResult:

Code
AuthenticationResult result;
In the Main method, perform the following actions to acquire a token interactively and store the output in the result variable:

Add the following line of code to access the app variable:

Code
app
Update the previous line of code by adding another line of code to use the AcquireTokenInteractive() method of the IPublicClientApplicationBuilder interface, passing in the scopes variable as a parameter:

Code
app
    .AcquireTokenInteractive(scopes)
Update the previous line of code by adding another line of code to use the ExecuteAsync() method of the AbstractAcquireTokenParameterBuilder class:

Code
app
    .AcquireTokenInteractive(scopes)
    .ExecuteAsync();
Update the previous line of code by adding more code to process the expression asynchronously by using the await keyword:

Code
await app
    .AcquireTokenInteractive(scopes)
    .ExecuteAsync();
Update the previous line of code by adding more code to store the result of the expression in the result variable:

Code
result = await app
    .AcquireTokenInteractive(scopes)
    .ExecuteAsync();
In the Main method, add the following line of code to use the Console.WriteLine method to render the value of the AuthenticationResult.AccessToken member to the console:

Code
Console.WriteLine($"Token:\t{result.AccessToken}");
Observe the Main method, which should now include:

Code
public static async Task Main(string[] args)
{
    IPublicClientApplication app;

    app = PublicClientApplicationBuilder
        .Create(_clientId)
        .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
        .WithRedirectUri("http://localhost")
        .Build();

    List<string> scopes = new List<string> 
    { 
        "user.read"
    };

    AuthenticationResult result;
        
    result = await app
        .AcquireTokenInteractive(scopes)
        .ExecuteAsync();

    Console.WriteLine($"Token:\t{result.AccessToken}");
}
Save the Program.cs file.

Task 4: Test the updated application
In the Visual Studio Code window, right-click or activate the shortcut menu for the Explorer pane, and then select Open in Terminal.

At the open command prompt, enter the following command, and then select Enter to run the .NET web application:

Code
dotnet run
Note: If there are any build errors, review the Program.cs file in the Allfiles (F):\Allfiles\Labs\06\Solution\GraphClient folder.

The running console application will automatically open an instance of the default browser.

In the open browser window, perform the following actions:

Enter the email address for your Microsoft account, and then select Next.

Enter the password for your Microsoft account, and then select Sign in.

Note: You might have the option to select an existing Microsoft account as opposed to signing in again.

The browser window will automatically open the Permissions requested webpage. On this webpage, perform the following actions:

Review the requested permissions.

Select Accept.

Return to the currently running Visual Studio Code application.

Observe the token rendered in the output from the currently running console application.

Select Kill Terminal or the Recycle Bin icon to close the currently open terminal and any associated processes.

Review
In this exercise, you acquired a token from the Microsoft identity platform by using the MSAL.NET library.

Exercise 3: Query Microsoft Graph by using the .NET SDK
Task 1: Import the Microsoft Graph SDK from NuGet
In the Visual Studio Code window, right-click or activate the shortcut menu for the Explorer pane, and then select Open in Terminal.

At the command prompt, enter the following command, and then select Enter to import version 1.21.0 of Microsoft.Graph from NuGet:

Code
dotnet add package Microsoft.Graph --version 1.21.0
Note: The dotnet add package command will add the Microsoft.Graph package from NuGet. For more information, go to Microsoft.Graph.

At the command prompt, enter the following command, and then select Enter to import version 1.0.0-preview.2 of Microsoft.Graph.Auth from NuGet:

Code
dotnet add package Microsoft.Graph.Auth --version 1.0.0-preview.2
Note: The dotnet add package command will add the Microsoft.Graph.Auth package from NuGet. For more information, go to Microsoft.Graph.Auth.

At the command prompt, enter the following command, and then select Enter to build the .NET web application:

Code
dotnet build
Select Kill Terminal or the Recycle Bin icon to close the currently open terminal and any associated processes.

Task 2: Modify the Program class
In the Explorer pane of the Visual Studio Code window, open the Program.cs file.

On the code editor tab for the Program.cs file, add the following line of code to import the Microsoft.Graph namespace from the Microsoft.Graph package imported from NuGet:

Code
using Microsoft.Graph;
Add the following line of code to import the Microsoft.Graph.Auth namespace from the Microsoft.Graph.Auth package imported from NuGet:

Code
using Microsoft.Graph.Auth;
Observe the Program.cs file, which should now include the following using directives:

Code
using Microsoft.Graph;    
using Microsoft.Graph.Auth;
using Microsoft.Identity.Client;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
Task 3: Use the Microsoft Graph SDK to query user profile information
Within the Main method, perform the following actions to remove unnecessary code:

Delete the following line of code:

Code
AuthenticationResult result;
Delete the following block of code:

Code
result = await app
        .AcquireTokenInteractive(scopes)
        .ExecuteAsync();
Delete the following line of code:

Code
Console.WriteLine($"Token:\t{result.AccessToken}");
Observe the Main method, which should now include:

Code
public static async Task Main(string[] args)
{
    IPublicClientApplication app;

    app = PublicClientApplicationBuilder
        .Create(_clientId)
        .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
        .WithRedirectUri("http://localhost")
        .Build();

    List<string> scopes = new List<string> 
    { 
        "user.read"
    };
}
In the Main method, add the following line of code to create a new variable named provider of type DeviceCodeProvider that passes in the variables app and scopes as constructor parameters:

Code
DeviceCodeProvider provider = new DeviceCodeProvider(app, scopes);
In the Main method, add the following line of code to create a new variable named client of type GraphServiceClient that passes in the variable provider as a constructor parameter:

Code
GraphServiceClient client = new GraphServiceClient(provider);
In the Main method, perform the following actions to use the GraphServiceClient instance to asynchronously get the response of issuing an HTTP request to the relative /Me directory of the REST API:

Add the following line of code to get the Me property of the client variable:

Code
client.Me
Update the previous line of code by adding another line of code to get an object representing the HTTP request by using the Request() method:

Code
client.Me
    .Request()
Update the previous line of code by adding another line of code to issue the request asynchronously by using the GetAsync() method:

Code
client.Me
    .Request()
    .GetAsync()
Update the previous line of code by adding more code to process the expression asynchronously by using the await keyword:

Code
await client.Me
    .Request()
    .GetAsync()
Update the previous line of code by adding more code to store the result of the expression in a new variable named myProfile of type User:

Code
User myProfile = await client.Me
    .Request()
    .GetAsync();
In the Main method, add the following line of code to use the Console.WriteLine method to render the value of the User.DisplayName member to the console:

Code
Console.WriteLine($"Name:\t{myProfile.DisplayName}");
In the Main method, add the following line of code to use the Console.WriteLine method to render the value of the User.Id member to the console:

Code
Console.WriteLine($"AAD Id:\t{myProfile.Id}");
Observe the Main method, which should now include:

Code
public static async Task Main(string[] args)
{
    IPublicClientApplication app;

    app = PublicClientApplicationBuilder
        .Create(_clientId)
        .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
        .WithRedirectUri("http://localhost")
        .Build();

    List<string> scopes = new List<string> 
    { 
        "user.read" 
    };

    DeviceCodeProvider provider = new DeviceCodeProvider(app, scopes);

    GraphServiceClient client = new GraphServiceClient(provider);

    User myProfile = await client.Me
        .Request()
        .GetAsync();

    Console.WriteLine($"Name:\t{myProfile.DisplayName}");
    Console.WriteLine($"AAD Id:\t{myProfile.Id}");
}
Save the Program.cs file.

Task 4: Test the updated application
In the Visual Studio Code window, right-click or activate the shortcut menu for the Explorer pane, and then select Open in Terminal.

At the open command prompt, enter the following command, and then select Enter to run the .NET web application:

Code
dotnet run
Note: If there are any build errors, review the Program.cs file in the Allfiles (F):\Allfiles\Labs\06\Solution\GraphClient folder.

Observe the message in the output from the currently running console application. Record the value of the code in the message. You’ll use this value later in the lab.

On the taskbar, select the Microsoft Edge icon.

In the open browser window, go to https://microsoft.com/devicelogin.

On the Enter code webpage, perform the following actions:

In the Code text box, enter the value of the code that you copied earlier in the lab.

Select Next.

On the login webpage, perform the following actions:

Enter the email address for your Microsoft account, and then select Next.

Enter the password for your Microsoft account, and then select Sign in.

Note: You might have the option to select an existing Microsoft account as opposed to signing in again.

Return to the currently running Visual Studio Code application.

Observe the output from the Microsoft Graph request in the currently running console application.

Select Kill Terminal or the Recycle Bin icon to close the currently open terminal and any associated processes.

Review
In this exercise, you queried Microsoft Graph by using the SDK and MSAL-based authentication.

Exercise 4: Clean up your subscription
Task 1: Delete the application registration in Azure AD
Return to the browser window with the Azure portal.

In the Azure portal’s navigation pane, select All services.

From the All services blade, select Azure Active Directory.

From the Azure Active Directory blade, select App registrations in the Manage section.

In the App registrations section, select the graphapp Azure AD application registration that you created earlier in this lab.

In the graphapp section, perform the following actions:

Select Delete.

In the confirmation pop-up dialog box, select Yes.

Task 2: Close the active applications
Close the currently running Microsoft Edge application.

Close the currently running Visual Studio Code application.
