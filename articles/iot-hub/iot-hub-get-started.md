---
title: Azure IOT Hub - IOT cihazları buluta bağlamak Başlarken | Microsoft Docs
description: IOT panoları ve başlangıç paketleri Azure IOT Hub'ına bağlanmak öğrenin. Cihazlarınızı IOT hub'ı ve IOT hub'ı telemetri izlemek ve cihazlarınızı yönetin gönderebilirsiniz.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
keywords: Azure IOT hub Öğreticisi
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: dafb8aca34a5a41f45f76d526aa3b8f3b1b792c4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Azure IOT Hub ile gerçek cihazları Başlarken

Nesnelerin interneti (IOT) çözümleri oluşturmak için Azure IOT Hub ve Azure IOT cihaz SDK'ları kullanabilirsiniz:

* Azure IOT Hub, güvenli bir şekilde bağlanır, izler ve IOT cihazları yöneten bulut tam olarak yönetilen bir hizmettir. IOT cihazlarınızı uygulamak için Azure IOT cihaz SDK'ları kullanın.
* Bir IOT ağ geçidi daha karmaşık IOT senaryolarda kullanın. Örneğin, burada, eski aygıtlar, bant genişliği maliyetlerini, güvenlik ve gizlilik ilkeleri veya edge veri işleme gibi etkenleri göz önünde bulundurmak gerekir. Bu senaryolarda kullanmak [Azure IOT kenar](https://docs.microsoft.com/azure/iot-edge/) cihazlarını IOT hub'ınızı bağlayan bir ağ geçidi uygulamak için.

## <a name="what-the-how-to-articles-cover"></a>Nasıl yapılır makaleleri ne kapsar

Bu makaleler Azure IOT Hub ve cihaz SDK'ları tanıtır. Makaleleri IOT hub'ı özelliklerini göstermek için yaygın IOT senaryolarını ele. Makaleler de diğer Azure Hizmetleri ve daha güçlü IOT çözümleri oluşturmak için Araçlar IOT hub'ı birleştirmek nasıl gösterilmektedir. Makalelerde, gerçek IOT cihazları kullanın.

## <a name="set-up-your-device"></a>Cihazınızı kurma

Bir IOT cihaz veya ağ geçidi Azure IOT Hub'ına Bağla:

| IOT cihaz                       | Programlama dili |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IOT DevKit                       | [VSCode Arduino][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit yumuşatma HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 şey geliştirme       | [Arduino][Th_Ard]              |
| Adafruit yumuşatma M0              | [Arduino][M0_Ard]              |
| Çevrimiçi aygıt benzeticisi         | [Böğürtlenli Pi (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
