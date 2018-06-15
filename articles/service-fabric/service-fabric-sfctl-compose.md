---
title: Azure Service Fabric CLI - sfctl oluştur | Microsoft Docs
description: Service Fabric CLI açıklar sfctl komutları oluşturun.
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
ms.openlocfilehash: cc3d3e35ce3dd457d981dfe9420be765cf9fc45a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763417"
---
# <a name="sfctl-compose"></a>sfctl compose
Oluşturma, silme ve Docker Compose uygulamalarını yönetme.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Service Fabric oluşturur dağıtım oluşturun. |
| liste | Listesini oluşturan Service Fabric kümesi içinde oluşturulan dağıtımlar alır. |
| kaldır | Siler var olan bir Service Fabric kümesinden dağıtım oluşturun. |
| durum | Service Fabric alır bilgilerini dağıtım oluşturun. |
| Yükseltme | Service Fabric kümesi oluştur dağıtımda yükseltmeyi başlatır. |
| Yükseltme durumu | Bu Service Fabric üzerinde gerçekleştirilen en son yükseltme alır ayrıntılarını dağıtım oluşturun. |

## <a name="sfctl-compose-create"></a>sfctl oluşturan oluşturma
Service Fabric oluşturur dağıtım oluşturun.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli] | Dağıtım adı. |
| --dosya yolu [gerekli] | Docker Compose hedef dosya yolu. |
| --şifrelenmiş geçişi | İçin bir kapsayıcı kayıt defteri parola isteyen yerine zaten şifrelenmiş bir parola deyimi kullanın. |
| --sahip geçişi | Kapsayıcı kayıt için bir parola sorar. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --kullanıcı | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-list"></a>sfctl listesi oluşturma
Listesini oluşturan Service Fabric kümesi içinde oluşturulan dağıtımlar alır.

Oluşturulan Oluştur dağıtımları hakkında veya Service Fabric kümesi oluşturuluyor sürecinde durumunu alır. Yanıt adı, durum ve oluşturma dağıtımları hakkındaki diğer ayrıntıları içerir. Dağıtımları listesi içindeki bir sayfayı uymuyorsa sonuçlarının bir sayfa, sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
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

## <a name="sfctl-compose-remove"></a>sfctl oluşturan Kaldır
Siler var olan bir Service Fabric kümesinden dağıtım oluşturun.

Var olan bir Service Fabric siler dağıtım oluşturun.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-status"></a>sfctl durumu oluşturma
Service Fabric alır bilgilerini dağıtım oluşturun.

Oluşturulan Oluştur dağıtım veya Service Fabric kümesi ve parametre olarak belirtilen bir adı eşleşen oluşturulmakta sürecinde durumu döndürür. Yanıt adı, durumu ve dağıtım ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli] | Dağıtım kimliği. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-upgrade"></a>sfctl oluşturan yükseltme
Service Fabric kümesi oluştur dağıtımda yükseltmeyi başlatır.

Sağlanan yükseltme parametreleri doğrular ve parametreleri geçerliyse dağıtım yükseltmeyi başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli] | Dağıtım adı. |
| --dosya yolu [gerekli] | Docker hedef yolu dosyası oluşturun. |
| --Varsayılan svc-türü-sistem durumu-eşleme | JSON Hizmetleri durumunu değerlendirmek için kullanılan sistem durumu ilkesi açıklar sözlük kodlanmış. |
| --şifrelenmiş geçişi | İçin bir kapsayıcı kayıt defteri parola isteyen yerine zaten şifrelenmiş bir parola deyimi kullanın. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --zorla yeniden başlatma | Yeniden başlatma. |
| --sahip geçişi | Kapsayıcı kayıt için bir parola sorar. |
| --Sistem durumu denetimi yeniden | Sistem durumu denetimi yeniden deneme zaman aşımı, milisaniye cinsinden ölçülür. |
| --Sistem durumu denetimi kararlı | Sistem durumu denetimi kararlı süresini milisaniye olarak ölçülür. |
| --Sistem durumu denetimi bekleme | Sistem durumu denetimi bekleme süresini milisaniye olarak ölçülür. |
| --çoğaltma kümesi onay | Yükseltme çoğaltma onay zaman aşımı saniye cinsinden ölçülen ayarlayın. |
| --svc türü sistem durumu eşleme | JSON kodlanmış farklı hizmet türlerinin durumunu değerlendirmek için kullanılan sistem durumu ilkeleri açıklayan nesnelerinin listesi. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --Uygulama sağlıksız | Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. |
| --Yükseltme etki alanı timeout | Yükseltme etki alanı zaman aşımı, milisaniye cinsinden ölçülür. |
| --yükseltme türü | Varsayılan\: alınıyor. |
| --Yükseltme modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --Yükseltme zaman aşımı | Yükseltme zaman aşımı, milisaniye cinsinden ölçülür. |
| --kullanıcı | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı. |
| --hata olarak uyarı | Uyarılar aynı önem derecesi hata olarak kabul edilir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl yükseltme durumu oluşturma
Bu Service Fabric üzerinde gerçekleştirilen en son yükseltme alır ayrıntılarını dağıtım oluşturun.

Hata ayıklama uygulama sistem durumu sorunları yardımcı olmak için ayrıntıları yanı sıra Oluştur dağıtım yükseltme durumu hakkında bilgi verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli] | Dağıtım kimliği. |
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