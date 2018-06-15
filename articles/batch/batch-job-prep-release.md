---
title: İşlerini ve işlem düğümlerinde - Azure Batch tam işleri hazırlamak için Görevler oluşturun | Microsoft Docs
description: Azure toplu işlem düğümleri için veri aktarımını en aza indirmek için iş düzeyinde hazırlama görevleri kullanın ve iş tamamlanma düğümü temizleme görevleri bırakın.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 543c03c22b31389c3d6e048cc9f13c24add5aae7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30314730"
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>İş hazırlama ve iş sürüm görevleri toplu işlem düğümleri

 Bir Azure toplu işlem, genellikle görevlerinin yürütülür ve Bakım görevlerinin tamamlandığında sonrası iş önce kurulum çeşit gerektirir. Ortak görev giriş verileri, işlem düğümlerinizi indirme veya işi tamamlandıktan sonra görev çıktı verileri Azure depolama alanına yükleme gerekebilir. Kullanabileceğiniz **iş hazırlama** ve **iş yayın** bu işlemleri gerçekleştirmek için görevler.

## <a name="what-are-job-preparation-and-release-tasks"></a>Hangi iş hazırlama ve görevleri?
Bir iş görevlerinin çalıştırmadan önce en az bir görev çalıştırmak için zamanlanan tüm işlem düğümlerinde iş hazırlama görevini çalıştırır. İş tamamlandığında iş bırakma görevi havuzun en az bir görev yürütmüş her düğümünde çalıştırılır. Normal toplu görevler gibi bir iş hazırlık veya yayın görevi çalıştırıldığında çağrılacak komut satırını belirtebilirsiniz.

İş hazırlama ve bırakma görevleri dosya indirme gibi bilinen toplu görev özellikler sunar ([kaynak dosyaları][net_job_prep_resourcefiles]), yükseltilmiş yürütme, özel ortam değişkenleri, maksimum yürütme süresi, yeniden deneme sayısı ve dosyayı elde tutma süresi.

Aşağıdaki bölümlerde, nasıl kullanılacağını öğreneceksiniz [JobPreparationTask] [ net_job_prep] ve [JobReleaseTask] [ net_job_release] sınıfları bulundu [Batch .NET] [ api_net] kitaplığı.

> [!TIP]
> İş hazırlama ve bırakma görevleri "havuzu paylaşılan" ortamlarda, işlem düğümleri havuzu iş çalıştırmaları arasında devam ederse ve birçok iş tarafından kullanılan özellikle yararlı olur.
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a>İş hazırlama kullanın ve görevleri yayın zamanı
İş hazırlama ve iş sürüm görevleri aşağıdaki durumlar için iyi bir tercihtir şunlardır:

**Ortak görev verileri indirin**

Toplu veri ortak bir dizi iş görevleri için giriş olarak genellikle gerektirir. Örneğin, günlük risk analizi hesaplamalarda Pazar veri işi özgü, işteki tüm görevlerin henüz ortak. Bu Pazar veriler, genellikle birkaç gigabayt cinsinden boyutu, düğüm üzerinde çalışan herhangi bir görev kullanabilmesi için her işlem düğümü yalnızca bir kez indirilmelidir. Kullanım bir **iş hazırlama görevi** işinin yürütülmesi diğer görevleri kullanıcının önce bu verileri her düğüme karşıdan yüklemek için.

**İş ve Görev çıkış Sil**

Burada bir havuzun işlem düğümleri işleri arasında yetkisi olmayan, bir "havuzu paylaşılan" ortamında çalıştırmaları arasında iş veri silmeniz gerekebilir. Düğümlerde disk alanından tasarruf etmek ya da kuruluşunuzun güvenlik ilkelerine uygun gerekebilir. Kullanım bir **iş bırakma görevi** iş hazırlama görevi tarafından indirilen veya görev yürütme sırasında oluşturulan verilerini silmek için.

