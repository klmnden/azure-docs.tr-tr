---
title: Etkili liste sorguları - Azure Batch tasarlama | Microsoft Docs
description: Havuzlar, işler, görevler gibi Batch kaynaklarını bilgileri istenirken, sorgularınızı filtreleme yaparak performansı artırmak ve işlem düğümleri.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 12/07/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: ff3e95a603b8f9a188c7839578cd12287935de90
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918544"
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Sorguları listesi Batch kaynaklarını verimli bir şekilde oluşturun

İşler, görevler, işlem düğümlerini ve diğer kaynaklarla sorguladığınızda, hizmet tarafından döndürülen veri miktarını azaltarak Azure Batch uygulamanızın performansını artırmak nasıl burada öğreneceksiniz [Batch .NET] [ api_net] kitaplığı.

Herhangi bir türde Batch hizmeti düzenli aralıklarla genellikle sorguları izleme veya işlemi gerçekleştirmek neredeyse tüm Batch uygulamaları gerekir. Örneğin, bir işin kalan herhangi bir kuyruğa alınmış görev olup olmadığını belirlemek için işin her görevde veri almalısınız. Havuzunuzdaki düğümler durumunu belirlemek için havuzdaki her düğümde verileri almanız gerekir. Bu makalede, en verimli şekilde sorgularını yürütmek açıklanmaktadır.

> [!NOTE]
> Batch hizmeti bir işteki görevleri sayım ve Batch havuzundaki işlem düğümlerini sayım ortak senaryolar için özel API desteği sağlar. Bir liste sorgusu için kullanmak yerine, çağırabilirsiniz [Al görevi sayar] [ rest_get_task_counts] ve [listesi düğüm havuzu Sayılmaktadır] [ rest_get_node_counts] operations. Bu işlemleri bir liste sorgusu daha verimlidir, ancak dönüş bilgi daha sınırlı. Bkz: [sayısı görevleri ve işlem düğümleri duruma göre](batch-get-resource-counts.md). 


## <a name="meet-the-detaillevel"></a>DetailLevel karşılamak
Bir üretim ortamında Batch uygulaması, işler, görevler ve işlem düğümleri gibi varlıkları bin numaralandırabilirsiniz. Bu kaynaklar hakkında bilgi istediğinde, potansiyel olarak büyük miktarda veri "kablo Batch hizmetinden uygulamanızı her sorgu üzerinde çapraz gerekir". Bir sorgu tarafından döndürülen bilgi türünü ve öğe sayısını sınırlayarak, sorgularınızın hızını ve bu nedenle uygulamanızın performansını artırabilirsiniz.

Bu [Batch .NET] [ api_net] API kod parçacığı listeleri *her* ile birlikte bir işlemle ilişkili görev *tüm* her görevin özellikleri:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Ancak, sorgunuzu "ayrıntı düzeyi" uygulayarak çok daha verimli bir liste sorgusu gerçekleştirebilirsiniz. Sağlayarak bunu bir [ODATADetailLevel] [ odata] nesnesini [JobOperations.ListTasks] [ net_list_tasks] yöntemi. Bu kod parçacığı, yalnızca kimliği, komut satırı ve işlem düğümü bilgi özellikleri tamamlanan bir görev döndürür:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

İşteki görevler binlerce varsa bu örnek senaryoda, ikinci sorgu sonuçlarından genellikle çok döndürülecek ilk hızlıdır. Batch .NET API'si ile öğeleri listesinde ODATADetailLevel kullanma hakkında daha fazla bilgi bulunmaktadır [aşağıda](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Yüksek oranda olmasını öneririz, *her zaman* ODATADetailLevel nesneye en yüksek verimlilik ve uygulamanızın performansını sağlamak için .NET API listesi çağrılarınızı sağlayın. Ayrıntı düzeyi belirterek, Batch hizmeti yanıt sürelerini, ağ kullanımı artırmak ve istemci uygulamalar tarafından bellek kullanımını en aza indirmek için yardımcı olabilir.
> 
> 

## <a name="filter-select-and-expand"></a>Filtre, seçin ve genişletin
[Batch .NET] [ api_net] ve [Batch REST] [ api_rest] API'leri, hem bir listede, döndürülen öğe sayısını azaltma olanağı sağlar yanı her biri için döndürülen bilgileri tutar. Belirterek bunu **filtre**, **seçin**, ve **dizeleri genişletin** liste sorguları gerçekleştirirken.

### <a name="filter"></a>Filtre
Filtre dizesi döndürülen öğe sayısını azaltan bir ifadedir. Örneğin, bir işin yalnızca çalışan görevlerin listesi veya görevleri çalıştırmak hazır olan işlem düğümleri listeleyin.

