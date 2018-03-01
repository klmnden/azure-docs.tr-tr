---
title: Azure Service Fabric CLI sfctl uygulama | Microsoft Docs
description: "Service Fabric CLI sfctl uygulama komutlarını açıklar."
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
ms.date: 02/23/2018
ms.author: ryanwi
ms.openlocfilehash: 3a10437d0a2d680e586ada6a87750a69453c1f0c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-application"></a>sfctl application
Oluşturma, silme ve uygulama ve uygulama türlerini yönetme.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya       | Belirtilen açıklaması kullanarak bir Service Fabric uygulaması oluşturur.|
| sil       | Var olan bir Service Fabric uygulamasını siler.|
| Dağıtılan     | Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.|
| dağıtılan durumu | Service Fabric düğümde dağıtılan bir uygulama sistem durumu bilgilerini alır.|
| dağıtılan listesi| Service Fabric düğümde dağıtılan uygulamalar listesini alır.|
| sistem durumu       | Service fabric uygulaması durumunu alır.|
| bilgileri         | Service Fabric uygulaması hakkındaki bilgileri alır.|
| liste         | Parametre olarak belirtilen filtrelerle eşleşen Service Fabric kümesi içinde oluşturulan uygulamaların listesini alır.|
| yükleme | Yük Service Fabric uygulaması hakkında bilgileri alır. |
| Bildirimi     | Bir uygulama türünü tanımlayan bildirimi alır.|
| Sağlama    | Hükümler veya yazmaçlar Service Fabric uygulaması yazın .sfpkg paket dış mağazada veya uygulama paketi görüntü deposuna kullanarak küme ile.|
| Sistem Durumu raporu| Service Fabric uygulaması bir sistem durumu raporu gönderir.|
| type         | Uygulama türleri listesini tam olarak belirtilen adla eşleşen Service Fabric kümesi içinde alır.|
| tür listesi    | Service Fabric kümesi uygulama türlerinin bir listesini alır.|
| Sağlamayı kaldırma  | Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler.|
| Yükseltme      | Service Fabric kümesi uygulamada yükseltmeyi başlatır.|
| upgrade-resume  | Service Fabric kümesi uygulamada yükseltme devam eder.|
| upgrade-rollback| Uygulama şu anda devam eden yükseltmesini Service Fabric kümesi geri başlatır.|
| Yükseltme durumu  | Bu uygulamaya yapılan en son yükseltme için ayrıntıları alır.|
| Karşıya yükleme       | Service Fabric uygulama paketi görüntü deposuna kopyalama.|

## <a name="sfctl-application-create"></a>sfctl uygulaması oluşturma
Belirtilen açıklaması kullanarak bir Service Fabric uygulaması oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulamanızın-adı [gerekli]| Uygulamanın adı dahil olmak üzere ' doku:' URI düzeni.|
| --uygulama türü [gerekli]| Uygulama türü adı uygulama bildiriminde bulundu.|
| --app sürümlü [gerekli]| Uygulama bildiriminde tanımlandığı gibi uygulama türü sürümü.|
| --max-node-count     | Service Fabric bu uygulama için kapasite ayırdığını düğüm maksimum sayısı. Bu, bu uygulama hizmetleri tüm düğümleri yerleştirilir gelmez.|
| --Ölçümleri            | JSON uygulama kapasite ölçüm açıklamalarını kodlanmış. Ölçüm uygulama var. her düğüm için kapasiteler kümesi ile ilişkili bir ad olarak tanımlanır.|
| --min düğüm sayısı     | Düğüm sayısı alt sınırı Service Fabric bu uygulama için kapasite ayırır. Bu, bu uygulama hizmetleri tüm düğümleri yerleştirilir gelmez.|
| --Parametreler         | JSON olarak kodlanmış listesini uygulama parametresi, uygulama oluştururken uygulanacak geçersiz kılar.|
| --zaman aşımı -t         | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug              | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h            | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı          | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu              | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose            | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-delete"></a>sfctl uygulama silme
Var olan bir Service Fabric uygulamasını siler.

