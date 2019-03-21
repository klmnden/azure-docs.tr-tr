---
title: Azure IoT Central nedir? | Microsoft Docs
description: Azure IOT Central, oluşturmak ve özel IOT çözümünüzü yönetmek için kullanabileceğiniz bir uçtan uca SaaS çözümüdür. Bu makalede Azure IoT Central’ın özelliklerine genel bir bakış sunulmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 9fc565996797c90a6d2ac9b3851ac3408f1842c7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58183280"
---
<!---
Purpose of an Overview article: 
1. To give a TECHNICAL overview of a service/product: What is it? Why should I use it? It's a "learn" topic that describes key benefits and our competitive advantage. It's not a "do" topic.
2. To help audiences who are new to service but who may be familiar with related concepts. 
3. To compare the service to another service/product that has some similar functionality, ex. SQL Database / SQL Data Warehouse, if appropriate. This info can be in a short list or table. 
-->

# <a name="what-is-azure-iot-central"></a>Azure IoT Central nedir?

Azure IoT Central, fiziksel ve dijital dünyaları bağlayan ürünler oluşturmayı kolaylaştıran, tam yönetilen bir IoT Hizmet Olarak Yazılım çözümüdür. Aşağıdakileri yaparak bağlı ürününüzün vizyonunu hayata geçirebilirsiniz:

- Müşterileriniz için daha iyi ürün ve deneyimleri etkinleştirmek amacıyla bağlı cihazlardan yeni içgörüler edinme.
- Kuruluşunuz için yeni iş fırsatları oluşturma.

Tipik bir IoT projesine kıyasla Azure IoT Central, aşağıdakileri yaparak bir IoT çözümünü yönetme zahmetini ortadan kaldırır:

- Yönetim yükünü azaltma.
- İşletme maliyetlerini ve ek yükleri azaltma.
- Aşağıdakilerden yararlanırken uygulamanızı özelleştirmeyi kolaylaştırma:
  - [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) ve [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) gibi sektör lideri teknolojiler.
  - Uçtan uca şifreleme gibi kurumsal düzeyde güvenlik özellikleri.

Aşağıdaki video Azure IoT Central’a genel bir bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

Bu makalenin geri kalanında Azure IoT Central hakkında aşağıdaki bilgiler verilmektedir:

- Bir projeyle ilişkili tipik kişilikler.
- Uygulamanızı oluşturma.
- Cihazlarınızı uygulamanıza bağlama
- Uygulamanızı yönetme.

## <a name="personas"></a>Kişilikler

Azure IoT Central belgeleri, bir Azure IoT Central uygulaması ile etkileşimde bulunan dört tipik kişiliğe başvurur:

- _Oluşturucu_, uygulamaya bağlanan cihaz türlerini tanımlamaktan ve uygulamayı operatör için özelleştirmekten sorumludur.
- _Operatör_, uygulamaya bağlı cihazları yönetir.
- _Yönetici_, uygulamanın içindeki kullanıcı ve rolleri yönetme gibi yönetim görevlerinden sorumludur.
- _Cihaz geliştiricisi_, uygulamanıza bağlı bir cihaz üzerinde çalışan kodu oluşturur.

## <a name="create-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızı oluşturma

Oluşturucu olarak Azure IoT Central’ı, kuruluşunuz için özel, bulutta barındırılan bir IoT çözümü oluşturmak için kullanırsınız. Özel bir IoT çözümü genellikle aşağıdakilerden oluşur:

- Cihazlarınızdan telemetri alan ve bu cihazları yönetmenizi sağlayan bulut tabanlı bir uygulama.
- Bulut tabanlı uygulamanıza bağlı özel kod çalıştıran birden fazla cihaz.

