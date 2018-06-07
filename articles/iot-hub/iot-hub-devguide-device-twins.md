---
title: Azure IOT Hub cihaz çiftlerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu - kullanım cihaz çiftlerini IOT Hub ve aygıtlarınızın arasında durumu ve yapılandırma verileri eşitlemek için
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: c002685dfd3b8f86a8657b5d30dee29641cef932
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632885"
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Anlama ve IOT hub'da cihaz çiftlerini kullanın

*Cihaz çiftlerini* meta verileri, yapılandırmaları ve koşullar dahil olmak üzere cihaz durumu bilgilerini depolamak JSON belgeleri. Azure IOT Hub cihaz çifti IOT Hub'ına bağlanan her aygıt için tutar. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu makalede açıklanır:

* Cihaz çifti yapısını: *etiketleri*, *istenen* ve *özellikleri bildirilen*.
* Cihaz uygulamalarını hem de arka uçları cihaz çiftlerini üzerinde gerçekleştirebileceğiniz işlemler.

Cihaz çiftlerini kullanın:

* Aygıta özgü meta verileri bulutta depolayın. Örneğin, bir satış makinenin dağıtım konumu.
* Rapor geçerli durumu bilgileri kullanılabilir özellikleri ve aygıt uygulamanızdan koşulları gibi. Örneğin, bir cihaz IOT hub'ınıza bağlı üzerinden cep telefonu veya WiFi.
* Cihaz uygulaması ve arka uç uygulaması arasında uzun süre çalışan iş akışlarının durumunu eşitleyin. Örneğin, çözüm yedeklediğinizde yüklemek için yeni üretici yazılımı sürümüne sonunu belirtir ve güncelleştirme işlemi çeşitli aşamaları cihaz uygulaması raporlar.
* Cihaz meta verilerini, yapılandırma veya durumu sorgu.

Başvurmak [cihaz bulut iletişimi Kılavuzu] [ lnk-d2c-guidance] bildirilen özellikleri, cihaz bulut iletilerini veya karşıya dosya yükleme kullanma konusunda yönergeler için.
Başvurmak [bulut-cihaz iletişimi Kılavuzu] [ lnk-c2d-guidance] istenen özellikler, doğrudan yöntemleri veya Bulut-cihaz iletilerini kullanma konusunda yönergeler için.

## <a name="device-twins"></a>Cihaz çiftlerini
Cihaz çiftlerini cihaz ile ilgili bilgileri depolar:

* Cihaz ve arka uç aygıtı koşullar ve yapılandırma eşitlemek için kullanabilirsiniz.
* Çözüm arka ucu sorgu ve uzun süre çalışan hedef için kullanabileceğiniz işlemleri.

Karşılık gelen cihaz çifti yaşam döngüsü bağlı [cihaz kimliği][lnk-identity]. Cihaz çiftlerini örtük olarak oluşturulur ve bir cihaz kimliği oluşturulduğunda veya IOT hub'da silinmiş silinir.

Cihaz çifti içeren bir JSON belgesi şöyledir:

* **Etiketleri**. Çözüm arka ucu JSON belgesinin bir bölümünü okuma ve yazma. Etiketler cihaz uygulamaları için görünür değildir.
* **Özellikler istenen**. Aygıt Yapılandırması veya koşulları eşitlemek için bildirilen özellikleriyle birlikte kullanılır. Cihaz uygulaması okuyabilir ve çözüm arka ucu istenen özellikleri ayarlayabilirsiniz. Cihaz uygulaması, ayrıca istenen özelliklerinde değişikliklerin bildirimleri alabilirsiniz.
* **Özellikler bildirilen**. Aygıt Yapılandırması veya koşulları eşitlemek için istediğiniz özellikleri ile birlikte kullanılır. Cihaz uygulaması bildirilen özellikleri ayarlayabilirsiniz ve çözüm arka ucu okuyabilir ve bunları sorgulayabilirsiniz.
* **Cihaz kimlik özellikleri**. Cihaz çifti JSON belgesi kökündeki saklanan karşılık gelen bir cihaz kimliği salt okunur özelliklerinden içeren [kimlik kayıt defteri][lnk-identity].

![][img-twin]

Aşağıdaki örnek, bir cihaz çifti JSON belgesi gösterir:

