---
title: Azure Service Fabric CLI - sfctl uygulama | Microsoft Docs
description: Service Fabric CLI'sını sfctl uygulama komutlarını açıklamaktadır.
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
ms.openlocfilehash: d4fec5d8131d269d3df229360066452c37a92430
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837496"
---
# <a name="sfctl-application"></a>sfctl application
Oluşturma, silme ve uygulama ve uygulama türlerini yönetme.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Belirtilen tanımını kullanarak bir Service Fabric uygulaması oluşturur. |
| delete | Var olan bir Service Fabric uygulaması siler. |
| dağıtılan | Bir Service Fabric düğüm üzerinde dağıtılan bir uygulama hakkındaki bilgileri alır. |
| dağıtılan-health | Bir Service Fabric düğüm üzerinde dağıtılmış bir uygulamanın durumu hakkında bilgileri alır. |
| dağıtılan listesi | Bir Service Fabric düğümde dağıtılan uygulamalar listesini alır. |
| sağlık | Service fabric uygulaması durumunu alır. |
| bilgi | Service Fabric uygulaması hakkındaki bilgileri alır. |
| list | Belirtilen filtrelerle eşleşen ve Service Fabric kümesinde oluşturulan uygulamaların listesini alır. |
| yükleme | Service Fabric uygulaması hakkında bilgi alır yükleyin. |
| Bildirimi | Bir uygulama türünü tanımlayan bir bildirim alır. |
| sağlama | Hükümler veya kayıtları .sfpkg paket dış depoda veya görüntü deposundaki uygulama paketi kullanarak küme Service Fabric uygulaması yazın. |
| durumu- | Service Fabric uygulama üzerinde bir sistem durumu raporu gönderir. |
| type | Tam olarak belirtilen adla eşleşen bir Service Fabric kümesinde uygulama türleri listesini alır. |
| tür listesi | Service Fabric kümesinde uygulama türleri listesini alır. |
| sağlamayı kaldırma | Kümeden bir Service Fabric uygulama türünün kaydını ya da kaldırır. |
| yükselt | Uygulamayı Service Fabric kümesini yükseltme işlemini başlatır. |
| Yükseltme devam et | Uygulamayı Service Fabric kümesini yükseltme devam eder. |
| Yükseltmeyi geri alma | Şu anda devam eden bir uygulama yükseltmesini Service Fabric kümesinde geri başlatır. |
| Yükseltme durumu | Bu uygulama üzerinde gerçekleştirilen en son yükseltme ayrıntılarını alır. |
| karşıya yükle | Bir Service Fabric uygulama paketi görüntü deposuna kopyalayın. |

## <a name="sfctl-application-create"></a>sfctl uygulaması oluşturma
Belirtilen tanımını kullanarak bir Service Fabric uygulaması oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| [gerekli]--uygulama adı | Uygulamanın adı dahil olmak üzere ' fabric\:' URI düzeni. |
| --uygulama türü [gerekli] | Uygulama bildiriminde bulunan uygulama türü adı. |
| --Uygulama sürümü [gerekli] | Uygulama bildiriminde tanımlanan uygulama türü sürümü. |
| --max-node-count | Burada, Service Fabric bu uygulama için kapasite rezervasyonu yapar düğümleri sayısı. Bu hizmetler, bu uygulamanın tüm düğümleri yerleştirileceğini gelmez unutmayın. |
| --Ölçümleri | Bir JSON uygulama kapasite ölçüm açıklamalarını kodlanmış. Bir ölçüm kapasiteler üzerinde uygulama var. her düğüm için bir dizi ile ilişkili bir ad olarak tanımlanır. |
| --en düşük düğüm sayısı | Burada, Service Fabric bu uygulama için kapasite rezervasyonu yapar düğüm sayısı alt sınırı. Bu hizmetler, bu uygulamanın tüm düğümleri yerleştirileceğini gelmez unutmayın. |
| --Parametreler | Parametr aplikace JSON olarak kodlanmış bir listesi, uygulama oluştururken uygulanacak geçersiz kılar. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-delete"></a>sfctl uygulama silme
Var olan bir Service Fabric uygulaması siler.

Uygulamanın silinebilmesi için önce oluşturulması gerekir. Bir uygulama silindiğinde, bu uygulamanın parçası olan tüm hizmetleri silinir. Varsayılan olarak, Service Fabric hizmeti çoğaltmaları zarif bir şekilde kapatın ve ardından hizmeti silmek çalışacaktır. Ancak, hizmet çoğaltma düzgün biçimde kapatma sorunları yaşıyorsa, silme işlemi uzun zaman alabilir veya takılabilir. Normal kapatma sırası atlamak ve uygulamayı ve tüm hizmetlerinin zorla silme için isteğe bağlı ForceRemove bayrağı kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
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

