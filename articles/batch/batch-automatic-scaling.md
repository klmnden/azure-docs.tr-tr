---
title: "Ölçek işlem düğümlerini Azure Batch havuzunda otomatik olarak | Microsoft Docs"
description: "Havuzdaki işlem düğümü sayısını dinamik olarak ayarlamak için bir bulut havuzunda otomatik ölçeklendirmeyi etkinleştirebilirsiniz."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0e49cd8a64a48c53f5b6104703164a597c797f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Bir Batch havuzunda işlem düğümlerini ölçeklendirme bir otomatik ölçeklendirme formülü oluşturma

Azure Batch havuzları tanımladığınız parametrelere göre otomatik olarak ölçeklendirebilirsiniz. Otomatik ölçeklendirme, Batch havuzu görev taleplerini artış olarak düğümleri dinamik olarak ekler ve bunlar azaldıkça işlem düğümleri kaldırır. Toplu uygulamanız tarafından kullanılan işlem düğümü sayısını otomatik olarak ayarlayarak, zaman ve para tasarrufu yapabilirsiniz. 

Otomatik bir işlem düğümleri havuzu üzerinde ile ilişkilendirerek ölçeklendirmeyi etkinleştirmek bir *otomatik ölçeklendirme formülü* tanımladığınız. Batch hizmeti, iş yükünü yürütmek için gereken işlem düğümleri sayısını belirlemek için otomatik ölçeklendirme formülü kullanır. İşlem düğümleri, ayrılmış düğümleri olabilir veya [düşük öncelikli düğümleri](batch-low-pri-vms.md). Toplu düzenli aralıklarla toplanan hizmet ölçüm verilerini için yanıt verir. Bu ölçümleri verileri kullanarak, Batch formülünüzü ve yapılandırılabilir aralıklarla dayalı havuzdaki işlem düğümleri sayısını ayarlar.

Bir havuzu oluştururken veya var olan bir havuzu üzerinde otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Varolan bir formülün otomatik ölçeklendirmeyi için yapılandırılan bir havuz üzerinde de değiştirebilirsiniz. Batch havuzları atamadan önce formülleri değerlendirin ve otomatik ölçeklendirme çalıştırır durumunu izlemek için sağlar.

Bu makalede değişkenleri, işleçler, işlemleri ve işlevleri dahil olmak üzere, otomatik ölçeklendirme formüller yapmak çeşitli varlıkları anlatılmaktadır. Yığın içindeki çeşitli işlem kaynak ve görev ölçümleri edinme aşağıdakiler ele. Kaynak kullanımı ve görev duruma dayanarak havuza ait düğüm sayısını ayarlamak için bu ölçümleri kullanabilirsiniz. Biz sonra bir formül oluşturmak ve otomatik bir havuz üzerinde ölçeklendirme Batch REST ve .NET API'lerini kullanarak etkinleştirmek açıklar. Son olarak, biz birkaç örnek formüllerle sonlandırın.

