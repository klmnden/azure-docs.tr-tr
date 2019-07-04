---
title: 'Hızlı Başlangıç: .NET için Azure Blob Depolama istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için Azure Blob Depolama istemci kitaplığı (nesne) Blob Depolama alanında bir kapsayıcı ve blob oluşturmak için nasıl kullanılacağını öğrenin. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: mhopkins
ms.subservice: blobs
ms.openlocfilehash: c5e9981c6854ff778775631f1d671189830e564b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67435762"
---
# <a name="quickstart-azure-blob-storage-client-library-for-net"></a>Hızlı Başlangıç: .NET için Azure Blob Depolama istemci kitaplığı

.NET için Azure Blob Depolama istemci kitaplığı ile başlayın. Azure Blob Depolama Microsoft'un nesne depolama için bulut çözümüdür. Temel görevleri için örnek kod deneyin ve paketi yüklemek için adımları izleyin. BLOB Depolama, büyük miktarda yapılandırılmamış verileri depolamak için optimize edilmiştir.

Azure Blob Depolama istemci kitaplığı için .NET için kullanın:

* Bir kapsayıcı oluşturma
* Bir kapsayıcı izinleri ayarlama
* Azure depolamada blob oluşturma
* Blobu yerel bilgisayarınıza indirme
* Tüm bir kapsayıcıdaki blobları listeleme
* Kapsayıcı silme

