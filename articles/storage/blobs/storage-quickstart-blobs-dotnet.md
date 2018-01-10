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
ms.date: 12/04/2017
ms.author: tamram
ms.openlocfilehash: 5020f070a8eb9215f175fc3ff3a905cff28ce37f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>.NET kullanarak nesneleri Azure Blob depolama içine/dışına aktarma

Bu hızlı başlangıçta, kapsayıcıdaki blok bloblarını karşıya yüklemek, indirmek ve listelemek için Azure Depolama için .NET istemci kitaplığını nasıl kullanabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* [Linux](/dotnet/core/linux-prerequisites?tabs=netcore2x) veya [Windows](/dotnet/core/windows-prerequisites?tabs=netcore2x) için .NET core 2.0 uygulamasını yükleyin

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

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Bu bağlantı dizesini uygulamayı çalıştıran yerel makine üzerindeki bir ortam değişkeninde depolamanız önerilir. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.  \<yourconnectionstring\> yerine gerçek bağlantı dizenizi yerleştirin.

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```
### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek, Belgelerim’de bir test dosyası oluşturur, bunu Blob depolamaya yükler, blobları kapsayıcıda listeler ve ardından eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir. 

Uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```
dotnet run
```

Gösterilen çıkış aşağıdaki örneğe benzer:

```
Azure Blob storage quick start sample
Temp file = /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Uploading to Blob storage as blob 'QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt'
List blobs in container.
https://mystorageaccount.blob.core.windows.net/quickstartblobs/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Downloading blob to /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374_DOWNLOADED.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

Devam etmek için herhangi bir tuşa bastığınızda, depolama kapsayıcısını ve dosyaları siler. Devam etmeden önce, iki dosya için Belgelerim'i kontrol edin. Dosyaları açarak aynı olduklarını görebilirsiniz. Blob depolamadaki dosyanın içeriğini görüntülemek için, blobun URL'sini konsol penceresinden kopyalayın ve tarayıcıya yapıştırın.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için herhangi bir tuşa basın. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için Program.cs dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk yapılacak olan, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir ve her bir dosya, listede yanında yer alan dosya tarafından kullanılır.

* [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet) nesnesinin depolama hesabına işaret eden bir örneğini oluşturun.

* [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet) nesnesinin, depolama hesabınızdaki Blob hizmetine işaret eden bir örneğini oluşturun.

* [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet) nesnesinin, eriştiğiniz kapsayıcıyı temsil eden bir örneğini oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için kullanılır.

[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet) nesneniz olduktan sonra, ilgilendiğiniz belirli bir bloba işaret eden bir [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet) nesnesi örneği oluşturabilir ve karşıya yükleme, indirme, kopyalama gibi işlemler yapabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcı ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Bu bölümde, nesnelerin örneğini oluşturuyor, yeni kapsayıcı oluşturuyor ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak ve bloblara yalnızca bir URL ile erişilebilecek şekilde ayarlıyorsunuz. Bu kapsayıcının adı **quickstartblobs**’dur.

Örnek her çalıştırıldığında yeni bir kapsayıcı oluşturmak istediğimizden, bu örnekte [CreateIfNotExists](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexists?view=azure-dotnet) komutu kullanılır. Uygulamanın tamamında aynı kapsayıcıyı kullandığınız bir üretim ortamında, **CreateIfNotExists**’i yalnızca bir kez çağırmanız önerilir. Alternatif olarak, kapsayıcıyı önceden oluşturursanız kodda kapsayıcı oluşturmanıza gerek kalmaz.

```csharp
// Load the connection string for use with the application. The storage connection string is stored
// in an environment variable on the machine running the application called storageconnectionstring.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");
if (storageConnectionString == null)
{
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add a environment variable name 'storageconnectionstring' with the actual storage " +
        "connection string as a value.");
}
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConnectionString);

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
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
sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

Console.WriteLine("Temp file = {0}", sourceFile);
Console.WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the location where the blob is going to go, then upload the file.
// Upload the file you created, use localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);
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
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
    blobContinuationToken = results.ContinuationToken;
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
destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);  
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync) kullanarak kapsayıcının tamamını silebilirsiniz. Artık gerekmiyorsa oluşturulmuş olan dosyaları da silin.

```csharp
Console.WriteLine("Press the 'Enter' key to delete the sample files, example container, and exit the application.");
Console.ReadLine();
// Clean up resources. This includes the container and the two temp files.
Console.WriteLine("Deleting the container");
if (cloudBlobContainer != null)
{
    await cloudBlobContainer.DeleteIfExistsAsync();
}
Console.WriteLine("Deleting the source, and downloaded files");
File.Delete(sourceFile);
File.Delete(destinationFile);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları .NET kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama Nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-dotnet-how-to-use-blobs.md)

İndirip çalıştırabileceğiniz başka Azure Depolama kod örnekleri için lütfen [.NET kullanılan Azure Depolama örnekleri](../common/storage-samples-dotnet.md) listesine bakın.

Depolama Gezgini ve Bloblar hakkında daha fazla bilgi için bkz. [Azure Blob depolama kaynaklarını Depolama Gezgini'yle yönetme](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
