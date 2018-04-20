---
title: Bulut için - ESP8266 yumuşatma HUZZAH ESP8266 Azure IOT Hub'ına bağlanmak | Microsoft Docs
description: Kurulum ve Bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub için bu Adafruit yumuşatma HUZZAH ESP8266 bağlanma hakkında bilgi edinin.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: ''
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: b850bb25d7a6d06a2d0f55ed915321badf76bb39
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Bulutta Azure IOT Hub'ına Adafruit yumuşatma HUZZAH ESP8266 Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22, yumuşatma HUZZAH ESP8266 ve IOT hub'ı arasında bir bağlantı](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Neler


Adafruit yumuşatma HUZZAH ESP8266, oluşturduğunuz bir IOT hub'ına bağlanın. Sonra DHT22 algılayıcı sıcaklık ve nem veri toplamak üzere ESP8266 üzerinde bir örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza algılayıcı verileri gönderin.

> [!NOTE]
> Diğer ESP8266 panoları kullanıyorsanız, yine IOT hub'ınıza bağlanmak için aşağıdaki adımları izleyebilirsiniz. Kullanmakta olduğunuz ESP8266 Panosu bağlı olarak, yeniden yapılandırmanız gerekebilir `LED_PIN`. AI Thinker gelen ESP8266 kullanıyorsanız, örneğin, ondan değişebilir `0` için `2`. Bir pakete henüz yok mu? Elde [Azure Web sitesi](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Öğrenecekleriniz

* IOT hub'ı oluşturma ve yumuşatma HUZZAH ESP8266 için bir cihaz kaydetme
* Algılayıcı ve bilgisayarınızla yumuşatma HUZZAH ESP8266 bağlanma
* Yumuşatma HUZZAH ESP8266 üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl
* IOT hub'ınıza algılayıcı verileri gönderme

## <a name="what-you-need"></a>Ne gerekiyor

![Öğretici için gerekli bölümleri](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi tamamlamak için aşağıdaki bölümleri yumuşatma HUZZAH ESP8266 Starter Seti'nden gerekir:

* Yumuşatma HUZZAH ESP8266 Panosu
* Mikro USB tipi A USB kablosu

Ayrıca, geliştirme ortamınız için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Mac veya Windows veya Ubuntu çalıştıran bir bilgisayar.
* Yumuşatma HUZZAH ESP8266 bağlanmak için kablosuz ağ.
* Yapılandırma Aracı indirmek için Internet bağlantısı.
* [Visual Studio Code uzantısı Arduino için](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino).

> [!Note]
> Arduino sürüm 1.6.8 olması için Visual Studio Code uzantısı tarafından kullanılan Arduino IDE sürümü veya sonraki bir sürümü. Önceki sürümlerde AzureIoT kitaplığı ile çalışmaz.

Algılayıcı olmayan olasılığına aşağıdaki öğeler isteğe bağlıdır. Ayrıca sanal algılayıcı verilerini kullanma seçeneğiniz vardır.

* Adafruit DHT22 sıcaklık ve nem algılayıcısı
* Bir breadboard
* M/M anahtar kabloları


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Algılayıcı ve bilgisayarınızla yumuşatma HUZZAH ESP8266 Bağlan
Bu bölümde, algılayıcılar panonuz için bağlayın. Daha sonra Cihazınızı başka kullanmak için bilgisayarınıza takın.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Yumuşatma HUZZAH ESP8266 DHT22 sıcaklık ve nem algılayıcı Bağlan

Şu şekilde bağlantı kurmayı breadboard ve anahtar kablolarını kullanır. Algılayıcı yoksa, benzetimli algılayıcı verilerini yerine kullandığından bu bölümü atlayabilirsiniz.

![Bağlantı Başvurusu](media/iot-hub-arduino-huzzah-esp8266-get-started/17_connections_on_breadboard.png)


Algılayıcı PIN'ler için aşağıdaki kablolama kullanın:


| Başlangıç (algılayıcı)           | Bitiş (kartı)           | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 31F)            | 3v (PIN 58H)           | Kırmızı kablosu     |
| Veri (PIN 32F)           | GPIO'yu 2 (PIN 46A)       | Mavi kablosu    |
| GND (PIN 34F)            | GND (PIN 56I)          | Siyah kablosu   |

Daha fazla bilgi için bkz: [Adafruit DHT22 algılayıcı Kurulum](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) ve [Adafruit yumuşatma HUZZAH Esp8266 no'lu](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Şimdi yumuşatma Huzzah ESP8266 ile çalışma algılayıcı bağlı olması gerekir.

![Yumuşatma Huzzah ile DHT22 Bağlan](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Yumuşatma HUZZAH ESP8266 bilgisayarınıza bağlayın

Sonraki gösterildiği gibi geçiş yumuşatma HUZZAH ESP8266 bilgisayarınıza bağlanmak için mikro USB tipi A USB kablosu kullanın.

![Yumuşatma Huzzah bilgisayarınıza bağlayın](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seri bağlantı noktası izinleri (yalnızca Ubuntu) ekleyin


Ubuntu kullanırsanız, USB bağlantı noktası, yumuşatma HUZZAH ESP8266 üzerinde çalışması için izinlere sahip olduğunuzdan emin olun. Seri bağlantı noktası izinleri eklemek için aşağıdaki adımları izleyin:


1. Terminal aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktıları birini alın:

   * crw-rw---1 kök uucp xxxxxxxx
   * crw-rw---1 kök araması xxxxxxxx

   Çıktıda dikkat `uucp` veya `dialout` USB bağlantı noktasına Grup sahibi adıdır.

1. Kullanıcı, aşağıdaki komutu çalıştırarak gruba ekleyin:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` Önceki adımda elde ettiğiniz Grup sahibi adıdır. `<username>` Ubuntu kullanıcı adınızdır.

1. Ubuntu dışında oturum ve yeniden değişiklik görünmesi oturum açın.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplamak ve IOT hub'ınıza gönderin

Bu bölümde, dağıtın ve yumuşatma HUZZAH ESP8266 üzerinde bir örnek uygulamayı çalıştırın. Örnek uygulama LED yumuşatma HUZZAH ESP8266 üzerinde yanıp ve IOT hub'ınıza DHT22 algılayıcı toplanan sıcaklık ve nem verileri gönderir.

### <a name="get-the-sample-application-from-github"></a>Örnek uygulama Github'dan alma

Örnek uygulama, GitHub üzerinde barındırılır. Github'dan örnek uygulamayı içeren örnek depoyu kopyalayın. Örnek deposuna kopyalamak için aşağıdaki adımları izleyin:

1. Bir komut istemi veya terminal penceresi açın.
1. Depolanması için örnek uygulama istediğiniz bir klasöre gidin.
1. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Visual Studio Code yumuşatma HUZZAH ESP8266 için paketi yükleyin:

1. Örnek uygulama depolandığı klasörü açın.
1. Visual Studio Code uygulama klasöründe app.ino dosyasını açın.

   ![Örnek uygulama Visual Studio Code açın](media/iot-hub-arduino-huzzah-esp8266-get-started/10_vscode-open-sample-app.png)

1. Visual Studio kodunu girin `F1`.
1. Tür **Arduino** seçip **Arduino: Panosu Yöneticisi**.
1. İçinde **Arduino Panosu Yöneticisi** sekmesini tıklatın, **ek URL'leri**.

   ![VS Code Arduino Panosu Yöneticisi](media/iot-hub-arduino-huzzah-esp8266-get-started/11_vscode-arduino-board-manager.png)

1. İçinde **kullanıcı ayarları** penceresinde, aşağıdaki dosya sonunda kopyalayıp

   `"arduino.additionalUrls": "http://arduino.esp8266.com/stable/package_esp8266com_index.json"`

   ![VS Code'da Arduino paket URL'sini yapılandırın](media/iot-hub-arduino-huzzah-esp8266-get-started/12_vscode-package-url.png)

1. Dosyayı kaydedin ve kapatın **kullanıcı ayarları** sekmesi.
1. Tıklatın **yenileme paket dizinleri**. Yenileme tamamlandıktan sonra arama **esp8266**.
1. Tıklatın **yükleme** esp8266 düğmesi.

   Panoları Yöneticisi ESP8266 2.2.0 veya sonraki bir sürümü ile yüklü olduğunu gösterir.

   ![esp8266 paketi yüklü](media/iot-hub-arduino-huzzah-esp8266-get-started/13_vscode-esp8266-installed.png)

1. Girin `F1`, ardından **Arduino** seçip **Arduino: Panosu Config**.
1. İçin kutusu **seçili Panosu:** ve türü **esp8266**seçeneğini belirleyip **Adafruit HUZZAH ESP8266 (esp8266)**.

   ![Esp8266 tablosu seçin](media/iot-hub-arduino-huzzah-esp8266-get-started/14_vscode-select-esp8266.png)

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükleme

1. Visual Studio kodunu girin `F1`, ardından **Arduino** seçip **Arduino: Kitaplığı Yöneticisi**.
1. Aşağıdaki Kitaplığı Ara tek tek adları. Bulduğunuz her kitaplığını tıklatın **yükleme**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Gerçek DHT22 algılayıcı yok mu?

Örnek uygulama, gerçek DHT22 algılayıcı olmayan olasılığına sıcaklık ve nem veri benzetimini yapabilirsiniz. Örnek uygulamayı benzetimli veri kullanacak şekilde ayarlamak için aşağıdaki adımları izleyin:

1. Açık `config.h` dosyasını `app` klasör.
1. Aşağıdaki kod satırını bulun ve değeri değiştirin `false` için `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Örnek uygulamayı benzetimli veri kullanacak şekilde yapılandırma](media/iot-hub-arduino-huzzah-esp8266-get-started/15_vscode-configure-app-use-simulated-data.png)

1. Dosyayı kaydedin.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Yumuşatma HUZZAH ESP8266 örnek uygulamayı dağıtmak

1. Visual Studio kodda tıklatın **<Select Serial Port>** durumunu çubuk ve yumuşatma HUZZAH ESP8266 için seri bağlantı noktası'ı tıklatın.
1. Girin `F1`, yazın **Arduino** seçip **Arduino: karşıya** oluşturmak ve yumuşatma HUZZAH ESP8266 örnek uygulamayı dağıtmak için.

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra kimlik bilgilerinizi girmeniz için şu adımları izleyin:

1. Arduino IDE açın, **Araçları** > **seri İzleyici**.
1. Seri İzleyicisi penceresinde sağ alt köşedeki iki açılan listelerde dikkat edin.
1. Seçin **hiçbir satır bitiş** sol aşağı açılan listesi.
1. Seçin **115200 baud** sağda açılan listesi.
1. Bunları sağlayın ve ardından sorulursa seri İzleyici penceresinin en üstünde bulunan giriş kutusuna aşağıdaki bilgileri girin **Gönder**.
   * Wi-Fi SSID
   * Wi-Fi parola
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi EEPROM, yumuşatma HUZZAH ESP8266 içinde depolanır. Yumuşatma HUZZAH ESP8266 panosunda Sıfırla düğmesini tıklatın, örnek uygulamayı bilgileri silmek isteyip istemediğinizi sorar. Girin `Y` silinmesi bilgilere sahip olması. İkinci kez bilgileri vermeniz istenir.

### <a name="verify-the-sample-application-is-running-successfully"></a>Örnek Uygulama başarıyla çalıştığını doğrulayın

Yumuşatma HUZZAH ESP8266 üzerinde seri İzleyici penceresinin ve yanıp sönen LED aşağıdaki çıkışı görürseniz, örnek uygulamayı başarılı bir şekilde çalışıyor.

![Arduino IDE içinde son çıktı](media/iot-hub-arduino-huzzah-esp8266-get-started/16_arduino-ide-final-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla yumuşatma HUZZAH ESP8266 IOT hub'ına bağlı ve yakalanan algılayıcı verilerini IOT hub'ına gönderilen. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

