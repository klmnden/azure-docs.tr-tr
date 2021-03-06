---
title: Azure endüstriyel IOT genel bakış | Microsoft Docs
description: Endüstriyel IOT genel bakış
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 3c3a54d469d3dcbe04c11aa049906b551d68022f
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606194"
---
# <a name="what-is-industrial-iot-iiot"></a>Endüstriyel IOT (Iıot) nedir

Iıot endüstriyel nesnelerin interneti ' dir. Iıot aracılığıyla IOT uygulamayı üretim sektöründe endüstriyel verimliliği artırır. 

## <a name="improve-industrial-efficiencies"></a>Endüstriyel verimliliği artırın

Bağlı Fabrika çözüm Hızlandırıcı ile operasyonel verimliliğinizi ve Karlılığınızı artırın. Fabrika katında çalışmakta olan makineleriniz dahil olmak üzere endüstriyel ekipmanlarınızı ve cihazlarınızı bulutta bağlayıp izleyin. IoT verilerinizi analiz ederek tüm fabrika katının performansını artırmanıza yardımcı olacak içgörüler edinin.

Zaman alan bir işlem Fabrika katı OPC İkizi makinelerle erişme azaltmak ve zamanınızı Iıot çözümler oluşturmaya odaklanın. Sertifika yönetimi ve OPC Vault tümleştirmesiyle endüstriyel varlık kolaylaştırın ve varlık bağlantı sağlandığını kalacağından. Bir REST benzeri API üstünde Bu mikro hizmetler sağlamak [Azure endüstriyel IOT bileşenleri](https://github.com/Azure/azure-iiot-opc-ua). Hizmet API'si size edge modül işlevlerin denetimi. 

![Endüstriyel IOT genel bakış](media/overview-iot-industrial/overview.png)

> [!NOTE]
> Azure endüstriyel IOT hizmetleri hakkında daha fazla bilgi için bkz. GitHub [depo](https://github.com/Azure/azure-iiot-services).
Azure IOT Edge modüllerinin nasıl çalıştığı ile bilmiyorsanız, aşağıdaki makaleleri ile başlayın:
- [Azure IoT Edge hakkında](../iot-edge/about-iot-edge.md)
- [Azure IOT Edge modülleri](../iot-edge/iot-edge-modules.md)

## <a name="connected-factory"></a>Bağlı fabrika

[Bağlı Fabrika](../iot-accelerators/iot-accelerators-connected-factory-features.md) özel iş gereksinimlerinizi karşılayacak şekilde özelleştirilebilir Microsoft'un Azure endüstriyel IOT başvuru mimarisi uygulamasıdır. Tam çözüm kod, açık kaynaklı ve bağlı Fabrika çözüm Hızlandırıcı GitHub deposuna kullanılabilir. Ticari bir ürün için başlangıç noktası olarak kullanabilir ve Azure aboneliğinize dakikalar içinde önceden oluşturulmuş bir çözüm dağıtabilirsiniz. 

## <a name="factory-floor-connectivity"></a>Fabrika katı bağlantısı

OPC İkizi cihaz bulma ve kayıt otomatikleştirir ve REST API'ler aracılığıyla endüstriyel cihazlara uzaktan denetimini sunar Iıot bileşendir. OPC İkizi, Azure IOT Edge ve IOT hub'ı bulutta ve Fabrika ağ bağlanmak için kullanır. OPC İkizi Iıot geliştiricilerin güvenli bir şekilde şirket içi makinelerin erişme hakkında endişelenmeden Iıot uygulamaları derlemeye odaklanmasını sağlar.

## <a name="security"></a>Güvenlik

OPC kasa, OPC UA genel bulma sunucusu (yapılandırma, kaydetme ve sertifika yaşam döngüsü OPC UA sunucusu ve istemci uygulamalarını bulutta yönetme GDS) uygulamasıdır. OPC kasa uygulaması ve endüstriyel alanında güvenli varlık bağlantı bakımını kolaylaştırır. Sertifika yönetimi otomatik hale getirerek OPC kasa bağlantısı ve sertifika yönetimi ile ilişkilendirilmiş el ile ve karmaşık işlemlerden Fabrika işleçleri serbest bırakır.

## <a name="next-steps"></a>Sonraki adımlar

Endüstriyel IOT ve bileşenlerine giriş vardı, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC İkizi nedir](overview-opc-twin.md)
