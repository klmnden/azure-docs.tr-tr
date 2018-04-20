---
title: Azure Service Fabric CLI - sfctl choas | Microsoft Docs
description: Service Fabric CLI sfctl chaos komutlarını açıklar.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 34e4d47b1de509c2053996d9d1078733d7055447
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="sfctl-chaos"></a>sfctl chaos
Hizmet başlatma, durdurma ve karmaşası Raporu sınayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    rapor| Geçilen devamlılık belirteci veya geçen zaman aralığı göre Chaos raporun sonraki kesimini alır.|
|    start | Chaos kümede başlatır.|
|    Durdur  | Durdurur Chaos zaten çalışıyorsa, kümede Aksi takdirde, hiçbir şey yapmaz.|


## <a name="sfctl-chaos-report"></a>sfctl chaos raporu
Geçilen devamlılık belirteci veya geçen zaman aralığı göre Chaos raporun sonraki kesimini alır.

Chaos rapor sonraki kesimin almak için ContinuationToken ya da belirtebilirsiniz veya StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz, ancak aynı çağrısında ContinuationToken ve zaman aralığı belirtemezsiniz. 100'den fazla Chaos olayları olduğunda Chaos rapor, en fazla 100 Chaos olay kesimi içerdiği segmentlerinde döndürülür. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci| Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --Bitiş zamanı utc   | Windows Chaos rapor oluşturulacak zaman aralığı bitiş saati temsil eden saat dosya. Lütfen bakın [DateTime.ToFileTimeUtc yöntemi](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) Ayrıntılar için.|
| --Başlangıç zamanı utc | Windows Chaos rapor oluşturulacak zaman aralığı başlangıç saati temsil eden saat dosya. Lütfen bakın [DateTime.ToFileTimeUtc yöntemi](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) Ayrıntılar için.|
| --zaman aşımı -t     | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama          | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım        | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı      | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu          | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı        | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-chaos-start"></a>sfctl chaos Başlat
Chaos kümede başlatır.

