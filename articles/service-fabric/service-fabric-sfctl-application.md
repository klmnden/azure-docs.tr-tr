---
title: Azure Service Fabric CLI sfctl uygulama | Microsoft Docs
description: Service Fabric CLI sfctl uygulama komutlarını açıklar.
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
ms.openlocfilehash: 3ecc5a03ff1847dc11c5a5047e35566a4e68fec2
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763706"
---
# <a name="sfctl-application"></a>sfctl application
Oluşturma, silme ve uygulama ve uygulama türlerini yönetme.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Belirtilen açıklaması kullanarak bir Service Fabric uygulaması oluşturur. |
| sil | Var olan bir Service Fabric uygulamasını siler. |
| Dağıtılan | Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır. |
| dağıtılan durumu | Service Fabric düğümde dağıtılan bir uygulama sistem durumu bilgilerini alır. |
| dağıtılan listesi | Service Fabric düğümde dağıtılan uygulamalar listesini alır. |
| sistem durumu | Service fabric uygulaması durumunu alır. |
| bilgi | Service Fabric uygulaması hakkındaki bilgileri alır. |
| liste | Belirtilen filtrelerle eşleşen Service Fabric kümesi içinde oluşturulan uygulamaların listesini alır. |
| yükleme | Yük Service Fabric uygulaması hakkında bilgileri alır. |
| Bildirimi | Bir uygulama türünü tanımlayan bildirimi alır. |
| Sağlama | Hükümler veya yazmaçlar Service Fabric uygulaması yazın .sfpkg paket dış mağazada veya uygulama paketi görüntü deposuna kullanarak küme ile. |
| Sistem Durumu raporu | Service Fabric uygulaması bir sistem durumu raporu gönderir. |
| type | Uygulama türleri listesini tam olarak belirtilen adla eşleşen Service Fabric kümesi içinde alır. |
| tür listesi | Service Fabric kümesi uygulama türlerinin bir listesini alır. |
| Sağlamayı kaldırma | Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler. |
| Yükseltme | Service Fabric kümesi uygulamada yükseltmeyi başlatır. |
| Yükseltme devam et | Service Fabric kümesi uygulamada yükseltme devam eder. |
| Yükseltmeyi geri alma | Uygulama şu anda devam eden yükseltmesini Service Fabric kümesi geri başlatır. |
| Yükseltme durumu | Bu uygulamaya yapılan en son yükseltme için ayrıntıları alır. |
| karşıya yükle | Service Fabric uygulama paketi görüntü deposuna kopyalama. |

## <a name="sfctl-application-create"></a>sfctl uygulaması oluşturma
Belirtilen açıklaması kullanarak bir Service Fabric uygulaması oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulamanızın-adı [gerekli] | Uygulamanın adı dahil olmak üzere ' doku\:' URI düzeni. |
| --uygulama türü [gerekli] | Uygulama türü adı uygulama bildiriminde bulundu. |
| --app sürümlü [gerekli] | Uygulama bildiriminde tanımlandığı gibi uygulama türü sürümü. |
| --en fazla düğüm sayısı | Burada Service Fabric bu uygulama için yedek kapasite düğüm maksimum sayısı. Bu hizmetler, bu uygulamanın tüm düğümleri yükleneceğini gelmez unutmayın. |
| --Ölçümleri | JSON uygulama kapasite ölçüm açıklamalarını kodlanmış. Ölçüm uygulama var. her düğüm için kapasiteler kümesi ile ilişkili bir ad olarak tanımlanır. |
| --min düğüm sayısı | Düğüm sayısı alt sınırı burada Service Fabric bu uygulama için yedek kapasite. Bu hizmetler, bu uygulamanın tüm düğümleri yükleneceğini gelmez unutmayın. |
| --Parametreler | JSON olarak kodlanmış listesini uygulama parametresi, uygulama oluştururken uygulanacak geçersiz kılar. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-delete"></a>sfctl uygulama silme
Var olan bir Service Fabric uygulamasını siler.

