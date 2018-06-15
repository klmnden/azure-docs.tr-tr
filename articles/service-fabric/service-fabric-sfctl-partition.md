---
title: Azure Service Fabric CLI - sfctl bölüm | Microsoft Docs
description: Service Fabric CLI sfctl bölüm komutlarını açıklar.
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
ms.openlocfilehash: a9455683c5fad7fad4dda62fd967da617d8a8496
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763655"
---
# <a name="sfctl-partition"></a>sfctl partition
Sorgulamak ve herhangi bir hizmet için bölüm yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| veri kaybı | Bu API, belirtilen bölüm için veri kaybı anlamına. |
| veri kaybı durumu | StartDataLoss API kullanmaya bir bölüm veri kaybı işlemin ilerlemesini alır. |
| sistem durumu | Belirtilen Service Fabric bölüm durumunu alır. |
| bilgi | Service Fabric bölüm hakkında bilgi alır. |
| liste | Service Fabric hizmetinin bölümleri listesini alır. |
| yükleme | Belirtilen Service Fabric bölüm yük bilgilerini alır. |
| Yük-sıfırlama | Geçerli iş yükünü bir Service Fabric bölümü sıfırlar. |
| Çekirdek kayıp | Çekirdek kayıp verilen durum bilgisi olan hizmet bölümü uygulanmasını. |
| Çekirdek kayıp durumu | Çekirdek kayıp işlemin StartQuorumLoss API kullanmaya bir bölüme ilerlemesini alır. |
| Kurtarma | Service Fabric kümesi şu anda çekirdek kaybında takıldı belirli bir bölüm kurtarmayı denemesi belirtir. |
| tüm kurtarma | Service Fabric kümesi şu anda çekirdek kaybında takıldı (Sistem Hizmetleri dahil) tüm hizmetleri kurtarmayı denemesi belirtir. |
| Sistem Durumu raporu | Service Fabric bölüm üzerinde bir sistem durumu raporu gönderir. |
| Yeniden başlatma | Bu API, bazı veya tüm çoğaltmaları ya da belirtilen bölüm örneklerini yeniden başlatılır. |
| yeniden başlatma durumu | StartPartitionRestart kullanmaya PartitionRestart işlemin ilerlemesini alır. |
| SVC adı | Bir bölüm için Service Fabric hizmet adını alır. |

## <a name="sfctl-partition-data-loss"></a>sfctl bölüm veri kaybı
Bu API, belirtilen bölüm için veri kaybı anlamına.

Bölümün OnDataLossAsync API çağrısı tetikler.  Bu API, belirtilen bölüm için veri kaybı anlamına. Bölümün OnDataLoss API çağrısı tetikler. Gerçek veri kaybı üzerinde belirtilen DataLossMode bağlıdır. <br> PartialDataLoss - yalnızca bir çekirdek çoğaltmalarının kaldırılır ve OnDataLoss için bölüm tetiklenir ancak gerçek veri kaybı yürütülen çoğaltma varlığına bağlıdır. <br>Tüm çoğaltmaları FullDataLoss - olan kaldırılır ve bu nedenle tüm veriler kayıp OnDataLoss tetiklenir. <br>Bu API hedefi olarak yalnızca durum bilgisi olan hizmeti ile çağrılmalıdır. Bir sistem hizmet hedefi olarak ile bu API'yi çağıran önerilmez. 
> [!NOTE]
> Bu API çağrıldığında alınamaz. CancelOperation çağırma yalnızca yürütme durdurun ve iç sistem durumu yedeklemesi temizleyin. Komut yetecek kadar veri kaybına neden değişmiştir varsa veri geri yüklenmeyecek. Bu API'si ile çalışmaya işlemler hakkında bilgi döndürmek için aynı Operationıd GetDataLossProgress API'SİYLE çağırın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --veri kaybı-[gerekli] modu | Bu Sıralayıcı, ne tür bir veri kaybı anlamına belirtmek için StartDataLoss API'sine geçirilir. |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl bölüm veri kaybı-durum
StartDataLoss API kullanmaya bir bölüm veri kaybı işlemin ilerlemesini alır.

Operationıd kullanarak StartDataLoss ile başlatılan bir veri kaybı işlemin ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-health"></a>sfctl bölüm sistem durumu
Belirtilen Service Fabric bölüm durumunu alır.

Belirtilen bölüm sistem durumu bilgilerini alır. Sistem durumu olayları sistem durumuna bağlı hizmet bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. ReplicasHealthStateFilter bölüme ReplicaHealthState nesne koleksiyonundaki filtre uygulamak için kullanın. Health store içinde yok. bir bölüm belirtirseniz, bu istek bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir. |
| --çoğaltmaları sağlık Durumu Filtresi | Bölüm ReplicaHealthState nesneleri koleksiyonu filtrelemeye izin verir. Değer üyeleri veya HealthStateFilter üyeleri üzerinde bit düzeyinde işlemler alınamıyor. Filtreyle eşleşen çoğaltmaları döndürülür. Tüm çoğaltmaları toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-info"></a>sfctl bölüm bilgileri
Service Fabric bölüm hakkında bilgi alır.