```json
{
    "deviceId": "devA",
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

Kök nesnesinde cihaz kimlik özelliklerdir ve kapsayıcı nesneleri için `tags` ve her ikisi de `reported` ve `desired` özellikleri. `properties` Kapsayıcısı bazı salt okunur öğeleri içerir (`$metadata`, `$etag`, ve `$version`) açıklanan [cihaz çifti meta verilerini] [ lnk-twin-metadata] ve [ İyimser eşzamanlılık] [ lnk-concurrency] bölümler.

### <a name="reported-property-example"></a>Bildirilen özelliği örneği
Önceki örnekte, cihaz çifti içeren bir `batteryLevel` cihaz uygulaması tarafından bildirilen özelliği. Bu özellik, sorgu ve son bildirilen pil düzeyi temelinde cihazlarda çalıştırmak mümkün kılar. Cihaz uygulama raporlama cihaz özellikleri veya bağlantı seçenekleri diğer örnek olarak verilebilir.

> [!NOTE]
> Bildirilen özellikler çözüm arka ucu bir özelliğin bilinen son değerini nerede ilgilendiği senaryoları basitleştirin. Kullanım [cihaz bulut iletilerini] [ lnk-d2c] çözüm arka ucu cihaz telemetrisi zaman serisi gibi zaman damgalı olaylar dizisini biçiminde işlemek gerekip gerekmediğini.

### <a name="desired-property-example"></a>İstenen özelliği örneği
Önceki örnekte, `telemetryConfig` cihaz çifti istenen ve bildirilen özellikler çözüm arka ucu ve cihaz uygulaması tarafından bu cihaz için telemetri yapılandırma eşitlemek için kullanılır. Örneğin:

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

2. Cihaz uygulaması hemen bağlıysa değişiklik ya da ilk yeniden bağlanma sırasında bildirilir. Cihaz uygulaması güncelleştirilmiş yapılandırmayı daha sonra raporlar (veya bir hata koşulu kullanarak `status` özelliği). Bildirilen özellikleri kısmı şöyledir:

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

3. Çözüm arka ucu yapılandırma işleminin sonuçlarını birçok cihaz arasında göre izleyebilirsiniz [sorgulama] [ lnk-query] cihaz çiftlerini.

> [!NOTE]
> Önceki kod parçacıkları örnek, bir aygıt yapılandırmasını ve durumunu kodlamak için bir yol okunabilmesi için en iyi duruma getirilmiş verilebilir. Cihaz çifti istenen ve cihaz çiftlerini özelliklerinde rapor için IOT hub'ı belirli bir şema getirmez.
> 
> 

Bellenim güncelleştirmeleri gibi uzun süre çalışan işlemleri eşitlemek için çiftlerini kullanabilirsiniz. Özellikler eşitleyin ve cihaz üzerinden uzun süre çalışan bir işlemi izlemek için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [kullanmak istediğiniz cihazları yapılandırmak için Özellikler][lnk-twin-properties].

## <a name="back-end-operations"></a>Arka plan işlemleri
Çözüm arka ucu HTTPS kullanıma sunulan aşağıdaki atomik işlemleri kullanarak cihaz çifti çalıştırır:

* **Cihaz çifti Kimliğine göre almak**. Bu işlem, etiketler ve istenen ve bildirilen sistem özellikleri dahil olmak üzere cihaz çifti belge döndürür.
* **Cihaz çifti kısmen güncelleştirmek**. Bu işlem etiketler veya cihaz çifti istenen özelliklerinde kısmen güncelleştirmek çözüm arka ucu sağlar. Kısmi güncelleştirmeyi ekler veya herhangi bir özellik güncelleştirmeleri bir JSON belgesi olarak ifade edilir. Özelliklerini ayarlamak `null` kaldırılır. Aşağıdaki örnek yeni bir istenen özellik değeri ile oluşturur `{"newProperty": "newValue"}`, var olan değerini geçersiz kılar `existingProperty` ile `"otherNewValue"`ve kaldırır `otherOldProperty`. Herhangi bir değişiklik mevcut istenen özellikleri veya etiketleri oluşturulur:

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

* SQL benzeri kullanarak cihaz çiftlerini sorgulamak [IOT hub'ı sorgu dili][lnk-query].
* Büyük kümelerini kullanarak cihaz çiftlerini işlemleri [işleri][lnk-jobs].

## <a name="device-operations"></a>Aygıt işlemleri
Cihaz uygulaması aşağıdaki atomik işlemleri kullanarak cihaz çifti çalışır:

* **Cihaz çifti almak**. Bu işlem şu anda bağlı cihaz için (etiketleri ve istenen ve bildirilen sistem özellikleri dahil) cihaz çifti belgeyi döndürür.
* **Kısmen bildirilen özellikleri güncelleştirmek**. Bu işlem şu anda bağlı bir aygıt bildirilen özelliklerini kısmi güncelleştirilmesini sağlar. Bu işlem çözümü istenen özelliklerinin kısmi güncelleştirme son kullanımları geri aynı JSON güncelleştirme biçimi kullanır.
* **İstenen özelliklerde gözlemlemek**. Şu anda bağlı bir aygıt bunlar gerçekleştiğinde istenen özellikleri için güncelleştirmeler bildirilmesini seçebilirsiniz. Cihaz çözüm arka ucu tarafından yürütülen Güncelleştirmesi (kısmi veya tam değiştirme) aynı biçiminde alır.

Önceki tüm işlemleri gerektiren **DeviceConnect** tanımlandığı şekilde izin [güvenlik] [ lnk-security] makalesi.

[Azure IOT cihaz SDK'ları] [ lnk-sdks] kolaylaştıran birçok diller ve platformlar önceki işlemlerinden kullanın. İstenen özelliklerde eşitleme için IOT hub'ı elemanlar ayrıntıları hakkında daha fazla bilgi için bkz: [aygıt yeniden bağlanmayı akış][lnk-reconnection].

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

## <a name="device-twin-size"></a>Cihaz çifti boyutu
IOT hub'ı zorlayan bir 8KB boyutu sınırlaması her ilgili toplam değerlerinin `tags`, `properties/desired`, ve `properties/reported`, salt okunur öğeleri hariç.
Boyutu UNICODE denetim karakterleri (kesimleri C0 ve C1) hariç tüm karakterleri sayma tarafından hesaplanır ve dize sabitleri dışında olan alanları.
IOT hub'ı bir hata ile bu belgelerin sınırı üstünde boyutunu artırır tüm işlemleri reddeder.

## <a name="device-twin-metadata"></a>Cihaz çifti meta veriler
IOT Hub cihaz çiftine her bir JSON nesnesi için son güncelleştirme zaman damgası istenen ve Özellikler bildirilen korur. Zaman damgaları UTC biçimindedir ve kodlanmış [ISO8601] biçimi `YYYY-MM-DDTHH:MM:SS.mmmZ`.
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

Cihaz çifti istenen ve Özellikler rapor Etag'ler gerekmez, ancak sahip bir `$version` artımlı olması garanti değeri. Benzer şekilde bir ETag öğesine Sürüm güncelleştirme tarafın güncelleştirmeleri tutarlılığı zorlamak için kullanılabilir. Örneğin, bir cihaz uygulaması bildirilen bir özellik ya da istenen bir özellik için çözüm arka ucu için.

Bir gözlemci Aracı (örneğin, İstenen özelliklerde Gözlemleme cihaz uygulaması) alma işleminin sonucu bir güncelleştirme bildirimi arasındaki diğerleriyle mutabık kılmak sürümleri de yararlı olur. Bölüm [aygıt yeniden bağlanmayı akış] [ lnk-reconnection] daha fazla bilgi sağlar.

## <a name="device-reconnection-flow"></a>Cihaz yeniden bağlanmayı akışı
IOT Hub bağlantısı kesilmiş aygıtları için istenen özellikler güncelleştirme bildirimlerini korumaz. Bu, bağlanan bir aygıt için güncelleştirme bildirimlerini abone yanı sıra tam istenen özellikleri belgenin almanız gerekir izler. Güncelleştirme bildirimlerini ve tam alma arasında diğerleriyle olasılığını göz önüne alındığında, aşağıdaki akış güvence altına gerekir:

1. Cihaz uygulaması bir IOT hub'ına bağlanır.
2. Cihaz uygulaması istenen özelliklerini güncelleştirme bildirimlerini abone olur.
3. Cihaz uygulamasının tam belgenin istenen özellikleri alır.

Cihaz uygulaması ile tüm bildirimleri yoksayabilirsiniz `$version` tam alınan belge sürümünden eşit veya daha az. Bu yaklaşım olası nedeni IOT hub'ı sürümleri her zaman artırmak güvence altına alır.

> [!NOTE]
> Bu mantık zaten uygulanan [Azure IOT cihaz SDK'ları][lnk-sdks]. Bu açıklama, yalnızca cihaz uygulaması Azure IOT cihaz SDK'ları hiçbirini kullanamazsınız ve MQTT arabirimi doğrudan program gerekir yararlı olur.
> 
> 

## <a name="additional-reference-material"></a>Ek başvuru bilgileri
IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] makalede çeşitli uç noktaları her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunar.
* [Azaltma ve kotaları] [ lnk-quotas] makale IOT Hub hizmeti ve azaltma davranışı hizmetini kullandığınızda beklediğiniz uygulama kotaları açıklar.
* [Azure IOT SDK'ları cihazını ve hizmetini] [ lnk-sdks] makale çeşitli dil IOT Hub ile etkileşim hem cihaz hem de hizmet uygulamaları geliştirirken kullanabilir SDK'ları listeler.
* [IOT Hub cihaz çiftlerini, işler ve ileti yönlendirme için sorgu dili] [ lnk-query] makalede IOT Hub'ından, cihaz çiftlerini ve işleri hakkında bilgi almak için kullanabileceğiniz IOT hub'ı sorgu dili.
* [IOT Hub MQTT Destek] [ lnk-devguide-mqtt] makale MQTT protokolü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, öğrenilen cihaz çiftlerini hakkında aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilgilenebilirsiniz:

* [Anlama ve IOT hub'ında modülü çiftlerini kullanın][lnk-module-twins]
* [Bir cihazda doğrudan bir yöntem çağırma][lnk-methods]
* [Birden çok aygıta işleri zamanla][lnk-jobs]

Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğreticileri bakın:

* [Cihaz çifti kullanma][lnk-twin-tutorial]
* [Cihaz çifti özellikleri kullanma][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-module-twins]:iot-hub-devguide-module-twins.md

[img-twin]: media/iot-hub-devguide-device-twins/twin.png
