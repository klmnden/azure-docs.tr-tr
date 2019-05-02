---
title: 'Hızlı Başlangıç: Bir blob nesne depolama - Azure depolama alanında oluşturmak için .NET kullanma'
description: Bu hızlı başlangıçta, Blob (nesne) depolama alanında bir blob ve kapsayıcı oluşturmak amacıyla .NET için Azure Depolama istemci kitaplığının nasıl kullanılacağını öğreneceksiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: tamram
ms.openlocfilehash: 2708efc22d373db6ee55dfee6b8adfa35bd450ef
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924333"
---
# <a name="quickstart-use-net-to-create-a-blob-in-object-storage"></a>Hızlı Başlangıç: NET'i kullanarak nesne depolamada blob oluşturma

Bu hızlı başlangıçta, Blob (nesne) depolama alanında bir blob ve kapsayıcı oluşturmak amacıyla .NET için Azure Depolama istemci kitaplığının nasıl kullanılacağını öğreneceksiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Ardından, işletim sisteminiz için .NET Core 2.0’ı indirip yükleyin. Windows kullanıyorsanız, isterseniz Visual Studio yükleyip .NET Framework kullanabilirsiniz. Ayrıca, işletim sisteminizle birlikte kullanmak için bir düzenleyici yüklemeyi seçebilirsiniz.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

