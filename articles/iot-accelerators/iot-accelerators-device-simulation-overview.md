---
title: Cihaz benzetimi genel bakış - Azure | Microsoft Docs
description: Cihaz benzetimi çözüm Hızlandırıcısını ve özellikleri açıklaması.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 12/03/2018
ms.author: dobett
ms.openlocfilehash: f58eb05ed582cf18157a76f4d637d72a228f4e96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65464890"
---
# <a name="device-simulation-solution-accelerator-overview"></a>Cihaz Simülasyonu çözüm hızlandırıcısına genel bakış

Bir bulut tabanlı IOT çözümünde, bulut uç noktasına sıcaklık, konumu ve durumu gibi telemetri göndermek için cihazlarınızı bağlayın. Çözümünüzü eylemleri veya ınsights türetmeniz olanak tanıyarak bu telemetri, tüketir.

Bir IOT çözümü geliştirmek, deneme ve test işlemin önemli bölümleri şunlardır. Benzetim, bu süreci boyunca önemli bir araçtır. Cihaz benzetimi ile şunları yapabilirsiniz:

* Hızlı bir şekilde prototip çalışmaya başlamasına ve ardından ayarlayarak yineleme benzetimli cihaz davranışı hareket halindeyken. Bu işlem, maliyetli donanıma yatırım yapmadan önce fikir kanıtlamak sağlar. Özel cihazları web saniyeler içinde bir prototip cihaz oluşturmak için kullanıcı Arabirimi aracılığıyla oluşturabilirsiniz.
* Gerçek dünyadaki cihaz davranışlarının simülasyonunu yaparak çözümün cihazda beklendiği şekilde çalıştığını doğrulayın. Karmaşık cihaz davranışlarının gerçekçi sanal telemetri oluşturmak için JavaScript kullanarak betik oluşturabilirsiniz.
* Ölçek benzetimi normal, en yüksek ve en yüksek yük koşullar ötesinde çözümünüzü test edin. Ölçek testleri ayrıca doğru boyuta çözümünüzü çalıştırmak için gereken Azure kaynakları yardımcı olur.

![Örnek insansız hava aracı ile benzetimi](media/iot-accelerators-device-simulation-overview/dronesimulation.png)

Cihaz benzetimi, gerçek cihazlarınızı benzetimini yapmak için cihaz modelleri tanımlayabilirsiniz. Bu model, ileti formatları ikiz özelliklerini ve yöntemlerini içerir. Ayrıca, JavaScript ile karmaşık cihaz davranışlarının benzetimini yapabilirsiniz.

İçin tüm IOT hub'a bağlanan cihazlara binlerce simülasyon çalıştırabilirsiniz. Test ile yardımcı olmak için isteğe bağlı olarak bir tek başına ortam için bir IOT hub cihaz benzetimi birlikte dağıtabilirsiniz.

Cihaz benzetimi ücretsizdir. Ancak cihaz benzetimi, bulutta Azure aboneliğinize dağıtılır ve Azure kaynaklarını tüketebilir. Cihaz benzetimi, gereksinimlerinizi karşılamaması durumunda [kaynak kodu Github'da kullanılabilir ayrıca](https://github.com/Azure/device-simulation-dotnet) kopyalayın ve değiştirmek için.

## <a name="sample-simulations"></a>Örnek benzetimleri

Cihaz benzetimini dağıtma, bazı örnek simülasyonlar ve örnek cihazı alın. Cihaz benzetimi kullanma hakkında bilgi edinmek için bu örnekleri'ni kullanabilirsiniz. Başlamak için çalıştırma bir [örnek 10 kamyon benzetim benzetimi](quickstart-device-simulation-deploy.md). Ayrıca [sağlanan birçok örnek cihazı birini kullanarak kendi benzetimi oluşturmak](iot-accelerators-device-simulation-create-simulation.md).

![Benzetim yapılandırması](media/iot-accelerators-device-simulation-overview/samplesimulation1.png)

## <a name="custom-simulated-devices"></a>Özel sanal cihazlar

Cihaz benzetimi için kullanabileceğiniz [özel cihaz modelleri oluşturma](iot-accelerators-device-simulation-create-custom-device.md) simülasyonlarınızın içinde kullanılacak. Örneğin, sıcaklık ve nem telemetrisini gönderen yeni bir buzdolabı cihaz modeli tanımlayabilirsiniz. Özel sanal cihazlar, rastgele ile basit cihaz davranışlarının, artırma veya azaltma telemetri değerleri için idealdir.

![Cihaz modelini oluşturma](media/iot-accelerators-device-simulation-overview/adddevicemodel.png)

## <a name="advanced-simulated-devices"></a>Gelişmiş sanal cihazlar

Bir cihazın gönderdiği telemetri değerleri hakkında daha fazla denetime ihtiyacınız olduğunda, bir Gelişmiş cihaz modelini kullanabilirsiniz. Gelişmiş cihaz modelleri gönderilen telemetri değerlerini değiştirmek JavaScript desteği etkinleştirin. Örneğin, iç sıcaklık park etmiş bir araba benzetimini sık erişimli güneşli gün - dış sıcaklık olarak, iç sıcaklık katlanarak artar.

Gelişmiş cihaz modelleri izin [oluşturup kendi cihaz modelleri karşıya](iot-accelerators-device-simulation-advanced-device.md) JSON cihaz tanımı oluşur dosyası ve karşılık gelen JavaScript dosyaları.

Gelişmiş cihaz modelleri şunları yapmanızı sağlar:

* Telemetri türleri ile birlikte bir CİHAZDAN ileti biçimi gönderilen belirtin.
* Zaman içinde cihaz durumunu korumak telemetri değerleri oluşturmak için özel komut dosyası kullanın.
* Sanal cihaz yöntemlerini nasıl yanıt verdiğini belirlemek için özel komut dosyası kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, cihaz benzetimi çözüm Hızlandırıcısını ve özellikleri hakkında bilgi edindiniz. Çözüm Hızlandırıcısını kullanmaya başlamak için hızlı başlangıcı ile devam edin:

> [!div class="nextstepaction"]
> [Dağıtın ve bir IOT cihaz benzetimi Azure'da çalıştırın](quickstart-device-simulation-deploy.md)
