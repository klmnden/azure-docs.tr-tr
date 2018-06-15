---
title: Azure Service Fabric CLI - sfctl chaos | Microsoft Docs
description: Service Fabric CLI sfctl chaos komutlarını açıklar.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: a27cb32243d731850099da88a57f7093878becf6
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763672"
---
# <a name="sfctl-chaos"></a>sfctl chaos
Hizmet başlatma, durdurma ve karmaşası Raporu sınayın.

## <a name="subgroups"></a>Alt gruplar
|Alt grup|Açıklama|
| --- | --- |
| [schedule](service-fabric-sfctl-chaos-schedule.md) | Alın ve chaos zamanlamasını ayarlayın. |
## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| etkinlikler | Devamlılık belirteci veya zaman aralığı göre Chaos olayların sonraki kesimini alır. |
| Al | Karmaşık dünyada durumu alınamıyor. |
| start | Chaos kümede başlatır. |
| Durdur | Kümede çalışan ve durdurulmuş durumda Chaos zamanlama put Chaos durdurur. |

## <a name="sfctl-chaos-events"></a>sfctl chaos olayları
Devamlılık belirteci veya zaman aralığı göre Chaos olayların sonraki kesimini alır.

Sonraki segment Chaos olayların almak için ContinuationToken belirtebilirsiniz. Yeni bir segment Chaos olayların başlamak için StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz. Aynı çağrı ContinuationToken ve zaman aralığını belirtemezsiniz. 100'den fazla Chaos olayları olduğunda olayları, en fazla 100 Chaos olay kesimi içerdiği birden çok parçalı ve sonraki segment almak için döndürülen karmaşası devamlılık belirteci ile bu API çağrısına olun.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Bitiş zamanı utc | Windows Chaos rapor oluşturulacak zaman aralığı bitiş saati temsil eden saat dosya. Başvurun [DateTime.ToFileTimeUtc yöntemi](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) Ayrıntılar için. |
| --max sonuçları | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu sayıda sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir. |
| --Başlangıç zamanı utc | Windows Chaos rapor oluşturulacak zaman aralığı başlangıç saati temsil eden saat dosya. Başvurun [DateTime.ToFileTimeUtc yöntemi](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) Ayrıntılar için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-get"></a>sfctl chaos Al
Karmaşık dünyada durumu alınamıyor.

Karmaşık dünyada Chaos çalışıyor olsun veya olmasın, Chaos ve Chaos zamanlama durumunu çalıştırmak için kullanılan Chaos parametreleri belirten durumu alınamıyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-start"></a>sfctl chaos Başlat
Chaos kümede başlatır.

