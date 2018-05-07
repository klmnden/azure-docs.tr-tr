---
title: Azure IOT Hub modülü çiftlerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu - kullanım modülü çiftlerini IOT Hub ve aygıtlarınızın arasında durumu ve yapılandırma verileri eşitlemek için
services: iot-hub
documentationcenter: .net
author: chrissie926
manager: timlt
editor: ''
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: menchi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6fbbce06653e60cd914c2ed4d5990aac78ef53a8
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="understand-and-use-module-twins-in-iot-hub"></a>Anlama ve IOT hub'ında modülü çiftlerini kullanın

Bu makalede okuduğunuz varsayılır [anlamak ve IOT hub'da cihaz çiftlerini kullanmak] [ lnk-devguide-device-twins] ilk. IOT Hub ' her bir cihaz kimliği altında en fazla 20 modülü kimlikleri oluşturabilirsiniz. Her bir modül kimliği örtük olarak bir modül çifti oluşturur. Çok cihaz çiftlerini, modül çiftlerini meta verileri, yapılandırmaları ve koşullar dahil olmak üzere Modül durumu bilgilerini depolamak JSON belgelerini benzer. Azure IOT Hub, IOT Hub'ına bağlanan her modül için modülü twin tutar. 

Cihaz tarafında, IOT Hub cihaz SDK'ları, her IOT hub'ı bağımsız bir bağlantı açar modülleri oluşturma olanak tanır. Bu, Cihazınızda farklı bileşenler için ayrı ad alanları kullanmanıza olanak sağlar. Örneğin, üç farklı algılayıcılar bir satış makine gerekir. Her algılayıcı şirketinizdeki farklı Departmanlar tarafından denetlenir. Her algılayıcı için bir modül oluşturabilirsiniz. Bu şekilde, her bölüm yalnızca işleri veya doğrudan yöntemleri bunlar çakışmalarını ve kullanıcı hatalarını önleme denetimi algılayıcı gönderebilir.

 Modül kimliği ve modülü twin cihaz kimliğini ve devic çifti olarak ancak daha hassas bir ayrıntı düzeyi aynı yetenekleri sağlar. İşletim sistemi veya yapılandırma ve koşullar için bu bileşenlerin her birini yalıtmak için birden çok bileşen yönetme bellenim aygıtları aygıtları esasında özellikli cihazların yüksekse bu ayrıntı düzeyi sağlar. Modül kimliği ve modül çiftlerini sorunları yönetim ayrılması modüler yazılım bileşenleri sahip IOT cihazları ile çalışırken sağlar. Modül twin genel kullanılabilirlik tarafından modülü twin düzeyinde tüm cihaz çifti işlevlerini destekleyen en hedefleyin. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu makalede açıklanır:

* Modül twin yapısını: *etiketleri*, *istenen* ve *özellikleri bildirilen*.
* Modüller ve arka uçları modülü çiftlerini üzerinde gerçekleştirebileceğiniz işlemler.

Başvurmak [cihaz bulut iletişimi Kılavuzu] [ lnk-d2c-guidance] bildirilen özellikleri, cihaz bulut iletilerini veya karşıya dosya yükleme kullanma konusunda yönergeler için.
Başvurmak [bulut-cihaz iletişimi Kılavuzu] [ lnk-c2d-guidance] istenen özellikler, doğrudan yöntemleri veya Bulut-cihaz iletilerini kullanma konusunda yönergeler için.

## <a name="module-twins"></a>Modül çiftlerini
Modül çiftlerini modülü ile ilgili bilgileri depolar:

* IOT Hub ve cihaz modülleri modülü koşullar ve yapılandırma eşitlemek için kullanabilirsiniz.
* Çözüm arka ucu sorgu ve uzun süre çalışan hedef için kullanabileceğiniz işlemleri.

Karşılık gelen bir modül twin yaşam döngüsü bağlı [modül kimliği][lnk-identity]. Modülleri çiftlerini örtük olarak oluşturulur ve bir modül kimliği oluşturulduğunda veya IOT hub'da silinmiş silinir.

Bir modül twin içeren bir JSON belgesi şöyledir:

* **Etiketleri**. Çözüm arka ucu JSON belgesinin bir bölümünü okuma ve yazma. Etiketler cihaz modüllerinde görünür değildir. Etiketler amacı sorgulamak için ayarlanır.
* **Özellikler istenen**. Modül yapılandırması veya koşulları eşitlemek için bildirilen özellikleriyle birlikte kullanılır. Modül uygulama okuyabilir ve çözüm arka ucu istenen özellikleri ayarlayabilirsiniz. Modül uygulama ayrıca istenen özelliklerinde değişiklik bildirimleri alabilirsiniz.
* **Özellikler bildirilen**. Modül yapılandırması veya koşulları eşitlemek için istediğiniz özellikleri ile birlikte kullanılır. Modül uygulama bildirilen özellikleri ayarlayabilirsiniz ve çözüm arka ucu okuyabilir ve bunları sorgulayabilirsiniz.
* **Modül kimlik özellikleri**. Modül twin JSON belgesi kökündeki saklanan karşılık gelen bir modül kimliği salt okunur özelliklerinden içeren [kimlik kayıt defteri][lnk-identity].

![][img-module-twin]

Aşağıdaki örnek, bir modül twin JSON belgesi gösterir:

```json
{
    "deviceId": "devA",
    "moduleId": "moduleA",
    "etag": "AAAAAAAAAAc=", 
    "status": "enabled",
    "statusReason": "provisioned",
    "statusUpdateTime": "0001-01-01T00:00:00",
    "connectionState": "connected",
    "lastActivityTime": "2015-02-30T16:24:48.789Z",
    "cloudToDeviceMessageCount": 0, 
    "authenticationType": "sas",
    "x509Thumbprint": {     
        "primaryThumbprint": null, 
        "secondaryThumbprint": null 
    }, 
    "version": 2, 
    "tags": {
        "$etag": "123",
        "deploymentLocation": {
            "building": "43",
            "floor": "1"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata" : {...},
            "$version": 1
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            "batteryLevel": 55,
            "$metadata" : {...},
            "$version": 4
        }
    }
}
```

Kök nesnesinde modül kimliği özelliklerdir ve kapsayıcı nesneleri için `tags` ve her ikisi de `reported` ve `desired` özellikleri. `properties` Kapsayıcısı bazı salt okunur öğeleri içerir (`$metadata`, `$etag`, ve `$version`) açıklanan [modülü twin meta veri] [ lnk-module-twin-metadata] ve [ İyimser eşzamanlılık] [ lnk-concurrency] bölümler.

### <a name="reported-property-example"></a>Bildirilen özelliği örneği
Önceki örnekte modülü twin içeren bir `batteryLevel` modülü uygulama tarafından bildirilen özelliği. Bu özellik, sorgu ve son bildirilen pil düzeyi temelinde modülleri üzerinde çalıştırmak mümkün kılar. Diğer örnekler modülü uygulama raporlama modülü özelliklerini veya bağlantı seçenekleri içerir.

> [!NOTE]
> Bildirilen özellikler çözüm arka ucu bir özelliğin bilinen son değerini nerede ilgilendiği senaryoları basitleştirin. Kullanım [cihaz bulut iletilerini] [ lnk-d2c] çözüm arka ucu zaman serisi gibi zaman damgalı olaylar dizisini biçiminde modülü telemetri işlemek gerekiyorsa.

### <a name="desired-property-example"></a>İstenen özelliği örneği
Önceki örnekte, `telemetryConfig` modülü twin istenen ve bildirilen özellikler çözüm arka ucu ve modül uygulama tarafından bu modül için telemetri yapılandırma eşitlemek için kullanılır. Örneğin:

1. Çözüm arka ucu istenen özelliği istenen yapılandırma değeri ayarlar. İstenen özellik kümesine sahip belge kısmı şöyledir:

    ```json
    ...
    "desired": {
        "telemetryConfig": {
            "sendFrequency": "5m"
        },
        ...
    },
    ...
    ```

2. Modül uygulama hemen bağlıysa değişiklik ya da ilk yeniden bağlanma sırasında bildirilir. Modül uygulama daha sonra güncelleştirilmiş yapılandırmayı raporlar (veya bir hata koşulu kullanarak `status` özelliği). Bildirilen özellikleri kısmı şöyledir:

    ```json
    ...
    "reported": {
        "telemetryConfig": {
            "sendFrequency": "5m",
            "status": "success"
        }
        ...
    }
    ...
    ```

3. Çözüm arka ucu yapılandırma işleminin sonuçlarını birçok modül arasında göre izleyebilirsiniz [sorgulama] [ lnk-query] modülü çiftlerini.

> [!NOTE]
> Önceki kod parçacıkları örnek, bir modül yapılandırması ve durumunu kodlamak için bir yol okunabilmesi için en iyi duruma getirilmiş verilebilir. Modül twin istenen ve modül çiftlerini özelliklerinde rapor için IOT hub'ı belirli bir şema getirmez.
> 
> 

## <a name="back-end-operations"></a>Arka plan işlemleri
Çözüm arka ucu HTTPS kullanıma sunulan aşağıdaki atomik işlemleri kullanarak modülü twin çalıştırır:

* **Modül twin Kimliğine göre almak**. Bu işlem, etiketler ve istenen ve bildirilen sistem özellikleri dahil olmak üzere modülü twin belgeyi döndürür.
* **Kısmen modülü twin güncelleştirme**. Bu işlem, etiket veya modülü çiftine istenen özellikler kısmen güncelleştirmek çözüm arka ucu sağlar. Kısmi güncelleştirmeyi ekler veya herhangi bir özellik güncelleştirmeleri bir JSON belgesi olarak ifade edilir. Özelliklerini ayarlamak `null` kaldırılır. Aşağıdaki örnek yeni bir istenen özellik değeri ile oluşturur `{"newProperty": "newValue"}`, var olan değerini geçersiz kılar `existingProperty` ile `"otherNewValue"`ve kaldırır `otherOldProperty`. Herhangi bir değişiklik mevcut istenen özellikleri veya etiketleri oluşturulur:

    ```json
    {
        "properties": {
            "desired": {
                "newProperty": {
                    "nestedProperty": "newValue"
                },
                "existingProperty": "otherNewValue",
                "otherOldProperty": null
            }
        }
    }
    ```

* **İstenen Özellikleri Değiştir**. Bu işlem tamamen varolan tüm istenen özellikler üzerine ve yeni bir JSON belgesi için alternatif çözüm arka ucu sağlayan `properties/desired`.
* **Etiketleri değiştirmek**. Bu işlem tamamen varolan tüm etiketleri üzerine ve yeni bir JSON belgesi için alternatif çözüm arka ucu sağlayan `tags`.
* **Twin bildirimlerin**. Bu işlem twin değiştirildiğinde bildirim almak çözüm arka ucu sağlar. Bunu yapmak için IOT çözümünüzün bir rota oluşturmak ve veri kaynağı eşit ayarlamak için gereken *twinChangeEvents*. Varsayılan olarak, hiçbir twin bildirimler gönderilir, diğer bir deyişle, bu tür bir yollar önceden mevcut. Değişiklik hızı çok yüksekse, veya diğer nedenlerle iç hataları gibi IOT hub'ı tüm değişiklikleri içeren tek bir bildirim gönderebilirsiniz. Bu nedenle, uygulamanızın güvenilir denetim ve tüm ara durumlarıyla ilgili günlük gerekirse, cihaz bulut iletilerini kullanmanız gerekir. Özellikler ve gövde twin bildirim iletisi içerir.

    - Özellikler

    | Ad | Değer |
    | --- | --- |
    $content-türü | uygulama/json |
    $iothub-enqueuedtime |  Zaman zaman bildirim gönderildi |
    $iothub-ileti-kaynak | twinChangeEvents |
    $content-kodlama | UTF-8 |
    deviceId | Cihaz kimliği |
    Modül kimliği | Modül kimliği |
    hubName | IOT hub'ının adı |
    operationTimestamp | [ISO8601] işleminin zaman damgası |
    ıothub ileti şeması | deviceLifecycleNotification |
    opType | "replaceTwin" veya "updateTwin" |

    İleti sistemi özelliklerini öneki ile `'$'` simgesi.

    - Gövde
        
    Bu bölüm bir JSON biçiminde tüm twin değişiklikleri içerir. Bir düzeltme eki aynı biçimi kullanır, farkı olan tüm twin bölümleri içerebilir: etiketleri, properties.reported, properties.desired ve "$metadata" öğeleri içerir. Örneğin,

    ```json
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ```

Önceki tüm işlemleri destekleyen [iyimser eşzamanlılık] [ lnk-concurrency] ve gerektiren **ServiceConnect** tanımlandığı şekilde izin [güvenlik] [ lnk-security] makalesi.

Bu işlemlerin yanı sıra çözüm arka ucu yapabilirsiniz:

* SQL benzeri kullanarak modülü çiftlerini sorgulamak [IOT hub'ı sorgu dili][lnk-query].

## <a name="module-operations"></a>Modül işlemleri
Modül uygulama aşağıdaki atomik işlemleri kullanarak modülü twin üzerinde çalışır:

* **Modül twin almak**. Bu işlem için şu anda bağlı Modülü (etiketleri ve istenen ve bildirilen sistem özellikleri dahil) modülü twin belgeyi döndürür.
* **Kısmen bildirilen özellikleri güncelleştirmek**. Bu işlem şu anda bağlı modülü bildirilen özelliklerini kısmi güncelleştirilmesini sağlar. Bu işlem çözümü istenen özelliklerinin kısmi güncelleştirme son kullanımları geri aynı JSON güncelleştirme biçimi kullanır.
* **İstenen özelliklerde gözlemlemek**. Şu anda bağlı modülü bunlar gerçekleştiğinde istenen özellikleri için güncelleştirmeler bildirilmesini seçebilirsiniz. Modül aynı formun çözüm arka ucu tarafından yürütülen Güncelleştirmesi (kısmi veya tam değiştirme) alır.

Önceki tüm işlemleri gerektiren **ModuleConnect** tanımlandığı şekilde izin [güvenlik] [ lnk-security] makalesi.

[Azure IOT cihaz SDK'ları] [ lnk-sdks] kolaylaştıran birçok diller ve platformlar önceki işlemlerinden kullanın.

## <a name="tags-and-properties-format"></a>Etiketleri ve özellikleri biçimi
Etiketler, istenen özellikleri ve bildirilen özellikleri JSON nesnelerinin aşağıdaki kısıtlamalarla şunlardır:

* JSON nesnelerinin tüm anahtarları büyük küçük harfe duyarlı 64 baytı UTF-8 UNICODE dizeleri içindedir. Karakter hariç UNICODE denetim karakterleri (kesimleri C0 ve C1) izin verilen ve `'.'`, `' '`, ve `'$'`.
* JSON nesnelerinin tüm değerleri aşağıdaki JSON türde olabilir: boolean, sayı, dize, nesne. Diziler izin verilmiyor. Tamsayı değeri en fazla 4503599627370495 ve tamsayılar için en düşük değer-4503599627370496.
* Etiketler, istenen ve bildirilen özellikleri tüm JSON nesnelerinin maksimum derinliği 5 olabilir. Örneğin, aşağıdaki nesne geçerli değil:

    ```json
    {
        ...
        "tags": {
            "one": {
                "two": {
                    "three": {
                        "four": {
                            "five": {
                                "property": "value"
                            }
                        }
                    }
                }
            }
        },
        ...
    }
    ```

* Tüm dize değeri uzunluğu en fazla 4 KB olabilir.

## <a name="module-twin-size"></a>Modül twin boyutu
IOT hub'ı zorlayan bir 8KB boyutu sınırlaması her ilgili toplam değerlerinin `tags`, `properties/desired`, ve `properties/reported`, salt okunur öğeleri hariç.
Boyutu UNICODE denetim karakterleri (kesimleri C0 ve C1) hariç tüm karakterleri sayma tarafından hesaplanır ve dize sabitleri dışında olan alanları.
IOT hub'ı bir hata ile bu belgelerin sınırı üstünde boyutunu artırır tüm işlemleri reddeder.

## <a name="module-twin-metadata"></a>Modül twin meta verileri
IOT hub'ı son güncelleştirme modülü çiftine her bir JSON nesnesi için zaman damgasını istenen ve Özellikler bildirilen korur. Zaman damgaları UTC biçimindedir ve kodlanmış [ISO8601] biçimi `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Örneğin:

```json
{
    ...
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": {
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$lastUpdated": "2016-03-30T16:24:48.789Z"
                },
                "$lastUpdated": "2016-03-30T16:24:48.789Z"
            },
            "$version": 23
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            "batteryLevel": "55%",
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": "5m",
                    "status": {
                        "$lastUpdated": "2016-03-31T16:35:48.789Z"
                    },
                    "$lastUpdated": "2016-03-31T16:35:48.789Z"
                }
                "batteryLevel": {
                    "$lastUpdated": "2016-04-01T16:35:48.789Z"
                },
                "$lastUpdated": "2016-04-01T16:24:48.789Z"
            },
            "$version": 123
        }
    }
    ...
}
```

Bu bilgiler, Nesne anahtarları kaldırmak güncelleştirmeleri korumak için her düzeyde (yalnızca JSON yapısındaki bırakır) tutulur.

## <a name="optimistic-concurrency"></a>İyimser eşzamanlılık
Etiketler, istenen ve tüm destek iyimser eşzamanlılık özellikleri bildirdi.
Etiketler göre bir ETag sahip [RFC7232], etiketin JSON gösterimi temsil eden. Tutarlılık sağlamak için Etag'ler çözüm arka ucu koşullu güncelleştirme işlemlerini kullanabilirsiniz.

Modül twin istenen ve Özellikler rapor Etag'ler gerekmez, ancak sahip bir `$version` artımlı olması garanti değeri. Benzer şekilde bir ETag öğesine Sürüm güncelleştirme tarafın güncelleştirmeleri tutarlılığı zorlamak için kullanılabilir. Örneğin, bir modül uygulaması bildirilen bir özellik ya da istenen bir özellik için çözüm arka ucu için.

Bir gözlemci Aracı (örneğin, İstenen özelliklerde Gözlemleme modülü uygulama) alma işleminin sonucu bir güncelleştirme bildirimi arasındaki diğerleriyle mutabık kılmak sürümleri de yararlı olur. [Aygıt yeniden bağlanmayı akış] bölümünde [lnk-yeniden bağlanma] daha fazla bilgi sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğreticileri bakın:

* [.NET yedekleme ve .NET aygıt kullanarak IOT hub'ı modülü kimlik ve modül çifti ile çalışmaya başlama][lnk-module-twin-tutorial]

<!-- links and images -->

[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232

[lnk-module-twin-tutorial]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-module-twin-metadata]: iot-hub-devguide-module-twins.md#module-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[img-module-twin]: media/iot-hub-devguide-device-twins/module-twin.jpg
