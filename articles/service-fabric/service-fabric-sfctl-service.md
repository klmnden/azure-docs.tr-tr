---
title: Azure Service Fabric CLI sfctl hizmet | Microsoft Docs
description: Service Fabric CLI'sını sfctl hizmet komutlarını açıklamaktadır.
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
ms.openlocfilehash: e0454d0124efba04434884fbac9056c5e324710d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60556382"
---
# <a name="sfctl-service"></a>sfctl service
Oluşturma, silme ve hizmet, hizmet türlerini ve hizmet paketleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Uygulama adı | Bir hizmet için Service Fabric uygulamasının adını alır. |
| kod paketi listesi | Bir Service Fabric düğümde dağıtılan kod paketlerin listesini alır. |
| oluşturmaya | Belirtilen Service Fabric hizmeti oluşturur. |
| delete | Var olan bir Service Fabric hizmeti siler. |
| dağıtılan türü | Bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamayı belirtilen hizmet türü hakkındaki bilgileri alır. |
| dağıtılan-türü-list | Bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamaları hizmet türleri hakkında bilgi içeren listeyi alır. |
| açıklama | Mevcut bir Service Fabric hizmet açıklamasını alır. |
| Get-container-logs | Kapsayıcı günlükleri için kapsayıcı bir Service Fabric dağıtıldığını alır. |
| sağlık | Belirtilen Service Fabric hizmetinin sistem durumunu alır. |
| bilgi | Service Fabric uygulamaya ait belirli hizmet hakkındaki bilgileri alır. |
| list | Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetleri hakkındaki bilgileri alır. |
| Bildirimi | Hizmet türünü tanımlayan bir bildirim alır. |
| Paketi dağıtma | Belirtilen hizmet bildirimi için belirtilen düğümün görüntü önbelleğine ilişkili paketleri indirir. |
| paket durumu | Bir Service Fabric düğümü ve bir uygulama için dağıtılan belirli bir uygulama için bir hizmet paketi sistem durumu hakkında bilgi alır. |
| Paket bilgileri | Tam olarak belirtilen adla eşleşen bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır. |
| Paket listesi | Bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır. |
| Kurtarma | Service Fabric kümesine çekirdek kaybına şu anda takılı belirtilen hizmet, kurtarılır denemesi gösterir. |
| durumu- | Service Fabric hizmeti üzerinde bir sistem durumu raporu gönderir. |
| çöz | Bir Service Fabric bölümü çözümleyin. |
| tür listesi | Bir Service Fabric kümesindeki bir sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkındaki bilgileri içeren listeyi alır. |
| update | Belirtilen hizmet verilen güncelleştirme açıklaması kullanarak güncelleştirir. |

## <a name="sfctl-service-app-name"></a>sfctl hizmet-adı
Bir hizmet için Service Fabric uygulamasının adını alır.

Belirtilen hizmet için uygulamanın adını alır. Sağlanan hizmet Kimliğine sahip bir hizmet mevcut değilse bir 404 FABRIC_E_SERVICE_DOES_NOT_EXIST hata döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-service-code-package-list"></a>sfctl hizmet kod-paketi-listesi
Bir Service Fabric düğümde dağıtılan kod paketlerin listesini alır.

Belirli bir uygulama için bir Service Fabric düğüm dağıtılan kod paketlerin listesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --kod paket adı | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketi adı. |
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