Chaos kümede çalışmıyorsa Chaos geçirilen ile başlar Chaos parametrelerinde. Bu çağrısı yapıldığında Chaos zaten çalışıyorsa, çağrı FABRIC_E_CHAOS_ALREADY_RUNNING hata kodu ile başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama türü-sistem durumu-ilke-eşleme | JSON listesi belirli uygulama türleri için en fazla yüzde sağlıksız uygulamalarla kodlanmış. Her girişin bir anahtar uygulama türü adı ve değeri belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir tamsayı olarak belirtir. <br><br> Max yüzdesi sağlıksız uygulamalarla belirli uygulama türleri için bir eşleme tanımlar. Her giriş anahtar uygulama türü adı ve değeri belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir tamsayı olarak belirtir. <br><br> Uygulama türü sistem durumu ilkesi eşlem küme sistem durumu değerlendirmesi sırasında özel uygulama türlerini tanımlamak için kullanılabilir. Eşleme içinde bulunan uygulama türleri, küme sistem durumu ilkesinde tanımlanan genel MaxPercentUnhealthyApplications ile harita belirtilen yüzde karşı değerlendirilir. Uygulama türleri eşlemesinde belirtilen uygulamaları uygulamaları genel havuzu karşı sayılmaz. Örneğin, bazı uygulamalar türü kritik varsa, küme yönetici harita bu uygulama türü için bir giriş eklemek ve % 0 değerini atayın (diğer bir deyişle, hatalar tolerans değil). Diğer tüm uygulamalar için % 20 uygulama örnekleri binlerce dışında bazı hatalar tolerans koymak MaxPercentUnhealthyApplications ile değerlendirilebilir. Yalnızca küme bildiriminde HealthManager/EnableApplicationTypeHealthEvaluation için yapılandırma girdisi kullanarak uygulama türü sistem durumu değerlendirmesi etkinleştirirse uygulama türü sistem durumu ilkesi eşlem kullanılır. |
| --chaos hedef filtresi | JSON sözlük iki dize türü anahtarları ile kodlanmış. İki anahtar NodeTypeInclusionList ve ApplicationInclusionList ' dir. Bu anahtarların her ikisi de dize listesi değerler. chaos_target_filter hedeflenen Chaos hatalarına, örneğin, yalnızca belirli düğüm türleri hataya neden olan veya yalnızca belirli uygulamaları hataya neden olan tüm filtreleri tanımlar. <br><br> Chaos_target_filter kullanılmazsa, tüm küme varlıklar Chaos hataları. Chaos_target_filter kullanılırsa, Chaos chaos_target_filter belirtimi karşılayan varlıklar hataları. NodeTypeInclusionList ve ApplicationInclusionList yalnızca bir birleşim anlamsallarını izin verir. NodeTypeInclusionList ve ApplicationInclusionList kesişimini belirlemek mümkün değil. Örneğin, "Bu uygulama yalnızca bu düğüm türünde olduğunda hata." belirlemek mümkün değil Bir varlık NodeTypeInclusionList veya ApplicationInclusionList dahil sonra o varlık ChaosTargetFilter kullanarak tutulamaz. ApplicationX içinde ApplicationInclusionList bile görünmüyorsa, NodeTypeInclusionList dahil nodeTypeY bir düğümü üzerinde olmasını olur çünkü bazı Chaos yinelemede applicationX hatalı. NodeTypeInclusionList ve ApplicationInclusionList boşken ArgumentException atılır. <br><br> Hataları tüm türleri (düğümü yeniden başlatın, kod paketi yeniden, çoğaltmayı kaldırmak, çoğaltmayı yeniden başlatın, birincil taşımak ve ikincil taşıma) bu düğüm türleri düğümleri için etkinleştirilir. Düğüm türü (NodeTypeX söyleyin) NodeTypeInclusionList içinde görünmez sonra düğüm düzeyi hataları (gibi NodeRestart) NodeTypeX düğümleri için hiçbir zaman etkinleştirilir, ancak kod paketi ve çoğaltma hataları hala etkinleştirilebilir NodeTypeX için bir uygulamada varsa ApplicationInclusionList NodeTypeX düğümde olur. En fazla 100 düğüm türü adı bu sayıyı artırmak için bu listede, eklenebilir, MaxNumberOfNodeTypesInChaosEntityFilter yapılandırması için gerekli yapılandırma yükseltmedir. <br><br> Bu uygulamalar Hizmetleri'ne ait tüm çoğaltmaları uygun Chaos tarafından Çoğaltma hataları (yeniden başlatma çoğaltma, Kaldır çoğaltma, taşıma birincil ve ikincil taşıma). Kod paketi yalnızca bu uygulamaları çoğaltmalarının barındırıyorsa chaos kod paketi yeniden başlatılabilir. Bir uygulama bu listede görünmüyorsa, uygulama NodeTypeInclusionList içinde bulunan bir düğüm türünde bir düğümünde ererse, hala bazı Chaos yinelemede hatalı. Yerleştirme kısıtlamaları ve applicationX aracılığıyla nodeTypeY applicationX bağlıdır, ancak eksik ApplicationInclusionList ve nodeTypeY yok NodeTypeInclusionList sonra applicationX hiçbir zaman hatayla kapatılacak. En fazla 1000 uygulama adları bu sayıyı artırmak için bu listede, eklenebilir, MaxNumberOfApplicationsInChaosEntityFilter yapılandırması için gerekli yapılandırma yükseltmedir. |
| --bağlamı | (Dizesi;) JSON olarak kodlanmış haritasını anahtar-değer çiftlerini yazın. Harita Chaos işlemle ilgili bilgileri kaydetmek için kullanılabilir. Bu tür çiftleri 100'den fazla olamaz ve her bir dize (anahtar veya değer) en fazla 4095 karakterden uzun olamaz. Bu haritada bağlam belirli çalışma hakkında isteğe bağlı olarak depolamak için Çalıştır karmaşası başlatıcı tarafından ayarlanır. |
| --disable-move-replica-faults | Taşıma birincil devre dışı bırakır ve ikincil hataları taşıyın. |
| --en fazla küme sabitlemeyi | Tüm kararlı ve sağlam hale gelmesine varlık kümesi için beklenecek süreyi maksimum tutar.  Varsayılan\: 60. <br><br> Yinelemelerini Chaos yürütür ve isteğe bağlı olarak her bir yineleme başlangıcında küme varlıklar durumunu doğrular. Bir küme varlık kararlı ve Sağlıklı MaxClusterStabilizationTimeoutInSeconds içinde değilse, doğrulama sırasında Chaos doğrulama başarısız olayı oluşturur. |
| --en fazla eşzamanlı hataları | Eşzamanlı hatalarının sayısı yineleme kopyaladığınızda. Yinelemelerini Chaos yürütür ve iki ardışık yineleme doğrulama aşamasını tarafından ayrılır. Yüksek eşzamanlılık, daha katı hatalar ortaya çıkarmaya durumlardan daha karmaşık serisi inducing hatalarının--ekleme. , 2 veya 3 bir değerle başlayan ve taşırken dikkatli önerilir.  Varsayılan\: 1. |
| --max-yüzde-sağlıksız-uygulamalar | Küme durumu sırasında Chaos değerlendirirken, sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri ApplicationTypeHealthPolicyMap içerdiği uygulama türleri uygulamaların hariç kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. Küçük sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan, sıfır yüzdesidir. |
| --max yüzde-sağlıksız-düğümler | Küme durumu sırasında Chaos değerlendirirken, sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız düğümlerinin izin vermek için bu değer 10 olur. Yüzde küme hata olarak kabul edilmeden önce sağlıksız düğümleri maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate halde en az bir sağlıksız düğüm yoksa, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümler toplam sayısı üzerinden sağlıksız düğüm sayısını bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan, sıfır yüzdesidir. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarım için olacağından. |
| --zaman çalıştırma | Otomatik olarak durdurmadan önce Chaos çalışacağı toplam süre (saniye cinsinden). İzin verilen maksimum değer 4.294.967.295 (System.UInt32.MaxValue) ' dir.  Varsayılan\: 4294967295. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --hataları arasındaki bekleme süresi | Tek bir yineleme içinde art arda hatalar arasında bekleme süresi (saniye cinsinden).  Varsayılan\: 20. <br><br> Daha büyük değer, alt hataları ve basit arasında çakışan bir dizi durumu kümeyi geçtiği geçer. Taşırken 1 ve 5 ve alıştırma dikkat arasında bir değer ile başlamanız önerilir. |
| --yinelemeleri arasındaki bekleme süresi | Zaman-(saniye cinsinden) arasında ayırmayı karmaşık dünyada iki ardışık yineleme. Daha büyük değer, düşük hata ekleme oranı.  Varsayılan\: 30. |
| --hata olarak uyarı | Uyarı hata olarak değerlendirmek için sistem durumu ilkesi ayarlar. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-stop"></a>sfctl chaos Durdur
Kümede çalışan ve durdurulmuş durumda Chaos zamanlama put Chaos durdurur.

Chaos yeni hataları yürütülmesini durdurur. Yürütülen hataları bunlar tamamlanana kadar yürütülmeye devam eder. Geçerli Chaos zamanlamayı durdurulmuş bir duruma getirilir. Bir zamanlama durdurulduktan sonra durdurulmuş durumda kalır ve karmaşık dünyada Chaos zamanlama yeni çalışır kullanılmayacak. Yeni bir Chaos zamanlama zamanlama sürdürmek için ayarlamanız gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).