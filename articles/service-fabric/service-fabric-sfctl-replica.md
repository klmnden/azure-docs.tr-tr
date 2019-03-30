---
title: Azure Service Fabric CLI - sfctl çoğaltma | Microsoft Docs
description: Service Fabric CLI'sını sfctl çoğaltma komutlarını açıklamaktadır.
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
ms.openlocfilehash: d0a7199ff0e9cb17c3fbc179a9b37a6620f521f9
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666826"
---
# <a name="sfctl-replica"></a>sfctl replica
Hizmet bölümlere ait çoğaltmaları yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| dağıtılan | Bir Service Fabric dağıtıldığını çoğaltma ayrıntılarını alır. |
| dağıtılan listesi | Bir Service Fabric dağıtıldığını çoğaltmaların listesini alır. |
| sistem durumu | Bir Service Fabric durum bilgisi olan hizmet çoğaltma veya durum bilgisi olmayan hizmet durumunu alır. |
| bilgi | Bir Service Fabric bölümünün bir çoğaltma bilgilerini alır. |
| liste | Bir Service Fabric hizmeti bölüm çoğaltmaları hakkında daha fazla bilgi alır. |
| kaldır | Bir düğümde çalışan bir hizmet çoğaltmaya kaldırır. |
| durumu- | Service Fabric çoğaltma üzerindeki bir sistem durumu raporu gönderir. |
| yeniden başlat | Bir düğüm üzerinde çalışan kalıcı bir hizmet hizmeti çoğaltmasını yeniden başlatır. |

## <a name="sfctl-replica-deployed"></a>dağıtılan sfctl çoğaltma
Bir Service Fabric dağıtıldığını çoğaltma ayrıntılarını alır.

Bir Service Fabric dağıtıldığını çoğaltma ayrıntılarını alır. Hizmet türü, hizmet adı, geçerli hizmet işleminin bilgiler içerir, geçerli hizmet işleminin başlangıç tarih saat, bölüm kimliği, yineleme/örnek kimliği, bildirilen yükleme ve diğer bilgiler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çoğaltma kimliği [gerekli] | Çoğaltma tanımlayıcısı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-deployed-list"></a>dağıtılan sfctl çoğaltma-listesi
Bir Service Fabric dağıtıldığını çoğaltmaların listesini alır.

Bir Service Fabric dağıtıldığını çoğaltmaları hakkında daha fazla bilgi içeren listeyi alır. Bilgiler, bölüm kimliği, Yineleme Kimliği, adı Hizmet türü ve diğer bilgiler, hizmet adını, çoğaltma durumunu içerir. Bu parametreler için belirtilen değerlere eşleşen dağıtılan çoğaltmaları hakkında daha fazla bilgi almak için PartitionID veya ServiceManifestName sorgu parametrelerini kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --bölüm kimliği | Bölüm kimliği. |
| --Hizmet bildiriminin adı | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı bir hizmet bildiriminin adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-health"></a>sfctl çoğaltma durumu
Bir Service Fabric durum bilgisi olan hizmet çoğaltma veya durum bilgisi olmayan hizmet durumunu alır.

Bir Service Fabric çoğaltma durumunu alır. Sistem durumu olaylarını sistem durumuna bağlıdır çoğaltmadaki bildirilen koleksiyonunu filtrelemek için EventsHealthStateFilter kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çoğaltma kimliği [gerekli] | Çoğaltma tanımlayıcısı. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-info"></a>sfctl çoğaltma bilgileri
Bir Service Fabric bölümünün bir çoğaltma bilgilerini alır.

Yanıt kimliği, rol, durum, sistem durumu, düğüm adı, çalışma süresi ve çoğaltma ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çoğaltma kimliği [gerekli] | Çoğaltma tanımlayıcısı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-list"></a>sfctl çoğaltma listesi
Bir Service Fabric hizmeti bölüm çoğaltmaları hakkında daha fazla bilgi alır.

