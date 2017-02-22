---
title: "C#: Azure SQL Veritabanı&quot;nı kullanmaya başlama | Microsoft Docs"
description: "SQL ve C# uygulamalarını geliştirmek için SQL Database kullanmayı deneyin ve .NET için SQL Database Kitaplığı&quot;nı kullanarak C# ile bir Azure SQL Database oluşturun."
keywords: sql deneme, sql c#
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: cfff2299-a474-4054-8d99-759af1ae5188
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: dbf337a27c43fc6c91f1b061a1938c5471dd36a4
ms.openlocfilehash: bc1a78a2891c73df23bc2a57cec67e6b73414165


---
# <a name="use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>C# kullanarak .NET için SQL Veritabanı Kitaplığı ile bir SQL veritabanı oluşturma

[.NET için Microsoft Azure SQL Yönetim Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) ile bir Azure SQL veritabanı oluşturmak üzere C# dilini nasıl kullanacağınızı öğrenin. Bu makalede SQL ve C# ile tek bir veritabanını oluşturma işlemi açıklanmaktadır. Elastik havuz oluşturmak için bkz. [Elastik havuz oluşturma](sql-database-elastic-pool-manage-csharp.md).

.NET için Azure SQL Veritabanı Yönetim Kitaplığı [Resource Manager tabanlı SQL Veritabanı REST API'sini](https://msdn.microsoft.com/library/azure/mt163571.aspx) sarmalayan [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tabanlı bir API sağlar.

> [!NOTE]
> SQL Veritabanı’nın pek çok yeni özelliği, yalnızca [Azure Resource Manager dağıtım modeli](../azure-resource-manager/resource-group-overview.md) kullanıldığında desteklenir. Bu nedenle .NET için her zaman en son **Azure SQL Veritabanı Yönetim Kitaplığını ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet Paketi](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)) kullanmanız gerekir**. Eski [klasik dağıtım modeli tabanlı kitaplıklar](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql), yalnızca geriye dönük uyumluluk için desteklenir. Bu nedenle daha yeni Resource Manager tabanlı kitaplıkları kullanmanızı öneririz.
> 
> 

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* Azure aboneliği. Bir Azure aboneliğine ihtiyacınız varsa bu sayfanın üst kısmındaki **ÜCRETSİZ HESAP**'a tıklamanız yeterlidir; ardından makaleyi tamamlamak için geri dönün.
* Visual Studio. Ücretsiz bir Visual Studio kopyası için [Visual Studio İndirmeleri](https://www.visualstudio.com/downloads/download-visual-studio-vs) sayfasına göz atın.

> [!NOTE]
> Bu makalede, yeni ve boş bir SQL veritabanı oluşturulmuştur. Veritabanlarını kopyalamak, veritabanlarını ölçeklendirmek, havuzda bir veritabanı oluşturmak vb. için aşağıdaki örnekte *CreateOrUpdateDatabase(...)* yöntemini değiştirin. Daha fazla bilgi için bkz. [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) ve [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) sınıfları.
> 
> 

## <a name="create-a-console-app-and-install-the-required-libraries"></a>Bir konsol uygulaması oluşturun ve gerekli kitaplıkları yükleyin
1. Visual Studio’yu çalıştırın.
2. **Dosya** > **Yeni** > **Proje**’ye tıklayın.
3. Bir C# **Konsol Uygulaması** oluşturun ve şu şekilde adlandırın: *SqlDbConsoleApp*

C# ile bir SQL veritabanı oluşturmak için gerekli yönetim kitaplıklarını yükleyin ([paket yöneticisi konsolunu](http://docs.nuget.org/Consume/Package-Manager-Console) kullanarak):

1. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın.
2. En son [Microsoft Azure SQL Yönetim Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)’nı yüklemek için `Install-Package Microsoft.Azure.Management.Sql -Pre` yazın.
3. [Microsoft Azure Resource Manager Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)’nı yüklemek için `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` yazın.
4. [Microsoft Azure Sık Kullanılan Kimlik Doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication)’nı yüklemek için `Install-Package Microsoft.Azure.Common.Authentication -Pre` yazın. 

> [!NOTE]
> Bu makaledeki örneklerde, temel alınan hizmet üzerindeki REST çağrısı tamamlanana kadar her bir API isteğinin ve bloğunun zaman uyumlu bir biçimi kullanıldı. Zaman uyumsuz yöntemler de kullanılabilir.
> 
> 

## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Bir SQL Veritabanı sunucusu, güvenlik duvarı kuralı ve SQL veritabanı oluşturma - C# örneği
Aşağıdaki örnekte bir kaynak grubu, sunucu güvenlik duvarı kuralı ve bir SQL veritabanı oluşturulmuştur. `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` değişkenlerini almak için bkz. [Kaynaklara erişmek için hizmet sorumlusu oluşturma](#create-a-service-principal-to-access-resources).

**Program.cs** dosyasının içeriğini aşağıdakilerle değiştirin ve `{variables}` öğesini uygulama değerlerinizle güncelleştirin (`{}` öğesini dahil etmeyin).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Kaynaklara erişmek için hizmet sorumlusu oluşturma
Aşağıdaki PowerShell betiği Active Directory (AD) uygulamasını ve C# uygulamamızda kimlik doğrulamak için gereken hizmet sorumlusunu oluşturur. Betik önceki C# örneği için gereken değerleri çıkarır. Ayrıntılı bilgi için bkz. [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Sonraki adımlar
SQL Database'i ve C# ile bir veritabanını ayarlamayı denediğinize göre şu makaleler için hazırsınız:

* [SQL Server Management Studio ile SQL Veritabanı’na bağlanma ve bir örnek T-SQL sorgusu gerçekleştirme](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Ek Kaynaklar
* [SQL Veritabanı](https://azure.microsoft.com/documentation/services/sql-database/)
* [Veritabanı Sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)

<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png



<!--HONumber=Feb17_HO3-->


