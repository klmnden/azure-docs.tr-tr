---
title: .NET ile Azure Dosyaları için geliştirme | Microsoft Docs
description: Dosya verilerini depolamak için Azure Dosyaları'nı kullanan .NET uygulamaları ve hizmetlerini geliştirmeyi öğrenin.
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/22/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 38bafdb4753b41a9c8acd599e6b7215e1777c6cd
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65779475"
---
# <a name="develop-for-azure-files-with-net"></a>.NET ile Azure Dosyaları için geliştirme

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

Bu öğretici, dosya verilerini depolamak için [Azure Dosyaları](storage-files-introduction.md) kullanan uygulamalar geliştirmek üzere .NET kullanmayla ilgili temel bilgileri gösterir. Bu öğretici, .NET ve Azure Dosyaları ile temel eylemler gerçekleştirmek üzere basit bir konsol uygulaması oluşturur:

* Dosyanın içeriğini alma
* Dosya paylaşımı için kota (en fazla boyut) ayarlama.
* Paylaşımda tanımlı bir paylaşılan erişim ilkesi kullanan bir dosya için paylaşılan erişim imzası (SAS anahtarı) oluşturma.
* Bir dosyayı aynı depolama hesabındaki başka bir dosyaya kopyalama.
* Bir dosyayı aynı depolama hesabındaki bir bloba kopyalama.
* Sorun giderme için Azure Storage Ölçümleri’ni kullanacağız.

