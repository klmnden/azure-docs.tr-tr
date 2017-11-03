---
title: "Microsoft Azure Storage veri hareketi kitaplığı ile veri aktarımı | Microsoft Docs"
description: "Veri hareketi kitaplığı taşımak veya için veya blob ve dosya içeriğinden veri kopyalamak için kullanın. Verileri Azure depolama birimine yerel dosyalarından kopyalamak veya içinde veya depolama hesapları arasında veri kopyalama. Kolayca verilerinizi Azure depolama alanına geçiş."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/27/2017
ms.author: seguler
ms.openlocfilehash: 7890159574de0db58dd2e7d1b6a19305381d29d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a>Microsoft Azure Storage veri hareketi kitaplığı ile veri aktarımı

## <a name="overview"></a>Genel Bakış
Microsoft Azure Storage veri hareketi kitaplığı karşıya yükleme, indirme ve Azure Storage Bloblarında ve dosyaları kopyalama yüksek performans için tasarlanmış bir platformlar arası açık kaynak kitaplıktır. ' In temelini oluşturan çekirdek veri hareketi altyapısını bu kitaplığıdır [AzCopy](../storage-use-azcopy.md). Veri hareketi kitaplığı bizim geleneksel kullanılamaz kullanışlı yöntemler sağlar [.NET Azure Storage istemci Kitaplığı](../blobs/storage-dotnet-how-to-use-blobs.md). Bu paralel işlem sayısını ayarlayın, aktarma işleminin ilerleme durumunu izlemek, iptal edilen bir aktarımı ve çok daha fazlasını kolayca sürdürme yeteneğini içerir.  