## <a name="sfctl-application-deployed"></a>dağıtılan sfctl uygulaması
Bir Service Fabric düğüm üzerinde dağıtılan bir uygulama hakkındaki bilgileri alır.

Sistem uygulaması için sağlanan uygulama kimliği ise, bu sorgu sistemi uygulama bilgilerini döndürür. Sonuçları etkinleştirme ve indirme durumları etkin, dağıtılan uygulamalarda kapsayabilir. Bu sorgu, bir küme düğümünde düğüm adına karşılık geldiğini gerektirir. Belirtilen düğüm adı, küme üzerinde herhangi bir etkin hizmet Fabric düğümü işaret etmiyor, sorgu başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --dahil-sistem durumu-durum | Bir varlığın sistem durumunu içerir. Bu parametre false veya belirtilmedi ise, döndürülen durumu "Bilinmeyen" olan. Ne zaman sonuçları birleştirilmeden önce true olarak sorgu düğümü ve sistem durumu sistem hizmeti paralel gider. Sonuç olarak, sorguyu daha pahalıdır ve daha uzun sürebilir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-deployed-health"></a>dağıtılan sfctl uygulama-health
Bir Service Fabric düğüm üzerinde dağıtılmış bir uygulamanın durumu hakkında bilgileri alır.

Bir Service Fabric düğüm üzerinde dağıtılmış bir uygulamanın durumu hakkında bilgileri alır. Sistem durumu olayı nesnelerin sistem durumuna bağlıdır dağıtılan bir uygulama üzerinde bildirilen koleksiyonu için isteğe bağlı olarak filtrelemek için EventsHealthStateFilter kullanın. İsteğe bağlı olarak DeployedServicePackageHealth alt sistem durumuna göre filtrelemek için DeployedServicePackagesHealthStateFilter kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --deployed-service-packages-health-state-filter | Dağıtılmış uygulama sistem durumu sorgu sonucunda döndürülen dağıtılan hizmet paketi sistem durumu durumu nesneleri filtreleme, sistem durumuna bağlıdır sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen paketleri döndürülen service tarafından dağıtılan. Tüm dağıtılmış hizmet paketleri, toplanan dağıtılan uygulamanın durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise ardından hizmet paketleri sistem durumunu Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --exclude sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. İstatistik alt öğe sayısı durumunun Tamam, uyarı ve hata varlıkları gösterir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-deployed-list"></a>dağıtılan sfctl uygulama-list
Bir Service Fabric düğümde dağıtılan uygulamalar listesini alır.

Bir Service Fabric düğümde dağıtılan uygulamalar listesini alır. Sonuçları Dağıtılmış Sistem uygulamaları hakkında bilgi için kimliğe göre açıkça sorgulanan sürece dahil değildir Sonuçları etkinleştirme ve indirme durumları etkin, dağıtılan uygulamalarda kapsayabilir. Bu sorgu, bir küme düğümünde düğüm adına karşılık geldiğini gerektirir. Belirtilen düğüm adı, küme üzerinde herhangi bir etkin hizmet Fabric düğümü işaret etmiyor, sorgu başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --dahil-sistem durumu-durum | Bir varlığın sistem durumunu içerir. Bu parametre false veya belirtilmedi ise, döndürülen durumu "Bilinmeyen" olan. Ne zaman sonuçları birleştirilmeden önce true olarak sorgu düğümü ve sistem durumu sistem hizmeti paralel gider. Sonuç olarak, sorguyu daha pahalıdır ve daha uzun sürebilir. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-health"></a>sfctl uygulama durumu
Service fabric uygulaması durumunu alır.