Var olan bir Service Fabric uygulamasını siler. Uygulamanın silinebilmesi için önce oluşturulması gerekir. Bir uygulamayı sildiğinizde, uygulamanın parçası olan tüm hizmetleri silinmesine neden olur. Varsayılan olarak, normal bir şekilde hizmet çoğaltmaları kapatmak ve hizmeti silmek Service Fabric çalışacaktır. Ancak, bir hizmet çoğaltma düzgün biçimde kapatılması sorunları yaşıyorsa, silme işlemi uzun bir süre devam edebilir veya takılı. Normal kapatma sırası atlayın ve uygulama ve hizmetlerinin tümünün zorla silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zorla Kaldır | Bir Service Fabric uygulaması veya hizmeti zorla kapama sırası geçmeden kaldırın. Bu parametre zorla bir uygulamayı silmek için kullanılan veya hizmet için hangi silmeyi zaman aşımına uğramadan engelleyen hizmet kodda sorunları nedeniyle normal olduğundan kopyaları kapatın. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-deployed"></a>dağıtılan sfctl uygulaması
Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.

Service Fabric düğümde dağıtılan bir uygulamayla ilgili bilgileri alır.  Sağlanan uygulama kimliği sistem uygulaması için bu sorguyu sistem uygulama bilgilerini döndürür. Sonuçları etkinleştirme ve durumlarını indirme etkin, dağıtılan uygulamalar kapsar. Bu sorgu düğümü adı küme üzerinde bir düğüme karşılık gelen gerektiriyor. Sağlanan düğüm adı tüm küme etkin Service Fabric düğümlerinde işaret etmiyor, sorgu başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --dahil sistem durumu-durumu | Bir varlık sistem durumunu içerir. Bu parametre false veya belirtilmemiş ise, döndürülen sistem durumu "Bilinmiyor". Sonuçları birleştirilmeden önce true, sorgu kümesine düğüm ve sistem durumu sistem hizmeti paralel gittiğinde. Sonuç olarak, sorguyu daha pahalıdır ve daha uzun bir süre devam edebilir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-deployed-health"></a>dağıtılan sfctl uygulama-sistem
Service Fabric düğümde dağıtılan bir uygulama sistem durumu bilgilerini alır.

Service Fabric düğümde dağıtılan bir uygulama sistem durumu bilgilerini alır. EventsHealthStateFilter sistem durumuna bağlıdır dağıtılan bir uygulama bildirilen HealthEvent nesne koleksiyonundaki için isteğe bağlı olarak filtrelemek için kullanın. DeployedServicePackagesHealthStateFilter sistem durumuna bağlıdır DeployedServicePackageHealth çocuklar için isteğe bağlı olarak filtrelemek için kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --deployed-Service-Packages-Health-State-Filter | Sistem sağlığı durumlarına bağlı olarak, dağıtılmış uygulama sistem durumu sorgu sonucunda döndürülen dağıtılan hizmet paketi sistem durumu nesnelerin filtreleme sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen paketleri döndürülen hizmeti dağıtılmış. Tüm dağıtılmış hizmet paketleri, toplanan dağıtılan uygulamanın durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, sağlanan değer 6 ise sonra hizmet paketleri sistem durumunu Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-deployed-list"></a>dağıtılan sfctl uygulama-liste
Service Fabric düğümde dağıtılan uygulamalar listesini alır.

Service Fabric düğümde dağıtılan uygulamalar listesini alır. Sonuçları dağıtılan sistem uygulamaları hakkında bilgi açıkça için kimliği tarafından sorgulanan sürece dahil etmeyin Sonuçları etkinleştirme ve durumlarını indirme etkin, dağıtılan uygulamalar kapsar. Bu sorgu düğümü adı küme üzerinde bir düğüme karşılık gelen gerektiriyor. Sağlanan düğüm adı tüm küme etkin Service Fabric düğümlerinde işaret etmiyor, sorgu başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --dahil sistem durumu-durumu | Bir varlık sistem durumunu içerir. Bu parametre false veya belirtilmemiş ise, döndürülen sistem durumu durum "Bilinmiyor" şeklindedir. Sonuçları birleştirilmeden önce true, sorgu kümesine düğüm ve sistem durumu sistem hizmeti paralel gittiğinde. Sonuç olarak, sorguyu daha pahalıdır ve daha uzun bir süre devam edebilir. |
| --max sonuçları | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu sayıda sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-health"></a>sfctl uygulama durumu
Service fabric uygulaması durumunu alır.