**Günlük tutma**

Görevlerinizin oluşturduğu günlük dosyalarının bir kopyasını saklamak veya belki de başarısız uygulamalar tarafından üretilen döküm dosyalarını çökme isteyebilirsiniz. Kullanım bir **iş bırakma görevi** sıkıştırır ve bu verileri karşıya yüklemek için bu gibi durumlarda bir [Azure Storage] [ azure_storage] hesabı.

> [!TIP]
> Günlükleri ve diğer iş ve görev devam etmek için başka bir yol çıkış veri kullanmak için [Azure toplu işlem dosyası kuralları](batch-task-output.md) kitaplığı.
> 
> 

## <a name="job-preparation-task"></a>İş hazırlama görevi
Bir iş görevlerinin çalışmaya başlamadan önce toplu bir görevi çalıştırmak için zamanlanan her işlem düğümü üzerinde iş hazırlama görevi yürütür. Varsayılan olarak, Batch hizmeti düğüm üzerinde yürütmek için zamanlanmış görevler çalıştırmadan önce tamamlanması gereken iş hazırlama görevi bekler. Ancak, beklememek hizmetini yapılandırabilirsiniz. Düğümü yeniden başlatılırsa, iş hazırlama görevi yeniden çalışır, ancak aynı zamanda bu davranışı devre dışı bırakabilirsiniz.

İş hazırlama görevi görevi çalıştırmak için zamanlanan düğümlerinde yürütülür. Bir düğüm görev atanmamış durumunda bu hazırlık görevi gereksiz yürütme önler. Bir iş için görevleri sayısı bir havuzdaki düğümlerin sayısından daha az olduğunda bu durum oluşabilir. Ne zaman da uygulanır [eşzamanlı görev yürütme](batch-parallel-node-tasks.md) olduğu etkin görev sayısı, bazı düğümler boşta IF bırakır toplam olası eş zamanlı görevleri düşüktür. İş hazırlama görevi boşta düğümlerinde çalıştırarak değil, veri aktarımı ücretlerine üzerinde daha az para harcamanız.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] farklıdır [CloudPool.StartTask] [ pool_starttask] JobPreparationTask ancak her bir iş başlangıcında yürütür, StartTask yalnızca zaman bir işlem düğümünde önce bir havuz birleştirir yürütür veya yeniden başlatır.
> 
> 

## <a name="job-release-task"></a>İş bırakma görevi
Bir işi tamamlandı olarak işaretlendikten sonra iş bırakma görevi havuzun en az bir görev yürütmüş her düğümünde çalıştırılır. Bir iş, bir sonlandırma isteği vererek tamamlanan olarak işaretleyin. Batch hizmeti sonra iş durumu ayarlar *sonlandırma*işle ilişkili etkin ya da çalışan görevleri sonlandırır ve iş bırakma görevini çalıştırır. İş sonra taşır *tamamlandı* durumu.

> [!NOTE]
> İş silme ayrıca iş bırakma görevi yürütür. Bir iş zaten sona erdi, işi daha sonra silinirse, ancak sürüm görev ikinci kez çalıştırılmaz.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Hazırlığı işi ve görevleri Batch .NET ile bırakın
İş hazırlama görevi kullanmak için Ata bir [JobPreparationTask] [ net_job_prep] , iş nesnesine [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] özelliği. Benzer şekilde, başlatma bir [JobReleaseTask] [ net_job_release] ve, işin atayın [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] iş bırakma görevi ayarlamak için özellik.

Bu kod parçacığında bulunan `myBatchClient` örneği [BatchClient][net_batch_client], ve `myPool` toplu işlem hesabı içinde varolan bir havuzu.

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