Service fabric uygulaması sistem durumu durumunu döndürür. Yanıt, Tamam, hata veya uyarı sistem durumu bildirir. Health store içinde varlık bulunamadı, hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --deployed-applications-health-state-filter | Dağıtılan uygulamaları sistem durumu nesnelerini filtreleme, sistem durumuna bağlı uygulama sistem durumu sorgu sonucu döndürdü sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen dağıtılan uygulamalar döndürülür. Tüm dağıtılan uygulamaları, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise ardından dağıtılan uygulamalar sistem durumunu Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --exclude sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. İstatistik alt öğe sayısı durumunun Tamam, uyarı ve hata varlıkları gösterir. |
| --Hizmetleri Sistem Durumu Filtresi | Hizmetleri sistem durumu nesnelerini filtreleme, sistem durumuna bağlıdır Hizmetleri sistem durumu sorgusu sonucunu döndürdü sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen Hizmetleri döndürülür. Tüm hizmetler toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer ise, örneğin, ardından Tamam (2) ve (4) uyarı HealthState değeriyle hizmetlerinin sistem durumu döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-info"></a>sfctl uygulama bilgileri
Service Fabric uygulaması hakkındaki bilgileri alır.

Service Fabric kümesine ve parametre olarak belirtilen bir adı eşleşen oluşturulan sürecinde veya oluşturulan uygulama hakkında bilgileri döndürür. Yanıt adı, türü, durumu, parametreleri ve diğer uygulama ayrıntılarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --exclude-application-parameters | Uygulama parametreleri sonuçtan dışlanan olup olmadığını belirten bayrak. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-list"></a>sfctl uygulama listesi
Belirtilen filtrelerle eşleşen ve Service Fabric kümesinde oluşturulan uygulamaların listesini alır.

Oluşturulan veya Service Fabric'te oluşturulurken küme ve belirtilen filtrelerle eşleşen uygulamalar hakkındaki bilgileri alır. Yanıt adı, türü, durumu, parametreleri ve diğer uygulama ayrıntılarını içerir. Uygulamalar bir sayfaya uygun değilse bir sonraki sayfayı almak için kullanılan bir devamlılık belirteci yanı sıra bir sayfalık sonuç döndürülür. Filtreler ApplicationTypeName ve Applicationdefinitionkindfilter'dan aynı anda belirtilemez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --application-definition-kind-filter | Service Fabric uygulaması tanımlamak için kullanılan mekanizma olan ApplicationDefinitionKind üzerinde filtrelemek için kullanılır.  <br> -Default - varsayılan değer, "All" seçerek olarak aynı işlevi gerçekleştirir. Değer 0'dır.  <br> -All - giriş herhangi bir ApplicationDefinitionKind değeri ile eşleşen filtreleyin. Değer 65535'tir.  <br> -ServiceFabricApplicationDescription - giriş ServiceFabricApplicationDescription ApplicationDefinitionKind değeriyle eşleşen filtreleyin. Değer 1'dir.  <br> Giriş oluşturma ApplicationDefinitionKind değeriyle eşleşen - Oluştur - filtre. Değeri 2'dir. |
| --application-type-name | Sorgulamak için uygulamaları filtrelemek için kullanılan uygulama türü adı. Bu değer, bir uygulama türü sürümü içermemelidir. |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --exclude-application-parameters | Uygulama parametreleri sonuçtan dışlanan olup olmadığını belirten bayrak. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-load"></a>sfctl uygulama yükü
Service Fabric uygulaması hakkında bilgi alır yükleyin.

Oluşturulan uygulamayla ilgili veya Service Fabric kümesine ve parametre olarak belirtilen bir adı eşleşen oluşturulan sürecinde yük bilgileri döndürür. Yanıt adı, en düşük düğüm sayısı, en fazla düğüm, uygulama şu anda kaplamıyordur düğüm sayısını ve uygulama hakkında uygulama yük Ölçüm bilgilerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-manifest"></a>sfctl uygulama bildirimi
Bir uygulama türünü tanımlayan bir bildirim alır.

Yanıt, uygulama bildirimi XML'i bir dize olarak içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türü adı. |
| --uygulama-türü-sürüm [gerekli] | Uygulama türü sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-provision"></a>sfctl uygulama sağlama
Hükümler veya kayıtları .sfpkg paket dış depoda veya görüntü deposundaki uygulama paketi kullanarak küme Service Fabric uygulaması yazın.

