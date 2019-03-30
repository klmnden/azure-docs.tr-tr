---
title: Azure Service Fabric CLI - sfctl partition | Microsoft Docs
description: Service Fabric CLI'sını sfctl bölüm komutlarını açıklamaktadır.
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
ms.openlocfilehash: f7c9bcc51757100cb1fc957dee12213bc8bf2eec
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666775"
---
# <a name="sfctl-partition"></a>sfctl partition
Sorgulamak ve tüm bölümlerini yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| veri kaybı | Bu API, belirtilen bölüm için veri kaybı anlamına. |
| veri kaybı durumu | StartDataLoss API kullanmaya bölüm veri kaybı işlemin ilerlemesini alır. |
| sistem durumu | Belirtilen Service Fabric bölümü durumunu alır. |
| bilgi | Bir Service Fabric bölümü hakkında bilgi alır. |
| liste | Bir Service Fabric hizmeti bölümlerini listesini alır. |
| yük | Belirtilen Service Fabric bölümü yük bilgilerini alır. |
| Yük-Sıfırla | Bir Service Fabric bölümü geçerli iş yükünü sıfırlar. |
| Çekirdek kayıp | Belirli bir durum bilgisi olan hizmet bölüm çekirdek kayıp sevk. |
| Çekirdek kayıp durumu | StartQuorumLoss API kullanmaya bir bölüme bir çekirdek kayıp işleminin ilerleme durumunu alır. |
| Kurtarma | Service Fabric kümesine çekirdek kaybına şu anda takılı belirli bir bölüme, kurtarılır denemesi gösterir. |
| tüm kurtarma | Service Fabric kümesine çekirdek kaybına şu anda takılı kalıyor (sistem hizmetleri de dahil) tüm hizmetleri, kurtarılır denemesi gösterir. |
| durumu- | Service Fabric bölüm üzerinde bir sistem durumu raporu gönderir. |
| yeniden başlat | Bu API, bazı veya tüm çoğaltmalar veya belirtilen bölüm örneklerini yeniden başlatır. |
| yeniden başlatma durumu | StartPartitionRestart kullanmaya PartitionRestart işleminin ilerleme durumunu alır. |
| SVC adı | Bir bölüm için Service Fabric hizmeti adını alır. |

## <a name="sfctl-partition-data-loss"></a>sfctl bölüm veri kaybı
Bu API, belirtilen bölüm için veri kaybı anlamına.

Bu bölümün OnDataLossAsync API'sine çağrıda tetikler.  Bu API, belirtilen bölüm için veri kaybı anlamına. Bu bölümün OnDataLoss API'sine çağrıda tetikler. Gerçek veri kaybı üzerinde belirtilen DataLossMode bağlıdır.  <br> -PartialDataLoss - yalnızca bir çekirdek çoğaltmalarının kaldırılır ve OnDataLoss bölümü tetiklenir ancak gerçek veri kaybı uçuşan çoğaltma varlığını temel bağlıdır.  <br> -FullDataLoss - tüm çoğaltmaları kaldırılır OnDataLoss tetiklenir ve bu nedenle tüm veriler kaybolur. Bu API hedef olarak yalnızca durum bilgisi olan hizmetle çağrılmalıdır. Bu API ile bir sistem hizmet hedefi olarak çağırma önerilmez.

> [!NOTE] 
> Bu API çağrıldıktan sonra geri alınamaz. CancelOperation çağırma yalnızca yürütmeyi durdurun ve iç sistem durumunu temizleyin. Komut veri kaybına neden için yeteri kadar ilerledikten varsa veri geri yüklemez. Bu API ile başlatılan işlem hakkında bilgi döndürmek için aynı Operationıd GetDataLossProgress API'SİYLE çağırın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --veri kaybı-[gerekli] modu | Bu numaralandırma, hangi tür veri kaybı anlamına StartDataLoss API geçirilir. |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl bölüm veri kaybı-durumu
StartDataLoss API kullanmaya bölüm veri kaybı işlemin ilerlemesini alır.

Operationıd kullanarak StartDataLoss ile başlatılan bir veri kaybı işleminin ilerleme durumunu alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-health"></a>sfctl bölüm sistem durumu
Belirtilen Service Fabric bölümü durumunu alır.

