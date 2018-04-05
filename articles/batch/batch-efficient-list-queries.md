---
title: Verimli listesi sorguları - Azure Batch tasarlama | Microsoft Docs
description: Havuzlar, işler, görevler gibi Batch kaynaklarını hakkında bilgi isterken sorgularınızı filtreleyerek performansını artırmak ve işlem düğümleri.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 330350d6ac6838ea5b09763fe1f73fab1934710c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Sorguları listesi Batch kaynaklarını verimli bir şekilde oluşturun

Burada, sorgu işler, görevler ve işlem düğümleri ile hizmet tarafından döndürülen veri miktarını azaltarak Azure Batch uygulamanızın performansını artırmak öğreneceksiniz [Batch .NET] [ api_net]kitaplığı.

Neredeyse tüm Batch uygulamaları bazı türden bir Batch hizmeti düzenli aralıklarla genellikle sorgular izleme ya da başka işlem yapmanız gerekebilir. Örneğin, bir işin kalan tüm kuyruğa alınmış görevlerin olup olmadığını belirlemek için işteki her görevde veri almalısınız. Havuzunuzdaki düğümler durumunu belirlemek için verileri havuzdaki her düğümde edinmeniz gerekir. Bu makale, en verimli şekilde sorgularını yürütmek açıklanmaktadır.

> [!NOTE]
> Toplu işlem hizmetinin işteki görevler sayım yaygın bir senaryo için özel API desteği sağlar. Bir liste sorgusu için kullanmak yerine, çağırabilirsiniz [alma görev sayar] [ rest_get_task_counts] işlemi. Get görev sayıları kaç görevleri bekleyen, çalışan veya tamamlamak ve kaç tane görevlerin başarılı veya başarısız olduğunu gösterir. Görev Get sayar, liste sorgusu daha etkilidir. Daha fazla bilgi için bkz: [saymak durumuna (Önizleme) göre bir iş için görevleri](batch-get-task-counts.md). 
>
> Görev sayar alma işlemi 2017 06 01.5.1'den önceki toplu işlem hizmeti sürümlerinde kullanılamaz. Hizmeti daha eski bir sürümü kullanıyorsanız, bir liste sorgusu işteki görevler yerine saymak için kullanın.
>
> 

## <a name="meet-the-detaillevel"></a>DetailLevel karşılayan
Bir üretimde toplu uygulama varlıkları işler, görevler ve işlem düğümü gibi binlerce sayı. Bu kaynaklar hakkında bilgi istediklerinde, potansiyel olarak büyük miktarda veri "kablo toplu hizmetinden uygulamanıza her sorguda geçmelidir". Öğe sayısı ve türü bir sorgu tarafından döndürülen bilgilerin kısıtlayarak, sorgularınızı hızına ve bu nedenle, uygulamanızın performansını artırabilir.

Bu [Batch .NET] [ api_net] API kod parçacığını listeleri *her* ile birlikte bir işlemle ilişkili görev *tüm* her görev özellikleri:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Ancak, sorgunuza "ayrıntı düzeyi" uygulayarak çok daha verimli bir liste sorgusu gerçekleştirebilirsiniz. Sağlayarak bunu bir [ODATADetailLevel] [ odata] nesnesini [JobOperations.ListTasks] [ net_list_tasks] yöntemi. Bu kod parçacığında, yalnızca kimliği, komut satırı ve işlem düğümü bilgi tamamlanan görevler özelliklerini döndürür:

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

İşteki görevler binlerce varsa bu örnek senaryoda, ikinci sorgusundan gelen sonuçları genellikle çok döndürülecek ilk hızlıdır. Batch .NET API'si öğeleriyle listelediğinizde ODATADetailLevel kullanma hakkında daha fazla bilgi bulunmaktadır [aşağıda](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Yüksek oranda öneririz, *her zaman* ODATADetailLevel nesneye en yüksek verimlilik ve uygulamanızın performansını sağlamak için .NET API listesi çağrılarınızı sağlayın. Ayrıntı düzeyi belirterek, Batch hizmeti yanıt sürelerini alt ağ kullanımı iyileştirirsiniz ve istemci uygulamaları tarafından bellek kullanımını en aza indirmek için yardımcı olabilir.
> 
> 

## <a name="filter-select-and-expand"></a>Filtre, seçin ve genişletin
[Batch .NET] [ api_net] ve [Batch REST] [ api_rest] API'leri hem bir listede, döndürülen öğe sayısını düşürmek için olanağı sağlar yanı her biri için döndürülen bilgi tutarını. Belirterek bunu **filtre**, **seçin**, ve **dizeleri genişletin** listesi sorguları gerçekleştirirken.

