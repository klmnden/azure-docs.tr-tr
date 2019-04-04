---
title: .NET - Azure Batch dosya kuralları kitaplığı Azure Storage'a iş ve görev çıktılarını kalıcı hale getirmek | Microsoft Docs
description: .NET için Azure Batch dosya kuralları kitaplığı Azure Storage'a Batch görev ve iş çıkışı kalıcı hale getirme ve Azure Portalı'nda kalıcı çıkışı görüntülemek için kullanmayı öğrenin.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 11/14/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d30a5ca0910c5ceebb38dec7b4cdbffd9b3cf27e
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916793"
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net"></a>.NET için Azure Depolama'ya iş ve görev veri Batch dosya kuralları kitaplığı ile kalıcı

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Görev verileri kalıcı hale getirmek için tek bir yolu [.NET için Azure Batch dosya kuralları Kitaplığı][nuget_package]. Dosya kuralları Kitaplığı görev çıktı verileri Azure depolama için depolama işlemi basitleştirir ve alınması. Dosya kuralları Kitaplığı hem görev hem de istemci kod kullanabileceğiniz &mdash; dosyaları kalıcı görev kodunu ve listelemek ve onları almak için istemci kodu. Görev kodunuz içinde olduğu gibi Yukarı Akış görev çıktısını almak için de kitaplığı kullanabilirsiniz bir [görev bağımlılıkları](batch-task-dependencies.md) senaryo.

Dosya kuralları kitaplığı ile çıkış dosyalarını almak için belirli bir işin veya görevin dosyalarını amaçlı kimliği ile listeleyerek bulabilirsiniz. Adlar veya konumlar dosyaların bilmeniz gerekmez. Örneğin, belirli bir görev için tüm ara dosyaları listelemek için dosya kuralları kitaplığı kullanın veya belirli bir işin bir önizleme dosyası alın.

> [!TIP]
> Batch hizmeti API 2017-05-01 sürümü ile başlayarak, görevleri ve sanal makine yapılandırmasıyla oluşturulan havuzlar üzerinde çalışan iş yöneticisi görevleri için çıktı verilerini Azure Depolama'da kalıcı destekler. Batch hizmeti API'si, bir görev oluşturan ve alternatif dosya kuralları kitaplığı görevi gören kodundaki çıktısını kalıcı hale getirmek için basit bir yol sağlar. Göreviniz çalışmakta olan uygulamayı güncelleştirmek gerek kalmadan çıktılarını kalıcı hale getirme için Batch istemci uygulamalarınızı değiştirebilirsiniz. Daha fazla bilgi için [kalan görev veri Batch ile Azure depolama hizmeti API](batch-task-output-files.md).

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a>Dosya kuralları Kitaplığı görev çıktısını kalıcı hale getirmek için ne zaman kullanırım?

Azure Batch, görev çıktısını kalıcı hale getirmek için birden fazla yol sağlar. Dosya kuralları bu senaryolar için idealdir:

- Dosya kuralları kitaplığı kullanarak dosyaları kalıcı hale getirmek için görevi çalıştıran uygulama kodunu kolayca değiştirebilirsiniz.
- Görevin çalışmaya devam ederken akışı verilerini Azure Depolama'da istiyorsunuz.
- Bulut hizmeti yapılandırmasını veya sanal makine yapılandırması ile oluşturulan havuzlar verileri kalıcı hale getirmek istediğiniz.
- İstemci uygulamanız veya işteki diğer görevler bulun ve Kimliğine veya amacına göre Görev çıkış dosyalarını indirmek gerekir.
- Görev çıktısını Azure portalında görüntülemek istiyorsunuz.