Sistem durumu olaylarını sistem durumuna bağlı hizmette bildirilen koleksiyonunu filtrelemek için EventsHealthStateFilter kullanın. ReplicasHealthStateFilter bölümdeki ReplicaHealthState nesnelerinin koleksiyonunu filtrelemek için kullanın. Health store içinde var olmayan bir bölüm belirtirseniz, bu istek bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --exclude sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. İstatistik alt öğe sayısı durumunun Tamam, uyarı ve hata varlıkları gösterir. |
| --replicas-health-state-filter | Bölüm ReplicaHealthState nesneleri koleksiyonunu filtrelemeye izin verir. Değer üyeleri veya HealthStateFilter üyeleri üzerinde bit düzeyinde işlemler elde edilebilir. Filtreyle eşleşen çoğaltmaları döndürülür. Tüm çoğaltmalar toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer ise, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-info"></a>sfctl bölüm bilgileri
Bir Service Fabric bölümü hakkında bilgi alır.

Belirtilen bölüm hakkındaki bilgileri alır. Yanıt, bölüm kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem ve bölüm ilgili diğer ayrıntıları tarafından desteklenen anahtarlar içeriyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-list"></a>sfctl bölüm listesi
Bir Service Fabric hizmeti bölümlerini listesini alır.

Yanıt, bölüm kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem ve bölüm ilgili diğer ayrıntıları tarafından desteklenen anahtarlar içeriyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
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

## <a name="sfctl-partition-load"></a>sfctl bölüm yük
Belirtilen Service Fabric bölümü yük bilgilerini alır.

Belirtilen bölüm yükü hakkında bilgi döndürür. Yanıt, bir Service Fabric bölüm için yük raporların listesini içerir. Her rapor yük ölçüm adı, değer ve son bildirilen saati UTC biçiminde içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-load-reset"></a>sfctl bölüm yük Sıfırla
Bir Service Fabric bölümü geçerli iş yükünü sıfırlar.

Bir Service Fabric bölümü geçerli iş yükünü hizmeti için varsayılan yük sıfırlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl bölüm çekirdek zarar
Belirli bir durum bilgisi olan hizmet bölüm çekirdek kayıp sevk.

Bu API, hizmetiniz bir geçici çekirdek kayıp durumunuza için kullanışlıdır. Bu API ile başlatılan işlem hakkında bilgi döndürmek için aynı Operationıd GetQuorumLossProgress API'SİYLE çağırın. Bu durum bilgisi olan üzerinde çağrılabilir kalıcı (HasPersistedState == true) Hizmetleri.  Bu API, durum bilgisi olmayan hizmetler ya da durum bilgisi olan bellek içi yalnızca hizmetler kullanmayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çekirdek-kaybı-süresi [gerekli] | Çekirdek kaybına bölüm tutulacak süre miktarı.  Bu, saniyeler içinde belirtilmelidir. |
| --Çekirdek kayıp-[gerekli] modu | Bu sabit listesi, hangi tür anlamına çekirdek kaybı StartQuorumLoss API geçirilir. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-quorum-loss-status"></a>sfctl bölüm çekirdeği-kaybı-status
StartQuorumLoss API kullanmaya bir bölüme bir çekirdek kayıp işleminin ilerleme durumunu alır.

Sağlanan Operationıd kullanarak StartQuorumLoss ile başlatılan bir çekirdek kayıp işlemin ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-recover"></a>sfctl bölüm Kurtar
Service Fabric kümesine çekirdek kaybına şu anda takılı belirli bir bölüme, kurtarılır denemesi gösterir.

Bu işlemi yalnızca kapalı çoğaltmaları kurtarılamaz biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-recover-all"></a>sfctl bölüm tüm kurtarma
Service Fabric kümesine çekirdek kaybına şu anda takılı kalıyor (sistem hizmetleri de dahil) tüm hizmetleri, kurtarılır denemesi gösterir.

Bu işlemi yalnızca kapalı çoğaltmaları kurtarılamaz biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı, olası veri kaybına neden olabilir.

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

## <a name="sfctl-partition-report-health"></a>sfctl bölüm rapor-health
Service Fabric bölüm üzerinde bir sistem durumu raporu gönderir.

