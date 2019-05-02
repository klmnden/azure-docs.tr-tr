---
title: Azure IoT Central nedir? | Microsoft Docs
description: Azure IOT Central, oluşturmak ve özel IOT çözümünüzü yönetmek için kullanabileceğiniz bir uçtan uca SaaS çözümüdür. Bu makalede Azure IoT Central’ın özelliklerine genel bir bakış sunulmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 04/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 84fa7aa006a6bc5365527dbf8043797617543590
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704531"
---
<!---
Purpose of an Overview article: 
1. To give a TECHNICAL overview of a service/product: What is it? Why should I use it? It's a "learn" topic that describes key benefits and our competitive advantage. It's not a "do" topic.
2. To help audiences who are new to service but who may be familiar with related concepts. 
3. To compare the service to another service/product that has some similar functionality, ex. SQL Database / SQL Data Warehouse, if appropriate. This info can be in a short list or table. 
-->

# <a name="what-is-azure-iot-central"></a>Azure IoT Central nedir?

Azure IOT Central, fiziksel ve dijital dünyaları bağlanan ürünler oluşturmak kolaylaştıran tam olarak yönetilen bir IOT hizmet olarak yazılım çözümüdür. Aşağıdakileri yaparak bağlı ürününüzün vizyonunu hayata geçirebilirsiniz:

- Müşterileriniz için daha iyi ürün ve deneyimleri etkinleştirmek amacıyla bağlı cihazlardan yeni içgörüler edinme.
- Kuruluşunuz için yeni iş fırsatları oluşturma.

Azure IOT Central, normal bir IOT projesinin karşılaştırıldığında:

- Yönetim yükünü azaltır.
- İşletimsel maliyetleri ve ek yüklerini azaltır.
- Özelleştirme ile çalışırken, uygulamanızı daha kolay hale getirir:
  - [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) ve [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) gibi sektör lideri teknolojiler.
  - Uçtan uca şifreleme gibi kurumsal düzeyde güvenlik özellikleri.

Aşağıdaki video Azure IoT Central’a genel bir bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

Bu makalede, Azure IOT Central için özetlenmektedir:

- Bir projeyle ilişkili tipik kişilikler.
- Uygulamanızı oluşturma.
- Cihazlarınızı uygulamanıza bağlama
- Uygulamanızı yönetme.

## <a name="personas"></a>Kişilikler

Azure IOT Central belgeleri, Azure IOT Central uygulamayla etkileşime geçen dört kişilik ifade eder:

- _Oluşturucu_, uygulamaya bağlanan cihaz türlerini tanımlamaktan ve uygulamayı operatör için özelleştirmekten sorumludur.
- _Operatör_, uygulamaya bağlı cihazları yönetir.
- _Yönetici_, uygulamanın içindeki kullanıcı ve rolleri yönetme gibi yönetim görevlerinden sorumludur.
- _Cihaz geliştiricisi_, uygulamanıza bağlı bir cihaz üzerinde çalışan kodu oluşturur.

## <a name="create-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızı oluşturma

Oluşturucu olarak Azure IoT Central’ı, kuruluşunuz için özel, bulutta barındırılan bir IoT çözümü oluşturmak için kullanırsınız. Özel bir IoT çözümü genellikle aşağıdakilerden oluşur:

- Cihazlarınızdan telemetri alan ve bu cihazları yönetmenizi sağlayan bulut tabanlı bir uygulama.
- Bulut tabanlı uygulamanıza bağlı özel kod çalıştıran birden fazla cihaz.

Yeni bir Azure IOT Central uygulamasına hızla dağıtın ve ardından tarayıcınızda belirli gereksinimlerinizi özelleştirin. Bir oluşturucu web tabanlı araçlar oluşturmak için kullandığınız bir _cihaz şablonu_ uygulamanıza bağlanan cihazlar için. Bir cihaz şablonu gibi özellikleri ve cihaz türünü davranışını tanımlayan bir şema olarak:

- Telemetri gönderir.
- Bir operatörün değiştirebileceği iş özellikleri.
- Bir cihaz tarafından ayarlanan ve uygulamada salt okunur özellikte olan cihaz özellikleri.
- Eşikleri için uygulama yanıt verir.
- Cihazın davranışını belirleyen ayarlar.

