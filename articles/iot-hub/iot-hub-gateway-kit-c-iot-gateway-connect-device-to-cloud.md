---
title: "Bir aygıt Azure IOT Hub'ına bağlanmak için bir IOT ağ geçidi kullanın | Microsoft Docs"
description: "Intel NUC IOT ağ geçidi olarak tı SensorTag bağlanmak ve bulutta Azure IOT Hub'ına algılayıcı verileri göndermek için nasıl kullanılacağını öğrenin."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT ağ geçidi bulut aygıta bağlan"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 4fb77ed0241d15338c2574fd22828507c3e40cb3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a>IOT ağ geçidi bulut - Azure IOT Hub'ına SensorTag şeyler bağlamak için kullanın

> [!NOTE]
> Bu öğretici başlamadan önce tamamlandı emin olun [Intel NUC IOT ağ geçidi olarak ayarlama](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). İçinde [Intel NUC IOT ağ geçidi olarak ayarlamak](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), Intel NUC cihazı IOT ağ geçidi olarak ayarlayın.

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

Texas Instruments SensorTag (CC2650STK) Azure IOT Hub'ına bağlanmak için bir IOT ağ geçidi kullanmayı öğrenin. IOT ağ geçidi sıcaklık ve nem verileri toplanan SensorTag Azure IOT Hub'ına gönderir.

## <a name="what-you-will-do"></a>Ne yapacağını

- IOT hub'ı oluşturun.
- Bir cihaz IOT hub'ı SensorTag için kaydetme.
- IOT ağ geçidi ve SensorTag arasındaki bağlantıyı etkinleştirin.
- IOT hub'ınıza SensorTag veri göndermek için bırak örnek uygulamayı çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

- Öğretici [Intel NUC IOT ağ geçidi olarak ayarlama](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) tamamlandı, Intel NUC IOT ağ geçidi olarak ayarlamanız içinde.
- * Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
- Ana bilgisayar üzerinde çalışan bir SSH istemcisi. Putty'yi Windows önerilir. Linux ve macOS zaten bir SSH istemcisi ile gelir.
- IP adresi ve kullanıcı adı ve SSH istemciden ağ geçidi erişim için parola.
- Bir Internet bağlantısı.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Burada, SensorTag için bu yeni cihaz kaydedilir

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a>IOT ağ geçidi SensorTag arasındaki bağlantıyı etkinleştir

Bu bölümde, aşağıdaki görevleri gerçekleştirin:

- Bluetooth bağlantısı için SensorTag MAC adresini alın.
- SensorTag IOT ağ geçidi bir Bluetooth bağlantısını başlatma.

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a>Bluetooth bağlantısı için SensorTag MAC adresini alın

1. Ana bilgisayarda SSH istemcisi çalıştırın ve IOT ağ geçidi bağlanın.
1. Bluetooth engellemesini kaldırmak için aşağıdaki komutu çalıştırın:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. IOT ağ geçidi Bluetooth hizmetini başlatın ve aşağıdaki komutları çalıştırarak Bluetooth yapılandırmak için Bluetooth Kabuk girin:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Güç açma Bluetooth Kabuk aşağıdaki komutu çalıştırarak Bluetooth denetleyici:

   ```bash
   power on
   ```

   ![IOT ağ geçidi bluetoothctl ile Bluetooth denetleyicide güç](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Yakındaki Bluetooth cihazları için aşağıdaki komutu çalıştırarak Taramayı Başlat:

   ```bash
   scan on
   ```

   ![Bluetoothctl ile Bluetooth cihazları yakındaki tarama](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Üzerinde SensorTag eşleştirme düğmesine basın. Yeşil SensorTag yanıp sönen üzerinde GEREKTİRİYORDU.
1. Bluetooth Kabuk SensorTag bulunan görmeniz gerekir. SensorTag MAC adresini not edin. Bu örnekte, SensorTag MAC adresidir `24:71:89:C0:7F:82`.
1. Aşağıdaki komutu çalıştırarak taramayı devre dışı bırakın:

   ```bash
   scan off
   ```

   ![Bluetoothctl ile Bluetooth cihazları yakındaki Taramayı Durdur](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a>SensorTag IOT ağ geçidi bir Bluetooth bağlantısını başlatma

1. Aşağıdaki komutu çalıştırarak SensorTag Bağlan:

   ```bash
   connect <MAC address>
   ```

   ![SensorTag bluetoothctl ile bağlanma](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. SensorTag kesin ve aşağıdaki komutları çalıştırarak Bluetooth kabuktan çıkış:

   ```bash
   disconnect
   exit
   ```

   ![SensorTag bluetoothctl ile bağlantısını kesin](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

SensorTag ve IOT ağ geçidi arasındaki bağlantı başarıyla etkinleştirdikten.

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a>IOT hub'ınıza SensorTag veri göndermek için bırak örnek uygulamayı çalıştırın

Bluetooth düşük enerji (bırak) örnek uygulaması, Azure IOT kenar tarafından sağlanır. Örnek uygulama bırak bağlantısından veri toplar ve verileri, IOT hub'ına gönderin. Örnek uygulamayı çalıştırmak için aktarmanız gerekir:

1. Örnek uygulamayı yapılandırın.
1. Örnek uygulamayı IOT ağ geçidi üzerinde çalıştırın.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Aşağıdaki komutu çalıştırarak örnek uygulamasının klasöre gidin:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Aşağıdaki komutu çalıştırarak yapılandırma dosyasını açın:

   ```bash
   vi ble_gateway.json
   ```

1. Yapılandırma dosyasında aşağıdaki değerleri doldurun:

   **IoTHubName**: IOT hub'ın adı.

   **IoTHubSuffix**: Get IoTHubSuffix anahtarından aşağı not ettiğiniz birincil aygıt bağlantı dizesi. Birincil anahtar, cihaz bağlantı dizesi, birincil anahtarı değil, IOT hub bağlantı dizesine elde etmenizi sağlar. Birincil aygıt bağlantı dizesinin biçiminde anahtarıdır `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Aktarım**: varsayılan değer `amqp`. Bu değer protokolü sırasında transpotation gösterir. Olabilir `http`, `amqp`, veya `mqtt`.

   **macAddress**: aşağı ettiğiniz SensorTag MAC adresidir.

   **DeviceID**: IOT hub'ınıza oluşturduğunuz cihaz kimliği.

   **deviceKey**: birincil anahtar, cihaz bağlantı dizesi.

   ![Yapılandırma dosyası bırak örnek uygulamasının tamamlayın](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Tuşuna `ESC` ve türü `:wq` dosyayı kaydetmek için.

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

1. SensorTag açık olduğundan emin olun.
1. Şu komutu çalıştırın:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure IOT kenarıyla algılayıcı veri dönüştürme için IOT ağ geçidi kullan](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