Belirtilen bölüm çoğaltmaları hakkında daha fazla bilgi GetReplicas uç noktasını döndürür. Yanıt kimliği, rol, durum, sistem durumu, düğüm adı, çalışma süresi ve çoğaltma ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-remove"></a>sfctl çoğaltma Kaldır
Bir düğümde çalışan bir hizmet çoğaltmaya kaldırır.

Bu API, bir Service Fabric kümesinden bir çoğaltma kaldırarak bir Service Fabric çoğaltma hatası benzetimini yapar. Kaldırma çoğaltma kapatır, çoğaltma rolü yok ve ardından kaldırır tüm geçişler kümeden çoğaltmanın durumu bilgileri. Bu API, çoğaltma durumu kaldırma yolu test eder ve istemci API'leri aracılığıyla rapor hata kalıcı yolu benzetimini yapar. Bu API kullanıldığında gerçekleştirilen herhangi bir güvenlik denetimi uyarı - burada var. Bu API'nin yanlış kullanımı durum bilgisi olan hizmetler için veri kaybına neden olabilir. Ayrıca, forceRemove bayrağı aynı işlemde barındırılan tüm çoğaltmaları etkiler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çoğaltma kimliği [gerekli] | Çoğaltma tanımlayıcısı. |
| --force-Kaldır | Bir Service Fabric uygulaması veya hizmeti kapatılmasını sırasıyla gitmeden zorla kaldırın. Bu parametre, zorla bir uygulamayı silmek için kullanılabilir veya hizmet için hangi silme zaman aşımına uğramadan hizmet kodda engelleyen sorunları nedeniyle normal kopyaları kapatın. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-report-health"></a>sfctl çoğaltma durumu-
Service Fabric çoğaltma üzerindeki bir sistem durumu raporu gönderir.