Belirtilen bölüm hakkındaki bilgileri alır. Yanıt, bölüm kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem ve bölüm ile ilgili diğer ayrıntıları tarafından desteklenen anahtarlarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-list"></a>sfctl bölüm listesi
Service Fabric hizmetinin bölümleri listesini alır.

Service Fabric hizmetinin bölümleri listesini alır. Yanıt, bölüm kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem ve bölüm ile ilgili diğer ayrıntıları tarafından desteklenen anahtarlarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-load"></a>sfctl bölüm yükü
Belirtilen Service Fabric bölüm yük bilgilerini alır.

Belirtilen bölüm yükleme hakkında bilgi verir. Yanıt yük raporları bir Service Fabric bölüm listesi içeriyor. Her rapor yük ölçüm adı, değer ve son bildirilen zamanı UTC içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-load-reset"></a>sfctl bölüm yükünü sıfırlama
Geçerli iş yükünü bir Service Fabric bölümü sıfırlar.

Varsayılan yükleme hizmeti için geçerli iş yükünü bir Service Fabric bölümü sıfırlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl bölüm çekirdek zarar
Çekirdek kayıp verilen durum bilgisi olan hizmet bölümü uygulanmasını.

Çekirdek kayıp verilen durum bilgisi olan hizmet bölümü uygulanmasını.  Bu API, hizmetiniz bir geçici çekirdek kayıp durumunuza için kullanışlıdır. Bu API'si ile çalışmaya işlemler hakkında bilgi döndürmek için aynı Operationıd GetQuorumLossProgress API'SİYLE çağırın. Bu durum bilgisi olan üzerinde çağrılabilir kalıcı (HasPersistedState true ==) Hizmetleri.  Bu API, durum bilgisi olmayan hizmetler veya durum bilgisi olan bellek içi yalnızca hizmetler kullanmayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Çekirdek-kayıp-süre [gerekli] | Çekirdek kaybında bölüm tutulacak süre miktarı.  Saniye cinsinden belirtilen gerekir. |
| --Çekirdek kayıp-[gerekli] modu | Bu Sıralayıcı, ne tür bir çekirdek kayıp anlamına belirtmek için StartQuorumLoss API'sine geçirilir. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-quorum-loss-status"></a>sfctl bölüm çekirdek-kayıp-Durum
Çekirdek kayıp işlemin StartQuorumLoss API kullanmaya bir bölüme ilerlemesini alır.

Sağlanan Operationıd kullanarak StartQuorumLoss ile başlatılan bir çekirdek kayıp işlemin ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-recover"></a>sfctl bölüm Kurtar
Service Fabric kümesi şu anda çekirdek kaybında takıldı belirli bir bölüm kurtarmayı denemesi belirtir.

Service Fabric kümesi şu anda çekirdek kaybında takıldı belirli bir bölüm kurtarmayı denemesi belirtir. Bu işlem, yalnızca kapalı çoğaltmaları kurtarılamıyor biliniyorsa gerçekleştirilmelidir. Bu API yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-recover-all"></a>sfctl bölüm tüm Kurtar
Service Fabric kümesi şu anda çekirdek kaybında takıldı (Sistem Hizmetleri dahil) tüm hizmetleri kurtarmayı denemesi belirtir.

Service Fabric kümesi şu anda çekirdek kaybında takıldı (Sistem Hizmetleri dahil) tüm hizmetleri kurtarmayı denemesi belirtir. Bu işlem, yalnızca kapalı çoğaltmaları kurtarılamıyor biliniyorsa gerçekleştirilmelidir. Bu API yanlış kullanımı, olası veri kaybına neden olabilir.

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

## <a name="sfctl-partition-report-health"></a>sfctl bölüm rapor-durum
Service Fabric bölüm üzerinde bir sistem durumu raporu gönderir.

