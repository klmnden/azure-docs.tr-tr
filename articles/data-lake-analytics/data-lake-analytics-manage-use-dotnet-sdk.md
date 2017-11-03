---
title: "Azure .NET SDK kullanarak Azure Data Lake Analytics yönetme | Microsoft Docs"
description: "Data Lake Analytics işleri, veri kaynakları, kullanıcıların nasıl yöneteceğinizi öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 0f8a95f96ce4c816dfb9132923faa9a9bf20c205
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Azure .NET SDK kullanarak Azure Data Lake Analytics yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure Data Lake Analytics hesapları, veri kaynakları, kullanıcılar ve işleri Azure .NET SDK'yı kullanarak yönetmeyi öğrenin. 

## <a name="prerequisites"></a>Ön koşullar

* **Visual Studio 2015, Visual Studio 2013 Güncelleştirme 4 veya Visual C++ Yüklü Visual Studio 2012**.
* **.NET sürüm 2.5 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Gerekli NuGet paketleri**

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

|Paket|Sürüm|
|-------|-------|
|[Microsoft.Rest.ClientRuntime.Azure.Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| 2.3.1|
|[Microsoft.Azure.Management.DataLake.Analytics](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|3.0.0|
|[Microsoft.Azure.Management.DataLake.Store](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|2.2.0|
|[Microsoft.Azure.Management.ResourceManager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|1.6.0-Preview|
|[Microsoft.Azure.Graph.RBAC](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|3.4.0-Preview|

NuGet komut satırı aracılığıyla bu paketleri aşağıdaki komutlarla yükleyebilirsiniz:

```
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a>Genel değişkenler

``` csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Data Lake Analytics oturum açmak için birçok seçeneğiniz vardır. Aşağıdaki kod parçacığını bir açılır pencere ile etkileşimli kullanıcı kimlik doğrulaması ile kimlik doğrulaması örneği gösterir.

``` csharp
using System;
using System.IO;
using System.Threading;
using System.Security.Cryptography.X509Certificates;

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Analytics;
using Microsoft.Azure.Management.DataLake.Analytics.Models;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Azure.Graph.RBAC;

public static Program
{
   public static string TENANT = "microsoft.onmicrosoft.com";
   public static string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
   public static System.Uri ARM_TOKEN_AUDIENCE = new System.Uri( @"https://management.core.windows.net/");
   public static System.Uri ADL_TOKEN_AUDIENCE = new System.Uri( @"https://datalake.azure.net/" );
   public static System.Uri GRAPH_TOKEN_AUDIENCE = new System.Uri( @"https://graph.windows.net/" );

   static void Main(string[] args)
   {
      string MY_DOCUMENTS= System.Environment.GetFolderPath( System.Environment.SpecialFolder.MyDocuments);
      string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");

      var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
      var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var graphCreds = GetCreds_User_Popup(TENANT, GRAPH_TOKEN_AUDIENCE, CLIENTID, tokenCache);
   }
}
```

Kaynak kodu **GetCreds_User_Popup** ve kimlik doğrulama için diğer seçenekleri için kod ele alınmıştır [Data Lake Analytics .NET kimlik doğrulama seçenekleri](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)


## <a name="create-the-client-management-objects"></a>İstemci Yönetimi nesneleri oluşturma

``` csharp
var resourceManagementClient = new ResourceManagementClient(armCreds) { SubscriptionId = subid };

var adlaAccountClient = new DataLakeAnalyticsAccountManagementClient(armCreds);
adlaAccountClient.SubscriptionId = subid;

var adlsAccountClient = new DataLakeStoreAccountManagementClient(armCreds);
adlsAccountClient.SubscriptionId = subid;

var adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(adlCreds);
var adlaJobClient = new DataLakeAnalyticsJobManagementClient(adlCreds);

var adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

var  graphClient = new GraphRbacManagementClient(graphCreds);
graphClient.TenantID = domain;

```

## <a name="manage-accounts"></a>Hesapları yönetme

### <a name="create-an-azure-resource-group"></a>Azure Kaynak Grubu oluşturma

Zaten bir oluşturmadıysanız, Data Lake Analytics bileşenlerinizi oluşturmak için Azure kaynak grubu olması gerekir. Kimlik doğrulama kimlik bilgilerinizi, abonelik kimliği ve bir konum gerekir. Aşağıdaki kod, bir kaynak grubu oluşturmak gösterilmektedir:

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```
Daha fazla bilgi için bkz: [Azure kaynak grupları ve Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).

### <a name="create-a-data-lake-store-account"></a>Data Lake Store hesabı oluşturma

Şimdiye kadar ADLA hesap ADLS hesabına gerektirir. Zaten kullanmak üzere yoksa, aşağıdaki kod ile oluşturabilirsiniz:

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Aşağıdaki kod ADLS hesabına oluşturur

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a>Liste Data Lake Store hesapları

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a>Liste Data Lake Analytics hesapları

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a>Bir hesap var olup olmadığını denetleme

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a>Bir hesap hakkında bilgi edinin

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a>Hesap silme

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-the-default-data-lake-store-account"></a>Varsayılan Data Lake Store hesabı edinin

Her Data Lake Analytics hesabı, bir varsayılan Data Lake Store hesabı gerektirir. Bu kod analizi hesabı için varsayılan depolama hesabı belirlemek için kullanın.

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönet

Data Lake Analytics şu anda aşağıdaki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure depolama hesabı](../storage/common/storage-introduction.md)

### <a name="link-to-an-azure-storage-account"></a>Bir Azure depolama hesabı bağlantı

Azure depolama hesaplarına bağlantılar oluşturabilirsiniz.

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a>Azure depolama veri kaynakları listesi

``` csharp
var stg_accounts = adlaAccountClient.StorageAccounts.ListByAccount(rg, adla);

if (stg_accounts != null)
{
  foreach (var stg_account in stg_accounts)
  {
      Console.WriteLine($"Storage account: {0}", stg_account.Name);
  }
}
```

### <a name="list-data-lake-store-data-sources"></a>Liste Data Lake Store veri kaynakları

``` csharp
var adls_accounts = adlsClient.Account.List();

if (adls_accounts != null)
{
  foreach (var adls_accnt in adls_accounts)
  {
      Console.WriteLine($"ADLS account: {0}", adls_accnt.Name);
  }
}
```

### <a name="upload-and-download-folders-and-files"></a>Karşıya yükleme ve klasörleri ve dosyaları indirme
Data Lake Store dosya sistemi istemci Yönetim nesnesi karşıya yükleyin ve tek tek dosyalar veya klasörler Azure'dan yerel bilgisayarınıza aşağıdaki yöntemleri kullanarak yüklemek için kullanabilirsiniz:

- UploadFolder
- UploadFile
- DownloadFolder
- DownloadFile

İlk parametre bu yöntemleri için veri Gölü deposu kaynak yolu ve hedef yolu parametrelerini arkasından hesabı adıdır.

Aşağıdaki örnek, indirme klasörü Data Lake Store'da gösterilmektedir.

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a>Bir Data Lake Store hesabı oluşturma

``` csharp
using (var memstream = new MemoryStream())
{
   using (var sw = new StreamWriter(memstream, UTF8Encoding.UTF8))
   {
      sw.WriteLine("Hello World");
      sw.Flush();

      adlsFileSystemClient.FileSystem.Create(adls, "/Samples/Output/randombytes.csv", memstream);
   }
}
```

### <a name="verify-azure-storage-account-paths"></a>Azure depolama hesabı yolları doğrulayın
Aşağıdaki kod, bir Data Lake Analytics hesabı (analyticsAccountName) bir Azure Storage hesabı (storageAccntName) olup olmadığını ve Azure depolama hesabında bir kapsayıcı (containerName) olup olmadığını denetler.

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a>Katalog ve işleri Yönet
DataLakeAnalyticsCatalogManagementClient nesne her Azure Data Lake Analytics hesabı için sağlanan SQL veritabanını yönetme için yöntemleri sağlar. U-SQL betikleri veritabanıyla göndermek ve işlerini yönetmek için yöntemleri çalıştırma DataLakeAnalyticsJobManagementClient sağlar.

### <a name="list-databases-and-schemas"></a>Liste veritabanları ve şemaları
Listelemek birkaç nokta arasında en yaygın veritabanları ve bunların şema yer almaktadır. Aşağıdaki kod veritabanları koleksiyonunu alır ve her veritabanı için şema numaralandırır.

``` csharp
var databases = adlaCatalogClient.Catalog.ListDatabases(adla);
foreach (var db in databases)
{
  Console.WriteLine($"Database: {db.Name}");
  Console.WriteLine(" - Schemas:");
  var schemas = adlaCatalogClient.Catalog.ListSchemas(adla, db.Name);
  foreach (var schm in schemas)
  {
      Console.WriteLine($"\t{schm.Name}");
  }
}
```

### <a name="list-table-columns"></a>Liste tablo sütunları
Aşağıdaki kod, bir Data Lake Analytics Kataloğu yönetim istemcisi belirtilen bir tablodaki sütun listesi ile veritabanına erişmek gösterilmiştir.

``` csharp
var tbl = adlaCatalogClient.Catalog.GetTable(adla, "master", "dbo", "MyTableName");
IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

foreach (USqlTableColumn utc in columns)
{
  string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
  Stream scriptStrm = adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
  string scriptTxt = string.Empty;
  using (StreamReader sr = new StreamReader(scriptStrm))
  {
      scriptTxt = sr.ReadToEnd();
  }

  var jobName = "SR_Wikipedia";
  var jobId = Guid.NewGuid();
  var properties = new USqlJobProperties(scriptTxt);
  var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
  var jobInfo = adlaJobClient.Job.Create(adla, jobId, parameters);
  Console.WriteLine($"Job {jobName} submitted.");

}
```

### <a name="list-failed-jobs"></a>Başarısız olan işler listesi
Aşağıdaki kod, başarısız olan işlerin hakkında bilgileri listeler.

``` csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a>Liste komut zincirleri
Aşağıdaki kod hesabınıza gönderilen işler her ardışık düzeni hakkında bilgi listeler.

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a>Liste tekrarları
Aşağıdaki kod hesabınıza gönderilen işler her yineleme hakkındaki bilgileri listeler.

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a>Grafik senaryoları

### <a name="look-up-user-in-the-aad-directory"></a>AAD dizinde bir kullanıcıyı arayın

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-the-objectid-of-a-user-in-the-aad-directory"></a>Bir AAD dizini kullanıcı objectID alın

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme
DataLakeAnalyticsAccountManagementClient nesnesi bir Data Lake Analytics hesabı için işlem ilkelerini yönetme için yöntemleri sağlar.

### <a name="list-compute-policies"></a>Liste işlem ilkeleri
Aşağıdaki kod, bir Data Lake Analytics hesabı için işlem ilkelerinin bir listesini alır.

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a>Yeni bir işlem ilke oluşturun
Aşağıdaki kod kullanılabilir maksimum Avustralya 50 belirtilen kullanıcıya ve 250 en düşük iş önceliği ayarını bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur.

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics'i Azure portalını kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