[API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/storage?view=azure-dotnet) | [kitaplığı kaynak kodunu](https://github.com/Azure/azure-storage-net/tree/master/Blob) | [paketini (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/) | [örnekleri](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği - [ücretsiz oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı - [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* [NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core), veya sonraki bir işletim sistemi için

## <a name="setting-up"></a>Ayarlama

Bu bölüm, bir proje .NET için Azure Blob Depolama istemci kitaplığı ile çalışacak şekilde hazırlama konusunda size yol gösterir.

### <a name="create-the-project"></a>Proje oluşturma

İlk olarak adlandırılan bir .NET Core uygulaması oluşturmak **blob-quickstart**.

1. (Örneğin, cmd, PowerShell veya Bash) bir konsol penceresinde `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komut **blob-quickstart**. Bu komut, bir basit "Hello World" oluşturur C# tek bir dosya ile proje: **Program.cs**.

   ```console
   dotnet new console -n blob-quickstart
   ```

2. Yeni oluşturulan geçiş **blob-quickstart** klasörünü ve tüm iyi durumda olduğunu doğrulamak için uygulama oluşturma.

   ```console
   cd blob-quickstart
   ```

   ```console
   dotnet build
   ```

Beklenen bir derlemenin çıktısı aşağıdakine benzer görünmelidir:

```output
C:\QuickStarts\blob-quickstart> dotnet build
Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 44.31 ms for C:\QuickStarts\blob-quickstart\blob-quickstart.csproj.
  blob-quickstart -> C:\QuickStarts\blob-quickstart\bin\Debug\netcoreapp2.1\blob-quickstart.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:03.08
```

### <a name="install-the-package"></a>Paketi yükleyin

Uygulama dizininde hala, .NET paketi için Azure Blob Depolama istemci kitaplığı kullanarak yükleme `dotnet add package` komutu.

```console
dotnet add package Microsoft.Azure.Storage.Blob
```

### <a name="set-up-the-app-framework"></a>Uygulama Çerçevesi Kurulumu

Proje dizininden:

1. Düzenleyicinizde Program.cs dosyasını açın
2. Kaldırma **Console.WriteLine** deyimi
3. Ekleme **kullanarak** yönergeleri
4. Oluşturma bir **ProcessAsync** örneğin ana kod bulunacağı yöntemi
5. Zaman uyumsuz çağırma **ProcessAsync** yönteminden **ana**

Kod aşağıdaki gibidir:

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;

namespace blob_quickstart
{
    class Program
    {
        public static void Main()
        {
            Console.WriteLine("Azure Blob Storage - .NET quickstart sample\n");

            // Run the examples asynchronously, wait for the results before proceeding
            ProcessAsync().GetAwaiter().GetResult();

            Console.WriteLine("Press any key to exit the sample application.");
            Console.ReadLine();
        }

        private static async Task ProcessAsync()
        {
        }
    }
}
```

### <a name="copy-your-credentials-from-the-azure-portal"></a>Azure portalından kimlik bilgilerinizi kopyalama

Örnek uygulamanın, depolama hesabınıza erişim için kimlik doğrulaması gerçekleştirmesi gerekir. Kimlik doğrulaması gerçekleştirmek için, depolama hesabınızın kimlik bilgilerini uygulamaya bir bağlantı dizesi olarak ekleyin. Bu adımları izleyerek depolama hesabı kimlik bilgilerinizi görüntüleyin:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Depolama hesabınızı bulun.
3. Depolama hesabına genel bakışın **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Burada, hesap erişim anahtarlarınızı ve her anahtar için tam bağlantı dizesini görüntüleyebilirsiniz.
4. **key1** bölümünde **Bağlantı dizesi** değerini bulun ve **Kopyala** düğmesini seçerek bağlantı dizesini kopyalayın. Sonraki adımda bir ortam değişkenine bağlantı dizesini ekleyeceksiniz.

    ![Azure portalından bağlantı dizesinin kopyalanmasını gösteren ekran görüntüsü](../../../includes/media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Bağlantı dizenizi kopyaladıktan sonra uygulamayı çalıştıran yerel makine üzerindeki yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. Değiştirin `<yourconnectionstring>` gerçek bağlantı dizenizle.

Ortam değişkenini ekledikten sonra ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, düzenleyici olarak Visual Studio kullanıyorsanız, Visual Studio örneği çalıştırmadan önce yeniden başlatın.

#### <a name="windows"></a>Windows

```cmd
setx STORAGE_CONNECTION_STRING "<yourconnectionstring>"
```

#### <a name="linux"></a>Linux

```bash
export STORAGE_CONNECTION_STRING="<yourconnectionstring>"
```

#### <a name="macos"></a>macOS

```bash
export STORAGE_CONNECTION_STRING="<yourconnectionstring>"
```

## <a name="object-model"></a>Nesne modeli

Azure Blob Depolama, büyük miktarda yapılandırılmamış verileri depolamak için optimize edilmiştir. Yapılandırılmamış verileri belirli bir veri modeli veya tanım, metin veya ikili veri gibi kalmıyor verilerdir. BLOB Depolama üç tür kaynak sunar:

* Depolama hesabı.
* Depolama hesabında bir kapsayıcı
* Bir kapsayıcıdaki blob

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob Depolama mimarisi diyagramı](./media/storage-quickstart-blobs-dotnet/blob1.png)

Bu kaynaklarla etkileşim kurmak için aşağıdaki .NET sınıfları kullanın:

* [CloudStorageAccount](/dotnet/api/microsoft.azure.storage.cloudstorageaccount): **CloudStorageAccount** sınıfı, Azure depolama hesabınızın temsil eder. Bu sınıf, hesap erişim anahtarlarınızı kullanarak Blob Depolama erişim yetkisi vermek için kullanın.
* [CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient): **CloudBlobClient** sınıfı kodunuzda Blob hizmetine erişim noktası sağlar.
* [CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer): **CloudBlobContainer** sınıfı, bir blob kapsayıcısı, kodunuzda temsil eder.
* [CloudBlockBlob](//dotnet/api/microsoft.azure.storage.blob.cloudblockblob): **CloudBlockBlob** nesne kodunuzda bir blok blobu temsil eder. Blok blobları, ayrı ayrı yönetilebilen veri bloklarından oluşur.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, .NET için Azure Blob Depolama istemci kitaplığı ile aşağıdakileri yapmak nasıl gösterilmektedir:

   * [İstemci kimlik doğrulaması](#authenticate-the-client)
   * [Bir kapsayıcı oluşturma](#create-a-container)
   * [Bir kapsayıcı izinleri ayarlama](#set-permissions-on-a-container)
   * [Bir kapsayıcıya BLOB yükleme](#upload-blobs-to-a-container)
   * [Bir kapsayıcıdaki blobları Listele](#list-the-blobs-in-a-container)
   * [Blobları indirin](#download-blobs)
   * [Kapsayıcı silme](#delete-a-container)

### <a name="authenticate-the-client"></a>İstemci kimlik doğrulaması

Aşağıdaki kod, ortam değişkenini oluşturmak üzere ayrıştırılabilecek bir bağlantı dizesi içerip içermediğini denetler bir [CloudStorageAccount](/dotnet/api/microsoft.azure.storage.cloudstorageaccount?view=azure-dotnet) depolama hesabına işaret eden bir nesne. Bağlantı dizesinin geçerli olup olmadığını denetlemek için [TryParse](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.tryparse?view=azure-dotnet) yöntemini kullanın. **TryParse** başarılı olursa *storageAccount* değişkenini başlatır ve **true** değerini döndürür.

İçinde bu kod ekleme **ProcessAsync** yöntemi:

```csharp
// Retrieve the connection string for use with the application. The storage 
// connection string is stored in an environment variable on the machine 
// running the application called STORAGE_CONNECTION_STRING. If the 
// environment variable is created after the application is launched in a 
// console or with Visual Studio, the shell or application needs to be closed
// and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("STORAGE_CONNECTION_STRING");

// Check whether the connection string can be parsed.
CloudStorageAccount storageAccount;
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // If the connection string is valid, proceed with operations against Blob
    // storage here.
    // ADD OTHER OPERATIONS HERE
}
else
{
    // Otherwise, let the user know that they need to define the environment variable.
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add an environment variable named 'STORAGE_CONNECTION_STRING' with your storage " +
        "connection string as a value.");
    Console.WriteLine("Press any key to exit the application.");
    Console.ReadLine();
}
```

> [!NOTE]
> Bu makalede rest işlemlerini gerçekleştirmek için değiştirin **/ / buraya başka işlemler ADD** aşağıdaki bölümlerde yer alan kod parçacıkları ile koddaki.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Kapsayıcıyı oluşturmak için öncelikle [CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient) nesnesinin depolama hesabınızdaki Blob depolama alanına işaret eden bir örneğini oluşturun. Ardından, [CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer) nesnesinin bir örneğini ve sonra kapsayıcıyı oluşturun.

Bu durumda, kodu çağıran [CreateAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createasync) kapsayıcıyı oluşturmak için yöntemi. Kapsayıcı adının benzersiz olduğundan emin olmak için kapsayıcı adına bir GUID değeri eklenir. Bir üretim ortamında, bunu genellikle kullanılması tercih [Createıfnotexistsasync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createifnotexistsasync) yalnızca, zaten yoksa, bir kapsayıcı oluşturmak için yöntemi.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

```csharp
// Create the CloudBlobClient that represents the 
// Blob storage endpoint for the storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs' and 
// append a GUID value to it to make the name unique.
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs" + 
        Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();
```

### <a name="set-permissions-on-a-container"></a>Bir kapsayıcı izinleri ayarlama

Kapsayıcıdaki tüm blobların herkese açık olacak şekilde kapsayıcıdaki izinleri ayarlayın. Bir blob herkese açık ise, herhangi bir istemci tarafından anonim olarak erişilebilir.

```csharp
// Set the permissions so the blobs are public.
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-a-container"></a>Bir kapsayıcıya BLOB yükleme

Aşağıdaki kod parçacığı bir başvuru alır bir **CloudBlockBlob** çağırarak [Cloudblockblob](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getblockblobreference) önceki bölümde oluşturulan kapsayıcı üzerinde yöntemi. Seçili yerel dosya yüklemeleri için blob çağırarak, ardından [UploadFromFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.uploadfromfileasync) yöntemi. Bu yöntem, daha önce oluşturulmadıysa bir blob oluşturur, aksi takdirde üzerine yazar.

```csharp
// Create a file in your local MyDocuments folder to upload to a blob.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string sourceFile = Path.Combine(localPath, localFileName);
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

Kullanarak bir kapsayıcıdaki blobları listelemek [ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync) yöntemi. Bu durumda, yalnızca bir blob listeleme işlemi döndürecek şekilde yalnızca bir blob kapsayıcıya eklendi.

Tek çağrıda döndürülecek çok fazla sayıda blob (varsayılan olarak 5000'den fazla) varsa, **ListBlobsSegmentedAsync** yöntemi toplam sonuç kümesinin bir segmentini ve bir devamlılık belirtecini döndürür. Blobların sonraki segmentini almak için, devamlılık belirteci null olana kadar önceki çağrı tarafından döndürülen devamlılık belirtecini art arda sağlayın. Null devamlılık belirteci tüm blobların alındığını gösterir. Kod devamlılık belirteci en iyi uygulamalar gösterilmektedir.

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

Kullanarak yerel dosya sisteminize daha önce oluşturulan blobu indirme [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync) yöntemi. Her iki dosyayı yerel dosya sisteminde görebilmeniz için örnek kod için blob adına "_downloaded"son ekini son eki ekler.

```csharp
// Download the blob to a local file, using the reference created earlier.
// Append the string "_DOWNLOADED" before the .txt extension so that you 
// can see both files in MyDocuments.
string destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);
```

### <a name="delete-a-container"></a>Kapsayıcı silme

Aşağıdaki kod, uygulamayı kullanarak kapsayıcının tamamını silerek kaynakları temizlediğinden [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteasync). Ayrıca isterseniz yerel dosyaları silebilirsiniz.

```csharp
Console.WriteLine("Press the 'Enter' key to delete the example files, " +
    "example container, and exit the application.");
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

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, yerel bir sınama dosyası oluşturur. **MyDocuments** klasörü ve Blob depolama alanına yükler. Örnek kapsayıcı içindeki blobları listeler ve eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir.

Düzenleyici olarak Visual Studio kullanıyorsanız **F5** tuşuna basarak çalıştırın.

Aksi takdirde uygulama dizininize gidin sonra oluşturun ve uygulamayı çalıştırın.

```console
dotnet build
```

```console
dotnet run
```

Örnek uygulama çıktısı aşağıdaki örneğe benzer:

```output
Azure Blob storage - .NET Quickstart example

Created container 'quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f'

Temp file = C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-96
97c748f4db.txt
Uploading to Blob storage as blob 'QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f
4db.txt'

Listing blobs in container.
https://storagesamples.blob.core.windows.net/quickstartblobs33c90d2a-eabd-4236-
958b-5cc5949e731f/QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt

Downloading blob to C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926
-a9da-9697c748f4db_DOWNLOADED.txt

Press any key to delete the example files and example container.
```

Devam etmek için **Enter** tuşuna bastığınızda uygulama, depolama kapsayıcısını ve dosyaları siler. Silmeden önce, **MyDocuments** klasörünüzde iki dosyayı denetleyin. Dosyaları açarak aynı olduklarını görebilirsiniz. Konsol penceresinden blob URL’sini kopyalayıp tarayıcıya yapıştırarak blobun içeriğini görüntüleyin.

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için herhangi bir tuşa basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta .NET kullanarak blobları karşıya yükleme, indirme ve listeleme hakkında bilgi edindiniz.

Blob depolama alanına görüntü yükleyen bir web uygulamasının nasıl oluşturulacağını öğrenmek için devam edin:

> [!div class="nextstepaction"]
> [Karşıya yükleme ve görüntü işleme](storage-upload-process-images.md)

* .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
* Windows için Visual Studio’dan dağıtabileceğiniz örnek bir uygulamayı incelemek için bkz. [Azure Blob Depolama ile .NET Fotoğraf Galerisi Web Uygulaması Örneği](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/).
