---
title: Azure IOT protokolü ağ geçidini | Microsoft Docs
description: IOT Hub ile IOT Hub tarafından yerel olarak desteklenmeyen protokolleri hub'ınıza bağlanmak için özellikleri ve protokol desteği cihazlarını etkinleştirmek için genişletmek için bir Azure IOT protokolü ağ geçidini kullanma
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/11/2017
ms.openlocfilehash: 9dbb7905c2a0fed65ede610577e0fa11a1deef92
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345405"
---
# <a name="support-additional-protocols-for-iot-hub"></a>IOT hub'ı için ek protokol desteği

Azure IOT Hub, yerel olarak MQTT, AMQP ve HTTPS protokolleri üzerinden iletişimi destekler. Bazı durumlarda, cihazlar veya alan ağ geçitleri aşağıdaki standart protokollerden birini kullanın ve gerekli Protokolü uyarlama mümkün olmayabilir. Böyle durumlarda, özel bir ağ geçidi kullanabilirsiniz. Özel bir ağ geçidi, IOT hub'ı gelen ve giden trafiği köprüleme ile IOT Hub uç noktaları için protokol uyarlama sağlar. Kullanabileceğiniz [Azure IOT protokolü ağ geçidini](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) Protokolü uyarlama için IOT hub'ı etkinleştirmek için özel bir ağ geçidi olarak.

## <a name="azure-iot-protocol-gateway"></a>Azure IOT protokol ağ geçidi

Azure IOT protokol ağ geçidi, yüksek ölçekli, IOT Hub ile çift yönlü iletişim için tasarlanmış Protokolü uyarlama için bir çerçevedir. Protokol ağ geçidi, belirli bir protokolü üzerinden cihaz bağlantılarını kabul eden doğrudan bir bileşendir. IOT hub'ı trafiği AMQP 1.0 arasında köprü.

Azure Service Fabric, Azure Cloud Services çalışan rolleri ya da Windows sanal makineleri kullanarak azure'da protokol ağ geçidi yüksek düzeyde ölçeklenebilir bir yolla dağıtabilirsiniz. Ayrıca, alan ağ geçitleri gibi şirket içi ortamlarda protokol ağ geçidi dağıtılabilir.

Azure IOT protokol ağ geçidi, MQTT protokol davranışı gerekirse özelleştirmenize olanak tanıyan bir MQTT protokolünü bağdaştırıcı içerir. IOT hub'ı MQTT v3.1.1 protokolü için yerleşik destek sağlar. bu yana yalnızca Protokolü özelleştirmeleri veya belirli gereksinimleri için ek işlevler gerekmiyorsa, MQTT protokolünü bağdaştırıcısı kullanmayı düşünmeniz gerekir.

MQTT bağdaştırıcısı de diğer protokolleri için iletişim kuralı bağdaştırıcıları oluşturmaya yönelik programlama modelini gösterir. Ayrıca, Azure IOT protokol ağ geçidi programlama modeli, özel bileşenler için özel kimlik doğrulama, ileti dönüştürmeleri, sıkıştırma/sıkıştırma veya şifreleme/şifre çözme arasındaki trafiği gibi özelleştirilmiş işlem takın sağlar cihazlar ve IOT hub'ı.

Esneklik için Azure IOT protokolü ağ geçidini ve MQTT uygulama açık kaynak yazılım projesinde sağlanmıştır. Senaryonuz için uygulamayı özelleştirme veya çeşitli protokoller ve protokol sürümleri için destek eklemek için açık kaynaklı proje kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT protokolü ağ geçidini ve IOT çözümünüzün bir parçası olarak dağıtmak ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz:

* [Github'da Azure IOT protokol ağ geçidi deposu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)

* [Azure IOT protokol ağ geçidi Geliştirici Kılavuzu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

IOT Hub dağıtımınızı planlama hakkında daha fazla bilgi için bkz:

* [Event Hubs ile karşılaştırma](iot-hub-compare-event-hubs.md)

* [Ölçeklendirme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma](iot-hub-scaling.md)

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)