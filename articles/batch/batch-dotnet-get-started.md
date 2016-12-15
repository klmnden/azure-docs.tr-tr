---
title: "Öğretici - Azure Batch.NET kitaplığını kullanmaya başlama | Microsoft Belgeleri"
description: "Azure Batch’in temel kavramlarını ve basit bir senaryoyla Batch hizmetini geliştirmeyi öğrenin."
services: batch
documentationcenter: .net
author: mmacy
manager: timlt
editor: 
ms.assetid: 76cb9807-cbc1-405a-8136-d1e53e66e82b
ms.service: batch
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 11/22/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: ecf07295a2e56e1aae8fc8fce77ca219db1f371e


---
# <a name="get-started-with-the-azure-batch-library-for-net"></a>.NET için Azure Batch itaplığını kullanmaya başlama
> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
>
>

Adım adım C# örnek uygulamasında tartıştığımız gibi, bu makalede [Azure Batch][azure_batch] ve [Batch .NET][net_api] kitaplığı hakkında temel bilgileri öğrenin. Bu örnek uygulamanın dosya hazırlığı ve alımı için [Azure Depolama](../storage/storage-introduction.md) ile nasıl etkileşime girdiğinin yanı sıra bulutta paralel bir iş yükünü işlemek için Batch hizmetinden nasıl yararlandığını da göreceğiz. Ortak Batch uygulama iş akışını öğrenmenin yanı sıra işler, görevler, havuzlar ve işlem düğümü gibi başlıca Batch bileşenleri hakkında da temel bir anlayış kazanacaksınız.

![Batch çözümü iş akışı (temel)][11]<br/>

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, C# ve Visual Studio deneyimine sahip olduğunuz varsayılmaktadır. Azure’ün yanı sıra Batch ve Storage hizmetleri için aşağıda belirtilen hesap oluşturma gerekliliklerini karşılayabildiğiniz de varsayılmaktadır.

