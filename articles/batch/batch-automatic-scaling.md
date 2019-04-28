---
title: İşlem düğümleri Azure Batch havuzunda otomatik olarak | Microsoft Docs
description: Havuzdaki işlem düğümü sayısını dinamik olarak ayarlamak için bir bulut havuzunda otomatik ölçeklendirmeyi etkinleştirin.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: multiple
ms.date: 06/20/2017
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fdc2cd8f2218d50aa49d6b4eab2800eb6c92d9c9
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118120"
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Batch havuzundaki işlem düğümlerini ölçekleme için bir otomatik ölçeklendirme formülü oluşturma

Azure Batch havuzları, tanımladığınız parametrelere göre otomatik olarak ölçeklendirebilirsiniz. Otomatik ölçeklendirme, Batch havuzu görev taleplerini artış olarak düğümleri dinamik olarak ekler ve bunlar azaldıkça işlem düğümleri kaldırır. Batch uygulamanız tarafından kullanılan işlem düğümü sayısını otomatik olarak ayarlayarak hem zamandan ve paradan tasarruf sağlayabilirsiniz. 

Otomatik bir işlem düğümleri havuzu üzerinde ölçeklendirme ile ilişkilendirerek etkinleştirme bir *otomatik ölçeklendirme formülü* tanımladığınız. Batch hizmeti, otomatik ölçeklendirme formülü, iş yükünü yürütmek için gereken işlem düğümlerinin sayısını belirlemek için kullanır. İşlem düğümleri, adanmış düğümleri olabilir veya [düşük öncelikli düğümler](batch-low-pri-vms.md). Batch, düzenli aralıklarla toplanan hizmet ölçüm verileri için yanıt verir. Bu ölçüm verilerini kullanarak, Batch formülünüzü yapılandırılabilir bir aralıkta temel havuzdaki işlem düğümü sayısını ayarlar.

Bir havuz oluşturulduğunda veya var olan bir havuzu üzerinde otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Ayrıca, otomatik ölçeklendirme için yapılandırılan bir havuz üzerinde var olan bir formül de değiştirebilirsiniz. Batch havuzları atamadan önce formüllerinizin değerlendirilecek ve otomatik ölçeklendirme çalıştırma durumunu izlemek için sağlar.

Bu makalede, değişkenleri, işleçler, işlemler ve işlevler de dahil olmak üzere, otomatik ölçeklendirme formüllerinizin yapmak çeşitli varlıklar ele alınmaktadır. Yığın içindeki çeşitli bilgi işlem kaynak ve görev ölçümlerini edinme ele alır. Kaynak kullanımı ve görev durumu temelinde, havuzdaki düğüm sayısını ayarlamak için bu ölçümleri kullanabilirsiniz. Biz, ardından bir formül oluşturun ve otomatik bir havuzda ölçeklendirme Batch REST ve .NET API'lerini kullanarak etkinleştirme açıklanmaktadır. Son olarak, size birkaç örnek formüllerle sonlandırın.

