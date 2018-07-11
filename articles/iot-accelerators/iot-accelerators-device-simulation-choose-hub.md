---
title: Cihaz benzetimi çözümüyle - Azure var olan IOT hub'ı kullanma | Microsoft Docs
description: Bu makalede, var olan IOT hub'ı kullanmak için cihaz benzetimi çözüm Hızlandırıcısını yapılandırma açıklanır.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: ee96173ca5f36dee0f08c38e4b6e29da6fee804e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967561"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Mevcut bir IOT hub ile cihaz benzetimi çözüm Hızlandırıcısını kullanın

Cihaz benzetimi çözüm Hızlandırıcısını sağladığınızda, benzetim kullanmak için kaynak grubu Çözüm Hızlandırıcısı'nın IOT hub'ı dağıtmayı tercih edebilirsiniz.

İsteğe bağlı IOT hub'ı dağıtmak seçmezseniz, çalıştırdığınız herhangi simülasyonlar için kendi hub'ı kullanmanız gerekir. İsteğe bağlı IOT hub'ı dağıtmayı tercih ederseniz, bu isteğe bağlı bir hub veya hub'ınıza kendi kullanmayı da tercih edebilirsiniz.

IOT hub'ı yoksa, her zaman yeni bir tane oluşturabilirsiniz [Azure portalında](https://portal.azure.com).

Önceden var olan bir IOT hub'ı kullanmak için bir bağlantı dizesi gerekir. **iothubowner** paylaşılan erişim ilkesi. Bu bağlantı dizesinden alabilirsiniz [Azure portalında](https://portal.azure.com):

1. Hub'ın yapılandırma sayfasında portalında **paylaşılan erişim ilkeleri**.
1. Tıklayın **iothubowner**.
1. Birincil veya ikincil bağlantı dizesini kopyalayın.

[![Bağlantı dizesini alma](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Benzetim yapılandırdığınızda kopyaladığınız bağlantı dizesini kullanın:

[![Benzetim yapılandırın](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-expanded.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda, bir benzetim var olan IOT hub'ı kullanmayı öğrendiniz. Ardından, edinmek isteyebilir nasıl [özel cihaz modelini yapılandırmak](iot-accelerators-device-simulation-custom-model.md) bir simülasyonu için.
