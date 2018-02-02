---
title: "Azure IOT Hub Geliştirici Kılavuzu | Microsoft Docs"
description: "Azure IOT Hub Geliştirici Kılavuzu uç noktaları, güvenlik, kimlik kayıt defteri, cihaz yönetimi, doğrudan yöntemleri, cihaz çiftlerini, dosya yüklemeleri, işleri, IOT hub'ı sorgu dili tartışmalarını içerir ve mesajlaşma."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 37f9da7dcc8dd527fe0bfbf2fbcc40a3ba0e8a1c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-iot-hub-developer-guide"></a>Azure IOT Hub Geliştirici Kılavuzu

Azure IOT Hub, milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen hizmet etkinleştirin ve bir çözüm arka ucu ' dir.

Azure IOT Hub ile sağlar:

* Cihaz başına güvenlik kimlik bilgileri kullanılarak güvenli iletişim ve erişim denetimi.
* Birden çok cihaz Bulut ve bulut-cihaz hiper ölçekli iletişim seçenekleri.
* Sorgulanabilir depolama aygıt başına durum bilgilerini ve meta verileri.
* En popüler diller ve platformlar için cihaz kitaplıklarını ile kolay cihaz bağlantısı.

Bu IOT Hub Geliştirici Kılavuzu, aşağıdaki makaleler içerir:

* [Cihaz bulut iletişimi Kılavuzu] [ lnk-d2c-guidance] cihaz bulut iletilerini, cihaz çifti'nın bildirilen özellikleri ve karşıya dosya yükleme arasında seçtiğiniz yardımcı olur.
* [Bulut-cihaz iletişimi Kılavuzu] [ lnk-c2d-guidance] doğrudan yöntemleri, cihaz çifti'nın istenen özellikleri ve bulut-cihaz iletilerini arasında seçtiğiniz yardımcı olur.
* [Cihaz Bulut ve bulut cihaz IOT Hub ile Mesajlaşma] [ devguide-messaging] IOT hub'ı sunan Mesajlaşma özellikleri (cihaz Bulut ve bulut-cihaz) açıklar.
  * [IOT Hub cihaz bulut iletilerini göndermek][devguide-messages-d2c].
  * [Yerleşik uç noktasından cihaz bulut iletilerini okumanızı][devguide-builtin].
  * [Özel uç noktaları ve yönlendirme kuralları için cihaz bulut iletilerini kullanmak][devguide-custom].
  * [IOT Hub'ından bulut-cihaz iletilerini göndermek][devguide-messages-c2d].
  * [Oluşturun ve IOT hub'ı iletileri okumak][devguide-format].
* [Bir CİHAZDAN dosyaları karşıya yükleme] [ devguide-upload] aygıttan dosyaları karşıya nasıl yükleneceğini açıklar. Makale ayrıca karşıya yükleme işlemi gönderebilirsiniz bildirimleri gibi konular hakkında bilgi içerir.
* [IOT Hub cihaz kimliklerini yönetmek] [ devguide-identities] hangi bilgilerin açıklar her IOT hub'ın kimlik kayıt defteri depolar. Makale ayrıca erişmek ve değiştirmek nasıl açıklanmaktadır.
* [IOT Hub'ına erişim denetim] [ devguide-security] hem de IOT hub'ı işlevselliği erişim ve bileşenleri bulut için kullanılan güvenlik modeli açıklanır. Makaleyi belirteçleri ve X.509 sertifikalarını ve erişim izni verebilir izinleri ayrıntılarını kullanma hakkında bilgi içerir.
* [Durum ve yapılandırmaları eşitlemek için cihaz çiftlerini kullanmak] [ devguide-device-twins] açıklar *cihaz çifti* kavram. Makaleyi da descibes işlevselliği cihaz çiftlerini, bir cihaz, cihaz çifti ile eşitleme gibi açın. Makale bir cihaz çiftine depolanan verileri hakkında bilgiler içerir.
* [Bir cihazda doğrudan bir yöntem çağırma] [ devguide-directmethods] doğrudan bir yöntem yaşam döngüsü açıklar. Makale, arka uç uygulama cihaza yöntemleri çağırma ve Cihazınızda doğrudan yöntemi işlemek açıklar.
* [Zamanlama işlerini birden çok aygıta] [ devguide-jobs] işleri birden çok aygıta nasıl zamanlayabilirsiniz açıklar. Makale olarak cihaz çiftine kullanarak bir cihazı güncelleştirme doğrudan bir yöntem yürütülen görevleri gerçekleştiren iş göndermek açıklar. Ayrıca, bir işin durumunu sorgulamak nasıl açıklanır.
* [Başvuru - iletişim protokolü seçin] [ devguide-protocol] IOT Hub cihaz iletişimi destekler ve açık olması gereken bağlantı noktalarını listeler iletişim protokolleri açıklar.
* [Başvuru - IOT Hub uç noktaları] [ devguide-endpoints] her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunan çeşitli uç noktaları açıklar. Makalede ayrıca, IOT hub'ınıza ek uç noktaları nasıl oluşturabileceğinizi ve bir alan ağ geçidi, IOT Hub uç noktaları standart senaryolarda bağlantıyı etkinleştirmek için nasıl kullanılacağı açıklanmaktadır.
* [Başvuru - cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgu dili] [ devguide-query] hub'ınızın cihaz çiftlerini ve işleri hakkında bilgi almak sağlar, IOT hub'ı sorgu dili açıklar.
* [Başvuru - kotalar ve azaltma] [ devguide-quotas] kota aştığında oluşan IOT Hub hizmeti ve azaltma kotalar özetler.
* [Başvuru - fiyatlandırma] [ devguide-pricing] farklı SKU'ları ve IOT Hub ve nasıl çeşitli IOT hub'ı işlevler iletileri olarak IOT Hub tarafından ölçülen ile ilgili ayrıntılar için fiyatlandırma hakkında genel bilgiler sağlar.
* [Başvuru - cihazını ve hizmetini SDK'ları] [ devguide-sdks] IOT hub'ınıza etkileşim cihazını ve hizmetini uygulamaları geliştirmek için Azure IOT SDK'ları listeler. Makaleyi çevrimiçi API belgeleri bağlantılarını içerir.
* [Başvuru - IOT Hub MQTT Destek] [ devguide-mqtt] MQTT Protokolü IOT hub'ı nasıl desteklediği hakkında ayrıntılı bilgiler sağlar. Makale için Azure IOT SDK'ları yerleşik MQTT protokolü desteği açıklar ve MQTT protokolü kullanarak doğrudan hakkında bilgi sağlar.
* [Sözlük] [ devguide-glossary] ortak IOT Hub ile ilgili terimlerin bir listesi.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