> [!IMPORTANT]
> Bir Batch hesabı oluşturduğunuzda, belirtebileceğiniz [hesap Yapılandırması](batch-api-basics.md#account), havuzlarının Batch hizmeti bir abonelikte (varsayılan) ya da kullanıcı aboneliğinizdeki ayrılan belirler. Batch hesabınızın varsayılan Batch hizmeti yapılandırmasıyla oluşturulan, hesabınızı işleme için kullanılan çekirdek sayısı sınırlı olur. Batch hizmeti işlem düğümleri yalnızca bu çekirdek sınırı kadar olacak şekilde ölçeklendirir. Bu nedenle, Batch hizmeti bir otomatik ölçeklendirme formülü tarafından belirtilen işlem düğümlerinin hedef sayısıyla ulaşmıyor olabilir. Bkz: [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md) hesabı kotanızı artırmak ve görüntüleme hakkında bilgi.
>
>Hesabınızı kullanıcı aboneliği yapılandırmasıyla oluşturduysanız, hesabınızı aboneliğe ait çekirdek kotası paylaşır. Daha fazla bilgi için [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md) sayfasındaki [Sanal Makine limitleri](../azure-subscription-service-limits.md#virtual-machines-limits) bölümüne bakın.
>
>

## <a name="automatic-scaling-formulas"></a>Otomatik ölçeklendirme formülleri
Bir otomatik ölçeklendirme formülü bir veya daha fazla deyim içeren tanımladığınız bir dize değeridir. Otomatik ölçeklendirme formülü bir havuzun atanır [autoScaleFormula] [ rest_autoscaleformula] öğesi (Batch REST) ya da [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] özelliğini (Batch .NET). Batch hizmeti formülünüzü işleme belirlenen aralık boyunca bir havuzdaki işlem düğümü hedef sayısını belirlemek için kullanır. Formül dize 8 KB'lık aşamaz, noktalı virgül ile ayrılır ve satır sonları ve açıklamalar içerebilen en fazla 100 deyimleri içerebilir.

Otomatik ölçeklendirme formüller bir Batch otomatik ölçeklendirme "dil" düşünebilirsiniz Formül ifadeleri hem hizmet tanımlı değişkenleri (Batch hizmeti tarafından tanımlanan) hem de kullanıcı tanımlı değişkenleri (tanımladığınız) içerebilir serbest biçimli ifadelerdir. Yerleşik türler, işleçler ve işlevlerden kullanarak bu değerleri üzerinde çeşitli işlemler gerçekleştirebilirsiniz. Örneğin, bir deyimi aşağıdaki form alabilir:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formüller genellikle önceki deyimlerinde elde edilen değerleri işlemleri birden çok deyim içerir. Örneğin, biz için bir değer önce almanız `variable1`, ardından doldurmak için bir işleve geçirme `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Bu deyimler bir işlem düğümlerinin hedef numaradan ulaşması için otomatik ölçeklendirme formülü ekleyin. Adanmış düğümler ve düşük öncelikli düğümler kendi hedef ayarlarını olması, böylece her düğüm türü için bir hedef tanımlayabilirsiniz. Bir otomatik ölçeklendirme formülü adanmış düğümler, düşük öncelikli düğümler için bir hedef değer veya her ikisi için bir hedef değer içerebilir.

Hedef düğüm sayısı üst, alt ya da geçerli türü havuzdaki düğüm sayısını ile aynı. Toplu işlem, belirli bir aralıkta bir havuzun otomatik ölçeklendirme formülü değerlendirir (bkz [otomatik ölçeklendirme aralıkları](#automatic-scaling-interval)). Batch, değerlendirme sırasındaki otomatik ölçeklendirme formülü belirten sayı için havuzdaki düğüm türlerinin her biri hedef sayısını ayarlar.

### <a name="sample-autoscale-formula"></a>Örnek otomatik ölçeklendirme formülü

Çoğu senaryo için çalışacak şekilde ayarlanmış bir otomatik ölçeklendirme formülü bir örneği aşağıda verilmiştir. Değişkenleri `startingNumberOfVMs` ve `maxNumberofVMs` örnekte formül gereksinimleriniz için ayarlanabilir. Şu formül olarak ayarlayın, adanmış düğümleri ölçeklendirilir, ancak de ölçek düşük öncelikli düğümler uygulamak için değiştirilebilir. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Bu otomatik ölçeklendirme formülü ile havuzu başlangıçta ile tek bir VM oluşturulur. `$PendingTasks` Ölçüm çalışan veya kuyruğa alınmış görevlerin sayısını tanımlar. Son 180 saniye ve kümeleri formülü Bekleyen Görevler ortalama sayısını bulur `$TargetDedicatedNodes` değişkeni buna göre. Hedef adanmış düğüm sayısı 25 sanal makineleri hiçbir zaman aşıyor formülü sağlar. Yeni görevler gönderilen gibi havuzun otomatik olarak büyür. Görevler tamamlandı olarak ücretsiz tek tek VM'ler olur ve otomatik ölçeklendirme formülü havuzu küçültür.

## <a name="variables"></a>Değişkenler
Her ikisini birden kullanabilirsiniz **hizmet tarafından tanımlanan** ve **kullanıcı tanımlı** otomatik ölçeklendirme formüllerinizde değişkenleri. Hizmet tarafından tanımlanan değişkenleri Batch hizmeti için yerleşik olarak bulunmaktadır. Bazı hizmet tarafından tanımlanan değişkenler okuma-yazma ve bazı salt okunurdur. Kullanıcı tanımlı değişkenler, tanımladığınız değişkenlerdir. Önceki bölümde gösterilen örnek formülde `$TargetDedicatedNodes` ve `$PendingTasks` hizmet tarafından tanımlanan değişkenler. Değişkenleri `startingNumberOfVMs` ve `maxNumberofVMs` kullanıcı tanımlı değişkenler.

> [!NOTE]
> Hizmet tarafından tanımlanan değişkenleri her zaman bir dolar işareti ($) tarafından öncesinde olur. Kullanıcı tanımlı değişkenler için dolar işareti isteğe bağlıdır.
>
>

Aşağıdaki tablolar, Batch hizmeti tarafından tanımlanan hem okuma-yazma ve salt okunur değişkenler gösterir.

Alın ve bir havuzdaki işlem düğümü sayısını yönetmek için bu hizmet tarafından tanımlanan değişkenler değerlerini ayarlayın:

| Okuma-yazma hizmet tarafından tanımlanan değişkenler | Açıklama |
| --- | --- |
| $TargetDedicatedNodes |Hedef sayısı adanmış işlem düğümleri havuzu için. Bir havuz her zaman istenilen düğüm sayısına elde edebilirsiniz değil çünkü ayrılmış düğüm sayısını hedef olarak belirtilir. İlk hedef havuz ulaştı önce adanmış düğümlerin hedef sayısı bir otomatik ölçeklendirme değerlendirmesi tarafından değiştirilirse, örneğin, sonra havuzu hedef ulaşmıyor olabilir. <br /><br /> Hedef bir Batch hesabı düğümü veya çekirdek kotasını aşarsa bir havuz Batch hizmeti yapılandırmasıyla oluşturulan bir hesapta hedefine elde değil. Hedef abonelik için paylaşılan çekirdek kotasını aşarsa, bir hesap kullanıcı aboneliği yapılandırmasıyla oluşturulan bir havuzda hedefine elde değil.|
| $TargetLowPriorityNodes |Hedef sayısı düşük öncelikli işlem düğümleri havuzu için. Düşük öncelikli düğümlerin sayısını, bir havuzu her zaman istenilen düğüm sayısına elde edebilirsiniz değil çünkü hedef olarak belirtilir. İlk hedef havuz ulaştı önce düşük öncelikli düğümlerin hedef sayısı bir otomatik ölçeklendirme değerlendirmesi tarafından değiştirilirse, örneğin, sonra havuzu hedef ulaşmıyor olabilir. Hedef bir Batch hesabı düğümü veya çekirdek kotasını aşarsa bir havuzu de hedefine elde edebilirsiniz değil. <br /><br /> Düşük öncelikli işlem düğümleri hakkında daha fazla bilgi için bkz. [(Önizleme) Batch ile düşük öncelikli VM'ler kullanma](batch-low-pri-vms.md). |
| $NodeDeallocationOption |İşlem düğümü havuzdan kaldırıldığında, gerçekleşen eylemi. Olası değerler şunlardır:<ul><li>**yeniden kuyruğa alma**--görevler hemen sonlanır ve böylece bunlar yeniden bunları geri iş sıraya koyar.<li>**sonlandırma**--görevler hemen sonlanır ve bunları iş kuyruktan kaldırır.<li>**net_offline_option**--görevlerini tamamlamak için şu anda çalışan ve ardından düğümü havuzdan kaldırır bekler.<li>**retaineddata**--düğüm havuzdan kaldırılmadan önce temizlenmesi düğüm üzerindeki tüm yerel görev korunan veriler için bekler.</ul> |

Batch hizmetinden alınan ölçümleri temel alan ayarlamalar yapmak için bu hizmet tarafından tanımlanan değişkenler değerini alabilirsiniz:

| Salt okunur hizmet tarafından tanımlanan değişkenler | Açıklama |
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
| $ActiveTasks |Yürütülmeye hazır olan ancak değil henüz yürütülen görevleri sayısı. $ActiveTasks sayısı, etkin durumda olan ve bağımlılıklarını tatminkar tüm görevleri içerir. Etkin durumda olan ancak bağımlılıklarını tatminkar değil herhangi bir görevi $ActiveTasks sayısının dışında tutulur.|
| $RunningTasks |Çalışır durumda görev sayısı. |
| $PendingTasks |$ActiveTasks ve $RunningTasks toplamı. |
| $SucceededTasks |Başarıyla tamamlanmış görev sayısı. |
| $FailedTasks |Başarısız görev sayısı. |
| $CurrentDedicatedNodes |İşlem düğümleri olarak adanmış geçerli sayısı. |
| $CurrentLowPriorityNodes |Geçerli sayısını düşük öncelikli işlem düğümleri, ön alım tüm düğümler dahil olmak üzere. |
| $PreemptedNodeCount | Biterse bir durumda olan havuzdaki düğümlerin sayısı. |

> [!TIP]
> Önceki tabloda gösterilen salt okunur, hizmet tarafından tanımlanan değişkenler *nesneleri* her ilişkilendirilmiş verilere erişmek için çeşitli yöntemler sağlar. Daha fazla bilgi için [örnek verileri elde](#getsampledata) bu makalenin ilerleyen bölümlerinde.
>
>

## <a name="types"></a>Türler
Bu tür bir formülde desteklenir:

* double
* doubleVec
* doubleVecList
* string
* zaman damgası--zaman damgası aşağıdaki üyeleri içeren bileşik bir yapıdır:

  * yıl
  * Ay (1-12)
  * gün (1-31)
  * Haftanın günü (biçimde sayının; Örneğin, Pazartesi 1)
  * saat (24 saat biçiminde; Örneğin, 13 13'te anlamına gelir)
  * dakika (00-59)
  * saniye (00-59)
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
Bu işlemler, önceki bölümde listelenen türlerinde izin verilir.

| İşlem | Desteklenen işleçleri | Sonuç türü |
| --- | --- | --- |
| çift *işleci* çift |+, -, *, / |double |
| çift *işleci* TimeInterval |* |TimeInterval |
| doubleVec *işleci* çift |+, -, *, / |doubleVec |
| doubleVec *işleci* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *işleci* çift |*, / |TimeInterval |
| TimeInterval *işleci* TimeInterval |+, - |TimeInterval |
| TimeInterval *işleci* zaman damgası |+ |timestamp |
| zaman damgası *işleci* TimeInterval |+ |timestamp |
| zaman damgası *işleci* zaman damgası |- |TimeInterval |
| *İşleç*çift |-, ! |double |
| *İşleç*TimeInterval |- |TimeInterval |
| çift *işleci* çift |<, <=, ==, >=, >, != |double |
| dize *işleci* dize |<, <=, ==, >=, >, != |double |
| zaman damgası *işleci* zaman damgası |<, <=, ==, >=, >, != |double |
| TimeInterval *işleci* TimeInterval |<, <=, ==, >=, >, != |double |
| çift *işleci* çift |&&, &#124;&#124; |double |

Bir çift Üçlü işleci ile sınarken (`double ? statement1 : statement2`), sıfır dışında olan **true**, ve sıfır **false**.

## <a name="functions"></a>İşlevler
Bu önceden tanımlanmış **işlevleri** bir otomatik ölçeklendirme formülü tanımlarken kullanmanız için kullanılabilir.

| İşlev | Dönüş türü | Açıklama |
| --- | --- | --- |
| AVG(doubleVecList) |double |Tüm değerler için ortalama değeri içinde doubleVecList döndürür. |
| Len(doubleVecList) |double |DoubleVecList oluşturulmuş vektör uzunluğunu döndürür. |
| LG(double) |double |Günlük çift taban 2 döndürür. |
| LG(doubleVecList) |doubleVec |Değişkenlerin bileşen odaklı günlük doubleVecList taban 2 döndürür. Bir vec(double) açıkça parametresi için geçirilen gerekir. Aksi takdirde, çift lg(double) sürüm olduğu varsayılır. |
| ln(double) |double |Double'nın doğal logaritmayı döndürür. |
| ln(doubleVecList) |doubleVec |Değişkenlerin bileşen odaklı günlük doubleVecList taban 2 döndürür. Bir vec(double) açıkça parametresi için geçirilen gerekir. Aksi takdirde, çift lg(double) sürüm olduğu varsayılır. |
| log(double) |double |Günlük taban 10'a çift döndürür. |
| log(doubleVecList) |doubleVec |Değişkenlerin bileşen odaklı günlük doubleVecList taban 10 döndürür. Bir vec(double) açıkça tek bir çift parametresi için geçirilen gerekir. Aksi takdirde, çift log(double) sürüm olduğu varsayılır. |
| max(doubleVecList) |double |İçinde doubleVecList en büyük değeri döndürür. |
| Min(doubleVecList) |double |İçinde doubleVecList en küçük değeri döndürür. |
| Norm(doubleVecList) |double |DoubleVecList oluşturulur vektör iki norm döndürür. |
| yüzdebirlik (doubleVec v, çift p) |double |Vektör v yüzdebirlik öğeyi döndürür. |
| rand() |double |0,0 ile 1,0 arasında rastgele bir değer döndürür. |
| Range(doubleVecList) |double |DoubleVecList içinde en az ve maksimum değerleri arasındaki farkı döndürür. |
| std(doubleVecList) |double |İçinde doubleVecList örnek değerlerin standart sapmasını döndürür. |
| stop() | |Otomatik ölçeklendirme ifadesi değerlendirmesi durdurur. |
| SUM(doubleVecList) |double |DoubleVecList bileşenlerinin tümünü toplamını döndürür. |
| zaman (TarihSaat string = "") |timestamp |Geçirilirse, bu parametre geçirilmezse geçerli zamandan itibaren zaman damgası veya tarih saat dizesi zaman damgasını döndürür. Desteklenen tarih/saat biçimleri W3C DTF ve RFC 1123 olacaktır. |
| VAL (doubleVec v, çift i) |double |Konumda i vektör v, başlangıç dizini sıfır olan öğenin değerini döndürür. |

Önceki tabloda açıklanan işlevler bazılarını bir liste bağımsız değişken olarak kabul edebilir. Herhangi bir birleşimini virgülle ayrılmış listesidir *çift* ve *doubleVec*. Örneğin:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList* tek bir değere dönüştürülür *doubleVec* değerlendirme öncesi. Örneğin, varsa `v = [1,2,3]`, ardından arama `avg(v)` çağırmakla eşdeğerdir `avg(1,2,3)`. Çağırma `avg(v, 7)` çağırmakla eşdeğerdir `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Örnek verileri alır
Formülleri otomatik ölçeklendirme ölçüm verilerini (örnekler) Batch hizmeti tarafından sağlanan gerçekleştir. Bir formül büyüyor veya hizmetten alır değerlere göre havuz boyutunu küçültür. Açıklandığı gibi hizmet tanımlı değişkenler, daha önce bu nesneyle ilişkili verilere erişmek için çeşitli yöntemler sağlayan nesneleri olan. Örneğin, aşağıdaki ifade, CPU kullanımı son beş dakika alma isteği gösterilmektedir:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Yöntem | Açıklama |
| --- | --- |
| GetSample() |`GetSample()` Yöntemi veri örnekleri vektörünü döndürür.<br/><br/>Ölçüm verileri 30 saniye değerinde bir örnektir. Diğer bir deyişle, örnekler, her 30 saniyede elde edilir. Ancak, aşağıda belirtildiği gibi bir örnek ne zaman kullanılacağını ve bir formül olarak kullanılabilir olduğunda arasında bir gecikme yoktur. Bu nedenle, belirli bir süre için tüm örnekleri bir formül olarak değerlendirme için kullanılamıyor olabilir.<ul><li>`doubleVec GetSample(double count)`<br/>Toplanan en son örneklerini almak için örnek sayısını belirtir.<br/><br/>`GetSample(1)` kullanılabilir son örnek döndürür. Ölçümleri ister için `$CPUPercent`, bilmek olanaksızdır olduğundan ancak bu kullanılmamalıdır *olduğunda* örnek toplanmadı. Son olabilir ya da sistem sorunları nedeniyle daha eski olabilir. Böyle durumlarda, aşağıda gösterildiği gibi bir zaman aralığı kullanmak daha iyidir.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Örnek verileri toplamak için bir zaman çerçevesi belirtir. İsteğe bağlı olarak, istenen zaman dilimi içinde kullanılabilir örneklerin yüzdesi de belirtir.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)` Son 10 dakika için tüm örnekleri CPUPercent geçmişi mevcut olması durumunda 20 örnekleri getirir. Geçmiş, geçen dakikada kullanılabilir değilse, ancak yalnızca 18 örnekleri döndürülür. Bu durumda:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)` örnekler yüzde 90'ını yalnızca kullanılabilir olmadığından başarısız olur.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)` başarılı olabilir.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Başlangıç ve bitiş zamanını hem verileri toplamak için bir zaman çerçevesi belirtir.<br/><br/>Yukarıda belirtildiği gibi bir örnek ne zaman kullanılacağını ve bir formül olarak kullanılabilir olduğunda arasında bir gecikme olur. Kullandığınızda bu gecikmeyi dikkate `GetSample` yöntemi. Bkz: `GetSamplePercent` aşağıda. |
| GetSamplePeriod() |Alınan örnekleri dönemi geçmiş örnek veri kümesinde döndürür. |
| Count() |Örneklerin toplam sayısı, ölçüm geçmişi döndürür. |
| HistoryBeginTime() |Ölçüm için eski mevcut veriler örnek zaman damgasını döndürür. |
| GetSamplePercent() |Belirtilen zaman aralığı için kullanılabilir örneklerin yüzdesi döndürür. Örneğin:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Çünkü `GetSample` örneklerin yüzdesi döndürülür yöntemi bozulursa küçüktür `samplePercent` kullanabileceğiniz belirtilen `GetSamplePercent` denetlemek için önce yöntemi. Yetersiz örnekleri varsa, otomatik ölçeklendirme değerlendirme durdurma olmadan, alternatif bir eylem gerçekleştirebilirsiniz. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Örnekleri, örnek yüzdesi ve *GetSample()* yöntemi
Görev ve kaynak ölçüm verileri almak ve ardından bu verileri temel alan havuz boyutunu ayarlamak için bir otomatik ölçeklendirme formülü çekirdek işlemi var. Bu şekilde, formülleri otomatik ölçeklendirme ölçüm verilerini (örnekler) ile etkileşimini, açık bir bilgiye sahip olmanız önemlidir.

**Örnekler**

Batch hizmeti düzenli aralıklarla görev ve kaynak ölçümleri örneklerini alır ve otomatik ölçeklendirme formüllerinizi için kullanılabilir hale getirir. Bu örnekler, her 30 saniyede Batch hizmeti tarafından kaydedilir. Ancak, genellikle bir gecikme olur ve bunlar için kullanılabilir hale getirilir (ve tarafından okunabilir olduğunda) Bu örnekleri ne zaman kaydedilmiş arasında otomatik ölçeklendirme formüllerinizi. Ayrıca, ağ veya diğer altyapı sorunları gibi çeşitli Etkenler nedeniyle örnekleri için belirli bir aralık kaydedilmemiş olabilir.

**Örnek yüzdesi**

Zaman `samplePercent` geçirilir `GetSample()` yöntemi veya `GetSamplePercent()` yöntemi çağrıldığında _yüzde_ Batch hizmeti tarafından kaydedilen örneklerin toplam olası sayısı ve sayısı arasında bir karşılaştırma başvurur otomatik ölçeklendirme formülü kullanılabilir örnekler.

Örneğin 10 dakikalık timespan bakalım. Her 30 saniyede 10 dakikalık bir zaman aralığı içinde örnekleri kaydedildiğinden, Batch tarafından kaydedilen örneklerin toplam sayısı en fazla 20 örnekleri (2 dakika başına) olacaktır. Ancak, doğal gecikme süresini raporlama mekanizmasını ve azure'daki diğer sorunları nedeniyle, olabilir yalnızca 15 örnekler otomatik ölçeklendirme formülü okumak için kullanılabilir. Bu nedenle, örneğin, 10 dakikalık süre için yalnızca kaydedilen örneklerin toplam sayısı %75 Formülünüze bulunabilir.

**GetSample() ve örnek aralıkları**

Otomatik ölçeklendirme formüllerinize büyütme ve küçültme havuzlarınızı olacak &mdash; düğüm ekleme veya düğümleri kaldırma. Düğümleri para maliyeti olduğundan, formüllerinizin yeterli verilere dayalı bir analiz akıllı bir yöntem kullanmanızı sağlamak istiyorsunuz. Bu nedenle, bir eğilim türü analizi formüllerinize kullanmanızı öneririz. Bu tür büyür ve havuzlarınızı toplanan örnekler çeşitli küçültür.

Bunu yapmak için `GetSample(interval look-back start, interval look-back end)` örnekleri oluşan bir vektörü döndürmek için:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Yukarıdaki satırı Batch tarafından değerlendirilirken, bir vektör değerleri örnekleri bir dizi döndürür. Örneğin:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Daha sonra örnekleri vektör derledik sonra gibi işlevleri kullanabilirsiniz `min()`, `max()`, ve `avg()` toplanan aralıktan anlamlı değerlere türetmek için.

Ek güvenlik için belirli bir örnek yüzdenin belirli bir süre için kullanılabilir olduğunda da başarısız bir formül değerlendirmesinin zorlayabilirsiniz. Bir formül değerlendirme başarısız olmasına zorlar, daha fazla belirtilen örneklerin yüzdesi, kullanılabilir değilse, formülün değerlendirme sona ermesi için Batch'i isteyin. Bu durumda, değişiklik havuz boyutunu yapılmaz. Örnekler başarılı olması değerlendirme için gerekli bir yüzdesini belirtmek için üçüncü parametre olarak belirtmeden `GetSample()`. Burada, bir gereksinim örnekleri yüzde 75 belirtilir:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Örnek kullanılabilirlik bir gecikme olabilir çünkü, her zaman bir dakikadan daha eski bir görünüm sonradan başlangıç saatine sahip bir zaman aralığı belirtmek önemlidir. Bu örnekler sistem üzerinden yayılması için yaklaşık bir dakika sürer, bu nedenle aralığında örnekleri `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` kullanılamayabilir. Yüzde parametresi tekrar kullanabilirsiniz `GetSample()` belirli örnek yüzdesi gereksinimini zorlamak için.

> [!IMPORTANT]
> Biz **önemle tavsiye** aldığınız **Güvenmekten kaçının *yalnızca* üzerinde `GetSample(1)` otomatik ölçeklendirme formüllerinizde**. Bunun nedeni, `GetSample(1)` "Benim son örnek, verin var., ne kadar zaman önce onu aldığınız ne olursa olsun" Batch hizmetine temelde söylüyor Yalnızca tek bir örnek olduğunu ve eski bir örnek olabilir olduğundan, daha büyük resmi son görev ya da kaynak durumu temsilcisi olmayabilir. Kullanıyorsanız `GetSample(1)`, daha büyük bir deyim ve formülünüzü dayanan tek veri noktası parçası olduğundan emin olun.
>
>

## <a name="metrics"></a>Ölçümler
Bir formül tanımlarken, hem kaynak hem de görev ölçümleri kullanabilirsiniz. Adanmış düğümleri almak ve değerlendirme ölçüm verileri temel alan havuzdaki hedef sayısını ayarlayın. Bkz: [değişkenleri](#variables) bölümünde yukarıda her ölçümü hakkında daha fazla bilgi için.

<table>
  <tr>
    <th>Ölçüm</th>
    <th>Açıklama</th>
  </tr>
  <tr>
    <td><b>Kaynak</b></td>
    <td><p>Kaynak ölçümleri CPU, bant genişliği, işlem düğümlerinin bellek kullanımı ve düğüm sayısını temel alır.</p>
        <p> Bu hizmet tarafından tanımlanan değişkenler, düğüm sayısına göre ayarlamaları yapmak için kullanışlıdır:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Bu hizmet tarafından tanımlanan değişkenler düğümü kaynak kullanımını temel alarak ayarlamaları yapmak için kullanışlıdır:</p>
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
    <td><p>Görev ölçümleri bekleyen, etkin gibi görev durumlarını temel ve tamamlanır. Aşağıdaki hizmet tarafından tanımlanan değişkenleri, görev ölçümlere göre havuz boyutunu ayarlamaları yapmak için kullanışlıdır:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Bir otomatik ölçeklendirme formülü yazma
Yukarıdaki bileşenleri kullanan deyimleri oluşturan tarafından bir otomatik ölçeklendirme formülü oluşturun ve ardından bu ifadeleri tam bir formüle birleştirin. Bu bölümde, bazı gerçek ölçeklendirme kararları gerçekleştirebileceği bir örnek otomatik ölçeklendirme formülü oluştururuz.

İlk olarak, yeni otomatik ölçeklendirme formülümüz gereksinimlerini tanımlayalım. Formül gerekir:

1. CPU kullanımı yüksekse, adanmış işlem düğümleri havuzunda hedef sayısını artırın.
2. CPU kullanım düşük olduğunda adanmış işlem düğümleri havuzunda hedef sayısını azaltın.
3. Her zaman 400'e en fazla ayrılmış düğüm sayısını kısıtlar.

Yüksek CPU kullanımı sırasında düğüm sayısını artırmak için bir kullanıcı tanımlı değişken dolduran deyim tanımlayın (`$totalDedicatedNodes`) en düşük ortalama CPU kullanımı sırasında 110 yüzde geçerli hedef adanmış düğümler, ancak yalnızca bir değer ile Son 10 dakikasını yüzde 70'idi. Aksi takdirde, değeri geçerli adanmış düğümler sayısı için kullanın.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

İçin *azaltmak* aynı düşük CPU kullanımı, sonraki deyime formülümüz sırasında ayrılmış düğüm sayısını ayarlar `$totalDedicatedNodes` geçerli yüzde 90'değişkenini hedef adanmış düğümler sayısı, ortalama CPU kullanımı geçmişte 60 altında yüzde 20 dakika oluştu. Aksi takdirde geçerli değerini kullanın `$totalDedicatedNodes` , biz Yukarıdaki ifadede doldurulur.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Artık adanmış işlem düğümleri için 400 en fazla hedef sayısını sınırla:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Formülü tamamlama şu şekildedir:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>.NET ile otomatik ölçeklendirme özellikli bir havuz oluşturma

Otomatik ölçeklendirme etkin. NET'te bir havuz oluşturmak için aşağıdaki adımları izleyin:

1. Havuz oluşturma [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Ayarlama [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) özelliğini `true`.
3. Ayarlama [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) otomatik ölçeklendirme formülü ile özelliği.
4. (İsteğe bağlı) Ayarlama [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) özelliği (varsayılan değer 15 dakika).
5. İşleme havuzu [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) veya [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

Aşağıdaki kod parçacığı,. NET'te otomatik ölçeklendirme özellikli bir havuz oluşturur. Havuzun otomatik ölçeklendirme formülü Pazartesi günleri 5 ayrılmış düğümlerin hedef sayısı ve her bir haftanın gününü 1 ayarlar. [Otomatik ölçeklendirme aralığı](#automatic-scaling-interval) 30 dakika olarak ayarlanmıştır. Bu ve diğer C# kod parçacıkları, bu makalede `myBatchClient` düzgün başlatılmadı örneğidir [BatchClient] [ net_batchclient] sınıfı.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "standard_d1_v2",
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Otomatik ölçeklendirme özellikli bir havuz oluşturduğunuzda, belirtmeyin _targetDedicatedNodes_ parametresi veya _targetLowPriorityNodes_ çağrı parametresi **CreatePool** . Bunun yerine, belirtirsiniz **AutoScaleEnabled** ve **AutoScaleFormula** havuzu özellikleri. Bu özelliklerin değerlerini, her düğüm türü hedef sayısını belirlemek. Ayrıca, el ile yeniden boyutlandırma otomatik ölçeklendirme özellikli bir havuz için (örneğin, [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), ilk **devre dışı** üzerinde otomatik ölçeklendirme Havuz ve yeniden boyutlandırın.
>
>

Batch .NET ek olarak başka birini kullanabilirsiniz [Batch SDK'ları](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [Batch PowerShell cmdlet'leri](batch-powershell-cmdlets-get-started.md)ve [Batch CLI](batch-cli-get-started.md) için otomatik ölçeklendirmeyi yapılandırma.


### <a name="automatic-scaling-interval"></a>Otomatik ölçeklendirme aralığı
Varsayılan olarak, Batch hizmeti, 15 dakikada bir otomatik ölçeklendirme formülü göre bir havuzun boyutu ayarlar. Aşağıdaki havuzu özelliklerini kullanarak bu aralığı yapılandırılabilir:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API'si)

En düşük aralık beş dakikadır ve en fazla 168 saattir. Bu aralığın dışında kalan bir aralık belirtilirse, Batch hizmeti hatalı (400) istek bir hata döndürür.

> [!NOTE]
> Otomatik ölçeklendirme şu anda bir dakika içinde değişikliklerine yanıt verme değildir, ancak yerine bir iş yükü çalıştırma olarak kademeli olarak havuzunuz boyutunu ayarlamak için tasarlanmıştır.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Mevcut havuzlardan üzerinde otomatik ölçeklendirmeyi etkinleştir

Her Batch SDK'sını otomatik ölçeklendirmeyi etkinleştirmek için bir yol sağlar. Örneğin:

* [BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (Batch .NET)
* [Bir havuzda otomatik ölçeklendirmeyi etkinleştirmek] [ rest_enableautoscale] (REST API'si)

Mevcut havuzlardan üzerinde otomatik ölçeklendirmeyi etkinleştirdiğinizde, aşağıdaki noktaları göz önünde bulundurun:

* Otomatik ölçeklendirmeyi etkinleştirme isteği gönderdiğinizde otomatik ölçeklendirme şu anda havuzda devre dışı olduğunda, isteği gönderdiğinizde, geçerli bir otomatik ölçeklendirme formülü belirtmeniz gerekir. İsteğe bağlı olarak bir Otomatik ölçek değerlendirme aralığı belirtebilirsiniz. Varsayılan değer 15 dakikalık bir aralığı belirtmezseniz kullanılır.
* Otomatik ölçeklendirme havuzda etkinse, bir otomatik ölçeklendirme formülü, bir deneme aralığı veya her ikisi de belirtebilirsiniz. Bu özelliklerden en az birini belirtmeniz gerekir.

  * Yeni bir Otomatik ölçek değerlendirme aralığı belirtirseniz, var olan değerlendirme zamanlamasını durduruldu ve yeni bir zamanlama başlatılır. Yeni zamanlamanın başlangıç zamanı, otomatik ölçeklendirmeyi etkinleştirme isteği düzenlendiği zamandır.
  * Otomatik ölçeklendirme formülü veya deneme aralığı, Batch hizmeti atlarsanız, bu ayarın geçerli değerini kullanmaya devam eder.

> [!NOTE]
> Değerleri belirttiyseniz *targetDedicatedNodes* veya *targetLowPriorityNodes* parametrelerinin **CreatePool** ,. NET'te veya için havuz oluştururken yöntemi otomatik ölçeklendirme formülü değerlendirildiğinde, başka bir dil ve ardından bu değerleri karşılaştırılabilir parametreleri göz ardı edilir.
>
>

Bu C# kod parçacığını kullanır [Batch .NET] [ net_api] var olan bir havuzu üzerinde otomatik ölçeklendirmeyi etkinleştirmek üzere kitaplığı:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Güncelleştirme bir otomatik ölçeklendirme formülü

Otomatik ölçeklendirme özellikli mevcut havuzlardan formülü güncelleştirmek için yeni formülle otomatik ölçeklendirmeyi etkinleştirmek için işlemi çağırın. Örneğin, otomatik ölçeklendirme zaten etkin `myexistingpool` aşağıdaki .NET kod yürütüldüğünde, otomatik ölçeklendirme formülü içeriğiyle değiştirilir `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Otomatik ölçek aralığı güncelleştir

Otomatik ölçeklendirme özellikli var olan bir havuzu otomatik ölçek değerlendirme aralığı güncelleştirmek için otomatik ölçeklendirme yeni aralığı ile yeniden etkinleştirmek için işlemi çağırın. Örneğin,. NET'te otomatik ölçeklendirme etkin olan bir havuzu için 60 dakika otomatik ölçek değerlendirme aralığı ayarlamak için şunu yazın:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Bir otomatik ölçeklendirme formülü değerlendirmek

Bir havuza uygulamadan önce bir formül değerlendirebilirsiniz. Bu şekilde, formülü nasıl formülü üretime koymadan önce ifadeleri değerlendirme görmek için test edebilirsiniz.

Otomatik ölçeklendirme formülü değerlendirmek için önce geçerli bir formül ile havuz üzerinde otomatik ölçeklendirmeyi etkinleştirmeniz gerekir. Henüz otomatik ölçeklendirme etkin olmayan bir havuz üzerinde bir formül test etmek için satır içi bir formül kullanın `$TargetDedicatedNodes = 0` ilk kez etkinleştirdiğinizde otomatik ölçeklendirme. Ardından, aşağıdakilerden birini test etmek istediğiniz formülü değerlendirmek için kullanın:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) veya [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Bu Batch .NET yöntemleri değerlendirilecek kimliği var olan bir havuzu ve otomatik ölçeklendirme formülü içeren bir dize gerektirir.

* [Bir otomatik ölçeklendirme formülü değerlendirmek](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    Bu REST API istek urı'sindeki Havuz kimliği ve otomatik ölçeklendirme formülü belirtin *autoScaleFormula* istek gövdesi öğesidir. İşlemin yanıtı formüle ilgili hata bilgilerini içerir.

Bu [Batch .NET] [ net_api] kod parçacığı, biz bir otomatik ölçeklendirme formülü değerlendirmek. Havuzun otomatik ölçeklendirme etkin olmaması durumunda, ilk etkinleştiririz.

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
        autoscaleFormula: "$TargetDedicatedNodes = $CurrentDedicatedNodes;",
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

Bu kod parçacığında gösterilen formülün başarılı değerlendirme için benzer sonuçlar üretir:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Otomatik ölçeklendirme çalıştırma hakkında bilgi edinin

Formülünüzün beklendiği gibi çalıştığından emin olmak için Batch, havuzu üzerinde gerçekleştirir otomatik ölçeklendirme çalıştırmalarının sonuçlarını düzenli olarak kontrol etmeniz önerilir. Havuz başvurusu bu nedenle, Al (veya yenilemek için) ve çalıştırma, son otomatik ölçeklendirme özelliklerini inceleyin.

Batch .NET içindeki [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) özellik son otomatik gerçekleştirilen havuzda çalışan ölçeklendirme hakkında bilgi sağlayan çeşitli özelliklere sahiptir:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

REST API [havuz hakkında bilgi alma](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) istek çalıştırın en son otomatik ölçeklendirme içeren havuzu hakkındaki bilgileri döndürür [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) özelliği.

Aşağıdaki C# kod parçacığı, havuzda çalışan son otomatik ölçeklendirme hakkında bilgi yazdırmak için Batch .NET kitaplığını kullanır. _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Yukarıdaki kod parçacığında, örnek çıktı:

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

## <a name="example-autoscale-formulas"></a>Örnek otomatik ölçeklendirme formülleri
Bir havuzdaki işlem kaynaklarının miktarını ayarlamak için farklı yollar gösterilmiştir birkaç formülleri bakalım.

### <a name="example-1-time-based-adjustment"></a>Örnek 1: Zamana bağlı ayarlama
Haftanın günü ve günün saatini temel alan havuz boyutunu ayarlamak istediğinizi varsayalım. Bu örnek artırmak ya da buna göre havuzdaki düğüm sayısını azaltmak nasıl gösterir.

Formül, öncelikle geçerli saati alır. Haftanın günü (1-5) ise ve çalışma saatleri (8: 00 için 18: 00) içinde hedef havuz boyutunu 20 düğümlerine ayarlanır. Aksi takdirde, 10 düğümü için ayarlanır.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Örnek 2: Görev tabanlı ayarlama
Bu örnekte, havuz boyutunu, kuyruktaki görevlerin sayısına göre ayarlanır. Hem açıklamaları hem de satır sonları formül dizelerde kabul edilir.

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
Bu örnek, görevler sayısına göre havuz boyutunu ayarlar. Bu formül de dikkate [MaxTasksPerComputeNode] [ net_maxtasks] havuz için ayarlanan değer. Bu yaklaşım durumlarda yararlıdır burada [paralel görev yürütme](batch-parallel-node-tasks.md) havuzunuz üzerinde etkin.

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

### <a name="example-4-setting-an-initial-pool-size"></a>Örnek 4: İlk havuz boyutunu ayarlama
Bu örnek, bir C# kod parçacığı bir belirtilen sayıda düğüme ilk bir süre için havuz boyutunu ayarlayan bir otomatik ölçeklendirme formülü ile gösterir. Havuz boyutunu göre çalışan sayısı ve etkin ayarlar daha sonra ilk süre geçtikten sonra görevleri.

Aşağıdaki kod parçacığı formülü:

* Dört düğüm için ilk havuz boyutunu ayarlar.
* Havuz boyutunu havuzun yaşam döngüsünün ilk 10 dakika içinde ayarlanmaz.
* 10 dakika sonra sayısını öğesinin maksimum değerini alır. Son 60 dakika içinde çalışan ve etkin görevler.
  * Her iki değeri 0 (hiçbir görev çalışan ya da son 60 dakika etkin olduğunu gösterir), havuz boyutunu 0 olarak ayarlanır.
  * Her iki değer sıfırdan büyük ise, hiçbir değişiklik yapıldı.

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
* [Eşzamanlı düğüm görevleri ile Azure toplu işlem kaynak kullanımını en üst düzeye](batch-parallel-node-tasks.md) nasıl birden çok görev aynı anda havuzunuzdaki işlem düğümlerinde yürütebilirsiniz hakkında ayrıntılı bilgi içerir. Otomatik ölçeklendirme ek olarak, bu özellik iş süresi paradan tasarruf etmeniz bazı iş yükleri için düşük yardımcı olabilir.
* Başka bir verimlilik güçlendirici paket için toplu işlem uygulamanızı en iyi şekilde Batch hizmeti sorgular emin olun. Bkz: [Azure Batch hizmetini verimli bir şekilde sorgulama](batch-efficient-list-queries.md) binlerce işlem düğümü veya görevler durumunu sorguladığınızda kabloya veri miktarını sınırlamak öğrenin.

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
