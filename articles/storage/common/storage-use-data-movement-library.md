---
title: Microsoft Azure Storage veri hareketi kitaplığı ile veri aktarma | Microsoft Docs
description: Veri taşıma kitaplığı, taşıyın veya için veya blob ve dosya içeriği veri kopyalamak için kullanın. Yerel dosyaları Azure depolama alanına veri kopyalama veya içinde veya depolama hesapları arasında verileri kopyalayabilirsiniz. Kolayca verilerinizi Azure Depolama'ya geçirin.
services: storage
author: seguler
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 09/27/2017
ms.author: seguler
ms.subservice: common
ms.openlocfilehash: 0641a097761530285c2dd9aa176ddd8c2c159001
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58878749"
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a>Microsoft Azure Storage veri hareketi kitaplığı ile veri aktarma

## <a name="overview"></a>Genel Bakış
Microsoft Azure Storage veri hareketi kitaplığı karşıya yükleme, indirme ve Azure depolama Blobları ve dosyaları kopyalama yüksek performans için tasarlanmış bir platformlar arası, açık kaynak bir kitaplıktır. Bu bir kitaplıktır çalıştıran çekirdek veri hareketi altyapısını [AzCopy](../storage-use-azcopy.md). Veri taşıma kitaplığı geleneksel bizim kullanılamayan kullanışlı yöntemler sunar [.NET Azure depolama istemci Kitaplığı](../blobs/storage-dotnet-how-to-use-blobs.md). Bu, paralel işlemleri sayısını ayarlayın, aktarma işleminin ilerleme durumunu izlemek, iptal edilen bir aktarım ve daha fazlasını kolayca sürdürme olanağı içerir.

