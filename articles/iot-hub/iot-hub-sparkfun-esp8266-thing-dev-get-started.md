---
title: Buluta - Azure IOT hub'ına Sparkfun ESP8266 Thing Dev bağlanma ESP8266 | Microsoft Docs
description: Kurulum ve Bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub için bunu Sparkfun ESP8266 Thing Dev bağlanma hakkında bilgi edinin.
author: rangv
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 75ff53d5be29af08bb8e9c1b41f61040e5710cf7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452524"
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a>Azure IOT hub'ı bulutta Sparkfun ESP8266 Thing Dev bağlanma

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 Thing Dev ve IOT hub'ı arasında bir bağlantı](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Neler yapabileceği

Sparkfun ESP8266 Thing Dev oluşturacağınız bir IOT hub'a bağlayın. Ardından ESP8266 DHT22 algılayıcıdan sıcaklık ve nem verileri toplamak için örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

> [!NOTE]
> Diğer ESP8266 panoları kullanıyorsanız, IOT hub'ınıza bağlanmak için aşağıdaki adımları yine de takip edebilirsiniz. Kullanmakta olduğunuz ESP8266 tablosu bağlı olarak, yeniden yapılandırmanız gerekebilir `LED_PIN`. Yapay ZEKA Thinker gelen ESP8266 kullanıyorsanız, örneğin, ondan değişebilir `0` için `2`. Bir pakete henüz yok mu?: tıklayın [burada](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

* IOT hub'ı oluşturma ve şey geliştirme için bir cihazı kaydedin
* Thing Dev algılayıcı ve bilgisayarınızla bağlanma.
* Şey geliştirme örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="what-you-will-need"></a>İhtiyacınız