Bir kümedeki Service Fabric uygulama türüyle sağlar. Yeni uygulamalar oluşturulabilir önce bu gereklidir. Sağlama işlemi, ya da relativePathInImageStore veya dış .sfpkg URI'sini kullanarak belirtilen uygulama paketi üzerinde gerçekleştirilebilir. Bu komut, görüntü deposu sağlama dış sağlama ayarlanmadığı sürece--istedikleri.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama paketi indirme URI'si | Yolun nereden '.sfpkg' uygulama paketi için uygulama paketi HTTP veya HTTPS protokollerini kullanan indirilebilir. <br><br> Dış sağlama türü için yalnızca depolayın. Uygulama paketi dosyasını indirmek için alma işlemi sağlayan bir dış deposunda saklanır. Desteklenen protokoller HTTP ve HTTPS ve yolu okuma erişimine izin vermeniz gerekir. |
| --application-type-build-path | Sağlama türü görüntü deposu için yalnızca. Önceki karşıya yükleme işlemi sırasında belirtilen görüntü deposundaki uygulama paketi için göreli yol. |
| --application-type-name | Dış sağlama türü için yalnızca depolayın. Uygulama türü adı, uygulama bildiriminde bulunan uygulama türünün adını temsil eder. |
| --application-type-version | Dış sağlama türü için yalnızca depolayın. Uygulama türü sürümünü uygulama bildiriminde bulunan uygulama türü sürümünü temsil eder. |
| --external-provision | Uygulama paketi burada kayıtlı veya sağlanan konumu. Hazırlama bir dış depoya daha önce yüklenen bir uygulama paketi için olduğunu gösterir. Uygulama paketi uzantısı *.sfpkg ile sona erer. |
| --no-wait | Sağlama zaman uyumsuz olarak olmamalıdır olup olmadığını gösterir. <br><br> Sağlama işlemi döndürür, true olarak ayarlandığında istek sistem ve sağlama işlemi tarafından ne zaman kabul herhangi bir zaman aşımı sınırı devam eder. Varsayılan değer false'tur. Büyük uygulama paketleri için değerinin true olarak ayarlanmasını öneririz. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-report-health"></a>sfctl uygulama durumu-
Service Fabric uygulama üzerinde bir sistem durumu raporu gönderir.

Belirtilen Service Fabric uygulaması sistem durumunu raporlar. Rapor üzerinde bildirilen özellik ve sistem durumu raporu kaynağı hakkındaki bilgileri içermelidir. Rapor, bir Service Fabric ağ geçidine ileten sistem durumu deposu için uygulama gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sistem durumu deposu tarafından ek doğrulama sonrasında reddedilen. Örneğin, sistem durumu deposu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için uygulama durumunu alın ve rapor görüntülenip görüntülenmediğini kontrol edin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. <br><br> Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, uygulama adı ise ' fabric\:/myapp/app1 ', uygulama kimliği olur ' Uygulamam\~app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki. |
| --[gerekli] sistem durumu özelliği | Sistem durumu bilgileri özelliği. <br><br> Bir varlık sistem durumu raporlarının farklı özellikler için sahip olabilir. , Bir dize ve rapor tetikleyen durumu koşulu kategorilere ayırmak muhabir esnekliğini tanımak için olmayan bir sabit numaralandırma özelliğidir. Örneğin, "AvailableDisk" özelliği, düğüm üzerinde rapor için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı muhabir düğüm bağlantısı izleyebilirsiniz. Health store içinde bu raporları belirtilen düğüm için ayrı bir sistem durumu olayları olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgileri benzersiz olarak tanımlar. |
| --[gerekli] sistem durumu | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Warning', 'Error', 'Bilinmeyen'. |
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

## <a name="sfctl-application-type"></a>sfctl uygulama türü
Tam olarak belirtilen adla eşleşen bir Service Fabric kümesinde uygulama türleri listesini alır.

Service Fabric kümesinde sağlanırken sürecinde veya sağlanan uygulama türleri hakkında bilgi döndürür. Bu sonuçları uygulama adı, tam olarak parametre olarak belirtilen bir eşleşen ve hangi verilen sorgu parametreleri ile uyumlu türleridir. Uygulama türü adı ile eşleşen uygulama türünün tüm sürümler, bir uygulama türü olarak döndürülen her sürümü ile döndürülür. Yanıt adı, sürümü, durumu ve uygulama türünün ilgili diğer ayrıntıları içerir. Bu, aksi takdirde tüm uygulama türleri bir sayfasında uygun yani disk belleğine alınan sorgu, sonraki sayfaya ulaşmak için kullanılabilecek bir devamlılık belirteci yanı sıra bir sayfalık sonuç döndürülür. Örneğin, 10 uygulama türleri vardır, ancak bir sayfa ilk üç uygulama türleri yalnızca en uygun veya en fazla sonuç 3'e ayarlanırsa, sonra üç döndürülür. Sonuçları geri kalanını erişmek için sonraki sayfalarda sonraki sorguda döndürülen devamlılık belirteci kullanarak alın. Sonraki sayfa yok ise, boş bir devamlılık belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türü adı. |
| --application-type-version | Uygulama türü sürümü. |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --exclude-application-parameters | Uygulama parametreleri sonuçtan dışlanan olup olmadığını belirten bayrak. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-type-list"></a>sfctl uygulama türü-listesi
Service Fabric kümesinde uygulama türleri listesini alır.

