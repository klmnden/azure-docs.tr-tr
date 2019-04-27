---
title: Azure .NET SDK kullanarak Azure Data Lake Analytics'i yönetme
description: Bu makale, Data Lake Analytics işleri, veri kaynakları ve kullanıcıları yönettiğiniz uygulamaları yazmak için Azure .NET SDK'sını kullanmayı açıklar.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/18/2017
ms.openlocfilehash: 0a10af73d754596e9b5bb34b2974d7f1647d06f8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60617716"
---
# <a name="manage-azure-data-lake-analytics-a-net-app"></a>Azure Data Lake Analytics'i .NET uygulaması yönetme

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri Azure .NET SDK'sı kullanılarak yazılmış bir uygulamayı kullanarak yönetme konusunda açıklanır. 

## <a name="prerequisites"></a>Önkoşullar

* **Visual Studio 2015, Visual Studio 2013 Güncelleştirme 4 veya Visual C++ Yüklü Visual Studio 2012**.
* **.NET sürüm 2.5 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Gerekli NuGet paketleri**

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

|Paket|Version|
|-------|-------|
|[Microsoft.Rest.ClientRuntime.Azure.Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| 2.3.1|
|[Microsoft.Azure.Management.DataLake.Analytics](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|3.0.0|
|[Microsoft.Azure.Management.DataLake.Store](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|2.2.0|
|[Microsoft.Azure.Management.ResourceManager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|1.6.0-Preview|
|[Microsoft.Azure.Graph.RBAC](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|3.4.0-Preview|

NuGet komut satırı aracılığıyla bu paketler aşağıdaki komutlarla yükleyebilirsiniz:

```powershell
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a>Genel değişkenler

```csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Data Lake Analytics oturum açmak için birçok seçeneğiniz vardır. Aşağıdaki kod parçacığı bir açılır pencere ile etkileşimli kullanıcı kimlik doğrulaması ile kimlik doğrulaması örneği gösterir.

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

Kaynak kodu **GetCreds_User_Popup** ve kimlik doğrulaması için diğer seçenekler için kod içinde ele alınmıştır [Data Lake Analytics .NET kimlik doğrulama seçenekleri](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)


## <a name="create-the-client-management-objects"></a>İstemci yönetim nesneleri oluşturma

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

Zaten bir oluşturmadıysanız, Data Lake Analytics bileşenlerinizi oluşturmak için bir Azure kaynak grubu olmalıdır. Kimlik doğrulama kimlik bilgilerinizi, abonelik kimliği ve konum gerekir. Aşağıdaki kod, bir kaynak grubu oluşturma işlemi gösterilmektedir:

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```

Azure kaynak grupları ve Data Lake Analytics daha fazla bilgi için bkz.

### <a name="create-a-data-lake-store-account"></a>Data Lake Store hesabı oluşturma

Şimdiye kadar ADLA hesabı ADLS hesabı gerektirir. Kullanacağıma zaten yoksa, aşağıdaki kodla oluşturabilirsiniz:

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Aşağıdaki kod, ADLS hesabı oluşturur.

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

### <a name="checking-if-an-account-exists"></a>Bir hesabının bulunup bulunmadığı denetleniyor

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

Her Data Lake Analytics hesabı bir varsayılan Data Lake Store hesabı gerektirir. Analytics hesabı için varsayılan Store hesabını belirlemek için bu kodu kullanın.

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme

Data Lake Analytics, şu anda aşağıdaki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama Hesabı](../storage/common/storage-introduction.md)

### <a name="link-to-an-azure-storage-account"></a>Bir Azure depolama hesabına bağlama

Azure depolama hesapları için bağlantılar oluşturabilirsiniz.

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

Data Lake Store dosya sistemi istemci Yönetim nesnesi, karşıya yükleme ve tek tek dosya veya klasörleri Azure'dan yerel bilgisayarınıza aşağıdaki yöntemleri kullanarak yüklemek için kullanabilirsiniz:

- UploadFolder
- UploadFile
- DownloadFolder
- DownloadFile

Bu yöntemler için ilk parametre, Data Lake Store kaynak yolunu ve hedef yolu için parametreleri ardından hesabının adıdır.

Aşağıdaki örnek, bir Data Lake Store klasör indirmek gösterilmektedir.

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a>Bir Data Lake Store hesabında bir dosya oluşturun

``` csharp
using (var memstream = new MemoryStream())
{
   using (var sw = new StreamWriter(memstream, UTF8Encoding.UTF8))
   {
      sw.WriteLine("Hello World");
      sw.Flush();
      
      memstream.Position = 0;

      adlsFileSystemClient.FileSystem.Create(adls, "/Samples/Output/randombytes.csv", memstream);
   }
}
```

### <a name="verify-azure-storage-account-paths"></a>Azure depolama hesabı yolları doğrulayın

Aşağıdaki kod, bir Azure depolama hesabı (storageAccntName) bir Data Lake Analytics hesabı (analyticsAccountName) varsa ve Azure depolama hesabında bir kapsayıcı (containerName) varsa, denetler.

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a>Katalog ve işleri Yönet

DataLakeAnalyticsCatalogManagementClient nesnenin her bir Azure Data Lake Analytics hesabı için sağlanan SQL veritabanı'nı yönetmek için yöntemler sağlar. U-SQL betikleri ile veritabanına göndermek ve işlerini yönetmek için yöntemleri çalıştırmak DataLakeAnalyticsJobManagementClient sağlar.

### <a name="list-databases-and-schemas"></a>Veritabanlarını Listele ve şemalar

Liste birkaç nokta arasında en yaygın veritabanları ve şema yer almaktadır. Aşağıdaki kod, veritabanları koleksiyonunu alır ve ardından her veritabanı için şema numaralandırır.

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

Aşağıdaki kod, belirtilen tablodaki sütunları listelemek için bir Data Lake Analytics Kataloğu Yönetimi istemcisiyle veritabanına erişmek nasıl gösterir.

```csharp
var tbl = adlaCatalogClient.Catalog.GetTable(adla, "master", "dbo", "MyTableName");
IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

foreach (USqlTableColumn utc in columns)
{
  Console.WriteLine($"\t{utc.Name}");
}
```

### <a name="submit-a-u-sql-job"></a>U-SQL işi gönderme

Aşağıdaki kod, bir işi göndermek için bir Data Lake Analytics işi yönetim istemcisi kullanmayı gösterir.

``` csharp
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
```

### <a name="list-failed-jobs"></a>Başarısız olan işler listesi

Aşağıdaki kod, başarısız olan işler hakkında bilgiler listeler.

```csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a>Ardışık düzenler

Aşağıdaki kod, hesabınıza gönderilen bir iş, her işlem hattı hakkında bilgi listeler.

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a>Liste tekrarlar

Aşağıdaki kod, hesabınıza gönderilen bir iş, her yineleme hakkında bilgi listeler.

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a>Graf senaryoları

### <a name="look-up-user-in-the-aad-directory"></a>AAD kullanıcısı arayın

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-the-objectid-of-a-user-in-the-aad-directory"></a>AAD dizinde bir kullanıcının objectID alın

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme

DataLakeAnalyticsAccountManagementClient nesnesi, bir Data Lake Analytics hesabı için işlem ilkeleri yönetmek için yöntemler sağlar.

### <a name="list-compute-policies"></a>Liste işlem ilkeleri

Aşağıdaki kod, bir Data Lake Analytics hesabı için işlem ilkelerinin bir listesini alır.

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a>Yeni bir işlem ilkesi oluşturma

Aşağıdaki kod kullanılabilir maksimum AUS değerini belirtilen kullanıcı için 50 ve 250 en düşük iş önceliği ayarını bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur.

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics'i Azure portalını kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)