Var olan bir Service Fabric uygulamasını siler. Uygulamanın silinebilmesi için önce oluşturulması gerekir. Bir uygulamayı sildiğinizde, uygulamanın parçası olan tüm hizmetleri siler. Varsayılan olarak, normal bir şekilde hizmet çoğaltmaları kapatmak ve hizmeti silmek Service Fabric dener. Ancak hizmet çoğaltma düzgün biçimde kapatılması sorunları yaşıyorsa, silme işlemi uzun bir süre devam edebilir veya takılı. Normal kapatma sırası atlayın ve uygulama ve hizmetlerinin tümünün zorla silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı ise "fabric: / myapp/app1", uygulama kimliği olacaktır "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde.|
| --zorla Kaldır          | Bir Service Fabric uygulaması veya hizmeti zorla kapama sırası geçmeden kaldırın. Bu parametre zorla bir uygulamayı silmek için kullanılan veya hizmet için hangi silmeyi zaman aşımına uğramadan engelleyen hizmet kodda sorunları nedeniyle normal olduğundan kopyaları kapatın.|
| --zaman aşımı -t            | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-deployed"></a>dağıtılan sfctl uygulaması
Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.

Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.  Sağlanan uygulama kimliği sistem uygulaması için bu sorguyu sistem uygulama bilgilerini döndürür. Sonuçları etkinleştirme ve durumlarını indirme etkin, dağıtılan uygulamalar kapsar. Bu sorgu düğümü adı küme üzerinde bir düğüme karşılık gelen gerektiriyor. Sağlanan düğüm adı tüm küme etkin Service Fabric düğümlerinde işaret etmiyor, sorgu başarısız olur.
     
### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı ise "fabric: / myapp/app1", uygulama kimliği olacaktır "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde.|
| --düğüm adı [gerekli]| Düğümün adı.|
| --zaman aşımı -t            | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-health"></a>sfctl uygulama durumu
Service fabric uygulaması durumunu alır.

Service fabric uygulaması sistem durumu durumunu döndürür. Yanıt Tamam, hata veya uyarı sistem durumu raporları. Varlık health store içinde bulunmazsa hata verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı ise "fabric: / myapp/app1", uygulama kimliği olacaktır "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde.|
| --dağıtılan uygulamalar-sistem durumu-Durumu-Filtresi| Dağıtılan uygulamaları sistem durumu nesnelerini filtreleme kendi sistem durumuna bağlıdır uygulama sistem durumu sorgusunun sonucu döndürdü sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen dağıtılan uygulamalar döndürülür. Tüm dağıtılmış uygulamalar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, 6 sağlanan değer ise, ardından Tamam (2) ve uyarı (4), HealthState değeriyle dağıtılan uygulamalar, sistem durumu döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --events-health-state-filter            | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --exclude-health-statistics | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir.|
| --services-health-state-filter          | Kendi sistem durumuna bağlı Hizmetleri sistem durumu sorgusu sonucu döndürdü Hizmetleri sistem durumu nesnelerini filtreleme sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen Hizmetleri döndürülür. Tüm hizmetler toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından Tamam (2) ve uyarı (4), HealthState değeriyle sistem durumu hizmetlerinin döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --zaman aşımı -t                            | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                                 | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
| --verbose                               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-info"></a>sfctl uygulama bilgisi
Service Fabric uygulaması hakkındaki bilgileri alır.

Service Fabric kümesi ve parametre olarak belirtilen bir adı eşleşen oluşturulmakta sürecinde veya oluşturulmuş uygulama hakkında bilgi döndürür. Yanıt adı, türü, durumu, parametreleri ve uygulama ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı ise "fabric: / myapp/app1", uygulama kimliği olacaktır "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde.|
| --exclude-application-parameters| Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak.|
| --zaman aşımı -t                 | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                      | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                    | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                  | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.             Varsayılan: json.|
| --Sorgu                      | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
| --verbose                    | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-list"></a>sfctl uygulama listesi
Parametre olarak belirtilen filtrelerle eşleşen Service Fabric kümesi içinde oluşturulan uygulamaların listesini alır.