## <a name="sfctl-service-create"></a>sfctl hizmeti oluşturma
Belirtilen Service Fabric hizmeti oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, uygulama adı ise ' fabric\:/myapp/app1 ', uygulama kimliği olur ' Uygulamam\~app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki. |
| --Ad [gerekli] | Hizmetin adı. Bu, bir alt uygulama kimliği olmalıdır. Bu, tam adıyla birlikte `fabric\:` URI. Örneğin hizmet `fabric\:/A/B` uygulama alt `fabric\:/A`. |
| --service-type [gerekli] | Hizmet türünün adı. |
| --activation-mode | Hizmet Paketi için etkinleştirme modu. |
| --kısıtlamaları | Dize olarak yerleştirme kısıtlamaları. Yerleştirme kısıtlamaları, boolean ifadeler düğüm özellikleri ve hizmet gereksinimlerine göre belirli düğümler için bir hizmet sınırlamak için sağlar. Örneğin, yerleştirmek için bir hizmeti NodeType olduğu mavi düğümlerinde belirtin aşağıdaki\:"NodeColor mavi ==". |
| --correlated-service | İle ilişkilendirmek için hedef hizmet adı. |
| --Bağıntı | Hizmet hizalama benzeşim kullanarak mevcut bir hizmet ile ilişkilendirin. |
| --dns-name | Oluşturulacak hizmet DNS adı. Bu ayar için Service Fabric DNS sistem hizmetinin etkinleştirilmesi gerekir. |
| --örnek sayısı | Örnek sayısı. Bu, yalnızca durum bilgisi olmayan hizmetler için geçerlidir. |
| --int düzeni | Hizmet, işaretsiz tamsayılar aralığı arasında birörnek bölümlendirilmelidir gösterir. |
| --int düzeni sayısı | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, oluşturmak için tamsayı anahtar aralığı içindeki bölüm sayısı. |
| --int düzeni yüksek | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, anahtar tamsayı aralığı sonu. |
| --int düzeni düşük | Bir Tekdüzen tamsayı bölüm düzeni kullanıyorsanız, anahtar tamsayı aralığı başlangıcı. |
| --Yükleme ölçümleri | JSON düğümleri arasında Yük Dengeleme Hizmetleri çalıştırıldığında kullanılan ölçümlerin listesi kodlanmış. |
| --min-replica-set-size | En küçük çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --Taşıma maliyeti | Hizmet taşıma maliyeti belirtir. Olası değerler\: 'Sıfır', 'Düşük', 'Orta', 'Yüksek'. |
| --named-scheme | Hizmet birden çok adlandırılmış bölüm olmayacağını gösterir. |
| --adlı-şeması-list | JSON olarak kodlanmış hizmeti arasında adlandırılmış bölüm düzeni kullanıyorsanız bölümlemek için adları listesi. |
| --no-kalıcı-durum | TRUE ise bu hizmette yerel diskte depolanan kalıcı durum olduğunu veya yalnızca bellekte durumunu depolayan belirtir. |
| --yerleştirme ilke listesi | Hizmet yerleştirme ilkeleri listesi JSON kodlamalı ve tüm ilişkili etki alanı adları. İlkeleri, bir veya daha fazla olabilir\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --Çekirdek kayıp bekleyin | En fazla süreyi saniye cinsinden kendisi için bir bölüm çekirdek kaybı durumunda olmasına izin verilmiyor. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --replica-restart-wait | Bir çoğaltma olduğunda çalışmayı durdurursa ve yeni bir kopya oluşturulduğunda arasındaki saniye cinsinden süre. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --ölçeklendirme ilkeleri | Bu hizmet için ilkeleri ölçeklendirme listesi JSON kodlamalı. |
| --tekil düzeni | Hizmet tek bir bölüm veya bölümlenmemiş bir hizmeti gösterir. |
| --stand-by-replica-keep | Saniye cinsinden en uzun süresi için hangi bekleme kaldırılmadan önce çoğaltmaları korunur. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --durum bilgisi olan | Durum bilgisi olan hizmet hizmet olduğunu gösterir. |
| --stateless | Durum bilgisi olmayan hizmet hizmet olduğunu gösterir. |
| --target-replica-set-size | Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-delete"></a>sfctl hizmet Sil'i
Var olan bir Service Fabric hizmeti siler.

Bir hizmet silinebilmesi için önce oluşturulması gerekir. Varsayılan olarak, Service Fabric hizmeti çoğaltmaları zarif bir şekilde kapatın ve ardından hizmeti silmek çalışacaktır. Ancak, hizmet çoğaltma düzgün biçimde kapatma sorunları yaşıyorsa, silme işlemi uzun zaman alabilir veya takılabilir. İsteğe bağlı ForceRemove bayrak Normal kapatma sırası atlayıp zorla hizmeti silmek için kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
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

## <a name="sfctl-service-deployed-type"></a>sfctl hizmet dağıtılan türü
Bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamayı belirtilen hizmet türü hakkındaki bilgileri alır.

Bir özel hizmet türü bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamalar ile ilgili bilgileri içeren listeyi alır. Yanıt hizmet türü, kayıt durumunu ve etkinleştirme kayıtlı kod paketi adını içeriyor. hizmet paketi kimliği. Her girişin bir etkinleştirme etkinleştirme kimliği ile Ayrıştırılan bir hizmet türünün temsil eder.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --Hizmet-türü-name [gerekli] | Bir Service Fabric hizmet türünün adını belirtir. |
| --Hizmet bildiriminin adı | Dağıtılan hizmet türü bilgileri listesini filtrelemek için hizmet bildiriminin adı. Belirtilmişse yanıt yalnızca bu hizmet bildiriminde tanımlanan hizmet türleri hakkındaki bilgileri içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl hizmet dağıtılan-türü-listesi
Bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamaları hizmet türleri hakkında bilgi içeren listeyi alır.