- [Windows için .NET Core](https://www.microsoft.com/net/download/windows) veya [.NET Framework](https://www.microsoft.com/net/download/windows) (Windows için Visual Studio ile birlikte gelir) yükleyin
- [Windows için Visual Studio](https://www.visualstudio.com/) yükleyin. .NET Core kullanıyorsanız, Visual Studio yüklenmesi isteğe bağlıdır.  

.NET Core ve .NET Framework arasında seçim yapma hakkında bilgi için bkz: [Sunucu uygulamaları için .NET Framework ve .NET Core arasında seçim yapma](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

- [Linux için .NET Core](https://www.microsoft.com/net/download/linux) yükleyin
- İsteğe bağlı olarak [Visual Studio Code](https://www.visualstudio.com/) ve [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp&dotnetid=963890049.1518206068) yükleyin

# <a name="macostabmacos"></a>[macOS](#tab/macos)

- [macOS için .NET Core](https://www.microsoft.com/net/download/macos) yükleyin.
- İsteğe bağlı olarak [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) yükleyin

---

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan örnek uygulama, temel bir konsol uygulamasıdır. [GitHub](https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart) üzerindeki örnek uygulamayı inceleyebilirsiniz.

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Visual Studio çözümünü açmak için, *storage-blobs-dotnet-quickstart* klasörünü bulun, açın ve *storage-blobs-dotnet-quickstart.sln*'ye çift tıklayın.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamayı çalıştırmak istiyorsanız, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Örnek uygulama, bağlantı dizesini ortam değişkeninden okur ve Azure Depolama’ya yönelik istekleri yetkilendirmek için bu dizeyi kullanır.

Bağlantı dizenizi kopyaladıktan sonra uygulamayı çalıştıran yerel makine üzerindeki yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. `<yourconnectionstring>` değerini gerçek bağlantı dizenizle değiştirin:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

Ortam değişkenini ekledikten sonra, konsol penceresi de dahil olmak üzere ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, düzenleyici olarak Visual Studio kullanıyorsanız, örneği çalıştırmadan önce Visual Studio’yu yeniden başlatın.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export storageconnectionstring=<yourconnectionstring>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

.bash_profile dosyanızı düzenleyin ve ortam değişkenini ekleyin:

```bash
export storageconnectionstring=<yourconnectionstring>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

---

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek, yerel **MyDocuments** klasörünüzde bir sınama dosyası oluşturur ve Blob depolama alanına yükler. Örnek daha sonra kapsayıcı içindeki blobları listeler ve eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Düzenleyici olarak Visual Studio kullanıyorsanız **F5** tuşuna basarak çalıştırın.

Aksi takdirde uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```console
dotnet run
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```console
dotnet run
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```console
dotnet run
```

---

Örnek uygulama çıktısı aşağıdaki örneğe benzer:

```output
Azure Blob storage - .NET Quickstart sample

Created container 'quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f'

Temp file = C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt
Uploading to Blob storage as blob 'QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt'

Listing blobs in container.
https://storagesamples.blob.core.windows.net/quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f/QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt

Downloading blob to C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db_DOWNLOADED.txt

Press any key to delete the sample files and example container.
```

Devam etmek için **Enter** tuşuna bastığınızda uygulama, depolama kapsayıcısını ve dosyaları siler. Silmeden önce, **MyDocuments** klasörünüzde iki dosyayı denetleyin. Dosyaları açarak aynı olduklarını görebilirsiniz. Konsol penceresinden blob URL’sini kopyalayıp tarayıcıya yapıştırarak blobun içeriğini görüntüleyin.

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için herhangi bir tuşa basın. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için Program.cs dosyasını açabilirsiniz.

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Ardından, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyin.

### <a name="try-parsing-the-connection-string"></a>Bağlantı dizesini ayrıştırmayı deneyin

Örneğin yaptığı ilk işlem, ortam değişkeninin depolama hesabını işaret eden bir [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) nesnesi oluşturmak üzere ayrıştırılabilecek bir bağlantı dizesi içerip içermediğini denetlemektir. Bağlantı dizesinin geçerli olup olmadığını denetlemek için [TryParse](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.tryparse) yöntemini kullanın. **TryParse** başarılı olursa *storageAccount* değişkenini başlatır ve **true** değerini döndürür.

```csharp
// Retrieve the connection string for use with the application. The storage connection string is stored
// in an environment variable on the machine running the application called storageconnectionstring.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell or application needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");

// Check whether the connection string can be parsed.
CloudStorageAccount storageAccount;
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // If the connection string is valid, proceed with operations against Blob storage here.
    ...
}
else
{
    // Otherwise, let the user know that they need to define the environment variable.
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add an environment variable named 'storageconnectionstring' with your storage " +
        "connection string as a value.");
    Console.WriteLine("Press any key to exit the sample application.");
    Console.ReadLine();
}
```

### <a name="create-the-container-and-set-permissions"></a>Kapsayıcı oluşturma ve izinleri ayarlama

Daha sonra örnek, bir kapsayıcı oluşturur ve kapsayıcıdaki tüm blobların herkese açık olması için izinlerini ayarlar. Bir blob herkese açık ise, herhangi bir istemci tarafından anonim olarak erişilebilir.

Kapsayıcıyı oluşturmak için öncelikle [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient) nesnesinin depolama hesabınızdaki Blob depolama alanına işaret eden bir örneğini oluşturun. Ardından, [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer) nesnesinin bir örneğini ve sonra kapsayıcıyı oluşturun.

Bu durumda örnek, kapsayıcıyı oluşturmak için [CreateAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createasync) yöntemini çağırır. Kapsayıcı adının benzersiz olduğundan emin olmak için kapsayıcı adına bir GUID değeri eklenir. Bir üretim ortamında kapsayıcı oluştururken, yalnızca henüz mevcut değilse ve adlandırma çakışmalarını önlemek için [CreateIfNotExistsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexistsasync) yönteminin kullanılması tercih edilir.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

```csharp
// Create the CloudBlobClient that represents the Blob storage endpoint for the storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs' and append a GUID value to it to make the name unique.
CloudBlobContainer cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();

// Set the permissions so the blobs are public.
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Ardından örnek, blok blobuna yerel bir dosya yükler. Kod örneği, önceki bölümde oluşturulan kapsayıcı üzerinde [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference) yöntemini çağırarak bir **CloudBlockBlob** nesnesine başvuru alır. Daha sonra [UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync) yöntemini çağırarak seçili dosyayı bloba yükler. Bu yöntem, daha önce oluşturulmadıysa bir blob oluşturur, aksi takdirde üzerine yazar.

```csharp
// Create a file in your local MyDocuments folder to upload to a blob.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

Console.WriteLine("Temp file = {0}", sourceFile);
Console.WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the blob address, then upload the file to the blob.
// Use the value of localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Örnek, [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync) yöntemini kullanarak blobları kapsayıcıda listeler. Örnek söz konusu olduğunda, kapsayıcıya yalnızca bir blob eklendiği için listeleme işlemi yalnızca bir blob döndürür.

Tek çağrıda döndürülecek çok fazla sayıda blob (varsayılan olarak 5000'den fazla) varsa, **ListBlobsSegmentedAsync** yöntemi toplam sonuç kümesinin bir segmentini ve bir devamlılık belirtecini döndürür. Blobların sonraki segmentini almak için, devamlılık belirteci null olana kadar önceki çağrı tarafından döndürülen devamlılık belirtecini art arda sağlayın. Null devamlılık belirteci tüm blobların alındığını gösterir. Örnek kod, en iyi uygulamalar için devamlılık belirtecinin nasıl kullanılacağını gösterir.

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    // Get the value of the continuation token returned by the listing call.
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null); // Loop while the continuation token is not null.

```

### <a name="download-blobs"></a>Blob’ları indirme

Sonra örnek, daha önce oluşturulan blobu [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync) yöntemini kullanarak yerel dosya sisteminize indirir. Örnek kod, her iki dosyayı da yerel dosya sisteminde görebilmeniz için blob adına "_DOWNLOADED" son ekini ekler.

```csharp
// Download the blob to a local file, using the reference created earlier.
// Append the string "_DOWNLOADED" before the .txt extension so that you can see both files in MyDocuments.
destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);  
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek, [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync) ile tüm kapsayıcıyı silerek oluşturduğu kaynakları temizler. Ayrıca isterseniz yerel dosyaları silebilirsiniz.

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

## <a name="resources-for-developing-net-applications-with-blobs"></a>Bloblarla .NET uygulamaları geliştirme kaynakları

Blob depolama ile .NET geliştirmeye yönelik şu ek kaynaklara bakın:

### <a name="binaries-and-source-code"></a>İkili dosyalar ve kaynak kodu

- Azure Storage için [.NET istemci kitaplığının](https://www.nuget.org/packages/WindowsAzure.Storage/) en son sürümüne yönelik NuGet paketini indirin.
- GitHub üzerinde [.NET istemci kitaplığı kaynak kodunu](https://github.com/Azure/azure-storage-net) görüntüleyin.

### <a name="client-library-reference-and-samples"></a>İstemci kitaplığı başvurusu ve örnekleri

- .NET istemci kitaplığı hakkında daha fazla bilgi için bkz. [.NET API başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/storage).
- .NET istemci kitaplığı kullanılarak yazılmış [Blob depolama örneklerini](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob) araştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta .NET kullanarak blobları karşıya yükleme, indirme ve listeleme hakkında bilgi edindiniz.

Blob depolama alanına görüntü yükleyen bir web uygulaması oluşturma hakkında bilgi almak için [Azure Depolama ile görüntü verilerini buluta yükleme](storage-upload-process-images.md).

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-dotnet-how-to-use-blobs.md)

- .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
- Windows için Visual Studio’dan dağıtabileceğiniz örnek bir uygulamayı incelemek için bkz. [Azure Blob Depolama ile .NET Fotoğraf Galerisi Web Uygulaması Örneği](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/).
