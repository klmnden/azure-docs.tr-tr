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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: dc57c813a6aecabc21ac3931b7294bce909778d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-application"></a>sfctl uygulama
Oluşturma, silme ve uygulama ve uygulama türlerini yönetme.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya       | Belirtilen açıklaması kullanarak bir Service Fabric uygulaması oluşturur.|
| Sil       | Var olan bir Service Fabric uygulamasını siler.|
| Dağıtılan     | Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.|
| dağıtılan durumu | Sistem durumu hizmeti üzerinde dağıtılmış bir uygulama bilgilerini alır
                      Fabric düğümü.|
| dağıtılan listesi| Service Fabric düğümde dağıtılan uygulamalar listesini alır.|
| Sistem durumu       | Service fabric uygulaması durumunu alır.|
| bilgileri         | Service Fabric uygulaması hakkındaki bilgileri alır.|
| Liste         | Eşleşen Service Fabric kümesi içinde oluşturulan uygulamaların listesini alır
                      parametre olarak belirtilen filtreler.|
| yükleme | Yük Service Fabric uygulaması hakkında bilgileri alır. |
| Bildirimi     | Bir uygulama türünü tanımlayan bildirimi alır.|
| Sağlama    | Hükümler veya kasaların Service Fabric uygulaması kümeyle yazın.|
| Sistem Durumu raporu| Service Fabric uygulaması bir sistem durumu raporu gönderir.|
| type         | Service Fabric eşleşen küme içinde uygulama türleri listesini alır
                      tam olarak belirtilen adı.|
| tür listesi    | Service Fabric kümesi uygulama türlerinin bir listesini alır.|
| Sağlamayı kaldırma  | Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler.|
| Yükseltme      | Service Fabric kümesi uygulamada yükseltmeyi başlatır.|
| Yükseltme devam et  | Service Fabric kümesi uygulamada yükseltme devam eder.|
| Yükseltmeyi geri alma| Bir uygulamada şu anda devam eden yükseltmesini geri başlatır
                      Service Fabric kümesi.|
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
| --en fazla düğüm sayısı     | Service Fabric bu uygulama için kapasite ayırdığını düğüm maksimum sayısı. Bu, bu uygulama hizmetleri tüm düğümleri yerleştirilir gelmez.|
| --Ölçümleri            | JSON uygulama kapasite ölçüm açıklamalarını kodlanmış. Ölçüm uygulama var. her düğüm için kapasiteler kümesi ile ilişkili bir ad olarak tanımlanır.|
| --min düğüm sayısı     | Düğüm sayısı alt sınırı Service Fabric bu uygulama için kapasite ayırır. Bu, bu uygulama hizmetleri tüm düğümleri yerleştirilir gelmez.|
| --Parametreler         | JSON olarak kodlanmış listesini uygulama parametresi, uygulama oluştururken uygulanacak geçersiz kılar.|
| --zaman aşımı -t         | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama              | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım            | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı          | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu              | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı            | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-delete"></a>sfctl uygulama silme
Var olan bir Service Fabric uygulamasını siler.

Var olan bir Service Fabric uygulamasını siler. Uygulamanın silinebilmesi için önce oluşturulması gerekir. Bir uygulamayı sildiğinizde, uygulamanın parçası olan tüm hizmetleri siler. Varsayılan olarak, normal bir şekilde hizmet çoğaltmaları kapatmak ve hizmeti silmek Service Fabric dener. Ancak hizmet çoğaltma düzgün biçimde kapatılması sorunları yaşıyorsa, silme işlemi uzun bir süre devam edebilir veya takılı. Normal kapatma sırası atlayın ve uygulama ve hizmetlerinin tümünün zorla silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Başlayarak
                                 version 6.0, hierarchical names are delimited with the "~"
                                 character. For example, if the application name is
                                 "fabric://myapp/app1", the application identity would be
                                 "myapp~app1" in 6.0+ and "myapp/app1" in previous versions.|