### <a name="filter"></a>Filtre
Filtre dizesi döndürülen öğe sayısını azaltan bir ifadedir. Örneğin, bir iş için yalnızca çalışan görevleri listesinde ya da görevleri çalıştırmak hazır olan işlem düğümleri listeleyin.

* Filtre dizesi olan bir veya daha fazla ifade, bir özellik adı, işleç ve değer oluşan bir ifade oluşur. Her bir özellik için desteklenen işleçleri olarak belirtilebilir özellikler, sorgu, her bir varlık türü özgüdür.
* Mantıksal işleçler kullanarak birden çok ifadeleri birleştirilebilir `and` ve `or`.
* Bu örnek filtre dizesi listelerini çalışmasını "işleme yalnızca" görevler: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Şunu seçin:
Select dize her öğe için döndürülen özellik değerlerini sınırlar. Özellik adlarının bir listesini belirtin ve yalnızca bu özellik değerlerini sorgu sonuçlarında öğeleri için döndürülür.

* Select dize virgülle ayrılmış listesini özellik adlarının oluşur. Sorgulama varlık türü için özelliklerinden herhangi birini belirtebilirsiniz.
* Bu örnek Seç dize her görev için yalnızca üç özellik değerlerini döndürülmelidir belirtir: `id, state, stateTransitionTime`.

### <a name="expand"></a>Genişlet
Genişletilecek dize belirli bilgileri elde etmek için gereken API çağrılarının sayısını azaltır. Genişletme dizisi kullandığınızda, her öğe hakkında daha fazla bilgi alınabilir tek bir API çağrısı ile. İlk listesinde, her bir öğe için bilgi isteyen varlıkların listesi alma yerine bir genişletme dize tek bir API çağrısı aynı bilgileri almak için kullanın. Daha az API çağrıları daha iyi performans anlamına gelir.

* Select dizeye benzer, genişletilecek dize, bazı verileri listesi sorgu sonuçlarında dahil edilip edilmeyeceğini denetler.
* İşlerini, iş zamanlamalarını, görevler ve havuzları liste kullanıldığında genişletme dize yalnızca desteklenir. Şu anda, istatistik bilgileri yalnızca destekler.
* Tüm özellikleri gereklidir ve select dize belirtilirse, genişletilecek dize *gerekir* istatistik bilgileri almak için kullanılır. Select dize özelliklerinin bir alt ardından elde etmek için kullanılır, `stats` select dizesinde belirtilen ve Genişletilecek dize belirtilmesi gerekmez.
* Bu örnek genişletin dize istatistik bilgileri, listedeki her bir öğe için döndürülmelidir belirtir: `stats`.

