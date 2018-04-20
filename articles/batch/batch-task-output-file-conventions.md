---
title: -Azure Batch .NET için Azure Storage iş ve Görev çıkış dosyası kuralları kitaplığıyla kalıcı | Microsoft Docs
description: .NET için Azure toplu işlem dosyası kuralları kitaplığı toplu işlem görevi ve iş çıktısı Azure depolama için kalıcı ve kalıcı çıktı Azure portalında görüntülemek için nasıl kullanılacağını öğrenin.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bbfb40b3740f9ea43df327a01ba6f4cf52d80457
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net-to-persist"></a>İş ve görev verileri Azure depolama kalıcı hale getirmek .NET için toplu iş dosyası kuralları kitaplığıyla Sürdür 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Görev verilerini kalıcı hale getirmek için bir yolu [.NET için Azure toplu işlem dosyası kuralları Kitaplığı][nuget_package]. Görev çıktı verileri Azure depolama işlemi dosyası kuralları kitaplığı basitleştirir ve onu alınıyor. Görev ve istemci kodu dosyası kuralları kitaplıkta kullanabilirsiniz &mdash; kalıcı dosyaları için görev kodu ve listelemek ve onları almak için istemci kodu. Görev kodunuzu içinde gibi Yukarı Akış görevlerin çıktısını almak için de kitaplığı kullanabilirsiniz bir [görev bağımlılıkları](batch-task-dependencies.md) senaryo. 

Dosya kuralları kitaplığıyla çıktı dosyalarını almak için belirli iş ya da görev için dosyaları amacı ve kimliği listeleyerek bulabilirsiniz. Adlar veya konumlar dosyaların bilmeniz gerek yoktur. Örneğin, belirli bir görev için tüm ara dosyaları listelemek için dosyası kuralları kitaplığını kullanın veya belirli bir iş için bir önizleme dosya alın.

> [!TIP]
> Batch hizmeti API'si 2017-05-01 sürümünden başlayarak, görevler ve sanal makine yapılandırmasıyla oluşturulan havuzu çalışan iş yöneticisi görevleri için kalıcı çıktı verilerini Azure Storage'a destekler. Batch hizmeti API'si çıktısını bir görev oluşturur ve alternatif dosyası kuralları kitaplığına görevi gören kodundaki kalıcı hale getirmek için basit bir yol sağlar. Çıktı, görevin çalıştığı uygulama güncelleştirmeye gerek kalmadan kalıcı hale getirmek için Batch istemci uygulamalarınızın değiştirebilirsiniz. Daha fazla bilgi için bkz: [kalan görev verileri toplu ile Azure depolama hizmeti API](batch-task-output-files.md).
> 
> 

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a>Ne zaman dosyası kuralları Kitaplığı görev çıktısını kalıcı hale getirmek için kullanabilirim?

Azure Batch görev çıktısını kalıcı hale getirmek için birden fazla yol sağlar. Dosya kuralları bu senaryolar için uygundur:

- Görevinizi dosyası kuralları kitaplığı kullanarak dosyaları kalıcı hale getirmek için çalıştıran uygulama kodunu kolayca değiştirebilirsiniz.
- Görevin çalışmaya devam ederken, Azure Storage veri akışı istersiniz.
- Bulut hizmet yapılandırması veya sanal makine yapılandırması ile oluşturulan havuzlarından veri kalıcı hale getirmek istediğiniz.
- İstemci uygulamanız veya işteki diğer görevler bulun ve Görev çıkış dosyalarını kimlikle veya amacı indirmek gerekir. 
- Görev çıktısını Azure portalında görüntülemek istiyorsunuz.

Senaryonuz Yukarıda listelenenler farklıysa, farklı bir yaklaşım dikkate almanız gerekebilir. Kalıcı görev çıktısı için diğer seçenekleri hakkında daha fazla bilgi için bkz: [iş ve görev çıktı Azure depolama için kalıcı](batch-task-output.md). 

## <a name="what-is-the-batch-file-conventions-standard"></a>Standart toplu işlem dosyası kuralları nedir?

[Toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) bir adlandırma şeması hedef kapsayıcılarını ve çıktı dosyalarınızı yazılır blob yollar sağlar. Dosyaları kalıcı dosyası kuralları standardını kullanan Azure depolama için Azure portalında görüntülenmek üzere otomatik olarak kullanılabilir. Portal adlandırma kuralı farkındadır ve böylece ona uyması dosyaları görüntüleyebilirsiniz.

