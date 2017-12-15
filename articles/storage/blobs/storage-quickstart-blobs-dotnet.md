---
title: "Azure Hızlı Başlangıç - .NET kullanarak nesneleri Azure Blob depolama içine/dışına aktarma | Microsoft Docs"
description: ".NET kullanarak nesneleri Azure Blob depolama içine/dışına aktarmayı kısa sürede öğrenin"
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
ms.openlocfilehash: ca4cb2dea9cdd2e46c3aef042e525acdfc09de8e
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>.NET kullanarak nesneleri Azure Blob depolama içine/dışına aktarma

Bu hızlı başlangıçta, kapsayıcıdaki blok bloblarını karşıya yüklemek, indirmek ve listelemek için Azure Depolama için .NET istemci kitaplığını nasıl kullanabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)’yi aşağıdaki iş yüküyle yükleyin:

- **Azure geliştirme**

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan örnek uygulama, temel bir konsol uygulamasıdır. 

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)’i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Visual Studio çözümünü açmak için, storage-blobs-dotnet-quickstart klasörünü bulun, açın ve storage-blobs-dotnet-quickstart.sln'ye çift tıklayın. 

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Visual Studio'da Çözüm Gezgini'nden `app.config` dosyasını açın. `StorageConnectionString` girdisini bulun. **value** için, bağlantı dizesi değerinin tamamını Azure Portal'da kaydettiğiniz değerle değiştirin. `storageConnectionString` aşağıdakine benzer olmalıdır:

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

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek, Belgelerim’de bir test dosyası oluşturur, bunu Blob depolamaya yükler, blobları kapsayıcıda listeler ve ardından eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir. 

F5 tuşuna basarak örneği çalıştırın. Çıkışı, aşağıdakine benzer bir konsol penceresinde gösterir: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Devam etmek için herhangi bir tuşa bastığınızda, depolama kapsayıcısını ve dosyaları siler. Devam etmeden önce, iki dosya için Belgelerim'i kontrol edin. Dosyaları açarak aynı olduklarını görebilirsiniz. Blob depolamadaki dosyanın içeriğini görüntülemek için, blobun URL'sini konsol penceresinden kopyalayın ve tarayıcıya yapıştırın.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için herhangi bir tuşa basın. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için Program.cs dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk yapılacak olan, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir; her dosya, listede yanında yer alan dosya tarafından kullanılır.

* **CloudStorageAccount** nesnesinin depolama hesabına işaret eden bir örneğini oluşturun. 

* **CloudBlobClient** nesnesinin, depolama hesabınızdaki Blob hizmetine işaret eden bir örneğini oluşturun. 

* **CloudBlobContainer** nesnesinin, eriştiğiniz kapsayıcıyı temsil eden bir örneğini oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için kullanılır.

**CloudBlobContainer** nesneniz olduktan sonra, ilgilendiğiniz belirli bir bloba işaret eden bir **CloudBlockBlob** nesnesi örneği oluşturabilir ve karşıya yükleme, indirme, kopyalama gibi işlemler yapabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcı ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Bu bölümde, nesnelerin örneğini oluşturuyor, yeni kapsayıcı oluşturuyor ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak ve bloblara yalnızca bir URL ile erişilebilecek şekilde ayarlıyorsunuz. Bu kapsayıcının adı **quickstartblobs**’dur. 

Örnek her çalıştırıldığında yeni bir kapsayıcı oluşturmak istediğimizden, bu örnekte **CreateIfNotExists** komutu kullanılır. Uygulamanın tamamında aynı kapsayıcıyı kullandığınız bir üretim ortamında, **CreateIfNotExists**’i yalnızca bir kez çağırmanız önerilir. Alternatif olarak, kapsayıcıyı önceden oluşturursanız kodda kapsayıcı oluşturmanıza gerek kalmaz.

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

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en sık kullanılan bloblardır ve bu hızlı başlangıçta bu bloblar kullanılmıştır. 

Dosyayı bloba yüklemek için, hedef kapsayıcıda bloba bir başvuru alın. Blob başvurusunu aldıktan sonra, [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync) kullanarak verileri karşıya yükleyebilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob zaten varsa blobun üzerine yazılır.

Örnek kod, karşıya yükleme ve indirme için kullanılacak yerel bir dosya oluşturur, karşıya yüklenecek dosyayı **fileAndPath** olarak ve blob adını **localFileName** olarak depolar. Aşağıdaki örnek, dosyayı **quickstartblobs** adlı kapsayıcınıza yükler.

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

Blob depolamayla kullanabileceğiniz çeşitli karşıya yükleme yöntemleri vardır. Örneğin bir bellek akışınız varsa, UploadFromFileAsync yerine UploadFromStreamAsync yöntemini kullanabilirsiniz. 

Blok blobları herhangi bir metin veya ikili dosya türünde olabilir. Sayfa blobları öncelikli olarak IaaS VM'leri desteklemek için kullanılan VHD dosyalarında kullanılır. Ekleme blobları, bir dosyaya yazıp sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync) kullanarak kapsayıcıdaki dosyaların listesini alabilirsiniz. Aşağıdaki kod blob listesini alır, ardından bu bloblarda döngü yapar ve bulunan blobların URI değerlerini gösterir. Dosyayı görüntülemek için URI değerini komut penceresinden kopyalayıp tarayıcıya yapıştırabilirsiniz.

Kapsayıcıda 5.000 veya daha az blobunuz varsa, tüm blob adları tek bir ListBlobsSegmentedAsync çağrısında alınır. Kapsayıcıda 5.000'den fazla blobunuz varsa, hizmet tüm blob adları alınana kadar listeyi 5.000 blobluk kümeler halinde alır. Dolayısıyla bu API ilk kez çağrıldığında, ilk 5.000 blob adını ve bir de devamlılık belirteci döndürür. İkinci seferde, siz belirteci sağlarsınız ve hizmet bir sonraki blob adları kümesini alır; tüm blob adlarının alındığını gösterecek şekilde devamlılık belirteci null değerine sahip olana kadar bu şekilde devam eder. 

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

[CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync) kullanarak blobları yerel diskinize indirin.

Aşağıdaki kod önceki bir bölümde karşıya yüklenmiş olan blobu indirir; yerel diskinizde her iki dosyayı da görebilmeniz için indirilen blobun adına "_DOWNLOADED" son ekini koyar. 

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

Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync) kullanarak kapsayıcının tamamını silebilirsiniz. Artık gerekmiyorsa oluşturulmuş olan dosyaları da silin.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları .NET kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama Nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-dotnet-how-to-use-blobs.md)

İndirip çalıştırabileceğiniz başka Azure Depolama kod örnekleri için lütfen [.NET kullanılan Azure Depolama örnekleri](../common/storage-samples-dotnet.md) listesine bakın.

Depolama Gezgini ve Bloblar hakkında daha fazla bilgi için bkz. [Azure Blob depolama kaynaklarını Depolama Gezgini'yle yönetme](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