| --zorla Kaldır | Bir Service Fabric uygulaması veya hizmeti zorla kapama sırası geçmeden kaldırın. Hizmet için hangi silmeyi zaman aşımına uğramadan engelleyen hizmet kodda sorunları nedeniyle normal olduğundan çoğaltmalarının kapatın veya bu parametre zorla bir uygulamayı silmek için kullanılabilir. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-deployed"></a>dağıtılan sfctl uygulaması
Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.|
|     
### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Başlayarak
                                 version 6.0, hierarchical names are delimited with the "~"
                                 character. For example, if the application name is
                                 "fabric://myapp/app1", the application identity would be
                                 "myapp~app1" in 6.0+ and "myapp/app1" in previous versions.|
| --düğüm adı [gerekli] | Düğümün adı. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-health"></a>sfctl uygulama durumu
Service fabric uygulaması durumunu alır.

Service fabric uygulaması sistem durumu durumunu döndürür. Yanıt Tamam, hata veya uyarı sistem durumu raporları. Varlık health store içinde bulunmazsa hata verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak,
                                                 hierarchical names are delimited with the "~"
                                                 character. For example, if the application name is
                                                 "fabric://myapp/app1", the application identity
                                                 would be "myapp~app1" in 6.0+ and "myapp/app1" in
                                                 previous versions.|
| --dağıtılan uygulamalar-sistem durumu-durumu-filtre | Dağıtılan uygulamaları sistem durumu nesnelerini filtreleme kendi sistem durumuna bağlıdır uygulama sistem durumu sorgusunun sonucu döndürdü sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen dağıtılan uygulamalar döndürülür. Tüm dağıtılmış uygulamalar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir.                        Örneğin, 6 sağlanan değer ise, ardından Tamam (2) ve uyarı (4), HealthState değeriyle dağıtılan uygulamalar, sistem durumu döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir.                        Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır.                        Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. 65535 değeridir. | | --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür.                        Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. 65535 değeridir. | | --Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata alt sayısını istatistiklerini varlıklar göster. | | --Hizmetleri Sistem Durumu Filtresi | Kendi sistem durumuna bağlı Hizmetleri sistem durumu sorgusu sonucu döndürdü Hizmetleri sistem durumu nesnelerini filtreleme sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen Hizmetleri döndürülür. Tüm hizmetler toplanan sistem durumunu değerlendirmek için kullanılır.                        Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından Tamam (2) ve uyarı (4), HealthState değeriyle sistem durumu hizmetlerinin döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.                        -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. 65535 değeridir. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                                 | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-info"></a>sfctl uygulama bilgisi
Service Fabric uygulaması hakkındaki bilgileri alır.

Service Fabric kümesi ve parametre olarak belirtilen bir adı eşleşen oluşturulmakta sürecinde veya oluşturulmuş uygulama hakkında bilgi döndürür. Yanıt adı, türü, durumu, parametreleri ve uygulama ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ayrılmış
                                      with the "~" character. For example, if the application name
                                      is "fabric://myapp/app1", the application identity would be
                                      "myapp~app1" in 6.0+ and "myapp/app1" in previous versions.|
| --Dışlama uygulama parametreleri | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                      | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                    | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                  | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.             Varsayılan: json.|
| --Sorgu                      | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                    | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-list"></a>sfctl uygulama listesi
Parametre olarak belirtilen filtrelerle eşleşen Service Fabric kümesi içinde oluşturulan uygulamaların listesini alır.

Oluşturulan veya Service Fabric oluşturulan sürecinde küme ve parametre olarak belirtilen filtrelerle eşleşen uygulamalar hakkındaki bilgileri alır. Yanıt adı, türü, durumu, parametreleri ve uygulama ile ilgili diğer ayrıntıları içerir. Uygulamaları bir sayfasında uymuyorsa sonuçlarının bir sayfa sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
|--tanım türü filtre uygulama| ApplicationDefinitionKind için filtre uygulamak için kullanılan
                                          application query operations. - Default - Default value.
                                          Filter that matches input with any
                                          ApplicationDefinitionKind value. The value is 0. - All -
                                          Filter that matches input with any
                                          ApplicationDefinitionKind value. The value is 65535. -
                                          ServiceFabricApplicationDescription - Filter that matches
                                          input with ApplicationDefinitionKind value
                                          ServiceFabricApplicationDescription. The value is 1. -
                                          Compose - Filter that matches input with
                                          ApplicationDefinitionKind value Compose. The value is 2.
                                          Default: 65535.|