Belirtilen Service Fabric bölüm sistem durumunu raporlar. Rapor kaynağı özelliği üzerinde bildirilir ve sistem durumu raporu hakkında bilgiler içermelidir. Rapor bir Service Fabric ağ geçidi sistem durumu depoya iletir bölüm gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sonra ek doğrulama durumu mağaza tarafından reddedildi. Örneğin, sistem sağlığı deposunu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporu olayları bölümünde görünür denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Sistem durumu özelliği [gerekli] | Sistem durumu bilgisi özelliği. <br><br> Bir varlığın farklı özellikler için sistem durumu raporlarının olabilir. , Bir dize ve rapor tetikler durumu koşulu kategorilere ayırmak Raporlayıcı esnekliğini tanımak için olmayan bir sabit numaralandırması özelliğidir. Örneğin, bunu "AvailableDisk" özelliği bu düğümde raporlamak için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı Raporlayıcı düğümü bağlantı izleyebilirsiniz. Health store içinde bu raporları ayrı sistem durumu olayları için belirtilen düğümün olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgilerini benzersiz şekilde tanımlar. |
| --Sistem durumu [gerekli] | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Uyarı', 'Error', 'Bilinmeyen'. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --Kaynak Kimliği [gerekli] | Sistem durumu bilgisi oluşturulan izleme/istemci/sistem bileşeni tanımlayan kaynak adı. |
| --açıklaması | Sistem durumu bilgisi açıklaması. <br><br> Bu rapor hakkında İnsan okunabilir bilgi eklemek için kullanılan serbest metin temsil eder. Açıklama en çok dize uzunluğu 4096 karakter olabilir. Otomatik olarak sağlanan dize uzunsa kesilir. Kesildi, son açıklama işareti "[kesildi]" karakterinden ve toplam dize boyutu 4096 karakter olabilir. İşaretin durumunu kullanıcılara bu kesme gösteren oluştu. Kesirli kısmı, açıklama özgün dizesinden değerinden 4096 karakter olduğuna dikkat edin. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini gösteren bir bayrak. <br><br> Sistem Durumu raporu bir Service Fabric ağ geçidi sistem durumu depoya iletir uygulama gönderilir. Hemen ayarlanmışsa true, rapor hemen HTTP ağ geçidi uygulaması kullanarak doku istemci ayarlarına bakılmaksızın sistem durumu deposuna HTTP geçidinden gönderilir. Mümkün olan en kısa sürede gönderilmesi gereken kritik raporlar için kullanışlıdır. Zamanlama ve diğer koşullara bağlı olarak, raporu göndermeden hala, örneğin HTTP ağ geçidi kapalı veya ağ geçidi ileti ulaşmak değil başarısız olabilir. Hemen false olarak ayarlanırsa, rapor sistem istemci ayarlarına HTTP ağ geçidi'nden göre gönderilir. Bu nedenle, onu HealthReportSendInterval yapılandırmasına göre toplu hale. Sistem durumu iletileri durum deposu ve bunun yanı sıra sistem durumu raporu işleme raporlama iyileştirmek sistem durumu istemci izin verdiği için Önerilen ayar budur. Varsayılan olarak, raporları hemen gönderilmez. |
| --Kaldır zaman süresi | Süresi dolduğunda, sistem durumu deposundan rapor kaldırılmış olup olmadığını belirten değer. <br><br> Bunu süresi dolduktan sonra sistem durumu deposundan true olarak rapor kaldırılırsa. Raporun false olarak ayarlanırsa dolduğunda hata kabul edilir Bu özellik varsayılan olarak false değeridir. İstemcileri düzenli aralıklarla bildirdiğinde RemoveWhenExpired false (varsayılan) ayarlamanız gerekir. Bu şekilde Raporlayıcı sorunları (örneğin kilitlenme) var ve rapor veremez, sistem durumu raporu sona erdiğinde varlık hatasında değerlendirilir olur. Bu varlık sistem durumu hatası olarak işaretler. |
| --sıra numarası | Bu sistem durumu raporu sayısal dize olarak sıra numarası. <br><br> Rapor sıra numarası, eski raporları algılamak için sistem durumu mağaza tarafından kullanılır. Bir rapor eklendiğinde belirtilmezse, bir sıra numarası sistem durumu istemcisi tarafından otomatik olarak oluşturulan. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --ttl | Bu sistem durumu raporu geçerli olduğu süre. Bu alan, süresi belirtmek için ISO8601 biçimini kullanıyor. <br><br> İstemcileri düzenli aralıklarla bildirdiğinde, bunlar yaşam süresi'den daha yüksek sıklıkta raporları göndermesi gerekir. İstemcileri geçişi rapor varsa bunlar için sonsuz yaşam süresini ayarlayabilirsiniz. Yaşam süresi dolduğunda, sistem durumu bilgilerini içeren sistem durumu olayı RemoveWhenExpired ise, sistem durumu Mağazası'ndan kaldırılmış ya da doğru veya hatada, hesaplanan ise RemoveWhenExpired false. Aksi durumda belirtilen, süresi sonsuz değeri varsayılanlara. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-restart"></a>sfctl bölümü yeniden başlatma
Bu API, bazı veya tüm çoğaltmaları ya da belirtilen bölüm örneklerini yeniden başlatılır.

Bu API, yük devretme test etmek için kullanışlıdır. Durum bilgisiz hizmet bölüm hedeflemek için kullanılan RestartPartitionMode AllReplicasOrInstances olması gerekir. İlerleme durumunu almak için aynı Operationıd kullanarak GetPartitionRestartProgress API çağrısı.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --yeniden bölüm-[gerekli] modu | Yeniden başlatmak için hangi bölümleri açıklanmaktadır. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-restart-status"></a>sfctl bölümü yeniden başlatma durumu
StartPartitionRestart kullanmaya PartitionRestart işlemin ilerlemesini alır.

Sağlanan Operationıd kullanarak StartPartitionRestart ile başlatılan bir PartitionRestart ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-partition-svc-name"></a>sfctl bölüm svc-adı
Bir bölüm için Service Fabric hizmet adını alır.

Belirtilen bölüm için hizmetin adını alır. Bölüm kimliği kümedeki yoksa 404 hatası döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli] | Bölüm kimliği. |
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