Bu kitaplık, .NET Core, Windows, Linux ve macOS için .NET uygulamaları oluşturma sırasında kullanabileceğiniz anlamına gelir de kullanır. .NET Core hakkında daha fazla bilgi için bkz [.NET Core belgeleri](https://dotnet.github.io/). Bu kitaplık, geleneksel .NET Framework uygulamaları için Windows için de kullanılabilir.

Bu belge Windows, Linux ve macOS üzerinde çalışır ve aşağıdaki senaryolarda gerçekleştiren bir .NET Core konsol uygulaması oluşturma işlemini göstermektedir:

- Blob depolama alanına, dosyaları ve dizinleri karşıya yükleyin.
- Paralel işlem sayısı, veri aktarırken tanımlayın.
- Veri aktarımı ilerlemesini izle.
- Veri aktarımı sürdürme iptal edildi.
- Dosya URL'den Blob depolama alanına kopyalayın.
- BLOB depolama alanından, Blob depolama alanına kopyalayın.

**Gerekenler:**

* [Visual Studio Code](https://code.visualstudio.com/)
* Bir [Azure Storage hesabı](storage-quickstart-create-account.md)

> [!NOTE]
> Bu kılavuz, zaten aşina olduğunuzu varsayar [Azure depolama](https://azure.microsoft.com/services/storage/). Değilse, okunuyorsa [Azure Storage'a giriş](storage-introduction.md) belgeleri yararlıdır. En önemlisi de yapmanız [depolama hesabı oluşturma](storage-quickstart-create-account.md) veri hareketi kitaplığı kullanmaya başlamak için.
>
>

## <a name="setup"></a>Kurulum

1. Ziyaret [.NET Core Yükleme Kılavuzu](https://www.microsoft.com/net/core) .NET Core yüklemek için. Ortamınızı seçerken, komut satırı seçeneğini seçin.
2. Komut satırından, projeniz için bir dizin oluşturun. Git bu dizine yazın `dotnet new console -o <sample-project-name>` bir C# konsol projesi oluşturmak için.
3. Bu dizin, Visual Studio Code'da açın. Bu adım hızla ile komut satırını yazarak yapılabilir `code .` Windows içinde.
4. Yükleme [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio kod Market. Visual Studio Code'u yeniden başlatın.
5. Bu noktada, iki istem görmeniz gerekir. Biridir "derleme ve hata ayıklamak için gerekli varlıkları." eklemek için "Evet" seçeneğine tıklayın Çözümlenmemiş bağımlılıklar geri yüklemek için başka bir uyarı içindir. "Geri yükleme".
6. Değiştirme `launch.json` altında `.vscode` dış terminal bir konsol olarak kullanılacak. Bu ayar olarak okumanız gerekir `"console": "externalTerminal"`
7. Visual Studio Code, .NET Core uygulamalarında hata ayıklamak sağlar. İsabet `F5` uygulamanızı çalıştırın ve kurulumunuzu çalıştığını doğrulayın. "Hello World!" görmeniz gerekir konsola yazdırılır.

## <a name="add-data-movement-library-to-your-project"></a>Veri taşıma kitaplığı projenize ekleyin.

1. Veri taşıma kitaplığı için en son sürümünü eklemek `dependencies` bölümünü, `<project-name>.csproj` dosya. Makalenin yazıldığı sırada bu sürümü olacaktır `"Microsoft.Azure.Storage.DataMovement": "0.6.2"`
2. Projenizi geri yüklemek için bir istem görüntülemelidir. "Geri yükle" düğmesine tıklayın. Ayrıca, projenizi komut satırından komutu yazarak geri yükleyebilirsiniz `dotnet restore` , proje dizininizin kökünde.

Değiştirme `<project-name>.csproj`:

    <Project Sdk="Microsoft.NET.Sdk">

        <PropertyGroup>
            <OutputType>Exe</OutputType>
            <TargetFramework>netcoreapp2.0</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
            <PackageReference Include="Microsoft.Azure.Storage.DataMovement" Version="0.6.2" />
            </ItemGroup>
        </Project>

## <a name="set-up-the-skeleton-of-your-application"></a>Uygulamanızın çatıyı ayarlayın
Yapmamız gereken ilk şey, uygulamamız "skeleton" kod ayarlanır. Bu kod ABD için bir depolama hesabı adını ve hesap anahtarını ister ve oluşturmak için bu kimlik bilgilerini kullanan bir `CloudStorageAccount` nesne. Bu nesne, aktarımı senaryolarda tüm depolama hesabımız ile etkileşim kurmak için kullanılır. Kod aktarım işlemi yürütmek için istiyoruz türü seçmek için bize ister.

Değiştirme `Program.cs`:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Diagnostics;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.DataMovement;

namespace DMLibSample
{
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("Enter Storage account name:");
            string accountName = Console.ReadLine();

            Console.WriteLine("\nEnter Storage account key:");
            string accountKey = Console.ReadLine();

            string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + accountName + ";AccountKey=" + accountKey;
            CloudStorageAccount account = CloudStorageAccount.Parse(storageConnectionString);

            ExecuteChoice(account);
        }

        public static void ExecuteChoice(CloudStorageAccount account)
        {
            Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
            int choice = int.Parse(Console.ReadLine());

            if(choice == 1)
            {
                TransferLocalFileToAzureBlob(account).Wait();
            }
            else if(choice == 2)
            {
                TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
            }
            else if(choice == 3)
            {
                TransferUrlToAzureBlob(account).Wait();
            }
            else if(choice == 4)
            {
                TransferAzureBlobToAzureBlob(account).Wait();
            }
        }

        public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
        {

        }

        public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
        {

        }
    }
}
```

## <a name="transfer-local-file-to-azure-blob"></a>Azure Blob için yerel dosya aktarımı
Yöntemler `GetSourcePath` ve `GetBlob` için `Program.cs`:

```csharp
public static string GetSourcePath()
{
    Console.WriteLine("\nProvide path for source:");
    string sourcePath = Console.ReadLine();

    return sourcePath;
}

public static CloudBlockBlob GetBlob(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    Console.WriteLine("\nProvide name of new Blob:");
    string blobName = Console.ReadLine();
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    return blob;
}
```

Değiştirme `TransferLocalFileToAzureBlob` yöntemi:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    await TransferManager.UploadAsync(localFilePath, blob);
    Console.WriteLine("\nTransfer operation complete.");
    ExecuteChoice(account);
}
```

Bu kod yolunu yerel bir dosya, yeni veya var olan bir kapsayıcının adını ve yeni bir blob adı için bize ister. `TransferManager.UploadAsync` Yöntemi, bu bilgileri kullanarak karşıya yükleme gerçekleştirir.

İsabet `F5` uygulamanızı çalıştırmak için. Karşıya yükleme, depolama hesabınızla görüntüleyerek oluştuğunu doğrulayabilirsiniz [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Paralel işlem kümesi sayısı
Veri taşıma kitaplığı tarafından sunulan harika bir özellik, veri aktarımı verimliliğini artırmak için paralel işlem sayısını ayarlamak için olanağıdır. Varsayılan olarak, veri taşıma kitaplığı paralel işlem sayısı 8'e ayarlar * makinenizde çekirdek sayısı.

Düşük bant genişliğine sahip bir ortamda çok sayıda paralel işlemleri ağ bağlantısı doldurmaya ve gerçekten tam tamamlanmasını işlemlerini önlemek aklınızda bulundurun. Kullanılabilir ağ bant genişliğinizi ne en iyi temel yapılandırmayı belirlemek için bu ayarı ile denemeniz gerekir.

Kurmamızı paralel işlem sayısını ayarlamak biraz kod ekleyelim. Ayrıca ne kadar aktarımı tamamlamak gereken zaman kod ekleyelim.

Ekleme bir `SetNumberOfParallelOperations` yönteme `Program.cs`:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Değiştirme `ExecuteChoice` yönteminin kullanılacağını `SetNumberOfParallelOperations`:

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
    int choice = int.Parse(Console.ReadLine());

    SetNumberOfParallelOperations();

    if(choice == 1)
    {
        TransferLocalFileToAzureBlob(account).Wait();
    }
    else if(choice == 2)
    {
        TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
    }
    else if(choice == 3)
    {
        TransferUrlToAzureBlob(account).Wait();
    }
    else if(choice == 4)
    {
        TransferAzureBlobToAzureBlob(account).Wait();
    }
}
```

Değiştirme `TransferLocalFileToAzureBlob` yöntemi bir zamanlayıcı kullanmak için:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="track-transfer-progress"></a>Aktarım ilerlemesini İzle
Ne kadar verilerimizi aktarmak sürdüğünü bilerek harika bir yoldur. Ancak, bizim aktarımı ilerlemesini görmek işaretleyebilmesine *sırasında* aktarım işlemi daha iyi olacaktır. Bu senaryo elde etmek için oluşturmamız gerekir bir `TransferContext` nesne. `TransferContext` Nesne iki biçimde gelir: `SingleTransferContext` ve `DirectoryTransferContext`. Eski bir tek (şimdi neler yaptığımızın) dosyasını aktarmak için ve ikincisi bir dizin (daha sonra ekliyoruz) dosyaları aktarmak için.

Yöntemler `GetSingleTransferContext` ve `GetDirectoryTransferContext` için `Program.cs`:

```csharp
public static SingleTransferContext GetSingleTransferContext(TransferCheckpoint checkpoint)
{
    SingleTransferContext context = new SingleTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });

    return context;
}

public static DirectoryTransferContext GetDirectoryTransferContext(TransferCheckpoint checkpoint)
{
    DirectoryTransferContext context = new DirectoryTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });

    return context;
}
```

Değiştirme `TransferLocalFileToAzureBlob` yönteminin kullanılacağını `GetSingleTransferContext`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    Console.WriteLine("\nTransfer started...\n");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob, null, context);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="resume-a-canceled-transfer"></a>İptal edilen bir aktarım Sürdür
Veri taşıma kitaplığı tarafından sunulan başka bir uygun iptal edilen bir aktarım sürdürebilme özelliğidir. Geçici olarak yazarak aktarımı iptal et olanak sağlayan biraz kod ekleyelim `c`ve sonra da 3 saniyelik aktarım devam.

Değiştirme `TransferLocalFileToAzureBlob`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.UploadAsync(localFilePath, blob, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadAsync(localFilePath, blob, null, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Şimdiye bizim `checkpoint` değeri her zaman sınıflandırmalara ayarlandığı `null`. Şimdi biz aktarımı iptal ederseniz, biz bizim aktarımı son denetim noktasından almak ve ardından bu yeni bir kontrol noktası bizim aktarımı bağlamda kullanabilir.

## <a name="transfer-local-directory-to-azure-blob-directory"></a>Azure Blob dizin yerel dizine Aktarım
Veri taşıma kitaplığı aynı anda yalnızca bir dosya aktarımı istendiği olurdu. Neyse ki bu durumda değil. Veri taşıma kitaplığı, bir dizin, dosya ve alt dizinlerinde taşıma imkanı sağlar. Bunu sağlıyor biraz kod ekleyelim.

İlk olarak, yöntem ekleme `GetBlobDirectory` için `Program.cs`:

```csharp
public static CloudBlobDirectory GetBlobDirectory(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container. This can be a new or existing Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    CloudBlobDirectory blobDirectory = container.GetDirectoryReference("");

    return blobDirectory;
}
```

Daha sonra değiştirmek `TransferLocalDirectoryToAzureBlobDirectory`:

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account);
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    UploadDirectoryOptions options = new UploadDirectoryOptions()
    {
        Recursive = true
    };

    try
    {
        task = TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetDirectoryTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Bu yöntem ve tek bir dosyayı karşıya yükleme için yöntem arasındaki bazı farklar vardır. Artık kullanıyoruz `TransferManager.UploadDirectoryAsync` ve `getDirectoryTransferContext` daha önce oluşturduğumuz yöntemi. Ayrıca, artık sağlıyoruz bir `options` bizim karşıya yükleme alt dizinleri dahil etmek istiyoruz belirtmek sağlıyor bizim karşıya yükleme işlemi için değer.

## <a name="copy-file-from-url-to-azure-blob"></a>URL'den dosya Azure blob'a Kopyala
Artık, bir Azure Blob için bir URL bir dosyayı kopyalama olanak sağlayan bir kod ekleyelim.

Değiştirme `TransferUrlToAzureBlob`:

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Verileri başka bir bulut hizmetinden (örneğin, AWS) Azure'a taşımak, ihtiyacınız olduğunda bu özellik bir önemli kullanım durum geçerlidir. Kaynağa erişim veren bir URL'ye sahip olduğu sürece, kolayca, kaynak Azure BLOB'ları kullanarak taşıyabilirsiniz `TransferManager.CopyAsync` yöntemi. Bu yöntem, yeni bir Boole parametresi da tanıtılmaktadır. Bu parametre ayarını `true` zaman uyumsuz bir sunucu tarafı kopyalamayı yapmak istediğimizi belirtir. Bu parametre ayarını `false` kaynak yerel makinemizi ilk indirilir ve ardından Azure Blob karşıya anlamı zaman uyumlu kopya - gösterir. Ancak, zaman uyumlu kopyası şu anda yalnızca bir Azure depolama kaynağından diğerine kopyalamak için kullanılabilir.

## <a name="transfer-azure-blob-to-azure-blob"></a>Azure Blob Azure Blobuna aktarma
Benzersiz veri hareketi kitaplığı tarafından sağlanan başka bir Azure depolama kaynağından diğerine kopyalama özelliği özelliğidir.

Değiştirme `TransferAzureBlobToAzureBlob`:

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(sourceBlob, destinationBlob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(sourceBlob, destinationBlob, false, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Bu örnekte biz Boole parametresi kümesinde `TransferManager.CopyAsync` için `false` biz zaman uyumlu kopyasını yapmak istediğinizi belirtmek için. Bu kaynak yerel makinemizi ilk indirilen c:\kullanıcılar\kullanıcı\belgeler gibi Azure Blob karşıya olduğunu anlamına gelir. Zaman uyumlu kopyası seçeneği, kopyalama işlemi tutarlı bir hız olduğundan emin olmak için harika bir yoludur. Buna karşılık, zaman uyumsuz bir sunucu tarafı kopyalama hızına dalgalanma sunucu üzerinde kullanılabilir ağ bant genişliğine bağlıdır. Ancak, zaman uyumlu kopyası için zaman uyumsuz kopya kıyasla ek çıkış maliyet oluşturabilir. Önerilen yaklaşım, çıkış maliyet önlemek için kaynak depolama hesabının aynı bölgede olan bir Azure VM'de zaman uyumlu kopyası kullanmaktır.

## <a name="conclusion"></a>Sonuç
Veri taşıma uygulamamız tamamlanmıştır. [Tam kod örneği, Github'da kullanılabilir](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).

## <a name="next-steps"></a>Sonraki adımlar
Bu Başlarken'de, Azure depolama ile etkileşime geçer ve Windows, Linux ve macOS üzerinde çalışan bir uygulama oluşturduk. Bu Başlarken Blob depolamaya odaklı. Ancak, aynı bu bilgi, dosya depolama için uygulanabilir. Daha fazla bilgi için kullanıma [Azure Storage veri hareketi kitaplığı başvuru belgeleri](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]
