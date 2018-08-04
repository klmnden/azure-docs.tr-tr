---
title: Azure Service Fabric CLI - sfctl oluştur | Microsoft Docs
description: Service Fabric CLI'yı açıklar sfctl komutları oluşturun.
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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 3ce0b63c579412d9d8d35b835803becab09f7ef4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494161"
---
# <a name="sfctl-compose"></a>sfctl compose
Oluşturma, silme ve Docker Compose uygulamaları yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Oluşturur bir Service Fabric compose dağıtımı. |
| liste | Service Fabric kümesinde oluşturulan dağıtımlar alır listesi oluşturun. |
| kaldır | Siler mevcut bir Service Fabric Küme dağıtımı oluşturun. |
| durum | Compose dağıtımı bir Service Fabric hakkında bilgi alır. |
| yükselt | Service Fabric kümesindeki bir compose dağıtımı yükseltme işlemini başlatır. |
| Yükseltme durumu | Yazma dağıtımı bu Service Fabric üzerinde gerçekleştirilen en son yükseltme ayrıntılarını alır. |

## <a name="sfctl-compose-create"></a>sfctl compose oluşturma
Oluşturur bir Service Fabric compose dağıtımı.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım adı. |
| --dosya yolu [gerekli] | Hedef Docker Compose dosyasının yolu. |
| --şifrelenmiş geçişi | Bir kapsayıcı kayıt defteri parolasını isteyen yerine zaten şifrelenmiş bir parola deyimi kullanın. |
| --sahip geçişi | Kapsayıcı kayıt defterine bir parola sorar. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --kullanıcı | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım adı. |
| --dosya yolu [gerekli] | Yol ' % s'hedef Docker compose dosyası. |
| --Varsayılan-svc-türü-sistem durumu-map | JSON hizmetlerin durumunu değerlendirmek için kullanılan sistem durumu ilkesi açıklayan sözlük kodlanmış. |
| --şifrelenmiş geçişi | Bir kapsayıcı kayıt defteri parolasını isteyen yerine zaten şifrelenmiş bir parola deyimi kullanın. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --force-yeniden başlatma | Yeniden başlatmaya zorla. |
| --sahip geçişi | Kapsayıcı kayıt defterine bir parola sorar. |
| --Sistem durumu denetimi deneme | Sistem durumu denetimi yeniden deneme zaman aşımı, milisaniye cinsinden ölçülür. |
| --Sistem durumu denetimi kararlı | Sistem durumu denetimi kararlılık süresi milisaniye olarak ölçülür. |
| --Sistem durumu denetimi bekleme | Sistem durumu denetimi bekleme süresi, milisaniye cinsinden ölçülür. |
| --çoğaltma kümesi onay | Yükseltme çoğaltma onay zaman aşımı saniye cinsinden ölçülen ayarlayın. |
| --svc-türü-sistem durumu-map | JSON kodlamalı farklı hizmet türleri durumunu değerlendirmek için kullanılan sistem durumu ilkeleri açıklayan nesneleri listesi. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --Uygulama sağlıksız | İyi durumda olmayan uygulamalar yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan uygulamalar maksimum toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan uygulama sistem durumu uyarı olarak değerlendirilir. Bu, iyi durumda olmayan uygulamalar uygulama örnekleri kümedeki toplam sayısı üzerinden bölünmesiyle hesaplanır. |
| --Yükseltme-etki-zaman aşımı | Yükseltme etki alanı zaman aşımı, milisaniye cinsinden ölçülür. |
| --yükseltme türü | Varsayılan\: alınıyor. |
| --Yükseltme modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --Yükseltme zaman aşımı | Yükseltme zaman aşımının milisaniye olarak ölçülür. |
| --kullanıcı | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı. |
| --hata olarak uyarı | Uyarıları ile aynı önem hata olarak kabul edilir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı} [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).