Chaos kümede çalışmıyorsa Chaos geçirilen ile başlar Chaos parametrelerinde. Bu çağrısı yapıldığında Chaos zaten çalışıyorsa, çağrı FABRIC_E_CHAOS_ALREADY_RUNNING hata kodu ile başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama türü-sistem durumu-ilke-eşleme  | JSON listesi belirli uygulama türleri için en fazla yüzde sağlıksız uygulamalarla kodlanmış. Her girişin bir anahtar uygulama türü adı ve değeri belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir tamsayı olarak belirtir. Max yüzdesi sağlıksız uygulamalarla belirli uygulama türleri için bir eşleme tanımlar. Her giriş anahtar uygulama türü adı ve değeri belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir tamsayı olarak belirtir. Uygulama türü sistem durumu ilkesi eşlem küme sistem durumu değerlendirmesi sırasında özel uygulama türlerini tanımlamak için kullanılabilir. Eşleme içinde bulunan uygulama türleri, küme sistem durumu ilkesinde tanımlanan genel MaxPercentUnhealthyApplications ile harita belirtilen yüzde karşı değerlendirilir. Uygulama türleri eşlemesinde belirtilen uygulamaları uygulamaları genel havuzu karşı sayılmaz. Örneğin, bazı uygulamalar türü kritik varsa, küme yönetici harita bu uygulama türü için bir giriş eklemek ve % 0 değerini atayın (diğer bir deyişle, hatalar tolerans değil). Diğer tüm uygulamalar için % 20 uygulama örnekleri binlerce dışında bazı hatalar tolerans koymak MaxPercentUnhealthyApplications ile değerlendirilebilir. Yalnızca küme bildiriminde HealthManager/EnableApplicationTypeHealthEvaluation için yapılandırma girdisi kullanarak uygulama türü sistem durumu değerlendirmesi etkinleştirirse uygulama türü sistem durumu ilkesi eşlem kullanılır.|
|--chaos hedef filtresi         | JSON sözlük iki dize türü anahtarları ile kodlanmış. İki anahtar NodeTypeInclusionList ve ApplicationInclusionList ' dir. Bu anahtarların her ikisi de dize listesi değerler. chaos_target_filter hedeflenen Chaos hatalarına, örneğin, yalnızca belirli düğüm türleri hataya neden olan veya yalnızca belirli uygulamaları hataya neden olan tüm filtreleri tanımlar. Chaos_target_filter kullanılmazsa, tüm küme varlıklar Chaos hataları. Chaos_target_filter kullanılırsa, Chaos chaos_target_filter belirtimi karşılayan varlıklar hataları. NodeTypeInclusionList ve ApplicationInclusionList yalnızca bir birleşim anlamsallarını izin verir. NodeTypeInclusionList ve ApplicationInclusionList kesişimini belirlemek mümkün değil. Örneğin, "Bu uygulama yalnızca bu düğüm türünde olduğunda hata." belirlemek mümkün değil Bir varlık NodeTypeInclusionList veya ApplicationInclusionList dahil sonra o varlık ChaosTargetFilter kullanarak tutulamaz. ApplicationX içinde ApplicationInclusionList bile görünmüyorsa, NodeTypeInclusionList dahil nodeTypeY bir düğümü üzerinde olmasını olur çünkü bazı Chaos yinelemede applicationX hatalı. NodeTypeInclusionList ve ApplicationInclusionList boşken ArgumentException atılır. Hataları tüm türleri (düğümü yeniden başlatın, kod paketi yeniden, çoğaltmayı kaldırmak, çoğaltmayı yeniden başlatın, birincil taşımak ve ikincil taşıma) bu düğüm türleri düğümleri için etkinleştirilir. Düğüm türü (NodeTypeX söyleyin) NodeTypeInclusionList içinde görünmez sonra düğüm düzeyi hataları (gibi NodeRestart) NodeTypeX düğümleri için hiçbir zaman etkinleştirilir, ancak kod paketi ve çoğaltma hataları hala etkinleştirilebilir NodeTypeX için bir uygulamada varsa ApplicationInclusionList NodeTypeX düğümde olur. En fazla 100 düğüm türü adı bu sayıyı artırmak için bu listede, eklenebilir, MaxNumberOfNodeTypesInChaosEntityFilter yapılandırması için gerekli yapılandırma yükseltmedir. Bu uygulamalar Hizmetleri'ne ait tüm çoğaltmaları uygun Chaos tarafından Çoğaltma hataları (yeniden başlatma çoğaltma, Kaldır çoğaltma, taşıma birincil ve ikincil taşıma). Kod paketi yalnızca bu uygulamaları çoğaltmalarının barındırıyorsa chaos kod paketi yeniden başlatılabilir. Bir uygulama bu listede görünmüyorsa, uygulama NodeTypeInclusionList içinde bulunan bir düğüm türünde bir düğümünde ererse, hala bazı Chaos yinelemede hatalı. Yerleştirme kısıtlamaları ve applicationX aracılığıyla nodeTypeY applicationX bağlıdır, ancak eksik ApplicationInclusionList ve nodeTypeY yok NodeTypeInclusionList sonra applicationX hiçbir zaman hatayla kapatılacak. En fazla 1000 uygulama adları bu sayıyı artırmak için bu listede, eklenebilir, MaxNumberOfApplicationsInChaosEntityFilter yapılandırması için gerekli yapılandırma yükseltmedir.|
|--bağlamı                     | (Dizesi;) JSON olarak kodlanmış haritasını anahtar-değer çiftlerini yazın. Harita Chaos işlemle ilgili bilgileri kaydetmek için kullanılabilir. Bu tür çiftleri 100'den fazla olamaz ve her bir dize (anahtar veya değer) en fazla 4095 karakterden uzun olamaz. Bu haritada bağlam belirli çalışma hakkında isteğe bağlı olarak depolamak için Çalıştır karmaşası başlatıcı tarafından ayarlanır.|
| --disable-move-replica-faults | Taşıma birincil devre dışı bırakır ve ikincil hataları taşıyın.|
| --en fazla küme sabitlemeyi| Tüm kararlı ve sağlam hale gelmesine varlık kümesi için beklenecek süreyi maksimum tutar.  Varsayılan: 60. Yinelemelerini Chaos yürütür ve isteğe bağlı olarak her bir yineleme başlangıcında küme varlıklar durumunu doğrular. Bir küme varlık kararlı ve Sağlıklı MaxClusterStabilizationTimeoutInSeconds içinde değilse, doğrulama sırasında Chaos doğrulama başarısız olayı oluşturur.|
| --en fazla eşzamanlı hataları    | Eşzamanlı hatalarının sayısı yineleme kopyaladığınızda.           Varsayılan: 1. Yinelemelerini Chaos yürütür ve iki ardışık yineleme doğrulama aşamasını tarafından ayrılır. Yüksek eşzamanlılık, daha katı hatalar ortaya çıkarmaya durumlardan daha karmaşık serisi inducing hatalarının--ekleme. , 2 veya 3 bir değerle başlayan ve taşırken dikkatli önerilir.|
| --max-yüzde-sağlıksız-uygulamalar  | Küme durumu sırasında Chaos değerlendirirken, sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri ApplicationTypeHealthPolicyMap içerdiği uygulama türleri uygulamaların hariç kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. Küçük sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan, sıfır yüzdesidir.|
| --max yüzde-sağlıksız-düğümler | Küme durumu sırasında Chaos değerlendirirken, sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. Sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız düğümlerinin izin vermek için bu değer 10 olur. Yüzde küme hata olarak kabul edilmeden önce sağlıksız düğümleri maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate halde en az bir sağlıksız düğüm yoksa, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümler toplam sayısı üzerinden sağlıksız düğüm sayısını bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan, sıfır yüzdesidir. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarım için olacağından.|
| --zaman çalıştırma              | Otomatik olarak durdurmadan önce Chaos çalışacağı toplam süre (saniye cinsinden). İzin verilen maksimum değer 4.294.967.295 (System.UInt32.MaxValue) ' dir.  Varsayılan: 4294967295.|
| --zaman aşımı -t               | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|
| --hataları arasındaki bekleme süresi | Tek bir yineleme içinde art arda hatalar arasında bekleme süresi (saniye cinsinden).  Varsayılan: 20. Daha büyük değer, alt hataları ve basit arasında çakışan bir dizi durumu kümeyi geçtiği geçer. Taşırken 1 ve 5 ve alıştırma dikkat arasında bir değer ile başlamanız önerilir.|
| --yinelemeleri arasındaki bekleme süresi| Zaman-(saniye cinsinden) arasında ayırmayı karmaşık dünyada iki ardışık yineleme. Daha büyük değer, düşük hata ekleme oranı. Varsayılan: 30.|
| --hata olarak uyarı         | Küme durumu sırasında Chaos değerlendirirken, aynı önem uyarılarla hata olarak kabul eder.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                    | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                  | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.           Varsayılan: json.|
| --Sorgu                    | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı                  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-chaos-stop"></a>sfctl chaos Durdur
Durdurur Chaos zaten çalışıyorsa, kümede Aksi takdirde, hiçbir şey yapmaz.

Daha fazla hataları planlama durakları Chaos; Ancak, yürütülen hataları etkilenmez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t| Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama  | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım| Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu  | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı| Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).