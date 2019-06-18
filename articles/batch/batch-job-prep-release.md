---
title: İşler ve işlem düğümlerinde - Azure Batch tam işlerini hazırlamak için görevler oluştur | Microsoft Docs
description: Azure Batch işlem düğümleri için veri aktarımını en aza indirmek için proje düzeyinde hazırlama görevleri kullanın ve sürüm görevleriniz düğümün temizleme iş tamamlanma için.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 517ac0f612b9e5fc5909a7f0fe2ce088c9b367d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60776205"
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>İş hazırlama ve iş sürüm görevleri batch işlem düğümleri

 Azure Batch işi, genellikle görevlerinin yürütülür ve Bakım görevlerinin tamamlandığında sonrası iş önce kurulum çeşit gerektirir. İşlem düğümleri için yaygın görev giriş verilerini indirin veya iş tamamlandıktan sonra görev çıktı verilerini Azure Depolama'ya yüklemek gerekebilir. Kullanabileceğiniz **iş hazırlama** ve **iş yayın** bu işlemleri gerçekleştirmek için görevler.

## <a name="what-are-job-preparation-and-release-tasks"></a>Hangi iş hazırlama ve sürüm görevleriniz mı?
Bir işteki görevleri çalıştırmadan önce en az bir görevi çalıştırmak için zamanlanan tüm işlem düğümlerinde iş hazırlama görevini çalıştırır. İş tamamlandığında iş bırakma görevi havuzun en az bir görev yürütmüş her düğümünde çalışır. Normal Batch görevleriniz olduğu gibi iş hazırlama veya sürüm görevi çalıştırıldığında çağrılacak komut satırını belirtebilirsiniz.

İş hazırlama ve bırakma görevleri, dosya indirme gibi tanıdık Batch görev özellikler sunar ([kaynak dosyaları][net_job_prep_resourcefiles]), yükseltilmiş yürütme, özel ortam değişkenleri, en uzun yürütme süresi, yeniden deneme sayısı ve dosyayı elde tutma süresi.

Aşağıdaki bölümlerde, nasıl kullanılacağını öğreneceksiniz [JobPreparationTask] [ net_job_prep] ve [JobReleaseTask] [ net_job_release] sınıfları içindebulunamadı[ Batch .NET] [ api_net] kitaplığı.

> [!TIP]
> İş hazırlama ve bırakma görevleri, işlem düğümleri havuzu iş çalıştırmaları arasında devam ediyorsa ve birçok iş tarafından kullanılan "havuzu paylaşılan" ortamlarda, özellikle yararlı olur.
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a>İş hazırlama ve sürüm görevleriniz ne zaman
Aşağıdaki durumlarda uygun iş hazırlama ve iş sürüm görevleri şunlardır:

**Genel görev verileri indirme**

Toplu işleri genellikle iş görevlerinin için giriş olarak ortak bir veri kümesini gerektirir. Örneğin, günlük risk analizi hesaplamalarda verilerini işe özgü, işteki tüm görevler için ortak henüz. Bu verilerini, genellikle birkaç gigabayt boyutunda düğüm üzerinde çalışan herhangi bir görev kullanabilmesi için her bir işlem düğümünde yalnızca bir kez indirilmelidir. Kullanım bir **iş hazırlama görevi** işinin yürütülmesi diğer görevleri kullanıcının önce her düğüm için bu verileri indirmek için.

**İş ve görev çıktılarını Sil**

Bir havuzun işlem düğümleri arasında işleri yetkisi olmadığı, bir "havuzu paylaşılan" ortamında çalıştırma arasında iş verilerini silmeniz gerekebilir. Düğümler üzerinde disk alanından tasarruf etmek ya da kuruluşunuzun güvenlik ilkelerine uygun gerekebilir. Kullanım bir **iş bırakma görevi** , iş hazırlama görevi tarafından indirilen veya oluşturulan görev yürütme sırasında verilerini silmek için.