Senaryonuz yukarıda listelenenlerden farklıysa, farklı bir yaklaşım dikkate almanız gerekebilir. Görev çıktısını kalıcı hale getirme için diğer seçenekler hakkında daha fazla bilgi için bkz. [Azure Depolama'ya iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md).

## <a name="what-is-the-batch-file-conventions-standard"></a>Standart Batch dosya kuralları nedir?

[Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions) blob yollarının çıktı dosyalarınızı yazılır ve hedef kapsayıcı için bir adlandırma düzeni sağlar. Dosyaları kalıcı dosyası kuralları standardını kullanan Azure depolama için Azure portalında görüntülemek için otomatik olarak kullanılabilir. Portal adlandırma kuralının farkındadır ve bu nedenle ona bağlı dosyalar görüntüleyebilirsiniz.

.NET için dosya kuralları kitaplığı, depolama kapsayıcıları ve Görev çıkış dosyalarını standart dosya kurallarına göre otomatik olarak adlandırır. Dosya kuralları kitaplığı, ayrıca Çıkış dosyalarını Azure storage'da iş kimliği, görev kimliği veya amaçlı göre sorgulamak için yöntemler sağlar.

.NET dışında bir dil ile geliştiriyorsanız, uygulamanızı kendiniz dosya kuralları standart uygulayabilirsiniz. Daha fazla bilgi için [Batch dosya kuralları standart uygulama](batch-task-output.md#implement-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-to-your-batch-account"></a>Bir Azure depolama hesabını Batch hesabınıza bağlayın.

Dosya kuralları kitaplığı kullanarak Azure depolama için çıktı verilerini kalıcı hale getirmek için bir Azure depolama hesabını Batch hesabınıza bağlamanız gerekir. Henüz yapmadıysanız, bir depolama hesabını Batch hesabınıza kullanarak bağlantı [Azure portalında](https://portal.azure.com):

1. Azure portalında Batch hesabınıza gidin.
1. Altında **ayarları**seçin **depolama hesabı**.
1. Batch hesabınızla ilişkili bir depolama hesabı zaten yoksa tıklayın **depolama hesabı (hiçbiri)**.
1. Aboneliğiniz için listeden bir depolama hesabı seçin. En iyi performans için görevlerinizin çalıştığı aynı bölgede Batch hesabı olarak bir Azure depolama hesabı kullanın.

## <a name="persist-output-data"></a>Çıktı verilerini kalıcı

Dosya kuralları kitaplığı ile iş ve görev çıktı verileri kalıcı hale getirmek için Azure Depolama'da kapsayıcı oluşturma ve çıkış kapsayıcısı için kaydedin. Kullanım [.NET için Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage) görev çıktısı kapsayıcıya yüklemek için görev kod. 

Kapsayıcılar ve Azure Depolama'da bloblar ile çalışma hakkında daha fazla bilgi için bkz. [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Dosya kuralları kitaplığı aynı kapsayıcıda depolanan ile tüm iş ve görev çıktıları kalıcı. Aynı zamanda, dosyaların kalıcı olması çok sayıda görev çalışırsanız [depolama sınırları](../storage/common/storage-performance-checklist.md#blobs) zorlanabilir.

### <a name="create-storage-container"></a>Depolama kapsayıcısı oluşturma

Azure depolama için görev çıktısını kalıcı hale getirmek için önce bir kapsayıcı çağırarak oluşturun [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. Bu uzantı yöntemi alır bir [CloudStorageAccount] [ net_cloudstorageaccount] bir parametre olarak nesne. Dosya kuralları standardına göre içeriğini Azure portal tarafından bulunabilir ve böylelikle adlı bir kapsayıcı ve makalenin sonraki bölümlerinde ele alma yöntemi oluşturur.

İstemci uygulamanızı bir kapsayıcı oluşturmak için kod genellikle yerleştirdiğiniz &mdash; , havuzlar, işler ve görevler oluşturan uygulamasıdır.

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

### <a name="store-task-outputs"></a>Store görev çıktıları

Azure depolamadaki bir kapsayıcıda hazırladığınıza göre görevleri çıkış kapsayıcıya kullanarak kaydedebilirsiniz [TaskOutputStorage] [ net_taskoutputstorage] dosya kuralları Kitaplığı'nda sınıfı bulunamadı.

Görev kodunuzda ilk oluşturmak bir [TaskOutputStorage] [ net_taskoutputstorage] nesnesi ve ardından görev, çalışma tamamlandığında çağırılacak [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] çıktısını Azure depolamasına kaydetmek için yöntemi.

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

`kind` Parametresinin [TaskOutputStorage](/dotnet/api/microsoft.azure.batch.conventions.files.taskoutputstorage).[ SaveAsync](/dotnet/api/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync#overloads) yöntemi kalıcı dosyaları kategorilere ayırır. Dört önceden tanımlanmış vardır [TaskOutputKind] [ net_taskoutputkind] türleri: `TaskOutput`, `TaskPreview`, `TaskLog`, ve `TaskIntermediate.` çıktı özel kategoriler de tanımlayabilirsiniz.

Bu çıktı türleri, belirli bir görevin kalıcı çıktısı için daha sonra Batch sorguladığınızda listelemek için çıktı türünü belirtmenizi sağlar. Diğer bir deyişle, bir görev çıkışları listelediğinizde türlerden biri listeyi filtreleyebilirsiniz. Örneğin, "ver *Önizleme* için görev çıktısını *109*." Listeleme ve çıktılar alma hakkında daha fazla görünür makalenin sonraki bölümlerinde yer alma çıktısında.

> [!TIP]
> Çıkış türü de Azure portalında, belirli bir dosya göründüğü belirler: *TaskOutput*-sınıflandırılmış dosyalar görünür altında **bir görevin çıkış dosyaları**, ve *TaskLog* dosyalar görünür altında **görev günlükleri**.

### <a name="store-job-outputs"></a>İş çıkışları Store

Görev çıktılarının depolanması ek olarak, tüm bir işle ilişkili çıkışları depolayabilirsiniz. Örneğin, bir film işleme işi birleştirme görevde, tam olarak işlenen film bir iş çıktısı kalıcı olamadı. İş tamamlandığında, istemci uygulamanızı liste ve işi için çıkış almak ve tek tek görevler sorgu gerekmez.

Çağırarak çıkışını Store [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] yöntemini belirtin [JobOutputKind] [ net_joboutputkind] ve dosya adı:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Olduğu gibi **TaskOutputKind** türü görev çıktıları için kullandığınız [JobOutputKind] [ net_joboutputkind] kalıcı dosyaları bir işi kategorilere ayırmak için türü. Bu parametre için sonraki sorgu (liste) belirli türde bir çıktı sağlar. **JobOutputKind** türü çıkış hem Önizleme kategorileri içerir ve özel kategoriler oluşturulmasını destekler.

### <a name="store-task-logs"></a>Store görevi günlükleri

Bir görev veya iş tamamlandığında bir dosya kalıcı depolama kalıcı ek olarak, bir görevin yürütülmesi sırasında güncelleştirilen dosyaların kalıcı olması gerekebilir &mdash; günlük dosyalarını veya `stdout.txt` ve `stderr.txt`, örneğin. Bu amaç için Azure Batch dosya kuralları kitaplığı sağlar [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] yöntemi. İle [SaveTrackedAsync][net_savetrackedasync], güncelleştirmeleri bir dosyaya (sırasında belirttiğiniz bir aralık) düğümünde izleyebilir ve Azure Depolama'ya güncelleştirmelerde kalıcı.

Aşağıdaki kod parçacığında, kullandığımız [SaveTrackedAsync] [ net_savetrackedasync] güncelleştirilecek `stdout.txt` her 15 saniyede görevin yürütülmesi sırasında Azure Depolama'da:

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

Açıklamalı bölüm `Code to process data and produce output file(s)` göreviniz normalde gerçekleştirecek kod için bir yer tutucudur. Örneğin, Azure Storage'dan veri ve dönüştürme veya hesaplama üzerinde gerçekleştiren kod olabilir. Bu kod parçacığı önemli parçası gibi kodda nasıl kayabilir gösteren bir `using` düzenli aralıklarla bir dosyayı güncelleştirecek blok [SaveTrackedAsync][net_savetrackedasync].

Düğüm Aracısı, havuzdaki her düğümde çalışan ve Batch hizmeti düğüm arasındaki komut ve denetim arabirimi sağlayan bir programdır. `Task.Delay` Çağrıdır sonunda bu gerekli `using` düğüm Aracısı düğümde stdout.txt dosyaya standart dışı içeriğini temizlemek için zaman olduğundan emin olmak için blok. Bu gecikme olmadan çıkış son birkaç saniye kaçırmayın mümkündür. Bu gecikme, tüm dosyaları için gerekli olmayabilir.

> [!NOTE]
> İle izleme dosyası etkinleştirdiğinizde **SaveTrackedAsync**, yalnızca *ekler* izlenen dosyayı Azure Depolama'ya kalıcıdır. Yalnızca günlük dosyalarını döndürme olmayan veya dosyanın sonuna ekleme işlemleri ile yazılan diğer dosyaları izlemek için bu yöntemi kullanın.

## <a name="retrieve-output-data"></a>Çıktı verilerini alma

Azure Batch dosya kuralları kitaplığı kullanarak kalıcı çıkış aldığınızda, görev ve iş odaklı bir biçimde bunu yapın. Belirtilen çıkış talep edebilir görev veya Azure depolama veya bile, dosya adı yolu bilmenize gerek olmadan iş. Bunun yerine Çıkış dosyalarını görevi tarafından istek veya iş kimliği.

Aşağıdaki kod parçacığı, iş görevlerinin yinelenir, görevin çıkış dosyaları hakkında bazı bilgiler yazdırır ve dosyalarından Storage'dan.

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

## <a name="view-output-files-in-the-azure-portal"></a>Azure portalında Çıkış dosyalarını görüntüle

Görev çıkış dosyalarını Azure portalında görüntülenir ve bağlı bir Azure depolama için kalıcı günlükleri kullanarak hesap [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions). Bu kuralları kendiniz de uygulayabileceğiniz bir dil seçiminizi veya .NET uygulamalarınızda dosya kuralları kitaplığı kullanabilirsiniz.

Çıktı dosyalarınızı portalında görüntülenmesini etkinleştirmek için aşağıdaki gereksinimleri karşılamanız gerekir:

1. Bir Azure depolama hesabını Batch hesabınıza bağlayın.
1. Depolama kapsayıcıları ve dosya için önceden tanımlanmış adlandırma kuralları çıkışları kalıcı olduğunda izliyor. Dosya kuralları Kitaplığı'nda bu kuralları tanımını bulabilirsiniz [Benioku][github_file_conventions_readme]. Kullanırsanız [Azure Batch dosya kuralları] [ nuget_package] , çıkışı kalıcı hale getirmek için kitaplık dosyası kuralları standardına göre dosyalarınızı kalıcı.

Görev çıkış dosyalarını ve günlükleri, Azure portalında görüntülemek için ilgilendiğiniz, çıktısı'a tıklayın ya da görev için gidin **kaydedilen çıktı dosyaları** veya **günlükleri kaydedilmiş**. Bu görüntüde gösterilmektedir **kaydedilen çıktı dosyaları** "007" Kimliğine sahip görev için:

![Azure portalındaki dikey penceresinde görev çıktıları][2]

## <a name="code-sample"></a>Kod örneği

[PersistOutputs] [ github_persistoutputs] örnek proje, biridir [Azure Batch kod örnekleri] [ github_samples] GitHub üzerinde. Bu Visual Studio çözüm kalıcı depolama için görev çıktısını kalıcı hale getirmek için Azure Batch dosya kuralları kitaplığı kullanma işlemini gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Projeyi **Visual Studio 2017**.
2. Batch ve Storage ekleme **hesap kimlik bilgileri** için **AccountSettings.settings** Microsoft.Azure.Batch.Samples.Common projedeki.
3. **Derleme** (ancak çalıştırılmadı) çözümü. Herhangi bir NuGet paketinin istenirse geri yükleyin.
4. Karşıya yüklemek için Azure portalını kullanın. bir [uygulama paketini](batch-application-packages.md) için **PersistOutputsTask**. Dahil `PersistOutputsTask.exe` ve bunların bağımlı derlemeleri .zip paketindeki "PersistOutputsTask" için uygulama kimliği ve "1.0" için uygulama paketi sürümünü ayarlama.
5. **Başlangıç** (Çalıştır) **PersistOutputs** proje.
6. Örneği çalıştırmak için kullanmak için girin Kalıcılık teknoloji seçmeniz istendiğinde **1** görev çıktısını kalıcı hale getirmek için dosya kuralları kitaplığı kullanarak örneği çalıştırmak için. 

## <a name="next-steps"></a>Sonraki adımlar

### <a name="get-the-batch-file-conventions-library-for-net"></a>.NET için Batch dosya kuralları kitaplığı edinme

.NET için Batch dosya kuralları kitaplığı kullanılabilir [NuGet][nuget_package]. Kitaplık genişletir [CloudJob] [ net_cloudjob] ve [CloudTask] [ net_cloudtask] yeni yöntemlerle sınıfları. Ayrıca bkz: [başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) dosya kuralları kitaplığı.

[Kaynak kodu] [ github_file_conventions] için dosya kuralları kitaplığı Github'da .NET deposu için Microsoft Azure SDK'sı bulunur. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Diğer yaklaşımlar kalıcı çıkış verileri keşfedin

- Bkz: [Azure Depolama'ya iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md) kalıcı görev ve iş verilerini genel bakış.
- Bkz: [kalan görev veri Batch ile Azure depolama hizmeti API](batch-task-output-files.md) çıktı verileri kalıcı hale getirmek için Batch hizmeti API'sini kullanmayı öğrenin.

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
[storage_explorer]: https://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Kaydedilen çıktı dosyaları ve Portalı'nda kayıtlı günlükleri Seçici"
[2]: ./media/batch-task-output/task-output-02.png "Azure portalındaki dikey penceresinde görev çıktıları"