| --uygulama türü adı | Filtre için sorgulamak üzere uygulamalar için kullanılan uygulama türü adı. Bu değeri uygulama türü sürümü içermemesi gerekir. | | --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri URL kodlanmış olmamalıdır. | | --Dışlama uygulama parametreleri | Uygulama parametreleri sonucundan hariç tutulan olup olmadığını belirten bayrak. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                      | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                    | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                  | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.             Varsayılan: json.|
| --Sorgu                      | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                    | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-load"></a>sfctl uygulama yükleme
Yük Service Fabric uygulaması hakkında bilgileri alır.

        Returns the load information about the application that was created or in the process of
        being created in the Service Fabric cluster and whose name matches the one specified as the
        parameter. The response includes the name, minimum nodes, maximum nodes, the number of nodes
        the app is occupying currently, and application load metric information about the
        application.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam adıdır
                                 the application without the 'fabric:' URI scheme. Starting from
                                 version 6.0, hierarchical names are delimited with the "~"
                                 character. For example, if the application name is
                                 "fabric://myapp/app1", the application identity would be
                                 "myapp~app1" in 6.0+ and "myapp/app1" in previous versions. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--hata ayıklama                    | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
    ---h Yardım                  | Bu yardım iletisini ve çıkış gösterir.|
    ---o çıktı                | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan:
                                 JSON.|
    --Sorgu                    | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi için bkz.
                                 bilgi ve örnekler.|
    --ayrıntılı                  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

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
| --hata ayıklama                           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                  Varsayılan: json.|
| --Sorgu                           | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-provision"></a>sfctl uygulama sağlama
Hükümler veya kasaların Service Fabric uygulaması kümeyle yazın.
        
Hükümler veya kasaların Service Fabric uygulaması kümeyle yazın. Herhangi bir yeni uygulama örneğinin önce bu gereklidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-derleme-[] yolu gerekli| Uygulama paketi göreli görüntü deposu yolu.|
| --zaman aşımı -t                         | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                              | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                            | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                          | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                              | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                            | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-type"></a>sfctl uygulama türü

Uygulama türleri listesini tam olarak belirtilen adla eşleşen Service Fabric kümesi içinde alır.

Service Fabric kümesi sağlanacak sürecinde veya sağlanan uygulama türleri hakkında bilgi döndürür. Bu sonuçları uygulama adıyla eşleşen tam parametre olarak belirtilen bir ve hangi verilen sorgu parametreleri ile uyumlu türleridir. Bir uygulama türü olarak döndürülen her sürüm uygulama tür adıyla eşleşen uygulama türü'nin tüm sürümleri döndürülür. Yanıt, ad, sürüm, durum ve uygulama türü ile ilgili diğer ayrıntıları içerir. Bu, aksi takdirde tüm uygulama türleri bir sayfasında uygun yani disk belleğine alınan sorgu, olduğu sonuçlarının bir sayfa sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür. Örneğin, 10 uygulama türleri vardır, ancak bir sayfa ilk 3 uygulama türleri yalnızca uygun veya max sonuçları ayarlanmış 3'e, ardından 3 döndürülür. Sonuçları kalan erişmek için sonraki sorgusunda döndürülen devamlılık belirteci kullanarak sonraki sayfalarda alın. Sonraki sayfa varsa boş devamlılık belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli]| Uygulama türünün adı.|
| --devamlılık belirteci           | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --Dışlama uygulama parametreleri  | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak.|
| --max sonuçları                  | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu kadar sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir.|
| --zaman aşımı -t                   | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                        | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                      | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                    | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.               Varsayılan: json.|
| --Sorgu                        | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                      | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-unprovision"></a>sfctl uygulama sağlamayı kaldırma
Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler.

Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler. Bu işlem, yalnızca uygulama türünün tüm uygulama örneğini sildiyseniz gerçekleştirilebilir. Uygulama türü kaydı olduktan sonra bu belirli uygulama türü için yeni bir uygulama örneği oluşturulabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli]| Uygulama türünün adı.|
| --Uygulama-type-version [gerekli]| Uygulama türü sürümü.|
| --zaman aşımı -t                      | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                  Varsayılan: json.|
| --Sorgu                           | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-upgrade"></a>sfctl uygulama yükseltme
Service Fabric kümesi uygulamada yükseltmeyi başlatır.

