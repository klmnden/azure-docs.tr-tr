---
title: "Azure IOT Hub'ına bağlanan fiziksel cihazların kullanmaya başlama | Microsoft Docs"
description: "Fiziksel cihazlar ve panoları Azure IOT Hub'ına nasıl bağlayacağınızı öğrenin. Cihazlarınızı IOT hub'ı ve IOT hub'ı telemetri izlemek ve cihazlarınızı yönetin gönderebilirsiniz."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "Azure IOT hub Öğreticisi"
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Azure IOT Hub, fiziksel cihazların öğreticileri ile Başlarken

Bu öğreticiler Azure IOT Hub ve cihaz SDK'ları tanıtır. Öğreticiler IOT hub'ı özelliklerini göstermek için yaygın IOT senaryolarını ele. Öğreticiler ayrıca diğer Azure Hizmetleri ve daha güçlü IOT çözümleri oluşturmak için Araçlar IOT hub'ı birleştirmek nasıl gösterilmektedir. Aşağıdaki tabloda listelenen öğreticiler fiziksel IOT cihazları oluşturulacağını gösterir.

| IOT cihaz                       | Programlama dili |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IOT DevKit                      | [VSCode Arduino][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Adafruit yumuşatma HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 şey geliştirme      | [Arduino][Th_Ard]              |
| Adafruit yumuşatma M0             | [Arduino][M0_Ard]              |

Ayrıca, IOT hub'ınıza bağlanmak cihazları etkinleştirmek için bir IOT sınır ağ geçidi kullanabilirsiniz.

| Ağ geçidi aygıtı               | Programlama dili | Platform         |
|------------------------------|----------------------|------------------|
| Intel NUC (modeli DE3815TYKE) | C                    | [Rüzgar Akarsu Linux][NUC_Lnx] |

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
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
