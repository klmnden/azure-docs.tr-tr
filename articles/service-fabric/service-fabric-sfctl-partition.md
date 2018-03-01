---
title: "Azure Service Fabric CLI - sfctl bölüm | Microsoft Docs"
description: "Service Fabric CLI sfctl bölüm komutlarını açıklar."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 01dd1900fe765618e5da20bd289b9c3a021ea9a3
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-partition"></a>sfctl partition
Sorgulamak ve herhangi bir hizmet için bölüm yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    veri kaybı      | Bu API belirtilen bölüm için veri kaybı uygulanmasını.|
|    veri kaybı durumu  | StartDataLoss API kullanmaya bir bölüm veri kaybı işlemin ilerlemesini alır.|
|    sistem durumu         | Belirtilen Service Fabric bölüm durumunu alır.|
|    bilgileri           | Service Fabric bölüm hakkında bilgi alır.|
|    liste           | Service Fabric hizmetinin bölümleri listesini alır.|
|    yükleme           | Belirtilen Service Fabric bölüm yükünü alır.|
|    Yük-sıfırlama     | Geçerli iş yükünü bir Service Fabric bölümü sıfırlar.|
|    Çekirdek kayıp    | Çekirdek kayıp verilen durum bilgisi olan hizmet bölümü uygulanmasını.|
|    Çekirdek kayıp durumu| Çekirdek kayıp işlemin StartQuorumLoss API kullanmaya bir bölüme ilerlemesini alır.|
|    Kurtarma        | Service Fabric kümesi şu anda çekirdek kaybında takıldı belirli bir bölüme kurtarmayı denemesi belirtir.|
|    tüm kurtarma    | Service Fabric kümesi şu anda çekirdek kaybında takıldı (Sistem Hizmetleri dahil) tüm hizmetleri kurtarmayı denemesi belirtir.|
|    Sistem Durumu raporu  | Service Fabric bölüm üzerinde bir sistem durumu raporu gönderir.|
|    Yeniden başlatma        | Bu API, bazı veya tüm çoğaltmaları ya da belirtilen bölüm örneklerini yeniden başlatır.|
|    yeniden başlatma durumu | StartPartitionRestart kullanmaya PartitionRestart işlemin ilerlemesini alır.|
|    svc-name       | Bir bölüm için Service Fabric hizmet adını alır.|


## <a name="sfctl-partition-health"></a>sfctl bölüm sistem durumu
Belirtilen Service Fabric bölüm durumunu alır.

Belirtilen bölüm sistem durumu bilgilerini alır. Sistem durumu olayları sistem durumuna bağlı hizmet bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın.
ReplicasHealthStateFilter bölüme ReplicaHealthState nesne koleksiyonundaki filtre uygulamak için kullanın. Health store içinde yok. bir bölüm belirtirseniz, bu cmdlet bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --events-health-state-filter  | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir.                Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.                -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre.                Değer, 65535 ' dir.|
|--exclude-health-statistics   | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir.|
| --replicas-health-state-filter| Bölüm ReplicaHealthState nesneleri koleksiyonu filtrelemeye izin verir. Değer üyeleri veya HealthStateFilter üyeleri üzerinde bit düzeyinde işlemler alınamıyor. Filtreyle eşleşen çoğaltmaları döndürülür. Tüm çoğaltmaları toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --zaman aşımı -t               | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                    | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                  | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                Varsayılan: json.|
| --Sorgu                    | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın. |
| --verbose                  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-partition-info"></a>sfctl bölüm bilgileri
Service Fabric bölüm hakkında bilgi alır.

Bölümler son nokta belirtilen bölüm hakkında bilgi döndürür. Yanıt, bölüm kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem ve bölüm ile ilgili diğer ayrıntıları tarafından desteklenen anahtarlarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-partition-list"></a>sfctl bölüm listesi
Service Fabric hizmetinin bölümleri listesini alır.

Service Fabric hizmetinin bölümleri listesini alır. S bölüm kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem ve bölüm ile ilgili diğer ayrıntıları tarafından desteklenen anahtarları.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Hizmet adı; Örneğin, "fabric: / myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam ~ app1 ~ svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde.|
| --devamlılık belirteci| Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır.         Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --zaman aşımı -t        | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug             | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h           | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı         | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu             | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose           | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-partition-load"></a>sfctl bölüm yükü
Belirtilen Service Fabric bölüm yükünü alır.

Belirtilen bölüm hakkında bilgi döndürür. Yanıt yük bilgilerin listesini içerir. Her yük ölçüm adı, değer ve son bildirilen zamanı UTC bilgilerdir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-partition-recover"></a>sfctl bölüm Kurtar
Service Fabric kümesi şu anda çekirdek kaybında takıldı belirli bir bölüm kurtarmayı denemesi belirtir.

Service Fabric kümesi şu anda çekirdek kaybında takıldı belirli bir bölüm kurtarmayı denemesi belirtir. Bu işlem, yalnızca kapalı çoğaltmaları kurtarılamıyor biliniyorsa gerçekleştirilmelidir. Bu API yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-partition-restart"></a>sfctl bölümü yeniden başlatma
Bu API, bazı veya tüm çoğaltmaları ya da belirtilen bölüm örneklerini yeniden başlatır.

Bu API, yük devretme test etmek için kullanışlıdır. Durum bilgisiz hizmet bölüm hedeflemek için kullanılan RestartPartitionMode AllReplicasOrInstances olması gerekir. İlerleme durumunu almak için aynı Operationıd kullanarak GetPartitionRestartProgress API çağrısı.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli]| Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir.|
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --yeniden bölüm-[gerekli] modu| Yeniden başlatmak için hangi bölümleri açıklanmaktadır.|
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Hizmet adı; Örneğin, "fabric: / myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam ~ app1 ~ svc1" 6.0 + ve "myapp/app1/svc1" önceki ve rsions.|
| --zaman aşımı -t                    | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                         | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                       | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                     | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                     Varsayılan: json.|
| --Sorgu                         | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose                       | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).
