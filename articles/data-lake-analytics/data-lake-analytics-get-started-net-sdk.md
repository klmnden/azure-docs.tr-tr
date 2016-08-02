<properties 
   pageTitle=".NET SDK'yı kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Azure" 
   description="Data Lake Store hesapları oluşturmak, Data Lake Analytics işleri oluşturmak ve U-SQL'de yazılmış işleri göndermek için .NET SDK'nın nasıl kullanılacağını öğrenin. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="04/21/2016"
   ms.author="edmaca"/>

# Öğretici: .NET SDK'yı kullanarak Azure Data Lake Analytics ile çalışmaya başlama

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Azure Data Lake Analytics hesapları oluşturmak, Data Lake Analytics işlerini [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde tanımlamak ve Data Lake Analytic hesaplarına iş göndermek için Azure .NET SDK olanağının nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bunu virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir U-SQL betiği içeren bir C# konsolu uygulaması geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki sekmelere tıklayın.

[AZURE.INCLUDE [basic-process-include](../../includes/data-lake-analytics-basic-process.md)]

##Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Visual Studio 2015, Visual Studio 2013 Güncelleştirme 4 veya Visual C++ Yüklü Visual Studio 2012**.
- **.NET sürüm 2.5 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
- **[Visual Studio için Data Lake Araçları](http://aka.ms/adltoolsvs)**. 

##Konsol uygulaması oluşturma

Bu öğreticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir. 

Örnek bir arama günlüğü, ortak Azure Blob kapsayıcısına kopyalanmıştır. Uygulamada, dosyayı iş istasyonunuza indirir ve ardından varsayılan Data Lake Store hesabına yüklersiniz.

**Uygulama oluşturmak için**

1. Visual Studio'yu açın.
2. Bir C# konsol uygulaması oluşturun.
3. Nuget Paketi Yönetimi konsolunu açın ve aşağıdaki komutları çalıştırın:

        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
        Install-Package WindowsAzure.Storage

4. **SampleUSQLScript.txt** adlı projeye yeni bir dosya ekleyin ve ardından aşağıdaki U-SQL betiğini yapıştırın. Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi için bkz. [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur. 
    
    C# programında, **/Samples/Data/SearchLog.tsv** dosyasını ve **/Output/** klasörünü hazırlamanız gerekir.    
    
    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin: 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Mutlak yolları, bağlı Storage hesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Ortak blob veya ortak kapsayıcı erişim izinlerine sahip Azure Blob kapsayıcısı şu an için desteklenmemektedir.    
       
       
5. Program.cs içinde, aşağıdaki kodu yapıştırın:

        using System;
        using System.IO;
        using System.Security;
        using System.Text;
        using System.Collections.Generic;
        using System.Linq;

        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeAnalyticsAccountManagementClient _adlaClient;
                private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
                private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlaAccountName;
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                    _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>"; // TODO: Replace this value with the name for a NEW Analytics account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                    _location = "East US 2";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and contains the U-SQL script.
                    
                    // Authenticate the user
                    // For more information about applications and instructions on how to get a client ID, see: 
                    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
                    var tokenCreds = AuthenticateUser("common", "https://management.core.windows.net/",
                        "<APPLICATION-CLIENT-ID>", new Uri("https://<APPLICATION-REDIRECT-URI>")); // TODO: Replace bracketed values.
                    
                    SetupClients(tokenCreds, "<SUBSCRIPTION-ID>"); // TODO: Replace bracketed value.
                    
                    // Run sample scenarios
                    WaitForNewline("Authenticated.", "Creating NEW accounts.");
                    CreateAccounts();
                    WaitForNewline("Accounts created.", "Preparing the source data file.");

                    // Transfer the source file from a public Azure Blob container to Data Lake Store.
                    CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://adltutorials.blob.core.windows.net/adls-sample-data/SearchLog.tsv"));
                    blob.DownloadToFile(localFolderPath + "SearchLog.tsv", FileMode.Create); // from WASB
                    UploadFile(localFolderPath + "SearchLog.tsv", "/Samples/Data/SearchLog.tsv"); // to ADLS
                    WaitForNewline("Source data file prepared.", "Submitting a job.");

                    // Submit the job
                    Guid jobId = SubmitJobByPath(localFolderPath + "SampleUSQLScript.txt", "My First ADLA Job");
                    WaitForNewline("Job submitted.", "Waiting for job completion.");

                    // Wait for job completion
                    WaitForJob(jobId);
                    WaitForNewline("Job completed.", "Downloading job output.");

                    // Download job output
                    DownloadFile("/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");
                    WaitForNewline("Job output downloaded.", "Deleting accounts.");

                    // Delete accounts
                    DeleteAccounts();
                    WaitForNewline("Accounts deleted. You can now exit.");
                }

                // Helper function to show status and wait for user input
                public static void WaitForNewline(string reason, string nextAction = "")
                {
                    if (!String.IsNullOrWhiteSpace(nextAction))
                    {
                        Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                        Console.ReadLine();
                        Console.WriteLine(nextAction);
                    }
                    else
                    {
                        Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                        Console.ReadLine();
                    }
                }

                // Authenticate the user with AAD through an interactive popup.
                // You need to have an application registered with AAD in order to authenticate.
                //   For more information and instructions on how to register your application with AAD, see: 
                //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
                public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
                {
                    var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);

                    var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
                        PromptBehavior.Auto, UserIdentifier.AnyUser);

                    return new TokenCredentials(tokenAuthResult.AccessToken);
                }

                // Authenticate the application with AAD through the application's secret key.
                // You need to have an application registered with AAD in order to authenticate.
                //   For more information and instructions on how to register your application with AAD, see: 
                //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
                public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
                {
                    var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
                    var credential = new ClientCredential(appClientId, clientSecret);

                    var tokenAuthResult = authContext.AcquireToken(resource, credential);

                    return new TokenCredentials(tokenAuthResult.AccessToken);
                }

                //Set up clients
                public static void SetupClients(TokenCredentials tokenCreds, string subscriptionId)
                {
                    _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
                    _adlaClient.SubscriptionId = subscriptionId;

                    _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);

                    _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(tokenCreds);

                    _adlsClient = new DataLakeStoreAccountManagementClient(tokenCreds);
                    _adlsClient.SubscriptionId = subscriptionId;

                    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
                }

                // Create accounts
                public static void CreateAccounts()
                {
                    // Create ADLS account
                    var adlsParameters = new DataLakeStoreAccount(location: _location);
                    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

                    // Create ADLA account
                    var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(_adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
                    var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: _adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
                    var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: _location);
                    _adlaClient.Account.Create(_resourceGroupName, _adlaAccountName, adlaParameters);
                }

                // Delete accounts
                public static void DeleteAccounts()
                {
                    _adlaClient.Account.Delete(_resourceGroupName, _adlaAccountName);

                    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
                }

                // List all ADLA accounts within the subscription
                public static List<DataLakeAnalyticsAccount> ListAdlAnalyticsAccounts()
                {
                    var response = _adlaClient.Account.List(_adlaAccountName);
                    var accounts = new List<DataLakeAnalyticsAccount>(response);

                    while (response.NextPageLink != null)
                    {
                        response = _adlaClient.Account.ListNext(response.NextPageLink);
                        accounts.AddRange(response);
                    }

                    return accounts;
                }

                // List all ADLS accounts within the subscription
                public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
                {
                    var response = _adlsClient.Account.List(_adlsAccountName);
                    var accounts = new List<DataLakeStoreAccount>(response);
                    
                    while (response.NextPageLink != null)
                    {
                        response = _adlsClient.Account.ListNext(response.NextPageLink);
                        accounts.AddRange(response);
                    }

                    return accounts;
                }

                // Submit a U-SQL job by providing script contents.
                // Returns the job ID
                public static Guid SubmitJobByScript(string script, string jobName)
                {
                    var jobId = Guid.NewGuid();
                    var properties = new USqlJobProperties(script);
                    var parameters = new JobInformation(jobName, JobType.USql, properties);

                    var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);
                    
                    return jobId;
                }

                // Submit a U-SQL job by providing a path to the script
                public static Guid SubmitJobByPath(string scriptPath, string jobName)
                {
                    var script = File.ReadAllText(scriptPath);

                    var jobId = Guid.NewGuid();
                    var properties = new USqlJobProperties(script);
                    var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1000, degreeOfParallelism: 1);

                    var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);

                    return jobId;
                }

                public static JobResult WaitForJob(Guid jobId)
                {
                    var jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                    while (jobInfo.State != JobState.Ended)
                    {
                        jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                    }
                    return jobInfo.Result.Value;
                }

                // List jobs
                public static List<JobInformation> ListJobs()
                {
                    var response = _adlaJobClient.Job.List(_adlaAccountName);
                    var jobs = new List<JobInformation>(response);

                    while (response.NextPageLink != null)
                    {
                        response = _adlaJobClient.Job.ListNext(response.NextPageLink);
                        jobs.AddRange(response);
                    }

                    return jobs;
                }

                // Upload a file
                public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
                {
                    var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                    var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                    var uploader = new DataLakeStoreUploader(parameters, frontend);
                    uploader.Execute();
                }

                // Download file
                public static void DownloadFile(string srcPath, string destPath)
                {
                    var stream = _adlsFileSystemClient.FileSystem.Open(srcPath, _adlsAccountName);
                    var fileStream = new FileStream(destPath, FileMode.Create);

                    stream.CopyTo(fileStream);
                    fileStream.Close();
                    stream.Close();
                }
            }
        }

7. Uygulamayı çalıştırmak için **F5**'e basın.

## Ayrıca bkz.

- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
- Daha karmaşık bir sorgu görmek için bkz. [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
- U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).
- Yönetim görevleri için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
- Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).



<!--HONumber=Jun16_HO2-->