Yeni bir Azure IoT Central uygulamasını hızlıca dağıtabilir ve sonra tarayıcınızdan size özel gereksinimlere göre kişiselleştirebilirsiniz. Azure IoT Central oluşturucusu olarak, uygulamanıza bağlanan cihazlar için bir _cihaz şablonu_ oluşturmak üzere web tabanlı araçları kullanabilirsiniz. Cihaz şablonu bir cihaz modelinin şemasıdır. Aynı cihaz şablonundan oluşturulan tüm cihazlar şablonu ortak olarak kullanır. Cihaz şablonu, bir cihaz türünün aşağıdaki gibi özellik ve davranışlarını tanımlar:

- Gönderdiği telemetri.
- Bir operatörün değiştirebileceği iş özellikleri.
- Bir cihaz tarafından ayarlanan ve uygulamada salt okunur özellikte olan cihaz özellikleri.
- Uygulamanın yanıt verdiği eşikler.
- Cihazın davranışını belirleyen ayarlar.

Cihaz şablonlarınızı ve uygulamanızı Azure IoT Central’ın sizin için oluşturduğu sanal verilerle hemen test edebilirsiniz.

Oluşturucu olarak, Azure IoT Central uygulamanızın kullanıcı arabirimini uygulamanızın günlük kullanımından sorumlu operatörler için de özelleştirebilirsiniz. Bir oluşturucu aşağıdaki özelleştirmeleri yapabilir:

- Bir cihaz şablonundaki özellik ve ayarların düzenini tanımlama.
- Operatörlerin içgörüleri keşfetmesine ve sorunları daha hızlı çözümlemesine yardımcı olacak özel panoları yapılandırma.
- Bağlı cihazlarınızdan zaman serisi verilerini keşfetmek için özel analizler yapılandırma.

## <a name="connect-your-devices"></a>Cihazlarınızı bağlama

Oluşturucunun uygulamaya bağlanabilen cihaz türlerini tanımlamasından sonra, cihaz geliştiricisi cihazlar üzerinde çalıştırılacak kodu oluşturur. Cihaz geliştiricisi olarak, cihaz kodunuzu oluşturmak için Microsoft'un açık kaynak [Azure IoT SDK’larını](https://github.com/Azure/azure-iot-sdks) kullanırsınız. Bu SDK’lar, Azure IoT Central uygulamanıza cihazlarınızı bağlama gereksinimlerinizi karşılamak üzere geniş dil, platform ve protokol desteği sunar. SDK’lar Azure IoT Central’a bağlı cihaz üzerinde aşağıdaki görevleri gerçekleştirmenize yardımcı olur:

- Güvenli bir bağlantı oluşturma.
- Telemetri gönderme.
- Durum bildirme.
- Yapılandırma güncelleştirmeleri alma.

Daha fazla bilgi için, [Azure IoT SDK’larını kullanmanın avantajları ve kullanmamanız durumunda kaçınılması gereken tuzaklar](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) başlıklı blog gönderisine bakın.

## <a name="manage-your-application"></a>Uygulamanızı yönetme

Azure IoT Central uygulamaları tamamen Microsoft tarafından barındırılır, böylece uygulamalarınızı yönetmenin idari ek yükü azalır.

Bir operatör olarak, Azure IoT Central uygulamanızı kullanarak Azure IoT Central çözümünüzdeki cihazları yönetebilirsiniz. Operatörler aşağıdaki gibi görevleri gerçekleştirebilir:

- Uygulamaya bağlı cihazları izleme.
- Cihazlarla ilgili sorunları giderme ve düzeltme.
- Yeni cihazlar hazırlama.

Oluşturucu, cihaz şablonu düzeyindeki akış verileri üzerinde çalışan özel kurallar ve eylemler tanımlayabilir. Operatör ise uygulama içindeki görevleri denetlemek ve otomatik hale getirmek için cihaz düzeyinde bu kuralları etkinleştirebilir ya da devre dışı bırakabilir.

Yöneticiler, [kullanıcı rolleri ve izinleri](howto-administer.md) ile uygulamanıza erişimi yönetebilir.

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