Bu kitaplık ayrıca .NET uygulamaları için Windows, Linux ve macOS oluştururken kullanabileceğiniz anlamına gelir .NET Core kullanır. .NET Core hakkında daha fazla bilgi için bkz [.NET Core belgeleri](https://dotnet.github.io/). Bu kitaplık, geleneksel .NET Framework uygulamaları için Windows için de çalışır. 

Bu belge, Windows, Linux ve macOS üzerinde çalışır ve aşağıdaki senaryolarda gerçekleştiren bir .NET Core konsol uygulaması oluşturmak nasıl gösterir:

- Dosyalar ve dizinler Blob depolama alanına karşıya yükleyin.
- Paralel işlem sayısı, veri aktarırken tanımlayın.
- Veri aktarımı gelişimi izleme.
- İptal edilen veri aktarımını devam edin. 
- Dosya URL'den Blob depolama alanına kopyalayın. 
- BLOB depolama alanından Blob depolama alanına kopyalayın.

**Gerekenler:**

* [Visual Studio Code](https://code.visualstudio.com/)
* Bir [Azure Storage hesabı](storage-create-storage-account.md#create-a-storage-account)

> [!NOTE]
> Bu kılavuz, zaten aşina olduğunuzu varsayar [Azure Storage](https://azure.microsoft.com/services/storage/). Değilse, okuma, [Azure Storage'a giriş](storage-introduction.md) belgelerine yararlıdır. En önemlisi, gerek [depolama hesabı oluşturma](storage-create-storage-account.md#create-a-storage-account) veri hareketi kitaplığı kullanmaya başlamak için.
> 
> 

## <a name="setup"></a>Kurulum  

1. Ziyaret [.NET Core Yükleme Kılavuzu](https://www.microsoft.com/net/core) .NET Core yüklemek için. Ortamınızı seçerken, komut satırı seçeneğini seçin. 
2. Komut satırından projeniz için bir dizin oluşturun. Gidin bu dizine yazın `dotnet new console -o <sample-project-name>` bir C# konsol projesi oluşturmak için.
3. Bu dizin Visual Studio Code açın. Bu adım hızla komut satırı yazarak yapılabilir `code .` Windows.  
4. Yükleme [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio kod marketten. Visual Studio Code'u yeniden başlatın. 
5. Bu noktada, iki istem görmeniz gerekir. "Derleme ve hata ayıklamak için gerekli varlıklar." eklemek için biridir "Evet" i tıklatın Çözümlenmemiş bağımlılıkları geri yüklemek için başka bir uyarı içindir. "Geri yükleme".
6. Değiştirme `launch.json` altında `.vscode` dış terminal bir konsol olarak kullanılacak. Bu ayarı olarak okumanız gerekir` "console": "externalTerminal"`
7. Visual Studio Code .NET Core uygulamaları ayıklamanızı sağlar. İsabet `F5` uygulamanızı çalıştırın ve kurulumunuzu çalışır durumda olduğunu doğrulayın. "Hello World!" görmeniz gerekir konsola yazdırılır. 

## <a name="add-data-movement-library-to-your-project"></a>Veri hareketi kitaplığı projenize ekleyin

1. Veri hareketi kitaplığı için en son sürümünü eklemek `dependencies` bölümü, `<project-name>.csproj` dosya. Bu sürüm yazma zaman olacaktır`"Microsoft.Azure.Storage.DataMovement": "0.6.2"` 
2. Projenizi geri yüklemek için bir istem görüntülemelidir. "Geri yükleme" düğmesini tıklatın. Ayrıca, projenizin komut satırından komutu yazarak geri yükleyebilirsiniz `dotnet restore` proje dizininiz kök.

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

## <a name="set-up-the-skeleton-of-your-application"></a>Uygulamanızı çatıyı ayarlayın
Bunu yapmadan ilk şey, uygulamamız "çatıyı" kod ayarlanır. Bu kod bize için bir depolama hesabı adı ve hesap anahtarı ister ve oluşturmak için bu kimlik bilgilerini kullanan bir `CloudStorageAccount` nesnesi. Bu nesnenin tüm aktarımı senaryolarda bizim depolama hesabı ile etkileşim kurmak için kullanılır. Kod ayrıca bize aktarım işlemi yürütmek için isteriz türünü seçmek için istenir. 

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

## <a name="transfer-local-file-to-azure-blob"></a>Azure Blob yerel dosya aktarımı
Yöntemleri eklemek `GetSourcePath` ve `GetBlob` için `Program.cs`:

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

Bu kod bize yolunu yerel bir dosya, yeni veya mevcut bir kapsayıcı adı ve yeni blob adını ister. `TransferManager.UploadAsync` Yöntemi bu bilgileri kullanarak karşıya yükleme gerçekleştirir. 

İsabet `F5` uygulamanızı çalıştırmak için. Karşıya depolama hesabınızla görüntüleyerek oluştu doğrulayabilirsiniz [Microsoft Azure Storage Gezgini](http://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Paralel işlem sayısını ayarlama
Veri hareketi kitaplığı tarafından sunulan bir büyük veri aktarımı verimliliğini artırmak için paralel işlem sayısını ayarlama özelliği özelliğidir. Varsayılan olarak, veri hareketi kitaplığı paralel işlem sayısı 8'e ayarlar * makinenizde çekirdek sayısı. 

Düşük bant genişlikli ortamında çok sayıda paralel işlemleri ağ bağlantısı doldurmaya ve gerçekte tam olarak tamamlanmasını işlemlerini önlemek aklınızda bulundurun. Kullanılabilir ağ bant genişliğinizi neler en iyi tabanlı çalışır belirlemek için bu ayarı denemeniz gerekir. 

Bize paralel işlem sayısını ayarlamak sağlayan biraz kod ekleyelim. Ayrıca aktarımı tamamlamak gereken süreyi zaman kod ekleyelim.

Ekleme bir `SetNumberOfParallelOperations` yönteme `Program.cs`:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Değiştirme `ExecuteChoice` yöntemini kullanmak üzere `SetNumberOfParallelOperations`:

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

Değiştirme `TransferLocalFileToAzureBlob` süreölçer kullanılacak yöntemi:

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

## <a name="track-transfer-progress"></a>Aktarım gelişimi izleme
Bizim veri aktarmak sürdü ne kadar süreyle bilerek mükemmeldir. Bizim aktarımı ilerlemesini ancak geçirebilmek *sırasında* aktarım işlemi daha iyi olacaktır. Bu senaryo elde etmek için oluşturmamız gerekir bir `TransferContext` nesnesi. `TransferContext` Nesnesi iki biçimde gelir: `SingleTransferContext` ve `DirectoryTransferContext`. Eski (olan ne biz şimdi yaptığınız) tek dosya aktarmak için ve ikinci bir dizin (daha sonra ekliyoruz) dosyaları aktarmak için.

Yöntemleri eklemek `GetSingleTransferContext` ve `GetDirectoryTransferContext` için `Program.cs`: 

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

Değiştirme `TransferLocalFileToAzureBlob` yöntemini kullanmak üzere `GetSingleTransferContext`:

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

## <a name="resume-a-canceled-transfer"></a>İptal edilen bir aktarımı Sürdür
Veri hareketi kitaplığı tarafından sunulan başka bir uygun iptal edilmiş bir aktarımı sürdürebilme özelliğidir. Geçici olarak yazarak aktarımı iptal kurmamızı sağlayan biraz kod ekleyelim `c`ve ardından aktarımı 3 saniye sonra sürdürebilirsiniz.

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

Şimdiye kadar bizim `checkpoint` değeri her zaman sınıflandırmalara ayarlandığı `null`. Şimdi, biz aktarımı iptal ederseniz, biz bizim aktarımı son denetim noktası almak ve sonra bu yeni bir kontrol noktası bizim aktarımı bağlamda kullanın. 

## <a name="transfer-local-directory-to-azure-blob-directory"></a>Yerel dizin Azure Blob dizinine aktarma
Veri hareketi kitaplığı aynı anda yalnızca bir dosya aktarımı istendiği olacaktır. Bu durumda değil. Veri hareketi kitaplığı dosyaları ve tüm alt dizinlerinin dizini transfer olanağı sağlar. Bunu kurmamızı sağlayan biraz kod ekleyelim.

İlk olarak, yöntemi ekleyin `GetBlobDirectory` için `Program.cs`:

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

Ardından, değiştirin `TransferLocalDirectoryToAzureBlobDirectory`:

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

Bu yöntem ve tek bir dosyayı karşıya yükleme için kullanılan yöntem arasındaki bazı farklar vardır. Şu anda kullanmakta olduğunuz `TransferManager.UploadDirectoryAsync` ve `getDirectoryTransferContext` daha önce oluşturduğumuz yöntemi. Ayrıca, artık sağlıyoruz bir `options` bizim karşıya alt dizinleri dahil etmek istiyoruz belirtmek kurmamızı sağlayan bizim karşıya yükleme işlemi için değer. 

## <a name="copy-file-from-url-to-azure-blob"></a>Dosyayı Azure Blob URL'den kopyalayın
Şimdi, bize bir dosyayı Azure Blob için bir URL'den kopyalamak veren kod ekleyelim. 

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

Bu özellik için bir önemli kullanım örneği, verileri başka bir bulut hizmetinden (örneğin, AWS) Azure'a taşımak gereken zamandır. Kaynağa erişim sağlayan bir URL'ye sahip olduğu sürece, kolayca bu kaynak Azure BLOB'ları kullanarak taşıyabilirsiniz `TransferManager.CopyAsync` yöntemi. Bu yöntem aynı zamanda yeni bir boolean parametresiyle sunar. Bu parametre ayarını `true` zaman uyumsuz bir sunucu tarafı kopya yapmak istiyoruz gösterir. Bu parametre ayarını `false` kaynak bizim yerel makineye ilk indirilir, sonra Azure Blob karşıya anlamı zaman uyumlu kopyası - gösterir. Ancak, zaman uyumlu kopyası şu anda yalnızca bir Azure depolama kaynağının diğerine kopyalamak için kullanılabilir. 

## <a name="transfer-azure-blob-to-azure-blob"></a>Azure Blob Azure Blob Aktarım
Benzersiz olarak veri hareketi kitaplığı tarafından sağlanan başka bir Azure depolama kaynağının diğerine kopyalama becerisini özelliğidir. 

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

Bu örnekte, biz Boole parametresi kümesinde `TransferManager.CopyAsync` için `false` biz zaman uyumlu bir kopya yapmak istediğinizi belirtmek için. Bu kaynak bizim yerel makineye ilk indirilir, sonra Azure Blob karşıya olduğunu anlamına gelir. Zaman uyumlu kopyası seçeneği, kopyalama işlemi tutarlı bir hızda olduğundan emin olmak için harika bir yoludur. Buna karşılık, zaman uyumsuz bir sunucu tarafı kopyası hızına kullanılabilir ağ bant genişliği dalgalanma sunucuya bağlı. Ancak, zaman uyumlu kopyası zaman uyumsuz kopyaya karşılaştırıldığında ek çıkışı maliyeti oluşturabilir. Önerilen yaklaşım çıkışı maliyeti önlemek için kaynak depolama hesabınız ile aynı bölgede olan Azure VM'deki zaman uyumlu kopyası kullanmaktır.

## <a name="conclusion"></a>Sonuç
Veri taşıma uygulamamız tamamlanmıştır. [Tam kod örneği Github'da kullanılabilir](https://github.com/azure-samples/storage-dotnet-data-movement-library-app). 

## <a name="next-steps"></a>Sonraki adımlar
Bu Başlarken'de, Azure Storage ile etkileşim kurar ve Windows, Linux ve macOS üzerinde çalışan bir uygulama oluşturduk. Bu Başlarken Blob depolamaya odaklı. Ancak, aynı bu bilgiye dosya depolama birimine uygulanabilir. Daha fazla bilgi için kullanıma [Azure Storage veri hareketi kitaplığı başvuru belgeleri](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]




