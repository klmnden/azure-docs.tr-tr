---
title: Azure IOT Hub cihazı sağlama hizmeti için çıkış için cihaz kavramları | Microsoft Docs
description: Cihaz kavramları Azure IOT Hub cihazı sağlama hizmeti için çıkış açıklar
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: fa8cb29f145c7658227f93d08a990c98563a0cfc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59050858"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>IOT Hub cihaz çıkış kavramları

Bir IOT çözümünü yaşam döngüsü boyunca, cihazlar IOT hub'ları arasında taşımak için yaygındır. Bu taşıma nedenleri, aşağıdaki senaryolar şunları içerebilir:

* **Coğrafi konum / GeoLatency**: Bir cihaz konumlar arasında hareket ettikçe, ağ gecikmesi cihaz sağlayarak geliştirilmiştir daha yakın bir IOT hub'ına geçirilen.

* **Çok kiracılılık**: Bir cihaz aynı IOT çözüm içinde kullanılabilir ve bir yeni müşteri veya müşteri siteye yeniden atandı. Bu yeni müşteri, farklı bir IOT hub'ı kullanarak hizmet.

* **Çözüm değişiklik**: Bir cihaz yeni veya güncelleştirilmiş bir IOT çözüm taşınmış. Yeniden atama, diğer arka uç bileşenlerine bağlı yeni bir IOT hub ile iletişim kurmak için cihaz gerektirebilir.

* **Karantina**: Bir çözüm değişiklik benzer. Tehlikeye gerçekleştiriyor, bir cihaz veya güncel olmayan yalnızca bir IOT hub'ına atanabilmelerine güncelleştirin ve uyumluluk geri dönebilirsiniz. Cihazın düzgün sonra ana hub'a geçiş yaptı.

Cihaz sağlama hizmeti adresleri içinde desteği, bu ihtiyaçları çıkış. Cihazlar için yeni IOT hub cihaz kayıt girişi üzerinde yapılandırılan reprovisioning İlkesi göre otomatik olarak atanabilir.

## <a name="device-state-data"></a>Cihaz durumu verileri