Bir Service Fabric kümesindeki bir düğümde dağıtılan uygulamaları hizmet türleri hakkında bilgi içeren listeyi alır. Yanıt hizmet türü, kayıt durumunu ve etkinleştirme kayıtlı kod paketi adını içeriyor. hizmet paketi kimliği.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --Hizmet bildiriminin adı | Dağıtılan hizmet türü bilgileri listesini filtrelemek için hizmet bildiriminin adı. Belirtilmişse yanıt yalnızca bu hizmet bildiriminde tanımlanan hizmet türleri hakkındaki bilgileri içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-description"></a>sfctl hizmet açıklaması
Mevcut bir Service Fabric hizmet açıklamasını alır.

Mevcut bir Service Fabric hizmet açıklamasını alır. Bir hizmet açıklamasını edinmenizden önce oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-service-get-container-logs"></a>sfctl hizmet get-container-logs
Kapsayıcı günlükleri için kapsayıcı bir Service Fabric dağıtıldığını alır.

Kapsayıcı günlükleri için kapsayıcı bir Service Fabric düğüm belirli bir kod paketi için dağıtılan alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --kod-paketi-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketi adı. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --service-bildirim-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı bir hizmet bildiriminin adı. |
| --önceki | Kapsayıcı günlüklerini Al çıkıldı ölü kod paketi örneği kapsayıcılardan belirtir. |
| --tail | Günlükleri sonundan gösterilecek satırların sayısı. Varsayılan 100'dür. 'tüm' günlüklerin tamamını göstermek için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-health"></a>sfctl hizmet durumu
Belirtilen Service Fabric hizmetinin sistem durumunu alır.

Belirtilen hizmet durumu bilgilerini alır. Sistem durumu olaylarını sistem durumuna bağlı hizmette bildirilen koleksiyonunu filtrelemek için EventsHealthStateFilter kullanın. Döndürülen bölüm koleksiyonu filtrelemek için PartitionsHealthStateFilter kullanın. Health store içinde yok. bir hizmet belirtirseniz, bu istek bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --exclude sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. İstatistik alt öğe sayısı durumunun Tamam, uyarı ve hata varlıkları gösterir. |
| --bölümleri sağlık Durumu Filtresi | Bölümler sistem durumu nesnelerini filtreleme, sistem durumuna bağlıdır hizmeti sistem durumu sorgusu sonucunu döndürdü sağlar. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen bölümleri döndürülür. Tüm bölümlerin toplam sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değeri bir birleşimi olabilir. Sağlanan değer 6 ise, ardından bölümler sistem durumunu Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-info"></a>sfctl hizmet bilgileri
Service Fabric uygulamaya ait belirli hizmet hakkındaki bilgileri alır.

Belirtilen Service Fabric uygulamaya ait belirtilen hizmeti hakkında bilgi döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
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

## <a name="sfctl-service-list"></a>sfctl hizmet listesi
Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetleri hakkındaki bilgileri alır.

Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetleri hakkında bilgi döndürür

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --Hizmet türü adı | Sorgulamak için Hizmetleri filtrelemek için kullanılan hizmet türü adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-manifest"></a>sfctl hizmet bildirimi
Hizmet türünü tanımlayan bir bildirim alır.

Hizmet türünü tanımlayan bir bildirim alır. Yanıt, bir dize olarak hizmet bildirimi XML içeriyor.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama-türü-name [gerekli] | Uygulama türü adı. |
| --uygulama-türü-sürüm [gerekli] | Uygulama türü sürümü. |
| --service-bildirim-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı bir hizmet bildiriminin adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-deploy"></a>sfctl hizmet paketi dağıtma
Belirtilen hizmet bildirimi için belirtilen düğümün görüntü önbelleğine ilişkili paketleri indirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --app-türü-name [gerekli] | Uygulama bildiriminin adı için karşılık gelen istenen hizmet bildirimi. |
| --app-türü-sürüm [gerekli] | Uygulama bildirimi için karşılık gelen istenen hizmet bildirim sürümü. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --service-bildirim-name [gerekli] | İndirilecek paketlerle ilişkili hizmet bildiriminin adı. |
| --Paylaşımı-policy | Paylaşım ilkelerini JSON olarak kodlanmış listesi. Paylaşım her ilke öğesi bir 'name' ve 'kapsam' oluşur. Adı paylaşılmayan kod, yapılandırma veya veri paketi adına karşılık gelir. Kapsam ya da 'None' olabilir. 'All', 'Code', 'Config' veya 'Veri'. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-health"></a>sfctl hizmet paket durumu
Bir Service Fabric düğümü ve bir uygulama için dağıtılan belirli bir uygulama için bir hizmet paketi sistem durumu hakkında bilgi alır.

