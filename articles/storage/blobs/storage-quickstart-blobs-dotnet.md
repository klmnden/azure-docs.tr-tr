---
title: "Azure hızlı başlangıç - aktarımı nesneleri/.NET kullanarak Azure Blob depolama biriminden | Microsoft Docs"
description: ".NET kullanarak Azure Blob storage/gruptan nesneleri aktarmak hızlı bir şekilde öğrenin"
services: storage
documentationcenter: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: tamram
ms.openlocfilehash: 1eac4165c35cb116a359c074bd629c918b58097c
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>.NET kullanarak Azure Blob storage/aktarım nesneleri

Bu hızlı başlangıç Azure Storage için .NET istemci kitaplığı karşıya yükleyin, indirin ve blok BLOB'ları bir kapsayıcıda listelemek için nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç tamamlamak için yükleme [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) aşağıdaki iş yükü ile:
    
    - **Azure geliştirme**

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıç içinde kullanılan örnek uygulamayı temel konsol uygulamasıdır. 

Kullanım [git](https://git-scm.com/) geliştirme ortamınızı uygulamaya bir kopyasını indirmek için. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Bu komut, yerel git klasörünüze depoya klonlar. Visual Studio çözümü açmak için depolama BLOB'lar dotnet quickstart klasörü arayın, açmak ve depolama-BLOB'lar-dotnet-quickstart.sln üzerinde çift tıklatın. 

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamada, depolama hesabınız için bağlantı dizesi belirtmeniz gerekir. Açık `app.config` Visual Studio'daki Çözüm Gezgini'nden dosya. Bul `StorageConnectionString` girişi. İçin **değeri**, Azure portalından kaydettiğiniz bir bağlantı dizesi değerini değiştirin. `storageConnectionString` Aşağıdakine benzer görünmelidir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;
    AccountName=<NameHere>;
    AccountKey=<KeyHere>" />
  </appSettings>
</configuration>
```

## <a name="run-the-sample"></a>Örnek çalıştırın

Bu örnek Belgelerim bir test dosyası oluşturur, Blob depolama alanına yükler, kapsayıcıda BLOB'ları listeler ve sonra eski ve yeni dosyalar karşılaştırmak için dosyanın yeni bir adla yükler. 

F5 tuşuna basarak örneğini çalıştırın. Çıktı aşağıdakine benzer bir konsol penceresinde gösterir: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Devam etmek için herhangi bir tuşa basın, depolama kapsayıcısı ve dosyaları siler. Devam etmeden önce MyDocuments için iki dosyalarına bakın. Ekleri açmak ve aynı görebilirsiniz. Konsol penceresi dışında blob URL'sini kopyalayın ve Blob depolama alanına dosyasının içeriğini görüntülemek için bir tarayıcıya yapıştırın.

Bir aracı gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Blob storage'da dosyaları görüntülemek için. Azure Depolama Gezgini, depolama hesabı bilgilerinizi erişmenize olanak sağlayan ücretsiz bir platformlar arası aracıdır. 

Dosyaları doğrulandıktan sonra tanıtım ve test dosyalarını silmeniz herhangi bir tuşa basın. Örnek yaptığı bildiğinize göre kodu aramak için Program.cs dosyasını açın. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Ardından, böylece nasıl çalıştığını anlamanız biz örnek kodda yol.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleri başvuruları alma

Yapılacak ilk şey erişmek ve Blob Depolama yönetmek için kullanılan nesnelerin referansları oluşturmaktır. Bu nesneler birbirine yapı--her listedeki bir sonraki tarafından kullanılır.

* Bir örneğini oluşturmak **CloudStorageAccount** depolama hesabına işaret eden nesne. 

* Bir örneğini oluşturmak **CloudBlobClient** depolama hesabınızdaki Blob hizmetine işaret nesnesi. 

* Bir örneğini oluşturmak **CloudBlobContainer** erişme kapsayıcı temsil eden nesne. Kapsayıcıları dosyalarınızı düzenlemek için bilgisayarınızda klasörleri kullanmak gibi bloblarınızın düzenlemek için kullanılır.

Bulduktan sonra **CloudBlobContainer**, örneğini oluşturduğunuz **CloudBlockBlob** , ilgilendiğiniz ve karşıya yükleme, yükleme, kopyalama, vb. gerçekleştirmek belirli blob'u işaret nesnesi. işlem.

> [!IMPORTANT]
> Kapsayıcı adları küçük harf olmalıdır. Bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) kapsayıcı ve blob adları hakkında daha fazla bilgi için.

Bu bölümde, nesne örneğini oluşturmak, yeni bir kapsayıcı oluşturmak ve böylece BLOB genel ve yalnızca bir URL ile erişilebilir kapsayıcısında izinleri ayarlama. Kapsayıcı adı verilen **quickstartblobs**. 

Bu örnekte **CreateIfNotExists** her zaman yeni bir kapsayıcı oluşturmak istiyoruz çünkü örneğini çalıştırın. Bir uygulama boyunca aynı kapsayıcı kullandığınız bir üretim ortamında, yalnızca çağırmak için daha iyi bir uygulamadır **CreateIfNotExists** sonra. Alternatif olarak, sizin için önceden kapsayıcısı oluşturmak için bu kodu oluşturmanız gerekir.

```csharp
// Create a CloudStorageAccount instance pointing to your storage account.
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs");
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions();
permissions.PublicAccess = BlobContainerPublicAccessType.Blob;
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>BLOB kapsayıcıya karşıya yükle

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en yaygın olarak kullanılır ve bu hızlı başlangıç içinde kullanılan olmasıdır. 

Bir blobu bir dosyayı karşıya yüklemek için hedef kapsayıcıda blob başvurusu alın. Blob başvurusu edindiğinizde, veri kendisine kullanarak karşıya yükleyebilirsiniz [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Bu işlem zaten mevcut değil veya zaten mevcut değilse bu raporun üzerine blob oluşturur.

Örnek kod, yükleme ve indirme, olarak karşıya yüklenecek dosyayı depolamak için kullanılacak yerel bir dosya oluşturur **fileAndPath** ve içinde blob adını **YerelDosyaAdı**. Aşağıdaki örnek dosya adında, kapsayıcıya yüklemeleri **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string fileAndPath = Path.Combine(localPath, localFileName);
File.WriteAllText(fileAndPath, "Hello, World!");

//Upload the file.
CloudBlockBlob blockBlob = container.GetBlockBlobReference(localFileName);
await blockBlob.UploadFromFileAsync(fileAndPath);
```

Blob storage ile kullanabileceğiniz çeşitli karşıya yükleme yöntemler vardır. Örneğin, bir bellek akış varsa UploadFromFileAsync yerine UploadFromStreamAsync yöntemi kullanabilirsiniz. 

Blok blobları, metin veya ikili dosya herhangi bir türde olabilir. Sayfa blobları Iaas sanal makineleri yedeklemek için kullanılan VHD dosyalarını için birincil olarak kullanılır. Ekleme blobları gibi bir dosyaya yazmak ve daha fazla bilgi ekleme tutmak istediğiniz günlük için kullanılır. BLOB storage'da depolanan çoğu blok blobları nesneleridir.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Kapsayıcı kullanarak dosyaların bir listesini alabilirsiniz [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). Aşağıdaki kod BLOB'lar listesini alır ve ardından bunları bulunan blobların URI'ler gösteren döngü. Komut penceresinde URI'yi kopyalayın ve dosyayı görüntülemek için bir tarayıcıya yapıştırın.

5.000 veya daha az sayıda BLOB kapsayıcısında varsa, tüm blob adları ListBlobsSegmentedAsync yapılan bir çağrı alınır. 5. 000'den fazla BLOB kapsayıcısında varsa, tüm blob adları alınmadı kadar hizmet 5.000 kümesi listesinde getirir. Bu API ilk kez adlı şekilde ilk 5.000 blob adları ve devamlılık belirteci döndürür. İkinci kez belirteç sağlar ve hizmet blob adları bir sonraki kümesini alır. ve devamlılık belirteci kadar tüm blob adları alınmadı olduğunu gösteren, vb. NULL'dur. 

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    foreach(IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null);
```

### <a name="download-blobs"></a>Blob’ları indirme

Kullanarak yerel disk blobları indirmek [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

Aşağıdaki kod, önceki bölümde, yerel diskteki hem dosyaları görebilmeniz için blob adı "_DOWNLOADED" sonekine ekleme karşıya blob indirir. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı karşıya BLOB'ları artık ihtiyacınız varsa, tüm kapsayıcı kullanarak silebilirsiniz [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). Ayrıca artık gerekli değilse oluşturulan dosyaları silin.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç yerel disk ve .NET kullanarak Azure Blob storage arasında dosyaları aktarmak nasıl öğrendiniz. Blob storage ile çalışma hakkında daha fazla bilgi için nasıl yapılır Blob depolama alanına devam edin.

> [!div class="nextstepaction"]
> [BLOB Depolama işlemleri nasıl yapılır konuları](storage-dotnet-how-to-use-blobs.md)

İndirme ve çalıştırma, ek Azure Storage kod örnekleri görmek için listesini [.NET kullanarak Azure Storage örnekleri](../common/storage-samples-dotnet.md).

BLOB'ları ve Depolama Gezgini hakkında daha fazla bilgi için bkz: [Depolama Gezgini ile yönetme Azure Blob storage kaynaklarını](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
