---
title: Azure Service Fabric CLI sfctl hizmet | Microsoft Docs
description: Service Fabric CLI sfctl hizmeti komutlarını açıklar.
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
ms.openlocfilehash: e027bb7f61d19fc274f7eddd8f28e9e6dac099ce
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763553"
---
# <a name="sfctl-service"></a>sfctl service
Oluşturma, silme ve hizmet, hizmet türlerini ve hizmet paketleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Uygulama adı | Bir hizmet için Service Fabric uygulamanın adını alır. |
| kod paketi listesi | Service Fabric düğümde dağıtılan kod paketlerin listesini alır. |
| oluşturmaya | Belirtilen Service Fabric hizmeti oluşturur. |
| sil | Var olan bir Service Fabric hizmeti siler. |
| dağıtılan türü | Service Fabric kümesindeki bir düğümde dağıtılan uygulamayı belirtilen hizmet türü hakkındaki bilgileri alır. |
| dağıtılan-türü-liste | Service Fabric kümesindeki bir düğümde dağıtılan uygulamalardan hizmet türleri hakkında bilgi içeren listeyi alır. |
| açıklama | Var olan bir Service Fabric hizmetini açıklamasını alır. |
| Get kapsayıcı günlükleri | Service Fabric düğümde dağıtılan kapsayıcısı için kapsayıcı günlüklerini alır. |
| sistem durumu | Belirtilen Service Fabric hizmet durumunu alır. |
| bilgi | Service Fabric uygulamaya ait belirli hizmet hakkındaki bilgileri alır. |
| liste | Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetler hakkındaki bilgileri alır |
| Bildirimi | Bir hizmet türünün açıklayan bildirimi alır. |
| Paketi dağıtma | Belirtilen hizmet bildirimi belirtilen düğümün görüntüsünü önbelleğine ilişkili paketleri yükler. |
| paket durumu | Bir Service Fabric düğümü ve uygulama için dağıtılan belirli bir uygulama için bir hizmet paketi sistem durumu bilgilerini alır. |
| Paket bilgileri | Tam olarak belirtilen adla eşleşen bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır. |
| Paket listesi | Service Fabric düğümde dağıtılan hizmet paketleri listesini alır. |
| Kurtarma | Service Fabric kümesi şu anda çekirdek kaybında takıldı belirtilen hizmet kurtarmayı denemesi belirtir. |
| Sistem Durumu raporu | Sistem Durumu raporu Service Fabric hizmetine gönderir. |
| çöz | Service Fabric bölümü çözümleyin. |
| tür listesi | Service Fabric kümesi içinde sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkında bilgi içeren listeyi alır. |
| Güncelleştirme | Belirtilen güncelleştirme açıklamasını kullanarak belirtilen hizmeti güncelleştirir. |

## <a name="sfctl-service-app-name"></a>sfctl hizmet uygulaması adı
Bir hizmet için Service Fabric uygulamanın adını alır.

Belirtilen hizmet için uygulamanın adını alır. Sağlanan hizmet kimliği ile bir hizmeti yoksa, 404 FABRIC_E_SERVICE_DOES_NOT_EXIST hatası döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-service-code-package-list"></a>sfctl hizmet kod-paket-liste
Service Fabric düğümde dağıtılan kod paketlerin listesini alır.