Cihaz şablonlarınızı ve uygulamanızı Azure IoT Central’ın sizin için oluşturduğu sanal verilerle hemen test edebilirsiniz.

Oluşturucu olarak, Azure IoT Central uygulamanızın kullanıcı arabirimini uygulamanızın günlük kullanımından sorumlu operatörler için de özelleştirebilirsiniz. Bir oluşturucu aşağıdaki özelleştirmeleri yapabilir:

- Bir cihaz şablonundaki özellik ve ayarların düzenini tanımlama.
- Operatörlerin içgörüleri keşfetmesine ve sorunları daha hızlı çözümlemesine yardımcı olacak özel panoları yapılandırma.
- Bağlı cihazlarınızdan zaman serisi verilerini keşfetmek için özel analizler yapılandırma.

## <a name="connect-your-devices"></a>Cihazlarınızı bağlama

Oluşturucunun uygulamaya bağlanabilen cihaz türlerini tanımlamasından sonra, cihaz geliştiricisi cihazlar üzerinde çalıştırılacak kodu oluşturur. Cihaz geliştiricisi olarak, cihaz kodunuzu oluşturmak için Microsoft'un açık kaynak [Azure IoT SDK’larını](https://github.com/Azure/azure-iot-sdks) kullanırsınız. Bu SDK’lar, Azure IoT Central uygulamanıza cihazlarınızı bağlama gereksinimlerinizi karşılamak üzere geniş dil, platform ve protokol desteği sunar. SDK'ları aşağıdaki cihaz özelliklerini uygulamanıza yardımcı olur:

- Güvenli bir bağlantı oluşturma.
- Telemetri gönderme.
- Durum bildirme.
- Yapılandırma güncelleştirmeleri alma.

Daha fazla bilgi için, [Azure IoT SDK’larını kullanmanın avantajları ve kullanmamanız durumunda kaçınılması gereken tuzaklar](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) başlıklı blog gönderisine bakın.

## <a name="manage-your-application"></a>Uygulamanızı yönetme

Azure IoT Central uygulamaları tamamen Microsoft tarafından barındırılır, böylece uygulamalarınızı yönetmenin idari ek yükü azalır.

Bir operatör olarak, Azure IoT Central uygulamanızı kullanarak Azure IoT Central çözümünüzdeki cihazları yönetebilirsiniz. İşleçler gibi görevleri yapın:

- Uygulamaya bağlı cihazları izleme.
- Cihazlarla ilgili sorunları giderme ve düzeltme.
- Yeni cihazlar hazırlama.

Bir oluşturucu özel kurallar ve bağlı cihazlardan akış verileri üzerinde çalışan eylemlerin tanımlayabilirsiniz. Operatör ise uygulama içindeki görevleri denetlemek ve otomatik hale getirmek için cihaz düzeyinde bu kuralları etkinleştirebilir ya da devre dışı bırakabilir.

Yöneticiler, uygulamanıza erişimi Yönet [kullanıcı rolleri ve izinleri](howto-administer.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize göre, aşağıdaki önerilen adımlara geçebilirsiniz:

- [Azure IoT Central ile Azure IoT çözüm hızlandırıcıları](overview-iot-options.md) arasındaki farklılıkları anlayın.
- [Azure IoT Central Kullanıcı Arabirimi](overview-iot-central-tour.md)’ni tanıma.
- [Azure IoT Central uygulaması oluşturmaya](quick-deploy-iot-central.md) bağlama.
- Aşağıdaki işlemlerin nasıl yapılacağını gösteren bir öğretici dizisini takip etme:
  - [Oluşturucu olarak, bir cihaz şablonu oluşturma](tutorial-define-device-type.md)
  - [Oluşturucu olarak, çözümünüzü otomatikleştirmek için kurallar ekleme](tutorial-configure-rules.md)
  - [Oluşturucu olarak, uygulamanızı operatörler için özelleştirme](tutorial-customize-operator.md)
  - [Operatör olarak, cihazlarınızı izleme](tutorial-monitor-devices.md)
  - [Operatör olarak, çözümünüze gerçek bir cihaz ekleme](tutorial-add-device.md)
  - [Cihaz geliştiricisi olarak, cihazlarınız için kod oluşturma](tutorial-add-device.md#prepare-the-client-code)
