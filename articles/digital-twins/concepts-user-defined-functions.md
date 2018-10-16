---
title: Veri işleme ve kullanıcı tanımlı işlevleri ile Azure dijital İkizlerini | Microsoft Docs
description: Veri işleme, matchers ve kullanıcı tanımlı işlevleri ile Azure dijital İkizlerini genel bakış
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: b7561848ffd0158e22e97530774112dcee2a9864
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324302"
---
# <a name="data-processing-and-user-defined-functions"></a>Veri işleme ve kullanıcı tanımlı işlevler

Azure dijital İkizlerini Gelişmiş bilgi işlem özellikleri sunar. Geliştiriciler, tanımlamak ve özel işlevler için önceden tanımlı uç noktaları olayları göndermek için gelen telemetri iletilerini çalıştırın.

## <a name="data-processing-flow"></a>Veri işleme akış

Geliştiriciler cihazlar dijital çiftleri için telemetri verilerini gönderdikten sonra dört aşamada veri işleyebilir: _doğrulama_, _eşleşen_, _işlem_, ve _Gönderme_:

![Dijital İkizlerini veri işleme akış][1]

1. _Doğrulama_ aşaması dönüştüren bir anlaşılır gelen telemetri iletiye [ `data transfer object` ](https://en.wikipedia.org/wiki/Data_transfer_object) biçimi. Bu aşama, ayrıca cihaz ve algılayıcıyı doğrulama yürütür.
1. _Eşleşen_ aşaması çalıştırmak için uygun kullanıcı tanımlı işlevlere bulur. Önceden tanımlanmış matchers cihaz, sensör ve gelen telemetri iletileriyle alanı bilgileri kullanıcı tanımlı işlevlere göre bulur.
1. _İşlem_ aşama önceki aşamada eşleşen kullanıcı tanımlı işlevlere çalıştırır. Bu işlevlere okuma ve uzamsal grafik düğümleri üzerinde hesaplanan değerlerini güncelleştirin ve için özel bildirimleri gösterin.
1. _Gönderme_ aşaması herhangi bir özel işlem aşaması bildirim grafikte tanımlanan Uç noktalara yönlendirir.

## <a name="data-processing-objects"></a>Veri işleme nesneleri

Azure dijital İkizlerini veri işlemeye oluşur üç nesneleri tanımlama: _matchers_, _kullanıcı tanımlı işlevleri_, ve _rol atamaları_:

![Dijital İkizlerini veri işleme akış][2]

### <a name="matchers"></a>Matchers

_Matchers_ hangi eylemleri gelen algılayıcı telemetrisi göz önünde bulundurularak gerçekleşecek değerlendirme koşulları tanımlayın. Eşleşme belirlemek için bu koşullar özelliklerinden algılayıcı, algılayıcının üst cihaz ve algılayıcının üst alanı içerebilir. Koşullar karşılaştırmalar olarak ifade edilir bir [JSON yolu](http://jsonpath.com/) aşağıdaki örnekte özetlendiği gibi:

- Veri türünün tüm algılayıcılar `Temperature`.
- Sahip `01` kendi bağlantı noktası.
- Genişletilmiş özellik anahtarı ile cihazlara ait `Manufacturer` değerine ayarlanırsa `GoodCorp`.
- Tür alanları için ait `Venue`.
- Üst alt öğeleri olan `SpaceId` `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`.

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - JSON yolu büyük/küçük harfe duyarlıdır.
> - JSON yükü tarafından döndürülecek olan yük aynıdır:
>   - `/sensors/{id}?includes=properties,types` Algılayıcı için.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` bir algılayıcının üst aygıt için.
>   - `/spaces/{id}?includes=properties,types,location,timezone` bir algılayıcının üst alan.
> - Karşılaştırmalar büyük/küçük harfe duyarsızdır.

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

A _kullanıcı tanımlı işlev_, veya _UDF_, Azure dijital çiftleri olarak yalıtılmış bir ortamda çalışan özel bir işlev değil. De seçeceğine uzamsal graph ve dağıtıcı hizmet aldığı gibi UDF'ler hem ham algılayıcı telemetri iletisi erişebilir. UDF grafiğin içinde kaydedildiğinde, UDF çalıştırma zamanı belirtmek için (yukarıda açıklanmıştır) bir Eşleştiricisi oluşturulması gerekir. Eşleşen UDF dijital İkizlerini verilen algılayıcıdan yeni telemetri aldığında, bir son birkaç sensör okumaları hareketli ortalamayı örneğin hesaplayabilirsiniz.

UDF JavaScript'te yazılmış ve geliştiricilerin özel algılayıcı telemetri iletilerini karşı kod parçacıkları yürütmek. Kullanıcı tanımlı yürütme ortamında graph ile etkileşim kurmak için yardımcı yöntemler vardır. Bir UDF ile geliştiriciler şunları yapabilir:

- Graf algılayıcı nesnesinde üzerine doğrudan okuma algılayıcı ayarlayın.
- Graftaki bir alanı içindeki farklı sensör okumaları göre eylem gerçekleştirin.
- Bir gelen algılayıcı okuma için belirli koşullar karşılandığında bir bildirim oluşturun.
- Graf meta verileri bildirim gönderilmeden önce okuma algılayıcı iliştirin.

Başvurmak [User User-Defined işlevleri nasıl](how-to-user-defined-functions.md) daha fazla ayrıntı için.

### <a name="role-assignment"></a>Rol ataması

UDF'ın eylemleri hizmetindeki verilerin güvenliğini sağlamak için rol tabanlı erişim denetimi dijital İkizlerini tabidir. Rol atamaları verilen UDF uzamsal grafik ile etkileşim için uygun izinlere sahip olduğundan emin olun. Örneğin, bir UDF oluşturma, okuma, güncelleştirme veya belirli bir alanı altında graf verilerini silme girişiminde bulunabilir. UDF'ın erişim düzeyini UDF graf verilerimi isterse veya bir eylem çalışır denetlenir. Daha fazla bilgi için [rol tabanlı erişim denetimi](security-create-manage-role-assignments.md).

Rol ataması yok bir UDF tetiklemek bir Eşleştiricisi için mümkündür. Bu durumda, UDF herhangi bir veri Graph'tan okuma başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

Yönlendirme olayları ve diğer Azure Hizmetleri için telemetri iletilerini hakkında daha fazla bilgi edinmek için [yönlendirme olayları ve iletileri](concepts-events-routing.md).

Rol atamaları matchers ve kullanıcı tanımlı işlevler oluşturma hakkında daha fazla bilgi edinmek için [kullanıcı tanımlı işlevler kullanma Kılavuzu](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
