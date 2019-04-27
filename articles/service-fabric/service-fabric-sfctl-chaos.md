---
title: Azure Service Fabric CLI - sfctl chaos | Microsoft Docs
description: Service Fabric CLI'sını sfctl chaos komutlarını açıklamaktadır.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: b584ec301f0f4841c8df8fbbafb410abf645c373
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837360"
---
# <a name="sfctl-chaos"></a>sfctl chaos
Başlatma, durdurma ve kaos raporda test hizmeti.

## <a name="subgroups"></a>Alt gruplar
|Alt grubu|Açıklama|
| --- | --- |
| [schedule](service-fabric-sfctl-chaos-schedule.md) | Alın ve kaos zamanlamasını ayarlayın. |
## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| etkinlikler | Devamlılık belirteci veya zaman aralığına göre Chaos olayları sonraki segmentini alır. |
| Al | Kaos durumunu alın. |
| start | Kaos kümede başlatır. |
| Durdur | Kaos kümede çalışan ve kaos zamanlama durduruldu durumuna durdurur. |

## <a name="sfctl-chaos-events"></a>sfctl chaos olayları
Devamlılık belirteci veya zaman aralığına göre Chaos olayları sonraki segmentini alır.

Kaos olayları sonraki segmentini almak için ContinuationToken belirtebilirsiniz. Yeni bir segmentin Chaos olayların başlangıç yapmak, StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz. Aynı çağrıda ContinuationToken hem zaman aralığı belirtemezsiniz. Burada bir segment en fazla 100 Chaos olayları içeren birden çok parça parça ve sonraki kesim olayları döndürülür Chaos Chaos olayları 100'den fazla olduğunda, bu API devamlılık belirteci ile çağrı yapmak.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Bitiş zamanı utc | Windows Saati Chaos rapor oluşturulacak olduğu zaman aralığının son saati temsil eden dosya. Başvurun [DateTime.ToFileTimeUtc yöntemi](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) Ayrıntılar için. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --Başlangıç zamanı utc | Windows Saat Chaos rapor oluşturulacak olduğu zaman aralığı başlangıç saati temsil eden dosya. Başvurun [DateTime.ToFileTimeUtc yöntemi](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) Ayrıntılar için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-get"></a>sfctl chaos Al
Kaos durumunu alın.

Kaos çalışıyor olsun veya olmasın, Chaos ve kaos zamanlama durumunu çalıştırmak için kullanılan Chaos parametrelerini belirten Chaos durumunu alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-start"></a>sfctl chaos start
Kaos kümede başlatır.

