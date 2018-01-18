---
title: Azure Service Fabric CLI sfctl hizmet | Microsoft Docs
description: "Service Fabric CLI sfctl hizmeti komutlarını açıklar."
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
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 5c1f485812918397b5b52e650611032c9058e3ee
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-service"></a>sfctl hizmeti
Oluşturma, silme ve hizmet, hizmet türlerini ve hizmet paketleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    Uygulama adı       | Bir hizmet için Service Fabric uygulamanın adını alır.|
|    code-package-list | Service Fabric düğümde dağıtılan kod paketlerin listesini alır.|
|    oluşturmaya         | Belirtilen Service Fabric hizmeti açıklamasından oluşturur.|
|    sil         | Var olan bir Service Fabric hizmeti siler.|
|    dağıtılan türü  | Service Fabric kümesindeki bir düğümde dağıtılan uygulamayı belirtilen hizmet türü hakkındaki bilgileri alır.|
|    deployed-type-list| Service Fabric kümesindeki bir düğümde dağıtılan uygulamalardan hizmet türleri hakkında bilgi içeren listeyi alır.|
|    açıklama    | Var olan bir Service Fabric hizmetini açıklamasını alır.|
|    sistem durumu         | Belirtilen Service Fabric hizmet durumunu alır.|
|    bilgileri           | Bir Service Fabric uygulamaya ait belirli hizmet hakkındaki bilgileri alır.|
|    liste           | Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetler hakkındaki bilgileri alır|
|    Bildirimi       | Bir hizmet türünün açıklayan bildirimi alır.|
|    Paketi dağıtma | Belirtilen hizmet bildirimi belirtilen düğümün görüntüsünü önbelleğine ilişkili paketleri yükler.|
|    package-health | Bir Service Fabric düğümü ve uygulama için dağıtılan belirli bir uygulama için bir hizmet paketi sistem durumu bilgilerini alır.|
|    package-info   | Tam olarak belirtilen adla eşleşen bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır.|
|    Paket listesi   | Service Fabric düğümde dağıtılan hizmet paketleri listesini alır.|
|    Kurtarma        | Service Fabric kümesi şu anda çekirdek kaybında takıldı belirtilen hizmet kurtarmayı denemesi belirtir.|
|    Sistem Durumu raporu  | Sistem Durumu raporu Service Fabric hizmetine gönderir.|
|    çöz        | Service Fabric bölümü çözümleyin.|
|    tür listesi      | Service Fabric kümesi içinde sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkında bilgi içeren listeyi alır.|
|    Güncelleştirme         | Belirtilen güncelleştirme açıklamasını kullanarak belirtilen hizmeti güncelleştirir.|


