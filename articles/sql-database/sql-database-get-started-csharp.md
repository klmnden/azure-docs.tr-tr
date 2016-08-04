<properties
    pageTitle="SQL Database'i deneme: C# kullanarak SQL veritabanı oluşturma | Microsoft Azure"
    description="SQL ve C# uygulamalarını geliştirmek için SQL Database kullanmayı deneyin ve .NET için SQL Database Kitaplığı'nı kullanarak C# ile bir Azure SQL Database oluşturun."
    keywords="try sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="05/26/2016"
   ms.author="sstein"/>

# SQL Database'i deneme: C# kullanarak .NET için SQL Database Kitaplığı ile bir SQL veritabanı oluşturma


> [AZURE.SELECTOR]
- [Azure portalı](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

[.NET için Azure SQL Database Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) ile bir Azure SQL veritabanı oluşturmak üzere C# komutlarını nasıl kullanacağınızı öğrenin. SQL ve C# ile tek bir veritabanı oluşturarak SQL Database'i deneyeceksiniz. Esnek veritabanı havuzu oluşturmak için bkz. [Esnek veritabanı havuzu oluşturma](sql-database-elastic-pool-create-portal.md). Kod parçacıkları daha anlaşılır olmaları için ayrılmış olup tüm komutlar, makalenin alt kısmındaki bölümde örnek bir konsol uygulaması tarafından bir araya getirilmiştir.

.NET için Azure SQL Database Kitaplığı [Resource Manager tabanlı SQL Database REST API'sini](https://msdn.microsoft.com/library/azure/mt163571.aspx) sarmalayan [Azure Resource Manager](../resource-group-overview.md) tabanlı bir API sağlar. Bu istemci kitaplığı, Resource Manager tabanlı istemci kitaplıkları için yaygın olarak kullanılan deseni izler. Resource Manager için kaynak grupları ve [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (AAD) ile kimlik doğrulaması gerekir.

<br>

> [AZURE.NOTE] .NET için SQL Database Kitaplığı şu anda önizlemede.

<br>

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

- Azure aboneliği. Bir Azure aboneliğine ihtiyacınız varsa bu sayfanın üst kısmındaki **ÜCRETSİZ DENEME SÜRÜMÜ**'ne tıklamanız yeterlidir; ardından makaleyi tamamlamak için geri dönün.
- Visual Studio. Ücretsiz bir Visual Studio kopyası için [Visual Studio İndirmeleri](https://www.visualstudio.com/downloads/download-visual-studio-vs) sayfasına göz atın.


## Gerekli kitaplıkları yükleme

C# ile bir SQL veritabanını ayarlamak için Visual Studio'da bulunan [paket yöneticisi konsolunu](http://docs.nuget.org/Consume/Package-Manager-Console) (**Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**) kullanıp şu paketleri yükleyerek gerekli yönetim kitaplıklarını edinin:

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.ResourceManager –Pre
    Install-Package Microsoft.Azure.Common.Authentication –Pre


## Azure Active Directory ile kimlik doğrulamasını yapılandırma

İlk önce gerekli kimlik doğrulamasını ayarlayarak istemci uygulamanızın SQL Database hizmetine erişimini etkinleştirmeniz gerekir.

Geçerli kullanıcı için istemci uygulamanızın kimlik doğrulamasını gerçekleştirmek üzere ilk önce Azure kaynaklarının oluşturulduğu abonelikle ilişkili olan AAD etki alanındaki uygulamanızı kaydetmeniz gerekir. Azure aboneliğiniz iş veya okul hesabıyla değil de bir Microsoft hesabıyla oluşturulduysa zaten varsayılan bir AAD etki alanınız vardır.

Yeni bir uygulama oluşturmak ve uygulamayı doğru Active Directory'ye kaydetmek için şunları yapın:

1. [Klasik Azure Portalı](https://manage.windowsazure.com/)'na gidin.
1. Sol taraftan **Active Directory** hizmetini seçip uygulamanız için kimlik doğrulaması yapacağınız dizini seçin ve **Adına** tıklayın.

    ![SQL Database'i deneme: Azure Active Directory'yi (AAD) ayarlama.][1]

2. Dizin sayfasında **UYGULAMALAR**'a tıklayın.

    ![Uygulamalar'ı içeren dizin sayfası.][5]

4. Yeni bir uygulama oluşturmak için **EKLE**'ye tıklayın.

5. **Kuruluşumun geliştirdiği bir uygulama ekle**'yi seçin.

5. Uygulama için bir **AD** sağlayın ve **YEREL İSTEMCİ UYGULAMASI**'nı seçin.

    ![SQL C# uygulamanız ile ilgili bilgi sağlayın.][7]

6. Bir **YÖNLENDİRME URI'si** sağlayın. Gerçek bir uç nokta olması gerekmez, geçerli bir URI olması yeterlidir.

    ![SQL C# uygulamanız için bir yönlendirme URL'si ekleyin.][8]

7. Uygulamayı oluşturmayı sonlandırın, **YAPILANDIR**'a tıklayın ve **İSTEMCİ KİMLİĞİ**'ni kopyalayın. (İstemci kimliğine daha sonra kodunuzda ihtiyacınız olacak).

    ![SQL C# uygulamanız için bir istemci kimliği alın.][9]


1. Sayfanın alt kısmındaki **Uygulama ekle**'ye tıklayın.
1. **Microsoft Uygulamaları**'nı seçin.
1. **Azure Hizmet Yönetimi API'sini** seçip sihirbazı tamamlayın.
2. API seçtikten sonra **Azure Hizmet Yönetimi'ne (önizleme) Erişim** seçeneğini belirleyerek bu API'ye erişim için gereken belirli özel izinleri sağlamanız gerekir.

    ![İzinleri belirleyin.][2]

2. **KAYDET**'e tıklayın.



### Etki alanı adını tanımlama

Etki alanı adı, kodunuz için gereklidir. Uygun etki alanı adını tanımlamanın kolay bir yolu için şunları yapın:

1. [Azure Portal](http://portal.azure.com)'a gidin.
2. Sağ üst köşede bulunan adınızın üzerine gelin ve açılır pencerede görünen Etki Alanını not edin.

    ![Etki alan adını tanımlayın.][3]





**Ek AAD Kaynakları**  

[Bu faydalı blog gönderisine](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/) göz atarak, kimlik doğrulaması için Azure Active Directory kullanımı hakkında ek bilgi edinebilirsiniz.


### Geçerli kullanıcı için erişim belirtecini alma

İstemci uygulamasının geçerli kullanıcı için uygulama erişim belirtecini alması gerekir. Bu kod bir kullanıcı tarafından ilk kez yürütüldüğünde, kullanıcıdan kullanıcı kimlik bilgilerini girmesi istenir ve elde edilen belirteç yerel olarak önbelleğe alınır. Sonraki yürütmelerde belirteç önbellekten alınır ve kullanıcıdan yalnızca belirtecin süresinin dolması halinde oturum açması istenir.

Bu kod, bir Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationResult döndürür. Bunu aşağıdaki diğer kod parçacıklarında kullanmanız gerekir.

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



> [AZURE.NOTE] Bu makaledeki örneklerde, temel alınan hizmet üzerindeki REST çağrısı tamamlanana kadar her bir API isteğinin ve bloğunun zaman uyumlu bir biçimi kullanıldı. Zaman uyumsuz yöntemler de kullanılabilir.



## Kaynak grubu oluşturma

Resource Manager'da tüm kaynakların kaynak grubunda oluşturulması gerekir. Kaynak grubu, bir uygulama için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Azure SQL Database'de veritabanı sunucusunun var olan bir kaynak grubu içinde oluşturulması gerekir.

        static void CreateResourceGroup()
        {
            creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = location,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
        }


## Sunucu oluşturma

SQL veritabanları sunucularda bulunur. Sunucu adı tüm Azure SQL sunucuları arasında genel olarak benzersiz olmalıdır, bu nedenle sunucu adı daha önce alınmışsa burada bir hata ile karşılaşırsınız. Bu komutun tamamlanmasının birkaç dakika sürebileceğini unutmayın.

    static void CreateServer()
    {
        // Create a SQL Database management client
        SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

        // Create a server
        ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
        {
            Location = location,
            Properties = new ServerCreateOrUpdateProperties()
            {
                AdministratorLogin = administratorLogin,
                AdministratorLoginPassword = administratorPassword,
                Version = serverVersion
            }
        };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
    }


### Dış sunucu yöneticisi oluşturma


    // Create a server external admin
    ServerAdministratorCreateOrUpdateParameters aadAdminParameters = new ServerAdministratorCreateOrUpdateParameters()
    {
        Properties = new ServerAdministratorCreateOrUpdateProperties()
        {
            AdministratorType = "ActiveDirectory",
            Login = "<login>",
            Sid = new Guid("<Azure AD admin sid>"),
            TenantId = new Guid("<Azure AD tenant id>")
        }
    };

    var adminResult = sqlClient.ServerAdministrators.CreateOrUpdate(resourceGroupName, serverName, "ActiveDirectory", aadAdminParameters);
    var adminGetResult = sqlClient.ServerAdministrators.Get(resourceGroupName, serverName, "ActiveDirectory");




## Sunucuya erişim izni vermek için sunucu güvenlik duvarı kuralı oluşturma

Varsayılan olarak, bir sunucuya her konumdan bağlanılamaz. Bir sunucuya veya sunucu üzerindeki herhangi bir veritabanına bağlanmak için istemci IP adresinden erişime olanak sağlayan bir [güvenlik duvarı kuralı](https://msdn.microsoft.com/library/azure/ee621782.aspx) tanımlanmalıdır.

Aşağıdaki örnekte herhangi bir IP adresinden sunucuya erişimi açan bir kural oluşturulmuştur. Veritabanınızın güvenliğini sağlamak için, uygun SQL oturum açma işlemleri ve parolalar oluşturmanız ve yetkisiz erişimi engellemek üzere güvenlik duvarı kurallarını birincil önlem olarak görmemeniz önerilir.


        static void CreateFirewallRule()
        {
            // Create a firewall rule on the server
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };
            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
        }




Diğer Azure hizmetlerinin bir sunucuya erişimine izin vermek için bir güvenlik duvarı kuralı ekleyin ve hem StartIpAddress hem de EndIpAddress değerini 0.0.0.0 olarak ayarlayın. Bu işlem sonrasında *tüm* Azure aboneliklerinden gelen Azure trafiğinin sunucuya erişimine izin verildiğini unutmayın.


## C&#x23; kullanarak SQL veritabanı oluşturma

Aşağıdaki C# komutu kullanılarak sunucuda aynı ada sahip başka bir veritabanının olmaması halinde yeni bir SQL veritabanı oluşturulur ancak aynı ada sahip bir veritabanı varsa ilgili veritabanı güncelleştirilir.


        static void CreateDatabase()
        {
            // Create a database

            // Retrieve the server on which the database will be created
            Server currentServer = sqlClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel,
                }
            };
            var dbResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
        }



## Örnek C&#x23; konsol uygulaması

Aşağıdaki örnekte bir kaynak grubu, sunucu güvenlik duvarı kuralı ve bir SQL veritabanı oluşturulmuştur. Bu makalenin üst kısmındaki *Azure Active Directory ile kimlik doğrulamasını yapılandırma* bölümünde clientId, redirectUri ve domainName değişkenlerine ilişkin değerlerin nereden alındığı gösterilmektedir.


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace SqlDbConsoleApp
    {
    class Program
    {
        // Get these values from the Azure portal
        static string subscriptionId = "<Azure subscription id>";
        static string clientId = "<Azure App clientId>";
        static string redirectUri = "<Azure App redirectURI>";
        static string domainName = "<domain>";

        // You create these values
        static string resourceGroupName = "<your resource group name>";
        static string location = "<Azure data center location>";

        static string serverName = "<your server name>";
        static string administratorLogin = "<your server admin>";

        // store your password securely!
        static string administratorPassword = "<your server admin password>";
        static string serverVersion = "12.0";

        static string firewallRuleName = "<your firewall rule name>";

        static string databaseName = "dbfromcsharparticle";
        static string databaseEdition = "Basic"; // Basic, Standard, or Premium
        static string databasePerfLevel = "";

        static AuthenticationResult token;
        static Microsoft.Rest.TokenCredentials creds;

        static SqlManagementClient sqlClient;


        static void Main(string[] args)
        {
            Console.WriteLine("Logging in...");

            token = GetAccessToken();

            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            Console.WriteLine("Creating resource group...");
            CreateResourceGroup();

            sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            Console.WriteLine("Creating server...");
            CreateServer();

            Console.WriteLine("Creating firewall rule...");
            CreateFirewallRule();

            Console.WriteLine("Creating database...");

            DatabaseCreateOrUpdateResponse dbResponse = CreateDatabase();
            Console.WriteLine("Status: " + dbResponse.Status.ToString()
                + " Code: " + dbResponse.StatusCode.ToString());

            Console.WriteLine("Press enter to exit...");
            Console.ReadLine();
        }

        static void CreateResourceGroup()
        {
            creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = location,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
        }

        static void CreateServer()
        {
            // Create a SQL Database management client
            //SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = location,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = administratorLogin,
                    AdministratorLoginPassword = administratorPassword,
                    Version = serverVersion
                }
            };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
        }

        static void CreateFirewallRule()
        {
            // Create a firewall rule on the server
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    // replace with your client IP address
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };
            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
        }

        static DatabaseCreateOrUpdateResponse CreateDatabase()
        {
            // Create a database

            // Retrieve the server on which the database will be created
            Server currentServer = sqlClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel,
                }
            };
            var dbResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
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




## Sonraki Adımlar
SQL Database'i ve C# ile bir veritabanını ayarlamayı denediğinize göre şu makaleler için hazırsınız:

- [SQL Server Management Studio ile SQL Database'e bağlanma ve bir örnek T-SQL sorgusu gerçekleştirme](sql-database-connect-query-ssms.md)

## Ek Kaynaklar

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)





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



<!----HONumber=Jun16_HO2-->


