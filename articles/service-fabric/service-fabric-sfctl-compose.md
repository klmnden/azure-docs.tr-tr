---
title: Azure Service Fabric CLI - sfctl oluştur | Microsoft Docs
description: Service Fabric CLI'yı açıklar sfctl komutları oluşturun.
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
ms.openlocfilehash: 4b5cbb4a24b61de7e64a52ef950deedab3eec263
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837275"
---
# <a name="sfctl-compose"></a>sfctl compose
Oluşturma, silme ve Docker Compose uygulamaları yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| create | Oluşturur bir Service Fabric compose dağıtımı. |
| list | Service Fabric kümesinde oluşturulan dağıtımlar alır listesi oluşturun. |
| Kaldır | Siler mevcut bir Service Fabric Küme dağıtımı oluşturun. |
| status | Compose dağıtımı bir Service Fabric hakkında bilgi alır. |
| upgrade | Service Fabric kümesindeki bir compose dağıtımı yükseltme işlemini başlatır. |
| upgrade-rollback | Compose dağıtımı geri başladığında, Service Fabric kümesinde yükseltin. |
| upgrade-status | Yazma dağıtımı bu Service Fabric üzerinde gerçekleştirilen en son yükseltme ayrıntılarını alır. |

## <a name="sfctl-compose-create"></a>sfctl compose oluşturma
Oluşturur bir Service Fabric compose dağıtımı.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım adı. |
| --dosya yolu [gerekli] | Hedef Docker Compose dosyasının yolu. |
| --şifrelenmiş geçişi | Bir kapsayıcı kayıt defteri parolasını isteyen yerine zaten şifrelenmiş bir parola deyimi kullanın. |
| --sahip geçişi | Kapsayıcı kayıt defterine bir parola sorar. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --kullanıcı | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-list"></a>sfctl, liste oluşturma
Service Fabric kümesinde oluşturulan dağıtımlar alır listesi oluşturun.

Oluşturulan compose dağıtımları hakkında veya Service Fabric kümesinde oluşturulan sürecinde durumunu alır. Yanıt adı, durum ve compose dağıtımlarla ilgili diğer ayrıntıları içerir. Dağıtımını bir sayfasında uygun değilse bir sonraki sayfayı almak için kullanılan bir devamlılık belirteci yanı sıra bir sayfalık sonuç döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
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

## <a name="sfctl-compose-remove"></a>sfctl compose remove
Siler mevcut bir Service Fabric Küme dağıtımı oluşturun.

Yazma dağıtımı mevcut bir Service Fabric siler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-status"></a>sfctl oluşturma durumu
Compose dağıtımı bir Service Fabric hakkında bilgi alır.

Oluşturulan compose dağıtımı veya Service Fabric kümesine ve parametre olarak belirtilen bir adı eşleşen oluşturulan sürecinde durumu döndürür. Yanıt adı, durumu ve dağıtım hakkında diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-upgrade"></a>sfctl compose yükseltme
Service Fabric kümesindeki bir compose dağıtımı yükseltme işlemini başlatır.

Sağlanan yükseltme parametrelerini doğrular ve geçerliyse parametreleri dağıtımı yükseltme işlemini başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım adı. |
| --dosya yolu [gerekli] | Yol ' % s'hedef Docker compose dosyası. |
| --default-svc-type-health-map | JSON hizmetlerin durumunu değerlendirmek için kullanılan sistem durumu ilkesi açıklayan sözlük kodlanmış. |
| --şifrelenmiş geçişi | Bir kapsayıcı kayıt defteri parolasını isteyen yerine zaten şifrelenmiş bir parola deyimi kullanın. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --force-yeniden başlatma | Hatta kod sürümü değiştirilmedi işlemleri zorla yükseltme sırasında yeniden başlatılır. <br><br> Yükseltme, yapılandırma veya veri yalnızca değiştirir. |
| --sahip geçişi | Kapsayıcı kayıt defterine bir parola sorar. |
| --Sistem durumu denetimi deneme | Uygulama veya kümenin iyi durumda değilse, sistem durumu denetimleri gerçekleştirmek için girişimleri arasındaki süre uzunluğu. |
| --Sistem durumu denetimi kararlı | Süreyi sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya kümenin sağlıklı kalmasını gerekir. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Sistem durumu denetimi bekleme | Sistem başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süreyi işlemi denetler. |
| --replica-set-check | En uzun süreyi bir yükseltme etki alanını işlenmesini engellemek ve beklenmeyen sorunları kullanılabilirlik kaybını önlemek için. <br><br> Bu zaman aşımı süresi dolduğunda işlem yükseltme etki alanının kullanılabilirlik kaybı sorunları bağımsız olarak devam eder. Zaman aşımı, her bir yükseltme etki alanı başlangıcında sıfırlanır. Geçerli değerler 0 ile kapsamlı 42949672925 arasındadır. |
| --svc-type-health-map | JSON kodlamalı farklı hizmet türleri durumunu değerlendirmek için kullanılan sistem durumu ilkeleri açıklayan nesneleri listesi. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --Uygulama sağlıksız | İyi durumda olmayan uygulamalar yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan uygulamalar maksimum toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan uygulama sistem durumu uyarı olarak değerlendirilir. Bu, iyi durumda olmayan uygulamalar uygulama örnekleri kümedeki toplam sayısı üzerinden bölünmesiyle hesaplanır. |
| --Yükseltme-etki-zaman aşımı | Süreyi FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme türü | Varsayılan\: alınıyor. |
| --Yükseltme modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --Yükseltme zaman aşımı | Süreyi genel yükseltme FailureAction yürütülmeden önce tamamlanması gerekir. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --kullanıcı | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı. |
| --warning-as-error | Uyarıları hata olarak aynı önem derecesi kabul edilip edilmeyeceğini belirtir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-upgrade-rollback"></a>sfctl compose yükseltmeyi geri alma
Compose dağıtımı geri başladığında, Service Fabric kümesinde yükseltin.

Geri alma, service fabric compose dağıtımı yükseltme.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl, yükseltme durumu oluşturma
Yazma dağıtımı bu Service Fabric üzerinde gerçekleştirilen en son yükseltme ayrıntılarını alır.

Compose dağıtımı yükseltmenin yanı sıra hata ayıklama uygulama sistem durumu sorunları yardımcı olacak Ayrıntılar durumu hakkındaki bilgileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
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