Service fabric uygulaması sistem durumu durumunu döndürür. Yanıt Tamam, hata veya uyarı sistem durumu raporları. Health store içinde varlık bulunamadı, hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --dağıtılan uygulamalar-sistem durumu-Durumu-Filtresi | Dağıtılan uygulamaları sistem durumu nesnelerini filtreleme kendi sistem durumuna bağlıdır uygulama sistem durumu sorgusunun sonucu döndürdü sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen dağıtılan uygulamalar döndürülür. Tüm dağıtılmış uygulamalar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, 6 sağlanan değer ise, ardından Tamam (2) ve uyarı (4), HealthState değeriyle dağıtılan uygulamalar, sistem durumu döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir. |
| --Hizmetleri Sistem Durumu Filtresi | Kendi sistem durumuna bağlı Hizmetleri sistem durumu sorgusu sonucu döndürdü Hizmetleri sistem durumu nesnelerini filtreleme sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen Hizmetleri döndürülür. Tüm hizmetler toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından Tamam (2) ve uyarı (4), HealthState değeriyle sistem durumu hizmetlerinin döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-info"></a>sfctl uygulama bilgisi
Service Fabric uygulaması hakkındaki bilgileri alır.

Service Fabric kümesi ve parametre olarak belirtilen bir adı eşleşen oluşturulmakta sürecinde veya oluşturulmuş uygulama hakkında bilgi döndürür. Yanıt adı, türü, durumu, parametreleri ve uygulama ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --Dışlama uygulama parametreleri | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-list"></a>sfctl uygulama listesi
Belirtilen filtrelerle eşleşen Service Fabric kümesi içinde oluşturulan uygulamaların listesini alır.

Oluşturulan veya Service Fabric oluşturulan sürecinde küme ve belirtilen filtrelerle eşleşen uygulamalar hakkındaki bilgileri alır. Yanıt adı, türü, durumu, parametreleri ve uygulama ile ilgili diğer ayrıntıları içerir. Uygulamaları bir sayfasında uymuyorsa sonuçlarının bir sayfa, sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür. Filtreleri ApplicationTypeName ve ApplicationDefinitionKindFilter aynı anda belirtilemez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --tanım türü filtre uygulama | Service Fabric uygulaması tanımlamak için kullanılan mekanizma olan ApplicationDefinitionKind üzerinde filtrelemek için kullanılır.  <br> -Varsayılan - varsayılan değer, "Tümü" seçerek olarak aynı işlevi gerçekleştirir. Değeri 0'dır.  <br> -Tüm - giriş herhangi bir ApplicationDefinitionKind değeri ile eşleşen filtre. Değer, 65535 ' dir.  <br> -ServiceFabricApplicationDescription - giriş ServiceFabricApplicationDescription ApplicationDefinitionKind değeriyle eşleşen Filtresi. Değer 1'dir.  <br> Giriş Oluştur ApplicationDefinitionKind değeriyle eşleşen - Oluştur - Filtresi. Değer 2'dir. |
| --uygulama türü adı | Filtre için sorgulamak üzere uygulamalar için kullanılan uygulama türü adı. Bu değeri uygulama türü sürümü içermemesi gerekir. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Dışlama uygulama parametreleri | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak. |
| --max sonuçları | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu sayıda sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-load"></a>sfctl uygulama yükleme
Yük Service Fabric uygulaması hakkında bilgileri alır.

Yükleme hakkında bilgi oluşturulmuş uygulama veya Service Fabric kümesi ve parametre olarak belirtilen bir adı eşleşen oluşturulmakta sürecinde döndürür. Yanıt adı, en az düğüm, en fazla düğüm sayısı, uygulama şu anda kullandığı düğümleri ve uygulama hakkında uygulama yük ölçüm bilgi sayısını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-manifest"></a>sfctl uygulama bildirimi
Bir uygulama türünü tanımlayan bildirimi alır.

Bir uygulama türünü tanımlayan bildirimi alır. Yanıt bir dize XML uygulama bildirimi içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türünün adı. |
| --Uygulama-type-version [gerekli] | Uygulama türü sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-provision"></a>sfctl uygulama sağlama
Hükümler veya yazmaçlar Service Fabric uygulaması yazın .sfpkg paket dış mağazada veya uygulama paketi görüntü deposuna kullanarak küme ile.

