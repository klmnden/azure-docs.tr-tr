---
title: Azure IOT için güvenlik önerilerini | Microsoft Docs
description: Bu makalede, Azure IOT hub'ı çözümünüzün güvenliğini sağlamak için ek adımlar özetlenmektedir.
author: dsk-2015
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dkshir
ms.openlocfilehash: d079e082fb8ac90f398e46f283bd1e33e2b4ab40
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463090"
---
# <a name="security-recommendations-for-azure-internet-of-things-iot-deployment"></a>Azure nesnelerin interneti (IOT) dağıtımı için güvenlik önerileri

Bu makale, Azure IOT Hizmetleri için güvenlik önerileri içerir. Bu önerileri uygulayan yardım edecek ve Azure IOT, bir müşteri olarak güvenlik yükümlülüklerinizi yerine genel IOT çözümlerinizin güvenliğini artırın. Azure IOT tarafından sağlanan iç güvenlik özellikleri hakkında daha fazla bilgi için okuma [baştan sona IOT güvenliği](iot-security-ground-up.md).

## <a name="general"></a>Genel

| Öneri | Açıklamalar |
|-|-|
| Güncel olarak takip edin | Desteklenen platformlar, programlama dilleri, protokoller ve çerçeveleri en son sürümlerini kullanın. |
| Kimlik doğrulama anahtarlarını güvende tutun | Dağıtımdan sonra cihaz kimliklerini ve kimlik doğrulama anahtarları fiziksel olarak güvenli tutun. Bu kötü amaçlı cihaz geçici olarak kayıtlı bir cihaz kaçının. |
| Mümkün olduğunda cihaz SDK'kullanın | Cihaz SDK'ları gibi şifreleme, kimlik doğrulama ve benzeri bir güçlü ve güvenli bir cihaz uygulaması geliştirmeye yardımcı olmak için güvenlik özellikleri çeşitli uygulama. Bkz: [kavrama ve kullanma Azure IOT Hub SDK'ları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) daha fazla bilgi için. |


## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Açıklamalar |
|-|-|
| Hub için erişim denetimi tanımlayın | [Anlama ve erişim türünü tanımlayan](iot-security-deployment.md#securing-the-cloud) her bileşen, IOT Hub çözümde olacaktır işlevselliğine bağlı. İzin verilen izinler *kayıt defteri okuma*, *RegistryReadWrite*, *ServiceConnect*, ve *DeviceConnect*. Varsayılan [paylaşılan erişim ilkeleri IOT hub'ınızdaki](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#access-control-and-permissions) kendi rolüne göre her bileşeni için izinleri tanımlamanıza yardımcı olabilir. |
| Arka uç Hizmetleri için erişim denetimi tanımlayın | IOT Hub çözümünüzü tarafından içe alınan veri kullanılabilir göre diğer Azure Hizmetleri gibi [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/), [Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/), [App Service](https://docs.microsoft.com/azure/app-service/), [LogicApps](https://docs.microsoft.com/azure/logic-apps/), ve [Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction). Anlama ve bu hizmetler için belgelendiği gibi uygun erişim izinlerine emin olun. |


## <a name="data-protection"></a>Veri koruma

| Öneri | Açıklamalar |
|-|-|
| Cihaz kimlik doğrulaması güvenliğini sağlama | Kullanarak, cihazlarınız ve IOT hub'ınız arasında güvenli iletişim sağlamak [benzersiz bir kimlik anahtarı veya güvenlik belirteci](iot-security-deployment.md#iot-hub-security-tokens), veya [cihazda X.509 sertifikası](iot-security-deployment.md#x509-certificate-based-device-authentication) her aygıt için. İçin uygun yöntemi kullanın [(MQTT, AMQP veya HTTPS) seçtiğiniz protokolü temel güvenlik belirteçleri kullanmasını](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security). |
| Güvenli iletişim | IOT Hub sürümleri 1.2, 1.2 ve 1.0 destekleyen Aktarım Katmanı Güvenliği (TLS) standardını kullanarak cihazlarla bağlantı güvenliğini sağlar. Kullanım [TLS 1.2](https://tools.ietf.org/html/rfc5246) en yüksek güvenlik sağlamak için. |
| Hizmet iletişimi güvenli hale getirme | IOT Hub gibi arka uç hizmetlerine bağlanmak için uç noktaları sağlar [Azure depolama](/azure/storage/) veya [Event Hubs](/azure/event-hubs) şifrelenmemiş bir kanal yalnızca TLS protokolünü ve uç nokta kullanarak Internet'e açık. Bu veri depolama ya da analiz için bu arka uç Hizmetleri ulaştığında, hizmet için uygun güvenlik ve şifreleme yöntemleri görevlendirmek ve arka uçtaki hassas bilgileri korumak emin olun. |


## <a name="networking"></a>Ağ

| Öneri | Açıklamalar |
|-|-|
| Cihazlarınızı erişimi koruma | Donanım bağlantı noktalarına istenmeyen erişimi önlemek için en az için cihazlarınızı tutun. Buna ek olarak, fiziksel aygıt kurcalama algılamak veya önlemek için mekanizmaları oluşturun. Okuma [IOT güvenlik en iyi](iot-security-best-practices.md) Ayrıntılar için. |
| Güvenli donanımı oluşturun | Şifrelenmiş depolama ya da Güvenilir Platform Modülü (aygıtlarınız ve altyapınız daha güvenli tutmak için TPM), gibi güvenlik özelliklerini bir araya getirin. Cihaz işletim sistemi ve en son sürümlere yükseltilecektir sürücüleri tutun ve alan izin veriyorsa, virüsten koruma ve kötü amaçlı yazılımdan koruma özellikleri yükleyin. Okuma [IOT güvenlik mimarisi](iot-security-architecture.md) anlamak nasıl bu çeşitli güvenlik tehditleri önlemeye yardımcı olabilir. |


## <a name="monitoring"></a>İzleme

| Öneri | Açıklamalar |
|-|-|
| Cihazlarınızı yetkisiz erişimi izleme |  Tüm güvenlik ihlallerini veya fiziksel cihazın veya bağlantı noktalarını oynama izlemek için cihaz işletim sisteminizin günlüğe kaydetme özelliğini kullanın. |
| IOT çözümünüzü buluttan izleme | IOT hub'ı kullanmanın çözüm genel durumunu izlemek [Azure İzleyicisi'nde ölçümler](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics). |
| Tanılama ayarlama ayarlayın | Çözümünüzde olayları günlüğe kaydetme ve Azure İzleyici, bir performans sağladığını görünürlük elde etmek için tanılama günlükleri göndermesini yakından işlemlerinizi izleyin. Okuma [İzleyici ve IOT hub'ınızdaki sorunları tanılayın](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health) daha fazla bilgi için. |

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT içeren Gelişmiş senaryolar için ek güvenlik gereksinimleri göz önüne almanız gerekebilir. Bkz: [IOT güvenlik mimarisi](iot-security-architecture.md) daha fazla kılavuzluk için.

