---
title: "Bulut M0: yumuşatma M0 WiFi Azure IOT Hub'ına bağlanmak | Microsoft Docs"
description: Ayarlama ve Bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub Adafruit yumuşatma M0 WiFi bağlanmak öğrenin.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: ''
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: 2a6a65a3c4a69a49788ce9799ceed53d53edcd77
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a>Bulutta Azure IOT Hub'ına Adafruit yumuşatma M0 WiFi Bağlan
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![BME280, yumuşatma M0 WiFi ve IOT hub'ı arasında bir bağlantı](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

Bu öğreticide, Arduino panonuzu ile çalışmanın temelleri öğrenerek başlayın. Daha sonra sorunsuzca aygıtlarınızı buluta kullanarak bağlanmasına nasıl öğrenin [Azure IOT Hub](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>Neler

Adafruit yumuşatma M0 WiFi oluşturduğunuz bir IOT hub'ına bağlanın. Sonra M0 BME280 sıcaklık ve nem veri toplamak üzere WiFi üzerinde bir örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza algılayıcı verileri gönderin.


## <a name="what-you-learn"></a>Öğrenecekleriniz

* IOT hub'ı oluşturma ve yumuşatma M0 WiFi için bir cihaz kaydetme
* Algılayıcı ve bilgisayarınızla yumuşatma M0 WiFi bağlanma
* Yumuşatma M0 WiFi üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl
* IOT hub'ınıza algılayıcı verileri gönderme

## <a name="what-you-need"></a>Ne gerekiyor

![Öğretici için gerekli bölümleri](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi tamamlamak için aşağıdaki bölümleri yumuşatma M0 WiFi Starter Seti'nden gerekir:

* Yumuşatma M0 WiFi Panosu
* Mikro USB tipi A USB kablosu

Ayrıca, geliştirme ortamınız için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Mac veya Windows veya Ubuntu çalıştıran bir bilgisayar.
* Kablosuz ağ yumuşatma M0 WiFi bağlanmak için.
* Yapılandırma Aracı indirmek için Internet bağlantısı.
* [Arduino IDE](https://www.arduino.cc/en/main/software) sürüm 1.6.8 veya sonraki bir sürümü. Önceki sürümleri Azure IOT Hub kitaplığı ile çalışmaz.

Algılayıcı yoksa, aşağıdaki öğeler isteğe bağlıdır. Ayrıca sanal algılayıcı verilerini kullanma seçeneğiniz vardır:

* BME280 sıcaklık ve nem algılayıcı
* Bir breadboard
* M/M anahtar kabloları

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a>Algılayıcı ve bilgisayarınızla yumuşatma M0 WiFi Bağlan
Bu bölümde, algılayıcılar panonuz için bağlayın. Daha sonra Cihazınızı başka kullanmak için bilgisayarınıza takın.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a>Yumuşatma M0 WiFi DHT22 sıcaklık ve nem algılayıcı Bağlan

Bağlantıyı kurmak için breadboard ve anahtar kablolarını kullanır. Algılayıcı yoksa, benzetimli algılayıcı verilerini yerine kullandığından bu bölümü atlayabilirsiniz.

![Bağlantı Başvurusu](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Algılayıcı PIN'ler için aşağıdaki kablolama kullanın:


| Başlangıç (algılayıcı)           | Bitiş (kartı)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 27A)            | 3v (PIN 3A)            | Kırmızı kablosu     |
| GND (PIN 29A)            | GND (PIN 6A)           | Siyah kablosu   |
| SCK (PIN 30A)            | SCK (PIN 12A)          | Sarı kablosu  |
| SDO (PIN 31A)            | MI (PIN 14A)           | Beyaz kablosu   |
| SDI (PIN 32A)            | M0 (PIN 13A)           | Mavi kablosu    |
| CS (PIN 33A)             | GPIO'yu 5 (PIN 15J)       | Turuncu kablosu  |

Daha fazla bilgi için bkz: [Adafruit BME280 nem + Barometric baskısı sıcaklık algılayıcısı oturumdan](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) ve [Adafruit yumuşatma M0 WiFi no'lu](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Şimdi yumuşatma M0 WiFi ile çalışma algılayıcı bağlı olması gerekir.

![Yumuşatma Huzzah ile DHT22 Bağlan](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a>Yumuşatma M0 WiFi bilgisayarınıza bağlayın

Gösterildiği gibi bilgisayarınıza yumuşatma M0 WiFi bağlanmak için mikro USB tipi A USB kablosu kullanın:

![Yumuşatma Huzzah bilgisayarınıza bağlayın](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seri bağlantı noktası izinleri (yalnızca Ubuntu) ekleyin

Ubuntu kullanırsanız, USB bağlantı noktası, yumuşatma M0 WiFi üzerinde çalışması için izinlere sahip olduğunuzdan emin olun. Seri bağlantı noktası izinleri eklemek için aşağıdaki adımları izleyin:


1. Bir terminal aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktıları birini alın:

   * crw-rw---1 kök uucp xxxxxxxx
   * crw-rw---1 kök araması xxxxxxxx

   Çıktıda dikkat `uucp` veya `dialout` USB bağlantı noktasına Grup sahibi adıdır.

2. Gruba kullanıcı eklemek için aşağıdaki komutu çalıştırın:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   Önceki adımda aldığınız Grup sahibi adı `<group-owner-name>`. Ubuntu kullanıcı adınız `<username>`.

3. Değişiklik görünür, Ubuntu dışında oturum açın ve yeniden oturum açın.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplamak ve IOT hub'ınıza gönderin

Bu bölümde, dağıtın ve yumuşatma M0 WiFi üzerinde bir örnek uygulamayı çalıştırın. Örnek uygulama LED blink yumuşatma M0 WiFi üzerinde yapar. Ardından, IOT hub'ınıza BME280 algılayıcı toplanan sıcaklık ve nem verileri gönderir.

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a>Örnek uygulama Github'dan alma ve Arduino IDE hazırlama

Örnek uygulama, GitHub üzerinde barındırılır. Github'dan örnek uygulamayı içeren örnek depoyu kopyalayın. Örnek deposuna kopyalamak için aşağıdaki adımları izleyin:

1. Bir komut istemi veya terminal penceresi açın.

2. Depolanması için örnek uygulama istediğiniz bir klasöre gidin.
3. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a>Yumuşatma M0 WiFi Arduino IDE'de paketi yükle

1. Örnek uygulama depolandığı klasörü açın.

2. Arduino IDE uygulama klasöründe app.ino dosyasını açın.

   ![Örnek uygulamayı Arduino IDE içinde Aç](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Tıklatın **dosya** > **Tercihler** (Windows/Linux) veya **Arduino** > **Tercihler** (Mac) kopyalayıp ve Aşağıdaki bağlantıda içine yapıştırma **ek panoları yöneticisi URL'leri** Arduino IDE tercihlerinde seçeneği.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. ' I tıklatın **Araçları** > **Panosu** > **panoları Yöneticisi**ve ardından yükleyin `Arduino SAMD Boards` sürüm `1.6.2` veya sonraki bir sürümü. 

1. Aynı pencerede yüklemek `Adafruit SAMD Boards` Panosu dosya tanımları eklemek için paket.

   ![esp8266 paketi yüklü](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Tıklatın **Araçları** > **Panosu** > **Adafruit M0 WiFi**.

5. Sürücüleri (yalnızca Windows için) yükleyin. Yumuşatma M0 WiFi bağladığınızda, bir sürücü yüklemeniz gerekebilir. Tıklatın [sayfasındaki indirme bağlantısı](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) sürücü yükleyici indirmek için. İstediğiniz sürücüleri yüklemek için adımları izleyin.

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükleme

1. Arduino IDE'de tıklatın **taslak** > **dahil Kitaplığı** > **yönetmek kitaplıkları**.

2. Aşağıdaki Kitaplığı Ara tek tek adları. Bulduğunuz her kitaplığını tıklatın **yükleme**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. El ile yüklemeniz `Adafruit_WINC1500`. Git [bu Web sitesi](https://github.com/adafruit/Adafruit_WINC1500) tıklatıp **Kopyala veya indir** > **ZIP'i indir**. Arduino IDE'yi geçin **taslak** > **dahil Kitaplığı** > **.zip Kitaplığı eklemek** ve zip dosyası ekleyin.

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>Örnek uygulamayı gerçek BME280 algılayıcı yoksa kullanın

Gerçek BME280 algılayıcı yoksa, örnek uygulamayı sıcaklık ve nem veri benzetimini yapabilirsiniz. Örnek uygulamayı benzetimli veri kullanacak şekilde ayarlamak için aşağıdaki adımları izleyin:

1. Açık `config.h` dosyasını `app` klasör.

2. Aşağıdaki kod satırını bulun ve değeri değiştirin `false` için `true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Örnek uygulamayı benzetimli veri kullanacak şekilde yapılandırma](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Dosyayı kaydetmek `Control-s`.

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a>Yumuşatma M0 WiFi örnek uygulamayı dağıtmak

1. Arduino IDE'de tıklatın **aracı** > **bağlantı noktası**ve yumuşatma M0 WiFi için seri bağlantı noktası'ı tıklatın.

2. ' I tıklatın **taslak** > **karşıya** oluşturup yumuşatma M0 WiFi örnek uygulamayı dağıtın.

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra kimlik bilgilerinizi girmeniz için şu adımları izleyin:

1. Arduino IDE'de tıklatın **Araçları** > **seri İzleyici**.

2. Seri İzleyici penceresinin sağ alt köşesinde seçin **hiçbir satır bitiş** sol aşağı açılan listesinde.
3. Seçin **115200 baud** sağdaki aşağı açılan listesinde.
4. ' A tıklayın ve bunu sağlamak için sorulursa üst giriş kutusuna aşağıdaki bilgileri girin **Gönder**:

   * Wi-Fi SSID
   * Wi-Fi parola
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi EEPROM, yumuşatma M0 WiFi içinde depolanır. Yumuşatma M0 WiFi panosunda Sıfırla düğmesini tıklatın, örnek uygulamayı bilgileri silmek isteyip istemediğinizi sorar. Girin `Y` bilgileri silmek için. İkinci kez bilgileri vermeniz istenir.

### <a name="verify-that-the-sample-application-is-running-successfully"></a>Örnek Uygulama başarıyla çalıştığını doğrulayın

Yumuşatma M0 WiFi üzerinde seri İzleyici penceresinin ve yanıp sönen LED aşağıdaki çıkışı görürseniz örnek uygulama başarıyla çalıştırma:

![Arduino IDE içinde son çıktı](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla yumuşatma M0 WiFi IOT hub'ına bağlı ve IOT hub'ınıza yakalanan algılayıcı verilerini gönderilir. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