Service Fabric kümesinde sağlanırken sürecinde veya sağlanan uygulama türleri hakkında bilgi döndürür. Her uygulama türü sürümü, bir uygulama türü olarak döndürülür. Yanıt adı, sürümü, durumu ve uygulama türünün ilgili diğer ayrıntıları içerir. Bu, aksi takdirde tüm uygulama türleri bir sayfasında uygun yani disk belleğine alınan sorgu, sonraki sayfaya ulaşmak için kullanılabilecek bir devamlılık belirteci yanı sıra bir sayfalık sonuç döndürülür. Örneğin, 10 uygulama türleri vardır, ancak bir sayfa ilk üç uygulama türleri yalnızca en uygun veya en fazla sonuç 3'e ayarlanırsa, sonra üç döndürülür. Sonuçları geri kalanını erişmek için sonraki sayfalarda sonraki sorguda döndürülen devamlılık belirteci kullanarak alın. Sonraki sayfa yok ise, boş bir devamlılık belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --application-type-definition-kind-filter | Bir Service Fabric uygulama türünü tanımlamak için kullanılan mekanizma olan ApplicationTypeDefinitionKind filtrelemek için kullanılır.  <br> -Default - varsayılan değer, "All" seçerek olarak aynı işlevi gerçekleştirir. Değer 0'dır.  <br> -All - giriş herhangi bir ApplicationTypeDefinitionKind değeri ile eşleşen filtreleyin. Değer 65535'tir.  <br> -ServiceFabricApplicationPackage - giriş ServiceFabricApplicationPackage ApplicationTypeDefinitionKind değeriyle eşleşen filtreleyin. Değer 1'dir.  <br> Giriş oluşturma ApplicationTypeDefinitionKind değeriyle eşleşen - Oluştur - filtre. Değeri 2'dir. |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --exclude-application-parameters | Uygulama parametreleri sonuçtan dışlanan olup olmadığını belirten bayrak. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-unprovision"></a>sfctl uygulama sağlamayı kaldırma
Kümeden bir Service Fabric uygulama türünün kaydını ya da kaldırır.

Bu işlem, yalnızca tüm uygulama örnekleri uygulama türünün silinmiş olup olmadığını gerçekleştirilebilir. Uygulama türü kaydı eklendiğinde, bu belirli uygulama türü için hiçbir yeni bir uygulama örneği oluşturulabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türü adı. |
| --uygulama-türü-sürüm [gerekli] | Uygulama bildiriminde tanımlanan uygulama türü sürümü. |
| --zaman uyumsuz parametresi | Sağlamayı kaldırma zaman uyumsuz olarak olmamalıdır olup olmadığını belirten bayrak. Sağlamayı kaldırma işlemi döndürür, true olarak ayarlandığında sistem ve sağlamayı kaldırma işlemi isteği kabul zaman herhangi bir zaman aşımı sınırı devam eder. Varsayılan değer false'tur. Ancak, sağlanan büyük uygulama paketleri için true olarak ayarlamak öneririz. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade"></a>sfctl uygulama yükseltme
Uygulamayı Service Fabric kümesini yükseltme işlemini başlatır.