.NET için dosyası kuralları kitaplığı depolama kapsayıcıları ve Görev çıkış dosyalarını standart dosya kurallarına göre otomatik olarak adlandırır. Dosya kuralları kitaplık ayrıca iş kimliği, görev kimliği veya amaca göre Azure storage'da çıktı dosyaları sorgulamak için yöntemleri sağlar.   

.NET dışında bir dil ile geliştiriyorsanız, uygulamanızda kendiniz dosyası kuralları standart uygulayabilirsiniz. Daha fazla bilgi için bkz: [toplu iş dosyası kuralları hakkında standart](batch-task-output.md#about-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-to-your-batch-account"></a>Batch hesabınıza bir Azure depolama hesabı bağlantı

Çıktı verileri Azure dosyası kuralları kitaplığını kullanarak kalıcı hale getirmek için bir Azure depolama hesabı toplu işlem hesabınıza bağlamanız gerekir. Henüz yapmadıysanız, bir depolama hesabı toplu işlem hesabınızı kullanarak bağlantı [Azure portal](https://portal.azure.com):

1. Azure portalında Batch hesabınıza gidin. 
2. Altında **ayarları**seçin **depolama hesabı**.
3. Batch hesabınızla ilişkili bir depolama hesabı zaten yoksa tıklatın **depolama hesabı (hiçbiri)**.
4. Aboneliğiniz için listeden bir depolama hesabı seçin. En iyi performans için görevlerinizi çalıştırdığı aynı bölgede toplu işlem hesabı olan bir Azure depolama hesabı kullanın.

## <a name="persist-output-data"></a>Çıktı verilerini Sürdür

İş ve görev çıktı verileri dosyası kuralları kitaplığıyla kalıcı hale getirmek için Azure Storage'da kapsayıcı oluşturma ve kapsayıcıya çıkış kaydedin. Kullanım [.NET için Azure Storage istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage) görev çıktısı kapsayıcıya karşıya yüklemek için görev kodunuzda. 

Kapsayıcılar ve bloblar Azure depolama ile çalışma hakkında daha fazla bilgi için bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Kitaplığı, aynı kapsayıcıda depolanır dosyası kuralları olan tüm iş ve görev çıktıları kalıcı. Çok sayıda görevler dosyaları aynı anda kalıcı çalışırsanız [azaltma sınırları depolama](../storage/common/storage-performance-checklist.md#blobs) zorlanabilir.
> 
> 

### <a name="create-storage-container"></a>Depolama kapsayıcısı oluşturma

Görev çıktısını Azure depolama için kalıcı hale getirmek için önce bir kapsayıcı çağırarak oluşturmak [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. Bu uzantı metodu geçen bir [CloudStorageAccount] [ net_cloudstorageaccount] nesnesini parametre olarak. İçeriği Azure portal tarafından keşfedilebilir; böylece dosya kuralları standart göre adlı bir kapsayıcı ve makalenin sonraki bölümlerinde ele alma yöntemleri oluşturur.

Genellikle istemci uygulamanız bir kapsayıcı oluşturmak için kodu yerleştirdiğiniz &mdash; havuzlar, işler ve görevler oluşturan uygulamayı.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Depolama görev çıkarır

Azure storage'da kapsayıcı hazırladığınıza göre görevleri çıkış kapsayıcıya kullanarak kaydedebilirsiniz [TaskOutputStorage] [ net_taskoutputstorage] dosyası kuralları Kitaplığı'nda sınıfı bulunamadı.

Görev kodunuzda ilk oluşturun bir [TaskOutputStorage] [ net_taskoutputstorage] nesne sonra görev kendi iş tamamlandığında, çağrı [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] çıktısını Azure depolamasına kaydetmek için yöntem.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

`kind` Parametresinin [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) yöntemi kalıcı dosyaları kategorilere ayırır. Dört önceden tanımlanmış vardır [TaskOutputKind] [ net_taskoutputkind] türleri: `TaskOutput`, `TaskPreview`, `TaskLog`, ve `TaskIntermediate.` çıktı özel kategoriler de tanımlayabilirsiniz.

Bu çıktı türleri toplu belirli bir görevi kalıcı çıkışlar için daha sonra sorguladığınızda listelemek için çıktı türünü belirtmenizi sağlar. Diğer bir deyişle, bir görev çıktıları listelediğinizde çıkış türlerinden birini listede filtreleyebilirsiniz. Örneğin, "ver *Önizleme* için görev çıktısını *109*." Listeleme ve çıkışları alma hakkında daha fazla görünür [almak çıktı](#retrieve-output) sonraki makalede.

> [!TIP]
> Çıktı türü de Azure portalında belirli bir dosya göründüğü belirler: *TaskOutput*-kategorilere ayrılmış dosyaları görünür altında **Görev çıkış dosyaları**, ve *TaskLog* dosyaları görünür altında **görev günlükleri**.
> 
> 

### <a name="store-job-outputs"></a>Mağaza iş çıkarır

Görev çıktıları depolamak ek olarak, tüm bir işle ilişkili çıkışları depolayabilirsiniz. Örneğin, bir filmi işleme iş birleştirme görevinde, tamamen işlenmiş film iş çıktısı kalıcı olamadı. İş tamamlandığında, istemci uygulaması listesinde ve işi için çıkış almak ve tek tek görevler sorgu gerekmez.

İş çıktısı çağırarak depolamak [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] yöntemini belirtin [JobOutputKind] [ net_joboutputkind] ve dosya adı:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

İle **TaskOutputKind** türü görev çıktıları için kullandığınız [JobOutputKind] [ net_joboutputkind] bir işi kategorilere ayırma türü dosyaları kalıcı. Bu parametre için sonraki sorgu (liste) belirli bir çıkış türü sağlar. **JobOutputKind** türü çıkış ve önizleme kategorileri içerir ve özel kategoriler oluşturulmasını destekler.

### <a name="store-task-logs"></a>Depolama görev günlükleri

Bir görev veya iş tamamlandığında dayanıklı depolama bir dosyaya kalıcı ek olarak, bir görevin yürütülmesi sırasında güncelleştirilen dosyalar kalıcı gerekebilir &mdash; günlük dosyalarını veya `stdout.txt` ve `stderr.txt`, örneğin. Bu amaç için Azure toplu işlem dosyası kuralları kitaplığı sağlayan [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] yöntemi. İle [SaveTrackedAsync][net_savetrackedasync], bir dosyaya (belirttiğiniz bir aralıkta) düğümünde güncelleştirmeleri izlemek ve bu güncelleştirmeleri Azure Storage kalır.

Aşağıdaki kod parçacığında kullanırız [SaveTrackedAsync] [ net_savetrackedasync] güncelleştirmek için `stdout.txt` görevin yürütülmesi sırasında 15 dakikada Azure storage'da:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
     await Task.Delay(stdoutFlushDelay);
}
```

Açıklamalı bölüm `Code to process data and produce output file(s)` göreviniz normalde gerçekleştireceği kodu için bir yer tutucudur. Örneğin, veriler Azure depolama biriminden yükler ve dönüşüm ya da hesaplama üzerinde gerçekleştiren kodu olabilir. Bu tür kodda nasıl kayabilir bu parçacığı önemli bir parçası gösteren bir `using` sahip bir dosya düzenli olarak güncelleştirmek için blok [SaveTrackedAsync][net_savetrackedasync].

Düğüm Aracısı havuzdaki her düğüm üzerinde çalışır ve düğümü ile Batch hizmeti arasındaki komut ve denetim arabirimi sağlayan bir programdır. `Task.Delay` Çağrıdır sonunda bu gerekli `using` blok düğümü aracı düğümü üzerindeki stdout.txt dosyaya standart dışı içeriğini temizlemek için zaman sahip olduğundan emin olun. Bu gecikme olmadan çıkış son birkaç saniye kaçırılması mümkündür. Bu gecikme tüm dosyaları için gerekli olmayabilir.

> [!NOTE]
> Dosya ile izleme etkinleştirdiğinizde **SaveTrackedAsync**, yalnızca *ekler* izlenen dosyası Azure depolama alanına kalıcı. Yalnızca döndürme olmayan günlük dosyalarını veya dosyanın sonuna ekleme işlemleri ile yazılan diğer dosyalarını izlemek için bu yöntemi kullanın.
> 
> 

## <a name="retrieve-output-data"></a>Çıktı verilerini alma

Azure toplu işlem dosyası kuralları kitaplığı kullanarak, kalıcı çıktı aldığınızda, görev ve iş merkezli bir şekilde bunu. Belirtilen çıkış isteyebilir görev ya da Azure Storage veya bile dosya adıyla kendi yolunda bilmenize gerek olmadan iş. Bunun yerine, görev tarafından çıktı dosyaları istek veya iş kimliği

Aşağıdaki kod parçacığını bir işin görevlerini üzerinden tekrarlanan, görev için çıktı dosyaları hakkında bazı bilgiler yazdırır ve sonra dosyalarından depolama biriminden yükler.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-the-azure-portal"></a>Azure portalında çıktı dosyalarını görüntüle

Görev çıktı dosyalarını Azure portalı görüntüler ve bağlı bir Azure depolama alanına kalıcı günlükleri kullanarak hesap [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Bu kuralları kendiniz de uygulayabileceğiniz bir dil seçiminizi veya .NET uygulamalarınızın dosyası kuralları kitaplıkta kullanabilirsiniz.

Çıktı dosyalarınızı Portalı'nda görünen etkinleştirmek için aşağıdaki gereksinimleri karşılaması gerekir:

1. [Bir Azure depolama hesabı bağlantı](#requirement-linked-storage-account) Batch hesabınıza.
2. Depolama kapsayıcıları ve dosyalar için önceden tanımlanmış adlandırma kuralları için çıkışları kalıcı olduğunda uyar. Bu kuralları tanımı dosyası kuralları Kitaplığı'nda bulabilirsiniz [Benioku][github_file_conventions_readme]. Kullanırsanız [Azure toplu işlem dosyası kuralları] [ nuget_package] , çıktı kalıcı hale getirmek için kitaplık dosyası kuralları standart göre dosyalarınıza kalıcı.

Azure portalında görev çıktı dosyalarını ve günlükleri görüntülemek için ilgilendiğiniz, çıktısı'ye tıklayın ya da görev gidin **kayıtlı çıktı dosyaları** veya **Günlükleri kaydedilen**. Bu görüntü gösterir **kayıtlı çıktı dosyaları** "007" kimlikli görev için:

![Azure portalında görev çıktıları dikey penceresi][2]

## <a name="code-sample"></a>Kod örneği

[PersistOutputs] [ github_persistoutputs] örnek proje biridir [Azure Batch kod örnekleri] [ github_samples] github'da. Bu Visual Studio çözümü Azure toplu işlem dosyası kuralları Kitaplığı görev çıktısını dayanıklı depolama için kalıcı hale getirmek için nasıl kullanılacağını gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Projeyi **Visual Studio 2015 veya daha yeni**.
2. Batch ve Storage eklemek **hesabının kimlik bilgilerini** için **AccountSettings.settings** öğesini kullanıma alın projesinde.
3. **Yapı** (ancak çalışmaz) çözümü. İstenirse herhangi bir NuGet paketinin geri yükleyin.
4. Karşıya yüklemek için Azure portal'ı kullanmanızı bir [uygulama paketi](batch-application-packages.md) için **PersistOutputsTask**. Dahil `PersistOutputsTask.exe` ve uygulama kimliği "PersistOutputsTask" ve "1.0" uygulama paketi sürümüne bağımlı derlemeleri .zip paketindeki ayarlayın.
5. **Başlat** (Çalıştır) **PersistOutputs** projesi.
6. Örnek çalıştırmak için kullanmak için girin Kalıcılık teknolojisi seçmek için istendiğinde **1** görev çıktısını kalıcı hale getirmek için dosya kuralları kitaplığı kullanılarak örneği çalıştırmak için. 

## <a name="next-steps"></a>Sonraki adımlar

### <a name="get-the-batch-file-conventions-library-for-net"></a>Toplu iş dosyası kuralları kitaplığı için .NET Al

.NET için toplu iş dosyası kuralları kitaplığı kullanılabilir [NuGet][nuget_package]. Kitaplık genişletir [CloudJob] [ net_cloudjob] ve [CloudTask] [ net_cloudtask] yeni yöntemleri sınıflarıyla. Ayrıca bkz. [başvuru belgelerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) dosyası kuralları kitaplığı.

[Kaynak kodu] [ github_file_conventions] için dosya kuralları kitaplığı .NET deposu için Microsoft Azure SDK'sı Github'da kullanılabilir. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Diğer yaklaşımlar kalıcı çıkış verileri keşfedin

- Bkz: [iş ve görev çıktı Azure depolama için kalıcı](batch-task-output.md) kalıcı görevi ve iş verileri genel bakış.
- Bkz: [kalan görev verileri toplu ile Azure depolama hizmeti API](batch-task-output-files.md) Batch hizmeti API'si çıktı verilerini kalıcı hale getirmek için nasıl kullanılacağını öğrenin.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Kayıtlı çıktı dosyaları ve Portalı'nda kayıtlı günlükleri seçiciler"
[2]: ./media/batch-task-output/task-output-02.png "Azure portalında görev çıktıları dikey penceresi"