Kaos kümede çalışmıyorsa Chaos geçirilen ile başlar Chaos parametreleri. Bu çağrı yapıldığında Chaos zaten çalışıyorsa çağrı FABRIC_E_CHAOS_ALREADY_RUNNING hata koduyla başarısız oluyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --app-türü-sistem durumu-ilkeyi-map | JSON için belirli uygulama türlerinde en yüksek yüzdesi iyi durumda olmayan uygulamalar listesi kodlanmış. Her giriş anahtar uygulama türü adı olarak ve belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdeyi temsil eden bir tamsayı değeri olarak belirtir. <br><br> En yüksek yüzdesi iyi durumda olmayan uygulamalar için belirli uygulama türlerinde sahip bir eşleme tanımlar. Her giriş anahtarı uygulama türü adı ve değeri belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdeyi temsil eden bir tamsayı olarak belirtir. Uygulama türü sistem durumu ilkesi eşlem küme sistem durumu değerlendirmesi sırasında özel uygulama türlerini tanımlamak için kullanılabilir. Haritada yer uygulama türleri, eşleme ve küme sistem durumu İlkesi'nde tanımlanan genel MaxPercentUnhealthyApplications ile belirtilen yüzde karşı değerlendirilir. Uygulama türleri eşlemesinde belirtilen uygulamaları, uygulamaların genel havuzunun karşı sayılmaz. Örneğin, bazı uygulamalar bir tür kritik ise küme yönetici harita uygulama türü için bir giriş ekleyin ve % 0 değerini atayın (diğer bir deyişle, herhangi bir hata oluştuğunda değil). Diğer tüm uygulamaları ile uygulama örnekleri binlerce dışında bazı hatalar tolerans %20 değerine MaxPercentUnhealthyApplications değerlendirilebilir. Küme bildiriminde HealthManager/EnableApplicationTypeHealthEvaluation yapılandırma girişi kullanarak uygulama türü sistem durumu değerlendirmesi etkinleştirirse uygulama türü sistem durumu ilkesi eşlem kullanılır. |
| --chaos-target-filter | JSON sözlüğü iki dize türü anahtarlarla kodlanmış. İki anahtar chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in ' dir. Bu anahtarların her ikisini de değerleri dize listesi kullanılır. chaos_target_filter hedeflenen Chaos hataları, örneğin, yalnızca belirli düğüm türleri hataya neden olan veya yalnızca belirli uygulamaların hataya neden olan tüm filtreleri tanımlar. <br><br> Chaos_target_filter kullanılmıyorsa, tüm küme varlıklar Chaos hataları. Chaos_target_filter kullanılırsa, Chaos chaos_target_filter belirtimi karşılayan varlıklar hataları. Chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in yalnızca bir birleşim semantiği sağlar. Chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in kesişimini belirtmek mümkün değildir. Örneğin, "Bu uygulama yalnızca söz konusu düğüm türünde olduğunda hata." belirlemek mümkün değil Bir varlık chaostargetfilter'daki Nodetypeınclusionlist veya Applicationınclusionlist'in dahil sonra bu varlık birden kullanarak tutulamaz. ApplicationX Applicationınclusionlist'in görünmez olsa bile, bunu chaostargetfilter'daki Nodetypeınclusionlist içinde bulunan nodeTypeY düğümünde olması gerektiğinden bazı Chaos yinelemede applicationX hatalı. ArgumentException hem chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in boş ise oluşturulur. Tüm tür hataları (düğümü yeniden başlatın, kod paketi yeniden, çoğaltmayı kaldırmak, çoğaltmayı yeniden başlatın, birincil taşıma ve ikincil Taşı) bu düğüm türü için düğümleri etkinleştirilir. Düğüm türü (NodeTypeX diyelim) chaostargetfilter'daki Nodetypeınclusionlist görünmüyor sonra düğüm düzeyi hataları (gibi NodeRestart) hiçbir zaman NodeTypeX düğümleri için etkinleştirilecek ancak kod paketi ve çoğaltma hataları hala etkinleştirilebilir NodeTypeX için de bir uygulama bildirimi Applicationınclusionlist'in NodeTypeX düğümde olur. Bu sayıyı artırmak için bu listede, en fazla 100 düğüm tipi adları eklenebilir, yapılandırma yükseltme MaxNumberOfNodeTypesInChaosEntityFilter yapılandırma için gereklidir. Bu uygulamaların hizmetlerine ait tüm çoğaltmaları tfs'deki Chaos ile çoğaltma hataları (yeniden başlatma çoğaltma, çoğaltma Kaldır, taşıma birincil ve taşıma ikincil). Kod paketi çoğaltmaları bu uygulamaların yalnızca barındırıyorsa chaos bir kod paketi yeniden başlatılabilir. Bir uygulama bu listede görünmüyorsa, chaostargetfilter'daki Nodetypeınclusionlist içinde bulunan bir düğüm türü, bir düğüm üzerinde uygulama sona ererse, yine de bazı Chaos yinelemede hatalı. Yerleştirme kısıtlamaları ve applicationX aracılığıyla nodeTypeY applicationX bağlıdır, ancak eksik Applicationınclusionlist'in ve nodeTypeY eksik chaostargetfilter'daki Nodetypeınclusionlist sonra applicationX hiçbir zaman hatayla kapatılacak. En fazla 1000 uygulama adları bu sayıyı artırmak için bu listede, eklenebilir, yapılandırma yükseltme MaxNumberOfApplicationsInChaosEntityFilter yapılandırma için gereklidir. |
| --context | (String, string) JSON olarak kodlanmış haritasını, anahtar-değer çiftlerini yazın. Harita Chaos çalıştırma hakkında bilgi kaydetmek için kullanılabilir. 100'den fazla çiftleri olamaz ve (anahtar veya değer) her bir dizenin en fazla 4095 karakter uzunluğunda olabilir. Bu harita, isteğe bağlı olarak belirli bir işlemle ilgili bağlam depolamak için çalıştırması Chaos başlatıcı tarafından ayarlanır. |
| --disable-move-replica-faults | Taşıma birincil devre dışı bırakır ve ikincil hataları taşıyın. |
| --max-cluster-stabilization | En fazla tüm kararlı ve iyi durumda olacak varlık kümesi için beklenecek süre miktarı.  Varsayılan\: 60. <br><br> Kaos yinelemelerde yürütür ve isteğe bağlı olarak her yinelemenin başında küme varlık durumunu doğrular. Bir küme varlık kararlı ve Sağlıklı MaxClusterStabilizationTimeoutInSeconds içinde değilse, doğrulama sırasında Chaos doğrulama başarısız bir olay oluşturur. |
| --max-concurrent-faults | Eş zamanlı hatalarının sayısı yineleme başlattı. Kaos yinelemelerde yürütür ve iki ardışık yinelemeler doğrulama aşama tarafından ayrılır. Daha yüksek Eş zamanlılık, daha agresif hatalar ortaya çıkarmak için durumları daha karmaşık bir dizi inducing hatalarının--ekleme. , 2 veya 3 bir değer ile başlatmak ve taşırken dikkatli için önerilir.  Varsayılan\: 1. |
| --en fazla yüzde-sağlıksız-uygulamaları | Küme durumu sırasında Chaos değerlendirirken, iyi durumda olmayan uygulamalar yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> İyi durumda olmayan uygulamalar yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan uygulamalar maksimum toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan uygulama sistem durumu uyarı olarak değerlendirilir. Bu, iyi durumda olmayan uygulamalar uygulama örnekleri ApplicationTypeHealthPolicyMap içinde bulunan uygulama türleri uygulamaları hariç kümedeki toplam sayısı üzerinden bölünmesiyle hesaplanır. Az sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan yüzde sıfırdır. |
| --max-percent-unhealthy-nodes | Küme durumu sırasında Chaos değerlendirirken, iyi durumda olmayan düğümler yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> İyi durumda olmayan düğümler yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız düğümleri izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan düğümlerin en yüksek toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan düğüm, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümlerin toplam sayısı üzerinden iyi durumda olmayan düğüm sayısına bölünmesiyle hesaplanır. Az sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan yüzde sıfırdır. Bu yüzdesi, tolerans yapılandırılması için büyük kümelerde bazı düğümleri her zaman aşağı veya çıkış onarımı için olur. |
| --zaman Tıkla-Çalıştır | Otomatik olarak durdurmadan önce Chaos çalışacağı toplam süre (saniye cinsinden). İzin verilen maksimum değer 4.294.967.295'e (System.UInt32.MaxValue) ' dir.  Varsayılan\: 4294967295. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --hatalar arasındaki bekleme süresi | Tek bir yineleme içinde art arda hatalar arasındaki bekleme süresi (saniye cinsinden).  Varsayılan\: 20. <br><br> Değer, alt hataları ve basit arasında çakışan durumu dizisini küme geçtiği geçer. 1 ve 5 ve alıştırma uyarı taşırken arasında bir değer ile başlamanız önerilir. |
| --yinelemeleri arasındaki bekleme süresi | Saati-(saniye cinsinden) arasında ayrım Chaos iki ardışık yinelemesi. Değer, alt hata ekleme oranı.  Varsayılan\: 30. |
| --warning-as-error | Uyarıları hata olarak aynı önem derecesi kabul edilip edilmeyeceğini belirtir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-stop"></a>sfctl chaos Durdur
Kaos kümede çalışan ve kaos zamanlama durduruldu durumuna durdurur.

Kaos yeni hataların yürütülmesini durdurur. Yürütülen hataları tamamlanana kadar yürütülmeye devam eder. Geçerli Chaos zamanlaması durdurulmuş bir duruma getirilir. Bir zamanlama durdurulduktan sonra durdurulmuş durumda kalır ve kaos zamanlama yeni Chaos çalışır kullanılamaz. Yeni bir Chaos zamanlama zamanlama sürdürmek için ayarlamanız gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).