Sağlanan uygulama yükseltme parametreleri doğrular ve geçerliyse parametreleri uygulama yükseltme işlemini başlatır. Yükseltme açıklaması var olan uygulama açıklaması yerine geçer unutmayın. Bu parametre belirtilmezse, uygulamaları mevcut parametreler boş parametre listesi ile üzerine yazılacak anlamına gelir. Bu uygulama bildirimindeki parametreleri varsayılan değerini kullanarak uygulamayı neden olacaktır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. <br><br> Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --Uygulama sürümü [gerekli] | (Uygulama bildiriminde bulunur) hedef uygulama türü sürümünü uygulama yükseltmesi için. |
| --Parametreler [gerekli] | Uygulama yükseltme sırasında uygulanacak parametr aplikace JSON olarak kodlanmış listesi geçersiz kılar. |
| --Varsayılan-hizmet-sistem durumu ilkesi | Varsayılan olarak bir hizmet türünün durumunu değerlendirmek için kullanılan sistem durumu ilkesi belirtimi JSON kodlamalı. |
| --hatası eylemi | İzlenen yükseltme izleme ilke veya sistem durumu ilke ihlallerini karşılaştığında gerçekleştirilecek eylem. |
| --force-yeniden başlatma | Zorla kod sürümü değil değiştiğinde işlemleri yükseltme sırasında bile yeniden başlatın. |
| --Sistem durumu-onay-yeniden-zaman aşımı | Uygulama veya kümenin iyi durumda değilse, sistem durumu denetimleri gerçekleştirmek için girişimleri arasındaki süre uzunluğu.  Varsayılan\: PT0H10M0S. |
| --Sistem durumu denetimi kararlı süresi | Süreyi sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya kümenin sağlıklı kalmasını gerekir.  Varsayılan\: PT0H2M0S. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Sistem durumu denetimi bekleme süresi | Sistem başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süreyi işlemi denetler.  Varsayılan\: 0. |
| --en fazla sağlıksız-uygulamalar | İyi durumda olmayan dağıtılmış uygulamalar yüzdesi, izin verilen en fazla. 0 ile 100 arasında bir sayı olarak temsil edilir. |
| --modu | Sıralı yükseltme sırasında durumunu izlemek için kullanılan modu.  Varsayılan\: UnmonitoredAuto. |
| --replica-set-check-timeout | En uzun süreyi bir yükseltme etki alanını işlenmesini engellemek ve beklenmeyen sorunları kullanılabilirlik kaybını önlemek için. Saniye cinsinden ölçülür. |
| --Hizmet sistem durumu ilkesi | JSON hizmet türü sistem durumu ilkesi başına hizmet türü adı eşlemeyle kodlanmış. Bir eşlem boşsa, varsayılan olarak. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --Yükseltme-etki-zaman aşımı | Süreyi FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır.  Varsayılan\: P10675199DT02H48M05.4775807S. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Yükseltme zaman aşımı | Süreyi genel yükseltme FailureAction yürütülmeden önce tamamlanması gerekir.  Varsayılan\: P10675199DT02H48M05.4775807S. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --warning-as-error | Uyarıları hata olarak aynı önem derecesi kabul edilip edilmeyeceğini belirtir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade-resume"></a>sfctl uygulama yükseltme sürdürme
Uygulamayı Service Fabric kümesini yükseltme devam eder.

Sürdürür izlenmeyen el ile bir Service Fabric uygulaması yükseltme. Service Fabric, bir kerede tek bir yükseltme etki alanı yükseltir. Bir yükseltme etki alanı, Service Fabric tamamlandıktan sonra el ile izlenmeyen yükseltmeler için devam etmeden önce bu API için bir sonraki yükseltme etki alanına çağırmanızı bekler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --Yükseltme-etki-name [gerekli] | Yükseltme devam etmek, yükseltme etki alanı adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade-rollback"></a>sfctl uygulama yükseltme-geri alma
Şu anda devam eden bir uygulama yükseltmesini Service Fabric kümesinde geri başlatır.

Geçerli uygulamanın geri başlamadan önceki sürüme yükseltin. Bu API, yalnızca yeni sürüme İleri sarmadır geçerli Süren yükseltmeyi geri almak için kullanılabilir. Uygulama değil şu anda yükseltiliyor StartApplicationUpgrade API'yi bir önceki sürüme geri alma dahil olmak üzere istediğiniz sürüme yükseltmek için kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade-status"></a>sfctl uygulama yükseltme durumu
Bu uygulama üzerinde gerçekleştirilen en son yükseltme ayrıntılarını alır.

Hata ayıklama uygulama sistem durumu sorunları yardımcı olmak için birlikte en son uygulama yükseltmesi durumuyla ilgili bilgileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upload"></a>sfctl uygulama karşıya yükleme
Bir Service Fabric uygulama paketi görüntü deposuna kopyalayın.

İsteğe bağlı olarak paketteki her dosyayı karşıya yükleme ilerleme durumunu görüntüler. Karşıya yükleme ilerleme durumu gönderildiği `stderr`.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --yol [gerekli] | Yerel uygulama paketi yolu. |
| --imagestore dize | Hedef görüntü depolamak için uygulama paketini karşıya yüklemek için.  Varsayılan\: fabric\:ImageStore. |
| --show-progress | Büyük paketler için dosya karşıya yükleme ilerlemesini gösterir. |

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
