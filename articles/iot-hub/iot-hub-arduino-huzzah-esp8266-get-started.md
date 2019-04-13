---
title: Buluta - ESP8266 Feather HUZZAH ESP8266 Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Kurulum ve Bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub için bunu Adafruit Feather HUZZAH ESP8266 bağlanma hakkında bilgi edinin.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: wesmc
ms.openlocfilehash: 293901aca3fa1a94c9c6340d2e04f47914db0e07
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59524472"
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Adafruit Feather HUZZAH ESP8266 bulutta Azure IOT hub'a bağlanma

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 Feather HUZZAH ESP8266 ve IOT hub'ı arasında bir bağlantı](./media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Neler

Adafruit Feather HUZZAH ESP8266 oluşturduğunuz IOT hub'a bağlayın. Ardından, ESP8266 DHT22 algılayıcıdan sıcaklık ve nem verileri toplamak için örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

> [!NOTE]
> Diğer ESP8266 panoları kullanıyorsanız, IOT hub'ınıza bağlanmak için aşağıdaki adımları yine de takip edebilirsiniz. Kullanmakta olduğunuz ESP8266 tablosu bağlı olarak, yeniden yapılandırmanız gerekebilir `LED_PIN`. Yapay ZEKA Thinker gelen ESP8266 kullanıyorsanız, örneğin, ondan değişebilir `0` için `2`. Bir paket henüz yok mu? Yararlanabileceğimi [Azure Web sitesi](https://azure.com/iotstarterkits).

## <a name="what-you-learn"></a>Öğrenecekleriniz

* IOT hub'ı oluşturma ve bir cihaz kaydetmek için Feather HUZZAH ESP8266
* Algılayıcı ve bilgisayarınızla Feather HUZZAH ESP8266 bağlanma
* Feather HUZZAH ESP8266 örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl
* IOT hub'ınıza sensör verilerini gönderme

## <a name="what-you-need"></a>Ne gerekiyor