![Öğretici için gerekli olan bölümleri](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi tamamlamak için aşağıdaki bölümleri, Thing Dev başlangıç Seti gerekir:

* Sparkfun ESP8266 Thing Dev kartı.
* Türü bir USB kablosu Micro USB.

Ayrıca, geliştirme ortamınız için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Mac veya Windows veya Ubuntu çalıştıran bir bilgisayar.
* Sparkfun ESP8266 Thing Dev bağlanmak için kablosuz ağ.
* Yapılandırma Aracı indirmek için Internet Bağlantısı'nı tıklatın.
* [Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 sürümü (veya daha yeni), önceki sürümlerinde AzureIoT kitaplığı ile çalışmaz.

Algılayıcı yok durumunda aşağıdaki öğeler isteğe bağlıdır. Ayrıca, sanal sensör verilerini kullanma seçeneğiniz de vardır.

* Bir Adafruit DHT22 sıcaklık ve nem algılayıcı.
* Bir breadboard.
* M/dk anahtar kablolarına.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a>Algılayıcı ve bilgisayarınızla ESP8266 Thing Dev bağlanma

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a>DHT22 sıcaklık ve nem algılayıcı bağlanmak için ESP8266 Thing Dev

Şu şekilde bağlantı kurmak için breadboard ve anahtar kablo kullanın. Algılayıcı yoksa, bunun yerine sanal sensör verilerini kullanabileceğinizden bu bölümü atlayın.

![Bağlantı Başvurusu](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Algılayıcı sabitlemek için aşağıdaki kablolama kullanacağız:

| Başlangıç (algılayıcı)           | Bitiş (Pano)           | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 27F)            | 3v (PIN 8A)           | Kırmızı kablosu     |
| Veri (PIN 28F)           | Bir GPIO'yu 2 (PIN 9A)       | Beyaz kablosu    |
| GND (PIN 30F)            | GND (PIN 7J)          | Siyah kablo   |


- Daha fazla bilgi için bkz: [DHT22 algılayıcı Kurulum](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) ve [Sparkfun ESP8266 Thing Dev belirtimi](https://www.sparkfun.com/products/13711)

Şimdi, Sparkfun ESP8266 Thing Dev çalışma algılayıcısı ile bağlanmalıdır.

![dht22 ESP8266 Thing Dev ile bağlanma](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a>Sparkfun ESP8266 Thing Dev bilgisayarınıza bağlayın

Sparkfun ESP8266 Thing Dev gibi bilgisayarınıza bağlanmak için mikro USB türü bir USB kablosu kullanın.

![feather huzzah bilgisayarınıza bağlayın](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Seri bağlantı noktası izinleri – yalnızca Ubuntu ekleyin

Ubuntu kullanırsanız, normal bir kullanıcı USB bağlantı noktası, Sparkfun ESP8266 Thing geliştirme üzerinde çalışmak için izinlere sahip olduğundan emin olun Seri bağlantı noktası izinler normal bir kullanıcı eklemek için aşağıdaki adımları izleyin:

1. Bir terminalde aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktıları birini olursunuz:

   * 1 kök uucp xxxxxxxx crw-rw---
   * 1 kök araması xxxxxxxx crw-rw---

   Çıkışta, fark `uucp` veya `dialout` diğer bir deyişle USB bağlantı noktasına Grup sahibi adı.

1. Kullanıcı, aşağıdaki komutu çalıştırarak gruba ekleyin:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` Önceki adımda elde ettiğiniz Grup sahibi adıdır. `<username>` Ubuntu kullanıcı adınızdır.

1. Ubuntu out ve değişikliğin etkili olması için yeniden da oturum.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplamak ve IOT hub'ına gönderme

Bu bölümde, dağıtma ve Sparkfun ESP8266 Thing geliştirme hakkında bir örnek uygulamayı çalıştırma Örnek uygulamayı LED Sparkfun ESP8266 Thing Dev üzerinde yanıp ve IOT hub'ınıza DHT22 algılayıcıdan toplanan sıcaklık ve nem verileri gönderir.

### <a name="get-the-sample-application-from-github"></a>Örnek uygulamayı Github'dan alma

Örnek uygulama, GitHub üzerinde barındırılır. Github'dan örnek uygulamayı içeren örnek depoyu kopyalayın. Örnek depoyu kopyalamak için şu adımları izleyin:

1. Bir komut istemi veya terminal penceresi açın.
1. Depolanması için örnek uygulamayı istediğiniz klasöre gidin.
1. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Arduino IDE içindeki Sparkfun ESP8266 Thing Dev için paketi yükleyin:

1. Örnek uygulamayı depolandığı klasörü açın.
1. Arduino IDE uygulama klasöründe app.ino dosyasını açın.

   ![arduino IDE içinde örnek uygulama açın](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Arduino IDE içinde tıklayın **dosya** > **tercihleri**.
1. İçinde **tercihleri** iletişim kutusunda, yanındaki simgeye **ek panoları yöneticisi URL'leri** metin kutusu.
1. Açılır pencerede aşağıdaki URL'yi girin ve ardından **Tamam**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![arduino IDE içindeki bir paket URL'si gelin](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. İçinde **tercih** iletişim kutusu, tıklayın **Tamam**.
1. Tıklayın **Araçları** > **Panosu** > **panoları Manager**ve sonra esp8266 için arama yapın.
   ESP8266 2.2.0 veya sonraki bir sürümü ile yüklü olması gerekir.

   ![Esp8266 paketin yüklü](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Tıklayın **Araçları** > **Panosu** > **Sparkfun ESP8266 Thing Dev**.

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükleme

1. Arduino IDE içinde tıklayın **taslak** > **kitaplığı dahil** > **kitaplıklarını yönetme**.
1. Aşağıdaki kitaplık arama tek tek adları. Her bulduğunuz kitaplığının tıklayın **yükleme**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Gerçek bir DHT22 algılayıcı yok mu?

Örnek uygulama, gerçek DHT22 algılayıcı yok durumunda sıcaklık ve nem veri benzetimini yapabilirsiniz. Sanal veri kullanmak için örnek uygulamayı etkinleştirmek için aşağıdaki adımları izleyin:

1. Açık `config.h` dosyası `app` klasör.
1. Aşağıdaki kod satırını bulun ve değerini `false` için `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Sanal veri kullanmak için örnek uygulamayı yapılandırma](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Kaydettiğiniz `Control-s`.

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a>Sparkfun ESP8266 Thing Dev örnek uygulamayı dağıtma

1. Arduino IDE içinde tıklayın **aracı** > **bağlantı noktası**ve ardından seri bağlantı noktası Sparkfun ESP8266 Thing geliştirme için tıklayın
1. Tıklayın **taslak** > **karşıya** oluşturup Sparkfun ESP8266 Thing geliştirme için örnek uygulamayı dağıtma

> [!Note]
> MacOS kullanıyorsanız büyük olasılıkla karşıya yüklenirken aşağıdaki iletileri görebilirsiniz. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Lütfen, ternimal penceresi açın ve bu sorunu çözmek için eylemleri tamamlayın.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra kimlik bilgilerinizi girmeniz için adımları izleyin:

1. Arduino IDE içinde tıklayın **Araçları** > **seri İzleyici**.
1. Seri İzleyicisi penceresinde sağ alt köşesindeki iki açılan listelerde dikkat edin.
1. Seçin **sondaysa satır** sol aşağı açılan listesi.
1. Seçin **115200 baud** sağda açılan listesi.
1. İstenirse bunları sağlayın ve ardından seri İzleyici penceresinin en üstünde yer alan giriş kutusuna aşağıdaki bilgileri girin. **Gönder**.
   * Wi-Fi SSID
   * Wi-Fi parolası
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi EEPROM, Sparkfun ESP8266 Thing geliştirme içinde depolanır. Sparkfun ESP8266 Thing Dev kartı Sıfırla düğmesine tıklayın, örnek uygulama, bilgi silmek isteyip istemediğinizi sorar. Girin `Y` sağlamak için silinmesi bilgi ve bu bilgileri tekrar sağlamanız istenir.

### <a name="verify-the-sample-application-is-running-successfully"></a>Örnek Uygulama başarıyla çalıştığını doğrulayın

Sparkfun ESP8266 Thing Dev üzerinde aşağıdaki çıktıyı seri İzleme penceresi ve yanıp sönen LED görürseniz örnek uygulama başarıyla çalışıyor.

![arduino IDE içindeki son çıkış](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla Sparkfun ESP8266 Thing Dev IOT hub'ınıza bağlanan ve yakalanan sensör verilerini IOT hub'ına gönderilen. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