Bir Service Fabric düğümde dağıtılan belirli bir uygulama için bir hizmet paketi sistem durumu hakkında bilgi alır. Sistem durumu olayı nesnelerin sistem durumuna bağlıdır dağıtılan hizmet paketi üzerinde bildirilen koleksiyonu için isteğe bağlı olarak filtrelemek için EventsHealthStateFilter kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --hizmet-paketini-name [gerekli] | Hizmet paketi adı. |
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

## <a name="sfctl-service-package-info"></a>sfctl hizmet paket bilgileri
Tam olarak belirtilen adla eşleşen bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır.

Belirli bir uygulama için bir Service Fabric düğüm dağıtılan hizmet paketleri hakkında bilgi döndürür. Bu sonuçları adı hizmeti paket adı tam olarak eşleşen servis paketleri, parametre olarak belirtilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --hizmet-paketini-name [gerekli] | Hizmet paketi adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-package-list"></a>sfctl hizmet paketi-listesi
Bir Service Fabric düğümde dağıtılan hizmet paketleri listesini alır.

Belirli bir uygulama için bir Service Fabric düğüm dağıtılan hizmet paketleri hakkında bilgi döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-recover"></a>sfctl hizmet Kurtar
Service Fabric kümesine çekirdek kaybına şu anda takılı belirtilen hizmet, kurtarılır denemesi gösterir.

Service Fabric kümesine çekirdek kaybına şu anda takılı belirtilen hizmet, kurtarılır denemesi gösterir. Bu işlemi yalnızca kapalı çoğaltmaları kurtarılamaz biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-service-report-health"></a>sfctl hizmet durumu-
Service Fabric hizmeti üzerinde bir sistem durumu raporu gönderir.

Belirtilen Service Fabric hizmeti sistem durumunu raporlar. Rapor üzerinde bildirilen özellik ve sistem durumu raporu kaynağı hakkındaki bilgileri içermelidir. Rapor, bir Service Fabric ağ geçidine ileten sistem durumu deposu için hizmet gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sistem durumu deposu tarafından ek doğrulama sonrasında reddedilen. Örneğin, sistem durumu deposu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporu hizmetinin sistem durumu olaylarını göründüğünü kontrol edin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] sistem durumu özelliği | Sistem durumu bilgileri özelliği. <br><br> Bir varlık sistem durumu raporlarının farklı özellikler için sahip olabilir. , Bir dize ve rapor tetikleyen durumu koşulu kategorilere ayırmak muhabir esnekliğini tanımak için olmayan bir sabit numaralandırma özelliğidir. Örneğin, "AvailableDisk" özelliği, düğüm üzerinde rapor için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı muhabir düğüm bağlantısı izleyebilirsiniz. Health store içinde bu raporları belirtilen düğüm için ayrı bir sistem durumu olayları olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgileri benzersiz olarak tanımlar. |
| --[gerekli] sistem durumu | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Warning', 'Error', 'Bilinmeyen'. |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. <br><br> Bu, genellikle hizmet olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Hizmet adı; Örneğin, ' fabric\:/myapp/app1/svc1', hizmet kimliği olur ' Uygulamam\~app1\~svc1' 6.0 + ve ' myapp/app1/svc1' önceki sürümlerinde. |
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

## <a name="sfctl-service-resolve"></a>sfctl service resolve
Bir Service Fabric bölümü çözümleyin.

