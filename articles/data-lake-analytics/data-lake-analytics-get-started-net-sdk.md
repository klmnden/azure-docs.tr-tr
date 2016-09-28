<properties 
   pageTitle=".NET SDK'yı kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Azure" 
   description="Data Lake Store hesapları oluşturmak, Data Lake Analytics işleri oluşturmak ve U-SQL'de yazılmış işleri göndermek için .NET SDK'nın nasıl kullanılacağını öğrenin. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/22/2016"
   ms.author="edmaca"/>


# Öğretici: .NET SDK'yı kullanarak Azure Data Lake Analytics ile çalışmaya başlama

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Azure Data Lake Analytics hesapları oluşturmak, Data Lake Analytics işlerini [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde tanımlamak ve Data Lake Analytic hesaplarına iş göndermek için Azure .NET SDK olanağının nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bunu virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir U-SQL betiği içeren bir C# konsolu uygulaması geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki sekmelere tıklayın.

##Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Visual Studio 2015, Visual Studio 2013 Güncelleştirme 4 veya Visual C++ Yüklü Visual Studio 2012**.
- **.NET sürüm 2.5 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
- **[Visual Studio için Data Lake Araçları](http://aka.ms/adltoolsvs)**. 
- Bir Azure Active Directory (AAD) uygulaması oluşturun ve **İstemci Kimliği**, **Kiracı Kimliği** ve **Anahtar** bilgilerini alın. AAD uygulamaları hakkında daha fazla bilgi ve istemci kimliği almaya ilişkin yönergeler için bkz. [Portal kullanarak Active Directory uygulaması ve hizmet sorumlusu oluşturma](../resource-group-create-service-principal-portal.md). Uygulama ve anahtar oluşturulduktan sonra Yanıt URI’si ve Anahtar da portaldan alınabilir.


##Konsol uygulaması oluşturma

Bu öğreticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir. 

Örnek bir arama günlüğü, ortak Azure Blob kapsayıcısına kopyalanmıştır. Uygulamada, dosyayı iş istasyonunuza indirir ve ardından varsayılan Data Lake Store hesabına yüklersiniz.

**Uygulama oluşturmak için**

1. Visual Studio'yu açın.
2. Bir C# konsol uygulaması oluşturun.
3. NuGet Paketi Yönetimi konsolunu açın ve aşağıdaki komutları çalıştırın:

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

    >[AZURE.NOTE] Şu anda Azure Data Lake Hizmeti ile ilgili bilinen bir sorun yoktur.  Örnek uygulama kesintiye uğrar veya bir hatayla karşılaşırsa betiğin oluşturduğu Data Lake Store ve Data Lake Analytics hesaplarını elle silmeniz gerekebilir.  Portal hakkında bilgi sahibi değilseniz [Azure Portal kullanarak Azure Data Lake Analytics’i yönetme](data-lake-analytics-manage-use-portal.md) kılavuzu çalışmaya başlamanıza yardımcı olur.       
       
5. Program.cs içinde, aşağıdaki kodu yapıştırın:

        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System;
        using System.Collections.Generic;
        using System.IO;
        
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
            private static string _tenantId;
            private static string _subId;
            private static string _clientId;
            private static string _clientKey;
        
            private static void Main(string[] args)
            {
              _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
              _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>"; // TODO: Replace this value with the name for a NEW Analytics account.
              _resourceGroupName = "<RESOURCE-GROUP>"; // TODO: Replace this value. This resource group should already exist.
              _location = "East US 2";
              _tenantId = "<TENANT-ID>";
              _subId = "<SUBSCRIPTION-ID>";
              _clientId = "<CLIENT-ID>";
              _clientKey = "<CLIENT-KEY>";
        
              string localFolderPath = @"c:\temp\"; // TODO: Make sure this exists and contains SampleUSQLScript.txt.
              var tokenCreds = Authenticate(_tenantId, _clientId, _clientKey);
        
              SetupClients(tokenCreds, _subId); 
        
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
              DownloadFile(@"/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");
              WaitForNewline("Job output downloaded.", "Deleting accounts.");
        
              // Delete accounts
              _adlaClient.Account.Delete(_resourceGroupName, _adlaAccountName);
              _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
        
              WaitForNewline("Accounts deleted. You can now exit.");
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
              Console.WriteLine(reason + "\r\nPress ENTER to continue...");
        
              Console.ReadLine();
        
              if (!String.IsNullOrWhiteSpace(nextAction))
                Console.WriteLine(nextAction);
            }
        
            public static TokenCredentials Authenticate(string tenantId, string clientId, string clientKey)
            {
              var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + _tenantId);
              var creds = new ClientCredential(_clientId, _clientKey);
              var tokenAuthResult = authContext.AcquireTokenAsync("https://management.core.windows.net/", creds).Result;
        
              return new TokenCredentials(tokenAuthResult.AccessToken);
            }
        
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
        
            public static void CreateAccounts()
            {
              //ADLS account first, ADLA requires an ADLS account
              var adlsParameters = new DataLakeStoreAccount(location: _location);
              _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
        
              var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(_adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
              var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: _adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
              var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: _location);
              _adlaClient.Account.Create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }
        
            public static Guid SubmitJobByPath(string scriptPath, string jobName)
            {
              var script = File.ReadAllText(scriptPath);
        
              var jobId = Guid.NewGuid();
              var properties = new USqlJobProperties(script);
              var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
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
        
            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
              var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
              var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
              var uploader = new DataLakeStoreUploader(parameters, frontend);
              uploader.Execute();
            }
        
            public static void DownloadFile(string srcPath, string destPath)
            {
              var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
              var fileStream = new FileStream(destPath, FileMode.Create);
        
              stream.CopyTo(fileStream);
              fileStream.Close();
              stream.Close();
            }
          }
        }

6. Uygulamayı çalıştırmak için **F5**'e basın.

## Ayrıca bkz.

- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
- Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
- U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).
- Yönetim görevleri için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
- Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).



<!--HONumber=Sep16_HO3-->


