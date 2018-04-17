---
title: Bulut için - Sparkfun ESP8266 şey geliştirme bağlanmak Azure IOT Hub'ına ESP8266 | Microsoft Docs
description: Kurulum ve Bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub için bu Sparkfun ESP8266 şey geliştirme bağlanma hakkında bilgi edinin.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: ''
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: e6837d0312217d8e27e3639b8220f5016a2505a6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a>Bulutta Azure IOT Hub'ına Sparkfun ESP8266 şey geliştirme Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22, şey geliştirme ve IOT hub'ı arasında bir bağlantı](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Ne yapacağını

Sparkfun ESP8266 şey geliştirme oluşturacağınız bir IOT hub'ınıza bağlanın. Ardından örnek bir uygulama DHT22 algılayıcı sıcaklık ve nem verileri toplamak için ESP8266 çalıştırın. Son olarak, algılayıcı verilerini IOT hub'ınıza gönderir.

> [!NOTE]
> Diğer ESP8266 panoları kullanıyorsanız, yine IOT hub'ınıza bağlanmak için aşağıdaki adımları izleyebilirsiniz. Kullanmakta olduğunuz ESP8266 Panosu bağlı olarak, yeniden yapılandırmanız gerekebilir `LED_PIN`. AI Thinker gelen ESP8266 kullanıyorsanız, örneğin, ondan değişebilir `0` için `2`. Bir pakete henüz yok mu?: tıklatın [burada](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

* IOT hub'ı oluşturma ve şey istisnası için bir cihaz kaydetme
* Şey geliştirme algılayıcı ve bilgisayarınızla bağlanma.
* Örnek bir uygulama üzerinde şey istisnası çalıştırarak algılayıcı verilerini toplamak nasıl
* IOT hub'ınıza algılayıcı verileri göndermek nasıl.

## <a name="what-you-will-need"></a>İhtiyacınız olacak

![Öğretici için gerekli bölümleri](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi tamamlamak için aşağıdaki bölümleri şey geliştirme Starter Seti'nden gerekir:

* Sparkfun ESP8266 şey geliştirme Panosu.
* Mikro USB tipi A USB kablosu.

Ayrıca geliştirme ortamınız için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Mac veya Windows veya Ubuntu çalıştıran bir bilgisayar.
* Kablosuz ağ bağlanmak Sparkfun ESP8266 şey geliştirme için.
* Yapılandırma Aracı indirmek için Internet bağlantısı.
* [Arduino IDE](https://www.arduino.cc/en/main/software) sürüm 1.6.8 (veya daha yeni), önceki sürümleri AzureIoT kitaplığı ile çalışmaz.

Algılayıcı olmayan olasılığına aşağıdaki öğeler isteğe bağlıdır. Ayrıca sanal algılayıcı verilerini kullanma seçeneğiniz vardır.

* Bir Adafruit DHT22 sıcaklık ve nem algılayıcı.
* Bir breadboard.
* M/M anahtar kablolarını.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a>Algılayıcı ve bilgisayarınızla ESP8266 şey geliştirme Bağlan

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a>ESP8266 şey geliştirme DHT22 sıcaklık ve nem algılayıcı Bağlan

Şu şekilde bağlantı kurmayı breadboard ve anahtar kablolarını kullanır. Algılayıcı yoksa, benzetimli algılayıcı verilerini yerine kullandığından bu bölümü atlayabilirsiniz.

![Bağlantı Başvurusu](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Aşağıdaki kablolama algılayıcı PIN'ler için kullanırız:

| Başlangıç (algılayıcı)           | Bitiş (kartı)           | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 27F)            | 3v (PIN 8A)           | Kırmızı kablosu     |
| Veri (PIN 28F)           | GPIO'yu 2 (PIN 9A)       | Beyaz kablosu    |
| GND (PIN 30F)            | GND (PIN 7J)          | Siyah kablosu   |


- Daha fazla bilgi için bkz: [DHT22 algılayıcı Kurulum](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) ve [Sparkfun ESP8266 şey geliştirme belirtimi](https://www.sparkfun.com/products/13711)

Şimdi, Sparkfun ESP8266 şey geliştirme ile çalışma algılayıcı bağlı olması gerekir.

![dht22 ESP8266 şey geliştirme ile bağlanma](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a>Sparkfun ESP8266 şey geliştirme bilgisayarınıza bağlayın

Sparkfun ESP8266 şey geliştirme bilgisayarınıza şu şekilde bağlanmak için mikro USB tipi A USB kablosu kullanın.

![yumuşatma huzzah bilgisayarınıza bağlayın](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Seri bağlantı noktası izinleri – yalnızca Ubuntu ekleyin

Ubuntu kullanıyorsanız, normal bir kullanıcı USB bağlantı noktası, Sparkfun ESP8266 şey istisnası üzerinde çalışması için izinlere sahip olduğundan emin olun Normal bir kullanıcı için seri bağlantı noktası izinleri eklemek için aşağıdaki adımları izleyin:

1. Terminal aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktıları birini alın:

   * crw-rw---1 kök uucp xxxxxxxx
   * crw-rw---1 kök araması xxxxxxxx

   Çıktıda fark `uucp` veya `dialout` diğer bir deyişle USB bağlantı noktasına Grup sahibi adı.

1. Kullanıcı, aşağıdaki komutu çalıştırarak gruba ekleyin:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` Önceki adımda elde ettiğiniz Grup sahibi adıdır. `<username>` Ubuntu kullanıcı adınızdır.

1. Ubuntu ve değişikliğin etkili olması için yeniden için içindeki oturum.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplamak ve IOT hub'ınıza gönderin

Bu bölümde, dağıtma ve Sparkfun ESP8266 şey istisnası üzerinde örnek uygulamayı çalıştırma Örnek uygulama LED Sparkfun ESP8266 şey geliştirme üzerinde yanıp ve IOT hub'ınıza DHT22 algılayıcı toplanan sıcaklık ve nem verileri gönderir.

### <a name="get-the-sample-application-from-github"></a>Örnek uygulama Github'dan alma

Örnek uygulama, GitHub üzerinde barındırılır. Github'dan örnek uygulamayı içeren örnek depoyu kopyalayın. Örnek deposuna kopyalamak için aşağıdaki adımları izleyin:

1. Bir komut istemi veya terminal penceresi açın.
1. Depolanması için örnek uygulama istediğiniz bir klasöre gidin.
1. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Paket Sparkfun ESP8266 şey geliştirme Arduino IDE'de yükle:

1. Örnek uygulama depolandığı klasörü açın.
1. Arduino IDE uygulama klasöründe app.ino dosyasını açın.

   ![Örnek uygulamayı arduino IDE içinde Aç](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Arduino IDE'de tıklatın **dosya** > **Tercihler**.
1. İçinde **Tercihler** iletişim kutusunda, simgesine tıklayın **ek panoları yöneticisi URL'leri** metin kutusu.
1. Açılan pencerede aşağıdaki URL'yi girin ve ardından **Tamam**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Paket URL'sini arduino IDE'de işaret](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. İçinde **tercih** iletişim kutusu, tıklatın **Tamam**.
1. Tıklatın **Araçları** > **Panosu** > **panoları Yöneticisi**ve esp8266 için arama yapın.
   ESP8266 2.2.0 veya sonraki bir sürümüyle yüklenmesi gerekir.

   ![esp8266 paketi yüklü](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Tıklatın **Araçları** > **Panosu** > **Sparkfun ESP8266 şey geliştirme**.

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükleme

1. Arduino IDE'de tıklatın **taslak** > **dahil Kitaplığı** > **yönetmek kitaplıkları**.
1. Aşağıdaki Kitaplığı Ara tek tek adları. Her bulduğunuz kitaplığının tıklatın **yükleme**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Gerçek DHT22 algılayıcı yok mu?

Örnek uygulama, gerçek DHT22 algılayıcı olmayan olasılığına sıcaklık ve nem veri benzetimini yapabilirsiniz. Örnek uygulamayı benzetimli veri kullanacak şekilde etkinleştirmek için aşağıdaki adımları izleyin:

1. Açık `config.h` dosyasını `app` klasör.
1. Aşağıdaki kod satırını bulun ve değeri değiştirin `false` için `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Örnek uygulamayı benzetimli veri kullanacak şekilde yapılandırma](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. İle Kaydet `Control-s`.

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a>Sparkfun ESP8266 şey geliştirme için örnek uygulama dağıtma

1. Arduino IDE'de tıklatın **aracı** > **bağlantı noktası**ve ardından seri bağlantı noktası Sparkfun ESP8266 şey istisnası için tıklayın
1. Tıklatın **taslak** > **karşıya** oluşturmak ve Sparkfun ESP8266 şey istisnası örnek uygulamayı dağıtmak için

> [!Note]
> MacOS kullanıyorsanız büyük olasılıkla karşıya yükleme sırasında aşağıdaki iletileri görebilirsiniz. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Lütfen ternimal penceresini açın ve bu sorunu çözmek için eylemleri tamamlayın.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra kimlik bilgilerinizi girmeniz için adımları izleyin:

1. Arduino IDE'de tıklatın **Araçları** > **seri İzleyici**.
1. Seri İzleyicisi penceresinde sağ alt köşesindeki iki açılan listelerde dikkat edin.
1. Seçin **hiçbir satır bitiş** sol aşağı açılan listesi.
1. Seçin **115200 baud** sağda açılan listesi.
1. Bunları sağlayın ve ardından sorulursa seri İzleyici penceresinin en üstünde bulunan giriş kutusuna aşağıdaki bilgileri girin **Gönder**.
   * Wi-Fi SSID
   * Wi-Fi parola
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi EEPROM, Sparkfun ESP8266 şey istisnası içinde depolanır Sparkfun ESP8266 şey geliştirme panosunda Sıfırla düğmesini tıklatın, örnek uygulamayı bilgileri silmek isteyip istemediğinizi sorar. Girin `Y` sahip silinmesi bilgi ve bilgileri tekrar sağlamanız istenir.

### <a name="verify-the-sample-application-is-running-successfully"></a>Örnek Uygulama başarıyla çalıştığını doğrulayın

Seri İzleyici penceresinin ve yanıp sönen LED aşağıdaki çıkışı Sparkfun ESP8266 şey geliştirme üzerinde görürseniz, örnek uygulamayı başarılı bir şekilde çalışıyor.

![arduino IDE içinde son çıktı](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla Sparkfun ESP8266 şey geliştirme IOT hub'ına bağlı ve IOT hub'ınıza yakalanan algılayıcı verilerini gönderilir. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