> [!IMPORTANT]
> Bir toplu işlem hesabı oluşturduğunuzda, belirtebilirsiniz [hesabı Yapılandırması](batch-api-basics.md#account), havuzları bir toplu işlem hizmeti aboneliği (varsayılan) ya da kullanıcı aboneliğinizi ayrılan belirler. Varsayılan toplu hizmet yapılandırması, toplu işlem hesabı oluşturduysanız, ardından hesabınız bir en yüksek işleme için kullanılan çekirdek sayısı sınırlıdır. Batch hizmeti işlem düğümleri yalnızca bu çekirdek sınıra kadar ölçeklendirir. Bu nedenle, Batch hizmeti işlem düğümlerini otomatik ölçeklendirme formülü tarafından belirtilen hedef sayısını ulaşabilir değil. Bkz: [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md) görüntüleme ve hesap kotalarını artırma hakkında bilgi.
>
>Kullanıcı aboneliği yapılandırmayla hesabınızı oluşturduysanız, hesabınızın abonelik için çekirdek kota paylaşır. Daha fazla bilgi için [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md) sayfasındaki [Sanal Makine limitleri](../azure-subscription-service-limits.md#virtual-machines-limits) bölümüne bakın.
>
>

## <a name="automatic-scaling-formulas"></a>Otomatik ölçeklendirme formüller
Otomatik ölçeklendirme formülü bir veya daha fazla deyimleri içeren tanımladığınız bir dize değeridir. Otomatik ölçeklendirme formülü bir havuzun atanan [autoScaleFormula] [ rest_autoscaleformula] öğesi (Batch REST) ya da [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] özelliğini (Batch .NET). Batch hizmeti formülünüzü işleme sonraki aralığı havuzdaki işlem düğümleri hedef sayısını belirlemek için kullanır. Formül dizesi 8 KB'yi aşamaz, noktalı virgülle ayrılır ve satır sonları ve açıklamalar içerebilen en fazla 100 deyimleri içerebilir.

Otomatik ölçeklendirme formüllerini bir toplu otomatik ölçeklendirme "language" düşünebilirsiniz Formül deyimleri hem hizmet tanımlı değişkenleri (Batch hizmeti tarafından tanımlanan) hem de kullanıcı tarafından tanımlanan değişkenleri (tanımladığınız) içerebilir serbest biçimli ifadelerini. Yerleşik türler, işleçler ve işlevleri kullanarak bu değerleri üzerinde çeşitli işlemler yapabilirler. Örneğin, bir deyimi aşağıdaki form alabilir:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formülleri genellikle önceki deyimlerinde elde edilen değerleri işlemleri birden çok ifade içerir. Örneğin, biz için bir değer edinip `variable1`, doldurmak için bir işleve geçirme `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Bir işlem düğümlerinin hedef numaradan ulaşması için otomatik ölçeklendirme formülü bu deyimleri ekleyin. Ayrılmış düğümleri ve düşük öncelikli düğümleri kendi hedef ayarları olması, böylece her düğüm türü için bir hedef tanımlayabilirsiniz. Otomatik ölçeklendirme formülü bir hedef değer ayrılmış düğümleri, düşük öncelikli düğümler için bir hedef değer veya her ikisi de dahil edebilirsiniz.

Düğümlerin hedef sayısını daha yüksek alt ya da bu tür havuzdaki düğümlerin sayısı ile aynı. Toplu işlem belirli bir aralıkla bir havuzun otomatik ölçeklendirme formülü değerlendirir (bkz [otomatik aralıklarla ölçeklendirme](#automatic-scaling-interval)). Toplu otomatik ölçeklendirme formülü değerlendirme aynı anda belirten sayı havuzuna düğümünde her tür hedef sayısını ayarlar.

### <a name="sample-autoscale-formula"></a>Örnek otomatik ölçeklendirme formülü

Çoğu senaryo için çalışması için ayarlanmış bir otomatik ölçeklendirme formülü bir örneği burada verilmiştir. Değişkenleri `startingNumberOfVMs` ve `maxNumberofVMs` örnekte formülü gereksinimleriniz için ayarlanabilir. Bu formülü ayrılmış düğümleri ölçeklendirir ancak de ölçek düşük öncelikli düğümlerine uygulamak için değiştirilebilir. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Bu otomatik ölçeklendirme formülü ile havuzu başlangıçta tek bir VM ile oluşturulur. `$PendingTasks` Ölçüm çalışan ya da kuyruğa alınmış görevlerin sayısını tanımlar. Formül görevleri bekleyen ortalama sayısı Son 180 saniye ve ayarlar bulur `$TargetDedicatedNodes` değişkeni buna göre. Formül ayrılmış düğümlerin hedef sayısını hiçbir zaman 25 VM'ler aştığını sağlar. Yeni görevler gönderildiği haliyle havuzu otomatik olarak artar. Görevleri tam olarak boş bir birer birer VM'ler olur ve otomatik ölçeklendirme formülü havuzu küçültür.

## <a name="variables"></a>Değişkenler
Her ikisi de kullanabilirsiniz **hizmet tanımlı** ve **kullanıcı tanımlı** otomatik ölçeklendirme formüller değişkenleri. Hizmet tanımlı değişkenleri Batch hizmeti yerleşiktir. Bazı hizmet tanımlı değişkenler okuma-yazma ve bazı salt okunurdur. Kullanıcı tarafından tanımlanan tanımlarsınız değişkenleri değişkenlerdir. Önceki bölümde gösterilen örnek formülünde `$TargetDedicatedNodes` ve `$PendingTasks` hizmet tanımlı değişkenlerdir. Değişkenleri `startingNumberOfVMs` ve `maxNumberofVMs` kullanıcı tanımlı değişkenlerdir.

> [!NOTE]
> Hizmet tarafından tanımlanan değişkenler her zaman bir dolar işareti ($) öncesinde olur. Kullanıcı tanımlı değişkenleri dolar işareti isteğe bağlıdır.
>
>

Aşağıdaki tablolarda Batch hizmeti tarafından tanımlanan hem okuma-yazma ve salt okunur değişkenler gösterilmektedir.

Almak ve bir havuzdaki işlem düğümleri sayısını yönetmek için bu hizmet tanımlı değişkenlerin değerleri ayarlayın:

| Okuma-yazma hizmet tanımlı değişkenler | Açıklama |
| --- | --- |
| $TargetDedicatedNodes |İşlem düğümleri havuzu için ayrılmış hedef sayısı. Bir havuzu her zaman istenilen düğüm sayısına elde edebilirsiniz değil çünkü ayrılmış düğüm sayısı hedef olarak belirtilir. Havuz ilk hedef ulaştı önce bir otomatik ölçeklendirme değerlendirme tarafından ayrılmış düğümlerin hedef sayısını değiştirilirse, örneğin, sonra havuzu hedef ulaşabilir değil. <br /><br /> Toplu işlem hesabı düğümü veya çekirdek kotası hedef aşarsa, bir havuz Batch hizmeti yapılandırması ile oluşturulan bir hesap hedefine elde. Hedef abonelik için paylaşılan çekirdek kota aşarsa, bir kullanıcı aboneliği yapılandırması ile oluşturulan bir hesap havuzda hedefine elde.|
| $TargetLowPriorityNodes |Düşük öncelikli hedef sayısını işlem düğümleri havuzu için. Düşük öncelikli düğüm sayısını bir havuzu her zaman istenilen düğüm sayısına elde edebilirsiniz değil çünkü hedef olarak belirtilir. Düşük öncelikli düğümlerin hedef sayısını havuzu ilk hedef ulaştı önce bir otomatik ölçeklendirme değerlendirme tarafından değiştirilirse, örneğin, sonra havuzu hedef ulaşabilir değil. Toplu işlem hesabı düğümü veya çekirdek kotası hedef aşarsa, bir havuzu de hedefine elde. <br /><br /> Düşük öncelikli işlem düğümleri hakkında daha fazla bilgi için bkz: [toplu işlemi (Önizleme) ile düşük öncelikli sanal makineleri kullanmak](batch-low-pri-vms.md). |
| $NodeDeallocationOption |İşlem düğümü havuzdan kaldırıldığında oluşan eylem. Olası değerler şunlardır:<ul><li>**requeue**--görevleri hemen sonlandırır ve böylece bunlar yeniden bunları geri işi sıraya koyar.<li>**sonlandırma**--görevleri hemen sonlandırır ve iş kuyruktan kaldırır.<li>**net_offline_option**--çalışmakta olan görevleri tamamlamak için ve ardından düğümü havuzdan kaldırır bekler.<li>**retaineddata**--düğüm havuzdan kaldırılmadan önce temizlenmesi düğümdeki tüm yerel görev korunan veriler için bekler.</ul> |

Batch hizmeti ölçümleri dayalı ayarlamalar yapmak için bu hizmet tanımlı değişkenlerin değerini alabilirsiniz:

| Salt okunur hizmet tanımlı değişkenler | Açıklama |
| --- | --- |
| $CPUPercent |Ortalama CPU kullanımı yüzdesi. |
| $WallClockSeconds |Tüketilen saniye sayısı. |
| $MemoryBytes |Kullanılan megabayt cinsinden ortalama sayısı. |
| $DiskBytes |Yerel diskler üzerinde kullanılan gigabayt ortalama sayısı. |
| $DiskReadBytes |Okunan bayt sayısı. |
| $DiskWriteBytes |Yazılan bayt sayısı. |
| $DiskReadOps |Gerçekleştirilen disk okuma işlemlerinin sayısı. |
| $DiskWriteOps |Gerçekleştirilen yazma disk işlemlerinin sayısı. |
| $NetworkInBytes |Gelen bayt sayısı. |
| $NetworkOutBytes |Giden bayt sayısı. |
| $SampleNodeCount |İşlem düğümleri sayısı. |
| $ActiveTasks |Yürütülmeye hazır olan, ancak değil henüz yürütülen görevler sayısı. $ActiveTasks sayısı, etkin durumda olan ve bağımlılıklarını memnun tüm görevleri içerir. Etkin durumda olan ancak bağımlılıklarını değil uyduğunuzdan herhangi bir görevi $ActiveTasks sayısını hariç tutulur.|
| $RunningTasks |Görevler çalışır durumda sayısı. |
| $PendingTasks |$ActiveTasks ve $RunningTasks toplamı. |
| $SucceededTasks |Başarıyla tamamlandı görevleri sayısı. |
| $FailedTasks |Başarısız görevlerin sayısı. |
| $CurrentDedicatedNodes |İşlem düğümleri ayrılmış geçerli sayısı. |
| $CurrentLowPriorityNodes |Düşük öncelikli geçerli sayısını işlem düğümleri, biterse tüm düğümleri de dahil olmak üzere. |
| $PreemptedNodeCount | Havuzdaki biterse durumda düğüm sayısı. |

> [!TIP]
> Önceki tabloda gösterilen salt okunur, hizmet tarafından tanımlanan değişkenler *nesneleri* her ilişkilendirilmiş verilere erişmek için çeşitli yöntemler sağlar. Daha fazla bilgi için bkz: [örnek verileri elde](#getsampledata) bu makalenin ilerisinde yer.
>
>

## <a name="types"></a>Türler
Bu tür bir formüle desteklenir:

* Çift
* doubleVec
* doubleVecList
* Dize
* zaman damgası--zaman damgası aşağıdaki üyeleri içeren bileşik bir yapıdır:

  * Yıl
  * Ay (1-12)
  * gün (1-31)
  * Haftanın günü (biçimde numarasının; Örneğin, Pazartesi günü için 1)
  * saat (24 saatlik sayı biçiminde; Örneğin, 13 13'te anlamına gelir)
  * dakika (00-59)
  * İkinci (00-59)
* TimeInterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>İşlemler
Bu işlemler önceki bölümde listelenen türlerine izin verilir.

| İşlem | Desteklenen işleçleri | Sonuç türü |
| --- | --- | --- |
| çift *işleci* çift |+, -, *, / |Çift |
| çift *işleci* TimeInterval |* |TimeInterval |
| doubleVec *işleci* çift |+, -, *, / |doubleVec |
| doubleVec *işleci* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *işleci* çift |*, / |TimeInterval |
| TimeInterval *işleci* TimeInterval |+, - |TimeInterval |
| TimeInterval *işleci* zaman damgası |+ |timestamp |
| zaman damgası *işleci* TimeInterval |+ |timestamp |
| zaman damgası *işleci* zaman damgası |- |TimeInterval |
| *İşleç*çift |-, ! |Çift |
| *İşleç*TimeInterval |- |TimeInterval |
| çift *işleci* çift |<, <=, ==, >=, >, != |Çift |
| dize *işleci* dize |<, <=, ==, >=, >, != |Çift |
| zaman damgası *işleci* zaman damgası |<, <=, ==, >=, >, != |Çift |
| TimeInterval *işleci* TimeInterval |<, <=, ==, >=, >, != |Çift |
| çift *işleci* çift |&&, &#124;&#124; |Çift |

Bir çift Üçlü operatör ile sınarken (`double ? statement1 : statement2`), sıfır olmayan olan **true**, ve sıfır **false**.

## <a name="functions"></a>İşlevler
Bu önceden tanımlanmış **işlevleri** otomatik ölçeklendirme formülü tanımlarken kullanmanız için kullanılabilir.

| İşlevi | Dönüş türü | Açıklama |
| --- | --- | --- |
| AVG(doubleVecList) |Çift |Tüm değerler için ortalama değer doubleVecList döndürür. |
| Len(doubleVecList) |Çift |DoubleVecList oluşturulan vektör uzunluğunu döndürür. |
| LG(double) |Çift |Günlük çift temel 2 döndürür. |
| LG(doubleVecList) |doubleVec |Component-wise günlük doubleVecList temel 2 döndürür. Bir vec(double) parametresi için açıkça geçirilmelidir. Aksi takdirde, çift lg(double) sürümü varsayılır. |
| ln(double) |Çift |Çift doğal günlüğü döndürür. |
| ln(doubleVecList) |doubleVec |Component-wise günlük doubleVecList temel 2 döndürür. Bir vec(double) parametresi için açıkça geçirilmelidir. Aksi takdirde, çift lg(double) sürümü varsayılır. |
| log(double) |Çift |Günlük çift temel 10 döndürür. |
| log(doubleVecList) |doubleVec |Component-wise günlük doubleVecList temel 10 döndürür. Bir vec(double) açıkça için tek bir çift parametre geçirilmelidir. Aksi takdirde, çift log(double) sürümü varsayılır. |
| max(doubleVecList) |Çift |En büyük değer doubleVecList döndürür. |
| Min(doubleVecList) |Çift |En düşük değer doubleVecList döndürür. |
| Norm(doubleVecList) |Çift |DoubleVecList oluşturulan vektör norm iki döndürür. |
| Yüzdelik (doubleVec v, çift p) |Çift |V vektör yüzdebirlik öğesi döndürür. |
| rand() |Çift |0,0 ile 1,0 arasında rastgele bir değeri döndürür. |
| Range(doubleVecList) |Çift |En az ve maksimum değerleri arasındaki farkı doubleVecList döndürür. |
| Std(doubleVecList) |Çift |DoubleVecList değerleri örnek standart sapmasını döndürür. |
| Stop() | |Otomatik ölçeklendirmeyi ifadesi değerlendirmesi durdurur. |
| SUM(doubleVecList) |Çift |DoubleVecList tüm bileşenleri toplamını döndürür. |
| saat (dateTime dize = "") |timestamp |Kendisine geçirilen parametre aktarılırsa geçerli zaman zaman damgası veya zaman damgası tarih saat dizesi döndürür. Desteklenen tarih/saat biçimleri W3C DTF ve RFC 1123 olacaktır. |
| VAL (doubleVec v, çift i) |Çift |Konumda i vektör v, sıfır başlangıç dizinine sahip olan öğe değerini döndürür. |

Yukarıdaki tabloda açıklanan işlevleri bazıları listesini bağımsız değişken olarak kabul edebilir. Herhangi bir bileşimini virgülle ayrılmış listesidir *çift* ve *doubleVec*. Örneğin:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList* değeri tek bir dönüştürülür *doubleVec* değerlendirme önce. Örneğin, varsa `v = [1,2,3]`, ardından çağırma `avg(v)` arama için eşdeğer bir gruba `avg(1,2,3)`. Çağırma `avg(v, 7)` arama için eşdeğer bir gruba `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Örnek verileri alma
Otomatik ölçeklendirme formüller Batch hizmeti tarafından sağlanan ölçüm verilerini (örnekler) hareket. Bir formülü büyüyor veya hizmetinden alır değerlere göre havuz boyutu küçültür. Açıklandığı gibi hizmet tanımlı değişkenlerin önceden Bu nesneyle ilişkili verilere erişmek için çeşitli yöntemler sağlayan nesneleridir. Örneğin, aşağıdaki ifade, CPU kullanımı son beş dakika almak için bir istek gösterir:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Yöntem | Açıklama |
| --- | --- |
| GetSample() |`GetSample()` Yöntemi vektör verilerini örnekleri döndürür.<br/><br/>Ölçüm verilerini 30 saniyede bir örnektir. Diğer bir deyişle, örnekleri her 30 saniyede elde edilir. Ancak aşağıda belirtildiği gibi bir formülün kullanılabilir olduğunda ve bir örnek ne zaman toplanan arasında bir gecikme yoktur. Bu nedenle, belirli bir süre için tüm örnekleri bir formüle değerlendirme için kullanılabilir.<ul><li>`doubleVec GetSample(double count)`<br/>Toplanan en son örneklerini almak için örnek sayısını belirtir.<br/><br/>`GetSample(1)`Son kullanılabilir örneği döndürür. Ölçümleri ister için `$CPUPercent`, bilmeniz mümkün olduğundan ancak, bu kullanılmamalıdır *zaman* örnek toplanan. Son olabilir ya da sistem sorunları nedeniyle daha eski olabilir. Aşağıda gösterildiği gibi bir zaman aralığı kullanmak için bu gibi durumlarda daha iyidir.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Örnek verileri toplamak için bir zaman çerçevesi belirtir. İsteğe bağlı olarak, ayrıca istenen zaman dilimi içinde kullanılabilir olmalıdır örnekleri yüzdesini belirtir.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`Son 10 dakika için tüm örnekleri CPUPercent geçmişinde mevcut olup olmadığını 20 örnekleri döndürecektir. Son dakika geçmişi kullanılabilir değilse, ancak yalnızca 18 örnekleri döndürülür. Bu durumda:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`örnekler yüzde 90'ından yalnızca kullanılabilir olmadığından başarısız olur.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`başarılı.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Başlangıç zamanı ve bitiş saati ile veri toplama için bir zaman çerçevesi belirtir.<br/><br/>Yukarıda belirtildiği gibi bir formülün kullanılabilir olduğunda ve bir örnek ne zaman toplanan arasında bir gecikme olur. Bu gecikme kullanırken dikkate `GetSample` yöntemi. Bkz: `GetSamplePercent` aşağıda. |
| GetSamplePeriod() |Gerçekleştirilen örnekleri süresi geçmiş örnek veri kümesinde döndürür. |
| Count() işlevi |Ölçüm geçmişinde örneklerin toplam sayısını döndürür. |
| HistoryBeginTime() |Eski kullanılabilir veri örneği ölçümü için zaman damgasını döndürür. |
| GetSamplePercent() |Belirli bir zaman aralığı için kullanılabilen örnekleri yüzdesini döndürür. Örneğin:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Çünkü `GetSample` yöntem başarısız olursa örnekleri yüzdesi döndürülür küçük `samplePercent` kullanabileceğiniz belirtilen `GetSamplePercent` denetlemek için önce yöntemi. Ardından, yetersiz örnekleri varsa, otomatik ölçeklendirme değerlendirme durdurma olmadan alternatif bir eylem gerçekleştirebilirsiniz. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Örnekleri, örnek yüzdesi ve *GetSample()* yöntemi
Çekirdek otomatik ölçeklendirme formülü görev ve kaynak ölçüm verileri elde etmek ve verilere dayanan havuz boyutunu ayarlamak için bir işlemdir. Bu nedenle, otomatik ölçeklendirme formüller (örnekler) ölçüm verileri ile nasıl etkileşim NET bir anlayış olması önemlidir.

**Örnekler**

Batch hizmeti düzenli aralıklarla görev ve kaynak ölçümleri örneklerini alır ve otomatik ölçeklendirme formüller için kullanılabilir hale getirir. Bu örnekler, her 30 saniyede Batch hizmeti tarafından kaydedilir. Ancak, genellikle bir gecikme olur ve bunlar için kullanılabilir hale getirilir (ve tarafından okunur olduğunda) ne zaman bu örnekleri kaydedilmiş arasında otomatik ölçeklendirme formüller. Ayrıca, ağ ve diğer altyapı sorunlar gibi çeşitli Etkenler nedeniyle örnekleri için belirli bir aralık kaydedilmemiş olabilir.

**Örnek yüzdesi**

Zaman `samplePercent` geçirilir `GetSample()` yöntemi veya `GetSamplePercent()` yöntemi çağrıldığında, _yüzde_ Batch hizmeti tarafından kaydedilen örnek olası toplam sayısı ve otomatik ölçeklendirme formülü kullanılabilir örnek sayısı arasında bir karşılaştırma başvuruyor.

Örneğin 10 dakikalık timespan bakalım. Örnekleri her 30 saniyede 10 dakikalık timespan içinde kayıtlı olduğundan, Batch tarafından kaydedilen örnekleri en fazla toplam sayısı 20 örnekleri (dakika başına 2) olacaktır. Ancak, nedeniyle devralınmış gecikme raporlama mekanizması ve Azure içindeki diğer sorunlar olabilir yalnızca okumak için otomatik ölçeklendirme formülü kullanılabilir 15 örnekleri. Bu nedenle, örneğin, söz konusu 10 dakikalık dönem için kaydedilen örnek toplam sayısı % 75'yalnızca formülü bulunabilir.

**GetSample() ve örnek aralıkları**

Otomatik ölçeklendirme formüller büyüyen ve havuzlarınızı küçültme olacak &mdash; düğüm ekleme veya düğümleri kaldırma. Düğümleri para maliyet olduğundan, formüller yeterli verilerine dayalı analiz akıllı bir yöntem kullanmanızı sağlamak istiyorsunuz. Bu nedenle, eğilim türü çözümlemesi formüllerde kullanmanızı öneririz. Bu tür büyür ve toplanan örnekleri bir aralığı tabanlı havuzlarınızı küçültür.

Bunu yapmak için kullanın `GetSample(interval look-back start, interval look-back end)` örnekleri vektör döndürmek için:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Yukarıdaki satırında toplu işi tarafından değerlendirildiğinde değerlerin vektör olarak örnekleri bir dizi döndürür. Örneğin:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Vektör örnekleri derledik sonra gibi İşlevler ardından kullanabilirsiniz `min()`, `max()`, ve `avg()` toplanan aralıktan anlamlı değerleri çıkarmaya.

Ek güvenlik için belirli bir örnek yüzdeden küçük belirli bir süre için kullanılabilir, başarısız olması için bir formül değerlendirme zorlayabilirsiniz. Bir formülü değerlendirmesi başarısız olmasına zorlar, örnekleri belirtilen yüzdesi kullanılabilir değilse, daha fazla formülü değerlendirmesi sona toplu isteyin. Bu durumda havuz boyutunu değişiklik yapılmaz. Örnekler başarılı olması değerlendirme için gerekli bir yüzdesini belirtmek için üçüncü parametre olarak belirtmeden `GetSample()`. Burada, örnekler yüzde 75'ini gereksinimi belirtilir:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Örnek kullanılabilirlik bir gecikme olabilir çünkü, her zaman bir dakikadan daha eski bir arka plan görünüm başlangıç saatine sahip bir zaman aralığı belirtmek önemlidir. Sistem üzerinden yaymak örnekler için yaklaşık bir dakika sürer, bu nedenle aralığında örnekleri `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` kullanılamayabilir. Yüzde parametresinin tekrar kullanabilirsiniz `GetSample()` belirli örnek yüzdesi gereksinimini zorlamak için.

> [!IMPORTANT]
> Biz **tavsiye** , **bağlı olan kaçının *yalnızca* üzerinde `GetSample(1)` , otomatik ölçeklendirme formüllerde**. Bunun nedeni, `GetSample(1)` "Benim son örnek, verin var., ne kadar zaman önce siz aldıktan olsun" Batch hizmeti için temelde diyor Yalnızca tek bir örnek olduğundan ve eski bir örnek olabilir olduğundan, son görev veya kaynak durumunu daha büyük resmi temsilcisi olmayabilir. Kullanırsanız `GetSample(1)`, daha büyük bir deyim ve formülünüzü güvenen yalnızca veri noktası parçası olduğundan emin olun.
>
>

## <a name="metrics"></a>Ölçümler
Bir formülü tanımlarken, hem kaynak hem de görev ölçümleri kullanabilirsiniz. Edinmek ve değerlendirmek ölçümleri verileri temel alan havuzundaki ayrılmış düğümlerin hedef sayısını ayarlayın. Bkz: [değişkenleri](#variables) bölümünde yukarıda her ölçümü hakkında daha fazla bilgi için.

<table>
  <tr>
    <th>Ölçüm</th>
    <th>Açıklama</th>
  </tr>
  <tr>
    <td><b>Kaynak</b></td>
    <td><p>Kaynak ölçümleri CPU, bant genişliği, işlem düğümleri bellek kullanımı ve düğüm sayısını temel alır.</p>
        <p> Bu hizmet tarafından tanımlanan değişkenler, düğüm sayısına göre ayarlamaları yapmak için yararlıdır:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Bu hizmet tarafından tanımlanan değişkenler, düğüm kaynak kullanımına bağlı ayarlamaları yapmak için yararlıdır:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Görev</b></td>
    <td><p>Görev ölçümleri etkin gibi görevlerin durumunu Beklemede, temel ve tamamlandı. Aşağıdaki hizmet tanımlı değişkenler, görev ölçümleri temel havuz boyutu ayarlamaları yapmak için yararlıdır:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Otomatik ölçeklendirme formülü yazma
Yukarıdaki bileşenlerinin deyimleri oluşturan tarafından bir otomatik ölçeklendirme formülü yapı ardından tam bir formüle bu deyimleri birleştirin. Bu bölümde, bazı gerçek ölçeklendirme kararları gerçekleştirebileceğiniz bir örnek otomatik ölçeklendirme formülü oluşturun.

İlk olarak, şirketinizdeki bizim yeni otomatik ölçeklendirme formülü için gereksinimleri tanımlayın. Formül gerekir:

1. CPU kullanımı yüksekse, adanmış bir işlem düğümleri havuzunda hedef sayısını artırın.
2. CPU kullanımı düşük olduğunda bir havuzdaki adanmış bir işlem düğümleri sayısını azaltın.
3. Her zaman 400 en fazla adanmış düğüm sayısını kısıtlar.

Yüksek CPU kullanımı sırasında düğümlerin sayısını artırmak için bir kullanıcı tanımlı değişken doldurur deyimi tanımlamak (`$totalDedicatedNodes`) adanmış düğümleri, ancak yalnızca geçerli hedef sayısı 110 yüzdesi olan bir değer ile son 10 dakika sırasında minimum ortalama CPU kullanımını yüzde 70 oluştu. Aksi takdirde değer, geçerli ayrılmış düğümleri sayısı için kullanın.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

İçin *azaltmak* aynı düşük CPU kullanımı, bizim formülünde next deyimi sırasında ayrılmış düğüm sayısını ayarlar `$totalDedicatedNodes` geçerli son 60 dakika cinsinden ortalama CPU kullanımı yüzde 20 altında olursa ayrılmış düğümlerin hedef sayısını yüzde 90'ından değişken. Aksi takdirde geçerli değeri kullanın `$totalDedicatedNodes` biz yukarıdaki deyiminde doldurulmuş.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Şimdi en fazla 400 adanmış bir işlem düğümlerine hedef sayısı sınırı:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Tam formülü şöyledir:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>.NET ile birlikte otomatik ölçeklendirme özellikli bir havuz oluşturma

.NET etkin otomatik ölçeklendirmeyi ile bir havuzu oluşturmak için aşağıdaki adımları izleyin:

1. Havuz oluşturma [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Ayarlama [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) özelliğine `true`.
3. Ayarlama [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) otomatik ölçeklendirme formülü özelliğiyle.
4. (İsteğe bağlı) Ayarlama [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) özelliği (varsayılan değer 15 dakika).
5. Havuz yürütme [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) veya [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

Aşağıdaki kod parçacığını .NET otomatik ölçeklendirme özellikli bir havuz oluşturur. Havuzun otomatik ölçeklendirme formülü Pazartesi 5 için ayrılmış düğümlerin hedef sayısını ve her bir haftanın gününü 1 ayarlar. [Otomatik ölçeklendirme aralığı](#automatic-scaling-interval) 30 dakika olarak ayarlanmıştır. Bu ve diğer C# kod parçacıkları bu makalede, `myBatchClient` düzgün başlatılmadı örneğidir [BatchClient] [ net_batchclient] sınıfı.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "small", // single-core, 1.75 GB memory, 225 GB disk
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Otomatik ölçeklendirme özellikli bir havuz oluşturduğunuzda, belirtmeyin _targetDedicatedComputeNodes_ parametresi veya _targetLowPriorityComputeNodes_ çağrısı parametresini **CreatePool**. Bunun yerine, belirtin **AutoScaleEnabled** ve **AutoScaleFormula** havuzu özellikleri. Bu özelliklerin değerlerini her düğüm türünün hedef sayısını belirler. Ayrıca, el ile yeniden boyutlandırma otomatik ölçeklendirme özellikli bir havuz için (örneğin, [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), ilk **devre dışı** otomatik havuz üzerinde ölçeklendirmeyi daha sonra yeniden boyutlandırmak.
>
>

Batch .NET yanı sıra herhangi diğer kullanabilirsiniz [Batch SDK'ları](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [Batch PowerShell cmdlet'leri](batch-powershell-cmdlets-get-started.md)ve [toplu CLI](batch-cli-get-started.md) otomatik ölçeklendirmeyi yapılandırmak için.


### <a name="automatic-scaling-interval"></a>Otomatik ölçeklendirme aralığı
Varsayılan olarak, Batch hizmeti her 15 dakikada bir havuzun boyutu otomatik ölçeklendirme formülü göre ayarlar. Bu aralık aşağıdaki havuzu özellikleri kullanılarak yapılandırılabilir:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API'si)

Minimum aralık beş dakikadır ve en fazla 168 saattir. Bu aralığın dışında kalan bir aralık belirtilirse, Batch hizmeti hatalı istek (400) bir hata döndürür.

> [!NOTE]
> Otomatik ölçeklendirmeyi şu anda yapılan değişiklikler değerinden bir dakika içinde yanıt kullanılmaya yönelik değildir, ancak yerine bir iş yükünü çalıştırmak olarak kademeli olarak havuzunuzun boyutunu ayarlamak için tasarlanmıştır.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Var olan bir havuzu üzerinde otomatik ölçeklendirmeyi etkinleştirme

Her Batch SDK otomatik ölçeklendirmeyi etkinleştirmek için bir yol sağlar. Örneğin:

* [BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (Batch .NET)
* [Otomatik bir havuz üzerinde ölçeklendirmeyi etkinleştirmek] [ rest_enableautoscale] (REST API'si)

Var olan bir havuzu üzerinde otomatik ölçeklendirmeyi etkinleştirdiğinizde, aşağıdaki noktaları göz önünde bulundurun:

* Otomatik ölçeklendirmeyi etkinleştirmek üzere isteği gönderdiğinizde otomatik ölçeklendirmeyi şu anda havuzunda devre dışıysa, isteği gönderdiğinizde, geçerli otomatik ölçeklendirme formülü belirtmeniz gerekir. İsteğe bağlı olarak, otomatik ölçeklendirme deneme aralığı belirtebilirsiniz. Bir aralık belirtmezseniz, varsayılan değer 15 dakika kullanılır.
* Otomatik ölçeklendirme havuzunda etkinse, otomatik ölçeklendirme formülü, bir deneme aralığı ya da her ikisini de belirtebilirsiniz. Bu özelliklerden en az birini belirtmeniz gerekir.

  * Yeni bir otomatik ölçeklendirme değerlendirme aralığı belirtirseniz, varolan değerlendirme zamanlamasını durdurulur ve yeni bir zamanlama başlatılır. Yeni zamanlamanın başlangıç zamanı otomatik ölçeklendirmeyi etkinleştirmek üzere isteği düzenlendiği zamandır.
  * Atlarsanız otomatik ölçeklendirme formülü veya deneme aralığı, Batch hizmeti bu ayarın geçerli değeri kullanmaya devam eder.

> [!NOTE]
> İçin değerler belirtilmişse *targetDedicatedComputeNodes* veya *targetLowPriorityComputeNodes* parametrelerinin **CreatePool** .NET içinde havuzu oluşturulduğunda veya başka bir dilde karşılaştırılabilir parametreleri için otomatik ölçeklendirme formülü değerlendirildiğinde ardından bu değerleri yoksayılır yöntemi.
>
>

Bu C# kod parçacığını kullanan [Batch .NET] [ net_api] var olan bir havuzu üzerinde otomatik ölçeklendirmeyi etkinleştirmek üzere kitaplığı:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Otomatik ölçeklendirme formülü güncelleştir

Var olan bir otomatik ölçeklendirme özellikli havuzu formülü güncelleştirmek için yeni formül'yla yeniden otomatik ölçeklendirmeyi etkinleştirmek üzere işlemini çağırın. Örneğin, otomatik ölçeklendirmeyi zaten etkinleştirilmişse `myexistingpool` aşağıdaki .NET kodu yürütüldüğünde, otomatik ölçeklendirme formülü içeriğiyle değiştirilir `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Güncelleştirme otomatik ölçeklendirme aralığı

Var olan bir otomatik ölçeklendirme özellikli havuzu otomatik ölçeklendirme değerlendirme aralığını güncelleştirmek için yeni aralığı'yla yeniden otomatik ölçeklendirmeyi etkinleştirmek üzere işlemini çağırın. Örneğin, otomatik ölçeklendirme özellikli .NET içinde zaten bir havuz için 60 dakika için otomatik ölçeklendirme değerlendirme aralığını ayarlamak için şunu yazın:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Otomatik ölçeklendirme Formülü Değerlendir

Bir havuza uygulamadan önce bir formül değerlendirebilirsiniz. Bu şekilde, nasıl formülü üretime koymadan önce deyimleri değerlendirmek görmek için formülü test edebilirsiniz.

Otomatik ölçeklendirme formülü değerlendirmek için önce geçerli bir formül havuzuyla üzerinde otomatik ölçeklendirmeyi etkinleştirmeniz gerekir. Formül etkin otomatik ölçeklendirmeyi henüz yok bir havuz üzerinde test etmek için tek satır formülü kullanın `$TargetDedicatedNodes = 0` ilk etkinleştirdiğinizde otomatik ölçeklendirmeyi. Ardından, aşağıdakilerden birini test etmek istediğiniz formülü değerlendirmek için kullanın:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) veya [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Bu Batch .NET yöntemleri değerlendirmek var olan bir havuzu ve otomatik ölçeklendirme formülü içeren bir dize kimliği gerektirir.

* [Otomatik ölçeklendirme Formülü Değerlendir](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    Bu REST API istek URI havuz Kimliğini ve otomatik ölçeklendirme formülü belirtin *autoScaleFormula* istek gövdesi öğesidir. İşlem yanıt formüle ilgili tüm hata bilgilerini içerir.

Bu [Batch .NET] [ net_api] kod parçacığında, biz otomatik ölçeklendirme formülü değerlendirin. Havuz etkin otomatik ölçeklendirmeyi yoksa, biz öncelikle etkinleştirin.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = {pool.CurrentDedicatedNodes};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this code does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Bu kod parçacığında gösterildiği formülü başarılı değerlendirmesi için benzer sonuçlar üretir:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Otomatik ölçeklendirme çalışmaları hakkında bilgi edinin

Formül beklenen şekilde çalıştığını emin olmak için düzenli aralıklarla toplu havuzunuzun üzerinde gerçekleştirir otomatik ölçeklendirmeyi çalıştırır sonuçlarını denetleyin öneririz. Bir başvuru havuzu bu nedenle, alma (veya yenilemek için) ve çalıştırmak, son otomatik ölçeklendirme özelliklerini inceleyin.

Batch .NET içindeki [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) özelliği son otomatik havuzu üzerinde gerçekleştirilen çalışır ölçeklendirme hakkında bilgi sağlayan çeşitli özellikler vardır:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

REST API'sindeki [bir havuzu hakkında bilgi alma](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) istek bilgileri Çalıştır son otomatik ölçeklendirmeyi içerir havuzu hakkındaki bilgileri döndürür [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) özelliği.

Aşağıdaki C# kod parçacığını havuzu üzerinde çalışır son otomatik ölçeklendirmeyi hakkındaki bilgileri yazdırmak için Batch .NET kitaplığını kullanan _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Yukarıdaki kod parçacığında örnek çıktı:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Örnek otomatik ölçeklendirme formüller
Bir havuzdaki işlem kaynakları miktarını ayarlamak için farklı yollar gösterir birkaç formüller bakalım.

### <a name="example-1-time-based-adjustment"></a>Örnek 1: Zamana dayalı ayarlama
Haftanın gününü ve günün saatini göre havuz boyutu ayarlamak istediğinizi varsayalım. Bu örnek artırabilir ya da buna uygun olarak havuzdaki düğümlerin sayısını azaltmak nasıl gösterir.

Formül, ilk geçerli saati alır. Haftanın günü (1-5) ise ve çalışma saatleri (8: 00 için 6 PM) içinde hedef havuz boyutu 20 düğümlerine ayarlanır. Aksi halde, 10 düğümlerine ayarlanır.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Örnek 2: Görev tabanlı ayarlama
Bu örnekte, havuz boyutunu sayıda görev sırasına göre ayarlanır. Hem açıklamaları hem de satır sonu formül dizelerde kabul edilir.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Örnek 3: Paralel Görevler için hesap oluşturma
Bu örnekte görevler sayısına göre havuz boyutu ayarlar. Bu formülü de dikkate alır [MaxTasksPerComputeNode] [ net_maxtasks] havuzu için ayarlanan değeri. Bu yaklaşım durumlarda faydalıdır nerede [paralel görev yürütme](batch-parallel-node-tasks.md) havuzunuzun üzerinde etkin.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Örnek 4: ilk havuz boyutu ayarlama
Bu örnek, bir havuz boyutu, ilk bir süre için belirtilen sayıda düğüme ayarlar bir otomatik ölçeklendirme formülü ile C# kod parçacığını gösterir. Çalışan sayısı bağlı ve etkin havuz boyutu ayarlar daha sonra ilk süre geçtikten sonra görevler.

Aşağıdaki kod parçacığını formülde:

* İlk havuz boyutu dört düğüm ayarlar.
* Havuz boyutu, havuzun yaşam döngüsünün ilk 10 dakika içinde ayarlanmaz.
* 10 dakika sonra sayısı maksimum değerini alır son 60 dakika içinde çalışan ve etkin görevler.
  * Her iki değerler 0 (hiçbir görev çalışan ya da son 60 dakika etkin olduğunu gösteren) ise havuz boyutu 0 olarak ayarlanır.
  * İki değer sıfırdan büyük olması durumunda değişiklik yapılmaz.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Eşzamanlı düğüm görevleri ile Azure toplu işlem kaynak kullanımını en üst düzeye](batch-parallel-node-tasks.md) nasıl birden fazla görev aynı anda havuzunuzdaki işlem düğümlerinde yürütebilirsiniz hakkında ayrıntılar içerir. Otomatik ölçeklendirmeyi ek olarak, bu özellik iş süresi paradan tasarruf bazı iş yükleri için düşük yardımcı olabilir.
* Başka bir verimlilik güçlendirici Batch uygulamanızı en iyi şekilde Batch hizmeti sorgular emin olun. Bkz: [Azure Batch hizmetinin verimli bir şekilde sorgu](batch-efficient-list-queries.md) kablo binlerce işlem düğümleri veya görevlerin durumunu sorguladığınızda yayılan veri miktarını sınırlamak nasıl öğrenin.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
