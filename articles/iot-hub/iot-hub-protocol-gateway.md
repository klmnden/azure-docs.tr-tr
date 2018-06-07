---
title: Azure IOT protokolü ağ geçidini | Microsoft Docs
description: IOT Hub tarafından yerel olarak desteklenmeyen protokolleri kullanarak hub'ınıza bağlanmak için özellikleri ve protokol desteği cihazlarının sağlamak için IOT hub'ı genişletmek için bir Azure IOT protokolü ağ geçidini kullanmak nasıl.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: elioda
ms.openlocfilehash: 2c90ee899d0002d41ca21ed4a4927470ee53b2e1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635313"
---
# <a name="support-additional-protocols-for-iot-hub"></a>IOT hub'ına yönelik destek ek protokoller
Azure IOT hub'ı yerel olarak MQTT, AMQP ve HTTPS protokolleri iletişimi destekler. Bazı durumlarda, aygıtları veya alan ağ geçidi Protokolü uyarlama gerektirir ve bu standart protokollerden birini kullanmak mümkün olmayabilir. Böyle durumlarda, özel bir ağ geçidi kullanabilirsiniz. Özel bir ağ geçidi Protokolü uyarlama IOT Hub uç noktaları için IOT hub'ı gelen ve giden trafiği köprü oluşturma sağlar. Kullanabileceğiniz [Azure IOT protokolü ağ geçidini](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) Protokolü uyarlama IOT hub'ı etkinleştirmek için özel bir ağ geçidi olarak.

## <a name="azure-iot-protocol-gateway"></a>Azure IOT protokolü ağ geçidi
Azure IOT protokolü ağ geçidini, büyük ölçekli, IOT Hub ile çift yönlü aygıt iletişim için tasarlanmış Protokolü uyarlama için bir çerçevedir. Protokol ağ geçidi, belirli bir protokolü üzerinden cihaz bağlantılarını kabul doğrudan bir bileşenidir. IOT Hub'ına trafiği AMQP 1.0 arasında köprü. 

Azure Service Fabric, Azure Cloud Services çalışan roller ya da Windows sanal makineleri kullanarak yüksek düzeyde ölçeklenebilir bir şekilde protokol ağ geçidi azure'da dağıtabilirsiniz. Ayrıca, protokol ağ geçidi alan ağ geçitleri gibi şirket içi ortamlarda dağıtılabilir.

Azure IOT protokolü ağ geçidi, MQTT protokol davranışı gerekirse özelleştirmenize olanak tanıyan bir MQTT Protokolü bağdaştırıcısını içerir. IOT hub'ı MQTT v3.1.1 protokolü için yerleşik destek sağlayan bu yana yalnızca Protokolü özelleştirmeleri veya ek işlevsellik için belirli gereksinimler gerekirse MQTT Protokolü bağdaştırıcısını kullanmayı düşünmeniz gerekir.

MQTT bağdaştırıcısı de diğer protokolleri için iletişim kuralı bağdaştırıcıları oluşturmak için programlama modelini gösterir. Ayrıca, Azure IOT protokolü ağ geçidi programlama modeli özel kimlik doğrulama, ileti dönüşümleri, sıkıştırma/açma veya şifreleme/şifre çözme trafik gibi özel işleme için özel bileşenleri takın sağlar cihazlar ve IOT hub'ı.

Esneklik için açık kaynaklı yazılım projesinde Azure IOT protokolü ağ geçidini ve MQTT uygulama sağlanır. Çeşitli protokolleri ve protokol sürümleri için destek eklemek için açık kaynaklı proje kullanabilir veya Uygulama senaryonuz için özelleştirebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Azure IOT protokolü ağ geçidini ve IOT çözümünüzün bir parçası olarak dağıtın ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz:

* [Github'daki Azure IOT protokolü ağ geçidi deposu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IOT protokolü ağ geçidi Geliştirici Kılavuzu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

IOT hub'ı dağıtımınızı planlama hakkında daha fazla bilgi için bkz:

* [Event Hubs ile karşılaştırın][lnk-compare]
* [Ölçeklendirme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma][lnk-scaling]
* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