Bir işi sona erdi veya silindiğinde daha önce belirtildiği gibi bırakma görevi yürütülür. Bir işin sonlandırmak [JobOperations.TerminateJobAsync][net_job_terminate]. Bir işin silme [JobOperations.DeleteJobAsync][net_job_delete]. Genellikle sonlandırma veya bir iş görevlerinin tamamlandığında veya tanımladığınız bir zaman aşımı erişildiğinde silin.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Github'daki kod örneği
İş hazırlama görmek ve eylem görevlerinde yayın için kullanıma [JobPrepRelease] [ job_prep_release_sample] örnek proje github'da. Bu konsol uygulamasını şunları yapar:

1. Bir havuz ile iki "küçük" düğümü oluşturur.
2. Bir işi, iş hazırlama, sürüm ve standart görevler oluşturur.
3. İlk düğüm kimliği bir düğümün "paylaşılan" dizininde bir metin dosyasına yazan iş hazırlama görevini çalıştırır.
4. Görev Kimliğine aynı metin dosyasına yazan her bir düğümde bir görev çalıştırır.
5. Tüm görevler tamamlandığında (veya zaman aşımı ulaşıldıktan sonra), konsola her düğümün metin dosyasının içeriğini yazdırır.
6. İş tamamlandığında düğümden dosyayı silmek için iş bırakma görevini çalıştırır.
7. İş hazırlama ve bırakma görevleri üzerinde yürütülen her düğüm için çıkış kodlarını yazdırır.
8. Duraklatır yürütme'işinin ve/veya havuzu silme onayı izin vermek için.

Örnek uygulamasının çıktısının aşağıdakine benzer:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
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
> Değişken oluşturma ve başlangıç saatini (bazı düğümler diğerlerinden önce görevler için hazır) yeni bir havuzdaki düğümlerin nedeniyle, farklı çıkış görebilirsiniz. Özellikle, görevleri hızlı bir şekilde tamamlamak için havuzdaki düğümlerin birinde tüm iş görevleri yürütür. Bu meydana gelirse, iş prep görürsünüz ve bırakma görevleri hiçbir görev yürütülen düğümü için mevcut değil.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>İş hazırlama ve bırakma görevleri Azure portalında inceleyin.
Örnek uygulamayı çalıştırdığınızda, kullanabileceğiniz [Azure portal] [ portal] işi ve görevleri özelliklerini görüntülemek veya hatta iş görevleri tarafından değiştirilmiş paylaşılan metin dosyasını indirin.

Aşağıda gösterildiği ekran **hazırlama görevleri dikey** örnek uygulamasının bir farklı çalıştır sonra Azure portalında. Gidin *JobPrepReleaseSampleJob* özellikleri görevleri tamamladıktan sonra (ancak işi ve havuzu silmeden önce) tıklatıp **hazırlama görevleri** veya **yayın görevleri** özelliklerini görüntülemek için.

![İş hazırlama özelliklerini Azure portalında][1]

## <a name="next-steps"></a>Sonraki adımlar
### <a name="application-packages"></a>Uygulama paketleri
İş hazırlama görevi ek olarak da kullanabilirsiniz [uygulama paketleri](batch-application-packages.md) işlem düğümleri için görev yürütme hazırlamak için Toplu özellik. Bu özellik bir yükleyici, çok sayıda (100 +) dosyalar içeren uygulamalar veya katı sürüm denetimi gerektiren uygulamalar çalıştıran gerektirmeyen uygulamalarını dağıtmak için özellikle yararlıdır.

### <a name="installing-applications-and-staging-data"></a>Uygulama yükleme ve verileri hazırlama
Bu MSDN Forumu gönderisi düğümleriniz hazırlama görevleri çalıştırmak için çeşitli yöntemler genel bir bakış sağlar:

[İşlem düğümlerine uygulamaların yüklenmesi ve toplu veri hazırlama][forum_post]

Azure Batch ekip üyelerinin biri tarafından yazılmış, uygulamaları ve verileri işlem düğümleri için dağıtmak için kullanabileceğiniz çeşitli teknikleri açıklar.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
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