Cihaz durumu verilerini oluşan [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve cihaz özellikleri. Bu veriler, cihaz sağlama hizmeti örneğinizi ve bir cihaza atanan IOT hub'ı depolanır.

![Cihaz sağlama hizmeti sağlama](./media/concepts-device-reprovisioning/dps-provisioning.png)

Aşağıdaki adımlar, bir cihaz ilk kez bir cihaz sağlama hizmeti örneğine sağlandığında gerçekleştirilir:

1. Cihaz, cihaz sağlama hizmeti örneğine bir sağlama isteği gönderir. Hizmet örneği, bir kayıt girişi temel alarak cihaz kimliğini doğrular ve cihaz durumu verilerinin ilk yapılandırması oluşturur. Hizmet örneği, cihaz kaydını yapılandırmaya göre bir IOT hub'ına atar ve bu IOT hub'ı atamayı cihaza döndürür.

2. Sağlama hizmeti örneği, tüm ilk cihaz durumu verilerinin bir kopyasını atanan IOT hub'ına verir. Cihaz atanan IOT hub'ına bağlanır ve işlemleri başlatır.

Zamanla, IOT hub cihaz durumu verilerini tarafından güncelleştirilebilir [cihaz işlemleri](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) ve [arka uç işlemleri](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations). Cihaz sağlama hizmeti örneğine depolanmış ilk cihaz durumu bilgilerini dokunulmadan kalır. Bu dokunulmadan cihaz durumu verilerini ilk yapılandırmadır.

![Cihaz sağlama hizmeti sağlama](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

Bir cihaz IOT hub'ları arasında hareket ettikçe senaryoya bağlı olarak ayrıca yeni IOT hub'ına önceki IOT hub güncelleştirilmiş cihaz durumu geçirmek gerekebilir. Bu geçiş, cihaz sağlama hizmeti ilkelerinde çıkış tarafından desteklenir.

## <a name="reprovisioning-policies"></a>Çıkış ilkeleri

Senaryoya bağlı olarak bir cihazı genellikle bir isteği bir sağlama hizmeti örneğine yeniden başlatmada gönderir. Ayrıca isteğe bağlı olarak sağlama el ile tetiklemek için bir yöntem de destekler. Bir kayıt girişi reprovisioning ilke, cihaz sağlama hizmeti örneği bu istekleri sağlama nasıl işleyeceğini belirler. İlke ayrıca, cihaz durumu verilerini çıkış sırasında geçirilmesinin gerekip gerekmediğini belirler. Bireysel kayıtlar ve kayıt grupları için aynı ilkeleri kullanılabilir:

* **Yeniden sağlama ve veri geçişi**: Bu ilke, yeni bir kayıt girdileri için varsayılandır. Bu ilke, cihaz kayıt girişi ile ilişkili (1) yeni bir istek gönderdiğinde eylemi gerçekleştirir. Kayıt girdisi yapılandırmasına bağlı olarak, cihazın başka bir IOT hub'ına atanabilir. İlk IOT hub ile cihaz kaydı, cihaz IOT hub'ları değişiyorsa kaldırılacak. Bu ilk IOT hub'ından güncelleştirilmiş cihaz durum bilgilerini yeni IOT hub (2) üzerinden geçirilir. Geçiş sırasında cihazın durumu olarak raporlanır **atama**.

    ![Cihaz sağlama hizmeti sağlama](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)

* **Yeniden sağlama ve ilk yapılandırmaya Sıfırla**: Bu ilke, cihaz kayıt girişi ile ilişkili yeni bir sağlama isteği (1) gönderdiğinizde eylemi gerçekleştirir. Kayıt girdisi yapılandırmasına bağlı olarak, cihazın başka bir IOT hub'ına atanabilir. İlk IOT hub ile cihaz kaydı, cihaz IOT hub'ları değişiyorsa kaldırılacak. Cihaz sağlanırken sağlama hizmeti örneği alınan ilk yapılandırma verileri, yeni IOT hub (2) sağlanır. Geçiş sırasında cihazın durumu olarak raporlanır **atama**.

    Bu ilke genellikle, IOT hub'ları değiştirmeden Fabrika için kullanılır.

    ![Cihaz sağlama hizmeti sağlama](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)

* **Hiçbir zaman yeniden sağlanması**: Cihaz, farklı bir hub'ına hiçbir zaman atanır. Bu ilke, geriye dönük uyumluluğu yönetmek için sağlanır.

### <a name="managing-backwards-compatibility"></a>Geriye dönük uyumluluk yönetme

Eylül 2018'den önce Yapışkan davranışı IOT hub'larına cihaz atamaları içeriyor. Bir cihaz üzerinden sağlama işlemine geri oluştu, bunu yalnızca aynı IOT hub atanır.

Bağımlı Bu davranış önlemlerin çözümleri için sağlama hizmeti, geriye dönük uyumluluk içerir. Şu anda bu davranışı aşağıdaki ölçütlere göre cihazlar korunur:

1. Cihaz sağlama hizmeti yerel çıkış destek kullanılabilirliğini önce bir API sürümü ile cihazları bağlayın. API için aşağıdaki tabloya bakın.

2. Cihazlar için kayıt girişi kendileri için ayarlanmış bir reprovisioning İlkesi yok.

Bu uyumluluk, daha önce cihazları deneyimi ilk test sırasında mevcut aynı davranışı dağıtılan emin olur. Önceki davranışı korumak için bu kayıtları için reprovisioning ilke kaydetme. Reprovisioning bir ilke olarak ayarlanırsa reprovisioning ilke davranışı üzerinde öncelik kazanır. Öncelikli reprovisioning ilkenin izin vererek, müşterilere cihaz görüntüsünü yeniden oluşturmak zorunda kalmadan cihaz davranışının güncelleştirebilirsiniz.

Aşağıdaki akış çizelgesi davranışı olduğunda gösterilecek yardımcı olur:

![Geriye dönük uyumluluk akış çizelgesi](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

Aşağıdaki tabloda, cihaz sağlama hizmetinde çıkış yerel destek kullanılabilirliğini önce API sürümlerini gösterir:

| REST API | C SDK'SI | Python SDK'sı |  Node SDK | Java SDK | .NET SDK |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 ve önceki sürümleri](/rest/api/iot-dps/createorupdateindividualenrollment/createorupdateindividualenrollment#uri-parameters) | [1.2.8 ve önceki sürümleri](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 ve önceki sürümleri](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 veya önceki bir sürümü](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 veya önceki bir sürümü](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 veya önceki bir sürümü](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Bu değerleri ve bağlantıları değiştirme olasılığı düşüktür. Bu sürümler, müşteri tarafından burada belirlenebilir ve beklenen sürümleri ne olacağını belirlemek için yalnızca bir yer tutucu girişimi olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Cihazları yeniden sağlamak nasıl](how-to-reprovision.md)
