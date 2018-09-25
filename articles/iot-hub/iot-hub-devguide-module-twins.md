---
title: Azure IOT hub'ı modül ikizlerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub ve cihazlarınız arasında durum ve yapılandırma verilerini eşitlemek için modül ikizlerini kullanma
author: chrissie926
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: 0df1170079e66bda95e38bcf17dcce738269eeb0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039392"
---
# <a name="understand-and-use-module-twins-in-iot-hub"></a>Anlama ve IOT Hub'ında modül ikizlerini kullanma

Bu makalede okuduğunuz varsayılır [anlamak ve IOT Hub'ında cihaz ikizlerini kullanma] [ lnk-devguide-device-twins] ilk. IOT Hub'ında her bir cihaz kimliği altında en fazla 20 modülü kimlikleri oluşturabilirsiniz. Her bir modül kimliği, bir modül ikizi örtük olarak oluşturur. Cihaz ikizlerini çok benzer, modül ikizlerini meta veriler, yapılandırmalar ve koşullar dahil olmak üzere modülün durum bilgilerini depolayan JSON belgelerdir. Azure IOT Hub, IOT Hub'ına bağladığınız her bir modül için bir modül ikizi tutar. 

Cihaz tarafında, IOT Hub cihazı SDK'ları olanak tanır, her IOT hub'ı bağımsız bir bağlantı açar modülleri oluşturabilir. Bu, Cihazınızda farklı bileşenleri için ayrı ad alanları kullanmanıza olanak sağlar. Örneğin, üç farklı algılayıcılar bir satış makine var. Her algılayıcı, şirketinizdeki farklı Departmanlar tarafından denetlenir. Her algılayıcı için bir modül oluşturabilirsiniz. Bu şekilde, her bölüm yalnızca bunlar çakışmalar veya kullanıcı hatalarından kaçınmak denetimi algılayıcı için işler veya doğrudan yöntemler göndermek kullanabilirsiniz.

 Modül kimliği ve modül ikizi cihaz kimliği ve cihaz ikizi ancak daha iyi tanecikli aynı özellikleri sağlar. Cihazlar veya üretici yazılımı cihazları yapılandırma ve koşullar için bu bileşenlerin her birini yalıtmak için birden çok bileşen yönetme işletim sistemlerinde olduğu gibi bu daha iyi tanecikli özellikli cihazlarda sağlar. Modül kimliği ve modül ikizlerini yönetim ayrımı nettir modüler yazılım bileşenleri IOT cihazları ile çalışırken sağlar. Biz, modül ikizi genel kullanılabilirlik tarafından modül ikizi düzeyinde tüm cihaz ikizi işlevselliği destekleyen en hedeflenir. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu makalede açıklanır:

* Modül ikizi yapısını: *etiketleri*, *istenen* ve *bildirilen özellikler*.
* Modüller ve arka uçları modül ikizlerini üzerinde gerçekleştirebileceğiniz işlemler.

Başvurmak [CİHAZDAN buluta iletişim Kılavuzu] [ lnk-d2c-guidance] bildirilen özellikleri, CİHAZDAN buluta iletileri ya da karşıya dosya yükleme kullanma yönergeleri için.
Başvurmak [bulut buluttan cihaza iletişim Kılavuzu] [ lnk-c2d-guidance] istenen özellikler, doğrudan yöntemler ve bulut-cihaz iletilerini kullanma yönergeleri için.

## <a name="module-twins"></a>Modül ikizlerini
Modül ikizlerini modülü ile ilgili bilgileri depolar:

* IOT Hub ve cihaz modülleri modülü koşullar ve yapılandırma eşitlemek için kullanabilirsiniz.
* Çözüm arka ucu, sorgu ve uzun süre çalışan hedef için kullanabileceğiniz işlemleri.

Modül ikizi yaşam döngüsü karşılık gelen bağlı [modülü kimlik][lnk-identity]. Modül ikizlerini örtük olarak oluşturulur ve bir modül kimliği oluşturulduğunda veya IOT Hub'ında silinmiş silinir.

Modül ikizi içeren bir JSON belgesidir:

* **Etiketleri**. Çözüm arka ucu, okuma ve yazma JSON belgesinin bölümü. Etiketler, cihazdaki modüller için görünür değildir. Etiketler, amacı sorgulama için ayarlanır.
* **İstenen özellikleri**. Modül yapılandırması veya koşulları eşitlemek için bildirilen özellikleriyle birlikte kullanılır. Modül uygulama okuyabilirsiniz ve çözüm arka ucu, istenen özellikleri ayarlayabilirsiniz. Modülü uygulama, ayrıca değişiklik bildirimleri istenen özellikleri alabilir.
* **Bildirilen özellikler**. Modül yapılandırması veya koşulları eşitlemek için istenen özellikleri ile birlikte kullanılır. Modül uygulama bildirilen özellikleri ayarlayabilirsiniz ve çözüm arka ucu okuyun ve bunları sorgulayabilirsiniz.
* **Modülü kimlik özellikleri**. Modül ikizi JSON belgesinin kök saklanan karşılık gelen bir modül kimliği salt okunur özelliklerini içeren [kimlik kayıt defteri][lnk-identity].

![][img-module-twin]

Aşağıdaki örnek, bir modül ikizi JSON belgesini gösterir:

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

Kapsayıcı nesneleri için ve kök nesne modülü kimlik özelliklerdir `tags` ve her ikisi de `reported` ve `desired` özellikleri. `properties` Kapsayıcı bazı salt okunur öğeleri içerir (`$metadata`, `$etag`, ve `$version`) açıklanan [modül ikizi meta verileri] [ lnk-module-twin-metadata] ve [ İyimser eşzamanlılık] [ lnk-concurrency] bölümler.

### <a name="reported-property-example"></a>Bildirilen özellik örneği
Önceki örnekte, modül ikizi içeren bir `batteryLevel` modülü uygulama tarafından bildirilen özellik. Bu özellik, sorgu ve son bildirilen pil düzeyini temel alan modüller işlem mümkün kılar. Diğer örnekler modülü uygulama raporlama modülü özellikleri veya bağlantı seçenekleri içerir.

> [!NOTE]
> Bildirilen özellikler, çözüm arka ucu bir özelliğin son bilinen değer burada ilgilendiği senaryoları basitleştirin. Kullanım [CİHAZDAN buluta iletileri] [ lnk-d2c] çözüm arka ucu modülü telemetri zaman serisi gibi zaman damgalı olaylar dizisini biçiminde işlemek gerekiyorsa.

### <a name="desired-property-example"></a>İstenen özellik örneği
Önceki örnekte, `telemetryConfig` modül ikizi istenen ve bildirilen özellikleri modülü uygulama ve çözüm arka ucu ile bu modül için telemetri yapılandırma eşitlemek için kullanılır. Örneğin:

1. Çözüm arka ucu, istenen yapılandırma değeri ile istenen özelliği ayarlar. İstenen özellik kümesine sahip bir belge bölümü aşağıda verilmiştir:

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

2. Modül uygulamayı hemen bağlıysa değişiklik veya ilk yeniden bildirilir. Modül uygulama daha sonra güncelleştirilmiş yapılandırmayı raporlar (veya bir hata koşulunu kullanarak `status` özelliği). Bildirilen özellikler bölümü aşağıda verilmiştir:

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

3. Çözüm arka ucu yapılandırma işlemi sonuçlarını birçok modülleri arasında göre izleyebilirsiniz [sorgulama] [ lnk-query] modül ikizlerini.

> [!NOTE]
> Yukarıdaki örnek, bir modül yapılandırması ve durumunu kodlama yollarından biri okunabilirlik için en iyi duruma getirilmiş parçacıklarıdır. Modül ikizi istenen ve modül ikizlerini özelliklerinde bildirilen IOT hub'ı belirli bir şemaya uygulamaz.
> 
> 

## <a name="back-end-operations"></a>Arka uç işlemleri
Çözüm arka ucu kullanarak HTTPS sunulan aşağıdaki atomik işlemler, modül ikizi çalışır:

* **Modül ikizi Kimliğe göre Al**. Bu işlem, etiketler ve istenen ve bildirilen sistem özellikleri dahil olmak üzere modül ikizi belgeyi döndürür.
* **Modül ikizi kısmen güncelleştirmek**. Bu işlem, etiketler ya da bir modül ikizi istenen özelliği kısmen güncelleştirmek çözüm arka ucu sağlar. Kısmi güncelleştirmeyi ekler veya güncelleştirir herhangi bir özelliği bir JSON belgesi olarak ifade edilir. Özelliklerini ayarlamak `null` kaldırılır. Aşağıdaki örnek yeni bir istenen özellik değeriyle oluşturur `{"newProperty": "newValue"}`, mevcut değerini geçersiz kılar `existingProperty` ile `"otherNewValue"`ve kaldırır `otherOldProperty`. Başka bir değişiklik, mevcut istenen özellikleri veya etiketleri oluşturulur:

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

* **İstenen özellikleri değiştirin**. Bu işlemi tamamen mevcut tüm istenen özellikleri üzerine ve yeni bir JSON belgesi için alternatif çözüm arka ucu sağlar `properties/desired`.
* **Etiketleri değiştirin**. Bu işlemi tamamen tüm mevcut etiketlerin üzerine yazarsınız ve yeni bir JSON belgesi için alternatif çözüm arka ucu sağlar `tags`.
* **İkiz bildirimlerin**. Bu işlem ikizi değiştirildiğinde bildirim almak çözüm arka ucu sağlar. Bunu yapmak için IOT çözümünüzün yönlendirme oluşturma ve veri kaynağı eşit ayarlamak için gereken *twinChangeEvents*. Varsayılan olarak, hiçbir ikizi bildirimler gönderilir, diğer bir deyişle, bu tür bir yol önceden mevcut. Değişim oranı çok yüksek olup olmadığını veya iç hatalar gibi diğer nedenlerle, IOT hub'ı tüm değişiklikleri içeren tek bir bildirim gönderebilir. Bu nedenle, uygulamanızın güvenilir denetim ve tüm ara durumlarını günlük gerekiyorsa, CİHAZDAN buluta iletileri kullanmanız gerekir. İkiz bildirim iletisi, özellikleri ve gövdesini içerir.

    - Özellikler

    | Ad | Değer |
    | --- | --- |
    $content-türü | uygulama/json |
    $iothub-enqueuedtime |  Bildirim zaman gönderildiği zaman |
    $iothub-message-kaynak | twinChangeEvents |
    $content-kodlama | UTF-8 |
    deviceId | Cihaz kimliği |
    Modül kimliği | Modül kimliği |
    HubName | IOT hub'ı adı |
    operationTimestamp | [ISO8601] işleminin zaman damgası |
    ıothub ileti şeması | deviceLifecycleNotification |
    opType | "replaceTwin" veya "updateTwin" |

    İleti sistemi özellikleri önekiyle `'$'` simgesi.

    - Gövde
        
    Bu bölüm, JSON biçiminde tüm ikizi değişiklikleri içerir. Bir düzeltme eki aynı biçimi kullanır, farkı olan tüm ikizi bölümleri içerebilir: etiketler, properties.reported, properties.desired ve "$metadata" öğeleri içerir. Örneğin,

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

Önceki tüm işlemleri destekleyen [iyimser eşzamanlılık] [ lnk-concurrency] ve gerekli **ServiceConnect** tanımlandığı gibi izni [güvenlik] [ lnk-security] makalesi.

Bu işlemlere ek olarak çözüm arka ucu şunları yapabilir:

* Modül ikizlerini kullanarak SQL benzeri sorgu [IOT Hub sorgu dili][lnk-query].

## <a name="module-operations"></a>Modül işlemleri
Modül uygulama aşağıdaki atomik işlemleri kullanarak modül ikizi üzerinde çalışır:

* **Modül ikizi almak**. Bu işlem için şu anda bağlı Modülü (etiketler ve istenen ve bildirilen sistem özellikleri dahil) modül ikizi belgeyi döndürür.
* **Kısmen bildirilen özellikleri güncelleştirmek**. Bu işlem şu anda bağlı modülü bildirilen özelliklerinin kısmi güncelleştirme sağlar. Bu işlem, çözüm istenen özellikler kısmen güncelleştirmek için kullandığı uç geri JSON güncelleştirme biçimini kullanır.
* **İstenen özellikleri gözlemleyin**. Şu anda bağlı modülü, İstenen özelliklerde yapılan güncelleştirmelerin gerçekleştiğinde bildirim almak seçebilirsiniz. Modül, formun çözüm arka ucu tarafından yürütülen Güncelleştirmesi (kısmi veya tam değiştirme) alır.

Önceki tüm işlemleri gerektiren **ModuleConnect** tanımlandığı gibi izni [güvenlik] [ lnk-security] makalesi.

[Azure IOT cihaz SDK'ları] [ lnk-sdks] birçok dilleri ve platformları önceki işlemlerle kolaylaştırır.

## <a name="tags-and-properties-format"></a>Etiketler ve Özellikler biçimi
Aşağıdaki kısıtlamalarla JSON nesneleri, etiketler ve istenen özellikler bildirilen özellikleri şunlardır:

* Tüm JSON nesneleri büyük/küçük harfe 64 baytın UTF-8 UNICODE dizelerini anahtarlarıdır. İzin verilen karakterleri UNICODE denetim karakterlerini (segmentler C0 ve C1) hariç tutmak ve `'.'`, `' '`, ve `'$'`.
* JSON nesneleri tüm değerleri aşağıdaki JSON türde olabilir: boolean, sayı, dize, nesne. Diziler izin verilmez. Tamsayılar için maksimum değer 4503599627370495 ve tamsayılar için en düşük değer-4503599627370496.
* Etiketler, istenen ve bildirilen özellikler tüm JSON nesneleri 5 en yüksek derinliği olabilir. Örneğin, aşağıdaki nesne geçerli değil:

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

* Tüm dize değerleri, en fazla 512 bayt uzunluğunda olabilir.

## <a name="module-twin-size"></a>Modül ikizi boyutu
IOT hub'ı zorunlu bir 8 KB'lık boyut sınırlaması ilgili toplam değerlerinin her `tags`, `properties/desired`, ve `properties/reported`, salt okunur öğeleri hariç.
UNICODE denetim karakterlerini (segmentler C0 ve C1) hariç tüm karakter sayılarak boyutu hesaplanır ve dize sabitleri dışında olan alanları.
IOT Hub ile ilgili bir hata bu belgelerin üst sınırı boyutunu artırır tüm işlemleri reddeder.

## <a name="module-twin-metadata"></a>Modül ikizi meta verileri
IOT Hub, zaman damgası son güncelleştirme her JSON nesnesi, modül ikizi için istenen ve bildirilen özellikler oluşturur. Zaman damgaları UTC biçiminde ve kodlanmış [ISO8601] biçimi `YYYY-MM-DDTHH:MM:SS.mmmZ`.
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

Bu bilgiler, Nesne anahtarları kaldırma güncelleştirmeleri korumak için her düzeyde (yalnızca JSON yapısı Yapraklar) tutulur.

## <a name="optimistic-concurrency"></a>İyimser eşzamanlılık
Etiketler, istenen ve tüm destek iyimser eşzamanlılık bildirilen özellikler.
Etikete göre bir ETag sahip [RFC7232], etiketin JSON gösterimi, temsil eder. Tutarlılık sağlamak için Etag'ler çözüm arka ucu koşullu güncelleştirme işlemlerini kullanabilirsiniz.

Modül ikizi istenen ve bildirilen özellikler Etag'ler yoktur, ancak sahip bir `$version` artımlı olması garanti değeri. Benzer şekilde bir ETag öğesine sürümü güncelleştirme tarafça güncelleştirmeleri tutarlılığını zorlamak için kullanılabilir. Örneğin, bir modül uygulaması bildirilen özellik veya istenen özelliği için çözüm arka ucu için.

Gözlemci Aracı (örneğin, istenen özellikleri gözleme modülü uygulama) alma işleminin sonucu bir güncelleştirme bildirimi arasındaki yarışa hazır mutabık kılmanız gerekir sürümleri de yararlıdır. ' % S'bölümü [cihaz yeniden akış] [lnk-yeniden bağlanma] daha fazla bilgi sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki öğreticilerde IOT Hub bakın:

* [.NET arka ucu ve cihaz .NET kullanarak IOT hub'ı modül kimlik ve modül ikizi ile çalışmaya başlama][lnk-module-twin-tutorial]

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