**Günlük tutma**

Görevlerinizin oluşturduğu günlük dosyalarının bir kopyasını tutmak veya belki de başarısız uygulamalar tarafından oluşturulmuş döküm dosyalarını kilitlenme isteyebilirsiniz. Kullanım bir **iş bırakma görevi** sıkıştırma ve bu verileri karşıya yüklemek için bu gibi durumlarda bir [Azure depolama] [ azure_storage] hesabı.

> [!TIP]
> Günlükleri ve diğer iş ve görev kalıcı hale getirmek için başka bir şekilde çıkış veri olarak kullanmak için [Azure Batch dosya kuralları](batch-task-output.md) kitaplığı.
> 
> 

## <a name="job-preparation-task"></a>İş hazırlama görevi
İş görevlerinin yürütülmesini önce toplu iş hazırlama görevi bir görev çalışacak şekilde zamanlanmış her işlem düğümünde yürütür. Varsayılan olarak, Batch hizmeti iş hazırlama görevi bir düğümde yürütülmek için zamanlandığı görevleri çalıştırmadan önce tamamlanması için bekler. Ancak, beklememeyi hizmeti yapılandırabilirsiniz. Düğümü yeniden başlatılırsa, iş hazırlama görevi tekrar çalışır, ancak aynı zamanda bu davranışı devre dışı bırakabilirsiniz.

İş hazırlama görevi, bir görevi çalıştırmak için zamanlanan düğümlerinde yürütülür. Bir düğüm göreve atanmaz durumunda bu hazırlık görevi gereksiz yürütülmesini engeller. Bir iş için görevleri sayısı bir havuzdaki düğümlerin sayısından daha az olduğunda bu durum oluşabilir. Ne zaman da geçerlidir [eşzamanlı görev yürütme](batch-parallel-node-tasks.md) olan etkin görev sayısı bazı düğümler boşta if bırakan toplam olası eş zamanlı görevleri düşüktür. İş hazırlama görevi boşta düğüm üzerinde çalıştırarak değil, veri aktarım ücretleri üzerinde daha az para harcamanız.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] farklıdır [CloudPool.StartTask] [ pool_starttask] ise her bir iş başında JobPreparationTask yürütür, StartTask yalnızca ne zaman bir işlem düğümünde önce bir havuza katıldığında yürütür veya yeniden başlatır.
> 
> 

## <a name="job-release-task"></a>İş bırakma görevi
Bir işin tamamlandı olarak işaretlendikten sonra iş bırakma görevi havuzun en az bir görev yürütmüş her düğümünde çalıştırılır. Bir iş, bir sonlandırma isteği göndererek tamamlandı olarak işaretleyin. Batch hizmeti daha sonra iş durumu ayarlar *sonlandırma*işle ilişkili etkin ya da çalışmayan görevler sonlandırır ve iş bırakma görevini çalıştırır. Taşınır ardından işi *tamamlandı* durumu.

> [!NOTE]
> Proje silme, ayrıca iş bırakma görevi yürütür. Bir iş zaten sona erdi, işi daha sonra silinirse, ancak bırakma görevi ikinci kez çalıştırılmaz.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>İş hazırlığı ve görevleri Batch .NET ile yayın
İş hazırlama görevi kullanmak için Ata bir [JobPreparationTask] [ net_job_prep] işinizin nesnesine [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] özelliği. Benzer şekilde, başlatmak bir [JobReleaseTask] [ net_job_release] ve işinizin için atama [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] işin ayarlamak için özellik bırakma görevi.

