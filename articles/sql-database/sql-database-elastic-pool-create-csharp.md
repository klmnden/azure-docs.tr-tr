<properties
    pageTitle="C# ile esnek veritabanı havuzu oluşturma | Microsoft Azure"
    description="Kaynakları birçok veritabanı arasında paylaşabilmek üzere Azure SQL Database'de ölçeklendirilebilir bir esnek veritabanı havuzu oluşturmak için C# veritabanı geliştirme tekniklerini kullanın."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="07/22/2016"
    ms.author="sstein"/>

# C &#x23; ile yeni bir esnek veritabanı havuzu oluşturma

> [AZURE.SELECTOR]
- [Azure portalı](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


C# kullanarak [esnek veritabanı havuzu](sql-database-elastic-pool.md) oluşturmayı öğrenin. 

Genel hata kodları için bkz. [SQL Database istemci uygulamaları için SQL hata kodları: Veritabanı bağlantı hatası ve diğer sorunlar](sql-database-develop-error-messages.md).

Aşağıdaki örneklerde [.NET için SQL Veritabanı Kitaplığı](https://msdn.microsoft.com/library/azure/mt349017.aspx) kullanılmaktadır; bu nedenle henüz yüklenmemişse devam etmeden önce bu kitaplığı yüklemeniz gerekir. Visual Studio'da bulunan [paket yöneticisi konsolunda](http://docs.nuget.org/Consume/Package-Manager-Console) (**Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**) aşağıdaki komutu çalıştırarak bu kitaplığı yükleyebilirsiniz:

    Install-Package Microsoft.Azure.Management.Sql –Pre

## Yeni bir havuz oluşturma

[Azure Active Directory](sql-database-client-id-keys.md) değerlerini kullanarak bir [SqlManagementClient](https://msdn.microsoft.com/library/microsoft.azure.management.sql.sqlmanagementclient) örneği oluşturun. Bir [ElasticPoolCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.elasticpoolcreateorupdateparameters) örneği oluşturun ve [CreateOrUpdate](https://msdn.microsoft.com/library/microsoft.azure.management.sql.databaseoperationsextensions.createorupdate) yöntemini.çağırın. Havuz başına eDTU değerlerinin yanı sıra minimum ve maksimum DTU değerleri, hizmet katmanı değerine (temel, standart veya premium) göre kısıtlanır. Bkz. [Esnek havuzlar ve esnek veritabanları için eDTU ve depolama sınırları](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).


    ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = "South Central US",
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = "Standard",
            Dtu = 400,
            DatabaseDtuMin = 0,
            DatabaseDtuMax = 100
         }
    };

    // Create the pool
    var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);

## Havuzda yeni bir veritabanı oluşturma

Bir [DataBaseCreateorUpdateProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreateorupdateproperties) örneği oluşturun ve yeni veritabanının özelliklerini ayarlayın. Ardından kaynak grubu, sunucu adı ve yeni veritabanı adı ile CreateOrUpdate yöntemini çağırın.

    // Create a database: configure create or update parameters and properties explicitly
    DatabaseCreateOrUpdateParameters newPooledDatabaseParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentServer.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = 268435456000, // 250 GB,
            Collation = "SQL_Latin1_General_CP1_CI_AS"
        }
    };

    var poolDbResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database2", newPooledDatabaseParameters);

Var olan bir veritabanını bir havuza taşımak için bkz. [Veritabanını bir esnek havuza taşıma](sql-database-elastic-pool-manage-csharp.md#Move-a-database-into-an-elastic-pool).

## Örnek: C&#x23; kullanarak havuz oluşturma

Bu örnekte yeni bir Azure kaynak grubu, yeni bir Azure SQL Server örneği ve yeni bir esnek havuz oluşturulacak. 
 

Bu örneğin çalıştırılabilmesi için aşağıdaki kitaplıklar gereklidir. Visual Studio'da bulunan [paket yöneticisi konsolunda](http://docs.nuget.org/Consume/Package-Manager-Console) (**Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**) aşağıdaki komutları çalıştırarak yükleyebilirsiniz:

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.ResourceManager –Pre -Version 1.1.1-preview
    Install-Package Microsoft.Azure.Common.Authentication –Pre

Bir konsol uygulaması oluşturun ve Program.cs içeriğini aşağıdakiyle değiştirin. Gerekli istemci kimliğini ve ilgili değerleri almak için bkz. [Uygulamanızı SQL Database'e bağlamak için kaydetme ve gerekli istemci değerlerini alma](sql-database-client-id-keys.md). SubscriptionId değerini almak için [Get-AzureRmSubscription](https://msdn.microsoft.com/library/mt619284.aspx) cmdlet'ini kullanın.

    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbElasticPoolsSample
    {
    class Program
    {
        // authentication variables
        static string subscriptionId = "<your Azure subscription>";
        static string clientId = "<your client id>";
        static string redirectUri = "<your redirect URI>";
        static string domainName = "<i.e. microsoft.onmicrosoft.com>";
        static AuthenticationResult token;

        // resource group variables
        static string datacenterLocation = "<location>";
        static string resourceGroupName = "<resource group name>";

        // server variables
        static string serverName = "<server name>";
        static string adminLogin = "<server admin>";
        static string adminPassword = "<server password (store it securely!)>";
        static string serverVersion = "12.0";

        // pool variables
        static string elasticPoolName = "<pool name>";
        static string poolEdition = "Standard";
        static int poolDtus = 400;
        static int databaseMinDtus = 0;
        static int databaseMaxDtus = 100;


        static void Main(string[] args)
        {
            // Sign in to Azure
            token = GetAccessToken();
            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            // Create a resource group
            ResourceGroup rg = CreateResourceGroup();
            Console.WriteLine(rg.Name + " created.");

            // Create a server
            Console.WriteLine("Creating server... ");
            ServerGetResponse srvr = CreateServer();
            Console.WriteLine("Creation of server " + srvr.Server.Name + ": " + srvr.StatusCode.ToString());

            // Create a pool
            Console.WriteLine("Creating elastic database pool... ");
            ElasticPoolCreateOrUpdateResponse epool = CreateElasticDatabasePool();
            Console.WriteLine("Creation of pool " + epool.ElasticPool.Name + ": " + epool.Status.ToString());


            // keep the console open until Enter is pressed!
            Console.WriteLine("Press Enter to exit.");
            Console.ReadLine();
        }


        static ElasticPoolCreateOrUpdateResponse CreateElasticDatabasePool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus, 
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, elasticPoolName, newPoolParameters);
            return newPoolResponse;
        }



        static ServerGetResponse CreateServer()
        {

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = adminLogin,
                    AdministratorLoginPassword = adminPassword,
                    Version = serverVersion
                }
            };

            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;

        }


        static ResourceGroup CreateResourceGroup()
        {
           
            Microsoft.Rest.TokenCredentials creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);
            // Create a resource management client 
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = datacenterLocation,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }


        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }
    }
    }

  

## Sonraki adımlar

- [Havuzunuzu yönetme](sql-database-elastic-pool-manage-csharp.md)
- [Esnek iş oluşturma](sql-database-elastic-jobs-overview.md): Esnek işler, havuzda bulunan herhangi bir sayıdaki veritabanı için T-SQL betiklerini kullanmanızı sağlar.
- [Azure SQL Database ile ölçek genişletme](sql-database-elastic-scale-introduction.md): Ölçek genişletmek için esnek veritabanı araçlarını kullanın.

## Ek Kaynaklar

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Resource Management API'leri](https://msdn.microsoft.com/library/azure/dn948464.aspx)



<!--HONumber=Aug16_HO1-->


