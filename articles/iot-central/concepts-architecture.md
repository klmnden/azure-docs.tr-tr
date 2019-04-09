---
title: Azure IOT Central mimari kavramları | Microsoft Docs
description: Bu makalede Azure IOT Central mimarisiyle ilgili temel kavramlar tanıtılmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 03/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 4f4b917808f4973dc83294391f58d7e0e2d01c4c
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59260961"
---
# <a name="azure-iot-central-architecture"></a>Azure IOT Central mimarisi

Bu makalede, Microsoft Azure IOT Central mimarisine genel bir bakış sağlar.

![Üst düzey mimarisi](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Cihazlar

Cihazlar, Azure IOT Central uygulamanız ile veri değişimi. Bir cihazı edebilirsiniz:

- Ölçümler gibi telemetri gönderin.
- Ayarları uygulamanız ile eşitleyin.

Azure IOT Central içinde bir cihaz uygulamanızla gönderip verileri bir cihaz şablonunda belirtilir. Cihaz şablonları hakkında daha fazla bilgi için bkz. [meta veri yönetimi](#metadata-management).

Cihazlar, Azure IOT Central uygulamasına nasıl bağlanacağını hakkında daha fazla bilgi için bkz: [cihaz bağlantısı](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Bulut ağ geçidi

Azure IOT Central, Azure IOT Hub, cihaz bağlantısı sağlayan bir bulut ağ geçidi olarak kullanır. IOT Hub'ı sağlar:

- Bulut ölçeğinde veri alımı.
- Cihaz yönetimi.
- Cihaz bağlantısı güvenli hale getirin.

IOT Hub hakkında daha fazla bilgi edinmek için [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/).

Azure IOT Central, cihaz bağlantısı hakkında daha fazla bilgi için bkz: [cihaz bağlantısı](concepts-connectivity.md).

## <a name="data-stores"></a>Veri depolama alanları

Azure IOT Central, uygulama verilerini bulutta depolar. Uygulama verileri depolanan içerir:

- Cihaz şablonları.
- Cihaz kimlikleri.
- Cihaz meta verileri.
- Kullanıcı ve rol verileri.

Azure IOT Central, cihazlardan gönderilen ölçüm verileri için bir zaman serisi depolama kullanır. Analytics hizmeti tarafından kullanılan cihazlardan zaman serisi verileri.

## <a name="analytics"></a>Analiz

Analytics service, uygulama görüntüler özel raporlama verileri oluşturmak için sorumludur. Bir işleç için [analizini özelleştirme](howto-create-analytics.md) uygulamada görüntülenir. Analiz Hizmeti üstünde oluşturulmuş [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) ve ölçüm verileri cihazlarınızdan gönderilen işler.

## <a name="rules-and-actions"></a>Kurallar ve eylemler

[Kurallar ve Eylemler](howto-create-telemetry-rules.md) uygulama içinde görevleri otomatikleştirmek için yakından birlikte çalışan. Bir oluşturucu, tanımlı bir eşiğin aşılması sıcaklık gibi cihaz telemetrisi temel alarak kurallar tanımlayabilirsiniz. Azure IOT Central, kural koşulları ne zaman karşılandığını belirlemek için bir akış işlemci kullanır. Bir kural koşulu karşılandığında, Oluşturucu tarafından tanımlanan bir eylem tetikler. Örneğin, bir eylem mühendisin sıcaklık bir aygıtta çok yüksek olduğunu bildiren bir e-posta gönderebilirsiniz.

## <a name="metadata-management"></a>Meta veri yönetimi

Azure IOT Central bir uygulamada, cihaz şablonları, cihaz türlerinin özellik ve davranışını tanımlar. Örneğin, bir buzdolabı için uygulamanızın gönderdiği telemetri buzdolabınız cihaz şablonu belirtir.

![Şablonu Mimarisi](media/concepts-architecture/template_architecture.png)

Bir cihaz şablonunda:

- **Ölçümleri** cihazın telemetri gönderen uygulamaya belirtin.
- **Ayarları** operatörün ayarlayabilirsiniz yapılandırmaları belirtin.
- **Özellikleri** operatörün ayarlayabilirsiniz meta verileri belirtin.
- **Kuralları** bir CİHAZDAN gönderilen verileri temel alan uygulama davranışını otomatikleştirin.
- **Panolar** uygulamasında bir cihazı özelleştirilebilir görünümleridir.

Bir uygulama, her cihaz şablonu temel alan bir veya daha fazla sanal ve gerçek cihazlar olabilir.

## <a name="role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC)

Bir [yönetici erişim kurallarını tanımlayabilir](howto-administer.md) önceden tanımlı roller kullanarak Azure IOT Central uygulaması için. Yönetici kullanıcılar uygulamanın hangi alanlarda kullanıcının erişebileceği belirlemek roller atayabilirsiniz.

## <a name="security"></a>Güvenlik

Azure IOT Central içindeki güvenlik özellikleri içerir:

- Veriler aktarımda ve bekleme sırasında şifrelenir.
- Kimlik doğrulaması, Azure Active Directory veya Microsoft Account tarafından sağlanır. İki öğeli kimlik doğrulaması desteklenir.
- Tam Kiracı yalıtımı.
- Cihaz düzeyinde güvenlik.

## <a name="ui-shell"></a>UI Kabuğu

Modern, duyarlı, HTML5 tarayıcı tabanlı bir uygulama UI kabuktur.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central'ın mimarisi hakkında bilgi edindiniz, hakkında bilgi edinmek için önerilen sonraki adım olan [cihaz bağlantısı](concepts-connectivity.md) Azure IOT Central içinde.