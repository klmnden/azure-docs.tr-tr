---
title: Cihaz benzetimi çözümüyle - Azure var olan IOT hub'ı kullanma | Microsoft Docs
description: Bu makalede, var olan IOT hub'ı kullanmak için cihaz benzetimi çözüm Hızlandırıcısını yapılandırma açıklanır.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 38cde750ce07741a433baa1b8607a584f94ad9b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62123667"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Mevcut bir IOT hub ile cihaz benzetimi çözüm Hızlandırıcısını kullanın

Cihaz benzetimini dağıtma, benzetim kullanmak için bir IOT hub'ı dağıtmayı seçebilirsiniz. Bu seçenek dağıtan bir [S2 katmanı IOT hub ile tek bir ölçek birimi](../iot-hub/iot-hub-scaling.md). Bu isteğe bağlı IOT hub'ı dağıtırsanız, yine de bir simülasyon çalıştırma için başka bir IOT Hub'ı hedefleyecek şekilde seçebilirsiniz.

İsteğe bağlı IOT hub'ı dağıtma kullanmayı tercih ederseniz, çalıştırdığınız herhangi simülasyonlar için kendi hub'ı kullanmanız gerekir.

IOT hub'ı yoksa, her zaman yeni bir tane oluşturabilirsiniz [Azure portalında](https://portal.azure.com).

Önceden var olan bir IOT hub'ı kullanmak için bağlantı dizesi gerekir. **iothubowner** paylaşılan erişim ilkesi. Bu bağlantı dizesinden alabilirsiniz [Azure portalında](https://portal.azure.com):

1. Hub'ın yapılandırma sayfasında portalında **paylaşılan erişim ilkeleri**.

1. Tıklayın **iothubowner**.

1. Birincil veya ikincil bağlantı dizesini kopyalayın.

[![Bağlantı dizesini alma](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Benzetim yapılandırdığınızda kopyaladığınız bağlantı dizesini kullanın:

![Benzetim yapılandırın](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation.png)

### <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda, bir benzetim var olan IOT hub'ı kullanmayı öğrendiniz. Ardından, edinmek isteyebilir nasıl [Gelişmiş cihaz modeli oluşturma](iot-accelerators-device-simulation-advanced-device.md) bir simülasyonu için.