> [!NOTE]
> Üç sorgu dizesi türlerinden herhangi birini oluşturulurken (filtre, seçin ve genişletin), özellik adları ve çalışması, REST API öğesi dekiler eşleştiğinden emin olun. Örneğin, .NET ile çalışırken [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) sınıfı belirtmeniz gerekir **durumu** yerine **durumu**, .NET özelliğini olsa bile [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). .NET ve REST API'leri arasında özellik eşlemeleri için aşağıdaki tablolara bakın.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Filtre için kuralları seçin ve dizeleri genişletin
* Filtre, Özellikler adlarında seçin ve dizeleri genişletin yaptıkları gibi görünmelidir [Batch REST] [ api_rest] kullandığınızda da API-- [Batch .NET] [ api_net]veya diğer toplu Sdk'lardan birini.
* Tüm özellik adları büyük/küçük harfe duyarlıdır, ancak özellik değerleri büyük küçük harfe duyarlı.
* Tarih/saat dizeleri iki biçimlerden birinde olabilir ve ile gelmelidir `DateTime`.
  
  * W3C DTF biçimi örneği: `creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * RFC 1123 biçimi örneği: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Boole dizelerdir ya da `true` veya `false`.
* Bir geçersiz özellik ya da operatör belirtilirse, bir `400 (Bad Request)` hata neden olur.

## <a name="efficient-querying-in-batch-net"></a>Verimli Batch .NET içinde sorgulama
İçinde [Batch .NET] [ api_net] API, [ODATADetailLevel] [ odata] sınıfı, filtre sağlama için kullanılır, seçin ve listeye dizeleri genişletin işlemler. ODataDetailLevel sınıfı oluşturucuda belirtilen veya doğrudan nesnesinde ayarlanan üç genel dize özellikleri vardır. Ardından ODataDetailLevel nesnesini parametre olarak çeşitli listeleme işlemleri gibi geçirdiğiniz [ListPools][net_list_pools], [ListJobs][net_list_jobs], ve [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[FilterClause][odata_filter]: Limit the number of items that are returned.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: hangi özellik değerlerini her bir öğeyle döndürülür belirtin.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: tek bir API tüm öğeleri için verileri almak yerine ayrı çağrıları her öğe için çağırın.

Aşağıdaki kod parçacığını Batch .NET API'si havuzları belirli bir dizi istatistiklerini Batch hizmeti verimli bir şekilde sorgulamak için kullanır. Bu senaryoda, test ve Üretim havuzları toplu kullanıcı sahiptir. Test havuzu kimlikleri "ile bir test" öneki ve üretim havuzu kimlikleri "ile üretim" öneki. Parçacığında bulunan *myBatchClient* düzgün başlatılmadı örneğidir [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) sınıfı.

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
> Örneği [ODATADetailLevel] [ odata] seçin ile yapılandırılmış ve genişletme yan tümceleri de geçirilebilir uygun Get yöntemleri gibi [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), döndürülen veri miktarını sınırlamak için.
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a>Batch REST .NET API eşlemeleri
Filtre, özellik adlarını seçin ve dizeleri genişletin *gerekir* REST API'dekiler, hem adı ve durum yansıtır. Aşağıdaki tablolarda, .NET ve REST API ortaklarınıza arasındaki eşlemeleri sağlar.

### <a name="mappings-for-filter-strings"></a>Filtre dizeleri eşlemeleri
* **.NET listesi yöntemleri**: Bu sütunda .NET API yöntemlerin her biri kabul eden bir [ODATADetailLevel] [ odata] nesnesini parametre olarak.
* **REST listesi istekleri**: Bu sütuna bağlı her REST API sayfa izin işlemleri ve özellikleri belirten bir tablo içeriyor *filtre* dizeleri. Oluşturmak, bu özellik adları ve işlemleri için kullanacağı bir [ODATADetailLevel.FilterClause] [ odata_filter] dize.

| .NET listesi yöntemleri | REST listesi istekleri |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Bir hesap sertifikaları listesi][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[Bir görev ile ilişkili dosyaları listesi][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[İş hazırlama ve iş sürüm görevleri bir iş için durumu listesi][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Bir hesap işleri listesi][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Bir düğümdeki dosya listesi][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Bir işi ile ilişkili görevleri listeler][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Bir hesaptaki iş zamanlamalarını listesi][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Bir iş zamanlaması ile ilişkili işleri listesi][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Bir havuzdaki işlem düğümleri listesi][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Bir hesap havuzlarını Listele][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Select dizeleri eşlemeleri
* **Batch .NET türleri**: Batch .NET API'si türleri.
* **REST API varlıklar**: Bu sütundaki her bir sayfa türü için REST API özellik adları listesinde bir veya daha fazla tablo içeriyor. Bu özellik adları, yapısı oluştururken kullanılan *seçin* dizeleri. Oluşturmak, bu aynı özellik adları için kullanacağı bir [ODATADetailLevel.SelectClause] [ odata_select] dize.

| Batch .NET türleri | REST API varlıklar |
| --- | --- |
| [Sertifika][net_cert] |[Bir sertifika hakkında bilgi edinin][rest_get_cert] |
| [CloudJob][net_job] |[Bir iş hakkında bilgi edinin][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Bir iş zamanlaması hakkında bilgi edinin][rest_get_schedule] |
| [ComputeNode][net_node] |[Bir düğüm hakkında bilgi edinin][rest_get_node] |
| [CloudPool][net_pool] |[Bir havuzu hakkında bilgi edinin][rest_get_pool] |
| [CloudTask][net_task] |[Bir görev hakkında bilgi edinin][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Örnek: bir filtre dizesi oluşturun
Bir filtre dizesi oluşturmak zaman [ODATADetailLevel.FilterClause][odata_filter], yukarıdaki tabloda "Eşlemeleri filtre dizeleri için" altında karşılık gelen Bul REST API belgelerine sayfasına bakın gerçekleştirmek istediğiniz işlem listesi. Bu sayfada ilk çok satırlı tablodaki filtrelenebilir özellikleri ve bunların desteklenen işleçleri bulacaksınız. Çıkış kodu sıfır olmayan tüm görevler almak isterseniz, örneğin, bu satırın üzerinde [bir işlemle ilişkili görevleri listesinde] [ rest_list_tasks] geçerli özellik dizesi ve izin verilen işleçleri belirtir:

| Özellik | İzin verilen işlemler | Tür |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Bu nedenle, sıfır olmayan çıkış kodu ile tüm görevleri listeleme için filtre dizesini olacaktır:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Örnek: select dizesi oluşturun
Oluşturmak için [ODATADetailLevel.SelectClause][odata_select], yukarıdaki tabloda "Select dizeleri eşlemeleri" altında başvurun ve listeleme varlık türüne karşılık gelen REST API sayfasına gidin. Bu sayfada ilk çok satırlı tablodaki seçilebilir özellikleri ve bunların desteklenen işleçleri bulacaksınız. Yalnızca kimliği ve listedeki her görev için komut satırı almak isterseniz, örneğin, bu satırlar geçerli tabloda üzerinde bulacaksınız [bir görev hakkında bilgi alma][rest_get_task]:

| Özellik | Tür | Notlar |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Yalnızca kimliği ve listelenen her görev komut satırıyla dahil etmek için seçim dizesi sonra aşağıdaki gibi olmalıdır:

`id, commandLine`

## <a name="code-samples"></a>Kod örnekleri
### <a name="efficient-list-queries-code-sample"></a>Verimli listesi sorguları kod örneği
Kullanıma [EfficientListQueries] [ efficient_query_sample] örnek proje nasıl verimli sorgulama listesini görmek için GitHub üzerindeki bir uygulama performansını etkileyebilir. Bu C# konsol uygulaması oluşturur ve çok sayıda görevler bir projeye ekler. Ardından, birden çok çağrıları yapan [JobOperations.ListTasks] [ net_list_tasks] yöntemi ve geçişleri [ODATADetailLevel] [ odata] nesneleri döndürülecek veri miktarı değiştirmek için farklı özellik değerleri ile yapılandırılır. Aşağıdakine benzer bir çıktı üretir:

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

Geçen kez gösterildiği gibi özellikleri ve döndürülen öğe sayısını sınırlayarak sorgusu yanıt sürelerini önemli ölçüde düşürebilirsiniz. Bu ve diğer örnek projelerinde bulabilirsiniz [azure-batch-samples] [ github_samples] github'daki.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics kitaplığı ve kod örneği
EfficientListQueries kod örneği yanı sıra yukarıdaki, bulduğunuz [BatchMetrics] [ batch_metrics] proje [azure-batch-samples] [ github_samples]GitHub depo. BatchMetrics örnek proje verimli bir şekilde Batch API'sini kullanarak Azure Batch iş ilerleme durumunu izlemek nasıl gösterir.

[BatchMetrics] [ batch_metrics] örnek, kendi projeleri ve çalışma ve kitaplık kullanımı tanıtmak için basit bir komut satırı program dahil edebilirsiniz .NET sınıf kitaplığı proje içerir.

Proje içinde örnek uygulama aşağıdaki işlemleri gösterir:

1. Özel öznitelikler yalnızca gereksinim duyduğunuz özellikleri indirmek için seçme
2. Değişiklikler yalnızca son sorgu itibaren indirmek için durum geçiş süreleri filtreleme

Örneğin, aşağıdaki yöntemi BatchMetrics Kitaplığı'nda görünür. Yalnızca belirleyen bir ODATADetailLevel döndürür `id` ve `state` özellikleri için sorgulanır varlıklar elde. Ayrıca, belirtir, durumu belirtilen bu yana değişti varlıklar `DateTime` parametresi döndürülmesi.

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
### <a name="parallel-node-tasks"></a>Paralel düğüm görevleri
[Eşzamanlı düğüm görevleri ile Azure toplu işlem kaynak kullanımını en üst düzeye](batch-parallel-node-tasks.md) başka bir makaleye toplu uygulama performansı ile ilgili. İş yüklerinin bazı türleri üzerinde Paralel Görevler yürütülürken yararlanabilir büyük--ancak daha az--işlem düğümlerini. Kullanıma [Örnek senaryo](batch-parallel-node-tasks.md#example-scenario) makalede böyle bir senaryo hakkında ayrıntılı bilgi için.

### <a name="batch-forum"></a>Toplu işlem Forumu
[Azure toplu işlem Forumu] [ forum] MSDN'de toplu ele almaktadır ve hizmet hakkında sorular sormak için iyi bir yerdir. HEAD üzerinde üzerinden faydalı "Yapışkan" gönderiler için ve Batch çözümlerinizi derleme sırasında çıktıkları anda sorularınızı gönderin.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
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

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job