Bir küme Service Fabric uygulaması türüyle sağlar. Herhangi bir yeni uygulama örneğinin önce bu gereklidir. Sağlama işlemi ya da relativePathInImageStore veya dış .sfpkg URI'sini kullanarak belirtilen uygulama paketinin üzerinde gerçekleştirilebilir. Dış sağlama ayarlanmadığı sürece--bu komut görüntü deposu sağlama bekler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama paketi yükleme URI | Yolun nereden '.sfpkg' uygulama paketi için uygulama paketi HTTP veya HTTPS protokolleri kullanılarak yüklenebilir. <br><br> Dış sağlama türü için yalnızca depolar. Uygulama paketi dosyasını karşıdan yüklemek için alma işlemi sağlayan bir dış mağazada depolanabilir. Desteklenen HTTP ve HTTPS kurallarıdır ve yolun okuma erişimine izin vermesi gerekir. |
| --uygulama türü derleme yolu | Sağlama tür görüntü deposu için yalnızca. Önceki karşıya yükleme işlemi sırasında belirtilen Image store uygulama paketinde göreli yolu. |
| --uygulama türü adı | Dış sağlama türü için yalnızca depolar. Uygulama türü adı uygulama bildiriminde bulunan uygulama türü adını temsil eder. |
| --uygulama türü sürümü | Dış sağlama türü için yalnızca depolar. Uygulama türü sürümü, uygulama bildiriminde bulunan uygulama türü sürümü temsil eder. |
| --Dış sağlama | Uygulama paketi burada kayıtlı veya sağlanan konumdaki. Hazırlama dış bir depoya daha önce yüklenen bir uygulama paketi için olduğunu belirtir. Uygulama paketi uzantısı *.sfpkg ile sona erer. |
| --yok bekleyin | Sağlama zaman uyumsuz olarak gerçekleşeceğini olup olmadığını gösterir. <br><br> Sağlama işlemi döndürür, true olarak ayarlandığında istek sistem ve sağlama işlemi tarafından ne zaman kabul hiçbir zaman aşımı sınırı devam eder. Varsayılan değer false. Büyük uygulama paketlerini için değeri true olarak ayarlanmasını öneririz. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-report-health"></a>sfctl uygulama rapor-sistem
Service Fabric uygulaması bir sistem durumu raporu gönderir.

Belirtilen Service Fabric uygulaması sağlık durumunu raporlar. Rapor kaynağı özelliği üzerinde bildirilir ve sistem durumu raporu hakkında bilgiler içermelidir. Rapor bir Service Fabric ağ geçidi sistem durumu depoya iletir uygulama gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sonra ek doğrulama durumu mağaza tarafından reddedildi. Örneğin, sistem sağlığı deposunu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için uygulama durumunu almak ve rapor görünüp görünmediğini denetlemek.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. <br><br> Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, uygulama adı ise ' doku\:/myapp/app1 ', uygulama kimliği olacaktır ' Uygulamam\~app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki. |
| --Sistem durumu özelliği [gerekli] | Sistem durumu bilgisi özelliği. <br><br> Bir varlığın farklı özellikler için sistem durumu raporlarının olabilir. , Bir dize ve rapor tetikler durumu koşulu kategorilere ayırmak Raporlayıcı esnekliğini tanımak için olmayan bir sabit numaralandırması özelliğidir. Örneğin, bunu "AvailableDisk" özelliği bu düğümde raporlamak için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı Raporlayıcı düğümü bağlantı izleyebilirsiniz. Health store içinde bu raporları ayrı sistem durumu olayları için belirtilen düğümün olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgilerini benzersiz şekilde tanımlar. |
| --Sistem durumu [gerekli] | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Uyarı', 'Error', 'Bilinmeyen'. |
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

## <a name="sfctl-application-type"></a>sfctl uygulama türü
Uygulama türleri listesini tam olarak belirtilen adla eşleşen Service Fabric kümesi içinde alır.

Service Fabric kümesi sağlanacak sürecinde veya sağlanan uygulama türleri hakkında bilgi döndürür. Bu sonuçları uygulama adıyla eşleşen tam parametre olarak belirtilen bir ve hangi verilen sorgu parametreleri ile uyumlu türleridir. Bir uygulama türü olarak döndürülen her sürüm uygulama tür adıyla eşleşen uygulama türü'nin tüm sürümleri döndürülür. Yanıt, ad, sürüm, durum ve uygulama türü ile ilgili diğer ayrıntıları içerir. Aksi takdirde tüm uygulama türleri bir sayfasında uygun yani disk belleğine alınan sorgu, budur sonuçlarının bir sayfa, sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür. Örneğin, 10 uygulama türleri vardır, ancak bir sayfa ilk üç uygulama türleri yalnızca uygun veya en çok sonuç 3'e ayarlanırsa, ardından 3 döndürülür. Sonuçları kalan erişmek için sonraki sorgusunda döndürülen devamlılık belirteci kullanarak sonraki sayfalarda alın. Sonraki sayfa varsa boş devamlılık belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türünün adı. |
| --uygulama türü sürümü | Uygulama türü sürümü. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Dışlama uygulama parametreleri | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak. |
| --max sonuçları | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu sayıda sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-type-list"></a>sfctl uygulama türü listesi
Service Fabric kümesi uygulama türlerinin bir listesini alır.