Bir Service Fabric hizmeti bölüm hizmeti çoğaltmaların uç noktalarını alma çözümleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu kimlik genellikle hizmet olmadan tam adı olan ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, "fabric\:/myapp/app1/svc1", hizmet kimliği olur "myapp\~app1\~svc1" 6.0 + ve "myapp/app1/svc1" önceki sürümlerinde. |
| --Bölüm anahtarı türü | Bölüm için anahtar türü. Hizmet bölüm şeması Int64Range veya adlandırılmış ise bu parametre gereklidir. Olası değerler aşağıda verilmiştir. -Hiçbiri (1) - PartitionKeyValue parametresinin belirtilmediğinden gösterir. Bu, bölümleme düzeni olarak Singleton ile bölümler için geçerlidir. Varsayılan değer budur. Değer 1'dir. -Int64Range (2) - PartitionKeyValue parametresi bir Int64 bölüm anahtarı olduğunu gösterir. Bu, bölümleme düzeni olarak Int64Range ile bölümler için geçerlidir. Değeri 2'dir. -(3) - adlı PartitionKeyValue parametresi bir bölümün adı olduğunu gösterir. Bu, bölümleme düzeni olarak adlandırılmış ile bölümler için geçerlidir. Değer 3'tür. |
| --Bölüm anahtarı değeri | Bölüm anahtarı. Hizmet bölüm şeması Int64Range veya dosya adı varsa, bu gereklidir. Bu bölüm kimliği değil, ancak bunun yerine, tamsayı anahtar değeri veya bölüm kimliğini adı Örneğin, hizmetiniz aralıklı bölümlerin 0'dan 10 kullanıyorsanız, bunların PartitionKeyValue bu aralıktaki bir tamsayı olması. Adı veya aralığı görmek için hizmet açıklaması sorgulayın. |
| --rsp sürümü önceki | Daha önce alınan yanıtın sürüm alandaki değer. Kullanıcı edinmiş sonucu daha önce eski olduğunu biliyorsa, bu gereklidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-service-type-list"></a>sfctl hizmet türü-listesi
Bir Service Fabric kümesindeki bir sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkındaki bilgileri içeren listeyi alır.

Bir Service Fabric kümesindeki bir sağlanan uygulama türü tarafından desteklenen hizmet türleri hakkındaki bilgileri içeren listeyi alır. Sağlanan uygulama türünün mevcut olması gerekir. Aksi halde bir 404 durum döndürülür.

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

## <a name="sfctl-service-update"></a>sfctl hizmet güncelleştirmesi
Belirtilen hizmet verilen güncelleştirme açıklaması kullanarak güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hizmet kimliği [gerekli] | Hizmet kimliği. Bu, genellikle hizmet olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Hizmet adı; Örneğin, ' fabric\:/myapp/app1/svc1', hizmet kimliği olur ' Uygulamam\~app1\~svc1' 6.0 + ve ' myapp/app1/svc1' önceki sürümlerinde. |
| --kısıtlamaları | Dize olarak yerleştirme kısıtlamaları. Yerleştirme kısıtlamaları, boolean ifadeler düğüm özellikleri ve hizmet gereksinimlerine göre belirli düğümler için bir hizmet sınırlamak için sağlar. Örneğin, yerleştirmek için bir hizmeti NodeType olduğu mavi düğümlerinde belirtin aşağıdaki\: "NodeColor mavi ==". |
| --correlated-service | İle ilişkilendirmek için hedef hizmet adı. |
| --Bağıntı | Hizmet hizalama benzeşim kullanarak mevcut bir hizmet ile ilişkilendirin. |
| --örnek sayısı | Örnek sayısı. Bu, yalnızca durum bilgisi olmayan hizmetler için geçerlidir. |
| --Yükleme ölçümleri | JSON olarak kodlanmış ölçümlerin listesi kullanılan yük düğümleri arasında dengeleme. |
| --min-replica-set-size | En küçük çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --Taşıma maliyeti | Hizmet taşıma maliyeti belirtir. Olası değerler\: 'Sıfır', 'Düşük', 'Orta', 'Yüksek'. |
| --yerleştirme ilke listesi | Hizmet yerleştirme ilkeleri listesi JSON kodlamalı ve tüm ilişkili etki alanı adları. İlkeleri, bir veya daha fazla olabilir\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --Çekirdek kayıp bekleyin | En fazla süreyi saniye cinsinden kendisi için bir bölüm çekirdek kaybı durumunda olmasına izin verilmiyor. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --replica-restart-wait | Bir çoğaltma olduğunda çalışmayı durdurursa ve yeni bir kopya oluşturulduğunda arasındaki saniye cinsinden süre. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --ölçeklendirme ilkeleri | Bu hizmet için ilkeleri ölçeklendirme listesi JSON kodlamalı. |
| --stand-by-replica-keep | Saniye cinsinden en uzun süresi için hangi bekleme kaldırılmadan önce çoğaltmaları korunur. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
| --durum bilgisi olan | Hedef hizmet durum bilgisi olan hizmet olduğunu gösterir. |
| --stateless | Hedef hizmet durum bilgisi olmayan hizmet olduğunu gösterir. |
| --target-replica-set-size | Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu, yalnızca durum bilgisi olan hizmetler için geçerlidir. |
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
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).