Belirtilen Service Fabric çoğaltma sistem durumunu raporlar. Rapor üzerinde bildirilen özellik ve sistem durumu raporu kaynağı hakkındaki bilgileri içermelidir. Rapor, bir Service Fabric ağ geçidine ileten sistem durumu deposu için çoğaltma gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sistem durumu deposu tarafından ek doğrulama sonrasında reddedilen. Örneğin, sistem durumu deposu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Çalışma raporu health store içinde uygulanmış olup olmadığını görmek için çoğaltma durumunu ve rapor HealthEvents bölümde görünen onay alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] sistem durumu özelliği | Sistem durumu bilgileri özelliği. <br><br> Bir varlık sistem durumu raporlarının farklı özellikler için sahip olabilir. , Bir dize ve rapor tetikleyen durumu koşulu kategorilere ayırmak muhabir esnekliğini tanımak için olmayan bir sabit numaralandırma özelliğidir. Örneğin, "AvailableDisk" özelliği, düğüm üzerinde rapor için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı muhabir düğüm bağlantısı izleyebilirsiniz. Health store içinde bu raporları belirtilen düğüm için ayrı bir sistem durumu olayları olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgileri benzersiz olarak tanımlar. |
| --[gerekli] sistem durumu | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Warning', 'Error', 'Bilinmeyen'. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çoğaltma kimliği [gerekli] | Bölüm kimliği. |
| --Kaynak Kimliği [gerekli] | Kaynak adı, sistem durumu bilgileri oluşturulan izleme/istemci/sistem bileşeni belirtir. |
| --açıklaması | Sistem durumu bilgileri açıklaması. <br><br> Bu, insan tarafından okunabilir rapor bilgilerini eklemek için kullanılan serbest metin temsil eder. Açıklama maksimum dize uzunluğu 4096 karakter olabilir. Sağlanan dize uzun olduğunda otomatik olarak kesilir. Kesirli kısmı, bir işaretçi "[kesildi]" açıklama son karakterleri içeren ve toplam dize boyutu 4096 karakter. İşaretleyici varlığı kullanıcılara bu kesme gösterir oluştu. Kesirli kısmı, açıklama orijinal dizeden küçüktür 4096 karakter olduğuna dikkat edin. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini gösteren bir bayrak. <br><br> Sistem Durumu raporu, Service Fabric için sistem durumu deposu ileten uygulama ağ geçidi için gönderilir. Hemen ayarlanmışsa true, raporun hemen sistem durumu deposu, HTTP ağ geçidi uygulaması kullanarak doku istemci ayarlarına bakılmaksızın HTTP ağ geçidi'ndeki gönderilir. Bu, olabildiğince çabuk gönderilmesi gereken kritik raporlar için kullanışlıdır. Zamanlama ve diğer koşullara bağlı olarak, rapor gönderme yine de, örneğin HTTP ağ geçidini kapalı veya ağ geçidi ileti ulaşmaz başarısız olabilir. Hemen false olarak ayarlarsanız, raporun durumu istemci ayarlarının HTTP ağ geçidi'nden göre gönderilir. Bu nedenle, bunu HealthReportSendInterval yapılandırmasına göre toplu olarak. Sistem Durumu raporu işleme yanı sıra health store iletilere raporlama sistem durumu iyileştirmek sistem durumu istemci izin verdiğinden Önerilen ayar budur. Varsayılan olarak, raporları hemen gönderilmez. |
| --remove-zaman süresi | Belirtecin süresi dolduğunda, health Store'dan rapor kaldırılmış olup olmadığını gösteren değer. <br><br> Süresi dolduktan sonra health Store'dan true olarak rapor kaldırılırsa. Rapor false olarak ayarlanırsa süresi dolduğunda hata kabul edilir Bu özellik varsayılan olarak false değeridir. İstemciler düzenli aralıklarla bildirdiğinde RemoveWhenExpired false (varsayılan) ayarlamanız gerekir. Bu şekilde muhabir sorunları (örneğin, kilitlenme) ve rapor veremez, varlık sistem durumu raporu süresi dolduğunda hatası değerlendirilir ' dir. Bu varlık sistem durumu hatası olarak işaretler. |
| --sıra numarası | Bu sistem durumu raporu sayısal dize olarak için sıra numarası. <br><br> Rapor sıra numarası, eski raporlar algılamak için sistem durumu deposu tarafından kullanılır. Bir rapora eklendiğinde belirtilmezse, bir sıra numarası otomatik olarak sistem istemci tarafından üretilir. |
| --service-kind | Sistem bildirildiğinden tür hizmeti çoğaltma (durum bilgisi olan veya). Olası değerler şunlardır\: 'Durum bilgisiz', 'Durum bilgisi olan'.  Varsayılan\: durum bilgisi olan. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --ttl | Bu sistem durumu raporu geçerli olduğu süre. Bu alan, süresi belirtmek için ISO8601 biçimini kullanır. <br><br> İstemciler düzenli aralıklarla rapor, yaşam süresi daha yüksek sıklıkta raporları göndermelisiniz. İstemcileri geçişi bildirirse, bunlar sonsuz için yaşam süresi ayarlayabilirsiniz. Yaşam süresi dolduğunda, sistem durumu bilgilerini içeren sistem durumu olayı RemoveWhenExpired ise health Store'dan kaldırıldı ya da doğru veya hata sırasında değerlendirilen ise RemoveWhenExpired false. Aksi durumda sonsuz değer varsayılan olarak belirtilen, süresi. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-replica-restart"></a>sfctl çoğaltma yeniden başlatma
Bir düğüm üzerinde çalışan kalıcı bir hizmet hizmeti çoğaltmasını yeniden başlatır.

Bir düğüm üzerinde çalışan kalıcı bir hizmet hizmeti çoğaltmasını yeniden başlatır. Bu API kullanıldığında gerçekleştirilen herhangi bir güvenlik denetimi uyarı - burada var. Bu API'nin yanlış kullanımı durum bilgisi olan hizmetler için kullanılabilirlik kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çoğaltma kimliği [gerekli] | Çoğaltma tanımlayıcısı. |
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
