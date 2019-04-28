---
title: Veri işleme ve kullanıcı tanımlı işlevleri ile Azure dijital İkizlerini | Microsoft Docs
description: Veri işleme, matchers ve kullanıcı tanımlı işlevleri ile Azure dijital İkizlerini genel bakış.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: alinast
ms.openlocfilehash: 4db515a931bc7f423eb11ae31b7304a602f0da46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60925934"
---
# <a name="data-processing-and-user-defined-functions"></a>Veri işleme ve kullanıcı tanımlı işlevleri

Azure dijital İkizlerini Gelişmiş bilgi işlem özellikleri sunar. Geliştiriciler, tanımlamak ve özel işlevler için önceden tanımlı bir uç nokta olayları göndermek için gelen telemetri iletilerini çalıştırın.

## <a name="data-processing-flow"></a>Veri işleme akış

Cihazları Azure dijital çiftleri için telemetri verilerini gönderdikten sonra geliştiricilerin dört aşamada veri işleyebilir: *doğrulama*, *eşleşen*, *işlem*, ve *gönderme* .

![Azure dijital İkizlerini veri işleme akış][1]

1. Doğrulama aşamasında bir anlaşılır gelen telemetri iletiye dönüştürür [veri aktarımı nesnesi](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) biçimi. Bu aşama, ayrıca cihaz ve algılayıcıyı doğrulama yürütür.
1. Eşleştirme aşaması çalıştırmak için uygun olan kullanıcı tanımlı işlevleri bulur. Önceden tanımlanmış matchers cihaz, sensör ve gelen telemetri iletileriyle alanı bilgileri temel alarak kullanıcı tanımlı işlevleri bulun.
1. İşlem aşama önceki aşamada eşleşen kullanıcı tanımlı işlevleri çalışır. Bu işlevler okuyun ve uzamsal hesaplanan değerleri güncelleştirme grafik düğümleri ve özel bildirimleri gönderebilir.
1. Dağıtım aşaması, herhangi bir özel işlem aşaması bildirim grafikte tanımlanan Uç noktalara yönlendirir.

## <a name="data-processing-objects"></a>Veri işleme nesneleri

Azure dijital İkizlerini veri işlemeye oluşur üç nesneleri tanımlama: *matchers*, *kullanıcı tanımlı işlevleri*, ve *rol atamaları*.

![Azure dijital İkizlerini veri işleme nesneleri][2]

<div id="matcher"></div>

### <a name="matchers"></a>Matchers