Belirtilen uygulama için bir Service Fabric düğümü dağıtılan kod paketlerin listesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --kod paket adı | Uygulama türü bir Service Fabric kümesindeki bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketi adı. |
| --hizmet bildirimi adı | Uygulama türü bir Service Fabric kümesindeki bir parçası olarak kayıtlı bir hizmet bildirimi adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-create"></a>sfctl hizmet oluşturma
Belirtilen Service Fabric hizmeti oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, uygulama adı ise ' doku\:/myapp/app1 ', uygulama kimliği olacaktır ' Uygulamam\~app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki. |
| --Ad [gerekli] | Hizmetin adı. Bu bir alt uygulama kimliği olmalıdır Bu tam olduğu da dahil olmak üzere `fabric\:` URI. Örneğin hizmet `fabric\:/A/B` uygulama alt `fabric\:/A`. |
| --Hizmet türü [gerekli] | Hizmet türünün adı. |
| --etkinleştirme modu | Hizmet Paketi için etkinleştirme modu. |
| --kısıtlamaları | Dize olarak yerleştirme kısıtlamaları. Kısıtlamalarından düğüm özellikleri boolean ifadeleri ve hizmet gereksinimlerine bağlı olarak belirli düğümler için bir hizmet sınırlamak için izin veren. Örneğin, yerleştirmek için bir hizmet NodeType olduğu mavi düğümlerde belirtin aşağıdaki\:"NodeColor mavi ==". |
| --bağıntılı hizmeti | İle ilişkilendirmek için hedef hizmet adı. |
| --Bağıntı | Hizmet hizalama benzeşim kullanarak var olan bir hizmeti ile ilişkilendirilmesi. |
| --dns adı | Oluşturulacak hizmetin DNS adı. Service Fabric DNS sistem hizmeti bu ayarın etkinleştirilmesi gerekir. |
| --örnek sayısı | Örnek sayısı. Bu, yalnızca durum bilgisi olmayan hizmetler için geçerlidir. |
| --int düzeni | Hizmet hep bir işaretsiz tamsayı aralığı bölümlenmiş gösterir. |
| --int düzeni sayısı | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, oluşturmak için tamsayı anahtar aralık içinde bölüm sayısı. |
| --int düzeni yüksek | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, anahtar tamsayı aralığı sonu. |
| --int düzeni düşük | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, anahtar tamsayı aralığı başlangıcı. |
| --Yük ölçümleri | JSON olarak kodlanmış düğümleri arasında Yük Dengeleme Hizmetleri zaman ölçülerine listesi. |
| --en küçük çoğaltma kümesi boyutu | En küçük çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --Taşıma maliyeti | Hizmeti için taşıma maliyeti belirtir. Olası değerler şunlardır:\: 'Sıfır', 'Düşük', 'Orta', 'Yüksek'. |
| --adlı düzeni | Hizmet birden çok adlandırılmış bölüm olmayacağını gösterir. |
| --adlı-düzeni-liste | JSON olarak kodlanmış adlandırılmış bölüm düzeni kullanıyorsanız, üzerinde hizmet bölüm adları listesi. |
| --yok kalıcı durumu | TRUE ise, bu hizmet yerel diskte depolanan hiçbir kalıcı durumuna sahip veya yalnızca bellek durumu depolar belirtir. |
| --yerleştirme ilke listesi | Hizmeti için yerleşim ilkeleri listesi JSON kodlanmış ve ilişkilendirilen tüm etki alanı adları. İlkeleri, bir veya daha fazla olabilir\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --Çekirdek kayıp bekleyin | Kendisi için bir bölüm çekirdek kayıp durumda olmasına izin verilen saniye cinsinden en uzun süresi. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --çoğaltma yeniden bekleyin | Bir çoğaltma gittiğinde ve yeni bir kopya oluşturulduğunda arasındaki saniye cinsinden süre. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --ölçeklendirme ilkeleri | JSON ilkeleri bu hizmet için ölçeklendirme listesi kodlanmış. |
| --singleton düzeni | Hizmetin tek bir bölüm olması veya bölümlenmemiş hizmeti gösterir. |
| --yedek tarafından çoğaltma koru | Saniye cinsinden en uzun süresi için hangi bekleme kaldırılmadan önce çoğaltmaları korunur. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --durum bilgisi olan | Hizmet durum bilgisi olan hizmet gösterir. |
| --Durum bilgisiz | Bir durum bilgisi olmayan hizmetin hizmet olduğunu gösterir. |
| --Hedef çoğaltma kümesi boyutu | Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-delete"></a>sfctl hizmet silme
Var olan bir Service Fabric hizmeti siler.

Bir hizmet silinebilmesi için önce oluşturulması gerekir. Varsayılan olarak, normal bir şekilde hizmet çoğaltmaları kapatmak ve hizmeti silmek Service Fabric çalışacaktır. Ancak, hizmet çoğaltma düzgün biçimde kapatılması sorunları yaşıyorsa, silme işlemi uzun bir süre devam edebilir veya takılı. Normal kapatma sırası atlayıp zorla hizmeti silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
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

