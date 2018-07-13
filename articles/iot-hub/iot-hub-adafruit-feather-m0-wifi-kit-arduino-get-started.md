---
title: Buluta--M0 Feather M0 WiFi Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Ayarlama ve Azure IOT Hub'ı Bu öğreticide Azure bulut platformuna veri göndermesini Adafruit Feather M0 WiFi bağlanma hakkında bilgi edinin.
author: rangv
manager: nasing
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 2a6899bbd294a16dee3a767e976a92deaa00f0e2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38676701"
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a>Adafruit Feather M0 WiFi bulutta Azure IOT hub'a bağlanma
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![BME280, Feather M0 WiFi ve IOT hub'ı arasında bir bağlantı](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

Bu öğreticide, Arduino panosu ile çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra kullanarak cihazlarınızı buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>Neler

Adafruit Feather M0 WiFi oluşturduğunuz IOT hub'a bağlayın. Ardından bir BME280 sıcaklık ve nem verileri toplamak üzere M0 WiFi üzerinde bir örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.


## <a name="what-you-learn"></a>Öğrenecekleriniz

* IOT hub'ı oluşturma ve Feather M0 WiFi için bir cihazı kaydedin
* Algılayıcı ve bilgisayarınızla Feather M0 WiFi bağlanma
* Feather M0 WiFi örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl
* IOT hub'ınıza sensör verilerini gönderme

## <a name="what-you-need"></a>Ne gerekiyor

![Öğretici için gerekli olan bölümleri](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi tamamlamak için aşağıdaki bölümleri Feather M0 WiFi başlangıç Seti'nden gerekir:

* Feather M0 WiFi Panosu
* Bir mikro USB türü bir USB kablosu

Ayrıca geliştirme ortamınız için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir Mac veya Windows veya Ubuntu çalıştıran bir bilgisayar.
* Kablosuz ağa bağlanmak Feather M0 WiFi için.
* Yapılandırma Aracı indirmek için Internet bağlantısı.
* [Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 sürümü veya üzeri. Önceki sürümleri Azure IOT hub'ı kitaplığı ile çalışmaz.

Bir algılayıcı yoksa, aşağıdaki öğeler isteğe bağlıdır. Ayrıca, sanal sensör verilerini kullanma seçeneğiniz de vardır:

* BME280 sıcaklık ve nem algılayıcısı
* Bir breadboard
* M/dk anahtar kabloları

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a>Algılayıcı ve bilgisayarınızla Feather M0 WiFi bağlanma
Bu bölümde, panonuza algılayıcıları bağlayın. Ardından bilgisayarınıza başka amaçlarla kullanmak için cihaz takın.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a>Feather M0 WiFi DHT22 sıcaklık ve nem algılayıcı bağlanma

Bağlantı kurmak için breadboard ve anahtar kablo kullanın. Algılayıcı yoksa, bunun yerine sanal sensör verilerini kullanabileceğinizden bu bölümü atlayın.

![Bağlantı Başvurusu](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Algılayıcı sabitlemek için aşağıdaki bağlantı kullanın:


| Başlangıç (algılayıcı)           | Bitiş (Pano)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 27A)            | 3v (PIN 3A)            | Kırmızı kablosu     |
| GND (PIN 29A)            | GND (PIN 6A)           | Siyah kablo   |
| SCK (PIN 30A)            | SCK (PIN 12A)          | Sarı kablosu  |
| SDO (PIN 31A)            | MI (PIN 14A)           | Beyaz kablosu   |
| SDI (PIN 32A)            | M0 (PIN 13A)           | Mavi kablosu    |
| CS (PIN 33A)             | Bir GPIO'yu 5 (PIN 15J)       | Turuncu kablosu  |

Daha fazla bilgi için [Adafruit BME280 nem + Barometric baskısı sıcaklık algılayıcısı Kırılımı](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) ve [Adafruit Feather M0 WiFi no'lu](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Şimdi, Feather M0 WiFi çalışma algılayıcısı ile bağlanmalıdır.

![DHT22 Feather Huzzah ile bağlanma](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a>Feather M0 WiFi bilgisayarınıza bağlayın

Türü bir USB kablosu Micro USB Feather M0 WiFi bilgisayarınıza bağlanmak için gösterildiği gibi kullanın:

![Feather Huzzah bilgisayarınıza bağlayın](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seri bağlantı noktası izinleri (sadece Ubuntu) ekleme

Ubuntu kullanırsanız, USB bağlantı noktası, Feather M0 WiFi üzerinde çalışmak için izinlere sahip olduğunuzdan emin olun. Seri bağlantı noktası izinleri eklemek için aşağıdaki adımları izleyin:


1. Bir terminalde aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktıları birini olursunuz:

   * 1 kök uucp xxxxxxxx crw-rw---
   * 1 kök araması xxxxxxxx crw-rw---

   Çıktıda dikkat `uucp` veya `dialout` USB bağlantı noktasına Grup sahibi adıdır.

2. Gruba kullanıcı eklemek için aşağıdaki komutu çalıştırın:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   Grup sahibi adı önceki adımda elde ettiğiniz `<group-owner-name>`. Ubuntu kullanıcı adınız `<username>`.

3. Değişikliğin görünmesi Ubuntu dışında oturumu kapatıp tekrar açın.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplamak ve IOT hub'ına gönderme

Bu bölümde, dağıtın ve Feather M0 WiFi üzerinde bir örnek uygulamayı çalıştırın. Örnek uygulamayı LED yanıp sönme Feather M0 WiFi üzerinde yapar. Ardından, IOT hub'ınıza BME280 algılayıcıdan toplanan sıcaklık ve nem verileri gönderir.

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a>Arduino IDE hazırlamak ve örnek uygulamayı Github'dan alma

Örnek uygulama, GitHub üzerinde barındırılır. Github'dan örnek uygulamayı içeren örnek depoyu kopyalayın. Örnek depoyu kopyalamak için şu adımları izleyin:

1. Bir komut istemi veya terminal penceresi açın.

2. Depolanması için örnek uygulamayı istediğiniz klasöre gidin.
3. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a>Arduino IDE Feather M0 WiFi için paketi yükleyin

1. Örnek uygulamayı depolandığı klasörü açın.

2. Arduino IDE uygulama klasöründe app.ino dosyasını açın.

   ![Arduino IDE içinde örnek uygulama açın](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Tıklayın **dosya** > **tercihleri** (Windows/Linux) veya **Arduino** > **tercihleri** (Mac) kopyalayın ve Aşağıdaki bağlantıyı içine yapıştırın **ek panoları yöneticisi URL'leri** Arduino IDE Tercihler seçeneği.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. Tıklayın **Araçları** > **Panosu** > **panoları Manager**ve yüklemeyi `Arduino SAMD Boards` sürüm `1.6.2` veya üzeri. 

1. Aynı pencerede yüklemeyi `Adafruit SAMD Boards` Pano dosya tanımları eklemek için paket.

   ![Esp8266 paketin yüklü](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Tıklayın **Araçları** > **Panosu** > **Adafruit M0 WiFi**.

5. Sürücüleri (yalnızca Windows için) yükleyin. Feather M0 WiFi içinde bağladığınızda, bir sürücüyü yüklemek gerekebilir. Tıklayın [sayfasındaki indirme bağlantısı](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) sürücü yükleyiciyi indirmek için. İstediğiniz sürücüleri yüklemek için adımları izleyin.

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükleme

1. Arduino IDE içinde tıklayın **taslak** > **kitaplığı dahil** > **kitaplıklarını yönetme**.

2. Aşağıdaki kitaplık arama tek tek adları. Bulduğunuz her Kitaplığı'nı **yükleme**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. El ile yükleme `Adafruit_WINC1500`. Git [bu Web sitesi](https://github.com/adafruit/Adafruit_WINC1500) tıklatıp **Kopyala veya indir** > **ZIP'i indir**. Arduino IDE'nizi geçin **taslak** > **kitaplığı dahil** > **.zip kitaplığı ekleme** ve zip dosyası ekleyin.

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>Gerçek bir BME280 algılayıcı yoksa, örnek uygulama kullanma

Örnek uygulama, gerçek bir BME280 algılayıcı yoksa, sıcaklık ve nem veri benzetimini yapabilirsiniz. Sanal veri kullanmak için örnek uygulamayı ayarlamak için aşağıdaki adımları izleyin:

1. Açık `config.h` dosyası `app` klasör.

2. Aşağıdaki kod satırını bulun ve değerini `false` için `true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Sanal veri kullanmak için örnek uygulamayı yapılandırma](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Dosyayı kaydetmek `Control-s`.

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a>Feather M0 WiFi örnek uygulamayı dağıtma

1. Arduino IDE içinde tıklayın **aracı** > **bağlantı noktası**ve ardından seri bağlantı noktası Feather M0 WiFi için tıklayın.

2. Tıklayın **taslak** > **karşıya** oluşturup Feather M0 WiFi için örnek uygulama dağıtırsınız.

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra kimlik bilgilerinizi girmek için aşağıdaki adımları izleyin:

1. Arduino IDE içinde tıklayın **Araçları** > **seri İzleyici**.

2. Seri İzleyici penceresinin sağ alt köşesinde seçin **sondaysa satır** soldaki aşağı açılan listesinde.
3. Seçin **115200 baud** sağdaki aşağı açılan listesinde.
4. Bunu sağlamak ve istenirse üst giriş kutusuna aşağıdaki bilgileri girin. **Gönder**:

   * Wi-Fi SSID
   * Wi-Fi parolası
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi EEPROM, Feather M0 WiFi içinde depolanır. Feather M0 WiFi panosuna Sıfırla düğmesine tıklarsanız, örnek uygulama bilgileri silmek isteyip istemediğinizi sorar. Girin `Y` bilgileri silmek için. İkinci kez bilgileri sağlamanız istenir.

### <a name="verify-that-the-sample-application-is-running-successfully"></a>Örnek Uygulama başarıyla çalıştığını doğrulayın

Feather M0 WiFi üzerinde aşağıdaki çıktıyı seri İzleme penceresi ve yanıp sönen LED görürseniz örnek uygulama başarıyla çalışıyor:

![Arduino IDE içindeki son çıkış](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla Feather M0 WiFi IOT hub'ınıza bağlanan ve yakalanan sensör verilerini IOT hub'ına gönderilir. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