Bu kod parçacığında `myBatchClient` örneğidir [BatchClient][net_batch_client], ve `myPool` Batch hesabında var olan bir havuzu.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobReleaseTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Bir işi sona erdi ya da silindiğinde bırakma görevi daha önce bahsedildiği gibi yürütülür. İle bir işlemi sonlandırmak [JobOperations.TerminateJobAsync][net_job_terminate]. Bir işi şununla silin [JobOperations.DeleteJobAsync][net_job_delete]. Genellikle sonlandırma veya iş görevlerinin tamamlandığında veya sizin tanımladığınız bir zaman aşımı ulaşıldığında Sil.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>GitHub üzerinde örnek kod
İş hazırlama görmek ve sürüm görevleriniz uygulamada kullanıma [JobPrepRelease] [ job_prep_release_sample] GitHub üzerinde örnek proje. Bu konsol uygulaması şunları yapar:

1. İki düğümleri havuzu oluşturur.
2. İş hazırlama, sürüm ve standart görevler, bir iş oluşturur.
3. İlk düğüm kimliği bir düğümün "paylaşılan" dizininde bir metin dosyasına yazan iş hazırlama görevi çalıştırır.
4. Bir görev alt görev kimliği aynı metin dosyasına yazan her düğümünde çalıştırılır.
5. Tüm görevler tamamlandığında (veya zaman aşımı ulaşıldıktan sonra), konsola her düğümün metin dosyasının içeriğini yazdırır.
6. İş tamamlandığında iş bırakma görevi düğümden dosyayı silmek için çalışır.
7. İş hazırlama ve bırakma görevleri üzerinde yürütülen her düğüm için çıkış kodlarını yazdırır.
8. İşinin ve/veya havuzu silme onayı izin vermek için yürütme duraklatır.

Örnek uygulama çıktısı aşağıdakine benzer:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

> [!NOTE]
> Değişken oluşturma ve başlangıç saati (bazı görevler diğerlerinden önce hazır düğümlerdir) yeni bir havuzdaki düğümlerin nedeniyle, farklı çıkış görebilirsiniz. Özellikle, görevleri hızlı bir şekilde tamamlamak için havuz düğümlerinden biri tüm işin görevleri yürütebilir. Bu meydana gelirse, iş hazırlama olduğunu fark edeceksiniz ve sürüm görevleri hiçbir görev yürütülen düğüm için mevcut değil.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>İş hazırlama ve bırakma görevleri Azure portalında inceleyin
Örnek uygulamayı çalıştırdığınızda, kullanabileceğiniz [Azure portalında] [ portal] iş ve görevleri özelliklerini görüntüleyin ya da devre dışı bile iş görevlerinin tarafından değiştirilen paylaşılan metin dosyasını indirin.

Gösterir aşağıdaki ekran görüntüsünde **hazırlık görevleri dikey** örnek uygulamayı çalıştırma sonra Azure portalında. Gidin *JobPrepReleaseSampleJob* özellikleri görevleri tamamladıktan sonra (ancak işi ve havuzu silmeden önce) tıklayın **hazırlama görevleri** veya **yayın görevleri**özelliklerini görüntülemek için.

![Azure portalında iş hazırlama özellikleri][1]

## <a name="next-steps"></a>Sonraki adımlar
### <a name="application-packages"></a>Uygulama paketleri
İş hazırlama görevi ek olarak, ayrıca kullanabileceğiniz [uygulama paketleri](batch-application-packages.md) işlem düğümleri için görev yürütme hazırlamak için Toplu özellik. Bu özellik bir yükleyici, çok sayıda (100'den fazla) dosyaları içeren uygulamalar veya katı sürüm denetimi gerektiren uygulamalar çalıştıran gerektirmeyen uygulama dağıtmak için özellikle yararlıdır.

### <a name="installing-applications-and-staging-data"></a>Uygulamaları yükleme ve verileri hazırlama
Bu MSDN forum gönderisi, görevleri çalıştırmak için düğümlerinizin hazırlama çeşitli yöntemler için genel bir bakış sağlar:

[İşlem düğümleri uygulamaları yükleme ve batch veri hazırlama][forum_post]

Bir Azure Batch ekip üyeleri tarafından yazılan uygulamaları ve verileri işlem düğümlerine dağıtmak için kullanabileceğiniz çeşitli teknikler açıklanır.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