## <a name="sfctl-service-deployed-type"></a>sfctl hizmet dağıtılan türü
Service Fabric kümesindeki bir düğümde dağıtılan uygulamayı belirtilen hizmet türü hakkındaki bilgileri alır.

Belirli hizmet türü bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamalar ile ilgili bilgileri içeren listeyi alır. Hizmet türü, kayıt durumunu ve etkinleştirme kayıtlı kod paketi adını yanıtı içeren hizmet paketi kimliği. Her giriş etkinleştirme kimliğine göre Ayrıştırılan bir hizmet türünün bir etkinleştirme temsil eder

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-türü-name [gerekli] | Bir Service Fabric hizmet türünün adını belirtir. |
| --hizmet bildirimi adı | Dağıtılan hizmet türü bilgileri listesini filtrelemek için hizmet bildirimi adı. Belirtilmişse, yanıt yalnızca bu hizmeti bildiriminde tanımlanan hizmet türleri hakkındaki bilgileri içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl hizmet dağıtılan-türü-liste
Service Fabric kümesindeki bir düğümde dağıtılan uygulamalardan hizmet türleri hakkında bilgi içeren listeyi alır.

Service Fabric kümesindeki bir düğümde dağıtılan uygulamalardan hizmet türleri hakkında bilgi içeren listeyi alır. Hizmet türü, kayıt durumunu ve etkinleştirme kayıtlı kod paketi adını yanıtı içeren hizmet paketi kimliği.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --hizmet bildirimi adı | Dağıtılan hizmet türü bilgileri listesini filtrelemek için hizmet bildirimi adı. Belirtilmişse, yanıt yalnızca bu hizmeti bildiriminde tanımlanan hizmet türleri hakkındaki bilgileri içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-description"></a>sfctl hizmet açıklaması
Var olan bir Service Fabric hizmetini açıklamasını alır.

Var olan bir Service Fabric hizmetini açıklamasını alır. Bir hizmet açıklamasını elde edilebilir önce oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-service-get-container-logs"></a>sfctl hizmeti get-kapsayıcı-günlükleri
Service Fabric düğümde dağıtılan kapsayıcısı için kapsayıcı günlüklerini alır.

Belirtilen kod paketi için bir Service Fabric düğümü dağıtılan kapsayıcısı için kapsayıcı günlüklerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --kod-paket-name [gerekli] | Uygulama türü bir Service Fabric kümesindeki bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketi adı. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-bildirimi-name [gerekli] | Uygulama türü bir Service Fabric kümesindeki bir parçası olarak kayıtlı bir hizmet bildirimi adı. |
| --önceki | Kod paketi örnek çıktı atılacak kapsayıcılardan kapsayıcı günlükleri almak belirtir. |
| --tail | Günlükleri sonundan göstermek için satır sayısı. Varsayılan 100'dür. 'tüm' tam günlükleri göstermek için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-health"></a>sfctl hizmet durumu
Belirtilen Service Fabric hizmet durumunu alır.

Belirtilen hizmet sistem durumu bilgilerini alır. Sistem durumu olayları sistem durumuna bağlı hizmet bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Döndürülen bölüm koleksiyonu filtrelemek için PartitionsHealthStateFilter kullanın. Health store içinde yok. bir hizmeti belirtirseniz, bu istek bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir. |
| --bölümleri sağlık Durumu Filtresi | Sistem sağlığı durumlarına bağlı hizmet sistem durumu sorgusunun sonucu döndürdü bölümleri sistem durumu nesnelerini filtreleme sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen bölümleri döndürülür. Tüm bölümleri toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından Tamam (2) ve uyarı (4), HealthState değeriyle bölümlerinin sistem durumu döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-info"></a>sfctl hizmet bilgileri
Service Fabric uygulamaya ait belirli hizmet hakkındaki bilgileri alır.

Belirtilen Service Fabric uygulamaya ait belirtilen hizmeti hakkında bilgi döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
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

## <a name="sfctl-service-list"></a>sfctl hizmet listesi
Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetler hakkındaki bilgileri alır

Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetleri hakkında bilgi döndürür

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Hizmet türü adı | Hizmetler için sorgu filtre uygulamak için kullanılan hizmet türü adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-manifest"></a>sfctl hizmet bildirimi
Bir hizmet türünün açıklayan bildirimi alır.