Sağlanan uygulama yükseltme parametreleri doğrular ve parametreleri geçerliyse uygulama yükseltmeyi başlatır. Lütfen yükseltme açıklama varolan uygulama açıklaması değiştirir unutmayın. Bu parametre belirtilmezse, uygulamaları mevcut parametreleri ile boş parametre listesi üzerine yazılacak anlamına gelir. Bu, uygulama bildirimi parametrelerini varsayılan değerini kullanarak uygulama sonuçlanır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli]| Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile sınırlandırılmıştır ' ~' karakter. için
        example, if the application name is 'fabric://myapp/app1', the application identity would be
        'myapp~app1' in 6.0+ and 'myapp/app1' in previous versions.|
| --app sürümlü [gerekli] | Hedef uygulama sürümü. | | --Parametreler [gerekli] | Uygulama yükseltme sırasında uygulanacak JSON olarak kodlanmış listesini uygulama parametresi geçersiz kılar. | | --Varsayılan hizmet sistem durumu ilkesi | JSON olarak kodlanmış bir hizmet türünün durumunu değerlendirmek için varsayılan olarak kullanılan sistem durumu ilkesi belirtimi. | | --hatası eylemi | İzlenen yükseltme İzleme İlkesi veya sistem durumu ilkesi ihlali karşılaştığında gerçekleştirilecek eylem. | | --zorla yeniden | Kod sürümü değişmediğini olduğunda işlemler yükseltme sırasında bile zorla yeniden. | | --Sistem durumu denetimi yeniden deneme aşımı | Uygulama ya da küme önce hatası eylemi sağlıksız olduğunda sistem durumu değerlendirmesini yeniden denemek için süre miktarını yürütülür. Milisaniye cinsinden ölçülür.  Varsayılan: PT0H10M0S. | | --Sistem durumu denetimi kararlı süre | Süre miktarını bir sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya küme sağlıklı kalmaları gerekir.            Milisaniye cinsinden ölçülür.  Varsayılan: PT0H2M0S. | | --Sistem durumu denetimi bekleme süresi | Sistem durumu ilkeleri uygulanmadan önce bir yükseltme etki alanına tamamladıktan sonra beklenecek süre miktarı. Milisaniye cinsinden ölçülür.            Varsayılan: 0. | | --max sağlıksız-apps | Sağlıksız dağıtılmış uygulamalar yüzdesi izin verilen en fazla. 0 ile 100 arasında bir sayı olarak gösterilen. | | --modu | Yükseltme sırasında durumunu izlemek için kullanılan modu.            Varsayılan: UnmonitoredAuto. | | --çoğaltma kümesi onay aşımı | En uzun süreyi bir yükseltme etki alanına işlenmesini engelleyebilir veya beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için. Saniye cinsinden ölçülen. | | --Hizmet sistem durumu ilkesi | JSON service type durum ilkesi hizmet türü adı başına Haritası kodlanmış. Harita boştur varsayılan. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. | | --Yükseltme etki alanı timeout | Süre miktarını FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır. Milisaniye cinsinden ölçülür.  Varsayılan: P10675199DT02H48M05.4775807S. | | --Yükseltme zaman aşımı | Süre miktarını FailureAction yürütülmeden önce tamamlamak genel yükseltme vardır. Milisaniye cinsinden ölçülür.  Varsayılan: P10675199DT02H48M05.4775807S. | | --hata olarak uyarı | Aynı önem derecesi sistem durumu değerlendirme uyarıları hata ele. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                     | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                   | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                 | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.            Varsayılan: json.|
| --Sorgu                     | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                   | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-application-upload"></a>sfctl uygulama karşıya yükleme
Service Fabric uygulama paketi görüntü deposuna kopyalama.

İsteğe bağlı olarak her dosya için karşıya yükleme ilerlemesi pakette görüntüler. Karşıya yükleme ilerleme durumu gönderildiği `stderr`.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --yolu [gerekli]| Yerel uygulama paketin yolu.|
|--Görüntü dizesi| Hedef görüntü depolamak için uygulama paketini karşıya yüklemek için.  Varsayılan:
                         Doku: görüntü.|
| --ilerleme durumunu göster  | Büyük paketler için dosya karşıya yükleme ilerlemesini gösterir.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama       | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım     | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı   | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu       | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın ve
                       örnekler.|
| --ayrıntılı     | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).