Matchers bir dizi hangi işlemlerin zaman gelen algılayıcı telemetrisi göz önünde bulundurularak değerlendirme koşulları tanımlayın. Eşleşme belirlemek için koşullar özelliklerinden algılayıcı, algılayıcının üst cihaz ve algılayıcının üst alanı içerebilir. Koşullar karşılaştırmalar olarak ifade edilir bir [JSON yolu](https://jsonpath.com/) Bu örnekte özetlendiği gibi:

- Veri türünün tüm algılayıcılar **sıcaklık** kaçan dize değeri tarafından temsil edilen `\"Temperature\"`
- Sahip `01` kendi bağlantı noktası
- Genişletilmiş özellik anahtarı ile cihazlara ait **üretici** kaçan dize değerine ayarlayın `\"GoodCorp\"`
- Atlanan dizesi tarafından belirtilen türün alanlarına ait olduğu `\"Venue\"`
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

Kullanıcı tanımlı bir işlev içinde yalıtılmış bir Azure dijital İkizlerini ortam yürütülen özel bir işlev değil. Alınan gibi kullanıcı tanımlı işlevleri ham algılayıcı telemetri iletileriyle erişebilir. Kullanıcı tanımlı işlevleri uzamsal graf ve dağıtıcı hizmeti de erişebilir. Kullanıcı tanımlı işlevi bir grafik içinde bir Eşleştiricisi kaydedildikten sonra (ayrıntılı [yukarıda](#matcher)) işlevin yürütüldüğü belirtmeyi oluşturulması gerekir. Örneğin, Azure dijital İkizlerini verilen algılayıcıdan yeni telemetri aldığında, eşleşen kullanıcı tanımlı işlevi bir son birkaç sensör okumaları hareketli ortalamayı hesaplayabilirsiniz.

Kullanıcı tanımlı işlevler, JavaScript dilinde yazılabilir. Yardımcı yöntemler kullanıcı tanımlı yürütme ortamında graph ile etkileşim kurun. Geliştiriciler, özel kod parçacıkları algılayıcı telemetri iletilerini karşı kod yürütebilir. Örneklere şunlar dahildir:

- Graf algılayıcı nesnesinde üzerine doğrudan okuma algılayıcı ayarlayın.
- Graftaki bir alanı içindeki farklı sensör okumaları göre eylem gerçekleştirin.
- Bir gelen algılayıcı okuma için belirli koşullar karşılandığında bir bildirim oluşturun.
- Graf meta verileri bildirim gönderilmeden önce okuma algılayıcı iliştirin.

Daha fazla bilgi için [kullanıcı tanımlı işlevler kullanma](./how-to-user-defined-functions.md).


#### <a name="examples"></a>Örnekler

[Dijital çiftleri için GitHub deposunu C# örnek](https://github.com/Azure-Samples/digital-twins-samples-csharp/) kullanıcı tanımlı işlevlerin bazı örnekleri içerir:
- [Bu işlev](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) tasarruf edilen karbon dioksit, hareket ve sıcaklık değerleri bir oda aralıktaki şu değerlerle kullanılabilir olup olmadığını belirlemek arar. [Dijital çiftleri için öğreticiler](tutorial-facilities-udf.md) bu işlevi daha ayrıntılı keşfedin. 
- [Bu işlev](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/multiplemotionsensors.js) verilerini birden çok hareket sensörden arar ve bunların hiçbiri herhangi bir hareket algılama alanının olduğunu belirler. Kullanıcı tanımlı işlev ya da kullanılan kolayca değiştirebilirsiniz [hızlı](quickstart-view-occupancy-dotnet.md), veya [öğreticiler](tutorial-facilities-setup.md), açıklamalar bölümünde belirtilen değişiklikler yaparak. 



### <a name="role-assignment"></a>Rol ataması

Azure dijital İkizlerini tabi kullanıcı tanımlı işlevin eylemlerdir [rol tabanlı erişim denetimi](./security-role-based-access-control.md) hizmetindeki verilerin güvenliğini sağlamak için. Rol atamaları, hangi kullanıcı tanımlı işlevleri uzamsal grafiği ve varlıklarını etkileşim için uygun izinlere sahip tanımlayın. Örneğin, bir kullanıcı tanımlı işlev özelliği ve izni olabilir *Oluştur*, *okuma*, *güncelleştirme*, veya *Sil* grafiği verileri belirli bir alanı altında. Kullanıcı tanımlı işlev graf verilerimi isterse veya bir eylem çalışır bir kullanıcı tanımlı işlevin erişim düzeyini denetlenir. Daha fazla bilgi için [rol tabanlı erişim denetimi](./security-create-manage-role-assignments.md).

Rol ataması yok kullanıcı tanımlı bir işlev tetiklemek bir Eşleştiricisi için mümkündür. Bu durumda, kullanıcı tanımlı işlevi herhangi bir veri Graph'tan okuma başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- Olay ve telemetri iletilerini diğer Azure hizmetlerine yönlendirme hakkında daha fazla bilgi edinmek için [olayları ve iletileri yönlendirmek](./concepts-events-routing.md).

- Rol atamaları matchers ve kullanıcı tanımlı işlevler oluşturma hakkında daha fazla bilgi edinmek için [kullanıcı tanımlı işlevler kullanma Kılavuzu](./how-to-user-defined-functions.md).

- Gözden geçirme [kullanıcı tanımlı işlev istemci Kitaplığı Başvurusu belgeleri](./reference-user-defined-functions-client-library.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