Bir hizmet türünün açıklayan bildirimi alır. Yanıt olarak bir dize hizmet bildirimi XML içeriyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türünün adı. |
| --Uygulama-type-version [gerekli] | Uygulama türü sürümü. |
| --service-bildirimi-name [gerekli] | Uygulama türü bir Service Fabric kümesindeki bir parçası olarak kayıtlı bir hizmet bildirimi adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-deploy"></a>sfctl hizmet paketini dağıtma
Belirtilen hizmet bildirimi belirtilen düğümün görüntüsünü önbelleğine ilişkili paketleri yükler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --app-türü-name [gerekli] | Karşılık gelen istenen hizmet bildirimi uygulama bildiriminin adı. |
| --app-type-version [gerekli] | Karşılık gelen istenen hizmet bildirimi için uygulama bildirimi sürümü. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-bildirimi-name [gerekli] | İndirilecek paketlerle ilişkili hizmet bildirimi adı. |
| --Paylaşımı İlkesi | Paylaşım ilkelerini JSON olarak kodlanmış listesi. Paylaşım her ilke öğesi bir 'name' ve 'kapsam' oluşur. Adı paylaşılmak üzere kod, yapılandırma veya veri paketi adına karşılık gelir. Kapsamı 'None' olabilir 'All', 'Code', 'Config' veya 'Verileri'. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-health"></a>sfctl hizmet paketini-durumu
Bir Service Fabric düğümü ve uygulama için dağıtılan belirli bir uygulama için bir hizmet paketi sistem durumu bilgilerini alır.

Service Fabric düğümde dağıtılan belirli bir uygulama için hizmet paketi sistem durumu bilgilerini alır. EventsHealthStateFilter sistem durumuna bağlıdır dağıtılmış hizmet paketi bildirilen HealthEvent nesne koleksiyonundaki için isteğe bağlı olarak filtrelemek için kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-paket-name [gerekli] | Hizmet paketi adı. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-info"></a>sfctl hizmet paketi bilgisi
Tam olarak belirtilen adla eşleşen bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır.

Belirtilen uygulama için bir Service Fabric düğümü dağıtılan hizmet paketleri hakkında bilgi verir. Bu sonuçları adı tam olarak hizmet paketinin adı eşleşen hizmet paketleri, parametre olarak belirtilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-paket-name [gerekli] | Hizmet paketi adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-list"></a>sfctl hizmet paketi listesi
Service Fabric düğümde dağıtılan hizmet paketleri listesini alır.

Belirtilen uygulama için bir Service Fabric düğümü dağıtılan hizmet paketleri hakkında bilgi verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "doku\:/myapp/app1", uygulama kimliği olacaktır "Uygulamam\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-recover"></a>sfctl hizmet Kurtar
Service Fabric kümesi şu anda çekirdek kaybında takıldı belirtilen hizmet kurtarmayı denemesi belirtir.

Service Fabric kümesi şu anda çekirdek kaybında takıldı belirtilen hizmet kurtarmayı denemesi belirtir. Bu işlem, yalnızca kapalı çoğaltmaları kurtarılamıyor biliniyorsa gerçekleştirilmelidir. Bu API yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-service-report-health"></a>sfctl hizmet rapor-durumu
Sistem Durumu raporu Service Fabric hizmetine gönderir.

