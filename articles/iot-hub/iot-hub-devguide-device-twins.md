---
title: Azure IOT Hub cihaz ikizlerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub ve cihazlarınız arasında durum ve yapılandırma verilerini eşitlemek için cihaz ikizlerini kullanma
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 883e81572218e39d84ad8793423b02468d49d00a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61322550"
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Anlama ve IOT Hub'ında cihaz ikizlerini kullanma

*Cihaz ikizlerini* meta veriler, yapılandırmalar ve koşullar gibi cihaz durumu bilgilerini depolayan JSON belgelerdir. Azure IOT Hub, IOT Hub'ına bağladığınız her cihaz için bir cihaz çifti tutar. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu makalede açıklanır:

* Cihaz ikizi yapısını: *etiketleri*, *istenen* ve *bildirilen özellikler*.
* Cihaz uygulamaları ve arka uçları cihaz ikizlerini üzerinde gerçekleştirebileceğiniz işlemler.

Cihaz ikizlerini kullanın:

* Cihaza özgü meta veriler bulutta Store. Örneğin, bir satış makinenin dağıtım konumu.

* Rapor geçerli durumu bilgileri kullanılabilir yetenekler ve cihaz uygulamanızı koşulları gibi. Örneğin, bir cihaz IOT hub'ınıza bağlı WiFi veya hücresel üzerinden.

* Cihaz uygulaması ve arka uç uygulaması arasında uzun süre çalışan iş akışları durumunu eşitleyin. Örneğin, çözüm yedeklediğinizde son yüklemek için yeni bellenim sürümünü belirtir ve cihaz uygulamasını güncelleştirme işleminin çeşitli aşamaları boyunca bildirir.

* Cihaz meta verilerini, yapılandırma ya da durum sorgulayın.

Başvurmak [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) bildirilen özellikleri, CİHAZDAN buluta iletileri ya da karşıya dosya yükleme kullanma yönergeleri için.

Başvurmak [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md) istenen özellikler, doğrudan yöntemler ve bulut-cihaz iletilerini kullanma yönergeleri için.

## <a name="device-twins"></a>Cihaz ikizlerini

Cihaz ikizlerini cihazlarla ilgili bilgileri depolar:

* Cihaz ve arka ucu, cihaz koşullar ve yapılandırma eşitlemek için kullanabilirsiniz.

* Çözüm arka ucu, sorgu ve uzun süre çalışan hedef için kullanabileceğiniz işlemleri.

Cihaz ikizi yaşam döngüsü karşılık gelen bağlı [cihaz kimliği](iot-hub-devguide-identity-registry.md). Cihaz ikizlerini örtük olarak oluşturulur ve bir cihaz kimliği oluşturulduğunda veya IOT Hub'ında silinmiş silinir.

Cihaz ikizi içeren bir JSON belgesidir:

* **Etiketleri**. Çözüm arka ucu, okuma ve yazma JSON belgesinin bölümü. Etiketler cihaz uygulamaları için görünür değildir.

* **İstenen özellikleri**. Cihaz yapılandırması veya koşulları eşitlemek için bildirilen özellikleriyle birlikte kullanılır. Cihaz uygulamasını okuyabilirsiniz ve çözüm arka ucu, istenen özellikleri ayarlayabilirsiniz. Cihaz uygulaması, istenen özelliklerinde değişiklik bildirimleri de alabilir.

* **Bildirilen özellikler**. Cihaz yapılandırması veya koşulları eşitlemek için istenen özellikleri ile birlikte kullanılır. Cihaz uygulaması, bildirilen özellikleri ayarlayabilirsiniz ve çözüm arka ucu okuyun ve bunları sorgulayabilirsiniz.

* **Cihaz kimlik özelliklerini**. Cihaz ikizi JSON belgesinin kök saklanan karşılık gelen bir cihaz kimliği salt okunur özelliklerini içeren [kimlik kayıt defteri](iot-hub-devguide-identity-registry.md).

![Cihaz ikizi özellikleri ekran görüntüsü](./media/iot-hub-devguide-device-twins/twin.png)

