---
title: Buluta (Node.js) - Azure IOT Hub'ına bağlanmak Raspberry Pi'yi web simulator Raspberry Pi'yi benzetimli | Microsoft Docs
description: Azure bulut veri göndermek Raspberry Pi'yi için Azure IOT Hub ile Raspberry Pi'yi web simulator bağlayın.
author: rangv
manager: ''
keywords: Böğürtlenli pi simulator azure IOT Böğürtlenli pi, Böğürtlenli PI IOT hub, bulut için Böğürtlenli pi gönderme, verileri buluta Böğürtlenli pi
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: dc8216f6b0a610b556b94637970bfd3e721f38d2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636231"
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Azure IOT Hub'ına (Node.js) Raspberry Pi'yi çevrimiçi simulator Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspberry Pi'yi çevrimiçi simulator ile çalışmanın temelleri öğrenerek başlayın. Daha sonra sorunsuzca Pi simulator buluta kullanarak bağlanmasına nasıl öğrenin [Azure IOT Hub](iot-hub-what-is-iot-hub.md). 

Fiziksel cihazlarınız varsa, ziyaret [Azure IOT Hub'ına bağlanmak Raspberry Pi'yi](iot-hub-raspberry-pi-kit-node-get-started.md) başlamak için. 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a>Neler

* Raspberry Pi'yi çevrimiçi simulator temellerini öğrenin.
* IOT hub'ı oluşturun.
* Bir cihaz IOT hub'ınıza Pi için kaydetme.
* Pi benzetimli algılayıcı verilerini IOT hub'ınıza gönderilecek örnek bir uygulamayı çalıştırın.

Benzetimli Raspberry Pi'yi, oluşturduğunuz bir IOT hub'ına bağlanın. Ardından Algılayıcı verileri oluşturmak için bir benzetici bir örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza algılayıcı verileri gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizenizi elde etme. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Raspberry Pi'yi çevrimiçi simulator ile çalışmaya nasıl.
* IOT hub'ınıza algılayıcı verileri göndermek nasıl.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Raspberry Pi'yi web simulator genel bakış

Raspberry Pi'yi çevrimiçi simulator başlatmak için düğmesini tıklatın.

> [!div class="button"]
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Böğürtlenli Pi Simulator Başlat</a>

Web benzeticisinde üç alan vardır.
1. Derleme - varsayılan hattı bir Pi BME280 algılayıcı ve bir LED bağlayan alanıdır. Alan Önizleme sürümünde kilitli özelleştirme bu nedenle şu anda yapamazsınız.
2. Alanı - Raspberry Pi'yi koduyla sizin için bir çevrimiçi kod düzenleyicisine kodlama. Varsayılan örnek uygulama BME280 algılayıcı algılayıcı verilerini toplamak için yardımcı olur ve Azure IOT Hub'ınıza gönderir. Uygulamayı gerçek Pi cihazları ile tam uyumludur. 
3. Tümleşik konsol penceresi - kodunuzu çıktısını gösterir. Bu penceresinin en üstünde üç düğme bulunur.
   * **Çalıştırma** -kodlama alanında uygulamayı çalıştırın.
   * **Sıfırlama** -varsayılan örnek uygulamaya kodlama alanı sıfırlayın.
   * **Katlama/genişletme** -sağ tarafında, konsol penceresi Katlama/genişletmek bir düğme vardır.

> [!NOTE] 
Raspberry Pi'yi web simulator Önizleme sürümünde kullanıma sunulmuştur. Sesiniz içinde duymak isteriz [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). Kaynak kodu üzerinde ortak [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Pi çevrimiçi simulator genel bakış](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>Örnek bir uygulama Pi web simulator'da çalıştırma

1. Alan kodlamasında, varsayılan örnek uygulama üzerinde çalıştığından emin olun. Satır 15 yer tutucunun Azure IOT hub cihaz bağlantı dizesi ile değiştirin.
   ![Cihaz bağlantı dizesini değiştirin](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Tıklatın **çalıştırmak** veya türü `npm start` uygulamayı çalıştırın.


Algılayıcı verilerini ve IOT hub'ına gönderilen iletileri gösterir aşağıdaki çıktı görmeniz gerekir ![çıktısı - Raspberry Pi'yi IOT hub'ına gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ınıza göndermek için örnek bir uygulamayı çalıştırdıktan.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