Belirtilen Service Fabric hizmeti sistem durumunu raporlar. Rapor kaynağı özelliği üzerinde bildirilir ve sistem durumu raporu hakkında bilgiler içermelidir. Rapor bir Service Fabric ağ geçidi sistem durumu depoya iletir hizmet gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sonra ek doğrulama durumu mağaza tarafından reddedildi. Örneğin, sistem sağlığı deposunu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporun hizmetinin sistem durumu olayları göründüğünü denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Sistem durumu özelliği [gerekli] | Sistem durumu bilgisi özelliği. <br><br> Bir varlığın farklı özellikler için sistem durumu raporlarının olabilir. , Bir dize ve rapor tetikler durumu koşulu kategorilere ayırmak Raporlayıcı esnekliğini tanımak için olmayan bir sabit numaralandırması özelliğidir. Örneğin, bunu "AvailableDisk" özelliği bu düğümde raporlamak için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı Raporlayıcı düğümü bağlantı izleyebilirsiniz. Health store içinde bu raporları ayrı sistem durumu olayları için belirtilen düğümün olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgilerini benzersiz şekilde tanımlar. |
| --Sistem durumu [gerekli] | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Uyarı', 'Error', 'Bilinmeyen'. |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. <br><br> Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, hizmet adı ise ' doku\:/myapp/app1/svc1', hizmet kimliği olacaktır ' Uygulamam\~app1\~svc1' 6.0 + ve ' myapp/app1/svc1' önceki sürümlerinde. |
| --Kaynak Kimliği [gerekli] | Sistem durumu bilgisi oluşturur izleme/istemci/sistem bileşeni tanımlayan kaynak adı. |
| --açıklaması | Sistem durumu bilgisi açıklaması. <br><br> Bu rapor hakkında İnsan okunabilir bilgi eklemek için kullanılan serbest metin temsil eder. Açıklama en çok dize uzunluğu 4096 karakter olabilir. Otomatik olarak sağlanan dize uzunsa kesilir. Kesildi, son açıklama işareti "[kesildi]" karakterinden ve toplam dize boyutu 4096 karakter olabilir. İşaretin durumunu kullanıcılara bu kesme gösteren oluştu. Kesirli kısmı, açıklama özgün dizesinden değerinden 4096 karakter olduğuna dikkat edin. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini gösteren bir bayrak. <br><br> Sistem Durumu raporu bir Service Fabric ağ geçidi sistem durumu depoya iletir uygulama gönderilir. Hemen ayarlanmışsa true, rapor hemen HTTP ağ geçidi uygulaması kullanarak doku istemci ayarlarına bakılmaksızın sistem durumu deposuna HTTP geçidinden gönderilir. Mümkün olan en kısa sürede gönderilmesi gereken kritik raporlar için kullanışlıdır. Zamanlama ve diğer koşullara bağlı olarak, raporu göndermeden hala, örneğin HTTP ağ geçidi kapalı veya ağ geçidi ileti ulaşmak değil başarısız olabilir. Hemen false olarak ayarlanırsa, rapor sistem istemci ayarlarına HTTP ağ geçidi'nden göre gönderilir. Bu nedenle, onu HealthReportSendInterval yapılandırmasına göre toplu hale. Sistem durumu iletileri durum deposu ve bunun yanı sıra sistem durumu raporu işleme raporlama iyileştirmek sistem durumu istemci izin verdiği için Önerilen ayar budur. Varsayılan olarak, raporları hemen gönderilmez. |
| --Kaldır zaman süresi | Süresi dolduğunda, sistem durumu deposundan rapor kaldırılmış olup olmadığını belirten değer. <br><br> Bunu süresi dolduktan sonra sistem durumu deposundan true olarak rapor kaldırılırsa. Raporun false olarak ayarlanırsa dolduğunda hata kabul edilir Bu özellik varsayılan olarak false değeridir. İstemcileri düzenli aralıklarla bildirdiğinde RemoveWhenExpired false (varsayılan) ayarlamanız gerekir. Bu şekilde Raporlayıcı sorunları (örneğin, kilitlenme) var ve rapor veremez, sistem durumu raporu sona erdiğinde varlık hatasında değerlendirilir olur. Bu varlık sistem durumu hatası olarak işaretler. |
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

## <a name="sfctl-service-resolve"></a>sfctl hizmet Çöz
Service Fabric bölümü çözümleyin.