Oluşturulan veya Service Fabric oluşturulan sürecinde küme ve parametre olarak belirtilen filtrelerle eşleşen uygulamalar hakkındaki bilgileri alır. Yanıt adı, türü, durumu, parametreleri ve uygulama ile ilgili diğer ayrıntıları içerir. Uygulamaları bir sayfasında uymuyorsa sonuçlarının bir sayfa sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür. Filtreleri ApplicationTypeName ve ApplicationDefinitionKindFilter aynı anda belirtilemez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
|--application-definition-kind-filter| Service Fabric uygulaması tanımlamak için kullanılan mekanizma olan ApplicationDefinitionKind filtrelemek için kullanılır. -Varsayılan - varsayılan değer, "Tümü" seçerek olarak aynı işlevi gerçekleştirir. Değeri 0'dır. -Tüm - giriş herhangi bir ApplicationDefinitionKind değeri ile eşleşen filtre. Değer, 65535 ' dir. -ServiceFabricApplicationDescription - giriş ServiceFabricApplicationDescription ApplicationDefinitionKind değeriyle eşleşen Filtresi. Değer 1'dir. Giriş Oluştur ApplicationDefinitionKind değeriyle eşleşen - Oluştur - Filtresi. Değer 2'dir.|
| --uygulama türü adı      | Filtre için sorgulamak üzere uygulamalar için kullanılan uygulama türü adı. Bu değeri uygulama türü sürümü içermemesi gerekir.|
| --devamlılık belirteci         | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --exclude-application-parameters| Uygulama parametreleri sonucundan hariç tutulan olup olmadığını belirten bayrak.|
| --max-results|Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorguları gibi çok sayıda sonuç dönüş iletiye sığmayacak mümkün olduğunca içerir.|
| --zaman aşımı -t                 | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                      | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                    | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                  | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.             Varsayılan: json.|
| --Sorgu                      | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose                    | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-load"></a>sfctl uygulama yükleme
Yük Service Fabric uygulaması hakkında bilgileri alır.

Yükleme hakkında bilgi oluşturulmuş uygulama veya Service Fabric kümesi ve parametre olarak belirtilen bir adı eşleşen oluşturulmakta sürecinde döndürür. Yanıt adı, en az düğüm, en fazla düğüm sayısı, uygulama şu anda kullandığı düğümleri ve uygulama hakkında uygulama yük ölçüm bilgi sayısını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "~" karakter. Örneğin, uygulama adı ise "fabric: / myapp/app1", uygulama kimliği olacaktır "Uygulamam ~ app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t               | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--debug                    | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
    --help -h                  | Bu yardım iletisini ve çıkış gösterir.|
    ---o çıktı                | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
    --Sorgu                    | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
    --verbose                  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-manifest"></a>sfctl uygulama bildirimi
Bir uygulama türünü tanımlayan bildirimi alır.
        
Bir uygulama türünü tanımlayan bildirimi alır. Yanıt bir dize XML uygulama bildirimi içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli]| Uygulama türünün adı.|
| --Uygulama-type-version [gerekli]| Uygulama türü sürümü.|
| --zaman aşımı -t                      | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                  Varsayılan: json.|
| --Sorgu                           | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
| --verbose                         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-provision"></a>sfctl uygulama sağlama
Hükümler veya yazmaçlar Service Fabric uygulaması yazın SFPKG paket dış mağazada veya uygulama paketi görüntü deposuna kullanarak küme ile.

Bir küme Service Fabric uygulaması türüyle sağlar. Herhangi bir yeni uygulama örneğinin önce bu gereklidir. Sağlama işlemi ya da relativePathInImageStore veya dış SFPKG URI'sini kullanarak belirtilen uygulama paketinin üzerinde gerçekleştirilebilir. Bu komut dış sağlama ayarlanmadığı sürece--Image store bekliyor

sağlama.
        


### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --application-package-download-uri| Yolun nereden '.sfpkg' uygulama paketi için uygulama paketi HTTP veya HTTPS protokolleri kullanılarak yüklenebilir. Yalnızca bir dış depolama alanından sağlamak için. Uygulama paketi dosyasını karşıdan yüklemek için alma işlemi sağlayan bir dış mağazada depolanabilir. Desteklenen HTTP ve HTTPS kurallarıdır ve yolun okuma erişimine izin vermesi gerekir.|
| --uygulama türü derleme yolu       | Sağlama tür görüntü deposu için yalnızca. Önceki karşıya yükleme işlemi sırasında belirtilen Image store uygulama paketinde göreli yolu. |
| --uygulama türü adı| Yalnızca bir dış depolama alanından sağlamak için. Uygulama türü adı uygulama bildiriminde bulunan uygulama türü adını temsil eder.|
| --application-type-version| Yalnızca bir dış depolama alanından sağlamak için. Uygulama türü sürümü, uygulama bildiriminde bulunan uygulama türü sürümü temsil eder.|
| --Dış sağlama| Uygulama paketi burada kayıtlı veya sağlanan konumdaki. Hazırlama dış bir depoya daha önce yüklenen bir uygulama paketi için olduğunu belirtir. Uygulama paketi uzantısı *.sfpkg ile sona erer.|
| --yok bekleyin| Sağlama zaman uyumsuz olarak gerçekleşeceğini olup olmadığını gösterir.  Sağlama işlemi döndürür, true olarak ayarlandığında istek sistem ve sağlama işlemi tarafından ne zaman kabul hiçbir zaman aşımı sınırı devam eder. Varsayılan değer false. Büyük uygulama paketlerini için değeri true olarak ayarlanmasını öneririz.|
| --zaman aşımı -t                      | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                              | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                            | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                          | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                              | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
| --verbose                            | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-type"></a>sfctl uygulama türü

Uygulama türleri listesini tam olarak belirtilen adla eşleşen Service Fabric kümesi içinde alır.

Service Fabric kümesi sağlanacak sürecinde veya sağlanan uygulama türleri hakkında bilgi döndürür. Bu sonuçları uygulama adıyla eşleşen tam parametre olarak belirtilen bir ve hangi verilen sorgu parametreleri ile uyumlu türleridir. Bir uygulama türü olarak döndürülen her sürüm uygulama tür adıyla eşleşen uygulama türü'nin tüm sürümleri döndürülür. Yanıt, ad, sürüm, durum ve uygulama türü ile ilgili diğer ayrıntıları içerir. Bu, aksi takdirde tüm uygulama türleri bir sayfasında uygun yani disk belleğine alınan sorgu, olduğu sonuçlarının bir sayfa sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür. Örneğin, 10 uygulama türleri vardır, ancak bir sayfa ilk 3 uygulama türleri yalnızca uygun veya max sonuçları 3'e ayarlanırsa, ardından 3 döndürülür. Sonuçları kalan erişmek için sonraki sorgusunda döndürülen devamlılık belirteci kullanarak sonraki sayfalarda alın. Sonraki sayfa varsa boş devamlılık belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli]| Uygulama türünün adı.|
| --application-type-version        | Uygulama türü sürümü.|
| --devamlılık belirteci           | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --exclude-application-parameters  | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak.|
| --max-results                  | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu sayıda sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir.|
| --zaman aşımı -t                   | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                        | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                      | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                    | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.               Varsayılan: json.|
| --Sorgu                        | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose                      | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-unprovision"></a>sfctl uygulama sağlamayı kaldırma
Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler.

Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler. Bu işlem, yalnızca uygulama türünün tüm uygulama örneğini sildiyseniz gerçekleştirilebilir. Uygulama türü kaydı olduktan sonra bu belirli uygulama türü için yeni bir uygulama örneği oluşturulabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli]| Uygulama türünün adı.|
| --Uygulama-type-version [gerekli]| Uygulama bildiriminde tanımlandığı gibi uygulama türü sürümü.|
|--zaman uyumsuz parametresi                    | Sağlamayı kaldırma zaman uyumsuz olarak gerçekleşeceğini olup olmadığını belirten bayrak. Sağlamayı kaldırma işlemi döndürür, true olarak ayarlandığında istek sistem ve sağlamayı kaldırma işlemi tarafından ne zaman kabul hiçbir zaman aşımı sınırı devam eder. Varsayılan değer false. Ancak, sağlanan büyük uygulama paketlerini true olarak ayarlamanızı öneririz.|
| --zaman aşımı -t                      | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                  Varsayılan: json.|
| --Sorgu                           | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
| --verbose                         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-upgrade"></a>sfctl uygulama yükseltme
Service Fabric kümesi uygulamada yükseltmeyi başlatır.