Azure Dosyaları hakkında daha fazla bilgi için bkz. [Azure Dosyaları'na Giriş](storage-files-introduction.md).

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="understanding-the-net-apis"></a>.NET API'lerini anlama

Azure dosyaları istemci uygulamalarına iki geniş yaklaşım sağlar: Sunucu İleti Bloğu (SMB) ve REST. .NET içinde bu yaklaşımlar `System.IO` ve `WindowsAzure.Storage` API'leri tarafından soyutlanır.

API | Kullanılması gereken durumlar | Notlar
----|-------------|------
[System.IO](https://docs.microsoft.com/dotnet/api/system.io) | Uygulamanız: <ul><li>SMB üzerinden dosya okuma/yazmaya ihtiyaç duyuyor</li><li>Azure Dosyaları hesabınıza 445 bağlantı noktası üzerinden erişimi olan bir cihazda çalışıyor</li><li>Dosya paylaşımının yönetim ayarlarından herhangi birini yönetmesi gerekmiyor</li></ul> | SMB üzerinden Azure Dosyaları ile dosya G/Ç kodlaması genellikle herhangi bir ağ dosya paylaşımı ya da yerel depolama cihazı ile G/Ç kodlaması işlemiyle aynıdır. .NET içindeki dosya G/Ç dahil birkaç özelliğe giriş için [bu öğreticiye](https://docs.microsoft.com/dotnet/csharp/tutorials/console-teleprompter) bakın.
[Microsoft.Azure.Storage.File](https://docs.microsoft.com/dotnet/api/overview/azure/storage#client-library) | Uygulamanız: <ul><li>Güvenlik duvarı veya ISS kısıtlamaları nedeniyle bağlantı noktası 445’te SMB üzerinden Azure Dosyalarına erişemiyor</li><li>Bir dosya paylaşımının kotasını ayarlama veya paylaşılan bir erişim imzası oluşturma gibi yönetim işlevleri gerektiriyor</li></ul> | Bu makalede dosya G/Ç için REST ile (SMB yerine) `Microsoft.Azure.Storage.File` kullanımı ve dosya paylaşımının yönetimi gösterilmektedir.

## <a name="create-the-console-application-and-obtain-the-assembly"></a>Konsol uygulaması oluşturma ve derleme alma
Visual Studio'da yeni bir Windows konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2017’de konsol uygulaması oluşturmayı gösterir, ancak adımlar, diğer Visual Studio sürümlerindekilerle aynıdır.

1. **Dosya** > **Yeni** > **Proje**’yi seçin
2. **Yüklü** > **Şablonlar** > **Visual C#** > **Windows Klasik Masaüstü** öğesini seçin
3. **Konsol Uygulaması (.NET Framework)** öğesini seçin
4. **Ad:** alanına uygulamanız için bir ad girin
5. **Tamam**’ı seçin

Bu öğreticideki tüm kod örnekleri konsol uygulamanızın `Program.cs` dosyasındaki `Main()` yöntemine eklenebilir.

Azure depolama istemci Kitaplığı'ndaki bir Azure bulut hizmeti veya web uygulaması da dahil olmak üzere, .NET uygulaması ve Masaüstü ve mobil uygulamaları herhangi bir türü kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

## <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma
Bu öğreticiyi tamamlamak için projenizde başvurmanız gereken iki paket vardır:

* [.NET için Microsoft Azure depolama ortak kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/): Bu paket depolama hesabınızdaki ortak kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure depolama blobu Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/): Bu paket depolama hesabınızdaki Blob kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Yapılandırma Yöneticisi Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/): Bu paket, uygulamanızın nerede çalıştığına bakmaksızın yapılandırma dosyasındaki bağlantı dizesini ayrıştırmak için bir sınıf sağlar.

Her iki paketi de almak için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Çözüm Gezgini**'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. Çevrimiçi olarak "WindowsAzure.Storage" ifadesini arayın ve Depolama İstemci Kitaplığı’nı ve bağımlılıklarını yüklemek için **Yükle**’ye tıklayın.
3. Çevrimiçi olarak "WindowsAzure.ConfigurationManager" ifadesini arayın ve Azure Yapılandırma Yöneticisi’ni yüklemek için **Yükle**’ye tıklayın.

## <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Depolama hesabı kimlik bilgilerinizi app.config dosyasına kaydetme
Sonraki adımda, kimlik bilgilerinizi projenizin app.config dosyasına kaydedin. app.config dosyasını aşağıdaki örneğe benzeyecek şekilde düzenleyin. `myaccount` değerini depolama hesabınızın adıyla ve `mykey` değerini depolama hesabınızın anahtarıyla değiştirin.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> Azure depolama öykünücüsünün en son sürümü Azure Dosyaları'nı desteklemez. Bağlantı dizeniz, Azure Dosyaları ile çalışmak için buluttaki bir Azure Depolama hesabını hedeflemelidir.

## <a name="add-using-directives"></a>Using yönergeleri ekleme
Çözüm Gezgini’nde `Program.cs` dosyasını açın ve aşağıdaki using yönergelerini dosyanın üst tarafına ekleyin.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.Azure.Storage; // Namespace for Storage Client Library
using Microsoft.Azure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.Azure.Storage.File; // Namespace for Azure Files
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-the-file-share-programmatically"></a>Dosya paylaşımına programlamayla erişme
Şimdi, bağlantı dizesini almak için aşağıdaki kodu `Main()` yöntemine (yukarıda gösterilen koddan sonra) ekleyin. Bu kod, daha önce oluşturduğumuz dosyaya başvuru alır ve bu dosyanın içeriğini konsol penceresine çıkarır.

```csharp
// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Çıkışı görmek konsol uygulamasını çalıştırın.

## <a name="set-the-maximum-size-for-a-file-share"></a>Dosya paylaşımı için boyut üst sınırını ayarlama
Azure Depolama İstemci Kitaplığı'nın 5.x sürümünden başlayarak, dosya paylaşımı için gigabayt cinsinden kota (veya boyut üst sınırı) ayarlayabilirsiniz. Paylaşımda halihazırda ne kadar verinin depolandığını da kontrol edebilirsiniz.

Paylaşım için kota ayarlayarak paylaşımda depolanan toplam dosya boyutunu kısıtlayabilirsiniz. Paylaşımdaki toplam dosya boyutu belirlediğiniz kotayı aşarsa, istemciler mevcut dosyaların boyutunu artıramaz veya boş olmamaları halinde yeni dosyalar oluşturamaz.

Aşağıdaki örnekte, paylaşımdaki mevcut kullanımını nasıl kontrol edileceği veya paylaşım için nasıl kota ayarlanacağı gösterilmiştir.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.Azure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Dosya veya dosya paylaşımı için paylaşılan erişim imzası oluşturma
Azure Storage İstemci Kitaplığı’nın 5.x sürümünden başlayarak, bir dosya paylaşımı veya yalnızca dosya için paylaşılan erişim imzası (SAS) oluşturabilirsiniz. Ayrıca, paylaşılan erişim imzalarını yönetmek için dosya paylaşımında bir paylaşılan erişim ilkesi oluşturabilirsiniz. Gizliliğinin tehlikeye girdiği durumlarda SAS’yi iptal etme aracı olarak kullanılabilmesi nedeniyle bir paylaşılan erişim ilkesi oluşturmanız önerilir.

Aşağıdaki örnekte, paylaşım için bir paylaşılan erişim ilkesi oluşturulur, daha sonra bu ilke paylaşımdaki bir dosyada bulunan SAS için sınırlamalar sağlamak amacıyla kullanılır.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authorized via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Paylaşılan erişim imzaları oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [kullanarak paylaşılan erişim imzaları (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="copy-files"></a>Dosyaları kopyalama
Azure Storage İstemci Kitaplığı’nın 5.x sürümünden başlayarak, bir dosyayı başka bir dosyaya, bir dosyayı başka bir bloba veya bir blobu bir dosyaya kopyalayabilirsiniz. Sonraki bölümlerde, bu kopyalama işlemlerini programlamayla nasıl gerçekleştirebileceğinizi göstereceğiz.

Bir dosyayı diğer bir dosyaya veya bir blobu bir dosyaya ya da tam tersini yapmak için AzCopy’i de kullanabilirsiniz. Bkz. [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

> [!NOTE]
> Bir blobu dosyaya veya bir dosyayı bloba kopyalamak için aynı depolama hesabında kopyalama yapıyor olsanız da kaynak nesnesi erişimini yetkilendirmek amacıyla bir paylaşılan erişim imzası (SAS) kullanmanız gerekir.
> 
> 

**Dosyayı başka bir dosyaya kopyalama** Aşağıdaki örnekte, bir dosya aynı paylaşımdaki başka bir dosyaya kopyalanır. Bu kopyalama işlemi aynı depolama hesabındaki dosyaları kopyaladığı için, kopyalama işlemini gerçekleştirmek üzere Paylaşılan Anahtar kimlik doğrulaması kullanabilirsiniz. 

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Dosyayı bir bloba kopyalama** Aşağıdaki örnekte, bir dosya oluşturulur ve aynı depolama hesabındaki bir bloba kopyalanır. Örnekte, kaynak dosya için hizmetin kopyalama sırasında kaynak dosyaya erişimi yetkilendirmek üzere kullandığı bir SAS oluşturulur.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authorize access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Aynı şekilde, bir blobu bir dosyaya kopyalayabilirsiniz. Kaynak dosya bir blob ise, kopyalama sırasında bu bloba erişimi yetkilendirmesi için bir SAS oluşturun.

## <a name="share-snapshots"></a>Paylaşım anlık görüntüleri
Azure depolama istemci Kitaplığı'nın 8.5 sürümünden başlayarak, bir paylaşım anlık görüntüsü oluşturabilirsiniz. Ayrıca paylaşım anlık görüntülerini listeleyebilir, onlara göz atabilir ve paylaşım anlık görüntülerini silebilirsiniz. Paylaşım anlık görüntüleri salt okunur olduğundan paylaşım anlık görüntüleri üzerinde yazma işlemi gerçekleştirilemez.

**Paylaşım anlık görüntüsü oluşturma**

Aşağıdaki örnekte dosya paylaşım anlık görüntüsü oluşturulmaktadır.

```csharp
storageAccount = CloudStorageAccount.Parse(ConnectionString); 
fClient = storageAccount.CreateCloudFileClient(); 
string baseShareName = "myazurefileshare"; 
CloudFileShare myShare = fClient.GetShareReference(baseShareName); 
var snapshotShare = myShare.Snapshot();

```
**Paylaşım anlık görüntülerini listeleme**

Aşağıdaki örnekte bir paylaşım üzerindeki paylaşım anlık görüntüleri listelenmektedir.

```csharp
var shares = fClient.ListShares(baseShareName, ShareListingDetails.All);
```

**Paylaşım anlık görüntüleri içindeki dosyalara ve dizinlere göz atma**

Aşağıdaki örnekte paylaşım anlık görüntüleri içindeki dosyalara ve dizinlere göz atılmaktadır.

```csharp
CloudFileShare mySnapshot = fClient.GetShareReference(baseShareName, snapshotTime); 
var rootDirectory = mySnapshot.GetRootDirectoryReference(); 
var items = rootDirectory.ListFilesAndDirectories();
```

**Paylaşımları ve paylaşım anlık görüntülerini listeleme ve dosya paylaşımlarını veya paylaşım anlık görüntülerindeki dosyaları geri yükleme** 

Bir dosya paylaşımının anlık görüntüsünü alarak daha sonra içindeki dosyaları veya dosya paylaşımının tamamını kurtarabilirsiniz. 

Bir dosya paylaşımının paylaşım anlık görüntülerini sorgulayarak bir dosya paylaşım anlık görüntüsündeki dosyayı geri yükleyebilirsiniz. Ardından belirli bir paylaşım ekran görüntüsüne ait olan bir dosyayı alabilir ve bu sürümü doğrudan okuyup karşılaştırmak veya geri yüklemek için kullanabilirsiniz.

```csharp
CloudFileShare liveShare = fClient.GetShareReference(baseShareName);
var rootDirOfliveShare = liveShare.GetRootDirectoryReference();

       var dirInliveShare = rootDirOfliveShare.GetDirectoryReference(dirName);
var fileInliveShare = dirInliveShare.GetFileReference(fileName);

           
CloudFileShare snapshot = fClient.GetShareReference(baseShareName, snapshotTime);
var rootDirOfSnapshot = snapshot.GetRootDirectoryReference();

       var dirInSnapshot = rootDirOfSnapshot.GetDirectoryReference(dirName);
var fileInSnapshot = dir1InSnapshot.GetFileReference(fileName);

string sasContainerToken = string.Empty;
       SharedAccessFilePolicy sasConstraints = new SharedAccessFilePolicy();
       sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
       sasConstraints.Permissions = SharedAccessFilePermissions.Read;
       //Generate the shared access signature on the container, setting the constraints directly on the signature.
sasContainerToken = fileInSnapshot.GetSharedAccessSignature(sasConstraints);

string sourceUri = (fileInSnapshot.Uri.ToString() + sasContainerToken + "&" + fileInSnapshot.SnapshotTime.ToString()); ;
fileInliveShare.StartCopyAsync(new Uri(sourceUri));

```


**Paylaşım anlık görüntülerini silme**

Aşağıdaki örnekte dosya paylaşım anlık görüntüsü silinmektedir.

```csharp
CloudFileShare mySnapshot = fClient.GetShareReference(baseShareName, snapshotTime); mySnapshot.Delete(null, null, null);
```

## <a name="troubleshooting-azure-files-using-metrics"></a>Ölçümleri kullanarak Azure Dosyaları sorunlarını giderme
Artık Azure Depolama Analizi, Azure Dosyaları için ölçümleri destekliyor. Ölçüm verilerini kullanarak istekleri ve tanılama sorunlarını izleyebilirsiniz.

Azure dosyaları için ölçümleri etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com). Ayrıca, REST API veya Depolama İstemci Kitaplığı'ndaki analoglarından biri aracılığıyla Dosya Hizmeti Özelliklerini Ayarla işlemine çağrı yaparak ölçümleri programlamayla etkinleştirebilirsiniz.

Aşağıdaki kodda, Azure Dosyaları için ölçümleri etkinleştirmek üzere .NET için Depolama İstemcisi Kitaplığı'nı nasıl kullanacağınız gösterilmiştir.

İlk olarak, yukarıda eklediğiniz yönergelere ek olarak aşağıdaki `using` yönergelerini `Program.cs` dosyanıza ekleyin:

```csharp
using Microsoft.Azure.Storage.File.Protocol;
using Microsoft.Azure.Storage.Shared.Protocol;
```

Azure Blobları, Azure Tablosu ve Azure Kuyruklarının `Microsoft.Azure.Storage.Shared.Protocol` ad alanındaki paylaşılan `ServiceProperties` türünü kullanmasına rağmen, Azure Dosyaları'nın `Microsoft.Azure.Storage.File.Protocol` ad alanında bulunan kendi `FileServiceProperties` türünü kullandığına dikkat edin. Aşağıdaki kodların derlenebilmesi için her iki ad alanına da kodunuzdan başvurulmuş olması gerekir.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.Azure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read the metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Uçtan uca sorun giderme kılavuzu için [Azure Dosyaları Sorun Giderme Makalesine](storage-troubleshoot-windows-file-connection-problems.md) de bakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

### <a name="conceptual-articles-and-videos"></a>Kavramsal makaleler ve videolar
* [Azure Dosyaları: Windows ve Linux için uyumlu bulut SMB dosya sistemi](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Azure Dosyaları'nı Linux ile kullanma](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>File Storage için araç desteği
* [Microsoft Azure Depolama ile AzCopy kullanma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Azure Depolama ile Azure CLI kullanma](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Azure Dosyaları sorunlarını giderme](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Başvuru
* [.NET başvurusu için Depolama İstemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Dosya Hizmeti REST API başvurusu](https://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Blog yazıları
* [Azure Dosyaları genel kullanıma sunulmuştur](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure Dosyaları İncelemesi](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Microsoft Azure Dosya Hizmeti’ne Giriş](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Microsoft Azure Dosyaları ile kalıcı bağlantılar](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
