---
title: Azure IOT Merkezi mimari kavramlarını | Microsoft Docs
description: Bu makale Azure IOT merkezi mimarisi ile ilgili temel kavramları tanıtır
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 44408e7b6ad1a068f265bf7b78d973e6aae3001b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628768"
---
# <a name="azure-iot-central-architecture"></a>Azure IOT Orta mimarisi

Bu makalede, Microsoft Azure IOT merkezi mimarisine genel bakış sağlar.

![Üst düzey mimari](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Cihazlar

Cihazların Azure IOT merkezi uygulamanız ile veri değişimi. Bir aygıt yapabilirsiniz:

- Ölçümleri telemetri gibi gönderin.
- Ayarları uygulamanız ile eşitleyin.

Azure IOT Merkezi, bir aygıt ile uygulamanızı değiştirebilir veri aygıt şablonda belirtilen. Cihaz şablonları hakkında daha fazla bilgi için bkz: [meta verileri Yönetim](#metadata-management).

Cihazların Azure IOT merkezi uygulamanızı nasıl bağlanacağını hakkında daha fazla bilgi için bkz: [cihaz bağlantısı](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Bulut ağ geçidi

Azure IOT Orta Azure IOT Hub cihaz bağlantısı sağlayan bir bulut ağ geçidi olarak kullanır. IOT hub'ı etkinleştirir:

- Bulut ölçeğinde veri alımı.
- Aygıt Yönetimi.
- Cihaz bağlantısı güvenliğini sağlayın.

IOT Hub hakkında daha fazla bilgi için bkz: [Azure IOT Hub](https://docs.microsoft.com/azure/iot-hub/).

Cihaz Bağlantısı'nda Azure IOT Merkezi hakkında daha fazla bilgi için bkz: [cihaz bağlantısı](concepts-connectivity.md).

## <a name="data-stores"></a>Veri depolama alanları

Azure IOT Orta bulutta uygulama verilerini depolar. Uygulama verileri depolanan şunları içerir:

- Cihaz şablonları.
- Cihaz kimlikleri.
- Cihaz meta veriler.
- Kullanıcı ve rol verileri.

Azure IOT Orta bir zaman serisi depolamak için aygıtlardan gönderilen ölçüm verilerini kullanır. Zaman serisi veri analizi hizmeti tarafından kullanılan aygıtlardan.

## <a name="analytics"></a>Analiz

Analytics hizmeti, uygulama görüntüler özel raporlama verilerini oluşturmaktan sorumlu. Bir işleç için [analytics özelleştirme](howto-create-analytics.md) uygulamada görüntülenir. Analytics hizmeti üstünde oluşturulmuş [Azure zaman serisi Öngörüler](https://azure.microsoft.com/services/time-series-insights/) ve cihazlarınızın gönderilen ölçüm verilerini işler.

## <a name="rules-and-actions"></a>Kurallar ve eylemler

[Kurallar ve Eylemler](howto-create-telemetry-rules.md) uygulama içinde görevleri otomatikleştirmek için yakından birlikte çalışma. Oluşturucu cihaz telemetrisi tanımlı bir eşiğin aşılması sıcaklık gibi temel alarak kurallar tanımlayabilirsiniz. Azure IOT Orta akış işlemci zaman kural koşulların karşılanıp karşılanmadığını belirlemek için kullanır. Bir kural koşulu karşılandığında Oluşturucu tarafından tanımlanan bir eylemi tetikler. Örneğin, bir eylem mühendisin aygıtta sıcaklık çok yüksek olduğunu bildiren bir e-posta gönderebilirsiniz.

## <a name="metadata-management"></a>Meta veri yönetimi

Azure IOT merkezi bir uygulamada, cihaz şablonları davranış ve aygıt türlerinin özelliği tanımlar. Örneğin, bir buzdolabı aygıt şablon uygulamanıza bir buzdolabı gönderdiği telemetri belirtir.

![Şablonu Mimarisi](media/concepts-architecture/template_architecture.png)

Bir aygıt şablonunda:

- **Ölçümleri** cihaz telemetri gönderen uygulama belirtin.
- **Ayarları** bir işleç ayarlayabilirsiniz yapılandırmalarını belirtin.
- **Özellikler** bir işleç ayarlayabilirsiniz meta verileri belirtin.
- **Kuralları** davranışı bir aygıttan gönderilen verileri temel alan uygulamasında otomatikleştirin.
- **Panolar** uygulamasında bir cihazı özelleştirilebilir görünümleridir.

Bir uygulama, her cihaz şablonuna dayalı bir veya daha fazla benzetilmiş ve gerçek aygıtları sahip olabilir.

## <a name="rbac"></a>RBAC

Bir [yönetici erişim kurallarını tanımlayabilir](howto-administer.md) önceden tanımlanmış roller kullanarak Azure IOT merkezi bir uygulama için. Yönetici kullanıcılar uygulamanın hangi alanlarda kullanıcının erişimi belirlemek roller atayabilirsiniz.

## <a name="security"></a>Güvenlik

Azure IOT Merkezi içindeki güvenlik özellikleri içerir:

- Veri aktarım ve REST şifrelenir.
- Kimlik doğrulaması Azure Active Directory veya Microsoft Account tarafından sağlanır. İki öğeli kimlik doğrulama desteklenir.
- Tam Kiracı yalıtımı.
- Aygıt düzeyi güvenlik.

## <a name="ui-shell"></a>UI Kabuğu

UI Kabuk bir modern, esnek, HTML5 tarayıcı tabanlı uygulamasıdır.

## <a name="next-steps"></a>Sonraki adımlar

Mimarisiyle Azure IOT Merkezi hakkında öğrendiniz, önerilen sonraki hakkında bilgi edinmek için adımdır [cihaz bağlantısı](concepts-connectivity.md) Azure IOT merkezi olarak.