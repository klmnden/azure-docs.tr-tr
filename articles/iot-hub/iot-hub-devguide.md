---
title: Azure IOT Hub Geliştirici Kılavuzu | Microsoft Docs
description: Azure IOT Hub Geliştirici kılavuzunun tartışmalar uç noktaları, güvenlik, kimlik kayıt defteri, cihaz yönetimi, doğrudan yöntemler, cihaz çiftleri, dosya yüklemeleri, işleri, IOT Hub sorgu dili içerir ve mesajlaşma.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 1ff7d430edd3f638ad5efcc5a89604e4ed732211
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400159"
---
# <a name="azure-iot-hub-developer-guide"></a>Azure IOT Hub Geliştirici Kılavuzu

Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Azure IOT Hub ile sağlar:

* Cihaz başına güvenlik kimlik bilgilerini kullanarak güvenli iletişim ve erişim denetimi.

* Birden çok cihaz-Bulut ve bulut-cihaz hiper ölçekli iletişim seçenekleri.

* Sorgulanabilir depolama alanı başına cihaz durumu bilgilerini ve meta verileri.

* En popüler diller ve platformlar için cihaz kitaplıkları ile kolay cihaz bağlantısı.

Bu IOT Hub Geliştirici kılavuzunun aşağıdaki makaleleri içerir:

* [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) CİHAZDAN buluta iletileri, cihaz ikizinin bildirilen özellikler ve karşıya dosya yükleme arasında seçtiğiniz yardımcı olur.

* [Bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md) doğrudan yöntemler, cihaz ikizinin istenen özellikleri ve bulut-cihaz iletilerini arasında seçtiğiniz yardımcı olur.

* [CİHAZDAN buluta ve bulut cihaz IOT Hub ile ileti](iot-hub-devguide-messaging.md) IOT hub'ı sunan Mesajlaşma özelliklerinin (CİHAZDAN buluta ve bulut-cihaz) açıklar.

  * [IOT Hub'ına CİHAZDAN buluta ileti gönderme](iot-hub-devguide-messages-d2c.md).

  * [CİHAZDAN buluta iletilerini yerleşik uç noktadan okuma](iot-hub-devguide-messages-read-builtin.md).

  * [Özel uç noktalar ve yönlendirme kuralları için CİHAZDAN buluta iletileri kullanın](iot-hub-devguide-messages-read-custom.md).

  * [IOT Hub'ından bulut-cihaz iletilerini göndermek](iot-hub-devguide-messages-c2d.md).

  * [Oluşturun ve IOT Hub iletilerini okumanızı](iot-hub-devguide-messages-construct.md).

* [Bir CİHAZDAN karşıya dosya yükleme](iot-hub-devguide-file-upload.md) dosyaları bir CİHAZDAN nasıl karşıya yükleneceğini açıklar. Makale, bildirimler gibi konular hakkında bilgi karşıya yükleme işlemi gönderebilir de içerir.

* [IOT hub cihaz kimliklerini yönetme](iot-hub-devguide-identity-registry.md)hangi bilgileri açıklayan her IOT hub'ının kimlik kayıt defteri depolar. Makalede ayrıca nasıl erişebilir ve onu değiştirme açıklanır.

* [IOT hub'a erişimi denetleme](iot-hub-devguide-security.md) hem cihazlar IOT hub'ı işlevsellik için erişim verin ve bileşenleri bulut için kullanılan güvenlik modeli açıklanır. Makale belirteçleri ve X.509 sertifikaları ve izinleri size verebilir ayrıntılarını kullanma hakkında bilgi içerir.

* [Durum ve yapılandırmaları eşitlemek için cihaz ikizlerini kullanma](iot-hub-devguide-device-twins.md) açıklar *cihaz ikizi* kavramı. Bu makalede, bir cihaz, cihaz ikizi ile eşitleme gibi ikizlerini kullanıma işlevi cihaz de anlatılmaktadır. Makale, bir cihaz ikizinde depolanan veriler hakkında bilgi içerir.

* [Bir cihazda doğrudan yöntem çağırma](iot-hub-devguide-direct-methods.md) yaşam döngüsünü bir doğrudan yöntem açıklar. Bu makalede, arka uç uygulamanızdan bir cihaz üzerinde yöntem çağırmak ve cihazda doğrudan yöntem işlemek açıklanır.

* [İşleri birden fazla cihazda zamanlama](iot-hub-devguide-jobs.md) nasıl birden fazla cihazda işleri zamanlayabilirsiniz açıklar. Makaleyi güncelleştirme cihaz ikizi'ni kullanarak bir cihazı bir doğrudan yöntem yürütme olarak görevler gerçekleştiren işleri göndermek nasıl açıklar. Ayrıca, bir işin durumunu sorgulamak nasıl açıklar.

* [Başvuru - iletişim protokolü seçme](iot-hub-devguide-protocols.md) IOT Hub için cihaz iletişimi destekler ve açık olması gereken bağlantı noktalarını listeler iletişim protokolleri açıklar.

* [Başvuru - IOT Hub uç noktaları](iot-hub-devguide-endpoints.md) her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar. Makale ayrıca ek uç noktalar IOT hub'ına nasıl oluşturabileceğinizi ve bir alan ağ geçidi standart dışı senaryolarda IOT Hub uç noktalarınızı bağlantıyı etkinleştirmek için nasıl kullanılacağını açıklar.

* [Başvuru - cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md) hub'ınızın cihaz ikizleri ve işler hakkında bilgi almak sayesinde, IOT Hub sorgu dili açıklar.

* [Başvuru - kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md) kota aştığında oluşan IOT Hub hizmeti ve azaltma kotaları özetler.

* [Başvuru - fiyatlandırma](iot-hub-devguide-pricing.md) farklı SKU'ları ve IOT Hub ve çeşitli IOT hub'ı işlevlerini iletileri olarak IOT Hub tarafından nasıl ölçülür ilgili ayrıntılar için fiyatlandırma hakkında genel bilgiler sağlar.

* [Başvuru - cihaz ve hizmet SDK'ları](iot-hub-devguide-sdks.md) IOT hub'ınıza etkileşim cihaz ve hizmet uygulamaları geliştirmek için Azure IOT SDK'ları listeler. Makale çevrimiçi API belgelerine bağlantılar içerir.

* [Başvuru - IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md) IOT hub'ı ve MQTT protokolünü nasıl desteklediği hakkında ayrıntılı bilgiler sağlar. Makale Azure IOT SDK'ları için yerleşik ve MQTT protokolünü desteğini açıklar ve MQTT protokolünü kullanarak doğrudan hakkında bilgi sağlar.

* [Sözlük](iot-hub-devguide-glossary.md) yaygın IOT Hub ile ilgili terimlerin bir listesi.