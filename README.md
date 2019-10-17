# Getting .NET CORE (3.0) and Sharepoint CSOM to Play Nice.
Getting .NET Core and SharePoint CSOM to Play Nice.

This was initially hosted on Raju Joseph's site. However it is not longer available.
http://rajujoseph.com/getting-net-core-and-sharepoint-csom-play-nice/

Also - if you need to deal with Interop objects such as office/excel/etc...

go [here](https://github.com/dotnet/samples/tree/master/core/extensions/ExcelDemo)

Anyways, I stumbled across this when developing a .NET CORE 3.0 WPF.

#### Instructions ####

 1. Download the CSOM library from windows.
    1. See Table below for SDK Versions
 2. Once SDK is downloaded, go to your solution/project and add reference
    1. browse to the following directory. Note path below is for 2016
    2. C:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI
 3. Ensure the following references are removed first.
 
     | DLL |
     | ------------- |
     | Microsoft.SharePoint.Client.dll |
     | Microsoft.SharePoint.Client.Runtime.dll |
  
 4. Add the following references
 
     | DLL |
     | ------------- |
     | Microsoft.SharePoint.Client.Portable.dll |
     | Microsoft.SharePoint.Client.Runtime.Portable.dll |
     | Microsoft.SharePoint.Client.Runtime.Windows.dll |

### Sample Code (.net CORE 3.0) ### 
_Without Authentication_
```
using Microsoft.SharePoint.Client;

    public void SampleSp()
    {
        // sample function for demo purposes.
        // please break this into proper OOP when implementing.
        
        ClientContext context = new ClientContext(siteUrl);
        Web site = context.Web;

        ListCollection spListCol = site.Lists;

        context.Load(spListCol);
        context.ExecuteQueryAsync().Wait();

        List spList = spListCol.GetByTitle("samplelist");
        context.Load(spList);

        ListItemCollection listAppointments = null;

        CamlQuery Query = new CamlQuery();
        Query.ViewXml = @"<View><Query> </Query></View>";

        listAppointments = spList.GetItems(Query);
        context.Load(listAppointments);

        context.ExecuteQueryAsync().Wait();
    }


```

_With Authentication_
```
using Microsoft.SharePoint.Client;

    public void SampleSp()
    {
        // sample function for demo purposes.
        // please break this into proper OOP when implementing.

        string username = "#####@#####.onmicrosoft.com";
        string password = "######";
        string siteUrl = "https://#####.sharepoint.com/sites/#####";

        ClientContext context = new ClientContext(siteUrl);
        Web site = context.Web;

        context.Credentials = new SharePointOnlineCredentials(username, password);

        ListCollection spListCol = site.Lists;

        context.Load(spListCol);
        context.ExecuteQueryAsync().Wait();

        List spList = spListCol.GetByTitle("samplelist");
        context.Load(spList);

        ListItemCollection listAppointments = null;

        CamlQuery Query = new CamlQuery();
        Query.ViewXml = @"<View><Query> </Query></View>";

        listAppointments = spList.GetItems(Query);
        context.Load(listAppointments);

        context.ExecuteQueryAsync().Wait();
    }


```
    
#### Sharepoint SDK (DLLs) ####    
Version  | Link
------------- | -------------
2016 | https://www.microsoft.com/en-ca/download/details.aspx?id=51679
2013 | https://www.microsoft.com/en-ca/download/details.aspx?id=35585

Cheers, I hope this helps some people out until Microsoft Supports .NET 3.0 CSOM
