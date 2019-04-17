---
title: Bulut (Node.js) - Azure IOT hub'a bağlanma Raspberry Pi web simülatörü Raspberry Pi'yi simülasyonu | Microsoft Docs
description: Raspberry Pi, Azure bulutuna veri göndermek için Azure IOT Hub ile Raspberry Pi web simülatörü bağlanın.
author: wesmc7777
manager: philmea
keywords: raspberry pi simülatör, azure IOT raspberry pi, raspberry pi IOT hub, bulut verilerini raspberry pi göndermek, buluta raspberry pi
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: wesmc
ms.openlocfilehash: 42c2c0d1a015baf4b846c86ed22e8383e21028b6
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59607578"
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Raspberry Pi çevrimiçi simülatör (Node.js) Azure IOT hub'a bağlama

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspberry Pi çevrimiçi simülatör ile çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra PI simülatör'ü kullanarak buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](about-iot-hub.md).

Fiziksel cihazlar varsa [Azure IOT hub'a bağlanma Raspberry Pi'yi](iot-hub-raspberry-pi-kit-node-get-started.md) kullanmaya başlamak için.

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3-banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6-button-default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5-button-click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6-button-default.png';">
</div>

## <a name="what-you-do"></a>Neler

* Raspberry Pi çevrimiçi simülatör ile ilgili temel bilgileri öğrenin.

* IOT hub oluşturun.

* Bir cihaz IOT hub'ına PI sayısı için kaydedin.

* IOT hub'ınıza sanal sensör verilerini göndermeyi Pi üzerinde bir örnek uygulamayı çalıştırın.

Benzetimli Raspberry Pi'yi oluşturduğunuz IOT hub'a bağlayın. Ardından simülatör ile algılayıcı verilerini oluşturmak için örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizesini almak nasıl. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.

* Raspberry Pi çevrimiçi simülatör ile çalışmayı öğrenin.

* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Raspberry Pi web simülatörü'ne genel bakış

Raspberry Pi çevrimiçi simülatör başlatmak için düğmeye tıklayın.

> [!div class="button"]
> <a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Raspberry Pi simülatörü başlatın</a>

Web benzetici üç alan vardır.

1. Derleme - varsayılan bağlantı hattının bir PI BME280 algılayıcı ve bir LED ile bağlanan alanıdır. Önizleme sürümünde alan kilitli özelleştirme bu nedenle şu anda bunu yapamazsınız.

2. Alan - bir çevrimiçi kod düzenleyicisine, kod ile Raspberry Pi kodlama. Varsayılan örnek uygulamayı BME280 algılayıcıdan algılayıcı verilerini toplamak için yardımcı olur ve Azure IOT Hub'ınıza gönderir. Uygulamayı gerçek PI cihazlarla tamamen uyumludur. 

3. Tümleşik bir konsol penceresi - kodunuzu çıktısını gösterir. Bu pencerenin en üstünde üç düğme bulunur.

   * **Çalıştırma** -kodlama alanında uygulamayı çalıştırın.

   * **Sıfırlama** -varsayılan örnek uygulamaya kodlama alan sıfırlayın.

   * **Katlama/genişletme** -sağ tarafta, konsol penceresinde Katlama/genişletmek bir düğme vardır.

> [!NOTE]
> Raspberry Pi web simülatörü artık Önizleme sürümünde kullanılabilir. Sesinizi de duymak istiyoruz [Gitter odası](https://gitter.im/Microsoft/raspberry-pi-web-simulator). Kaynak kodu public [GitHub](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Pi çevrimiçi simülatör'ne genel bakış](media/iot-hub-raspberry-pi-web-simulator/0-overview.png)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="run-a-sample-application-on-pi-web-simulator"></a>Pi web simülatörü hakkında bir örnek uygulamayı çalıştırma

1. Alan kodlama, varsayılan örnek uygulama üzerinde çalıştığından emin olun. Satır 15 yer tutucuyu, Azure IOT hub cihaz bağlantı dizesiyle değiştirin.
1. 
   ![Cihaz bağlantı dizesini değiştirin](media/iot-hub-raspberry-pi-web-simulator/1-connectionstring.png)

2. Seçin **çalıştırma** veya türü `npm start` uygulamayı çalıştırın.

Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir ![çıkış - IOT hub'ınıza Raspberry Pi'dan gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-web-simulator/2-run-application.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub'ınıza tarafından alınan iletileri okuma

Sanal CİHAZDAN IOT hub tarafından alınan iletileri izlemeye yönelik bir yolu, Visual Studio Code için Azure IOT araçları kullanmaktır. Daha fazla bilgi için bkz. [göndermek ve IOT Hub ve cihaz arasında iletileri almak Visual Studio Code için Azure IOT Araçları](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Cihazınız tarafından gönderilen verileri işlemek daha fazla yolu için açın sonraki bölüme devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