Belirtilen bölüm Service Fabric sistem durumunu raporlar. Rapor üzerinde bildirilen özellik ve sistem durumu raporu kaynağı hakkındaki bilgileri içermelidir. Rapor, bir Service Fabric ağ geçidine ileten sistem durumu deposu için bölüm gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sistem durumu deposu tarafından ek doğrulama sonrasında reddedilen. Örneğin, sistem durumu deposu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için rapor olayları bölümünde görünür denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] sistem durumu özelliği | Sistem durumu bilgileri özelliği. <br><br> Bir varlık sistem durumu raporlarının farklı özellikler için sahip olabilir. , Bir dize ve rapor tetikleyen durumu koşulu kategorilere ayırmak muhabir esnekliğini tanımak için olmayan bir sabit numaralandırma özelliğidir. Örneğin, "AvailableDisk" özelliği, düğüm üzerinde rapor için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı muhabir düğüm bağlantısı izleyebilirsiniz. Health store içinde bu raporları belirtilen düğüm için ayrı bir sistem durumu olayları olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgileri benzersiz olarak tanımlar. |
| --[gerekli] sistem durumu | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Warning', 'Error', 'Bilinmeyen'. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Kaynak Kimliği [gerekli] | Kaynak adı, sistem durumu bilgileri oluşturulan izleme/istemci/sistem bileşeni belirtir. |
| --açıklaması | Sistem durumu bilgileri açıklaması. <br><br> Bu, insan tarafından okunabilir rapor bilgilerini eklemek için kullanılan serbest metin temsil eder. Açıklama maksimum dize uzunluğu 4096 karakter olabilir. Sağlanan dize uzun olduğunda otomatik olarak kesilir. Kesirli kısmı, bir işaretçi "[kesildi]" açıklama son karakterleri içeren ve toplam dize boyutu 4096 karakter. İşaretleyici varlığı kullanıcılara bu kesme gösterir oluştu. Kesirli kısmı, açıklama orijinal dizeden küçüktür 4096 karakter olduğuna dikkat edin. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini gösteren bir bayrak. <br><br> Sistem Durumu raporu, Service Fabric için sistem durumu deposu ileten uygulama ağ geçidi için gönderilir. Hemen ayarlanmışsa true, raporun hemen sistem durumu deposu, HTTP ağ geçidi uygulaması kullanarak doku istemci ayarlarına bakılmaksızın HTTP ağ geçidi'ndeki gönderilir. Bu, olabildiğince çabuk gönderilmesi gereken kritik raporlar için kullanışlıdır. Zamanlama ve diğer koşullara bağlı olarak, rapor gönderme yine de, örneğin HTTP ağ geçidini kapalı veya ağ geçidi ileti ulaşmaz başarısız olabilir. Hemen false olarak ayarlarsanız, raporun durumu istemci ayarlarının HTTP ağ geçidi'nden göre gönderilir. Bu nedenle, bunu HealthReportSendInterval yapılandırmasına göre toplu olarak. Sistem Durumu raporu işleme yanı sıra health store iletilere raporlama sistem durumu iyileştirmek sistem durumu istemci izin verdiğinden Önerilen ayar budur. Varsayılan olarak, raporları hemen gönderilmez. |
| --remove-zaman süresi | Belirtecin süresi dolduğunda, health Store'dan rapor kaldırılmış olup olmadığını gösteren değer. <br><br> Süresi dolduktan sonra health Store'dan true olarak rapor kaldırılırsa. Rapor false olarak ayarlanırsa süresi dolduğunda hata kabul edilir Bu özellik varsayılan olarak false değeridir. İstemciler düzenli aralıklarla bildirdiğinde RemoveWhenExpired false (varsayılan) ayarlamanız gerekir. Bu şekilde muhabir sorunları (örneğin, kilitlenme) ve rapor veremez, varlık sistem durumu raporu süresi dolduğunda hatası değerlendirilir ' dir. Bu varlık sistem durumu hatası olarak işaretler. |
| --sıra numarası | Bu sistem durumu raporu sayısal dize olarak için sıra numarası. <br><br> Rapor sıra numarası, eski raporlar algılamak için sistem durumu deposu tarafından kullanılır. Bir rapora eklendiğinde belirtilmezse, bir sıra numarası otomatik olarak sistem istemci tarafından üretilir. |
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

## <a name="sfctl-partition-restart"></a>sfctl bölümü yeniden başlatma
Bu API, bazı veya tüm çoğaltmalar veya belirtilen bölüm örneklerini yeniden başlatır.

Bu API, yük devretme testi için kullanışlıdır. Durum bilgisi olmayan hizmet bölüm hedeflemek için kullanılan RestartPartitionMode AllReplicasOrInstances olması gerekir. GetPartitionRestartProgress ilerleme durumunu almak için aynı Operationıd'ni kullanarak API çağırın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --yeniden başlatma-partition-modu [gerekli] | Hangi bölümlerin yeniden açıklanmaktadır. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-restart-status"></a>sfctl bölümü yeniden başlatma durumu
StartPartitionRestart kullanmaya PartitionRestart işleminin ilerleme durumunu alır.

Sağlanan Operationıd kullanarak StartPartitionRestart ile başlatılan bir PartitionRestart ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-svc-name"></a>sfctl bölüm svc-adı
Bir bölüm için Service Fabric hizmeti adını alır.

Belirtilen bölüm için hizmetin adını alır. Bölüm kimliği kümede yoksa bir 404 hatası döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
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
