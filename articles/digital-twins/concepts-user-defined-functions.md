---
title: Veri işleme ve kullanıcı tanımlı işlevleri ile Azure dijital İkizlerini | Microsoft Docs
description: Veri işleme, matchers ve kullanıcı tanımlı işlevleri ile Azure dijital İkizlerini genel bakış
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 2703778cd2eab582a9e7311aaf2024f100261889
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624531"
---
# <a name="data-processing-and-user-defined-functions"></a>Veri işleme ve kullanıcı tanımlı işlevleri

Azure dijital İkizlerini Gelişmiş bilgi işlem özellikleri sunar. Geliştiriciler, tanımlamak ve özel işlevler için önceden tanımlı bir uç nokta olayları göndermek için gelen telemetri iletilerini çalıştırın.

## <a name="data-processing-flow"></a>Veri işleme akış

Cihazları Azure dijital çiftleri için telemetri verilerini gönderdikten sonra geliştiricilerin dört aşamada veri işleyebilir: doğrulayın, eşleşen, işlem ve gönderme.

![Azure dijital İkizlerini veri işleme akış][1]

1. Doğrulama aşamasında bir anlaşılır gelen telemetri iletiye dönüştürür [veri aktarımı nesnesi](https://en.wikipedia.org/wiki/Data_transfer_object) biçimi. Bu aşama, ayrıca cihaz ve algılayıcıyı doğrulama yürütür.
1. Eşleştirme aşaması, uygun bir kullanıcı tanımlı işlevler (UDF'ler) çalıştırılacak bulur. Önceden tanımlanmış matchers cihaz, sensör ve gelen telemetri iletileriyle alanı bilgileri temel alarak UDF'ler bulun.
1. İşlem aşama önceki aşamada eşleşen UDF'ler çalıştırır. Bu işlevler okuyun ve uzamsal hesaplanan değerleri güncelleştirme grafik düğümleri ve özel bildirimleri gönderebilir.
1. Dağıtım aşaması, herhangi bir özel işlem aşaması bildirim grafikte tanımlanan Uç noktalara yönlendirir.

## <a name="data-processing-objects"></a>Veri işleme nesneleri

Azure dijital İkizlerini veri işlemeye oluşur üç nesneleri tanımlama: matchers, kullanıcı tanımlı işlevler ve rol atamaları.

![Azure dijital İkizlerini veri işleme nesneleri][2]

### <a name="matchers"></a>Matchers

Matchers bir dizi hangi işlemlerin zaman gelen algılayıcı telemetrisi göz önünde bulundurularak değerlendirme koşulları tanımlayın. Eşleşme belirlemek için koşullar özelliklerinden algılayıcı, algılayıcının üst cihaz ve algılayıcının üst alanı içerebilir. Koşullar karşılaştırmalar olarak ifade edilir bir [JSON yolu](http://jsonpath.com/) Bu örnekte özetlendiği gibi:

- Veri türünün tüm algılayıcılar **sıcaklık**
- Sahip `01` kendi bağlantı noktası
- Genişletilmiş özellik anahtarı ile cihazlara ait **üretici** değerine ayarlayın `"GoodCorp"`
- Tür alanları için ait olduğu `"Venue"`
- Üst alt öğeleri olan **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

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
> - JSON yükü tarafından döndürülen yükü aynıdır:
>   - `/sensors/{id}?includes=properties,types` Algılayıcı için.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` bir algılayıcının üst aygıt için.
>   - `/spaces/{id}?includes=properties,types,location,timezone` bir algılayıcının üst alan.
> - Karşılaştırmalar büyük/küçük harfe duyarsızdır.

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı bir işlev içinde Azure dijital İkizlerini yalıtılmış bir ortamda çalışan özel bir işlev değil. Bu alındı olarak UDF'ler ham algılayıcı telemetri iletisi erişebilir. UDF uzamsal grafiği ve dağıtıcı hizmetine de erişebilirler. UDF grafiğin içinde kaydedildikten sonra UDF çalıştırma zamanı belirtmek için (yukarıda açıklanmıştır) bir Eşleştiricisi oluşturulması gerekir. Azure dijital İkizlerini verilen algılayıcıdan yeni telemetri aldığında, eşleşen UDF hareketli ortalama son birkaç sensör okumaları, örneğin hesaplayabilirsiniz.

JavaScript UDF'leri yazılabilir. Geliştiriciler, özel kod parçacıkları algılayıcı telemetri iletilerini karşı kod yürütebilir. Yardımcı yöntemler kullanıcı tanımlı yürütme ortamında graph ile etkileşim kurun. Bir UDF ile geliştiriciler şunları yapabilir:

- Graf algılayıcı nesnesinde üzerine doğrudan okuma algılayıcı ayarlayın.
- Graftaki bir alanı içindeki farklı sensör okumaları göre eylem gerçekleştirin.
- Bir gelen algılayıcı okuma için belirli koşullar karşılandığında bir bildirim oluşturun.
- Graf meta verileri bildirim gönderilmeden önce okuma algılayıcı iliştirin.

Daha fazla bilgi için [kullanıcı tanımlı işlevler kullanma](how-to-user-defined-functions.md).

### <a name="role-assignment"></a>Rol ataması

UDF'ın eylemleri hizmetindeki verilerin güvenliğini sağlamak için Azure dijital İkizlerini rol tabanlı erişim denetimi tabidir. Rol atamaları verilen UDF uzamsal grafik ile etkileşim için uygun izinlere sahip olduğundan emin olun. Örneğin, bir UDF oluşturma, okuma, güncelleştirme veya belirli bir alanı altında graf verilerini silme girişiminde bulunabilir. UDF'ın erişim düzeyini UDF graf verilerimi isterse veya bir eylem çalışır denetlenir. Daha fazla bilgi için [rol tabanlı erişim denetimi](security-create-manage-role-assignments.md).

Rol ataması yok bir UDF tetiklemek bir Eşleştiricisi için mümkündür. Bu durumda, UDF herhangi bir veri Graph'tan okuma başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* Olay ve telemetri iletilerini diğer Azure hizmetlerine yönlendirme hakkında daha fazla bilgi edinmek için [olayları ve iletileri yönlendirmek](concepts-events-routing.md).

* Rol atamaları matchers ve kullanıcı tanımlı işlevler oluşturma hakkında daha fazla bilgi edinmek için [kullanıcı tanımlı işlevler kullanma Kılavuzu](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