Service Fabric kümesi sağlanacak sürecinde veya sağlanan uygulama türleri hakkında bilgi döndürür. Her bir uygulama türü sürümü, bir uygulama türü olarak döndürülür. Yanıt, ad, sürüm, durum ve uygulama türü ile ilgili diğer ayrıntıları içerir. Aksi takdirde tüm uygulama türleri bir sayfasında uygun yani disk belleğine alınan sorgu, budur sonuçlarının bir sayfa, sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür. Örneğin, 10 uygulama türleri vardır, ancak bir sayfa ilk üç uygulama türleri yalnızca uygun veya en çok sonuç 3'e ayarlanırsa, ardından 3 döndürülür. Sonuçları kalan erişmek için sonraki sorgusunda döndürülen devamlılık belirteci kullanarak sonraki sayfalarda alın. Sonraki sayfa varsa boş devamlılık belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-tanımı-tür-filtresi | Service Fabric uygulama türünü tanımlamak için kullanılan mekanizma olan ApplicationTypeDefinitionKind üzerinde filtrelemek için kullanılır.  <br> -Varsayılan - varsayılan değer, "Tümü" seçerek olarak aynı işlevi gerçekleştirir. Değeri 0'dır.  <br> -Tüm - giriş herhangi bir ApplicationTypeDefinitionKind değeri ile eşleşen filtre. Değer, 65535 ' dir.  <br> -ServiceFabricApplicationPackage - giriş ServiceFabricApplicationPackage ApplicationTypeDefinitionKind değeriyle eşleşen Filtresi. Değer 1'dir.  <br> Giriş Oluştur ApplicationTypeDefinitionKind değeriyle eşleşen - Oluştur - Filtresi. Değer 2'dir. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Dışlama uygulama parametreleri | Uygulama parametreleri sonucundan dışlanan olup olmadığını belirten bayrak. |
| --max sonuçları | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı. Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar. En büyük ileti boyutu kısıtlamaları göredir iletisindeki uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürdü. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorgu sayıda sonuçlarını dönüş iletiye sığmayacak mümkün olduğunca içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-unprovision"></a>sfctl uygulama sağlamayı kaldırma
Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler.

Kaldırır veya kümeden bir Service Fabric uygulama türü kaydını siler. Bu işlem, yalnızca uygulama türünün tüm uygulama örneklerini sildiyseniz gerçekleştirilebilir. Uygulama türü kaydı olduktan sonra bu belirli uygulama türü için hiçbir yeni uygulama örneği oluşturulabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türünün adı. |
| --Uygulama-type-version [gerekli] | Uygulama bildiriminde tanımlandığı gibi uygulama türü sürümü. |
| --zaman uyumsuz parametresi | Sağlamayı kaldırma zaman uyumsuz olarak gerçekleşeceğini olup olmadığını belirten bayrak. Sağlamayı kaldırma işlemi döndürür, true olarak ayarlandığında istek sistem ve sağlamayı kaldırma işlemi tarafından ne zaman kabul hiçbir zaman aşımı sınırı devam eder. Varsayılan değer false. Ancak, için sağlanan büyük uygulama paketlerini true ayarlamanız önerilir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade"></a>sfctl uygulama yükseltme
Service Fabric kümesi uygulamada yükseltmeyi başlatır.

