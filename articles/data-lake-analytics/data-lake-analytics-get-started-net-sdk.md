---
title: ".NET SDK&quot;yı kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Microsoft Belgeleri"
description: "Data Lake Analytics hesapları oluşturmak, Data Lake Analytics işleri oluşturmak ve U-SQL de yazılmış işleri göndermek için .NET SDK nın nasıl kullanılacağını öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 1dfcbc3d-235d-4074-bc2a-e96def8298b6
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/26/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 8e092e30c9c4186e4687efeacf9ea1f6b4bf431c
ms.openlocfilehash: f617d997bc34d39f7635a87c4e5c88b1ebdc0ff8
ms.lasthandoff: 11/17/2016


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-net-sdk"></a>Öğretici: .NET SDK'yı kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure .NET SDK’sını kullanarak [U-SQL](data-lake-analytics-u-sql-get-started.md)’de yazılan işleri Data Lake Analytics’e gönderme hakkında bilgi edinin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir U-SQL işini göndermek üzere C# konsolu uygulaması geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu makalenin üst kısmındaki sekmelere tıklayın.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Visual Studio 2015, Visual Studio 2013 Güncelleştirme 4 veya Visual C++ Yüklü Visual Studio 2012**.
* **.NET sürüm 2.5 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Azure Data Lake Analytics hesabı**. Bkz. [Azure .NET SDK'yı kullanarak Data Lake Analytics’i yönetme](data-lake-analytics-manage-use-dotnet-sdk.md).

## <a name="create-console-application"></a>Konsol uygulaması oluşturma
Bu eğiticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir. 

Örnek bir arama günlüğü, ortak Azure Blob kapsayıcısında bulunabilir. Uygulamada, dosyayı iş istasyonunuza indirecek ve ardından Data Lake Analytics hesabınızın varsayılan Data Lake Store hesabına yükleyeceksiniz.

**U-SQL betiği oluşturmak için**

Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi için bkz. [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).

Aşağıdaki U-SQL betiğiyle bir **SampleUSQLScript.txt** dosyası oluşturun ve dosyayı **C:\temp\** yoluna yerleştirin.  Yol, sonraki yordamda oluşturduğunuz .NET uygulamasında sabit kodlanmıştır.  

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

Mutlak yolları, bağlı Depolama hesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Depolama hesabında depolanan dosyalar için söz dizimi şu şekildedir:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

> [!NOTE]
> Şu anda Azure Data Lake Hizmeti ile ilgili bilinen bir sorun yoktur.  Örnek uygulama kesintiye uğrar veya bir hatayla karşılaşırsa betiğin oluşturduğu Data Lake Store ve Data Lake Analytics hesaplarını elle silmeniz gerekebilir.  Azure portal hakkında bilgi sahibi değilseniz [Azure Portal kullanarak Azure Data Lake Analytics’i yönetme](data-lake-analytics-manage-use-portal.md) kılavuzu çalışmaya başlamanıza yardımcı olur.       
> 
> 

**Uygulama oluşturmak için**

1. Visual Studio'yu açın.
2. Bir C# konsol uygulaması oluşturun.
3. NuGet Paketi Yönetimi konsolunu açın ve aşağıdaki komutları çalıştırın:
   
        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package WindowsAzure.Storage
4. Program.cs içinde, aşağıdaki kodu yapıştırın:
   
        using System;
        using System.IO;
        using System.Collections.Generic;
        using System.Threading;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.WindowsAzure.Storage.Blob;
   
        namespace SdkSample
        {
          class Program
          {
            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
   
            private static string _adlaAccountName = "<Enter an Existing Data Lake Analytics Account Name>";
            private static string _adlsAccountName = "<Enter the default Data Lake Store Account Name>";
   
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
   
            private static void Main(string[] args)
            {
                string localFolderPath = @"c:\temp\";
   
                // Connect to Azure
                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);
   
                SetupClients(creds, SUBSCRIPTIONID);
   
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

                  WaitForNewline("Job output downloaded. You can now exit.");
            }

            public static ServiceClientCredentials AuthenticateAzure(
                string domainName,
                string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static void SetupClients(ServiceClientCredentials tokenCreds, string subscriptionId)
            {
                _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
                _adlaClient.SubscriptionId = subscriptionId;

                _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
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

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                Console.WriteLine(reason + "\r\nPress ENTER to continue...");

                Console.ReadLine();

                if (!String.IsNullOrWhiteSpace(nextAction))
                    Console.WriteLine(nextAction);
            }

            // List all Data Lake Analytics accounts within the subscription
            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                Console.WriteLine("You have %i Data Lake Analytics account(s).", accounts.Count);
                for (int i = 0; i < accounts.Count; i++)
                {
                    Console.WriteLine(accounts[i].Name);
                }

                return accounts;
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
          }
        }

5. Uygulamayı çalıştırmak için **F5**'e basın. Çıktı şu şekildedir:
   
    ![Azure Data Lake Analytics işi U-SQL .NET SDK çıktısı](./media/data-lake-analytics-get-started-net-sdk/data-lake-analytics-dotnet-job-output.png)
6. Çıktı dosyasını denetleyin.  Varsayılan yol ve dosya adı c:\Temp\SearchLog-from-Data-Lake.csv şeklindedir.

## <a name="see-also"></a>Ayrıca bkz.
* Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
* Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).