Aşağıdaki örnek, bir cihaz ikizi JSON belgesini gösterir:

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
            },
            "batteryLevel": 55,
            "$metadata" : {...},
            "$version": 4
        }
    }
}
```

Kapsayıcı nesneleri için ve kök nesne cihaz kimlik özelliklerdir `tags` ve her ikisi de `reported` ve `desired` özellikleri. `properties` Kapsayıcı bazı salt okunur öğeleri içerir (`$metadata`, `$etag`, ve `$version`) açıklanan [cihaz ikizi meta verilerini](iot-hub-devguide-device-twins.md#device-twin-metadata) ve [iyimser eşzamanlılık](iot-hub-devguide-device-twins.md#optimistic-concurrency) bölümler.

### <a name="reported-property-example"></a>Bildirilen özellik örneği

Önceki örnekte, cihaz ikizi içeren bir `batteryLevel` cihaza uygulama tarafından bildirilen özellik. Bu özellik, sorgu ve son bildirilen pil düzeyine bağlı cihazlarda çalıştırmak mümkün kılar. Diğer, cihaz uygulama raporlama cihaz özellikleri veya bağlantı seçenekleri örneklerindendir.

> [!NOTE]
> Bildirilen özellikler, çözüm arka ucu bir özelliğin son bilinen değer burada ilgilendiği senaryoları basitleştirin. Kullanım [CİHAZDAN buluta iletileri](iot-hub-devguide-messages-d2c.md) cihaz telemetrisi zaman serisi gibi zaman damgalı olaylar dizisini biçiminde işlemek çözüm arka ucu gerekip gerekmediğini.

### <a name="desired-property-example"></a>İstenen özellik örneği

Önceki örnekte, `telemetryConfig` cihaz ikizi istenen ve bildirilen özellikler cihaz uygulamasını ve çözüm arka ucu ile bu cihaz için telemetri yapılandırma eşitlemek için kullanılır. Örneğin:

1. Çözüm arka ucu, istenen yapılandırma değeri ile istenen özelliği ayarlar. İstenen özellik kümesine sahip bir belge bölümü aşağıda verilmiştir:

   ```json
   "desired": {
       "telemetryConfig": {
           "sendFrequency": "5m"
       },
       ...
   },
   ```

2. Cihaz uygulaması, değişikliği hemen bağlı veya ilk yeniden bildirilir. Cihaz uygulaması daha sonra güncelleştirilmiş yapılandırmayı raporlar (veya bir hata koşulunu kullanarak `status` özelliği). Bildirilen özellikler bölümü aşağıda verilmiştir:

   ```json
   "reported": {
       "telemetryConfig": {
           "sendFrequency": "5m",
           "status": "success"
       }
       ...
   }
   ```

3. Çözüm arka ucu tarafından birçok cihaz arasında yapılandırma işlemi sonuçlarını izleyebilir [sorgulama](iot-hub-devguide-query-language.md) cihaz ikizlerini.

> [!NOTE]
> Yukarıdaki örnek, bir cihaz yapılandırması ve durumunu kodlama yollarından biri okunabilirlik için en iyi duruma getirilmiş parçacıklarıdır. Cihaz ikizi istenen ve bildirilen özellikler cihaz çiftleri, IOT hub'ı belirli bir şemaya uygulamaz.
> 

Üretici yazılımı güncelleştirmelerini gibi uzun süre çalışan işlemleri eşitlenecek ikizlerini kullanabilirsiniz. Eşitleyebilir ve cihazlar arasında bir uzun süre çalışan işlemin İzleme özelliklerini kullanma hakkında daha fazla bilgi için bkz. [kullanmak istediğiniz cihazları yapılandırmak için Özellikler](tutorial-device-twins.md).

## <a name="back-end-operations"></a>Arka uç işlemleri

Çözüm arka ucu kullanarak HTTPS sunulan aşağıdaki atomik işlemler, cihaz ikizi çalışır:

* **Cihaz ikizi Kimliğe göre Al**. Bu işlem, etiketler ve istenen ve bildirilen sistem özellikleri dahil olmak üzere cihaz ikizi belgeyi döndürür.

* **Cihaz ikizi kısmen güncelleştirmek**. Bu işlem, etiketler veya bir cihaz ikizindeki istenen özellikler kısmen güncelleştirmek çözüm arka ucu sağlar. Kısmi güncelleştirmeyi ekler veya güncelleştirir herhangi bir özelliği bir JSON belgesi olarak ifade edilir. Özelliklerini ayarlamak `null` kaldırılır. Aşağıdaki örnek yeni bir istenen özellik değeriyle oluşturur `{"newProperty": "newValue"}`, mevcut değerini geçersiz kılar `existingProperty` ile `"otherNewValue"`ve kaldırır `otherOldProperty`. Başka bir değişiklik, mevcut istenen özellikleri veya etiketleri oluşturulur:

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

* **İkiz bildirimlerin**. Bu işlem ikizi değiştirildiğinde bildirim almak çözüm arka ucu sağlar. Bunu yapmak için IOT çözümünüzün yönlendirme oluşturma ve veri kaynağı eşit ayarlamak için gereken *twinChangeEvents*. İkiz bildirim gönderilir varsayılan olarak, bu tür bir yol, önceden mevcut. Değişim oranı çok yüksek olup olmadığını veya iç hatalar gibi diğer nedenlerle, IOT hub'ı tüm değişiklikleri içeren tek bir bildirim gönderebilir. Bu nedenle, uygulamanızın güvenilir denetim ve tüm ara durumlarını günlük gerekiyorsa, CİHAZDAN buluta iletileri kullanmanız gerekir. İkiz bildirim iletisi, özellikleri ve gövdesini içerir.

  - Özellikler

    | Ad | Değer |
    | --- | --- |
    $content-type | uygulama/json |
    $iothub-enqueuedtime |  Bildirim zaman gönderildiği zaman |
    $iothub-message-kaynak | twinChangeEvents |
    $content-encoding | UTF-8 |
    deviceId | Cihaz kimliği |
    HubName | IOT hub'ı adı |
    operationTimestamp | [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) işleminin zaman damgası |
    ıothub ileti şeması | deviceLifecycleNotification |
    opType | "replaceTwin" veya "updateTwin" |

    İleti sistemi özellikleri önekiyle `$` simgesi.

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

Önceki tüm işlemleri destekleyen [iyimser eşzamanlılık](iot-hub-devguide-device-twins.md#optimistic-concurrency) ve gerekli **ServiceConnect** tanımlandığı gibi izni [IOT hub'a erişimi denetleme](iot-hub-devguide-security.md).

Bu işlemlere ek olarak çözüm arka ucu şunları yapabilir:

* SQL benzeri kullanarak cihaz çiftlerini sorgulamak [IOT Hub sorgu dili](iot-hub-devguide-query-language.md).

* Cihaz ikizlerini kullanarak büyük kümesi işlemleri [işleri](iot-hub-devguide-jobs.md).

## <a name="device-operations"></a>Cihaz işlemleri

Cihaz uygulamasını kullanarak aşağıdaki atomik işlemler üzerinde cihaz ikizini çalışır:

* **Cihaz ikizi almak**. Bu işlem şu anda bağlı bir cihaz için cihaz ikizi belge (etiketler ve istenen ve bildirilen sistem özellikleri dahil) döndürür.

* **Kısmen bildirilen özellikleri güncelleştirmek**. Bu işlem, bağlı durumda cihaz, bildirilen özellikleri kısmi güncelleştirilmesini sağlar. Bu işlem, çözüm istenen özellikler kısmen güncelleştirmek için kullandığı uç geri JSON güncelleştirme biçimini kullanır.

* **İstenen özellikleri gözlemleyin**. Şu anda bağlı cihaz, İstenen özelliklerde yapılan güncelleştirmelerin gerçekleştiğinde bildirim almak seçebilirsiniz. Cihazın çözüm arka ucu tarafından yürütülen güncelleştirme (kısmi veya tam değiştirme) aynı biçiminde alır.

Önceki tüm işlemleri gerektiren **DeviceConnect** tanımlandığı gibi izni [IOT hub'a erişimi denetleme](iot-hub-devguide-security.md).

[Azure IOT cihaz SDK'ları](iot-hub-devguide-sdks.md) birçok dilleri ve platformları önceki işlemlerle kolaylaştırır. IOT hub'ı temel elemanlar için istenen özellikleri eşitleme ayrıntıları hakkında daha fazla bilgi için bkz. [cihaz yeniden akış](iot-hub-devguide-device-twins.md#device-reconnection-flow).

## <a name="tags-and-properties-format"></a>Etiketler ve Özellikler biçimi

Aşağıdaki kısıtlamalarla JSON nesneleri, etiketler ve istenen özellikler bildirilen özellikleri şunlardır:

* Tüm JSON nesneleri büyük/küçük harfe 64 baytın UTF-8 UNICODE dizelerini anahtarlarıdır. İzin verilen karakterleri UNICODE denetim karakterlerini (segmentler C0 ve C1) hariç tutmak ve `.`, `$`ve SP.

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

## <a name="device-twin-size"></a>Cihaz ikizi boyutu

IOT hub'ı zorunlu bir 8 KB'lık boyut sınırlaması ilgili toplam değerlerinin her `tags`, `properties/desired`, ve `properties/reported`, salt okunur öğeleri hariç.

UNICODE denetim karakterlerini (segmentler C0 ve C1) hariç tüm karakter sayılarak boyutu hesaplanır ve dize sabitleri dışında olan alanları.

IOT Hub ile ilgili bir hata bu belgelerin üst sınırı boyutunu artırır tüm işlemleri reddeder.

## <a name="device-twin-metadata"></a>Cihaz ikizi meta verileri

IOT Hub, zaman damgası son güncelleştirme her JSON nesnesi, cihaz ikizi için istenen ve bildirilen özellikler oluşturur. Zaman damgaları UTC biçiminde ve kodlanmış [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) biçimi `YYYY-MM-DDTHH:MM:SS.mmmZ`.

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
            },
            "batteryLevel": "55%",
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": "5m",
                    "status": {
                        "$lastUpdated": "2016-03-31T16:35:48.789Z"
                    },
                    "$lastUpdated": "2016-03-31T16:35:48.789Z"
                },
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
Etikete göre bir ETag sahip [RFC7232](https://tools.ietf.org/html/rfc7232), etiketin JSON gösterimi, temsil eder. Tutarlılık sağlamak için Etag'ler çözüm arka ucu koşullu güncelleştirme işlemlerini kullanabilirsiniz.

Cihaz ikizi istenen ve bildirilen özellikler Etag'ler yoktur, ancak sahip bir `$version` artımlı olması garanti değeri. Benzer şekilde bir ETag öğesine sürümü güncelleştirme tarafça güncelleştirmeleri tutarlılığını zorlamak için kullanılabilir. Örneğin, bir cihaz uygulaması için bildirilen bir özellik veya istenen özelliği için çözüm arka ucu.

(Örneğin, cihaz, istenen özellikleri gözleme uygulaması) bir gözlemci aracı alma işleminin sonucu bir güncelleştirme bildirimi arasındaki yarışa hazır mutabık kılmanız gerekir sürümleri de yararlıdır. [Cihaz yeniden akış bölüm](iot-hub-devguide-device-twins.md#device-reconnection-flow) daha fazla bilgi sağlar.

## <a name="device-reconnection-flow"></a>Cihaz yeniden akış

IOT Hub bağlantısı kesilmiş cihazlar için istenen özellikleri güncelleştirme bildirimlerini korumaz. Bağlanan bir cihaz için güncelleştirme bildirimlerini abone yanı sıra tam istenen özellikler belgenin alması gerekir takip eden. Güncelleştirme bildirimlerini ve tam alma arasında yarışa hazır olasılığını göz önünde bulundurulduğunda, aşağıdaki akış güvence altına gerekir:

1. Cihaz uygulaması, bir IOT hub'ına bağlanır.
2. Cihaz uygulaması için istenen özellikleri güncelleştirme bildirimlerini abone olur.
3. Cihaz uygulaması, istenen özellikleri için belgenin tamamını alır.

Cihaz uygulaması ile tüm bildirimleri yoksayabilirsiniz `$version` alınan belgenin tamamını sürümünden eşit veya daha küçük. Bu yaklaşım, IOT Hub sürümleri her zaman artırma gönderilmesini sağladığından mümkündür.

> [!NOTE]
> Bu mantık zaten uygulanan [Azure IOT cihaz SDK'ları](iot-hub-devguide-sdks.md). Bu açıklama, yalnızca cihaz uygulamasını herhangi birini Azure IOT cihaz SDK'ları kullanamazsınız ve MQTT arabirimi doğrudan program gerekir yararlı olur.
> 

## <a name="additional-reference-material"></a>Ek başvuru malzemesi

IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md) makale her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar](iot-hub-devguide-quotas-throttling.md) makalede IOT Hub hizmeti ve hizmetin kullandığınızda beklenir azaltma davranışını uygulanan kotalar.

* [Azure IOT cihaz ve hizmet SDK'ları](iot-hub-devguide-sdks.md) makalede çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.

* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md) makalede IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için kullanabileceğiniz IOT Hub sorgu dili.

* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md) makale ve MQTT protokolünü IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Artık öğrendiğiniz cihaz ikizleri hakkında aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilginizi çekebilir:

* [Anlama ve IOT Hub'ında modül ikizlerini kullanma](iot-hub-devguide-module-twins.md)
* [Bir cihazda doğrudan yöntem çağırma](iot-hub-devguide-direct-methods.md)
* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki öğreticilerde IOT Hub bakın:

* [Cihaz ikizi kullanma](iot-hub-node-node-twin-getstarted.md)
* [Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)
* [VS Code için Azure IOT araçları ile cihaz Yönetimi](iot-hub-device-management-iot-toolkit.md)