* Filtre dizesi, bir özellik adı, işleç ve değer içeren bir ifade olan bir veya daha fazla ifade oluşur. Her bir özellik için desteklenen işleçler gibi sorgu, her varlık türü için belirtilebilecek özelliklerini özgüdür.
* Mantıksal işleçler kullanarak birden fazla ifade birleştirilebilir `and` ve `or`.
* Bu örnekte filtre dizesi listelerini çalışmasını "işleme yalnızca" görevleri: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Şunu seçin:
Select dize her öğe için döndürülen özellik değerleri sınırlar. Özellik adlarının bir listesini belirtin ve öğeleri sorgu sonuçlarında yalnızca bu özellik değerleri döndürülür.

* Select dize bir virgülle ayrılmış özellik adları listesinden oluşur. Varlık türü, sorgulama için özelliklerinden herhangi birini belirtebilirsiniz.
* Bu örnek seçin dize yalnızca üç özellik değerlerini her görev için döndürülmesi gerektiğini belirtir: `id, state, stateTransitionTime`.

### <a name="expand"></a>Genişlet
Genişletme dizesi belirli bilgileri elde etmek için gereken API çağrısı sayısını azaltır. Bir genişletme dizesi kullandığınızda, her öğeyle ilgili daha fazla bilgi edinilebilir tek bir API çağrısı ile. İlk ardından listesindeki her bir öğe için bilgileri istenirken, varlıkların listesini almak yerine bir genişletme dizesi tek bir API çağrısı aynı bilgileri almak için kullanın. Daha az API çağrılarını daha iyi performans gösterir.

* Select dizesine benzer, genişletme dizesi, belirli veri liste sorgu sonuçlarını içerilip içerilmeyeceğini denetler.
* Genişletme dizesi yalnızca, işleri, iş zamanlamalarını, görevleri ve havuzları listesi içinde kullanıldığında desteklenir. Şu an istatistik bilgileri yalnızca destekler.
* Tüm özellikleri gereklidir ve seçim dizesi yok belirtilirse, genişletme dizesi *gerekir* istatistik bilgilerini almak için kullanılır. Bir select dize özelliklerinin bir alt ardından elde etmek için kullanılır, `stats` select dizesinde belirtilebilir ve genişletme dizesi belirtilmesi gerekmez.
* Bu örnekte, dize genişletin listedeki her öğe için istatistik bilgilerini döndürülmesi gerektiğini belirtir: `stats`.