![Öğretici için gerekli olan bölümleri](./media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi tamamlamak için aşağıdaki bölümleri Feather HUZZAH ESP8266 başlangıç Seti gerekir:

* Feather HUZZAH ESP8266 Panosu
* Bir mikro USB türü bir USB kablosu

Ayrıca geliştirme ortamınız için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir Mac veya Windows veya Ubuntu çalıştıran bir bilgisayar.
* Kablosuz ağa bağlanmak Feather HUZZAH ESP8266 için.
* Yapılandırma Aracı indirmek için Internet bağlantısı.
* [Arduino için Visual Studio Code uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino).

> [!Note]
> Arduino 1.6.8 olması için Visual Studio Code uzantısı tarafından kullanılan Arduino IDE sürümü veya üzeri. Önceki sürümlerde AzureIoT kitaplığı ile çalışmaz.

Algılayıcı yok durumunda aşağıdaki öğeler isteğe bağlıdır. Ayrıca, sanal sensör verilerini kullanma seçeneğiniz de vardır.

* Adafruit DHT22 sıcaklık ve nem algılayıcısı
* Bir breadboard
* M/dk anahtar kabloları

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Algılayıcı ve bilgisayarınızla Feather HUZZAH ESP8266 bağlanma

Bu bölümde, panonuza algılayıcıları bağlayın. Ardından bilgisayarınıza başka amaçlarla kullanmak için cihaz takın.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>DHT22 sıcaklık ve nem algılayıcı bağlanmak için Feather HUZZAH ESP8266

Şu şekilde bağlantı kurmak için breadboard ve anahtar kablo kullanın. Algılayıcı yoksa, bunun yerine sanal sensör verilerini kullanabileceğinizden bu bölümü atlayın.

![Bağlantı Başvurusu](./media/iot-hub-arduino-huzzah-esp8266-get-started/17_connections_on_breadboard.png)

Algılayıcı sabitlemek için aşağıdaki bağlantı kullanın:

| Başlangıç (algılayıcı)           | Bitiş (Pano)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------  |
| VDD (PIN 31F)            | 3v (sabitleme 58H)           | Kırmızı kablosu     |
| Veri (PIN 32F)           | GPIO 2 (Pin 46A)       | Mavi kablosu    |
| GND (PIN 34F)            | GND (PIN 56I)          | Siyah kablo   |

Daha fazla bilgi için [Adafruit DHT22 algılayıcı Kurulum](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) ve [Adafruit Feather HUZZAH Esp8266 no'lu](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).

Şimdi, Feather Huzzah ESP8266 çalışma algılayıcısı ile bağlanmalıdır.

![DHT22 Feather Huzzah ile bağlanma](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Feather HUZZAH ESP8266 bilgisayarınıza bağlayın

Sonraki gösterildiği gibi Feather HUZZAH ESP8266 bilgisayarınıza bağlanmak için mikro USB türü bir USB kablosu kullanın.

![Feather Huzzah bilgisayarınıza bağlayın](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seri bağlantı noktası izinleri (sadece Ubuntu) ekleme

Ubuntu kullanırsanız, USB bağlantı noktası, Feather HUZZAH ESP8266 üzerinde çalışmak için izinlere sahip olduğunuzdan emin olun. Seri bağlantı noktası izinleri eklemek için aşağıdaki adımları izleyin:

1. Bir terminalde aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktıları birini olursunuz:

   * 1 kök uucp xxxxxxxx crw-rw---
   * 1 kök araması xxxxxxxx crw-rw---

   Çıktıda dikkat `uucp` veya `dialout` USB bağlantı noktasına Grup sahibi adıdır.

2. Kullanıcı, aşağıdaki komutu çalıştırarak gruba ekleyin:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` Önceki adımda elde ettiğiniz Grup sahibi adıdır. `<username>` Ubuntu kullanıcı adınızdır.

3. Ubuntu dışında oturum açın ve yeniden değişikliğin görünmesi için oturum açın.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplamak ve IOT hub'ına gönderme

Bu bölümde, dağıtın ve Feather HUZZAH ESP8266 hakkında bir örnek uygulamayı çalıştırın. Örnek uygulamayı LED Feather HUZZAH ESP8266 üzerinde yanıp ve IOT hub'ınıza DHT22 algılayıcıdan toplanan sıcaklık ve nem verileri gönderir.

### <a name="get-the-sample-application-from-github"></a>Örnek uygulamayı Github'dan alma

Örnek uygulama, GitHub üzerinde barındırılır. Github'dan örnek uygulamayı içeren örnek depoyu kopyalayın. Örnek depoyu kopyalamak için şu adımları izleyin:

1. Bir komut istemi veya terminal penceresi açın.

2. Depolanması için örnek uygulamayı istediğiniz klasöre gidin.

3. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

   Ardından, Visual Studio code'da Feather HUZZAH ESP8266 için paketi yükleyin.

4. Örnek uygulamayı depolandığı klasörü açın.

5. Visual Studio code'da uygulama klasörü app.ino dosyasını açın.

   ![Örnek uygulamayı Visual Studio Code'da açın](media/iot-hub-arduino-huzzah-esp8266-get-started/10_vscode-open-sample-app.png)

6. Visual Studio Code'da girin `F1`.

7. Tür **Arduino** seçip **Arduino: Pano Yöneticisi'ni**.

8. İçinde **Arduino Panosu Manager** sekmesinde **ek URL'ler**.

   ![VS Code Arduino Panosu Yöneticisi](media/iot-hub-arduino-huzzah-esp8266-get-started/11_vscode-arduino-board-manager.png)

9. İçinde **kullanıcı ayarları** penceresinde, aşağıdaki dosya sonunda kopyalayıp

   ```json
   "arduino.additionalUrls": "http://arduino.esp8266.com/stable/package_esp8266com_index.json"
   ```

   ![VS Code'da Arduino paket URL'si yapılandırın](media/iot-hub-arduino-huzzah-esp8266-get-started/12_vscode-package-url.png)

10. Dosyayı kaydedin ve kapatın **kullanıcı ayarları** sekmesi.

11. Tıklayın **yenileme paket dizinleri**. Yenileme tamamlandıktan sonra arama **esp8266**.

12. Tıklayın **yükleme** esp8266 düğmesi.

    Panoları Manager ESP8266 2.2.0 veya sonraki bir sürümü ile yüklü olduğunu gösterir.

    ![Esp8266 paketin yüklü](media/iot-hub-arduino-huzzah-esp8266-get-started/13_vscode-esp8266-installed.png)

13. Girin `F1`, Anahtar'a **Arduino** seçip **Arduino: Pano Config**.

14. İçin kutusu **Seçilen Pano:** ve türü **esp8266**, ardından **Adafruit HUZZAH ESP8266 (esp8266)**.

    ![Esp8266 panosunu seçin](media/iot-hub-arduino-huzzah-esp8266-get-started/14_vscode-select-esp8266.png)

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükleme

1. Visual Studio Code'da girin `F1`, Anahtar'a **Arduino** seçip **Arduino: Kitaplık Yöneticisi**.

2. Aşağıdaki kitaplık arama tek tek adları. Bulduğunuz her Kitaplığı'nı **yükleme**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Gerçek bir DHT22 algılayıcı yok mu?

Örnek uygulama, gerçek DHT22 algılayıcı yok durumunda sıcaklık ve nem veri benzetimini yapabilirsiniz. Sanal veri kullanmak için örnek uygulamayı ayarlamak için aşağıdaki adımları izleyin:

1. Açık `config.h` dosyası `app` klasör.

2. Aşağıdaki kod satırını bulun ve değerini `false` için `true`:

   ```c
   define SIMULATED_DATA true
   ```

   ![Sanal veri kullanmak için örnek uygulamayı yapılandırma](media/iot-hub-arduino-huzzah-esp8266-get-started/15_vscode-configure-app-use-simulated-data.png)

3. Dosyayı kaydedin.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Feather HUZZAH ESP8266 örnek uygulamayı dağıtma

1. Visual Studio Code'da tıklayın  **\<seçin seri bağlantı noktası >** durumunu çubuk ve seri bağlantı noktası için Feather HUZZAH ESP8266'ye tıklayın.

2. Girin `F1`, Anahtar'a **Arduino** seçip **Arduino: Karşıya yükleme** oluşturup Feather HUZZAH ESP8266 için örnek uygulama dağıtırsınız.

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra kimlik bilgilerinizi girmek için aşağıdaki adımları izleyin:

1. Arduino IDE açın, **Araçları** > **seri İzleyici**.

2. Seri İzleyicisi penceresinde sağ alt köşedeki iki açılır liste dikkat edin.

3. Seçin **sondaysa satır** sol aşağı açılan listesi.

4. Seçin **115200 baud** sağda açılan listesi.

5. İstenirse bunları sağlayın ve ardından seri İzleyici penceresinin en üstünde yer alan giriş kutusuna aşağıdaki bilgileri girin. **Gönder**.

   * Wi-Fi SSID
   * Wi-Fi parolası
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi EEPROM, Feather HUZZAH ESP8266 içinde depolanır. Feather HUZZAH ESP8266 panosuna Sıfırla düğmesine tıklarsanız, örnek uygulama bilgileri silmek isteyip istemediğinizi sorar. Girin `Y` silinmesi bilgisi yok. İkinci kez bilgileri vermeniz istenir.

### <a name="verify-the-sample-application-is-running-successfully"></a>Örnek Uygulama başarıyla çalıştığını doğrulayın

Feather HUZZAH ESP8266 üzerinde aşağıdaki çıktıyı seri İzleme penceresi ve yanıp sönen LED görürseniz örnek uygulama başarıyla çalışıyor.

![Arduino IDE içindeki son çıkış](media/iot-hub-arduino-huzzah-esp8266-get-started/16_arduino-ide-final-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub'ınıza tarafından alınan iletileri okuma

Cihazınızdan IOT hub tarafından alınan iletileri izlemeye yönelik bir yolu, Visual Studio Code için Azure IOT araçları kullanmaktır. Daha fazla bilgi için bkz. [göndermek ve IOT Hub ve cihaz arasında iletileri almak Visual Studio Code için Azure IOT Araçları](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Cihazınız tarafından gönderilen verileri işlemek daha fazla yolu için açın sonraki bölüme devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla Feather HUZZAH ESP8266 IOT hub'ınıza bağlanan ve yakalanan sensör verilerini IOT hub'ına gönderilen.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]