Sağlanan uygulama yükseltme parametreleri doğrular ve parametreleri geçerliyse uygulama yükseltmeyi başlatır. Yükseltme açıklama varolan uygulama açıklaması değiştirir unutmayın. Bu parametre belirtilmezse, uygulamaları mevcut parametreleri ile boş parametre listesi üzerine yazılacak anlamına gelir. Bu uygulamanın uygulama bildirimi parametrelerini varsayılan değeri kullanmasına neden olacaktır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. <br><br> Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, uygulama adı ise ' doku\:/myapp/app1 ', uygulama kimliği olacaktır ' Uygulamam\~app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki. |
| --Uygulama sürümü [gerekli] | Hedef uygulama sürümü. |
| --Parametreler [gerekli] | Uygulama yükseltme sırasında uygulanacak JSON olarak kodlanmış listesini uygulama parametre değerini geçersiz kılar. |
| --Varsayılan hizmet sistem durumu ilkesi | JSON varsayılan olarak bir hizmet türünün durumunu değerlendirmek için kullanılan sistem durumu ilkesi belirtimi kodlanmış. |
| --hatası eylemi | İzlenen yükseltme İzleme İlkesi veya sistem durumu ilkesi ihlali karşılaştığında gerçekleştirilecek eylem. |
| --zorla yeniden başlatma | Zorla kod sürümü değil değiştiğinde işlemleri yükseltme sırasında bile yeniden başlatın. |
| --onay yeniden deneme aşımı sistem durumu | Uygulama ya da küme önce hatası eylemi sağlıksız olduğunda sistem durumu değerlendirmesini yeniden denemek için süre miktarını yürütülür. Milisaniye cinsinden ölçülür.  Varsayılan\: PT0H10M0S. |
| --Sistem durumu denetimi kararlı süre | Süre miktarını bir sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya küme sağlıklı kalmaları gerekir. Milisaniye cinsinden ölçülür.  Varsayılan\: PT0H2M0S. |
| --Sistem durumu denetimi bekleme süresi | Sistem durumu ilkeleri uygulanmadan önce bir yükseltme etki alanına tamamladıktan sonra beklenecek süre miktarı. Milisaniye cinsinden ölçülür.  Varsayılan\: 0. |
| --max sağlıksız-uygulamalar | Sağlıksız dağıtılmış uygulamalar yüzdesi izin verilen en fazla. 0 ile 100 arasında bir sayı olarak temsil. |
| --modu | Yükseltme sırasında durumunu izlemek için kullanılan modu.  Varsayılan\: UnmonitoredAuto. |
| --çoğaltma kümesi onay aşımı | En uzun süreyi bir yükseltme etki alanına işlenmesini engelleyebilir veya beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için. Saniye cinsinden ölçülür. |
| --Hizmet sistem durumu ilkesi | JSON service type durum ilkesi hizmet türü adı başına Haritası kodlanmış. Harita boştur varsayılan olabilir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --Yükseltme etki alanı timeout | Süre miktarını FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır. Milisaniye cinsinden ölçülür.  Varsayılan\: P10675199DT02H48M05.4775807S. |
| --Yükseltme zaman aşımı | Süre miktarını FailureAction yürütülmeden önce tamamlamak genel yükseltme vardır. Milisaniye cinsinden ölçülür.  Varsayılan\: P10675199DT02H48M05.4775807S. |
| --hata olarak uyarı | Sistem durumu değerlendirme aynı önem uyarılarla hata olarak kabul eder. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade-resume"></a>sfctl uygulama yükseltme-Sürdür
Service Fabric kümesi uygulamada yükseltme devam eder.

Sürdürür izlenmeyen el ile bir Service Fabric uygulaması yükseltin. Service Fabric aynı anda tek bir yükseltme etki alanı yükseltir. Bir yükseltme etki alanını Service Fabric bittikten sonra izlenmeyen el ile yükseltme yapmak için bir sonraki yükseltme etki alanına devam etmeden önce bu API çağrısı bekler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --Yükseltme-alanı-adı [gerekli] | Yükseltme devam etmek yükseltme etki alanının adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade-rollback"></a>sfctl uygulama yükseltme-geri alma
Uygulama şu anda devam eden yükseltmesini Service Fabric kümesi geri başlatır.

Geçerli uygulamanın geri başlatır önceki sürüme yükseltin. Bu API, yalnızca yeni sürüme İleri alma geçerli sürüyor yükseltmeyi geri almak için kullanılabilir. Uygulama değil şu anda yükseltiliyor StartApplicationUpgrade API bir önceki sürüme geri dahil olmak üzere, istenen sürüme yükseltmek için kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upgrade-status"></a>sfctl uygulama yükseltme-durum
Bu uygulamaya yapılan en son yükseltme için ayrıntıları alır.

Hata ayıklama uygulama sistem durumu sorunları yardımcı olmak için ayrıntıları birlikte en son uygulama yükseltme durumu hakkında bilgi verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-application-upload"></a>sfctl uygulama karşıya yükleme
Service Fabric uygulama paketi görüntü deposuna kopyalama.

İsteğe bağlı olarak her dosya için karşıya yükleme ilerlemesi pakette görüntüler. Karşıya yükleme ilerleme durumu gönderildiği `stderr`.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --yolu [gerekli] | Yerel uygulama paketin yolu. |
| --Görüntü dizesi | Hedef görüntü depolamak için uygulama paketini karşıya yüklemek için.  Varsayılan\: doku\:görüntü. |
| --ilerleme durumunu göster | Büyük paketler için dosya karşıya yükleme ilerlemesini gösterir. |

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