## <a name="sfctl-service-create"></a>sfctl hizmet oluşturma
Belirtilen Service Fabric hizmeti açıklamasından oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Üst uygulama kimliği. Bu genellikle tam uygulamayı olmadan kimliğidir ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile sınırlandırılmıştır ' ~' karakter. Örneğin, uygulama adı 'fabric://myapp/app1' ise, uygulama kimliği olması ' Uygulamam ~ app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki.|
| --Ad [gerekli]| Hizmetin adı. Bu bir alt uygulama kimliği olmalıdır           Bu tam olduğu da dahil olmak üzere `fabric:` URI. Örneğin hizmet `fabric:/A/B` uygulama alt `fabric:/A`.|
| --Hizmet türü [gerekli]| Hizmet türünün adı.|
| --activation-mode     | Hizmet Paketi için etkinleştirme modu.|
| --kısıtlamaları         | Dize olarak yerleştirme kısıtlamaları. Kısıtlamalarından düğüm özellikleri boolean ifadeleri ve hizmet gereksinimlerine bağlı olarak belirli düğümler için bir hizmet sınırlamak için izin veren. Örneğin, yerleştirmek için bir hizmet NodeType olduğu mavi düğümlerde belirtin aşağıdaki: "NodeColor mavi ==".|
| --correlated-service  | İle ilişkilendirmek için hedef hizmet adı.|
| --Bağıntı         | Hizmet hizalama benzeşim kullanarak var olan bir hizmeti ile ilişkilendirilmesi.|
| --dns-name            | Oluşturulacak hizmetin DNS adı. Service Fabric DNS sistem hizmeti bu ayarın etkinleştirilmesi gerekir.|
| --örnek sayısı      | Örnek sayısı. Bu, yalnızca durum bilgisi olmayan hizmetler için geçerlidir.|
| --int düzeni          | Hizmet hep bir işaretsiz tamsayı aralığı bölümlenmiş gösterir.|
| --int düzeni sayısı    | Bölüm içindeki tamsayı aralığı (bir Tekdüzen tamsayı bölüm düzeni) oluşturmak için anahtar.|
| --int düzeni yüksek     | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, anahtar tamsayı aralığı sonu.|
| --int düzeni düşük      | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, anahtar tamsayı aralığı başlangıcı.|
| --Yük ölçümleri        | JSON olarak kodlanmış düğümleri arasında Yük Dengeleme Hizmetleri zaman ölçülerine listesi.|
| --min-replica-set-size| En küçük çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --Taşıma maliyeti           | Hizmeti için taşıma maliyeti belirtir. Olası değerler şunlardır: 'Sıfır', 'Düşük', 'Orta', 'Yüksek'.|
| --named-scheme        | Hizmet birden çok adlandırılmış bölüm olmayacağını gösterir.|
| --named-scheme-list   | JSON olarak kodlanmış adlandırılmış bölüm düzeni kullanıyorsanız, üzerinde hizmet bölüm adları listesi.|
| --yok kalıcı durumu  | TRUE ise, bu hizmet yerel diskte depolanan hiçbir kalıcı durumuna sahip veya yalnızca bellek durumu depolar belirtir.|
| --yerleştirme ilke listesi  | Hizmeti için yerleşim ilkeleri listesi JSON kodlanmış ve ilişkilendirilen tüm etki alanı adları. İlkeleri, bir veya daha fazla olabilir: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`.|
| --Çekirdek kayıp bekleyin    | Kendisi için bir bölüm çekirdek kayıp durumda olmasına izin verilen saniye cinsinden en uzun süresi. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --replica-restart-wait| Bir çoğaltma gittiğinde ve yeni bir kopya oluşturulduğunda arasındaki saniye cinsinden süre. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --singleton düzeni    | Hizmet tek bir bölüm veya sahip olmayan bölümlenmiş hizmet gösterir.|
| --yedek tarafından çoğaltma koru  | Saniye cinsinden en uzun süresi kaldırılmadan önce çoğaltmaları için hangi bekleme saklanır. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --durum bilgisi olan            | Hizmet durum bilgisi olan hizmet gösterir.|
| --Durum bilgisiz           | Bir durum bilgisi olmayan hizmetin hizmet olduğunu gösterir.|
| --target-replica-set-size| Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-delete"></a>sfctl service delete
Var olan bir Service Fabric hizmeti siler.

Var olan bir Service Fabric hizmeti siler. Bir hizmet silinebilmesi için önce oluşturulması gerekir. Varsayılan olarak, normal bir şekilde hizmet çoğaltmaları kapatmak ve hizmeti silmek Service Fabric dener. Ancak hizmet çoğaltma düzgün biçimde kapatılması sorunları yaşıyorsa, silme işlemi uzun bir süre devam edebilir veya takılı. Normal kapatma sırası atlayıp zorla hizmeti silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı fabric://myapp/app1/svc1 ise ", hizmet kimliği olacaktır" Uygulamam ~ app1 ~ svc1 "6.0 + ve" myapp/app1/svc1"önceki sürümlerinde.|
| --zorla Kaldır      | Bir Service Fabric uygulaması veya hizmeti zorla kapama sırası geçmeden kaldırın. Bu parametre zorla bir uygulamayı silmek için kullanılan veya hizmet için hangi silmeyi zaman aşımına uğramadan engelleyen hizmet kodda sorunları nedeniyle normal olduğundan kopyaları kapatın.|
| --zaman aşımı -t        | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug             | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h           | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı         | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu             | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose           | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-description"></a>sfctl hizmet açıklaması
Var olan bir Service Fabric hizmetini açıklamasını alır.

Var olan bir Service Fabric hizmetini açıklamasını alır. Bir hizmet açıklamasını elde edilebilir önce oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı "fabric://myapp/app1/svc1" ise, hizmet kimliği olması "Uygulamam ~ app1 ~ svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde.|
| --zaman aşımı -t        | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug             | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h           | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı         | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu             | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose           | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-health"></a>sfctl hizmet durumu
Belirtilen Service Fabric hizmet durumunu alır.

Belirtilen hizmet sistem durumu bilgilerini alır. Sistem durumu olayları sistem durumuna bağlı hizmet bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Döndürülen bölüm koleksiyonu filtrelemek için PartitionsHealthStateFilter kullanın. Health store içinde yok. bir hizmeti belirtirseniz, bu cmdlet bir hata döndürür. .

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı "fabric://myapp/app1/svc1" ise, hizmet kimliği olması "Uygulamam ~ app1 ~ svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde.|
| --events-health-state-filter | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
|--exclude-health-statistics     | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir.|
| --bölümleri sağlık Durumu Filtresi| Sistem sağlığı durumlarına bağlı hizmet sistem durumu sorgusunun sonucu döndürdü bölümleri sistem durumu nesnelerini filtreleme sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen bölümleri döndürülür. Tüm bölümleri toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, "6" sağlanan değer ise, ardından Tamam (2) ve uyarı (4), HealthState değeriyle bölümlerinin sistem durumu döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir.                  Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --zaman aşımı -t                 | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                      | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                    | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                  | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                  Varsayılan: json.|
| --Sorgu                      | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose                    | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-info"></a>sfctl service info
Bir Service Fabric uygulamaya ait belirli hizmet hakkındaki bilgileri alır.

Belirtilen Service Fabric uygulamaya ait belirtilen hizmeti hakkında bilgi döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı "fabric://myapp/app1" ise, uygulama kimliği olması "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde.|
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı "fabric://myapp/app1/svc1" ise, hizmet kimliği olması "Uygulamam ~ app1 ~ svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde.|
| --zaman aşımı -t            | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-list"></a>sfctl hizmet listesi
Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetler hakkındaki bilgileri alır

Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetleri hakkında bilgi döndürür

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı "fabric://myapp/app1" ise, uygulama kimliği olması "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde.|
| --devamlılık belirteci    | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --Hizmet türü adı     | Hizmetler için sorgu filtre uygulamak için kullanılan hizmet türü adı.|
| --zaman aşımı -t            | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-manifest"></a>sfctl service manifest
Bir hizmet türünün açıklayan bildirimi alır.

Bir hizmet türünün açıklayan bildirimi alır. Yanıt olarak bir dize hizmet bildirimi XML içeriyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli]| Uygulama türünün adı.|
| --Uygulama-type-version [gerekli]| Uygulama türü sürümü.|
| --service-bildirimi-name [gerekli]| Uygulama türü bir Service Fabric kümesindeki bir parçası olarak kayıtlı bir hizmet bildirimi adı.|
| --zaman aşımı -t                      | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                       Varsayılan: json.|
| --Sorgu                           | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose                         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-recover"></a>sfctl hizmet Kurtar
Service Fabric kümesi şu anda çekirdek kaybında takıldı belirtilen hizmet kurtarmayı denemesi belirtir.

Service Fabric kümesi şu anda çekirdek kaybında takıldı belirtilen hizmet kurtarmayı denemesi belirtir. Kapalı çoğaltmaları kurtarılamazsa, bu işlem yalnızca gerçekleştirilmesi gerekir. Bu API yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı fabric://myapp/app1/svc1 ise ", hizmet kimliği olacaktır" Uygulamam ~ app1 ~ svc1 "6.0 + ve" myapp/app1/svc1"önceki sürümlerinde.|
| --zaman aşımı -t        | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug             | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h           | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı         | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu             | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose           | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-resolve"></a>sfctl hizmet Çöz
Service Fabric bölümü çözümleyin.

Hizmet çoğaltmaları uç noktalarına almak için Service Fabric hizmeti bölüm çözümleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı "fabric://myapp/app1/svc1" ise, hizmet kimliği olması "Uygulamam ~ app1 ~ svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde.|
| --Bölüm anahtarı türü| Bölüm için anahtar türü. Hizmet bölüm düzeni Int64Range veya adlandırılmış ise bu parametre gereklidir. Olası değerler aşağıdaki. -Yok (1) - gösterir PartitionKeyValue parametresi belirtilmedi. Bu, bölümleme düzeni Singleton olarak ile bölümler için geçerlidir. Varsayılan değer budur. Değer 1'dir. -Int64Range (2) - PartitionKeyValue parametresi bir Int64 bölüm anahtarı olduğunu gösterir. Bu, bölümleme düzeni olarak Int64Range ile bölümler için geçerlidir. Değer 2'dir. -(3) - adlı PartitionKeyValue parametresi bölümün adı olduğunu gösterir. Bu, bölümleme düzeni olarak adlandırılmış ile bölümler için geçerlidir. Değer 3'tür.|
| --Bölüm anahtarı değeri  | Bölüm anahtarı. Hizmet bölüm düzeni Int64Range veya adlandırılmış ise, bu gereklidir.|
| --rsp sürüm önceki | Daha önce alındı yanıtının sürüm alanındaki değer. Kullanıcı var sonuç daha önce eski biliyorsa, bu gereklidir.|
| --zaman aşımı -t        | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug             | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h           | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı         | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu             | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose           | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-service-update"></a>sfctl hizmet güncelleştirmesi
Belirtilen güncelleştirme açıklamasını kullanarak belirtilen hizmeti güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli]| Hizmetinin hedefi. Bu genellikle tam hizmeti olmadan kimliğidir ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, hizmet adı 'fabric://myapp/app1/svc1' ise, hizmet kimliği olması ' Uygulamam ~ app1 ~ svc1' 6.0 + ve ' myapp/app1/svc1' önceki sürümlerinde.|
| --kısıtlamaları         | Dize olarak yerleştirme kısıtlamaları. Kısıtlamalarından düğüm özellikleri boolean ifadeleri ve hizmet gereksinimlerine bağlı olarak belirli düğümler için bir hizmet sınırlamak için izin veren. Örneğin, yerleştirmek için bir hizmet NodeType olduğu mavi düğümlerde belirtin aşağıdaki: "NodeColor mavi ==".|
| --correlated-service  | İle ilişkilendirmek için hedef hizmet adı.|
| --Bağıntı         | Hizmet hizalama benzeşim kullanarak var olan bir hizmeti ile ilişkilendirilmesi.|
| --örnek sayısı      | Örnek sayısı. Bu, yalnızca durum bilgisi olmayan hizmetler için geçerlidir.|
| --Yük ölçümleri        | Kullanılan ölçümleri JSON olarak kodlanmış listesi Yük Dengeleme düğümü arasında.|
| --min-replica-set-size| En küçük çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --Taşıma maliyeti           | Hizmeti için taşıma maliyeti belirtir. Olası değerler şunlardır: 'Sıfır', 'Düşük', 'Orta', 'Yüksek'.|
| --yerleştirme ilke listesi  | Hizmeti için yerleşim ilkeleri listesi JSON kodlanmış ve ilişkilendirilen tüm etki alanı adları. İlkeleri, bir veya daha fazla olabilir: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`.|
| --Çekirdek kayıp bekleyin    | Kendisi için bir bölüm çekirdek kayıp durumda olmasına izin verilen saniye cinsinden en uzun süresi. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --replica-restart-wait| Bir çoğaltma gittiğinde ve yeni bir kopya oluşturulduğunda arasındaki saniye cinsinden süre. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --yedek tarafından çoğaltma koru  | Saniye cinsinden en uzun süresi kaldırılmadan önce çoğaltmaları için hangi bekleme saklanır. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --durum bilgisi olan            | Hedef hizmet durum bilgisi olan hizmet gösterir.|
| --Durum bilgisiz           | Hedef hizmet durumsuz bir hizmettir gösterir.|
| --target-replica-set-size| Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).