### <a name="accounts"></a>Hesaplar
* **Azure hesabı**: Henüz bir Azure aboneliğiniz yoksa, [ücretsiz Azure hesabı oluşturun][azure_free_account].
* **Batch hesabı**: Azure aboneliğiniz olduktan sonra, [Azure Batch hesabı oluşturun](batch-account-create-portal.md).
* **Storage hesabı**: Bkz. [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md) sayfası, [Storage hesabı oluşturma](../storage/storage-create-storage-account.md#create-a-storage-account) bölümü.

> [!IMPORTANT]
> Batch şu anda, [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md) bölümünde 5. adım olan [Depolama hesabı oluşturma](../storage/storage-create-storage-account.md#create-a-storage-account)da açıklandığı gibi, *sadece* **Genel amaçlı** depolama hesabı türünü destekler.
>
>

### <a name="visual-studio"></a>Visual Studio
Örnek proje oluşturmak için **Visual Studio 2015** sürümünüz olmalıdır. Visual Studio'nun ücretsiz ve deneme sürümlerini [Visual Studio 2015 Ürünlerine Genel Bakış][visual_studio] sayfasında bulabilirsiniz.

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial* kodu örneği
[DotNetTutorial][github_dotnettutorial] örneği GitHub’daki [azure-batch-samples][github_samples] deposunda bulunan çok sayıda Batch kodu örneğinden biridir. Örneklerin tümünü, depo giriş sayfasındaki **Kopyala veya indir > ZIP’i İndir**’e veya [azure-batch-samples-master.zip][github_samples_zip] doğrudan indirme bağlantısına tıklayarak indirebilirsiniz. ZIP dosyasının içeriğini ayıkladıktan sonra çözümü aşağıdaki klasörde bulabilirsiniz:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Batch Gezgini (isteğe bağlı)
[Azure Batch Gezgini][github_batchexplorer], GitHub’daki [azure-batch-samples][github_samples] deposunda yer alan ücretsiz bir yardımcı programdır. Bu öğreticiyi tamamlamak için gerekli olmasa da, Batch çözümlerinizi geliştirirken ve hatalarını ayıklarken yararlı olabilir.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial örnek projesine genel bakış
*DotNetTutorial* kod örneği buradaki iki projeden oluşan bir Visual Studio 2015 çözümüdür: **DotNetTutorial** ve **TaskApplication**.

* **DotNetTutorial**, işlem düğümlerinde paralel iş yükünü yürütmek için Batch ve Storage hizmetleriyle etkileşime giren istemci uygulamasıdır (sanal makineler). DotNetTutorial yerel iş istasyonunuzda çalışır.
* **TaskApplication**, asıl işi gerçekleştirmek için Azure’deki işlem düğümlerinde çalışan programdır. Örnekte, `TaskApplication.exe` metni (girdi dosyası), Azure Storage’dan indirilen dosyada ayrıştırıyor. Ardından, girdi dosyasında ilk üç sözcüğün göründüğü listenin bulunduğu bir metin dosyası (çıktı dosyası) oluşturur. TaskApplication, çıktı dosyasını oluşturduktan sonra dosyayı Azure Storage’a yükler. Böylece, indirmek üzere istemci uygulamasının kullanımına hazır hale getirir. TaskApplication, Batch hizmetinde paralel olarak birden çok işlem düğümünde çalışır.

Aşağıdaki diyagram, istemci uygulaması tarafından gerçekleştirilen birincil işlemleri, *DotNetTutorial* ve görevler tarafından yürütülen uygulamayı, *TaskApplication* göstermektedir. Bu temel iş akışı, Batch’le oluşturulan çok sayıda işlem çözümünün tipik halidir. Batch hizmetinde erişilebilir olan özelliklerin hepsini göstermese de, neredeyse tüm Batch senaryoları bu iş akışının bölümlerini içerir.

![Batch örnek iş akışı][8]<br/>

[**1. Adım.**](#step-1-create-storage-containers) Azure Blob Storage’da **kapsayıcılar** oluşturun.<br/>
[**2. Adım.**](#step-2-upload-task-application-and-data-files) Kapsayıcılara görev uygulaması dosyalarını ve girdi dosyalarını yükleyin.<br/>
[**3. Adım.**](#step-3-create-batch-pool) Batch **havuzu** oluşturun.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** **StartTask** havuzu görev ikili dosyalarını (TaskApplication), göreve katıldıkları için düğümlere indirir.<br/>
[**4. Adım.**](#step-4-create-batch-job) Batch **işi** oluşturun.<br/>
[**5. Adım**](#step-5-add-tasks-to-job) İşe **görevler** ekleyin.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Görevler, düğümlerde yürütmek üzere zamanlanır.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Her görev kendi girdi verilerini Azure Storage’dan indirip yürütmeye başlar.<br/>
[**6. Adım**](#step-6-monitor-tasks) Görevleri izleyin.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Görevlerin tamamlanmasıyla, kendi çıktı verilerini Azure Storage’a yükler.<br/>
[**7. Adım**](#step-7-download-task-output) Storage’dan görev çıktısını indirin.

Yukarıda belirtildiği gibi, her Batch çözümü tam olarak bu adımları gerçekleştirmese ve çok daha fazlasını içerebilse de; *DotNetTutorial* örnek uygulaması, Batch çözümünde bulunan ortak işlemleri göstermektedir.

## <a name="build-the-dotnettutorial-sample-project"></a>*DotNetTutorial* örnek projesini derleme
Örneği sorunsuz çalıştırmadan önce, *DotNetTutorial* projesinin `Program.cs` dosyasında Batch ve Storage hesabı kimlik bilgilerini belirtmelisiniz. Henüz bunu yapmadıysanız, Visual Studio'da `DotNetTutorial.sln` çözüm dosyasına çift tıklayarak çözümü açın. Bunun yerine, **Dosya > Aç > Proje/Çözüm** menüsünü kullanarak Visual Studio'da da açabilirsiniz.

*DotNetTutorial* projesinin içinde `Program.cs` öğesini açın. Sonra da kimlik bilgilerinizi belirtildiği gibi dosyanın en üstüne yakın bir yere ekleyin:

```csharp
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> Yukarıda da belirtildiği gibi, Azure Storage’daki kimlik bilgilerini **Genel amaçlı** depolama hesabı için belirtmelisiniz. Toplu Batch uygulamalarınız, **Genel amaçlı** depolama hesabında blob depolama kullanır. *Blob depolama* hesap türünü seçerek oluşturulmuş Storage hesabı için kimlik bilgilerini belirtmeyin.
>
>

Batch ve Depolama hesabı kimlik bilgilerinizi [Azure portalındaki][azure_portal] hizmetlere ilişkin hesap dikey pencerelerinde bulabilirsiniz:

![Portalda Batch kimlik bilgileri][9]
![Portalda Depolama kimlik bilgileri][10]<br/>

Kimlik bilgilerinizle projeyi güncelleştirdiğinizden, Çözüm Gezgini'ndeki çözüme sağ tıklayıp **Çözümü Derle**’ye tıklayın. İstenirse, herhangi bir NuGet paketinin geri yüklenmesini onaylayın.

> [!TIP]
> NuGet paketleri otomatik olarak geri yüklemezse ya da paketlerin geri yüklenmesiyle ilgili hatalar görürseniz, [NuGet Paket Yöneticisi][nuget_packagemgr]’sinin yüklü olduğundan emin olun. Sonra da eksik paketleri indirilmesini etkinleştirin. Paket indirilmesini etkinleştirmek için bkz. [Derleme sırasında paket Geri Yüklemeyi Etkinleştirme][nuget_restore].
>
>

Aşağıdaki bölümlerde, Batch hizmetinde iş yükünü işlemeyi gerçekleştiren örnek uygulamayı adımlara ayırdık ve bu adımlar üzerine ayrıntılı tartıştık. Örneğin her kod satırı tartışılmadığından, bu makalenin kalanında kendi işinizi yaparken Visual Studio’da çözüm açmak için başvurmanızı öneririz.

1. Adımla başlamak için *DotNetTutorial* projesinin `Program.cs` dosyasındaki `MainAsync` yönteminin en üstüne gidin. Aşağıdaki her adım bundan sonra kabaca `MainAsync` içindeki yöntem çağrılarının ilerleyişini izler.

## <a name="step-1-create-storage-containers"></a>1. Adım: Storage kapsayıcıları oluşturma
![Azure Depolama’da kapsayıcı oluşturma][1]
<br/>

Azure Storage ilet etkileşimde bulunmak için Batch’te yerleşik destek bulunur. Storage hesabınızdaki kapsayıcılar, Batch hesabınızda çalışan görevler için gerekli dosyaları sağlar. Kapsayıcılar ayrıca görevlerin oluşturduğu çıktı verilerini depolamak için bir yer sağlar. *DotNetTutorial* istemci uygulamasının yapacağı ilk şey [Azure Blob Storage](../storage/storage-introduction.md)’da üç kapsayıcı oluşturmaktır:

* **uygulama**: Bu kapsayıcı, DLL’ler gibi birçok bağlantının yanı sıra görevlerin yürüttüğü uygulamayı da depolar.
* **girdi**: Görevler, işlemek için veri dosyalarını *girdi* kapsayıcısından yükleyecektir.
* **çıktı**: Görevler girdi dosyası işlemeyi tamamladıklarında, sonuçları*çıktı* kapsayıcısına yüklerler.

Bir Depolama hesabıyla etkileşime geçmek ve kapsayıcılar oluşturmak için, [.NET için Azure Depolama İstemci Kitaplığı][net_api_storage] kullanıyoruz. [CloudStorageAccount][net_cloudstorageaccount] ile hesaba bir başvuru oluşturuyoruz ve buradan da bir [CloudBlobClient][net_cloudblobclient] oluşturuyoruz:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Uygulamanın tamamında `blobClient` başvurusunu kullanıyor ve bunu bir dizi yönteme parametre olarak geçiriyoruz. Bu, hemen yukarıdakini izleyen kod bloğundadır, burada gerçekten de kapsayıcı oluşturmak için buna `CreateContainerIfNotExistAsync` diyoruz.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Kapsayıcılar oluşturulduktan sonra uygulama artık görevler tarafından kullanılacak dosyaları karşıya yükleyebilir.

> [!TIP]
> [Net'ten Blob Storage kullanma](../storage/storage-dotnet-how-to-use-blobs.md), Azure depolama kapsayıcıları ve blob'larla çalışma hakkında kapsamlı bilgi sağlar. Batch’le çalışmaya başladığınızda okuma listenizin en üstüne yakın olması gerekir.
>
>

## <a name="step-2-upload-task-application-and-data-files"></a>2. Adım: Görev uygulamasını ve veri dosyalarını karşıya yükleme
![Görev uygulamasını ve girdi (veriler) dosyalarını kapsayıcılara yükleme][2]
<br/>

Dosyayı karşıya yükleme işleminde, *DotNetTutorial* önce **uygulama** ve **girdi** dosya yolları koleksiyonunu yerel makinede oldukları gibi tanımlar. Sonra da, bir önceki adımda oluşturduğunuz kapsayıcılara bu dosyaları yükler.

```csharp
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Karşıya yükleme işlemini oluşturan `Program.cs` öğesinde iki yöntem vardır:

* `UploadFilesToContainerAsync`: Bu yöntem, [ResourceFile][net_resourcefile] nesnelerinin bir koleksiyonunu döndürür (aşağıda açıklanmıştır) ve *filePaths* parametresine geçirilen her dosyayı karşıya yüklemek için `UploadFileToContainerAsync` çağrısı yapar.
* `UploadFileToContainerAsync`: Dosyayı gerçekten karşıya yüklemeyi gerçekleştiren ve [ResourceFile][net_resourcefile] nesnelerini oluşturan yöntem budur. Dosyayı karşıya yükledikten sonra, dosya için paylaşılan erişim imzasını (SAS) alır ve temsil ettiği bir ResourceFile nesnesini döndürür. Paylaşılan erişim imzaları aşağıda da açıklanmıştır.

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles
[ResourceFile][net_resourcefile], görev çalıştırılmadan önce işlem düğümüne yüklenecek, Azure Depolama’da yer alan bir dosyaya bağlantısı olan URL’ye sahip Batch’teki görevleri sağlar. [ResourceFile.BlobSource][net_resourcefile_blobsource] özelliği, Azure Depolama’da olduğu gibi dosyanın tam URL'sini belirtir. URL’de, dosyaya güvenli erişim sağlayan bir paylaşılan erişim imzası da (SAS) bulunabilir. Batch .NET’teki çoğu görev türünde *ResourceFiles* özelliği vardır; bu özellikte şunlar bulunur:

* [CloudTask][net_task]
* [StartTask][net_pool_starttask]
* [JobPreparationTask][net_jobpreptask]
* [JobReleaseTask][net_jobreltask]

DotNetTutorial örnek uygulaması JobPreparationTask veya JobReleaseTask görev türlerini kullanmaz; ancak, bununla ilgili daha fazla bilgiyi [Azure Batch işlem düğümlerinde iş hazırlama ve tamamlama görevlerini çalıştırma](batch-job-prep-release.md) makalesinden edinebilirsiniz.

### <a name="shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS)
Paylaşılan erişim imzalar, URL parçası olarak eklendiğinde Azure Storage'da kapsayıcılara ve blob’lara güvenli erişim sağlayan dizelerdir. DotNetTutorial uygulaması hem blob, hem de kapsayıcı paylaşılan erişim imzası URL’lerini kullanır ve Storage hizmetinden bu paylaşılan erişim imzalarının nasıl alındığını gösterir.

* **Blob paylaşılan erişim imzaları**: DotNetTutorial’daki havuza ait StartTask, Storage’dan uygulama ikililerini ve girdi veri dosyalarını indirdiğinde blob paylaşılan erişim imzalarını kullanır (bkz. 3. Adım; aşağıda). DotNetTutorial'ın `Program.cs` içindeki `UploadFileToContainerAsync` yönteminde her blob'un paylaşılan erişim imzasını alan kod bulunur. Bu işlemi [CloudBlob.GetSharedAccessSignature][net_sas_blob] çağırarak gerçekleştirir.
* **Kapsayıcı paylaşılan erişim imzaları**: Hesaplama düğümünde her görev işini bitirdiğinde, kendi çıktı dosyasını Azure Storage’daki *çıktı* kapsayıcısına yükler. Bunu yapmak için, TaskApplication dosyayı karşıya yüklediğinde yolun parçası olarak kapsayıcıya yazma izni sağlayan kapsayıcı paylaşılan erişim imzasını kullanır. Kapsayıcı paylaşılan erişim imzası blob paylaşılan erişim imzası alındığında yapılan işleme benzer. DotNetTutorial’de, bunu yapmak için `GetContainerSasUrl` yardımcı yönteminin [CloudBlobContainer.GetSharedAccessSignature][net_sas_container] çağırdığını görürsünüz. TaskApplication’ın kapsayıcı paylaşılan erişim imzasını kullanması hakkında daha fazla bilgi için "6. Adım: İzleme Görevleri" başlığı altındakileri okuyacaksınız.

> [!TIP]
> Storage hesabınızdaki verilere güvenli erişim sağlama hakkında daha fazla bilgi için paylaşılan erişim imzalarındaki iki parçalı seriyi kullanıma alın, [1. Bölüm: Paylaşılan erişim imzası (SAS) modelini anlama](../storage/storage-dotnet-shared-access-signature-part-1.md) ve [2. Bölüm: Blob depolama ile paylaşılan erişim imzasını (SAS) oluşturma ve kullanma](../storage/storage-dotnet-shared-access-signature-part-2.md).
>
>

## <a name="step-3-create-batch-pool"></a>3. Adım: Batch havuzu oluşturma
![Batch havuzu oluşturma][3]
<br/>

Batch **havuzu**, Batch’in işe ait görevleri yürüttüğü işlem düğümlerinin (sanal makineler) koleksiyonudur.

Uygulama ve veri dosyalarını Storage hesabına yükledikten sonra, *DotNetTutorial*, Batch .NET kitaplığını kullanarak Batch hizmetiyle etkileşimi başlatır. Bunu yapmak için önce bir [BatchClient][net_batchclient] oluşturulur:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Ardından, `CreatePoolAsync` çağrısıyla Batch hesabında işlem düğümü havuzu oluşturacak. `CreatePoolAsync`, [BatchClient.PoolOperations.CreatePool][net_pool_create] yöntemini kullanarak bir işlem düğümleri havuzu oluşturur.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

[CreatePool][net_pool_create] ile havuz oluşturduğunuzda, işlem düğümlerinin sayısı, [düğümlerin boyutu](../cloud-services/cloud-services-sizes-specs.md) ve düğümlerin işletim sistemi gibi parametreleri belirtirsiniz. *DotNetTutorial* ’da, [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md)’dan Windows Server 2012 R2'yi belirtmek için [CloudServiceConfiguration][net_cloudserviceconfiguration] kullanırız. Ancak bunun yerine [VirtualMachineConfiguration][net_virtualmachineconfiguration] belirterek, hem Windows hem de Linux görsellerinin yer aldığı Market görsellerinden havuz oluşturabilirsiniz; daha fazla bilgi için bkz. [Azure Batch havuzlarında Linux işlem düğümlerini hazırlama](batch-linux-nodes.md).

> [!IMPORTANT]
> Batch’teki işlem kaynakları ücretlidir. Maliyetleri en aza indirmek için, örneği çalıştırmadan önce `targetDedicated` değerini 1 olarak düşürün.
>
>

Bu fiziksel düğüm özellikleriyle birlikte, havuz için ayrıca bir [StartTask][net_pool_starttask] belirtebilirsiniz. StartTask, her düğümü havuza katıldığında ve her yeniden başlatıldığında yürütecektir. StartTask özellikle, görevler yürütülmeden önce işlem düğümlerine uygulamaların yüklenmesi için yararlıdır. Örneğin, görevleriniz verileri Python betiklerini kullanarak işliyorsa, işlem düğümlerine Python yüklemek için StartTask kullanabilirsiniz.

Bu örnek uygulamasında StartTask, Depolama’dan indirilen dosyaları ([StartTask][net_starttask].[ResourceFiles][net_starttask_resourcefiles] özelliği kullanılarak belirtilir), StartTask çalışma dizininden, erişilebilir düğümdeki *tüm* görevlerin çalıştığı paylaşılan dizine kopyalar. Aslında, `TaskApplication.exe` uygulamasını ve bağlantılarını, düğüm havuza katılmış olduğundan her düğüme kopyalar; bu nedenle düğümde çalışan görevler buna erişebilir.

> [!TIP]
> Azure Batch’in **uygulama paketleri** özelliği, havuzdaki işlem düğümlerinin uygulamanızı almasının başka bir yolunu sağlar. Ayrıntılı bilgi için bkz. [Azure Batch uygulama paketleriyle uygulama dağıtımı](batch-application-packages.md) .
>
>

Yukarıdaki kod parçacığında dikkat çeken bir şey de, StartTask’ın *CommandLine* özelliğinde iki ortam değişkenin kullanılmasıdır: `%AZ_BATCH_TASK_WORKING_DIR%` ve `%AZ_BATCH_NODE_SHARED_DIR%`. Batch havuzundaki her işlem düğümü, Batch’e özel bazı ortam değişkenleriyle yapılandırılmıştır. Görev tarafından yürütülen işlemlerin bu ortam değişkenlerine erişimi vardır.

> [!TIP]
> Batch havuzundaki işlem düğümlerinde bulunan ortam değişkenleri ve görev çalışma dizinleri hakkında daha fazla bilgi almak için [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) içindeki [Görevler için ortam değişkenleri](batch-api-basics.md#environment-settings-for-tasks) ve [Dosya ve dizinler](batch-api-basics.md#files-and-directories) bölümlerine bakın.
>
>

## <a name="step-4-create-batch-job"></a>4. Adım: Batch işi oluşturma
![Batch işi oluşturma][4]<br/>

Batch **işi**, görevler koleksiyonudur ve işlem düğümlerinin bir havuzuyla ilişkilidir. İşteki görevler ilişkili havuzunun işlem düğümlerini yürütür.

İşi yalnızca ilgili iş yüklerinde görevlerin düzenlenmesi ve izlenmesi için değil, aynı zamanda işin (ve buna bağlı olarak görevlerin) en uzun çalışma süresinin yanı sıra Batch hesabındaki diğer işlerle bağlantılı olarak iş önceliği gibi bazı kısıtlamalar getirmek için de kullanabilirsiniz. Ancak bu örnekte, iş yalnızca 3. adımda oluşturulan havuzla ilişkilendirilmektedir. Yapılandırılmış başka ek özellik yoktur.

Tüm Batch işleri belirli bir havuzla ilişkilidir. Bu ilişkilendirme, iş görevlerinin hangi düğümleri yürüteceğini belirtir. Bunu, aşağıdaki kod parçacığında gösterildiği gibi [CloudJob.PoolInformation][net_job_poolinfo] özelliğini kullanarak belirtirsiniz.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

İş oluşturulduğuna göre, artık çalışmak için görevler eklenir.

## <a name="step-5-add-tasks-to-job"></a>5. Adım: İşe görev ekleme
![İşe görev ekleme][5]<br/>
*(1) Görevler işe eklenir, (2) görevler düğümlerde çalışmak üzere zamanlanır ve (3) görevler işlemek üzere veri dosyalarını indirir*

Batch **görevleri**, işlem düğümlerinde yürütülen tek tek iş birimleridir. Görevde bir komut satırı vardır; bu komut satırında belirttiğiniz betikleri veya yürütülebilir dosyaları çalıştırır.

Aslına bakılırsa, çalışmayı gerçekleştirmek için görevlerin işe eklenmesi gerekir. Her [CloudTask][net_task], bir komut satırı özelliği ve komut satırı otomatik olarak yürütülmeden önce görevin düğüme yüklediği [ResourceFiles][net_task_resourcefiles] (havuzdaki StartTask gibi) kullanılarak yapılandırılır. *DotNetTutorial* örnek projesinde her görev yalnızca tek bir dosya işler. Bu nedenle, kendi ResourceFiles koleksiyonunda tek bir öğe bulunmaktadır.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> `%AZ_BATCH_NODE_SHARED_DIR%` gibi ortam değişkenlerine eriştiklerinde veya düğüme ait `PATH` öğesinde bulunmayan bir uygulama yürüttüklerinde görev komut satırları `cmd /c` önekini almalıdır. Bunu kesinlikle komut yorumlayıcı yürütecek ve komutunuzu uyguladıktan sonra sonlandırması talimatını verecektir. Görevleriniz düğümün `PATH` öğesinde (*robocopy.exe* veya *powershell.exe* gibi) bir uygulama yürütüyorsa ve hiç ortam değişkeni kullanılmıyorsa bu gereksinim gereksizdir.
>
>

Yukarıdaki kod parçacığında, `foreach` döngüsü içinde görevle ilgili komut satırının, üç komut satırı bağımsız değişkeni *TaskApplication.exe* dosyasına geçirilecek şekilde oluşturulduğunu görürsünüz:

1. **İlk bağımsız değişken** işlenecek dosyanın yoludur. Düğümde yer aldığından dosyanın yerel yolu budur. `UploadFileToContainerAsync` ResourceFile nesnesi ilk oluşturulduğunda dosya adı bu özellik için kullanılır (parametrenin ResourceFile oluşturucuda yaptığı gibi). Dosyanın *TaskApplication.exe* ile aynı dizinde bulunabileceğini belirtir.
2. **İkinci bağımsız değişken** ilk *N* sayıda sözcüğün çıktı dosyasına yazılması gerektiğini belirtir. Bu, örnekte sabit kodlanmıştır; bu nedenle çıktı dosyasına ilk üç sözcük yazılır.
3. **Üçüncü bağımsız değişken**, Azure Storage’da **çıktı** kapsayıcısına yazma erişimi sağlayan paylaşılan erişim imzasıdır (SAS). *TaskApplication.exe*, çıktı dosyasını Azure Storage’a yüklediğinde paylaşılan erişim imzası URL'sini kullanır. Bunun için kodu, TaskApplication projesinin `Program.cs` dosyasındaki `UploadFileToContainer` yönteminde bulabilirsiniz:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>6. Adım: Görevleri izleme
![Görevleri izleme][6]<br/>
*İstemci uygulaması (1) tamamlama ve başarı durumu için görevleri izler, (2) görevler de sonuç verilerini Azure Depolama’ya yükler.*

Görevler bir projeye eklendiğinde, otomatik olarak kuyruğa alınır ve işle ilişkili havuzun içindeki işlem düğümlerinde zamanlanırlar. Belirttiğiniz ayarlar temelinde, Batch tüm kuyruğa alınan, zamanlanan, yeniden denenen ve sizle ilgili diğer görev yönetimi görevlerini işler.

Görevin yürütülüşünün izlenmesi için birçok yaklaşım vardır. DotNetTutorial, yalnızca tamamlama, görev başarılı veya başarısız durumlarını raporlayan basit bir örnek gösterilmektedir. DotNetTutorial'in `Program.cs` konumundaki `MonitorTasks` yönteminde tartışmayı destekleyen üç Batch .NET kavramı vardır. Görüntülenme sırasıyla aşağıda listelenmişlerdir:

1. **ODATADetailLevel**: Liste işlemlerinde [ODATADetailLevel][net_odatadetaillevel] belirtilmesi (iş görevlerinin listesini almak gibi) Batch uygulaması performansını sağlamak için önemlidir. Batch uygulamanızda durum izlemenin herhangi bir biçimini yapmak için hazırlık yapıyorsanız okuma listenize [Azure Batch hizmetini etkin bir şekilde sorgulama](batch-efficient-list-queries.md) makalesini de ekleyin.
2. **TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor], görev durumlarının izlenmesi için yardımcı programlara sahip Batch .NET uygulamalarını sağlar. `MonitorTasks` konumunda, *DotNetTutorial* tüm görevler için sınırlı bir süre içinde [TaskState.Completed][net_taskstate] konumuna ulaşmak için bekler. Sonra da işi sonlandırır.
3. **TerminateJobAsync**: İşin [JobOperations.TerminateJobAsync][net_joboperations_terminatejob] ile sonlandırılması (veya JobOperations.TerminateJob’un engellenmesi) bu işin tamamlandı olarak işaretlenmesine yol açar. Batch çözümünüz [JobReleaseTask][net_jobreltask] kullanıyorsa bunun yapılması gereklidir. [İş hazırlama ve tamamlama görevleri](batch-job-prep-release.md)’nde açıklanan özel tür bir görevdir.

*DotNetTutorial*'ın `Program.cs` öğesine ait `MonitorTasks` yöntemi aşağıda görüntülenmektedir:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>7. Adım: Görev çıktısı indirme
![Storage’dan görev çıktısını indirme][7]<br/>

Artık iş tamamlandı, görevlere ait çıktı Azure Storage’dan indirilebilir. *DotNetTutorial*'a ait `Program.cs` içinde `DownloadBlobsFromContainerAsync` çağrısıyla yapılır:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [!NOTE]
> *DotNetTutorial* uygulamasındaki `DownloadBlobsFromContainerAsync` çağrısı dosyaların `%TEMP%` klasörünüze yüklenmiş olması gerektiğini belirtir. Bu çıktı konumunu değiştirmekten çekinmeyin.
>
>

## <a name="step-8-delete-containers"></a>8. Adım: Sil kapsayıcıları
Azure Storage’da yer alan veriler için ücretlendirildiğinizden, Batch işleriniz için artık gerekmeyen blobları kaldırmak iyi bir fikirdir. DotNetTutorial'ın `Program.cs` öğesinde, `DeleteContainerAsync` yardımcı yöntemine yönelik üç çağrı yapılır:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Yöntemin kendisi yalnızca kapsayıcıya başvuru alır ve ardından [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete] öğesini çağırır:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>9. Adım: İşi ve havuzu silme
Son adımda, DotNetTutorial uygulaması tarafından oluşturulan işi ve havuzu silmeniz istenir. İşlerin ve görevlerin kendileri için sizden ücret alınmasa da işlem düğümleri için *ücret alınır*. Bu nedenle, düğümleri yalnızca gerektiğinde ayırmanız önerilir. Kullanılmayan havuzların silinmesi bakım işleminizin bir parçası olabilir.

BatchClient'ın [JobOperations][net_joboperations] ve [PoolOperations][net_pooloperations] öğelerinin her ikisine de, kullanıcının silmeyi onaylaması durumunda karşılık gelecek silme yöntemleri vardır:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [!IMPORTANT]
> İşlem kaynaklarının ücretli olduğunu unutmayın; kullanılmayan havuzların silinmesi masrafları azaltacaktır. Bunun yanı sıra, bir havuzun silinmesinin, bu havuz içindeki tüm işlem düğümlerini de sileceğini unutmayın; bu nedenle, düğümlerdeki veriler de havuz silindikten sonra kurtarılamayacaktır.
>
>

## <a name="run-the-dotnettutorial-sample"></a>*DotNetTutorial* örneğini çalıştırma
Örnek uygulamayı çalıştırdığınızda, konsol çıktısı aşağıdakine benzer. Yürütme sırasında, havuzun işlem düğümleri başlatıldığı sırada `Awaiting task completion, timeout in 00:30:00...` öğesiyle karşılaşacaksınız. Havuzunuzu, işlem düğümlerinizi, işinizi ve görevlerinizi yürütme sırasında ve sonrasında izlemek için [Azure portalını][azure_portal] kullanın. Uygulamanın oluşturduğu Depolama kaynaklarını (kapsayıcılar ve bloblar) görüntülemek için [Azure portalını][azure_portal] veya [Azure Depolama Gezgini’ni][storage_explorers] kullanın.

Varsayılan yapılandırmasında uygulama çalıştırıldığında tipik yürütme süresi **yaklaşık 5 dakikadır**.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Sonraki adımlar
Farklı işlem senaryolarıyla denemeler yapmak için *DotNetTutorial* ve *TaskApplication* öğelerinde değişiklik yapmaktan çekinmeyin. Örneğin, uzun soluklu görevlerin benzetimini gerçekleştirmek ve bunları portalda izlemek için [Thread.Sleep][net_thread_sleep] ile olduğu gibi *TaskApplication*’a bir yürütme gecikmesi eklemeye çalışın. Daha fazla görev eklemeye veya işlem düğüm sayısını ayarlamaya çalışın. Yürütme süresini hızlandırmak için mevcut havuzun kullanımını denetleyip izin vermek için mantık ekleyin (*ipucu*: [azure-batch-samples][github_samples] içindeki [Microsoft.Azure.Batch.Samples.Common][github_samples_common] projesinde `ArticleHelpers.cs` öğesini kullanıma alın).

Batch çözümünün temel iş akışı hakkında artık bilginiz olduğuna göre, Batch hizmetinin ek özelliklerinin derinliklerine dalma zamanı gelmiştir.

* Hizmetle yeni tanışıyorsanız önerdiğimiz [Azure Batch özelliklerine genel bakış](batch-api-basics.md) makalesini gözden geçirin.
* [Batch öğrenme yolu][batch_learning_path]’ndaki **Ayrıntılı geliştirme** altında diğer Batch geliştirmesi makalelerine başlayın.
* [TopNWords][github_topnwords] örneğinde Batch tarafından kullanılan "ilk N sözcük" iş yükünü işlemenin farklı uygulamalarını kullanıma alın.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Azure Depolama’da kapsayıcı oluşturma"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Görev uygulamasını ve girdi (veriler) dosyalarını kapsayıcılara yükleme"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Batch havuzu oluşturma"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Batch işi oluşturma"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "İşe görev ekleme"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Görevleri izleme"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Depolama’dan görev çıkışını indirme"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Batch çözümü iş akışı (tam diyagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Portalda Batch kimlik bilgileri"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Portalda Depolama kimlik bilgileri"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Batch çözümü iş akışı (minimal diyagram)"



<!--HONumber=Dec16_HO1-->