Hizmet çoğaltmaları uç noktalarına almak için bir Service Fabric hizmeti bölüm çözümleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle tam hizmeti olmadan adıdır ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "doku\:/myapp/app1/svc1", hizmet kimliği olacaktır "Uygulamam\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --Bölüm anahtarı türü | Bölüm için anahtar türü. Hizmet bölüm düzeni Int64Range veya adlandırılmış ise bu parametre gereklidir. Olası değerler aşağıdaki. -Yok (1) - gösterir PartitionKeyValue parametresi belirtilmedi. Bu, bölümleme düzeni Singleton olarak ile bölümler için geçerlidir. Varsayılan değer budur. Değer 1'dir. -Int64Range (2) - PartitionKeyValue parametresi bir Int64 bölüm anahtarı olduğunu gösterir. Bu, bölümleme düzeni olarak Int64Range ile bölümler için geçerlidir. Değer 2'dir. -(3) - adlı PartitionKeyValue parametresi bölümün adı olduğunu gösterir. Bu, bölümleme düzeni olarak adlandırılmış ile bölümler için geçerlidir. Değer 3'tür. |
| --Bölüm anahtarı değeri | Bölüm anahtarı. Hizmet bölüm düzeni Int64Range veya adlandırılmış ise, bu gereklidir. |
| --rsp sürüm önceki | Daha önce alındı yanıtının sürüm alanındaki değer. Kullanıcı onayınızı sonuç daha önce eski biliyorsa, bu gereklidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-type-list"></a>sfctl hizmet türü listesi
Service Fabric kümesi içinde sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkında bilgi içeren listeyi alır.

Service Fabric kümesi içinde sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkında bilgi içeren listeyi alır. Sağlanan uygulama türü mevcut olması gerekir. Aksi takdirde, 404 durumu döndürülür.

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

## <a name="sfctl-service-update"></a>sfctl hizmet güncelleştirmesi
Belirtilen güncelleştirme açıklamasını kullanarak belirtilen hizmeti güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hizmeti kimliği [gerekli] | Hizmet kimliği. Bu genellikle hizmeti olmadan tam adını kimliğidir ' doku\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, hizmet adı ise ' doku\:/myapp/app1/svc1', hizmet kimliği olacaktır ' Uygulamam\~app1\~svc1' 6.0 + ve ' myapp/app1/svc1' önceki sürümlerinde. |
| --kısıtlamaları | Dize olarak yerleştirme kısıtlamaları. Kısıtlamalarından düğüm özellikleri boolean ifadeleri ve hizmet gereksinimlerine bağlı olarak belirli düğümler için bir hizmet sınırlamak için izin veren. Örneğin, yerleştirmek için bir hizmet NodeType olduğu mavi düğümlerde belirtin aşağıdaki\: "NodeColor mavi ==". |
| --bağıntılı hizmeti | İle ilişkilendirmek için hedef hizmet adı. |
| --Bağıntı | Hizmet hizalama benzeşim kullanarak var olan bir hizmeti ile ilişkilendirilmesi. |
| --örnek sayısı | Örnek sayısı. Bu, yalnızca durum bilgisi olmayan hizmetler için geçerlidir. |
| --Yük ölçümleri | Kullanılan ölçümleri JSON olarak kodlanmış listesi Yük Dengeleme düğümü arasında. |
| --en küçük çoğaltma kümesi boyutu | En küçük çoğaltma boyutu bir sayı olarak ayarlayın. Bu kümesi boyutu yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --Taşıma maliyeti | Hizmeti için taşıma maliyeti belirtir. Olası değerler şunlardır:\: 'Sıfır', 'Düşük', 'Orta', 'Yüksek'. |
| --yerleştirme ilke listesi | Hizmeti için yerleşim ilkeleri listesi JSON kodlanmış ve ilişkilendirilen tüm etki alanı adları. İlkeleri, bir veya daha fazla olabilir\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --Çekirdek kayıp bekleyin | Kendisi için bir bölüm çekirdek kayıp durumda olmasına izin verilen saniye cinsinden en uzun süresi. Bu süre yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --çoğaltma yeniden bekleyin | Bir çoğaltma gittiğinde ve yeni bir kopya oluşturulduğunda arasındaki saniye cinsinden süre. Bu süre yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --ölçeklendirme ilkeleri | JSON ilkeleri bu hizmet için ölçeklendirme listesi kodlanmış. |
| --yedek tarafından çoğaltma koru | Saniye cinsinden en uzun süresi için hangi bekleme kaldırılmadan önce çoğaltmaları korunur. Bu süre yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --durum bilgisi olan | Hedef hizmet durum bilgisi olan hizmet gösterir. |
| --Durum bilgisiz | Hedef hizmet durumsuz bir hizmettir gösterir. |
| --Hedef çoğaltma kümesi boyutu | Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu kümesi boyutu yalnızca durum bilgisi olan hizmetler için geçerlidir. |
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
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).