> [!NOTE]
> Üç sorgu dizesi türlerinden herhangi birini oluştururken (filtreleme, seçin ve genişletin) özellik adlarını ve çalışması, REST API öğesi karşılıkları eşleştiğinden emin olun. Örneğin, .NET ile çalışıyorsanız [CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask#microsoft_azure_batch_cloudtask) sınıfı belirtmelisiniz **durumu** yerine **durumu**, .NET özelliğinin olsa bile [ CloudTask.State](/dotnet/api/microsoft.azure.batch.cloudtask#microsoft_azure_batch_cloudtask.state). .NET ve REST API'lerinin arasında özellik eşlemeleri için aşağıdaki tabloya bakın.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Filtresi kuralları seçin ve dizeleri genişletin
* Filtre özellikleri adlarında seçin ve dizeleri genişletin yaptıkları gibi görünmelidir [Batch REST] [ api_rest] kullandığınızda da API-- [Batch .NET] [ api_net]veya toplu Sdk'lardan birini.
* Tüm özellik adları büyük/küçük harfe duyarlıdır, ancak özellik değerleri büyük/küçük harfe duyarsızdır.
* Tarih/saat dizeleri iki biçimlerinden biri olabilir ve ile gelmelidir `DateTime`.
  
  * W3C DTF biçim örneği: `creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * RFC 1123 biçiminde örnek: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Ya da olan Boolean dizeleri `true` veya `false`.
* Bir geçersiz özellik ya da operatör belirtilirse, bir `400 (Bad Request)` sonuçta hata alırsınız.

## <a name="efficient-querying-in-batch-net"></a>Verimli Batch. NET'te sorgulama
İçinde [Batch .NET] [ api_net] API [ODATADetailLevel] [ odata] sınıfı filtre sağlamak için kullanılır, seçin ve listelemek için dizeleri genişletin işlemler. ODataDetailLevel sınıfın oluşturucusunda belirtilen veya doğrudan nesnesinde ayarlanan üç genel dize özellikleri vardır. Ardından ODataDetailLevel nesnesini parametre olarak için çeşitli listeleme işlemleri gibi geçirdiğiniz [ListPools][net_list_pools], [ListJobs][net_list_jobs], ve [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[ FilterClause][odata_filter]: Döndürülen öğe sayısını sınırlayın.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: Her bir öğeyle döndürülen hangi özellik değerlerini belirtin.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: Her öğe için ayrı çağrılar yerine tek bir API çağrısı içindeki tüm öğeler için verileri alır.

Aşağıdaki kod parçacığı havuzları belirli bir dizi istatistiklerini için Batch hizmetini verimli bir şekilde sorgulamak için Batch .NET API kullanır. Bu senaryoda hem test hem de üretim havuzuna toplu kullanıcı sahiptir. "Test" ile test havuzu kimliklerini ön eki ve ürün havuzu kimlikleri "üretim" öneki. Kod parçacığında *myBatchClient* düzgün başlatılmadı örneğidir [BatchClient](/dotnet/api/microsoft.azure.batch.batchclient#microsoft_azure_batch_batchclient) sınıfı.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> Örneği [ODATADetailLevel] [ odata] seçim ile yapılandırılmış ve genişletme yan tümceleri de geçirilebilir uygun Get yöntemleri gibi [PoolOperations.GetPool](/dotnet/api/microsoft.azure.batch.pooloperations#Microsoft_Azure_Batch_PoolOperations_GetPool_System_String_Microsoft_Azure_Batch_DetailLevel_System_Collections_Generic_IEnumerable_Microsoft_Azure_Batch_BatchClientBehavior__), döndürülen veri miktarını sınırlamak için.
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a>Batch .NET API'si eşlemeleri REST
Filtre, özellik adları seçin ve dizeleri genişletin *gerekir* REST API karşılıkları, adı ve durum yansıtır. Aşağıdaki tablolarda, .NET ve REST API'si karşılıkları arasındaki eşlemeleri sağlar.

### <a name="mappings-for-filter-strings"></a>Filtre dizeleri eşlemeleri
* **.NET listesi yöntemleri**: Bu sütundaki .NET API yöntemlerin her biri kabul eden bir [ODATADetailLevel] [ odata] bir parametre olarak nesne.
* **REST listesi istekleri**: Bu sütunda bağlı her REST API sayfası özelliklerini ve izin verilen işlemleri belirten bir tablo içerir *filtre* dizeleri. Oluşturduğunuzda, bu özellik adları ve işlemleri kullanacağı bir [ODATADetailLevel.FilterClause] [ odata_filter] dize.

| .NET listesi yöntemleri | REST listesi istekleri |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Bir hesapta sertifikalarını listeleme][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[Bir görev ile ilişkili dosyaları Listele][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[İş hazırlama ve iş sürüm görevleri bir işin durumunu listeleyin][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Bir hesaptaki işleri listele][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Bir düğüm üzerindeki dosyalar][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Bir işi ile ilişkili görevleri listeleyin][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Bir hesaptaki iş zamanlamaları listesinde][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Bir iş zamanlamasıyla ilişkili işleri listele][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Bir havuzdaki işlem düğümleri listeleyin][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Bir hesapta havuzları listesi][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Select dizeleri eşlemeleri
* **Batch .NET türleri**: Batch .NET API'si türleri.
* **REST API varlıkları**: Bu sütundaki her bir sayfa türü için REST API özellik adları listesinden bir veya daha fazla tablo içerir. Bu özellik adlarını, oluştururken kullanılan *seçin* dizeleri. Oluşturduğunuzda, bu aynı özellik adları kullanacağınız bir [ODATADetailLevel.SelectClause] [ odata_select] dize.

| Batch .NET türleri | REST API varlıklar |
| --- | --- |
| [Sertifika][net_cert] |[Bir sertifika hakkında bilgi edinin][rest_get_cert] |
| [CloudJob][net_job] |[Bir iş hakkında bilgi edinin][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Bir iş zamanlaması hakkında bilgi edinin][rest_get_schedule] |
| [ComputeNode][net_node] |[Bir düğüm hakkında bilgi edinin][rest_get_node] |
| [CloudPool][net_pool] |[Bir havuz hakkında bilgi edinin][rest_get_pool] |
| [CloudTask][net_task] |[Bir görev hakkında bilgi edinin][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Örnek: bir filtre dizesi oluşturun
İçin bir filtre dizesi oluşturmak zaman [ODATADetailLevel.FilterClause][odata_filter], yukarıdaki tabloda "Eşlemeleri için filtre dizelerinde" altında karşılık gelen bulma REST API belgeleri sayfasına bakın gerçekleştirmek istediğiniz listeleme işlemi. Bu sayfadaki ilk çok satırlı tabloda filtrelenebilir özelliklerini ve bunların desteklenen işleçleri bulabilirsiniz. Çıkış kodu sıfır olmayan tüm görevleri almak istiyorsanız, örneğin, bu satır üzerinde [işi ile ilişkili görevleri listeleyin] [ rest_list_tasks] izin verilen işleçler ve ilgili özellik dize belirtir:

| Özellik | İzin verilen işlemleri | Type |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Bu nedenle, sıfır olmayan çıkış kodu ile tüm görevleri listelemek için filtre dizesini olacaktır:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Örnek: select bir dize oluşturur.
Oluşturmak için [ODATADetailLevel.SelectClause][odata_select], yukarıdaki tabloda "Select dizeleri eşlemeleri" altında başvurun ve listeleme varlık türü için karşılık gelen REST API sayfasına gidin. Bu sayfada ilk çok satırlı tablodaki seçilebilir özelliklerini ve bunların desteklenen işleçleri bulabilirsiniz. Yalnızca kimliği ve her görev bir liste için komut satırı almak istiyorsanız, örneğin, bu satırlar geçerli tabloda üzerinde bulursunuz [bir görev hakkında bilgi alma][rest_get_task]:

| Özellik | Type | Notlar |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Kimliği ve komut satırında listelenen her görev yalnızca dahil etmek için seçim dizesi şöyle olacaktır:

`id, commandLine`

## <a name="code-samples"></a>Kod örnekleri
### <a name="efficient-list-queries-code-sample"></a>Etkili liste sorguları kod örneği
Kullanıma [EfficientListQueries] [ efficient_query_sample] nasıl etkili sorgulamayı listesini görmek için GitHub üzerindeki örnek proje, uygulama performansını etkileyebilir. Bu C# konsol uygulaması oluşturur ve çok sayıda görevler bir projeye ekler. Ardından, çoklu çağrılardan kolaylaştırır [JobOperations.ListTasks] [ net_list_tasks] yöntemi ve geçişleri [ODATADetailLevel] [ odata] olan nesneler döndürülecek veri miktarına bağlı olarak değişen farklı özellik değerleri ile yapılandırılır. Bu, aşağıdakine benzer bir çıktı üretir:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Geçen kez gösterildiği gibi özellikler ve döndürülen öğe sayısını sınırlayarak sorgusu yanıt sürelerini önemli ölçüde düşürebilirsiniz. Bu ve diğer örnek projelerde bulabilirsiniz [azure-batch-samples] [ github_samples] github deposu.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics kitaplığı ve kod örneği
EfficientListQueries Yukarıdaki kod örneğinde yanı sıra, bulduğunuz [BatchMetrics] [ batch_metrics] projesi [azure-batch-samples] [ github_samples]GitHub deposu. BatchMetrics örnek proje, verimli ve Batch API'sini kullanarak Azure Batch iş ilerleme durumunu izlemek nasıl gösterir.

[BatchMetrics] [ batch_metrics] örnek içeren, kendi projeleriniz ve çalışma ve Kütüphane kullanımını gösteren basit bir komut satırı programı dahil edebilirsiniz bir .NET sınıf kitaplığı projesi.

Örnek uygulama proje içinde aşağıdaki işlemleri gösterir:

1. Yalnızca gereksinim duyduğunuz özellikleri indirebilmek için belirli öznitelikleri seçme
2. Değişiklikler yalnızca bu yana son sorgu indirebilmek için durum geçiş süreleri filtreleme

Örneğin, aşağıdaki yöntemi BatchMetrics Kitaplığı'nda görünür. Yalnızca belirten bir ODATADetailLevel döndürür `id` ve `state` özellikleri için sorgulanır varlıklar elde. Ayrıca, belirtir yalnızca varlık durumu belirtilen bu yana değişti `DateTime` parametresi döndürülemez.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Sonraki adımlar
### <a name="parallel-node-tasks"></a>Düğüm Paralel Görevler
[Eşzamanlı düğüm görevleri ile Azure toplu işlem kaynak kullanımını en üst düzeye](batch-parallel-node-tasks.md) başka bir makalede Batch uygulama performansı ile ilişkilidir. İş yüklerinin bazı türleri üzerinde Paralel Görevler yürütülürken avantaj elde edebileceği büyük--ancak daha az--işlem düğümleri. Kullanıma [Örnek senaryo](batch-parallel-node-tasks.md#example-scenario) makalede bu tür bir senaryonun hakkında ayrıntılı bilgi için.


[api_net]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

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

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: /rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