Sağlanan uygulama yükseltme parametreleri doğrular ve parametreleri geçerliyse uygulama yükseltmeyi başlatır. Yükseltme açıklama varolan uygulama açıklaması değiştirir. Bu parametre belirtilmezse, uygulamaları mevcut parametreleri ile boş parametre listesi üzerine anlamına gelir. Bu, uygulama bildirimi parametrelerini varsayılan değerini kullanarak uygulama sonuçlanır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile sınırlandırılmıştır ' ~' karakter. Örneğin, uygulama adı ise ' fabric: / myapp/app1 ', uygulama kimliği olacaktır ' Uygulamam ~ app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki.|
| --app sürümlü [gerekli]| Hedef uygulama sürümü.|
| --Parametreler [gerekli]| Uygulama yükseltme sırasında uygulanacak JSON olarak kodlanmış listesini uygulama parametre değerini geçersiz kılar.|
| --Varsayılan hizmet sistem durumu ilkesi| JSON varsayılan olarak bir hizmet türünün durumunu değerlendirmek için kullanılan sistem durumu ilkesi belirtimi kodlanmış.|
| --hatası eylemi            | İzlenen yükseltme İzleme İlkesi veya sistem durumu ilkesi ihlali karşılaştığında gerçekleştirilecek eylem.|
| --zorla yeniden başlatma             | Zorla kod sürümü değil değiştiğinde işlemleri yükseltme sırasında bile yeniden başlatın.|
| --health-check-retry-timeout| Uygulama ya da küme önce hatası eylemi sağlıksız olduğunda sistem durumu değerlendirmesini yeniden denemek için süre miktarını yürütülür. Milisaniye cinsinden ölçülür.  Varsayılan: PT0H10M0S.|
| --Sistem durumu denetimi kararlı süre | Süre miktarını bir sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya küme sağlıklı kalmaları gerekir.            Milisaniye cinsinden ölçülür.  Varsayılan: PT0H2M0S.|
| --health-check-wait-duration| Sistem durumu ilkeleri uygulanmadan önce bir yükseltme etki alanına tamamladıktan sonra beklenecek süre miktarı. Milisaniye cinsinden ölçülür.            Varsayılan: 0.|
| --max sağlıksız-uygulamalar        | Sağlıksız dağıtılmış uygulamalar yüzdesi izin verilen en fazla. 0 ile 100 arasında bir sayı olarak temsil.|
| --modu                      | Yükseltme sırasında durumunu izlemek için kullanılan modu.            Varsayılan: UnmonitoredAuto.|
| --replica-set-check-timeout | En uzun süreyi bir yükseltme etki alanına işlenmesini engelleyebilir veya beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için. Saniye cinsinden ölçülür.|
| --Hizmet sistem durumu ilkesi     | JSON service type durum ilkesi hizmet türü adı başına Haritası kodlanmış. Harita boştur varsayılan olabilir.|
| --zaman aşımı -t                | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|
| --upgrade-domain-timeout    | Süre miktarını FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır. Milisaniye cinsinden ölçülür.  Default:            P10675199DT02H48M05.4775807S.|
| --upgrade-timeout           | Süre miktarını FailureAction yürütülmeden önce tamamlamak genel yükseltme vardır. Milisaniye cinsinden ölçülür.  Default:            P10675199DT02H48M05.4775807S.|
| --warning-as-error          | Sistem durumu değerlendirme aynı önem uyarılarla hata olarak kabul eder.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                     | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                   | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                 | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.            Varsayılan: json.|
| --Sorgu                     | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose                   | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-upload"></a>sfctl uygulama karşıya yükleme
Service Fabric uygulama paketi görüntü deposuna kopyalama.

İsteğe bağlı olarak her dosya için karşıya yükleme ilerlemesi pakette görüntüler. Karşıya yükleme ilerleme durumu gönderildiği `stderr`.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --yolu [gerekli]| Yerel uygulama paketin yolu.|
|--Görüntü dizesi| Hedef görüntü depolamak için uygulama paketini karşıya yüklemek için.  Varsayılan: doku: görüntü.|
| --ilerleme durumunu göster  | Büyük paketler için dosya karşıya yükleme ilerlemesini gösterir.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug       | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h     | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı   | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu       | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın.|
| --verbose     | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).
