---
title: Buluta (Python) - Azure IOT Hub'ına bağlanmak Raspberry Pi'yi Böğürtlenli Pi | Microsoft Docs
description: Kurulum ve Raspberry Pi'yi'nın bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub ile Raspberry Pi'yi bağlanma hakkında bilgi edinin.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: Azure IOT Böğürtlenli PI Böğürtlenli PI IOT hub, bulut için Böğürtlenli pi gönderme, verileri buluta Böğürtlenli pi
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: 7069748c10f7c98f80fadc008f43a3aa02f7ac0e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-python"></a>Azure IOT Hub (Python) Böğürtlenli Pi Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspbian çalıştıran Raspberry Pi'yi ile çalışmanın temelleri öğrenerek başlayın. Daha sonra sorunsuzca aygıtlarınızı buluta kullanarak bağlanmasına nasıl öğrenin [Azure IOT Hub](iot-hub-what-is-iot-hub.md). Windows 10 IOT Core örnekleri için Git [Windows Geliştirme Merkezi](http://www.windowsondevices.com/).

Bir pakete henüz yok mu? Deneyin [Raspberry Pi'yi çevrimiçi simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Veya yeni bir paket satın [burada](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Neler

* IOT hub'ı oluşturun.
* Bir cihaz IOT hub'ınıza Pi için kaydetme.
* Böğürtlenli PI kurulumu.
* Pi algılayıcı verilerini IOT hub'ınıza gönderilecek örnek bir uygulamayı çalıştırın.

Raspberry Pi'yi, oluşturduğunuz bir IOT hub'ına bağlanın. Pi BME280 algılayıcı sıcaklık ve nem verileri toplamak için bir örnek uygulamayı çalıştırın. ardından. Son olarak, IOT hub'ınıza algılayıcı verileri gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizenizi elde etme.
* Pi BME280 algılayıcı ile bağlanma.
* Pi üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza algılayıcı verileri göndermek nasıl.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* Raspberry Pi 2 veya Raspberry Pi 3 Panosu.
* Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir izleyici, bir USB klavye ve fare Pi bağlanan.
* Mac veya Windows veya Linux çalıştıran bir bilgisayar.
* Bir Internet bağlantısı.
* 16 GB veya üzeri microSD kartı.
* İşletim sistemi görüntüsü microSD kartı üzerine yazmak için bir USB SD bağdaştırıcı veya microSD kartı.
* 5-volt 2 amp güç 6 kaplama alanı mikro USB kablosu ile sağlayın.

Aşağıdaki öğeler isteğe bağlıdır:

* Bir birleştirilmiş Adafruit BME280 sıcaklık, baskısı ve nem algılayıcı.
* Bir breadboard.
* 6 F/M anahtar kablolarını.
* Yayılmış 10 mm LED.


> [!NOTE] 
Kod örneği desteği algılayıcı verilerini benzetimli çünkü bu öğeler isteğe bağlıdır.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi'yi ayarlayın

### <a name="install-the-raspbian-operating-system-for-pi"></a>Pi için Raspbian işletim sistemini yükleme

MicroSD kartı Raspbian görüntünün yüklenmesi için hazırlayın.

1. Raspbian indirin.
   1. [Masaüstü ile Raspbian Jessie karşıdan](https://www.raspberrypi.org/downloads/raspbian/) (.zip dosyası).
   1. Raspbian görüntünün bilgisayarınızdaki bir klasöre ayıklayın.
1. Raspbian microSD kartı yükleyin.
   1. [Etcher SD kart yazıcı yardımcı programı yükleyip](https://etcher.io/).
   1. Etcher çalıştırın ve 1. adımda ayıkladığınız Raspbian görüntüyü seçin.
   1. MicroSD kartı sürücü seçin. Etcher zaten doğru sürücü seçmiş olabilirsiniz olduğunu unutmayın.
   1. Raspbian microSD kartı yüklemek için Flash'ı tıklatın.
   1. Yükleme tamamlandığında microSD kartı bilgisayarınızdan kaldırın. Etcher otomatik olarak çıkarır veya tamamlanmasından sonra microSD kartı çıkarır çünkü microSD kartı doğrudan kaldırmak güvenlidir.
   1. MicroSD kartı Pi yerleştirin.

### <a name="enable-ssh-and-i2c"></a>SSH ve I2C etkinleştir

1. Monitör, klavye ve fare pi bağlanmak, Pi başlatın ve ardından içinde Raspbian kullanarak oturum `pi` kullanıcı adı ve `raspberry` ve parola olarak.
1. Böğürtlenli simgesine tıklayın > **Tercihler** > **Raspberry Pi yapılandırma**.

   ![Raspbian Tercihler menüsü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Üzerinde **arabirimleri** sekmesinde, ayarlamak **I2C** ve **SSH** için **etkinleştirmek**ve ardından **Tamam**. Fiziksel algılayıcılar varsa ve benzetimli algılayıcı verilerini kullanmak istediğiniz yok, bu adım isteğe bağlıdır.

   ![I2C ve Böğürtlenli Pi üzerinde SSH etkinleştir](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
SSH ve I2C etkinleştirmek için daha fazla başvuru belgeleri bulabilirsiniz [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Pi algılayıcı Bağlan

Bir LED ve bir BME280 Pi şu şekilde bağlanmak için breadboard ve anahtar kablolarını kullanır. Algılayıcı yoksa [Bu bölüm atlayın](#connect-pi-to-the-network).

![Raspberry Pi'yi ve algılayıcı bağlantısı](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

BME280 algılayıcı sıcaklık ve nem verileri toplayabilir. Ve cihaz ve bulut arasındaki iletişimi ise LED blink. 

Algılayıcı PIN'ler için aşağıdaki kablolama kullanın:

| Başlangıç (algılayıcı & LED)     | Bitiş (kartı)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 5G)             | 3, 3v PWR (PIN 1)       | Beyaz kablosu   |
| GND (PIN 7G)             | GND (PIN 6)            | Kahverengi kablosu   |
| SDI (PIN 10G)            | I2C1 SDA (PIN 3)       | Kırmızı kablosu     |
| SCK (PIN 8G)             | I2C1 SCL (PIN 5)       | Turuncu kablosu  |
| LED VDD (PIN 18F)        | GPIO'yu 24 (PIN 18)       | Beyaz kablosu   |
| LED GND (PIN 17F)        | GND (PIN 20)           | Siyah kablosu   |

Görüntülemek için tıklatın [Raspberry Pi 2 ve 3 PIN eşlemeleri](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) daha sonra başvurmak üzere.

Raspberry Pi'yi BME280 başarıyla bağlandıktan sonra görüntü gibi olmalıdır.

![Bağlı Pi ve BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi ağa bağlanın

Pi üzerinde mikro USB kablosu ve güç kaynağı kullanarak açın. Pi kablolu ağa bağlanın veya izleyin için Ethernet kablosu kullanın [Raspberry Pi Foundation yönergeleri](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi kablosuz ağınıza bağlanmak için. Not yapmanıza gerek, Pi ağa başarıyla bağlandı sonra [IP adresi, pi'nin](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Kablolu ağa bağlı](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Pi bilgisayarınızın aynı ağa bağlı olduğundan emin olun. Örneğin, pi kablolu bir ağa bağlıyken, bilgisayarınızın kablosuz bir ağa bağlıysa, IP adresi devdisco çıkışı göremeyebilirsiniz.

## <a name="run-a-sample-application-on-pi"></a>Pi üzerinde bir örnek uygulamayı çalıştırın

### <a name="install-the-prerequisite-packages"></a>Önkoşul yükleme

Raspberry Pi'yi bağlanmak için ana bilgisayarınız aşağıdaki SSH istemcilerden birini kullanın.
   
   **Windows kullanıcıları**
   1. İndirme ve yükleme [PuTTY](http://www.putty.org/) Windows için. 
   1. Ana bilgisayar adı (veya IP adresi) içine Pi bölümünüzü IP adresini kopyalayın ve SSH bağlantı türü olarak seçin.
   
   
   **Mac ve Ubuntu kullanıcılar**
   
   Yerleşik SSH istemcisi Ubuntu veya macOS kullanın. Çalıştırmanız gerekebilir `ssh pi@<ip address of pi>` Pi SSH aracılığıyla bağlanmak için.
   > [!NOTE] 
   Varsayılan kullanıcı adı `pi` , ve parola `raspberry`.


### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Örnek uygulama, aşağıdaki komutu çalıştırarak kopyalama:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Yapılandırma dosyası, aşağıdaki komutları çalıştırarak açın:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   5 makroları vardır bu dosyada configurate olabilir. İlk sağlayıcıdır `MESSAGE_TIMESPAN`, bulut göndermek iki ileti süre (milisaniye cinsinden) tanımlar. İkincisi `SIMULATED_DATA`, benzetimli algılayıcı verilerini veya kullanıp kullanmayacağınızı için bir Boolean değeri değil. `I2C_ADDRESS` BME280 algılayıcı bağlı I2C adresidir. `GPIO_PIN_ADDRESS` için LED GPIO'yu adresidir. Son sunucudur `BLINK_TIMESPAN`, milisaniye cinsinden, LED açıldığında timespan tanımlı.

   Varsa, **algılayıcı yok**ayarlayın `SIMULATED_DATA` değeri `True` oluşturma ve sanal algılayıcı verilerini kullanma örnek uygulamayı yapmak için.

1. Kaydedip çıkmak denetimi O tuşlarına basarak > Enter > Denetim X.

### <a name="build-and-run-the-sample-application"></a>Derleme ve örnek uygulamayı çalıştırma

1. Örnek uygulama, aşağıdaki komutu çalıştırarak oluşturun. Python için Azure IOT SDK'ları Azure IOT cihaz C SDK'sı üstünde sarmalayıcıları olduğundan, istediğiniz veya Python kitaplıkları kaynak kodundan üretmek ihtiyacınız varsa C kitaplıkları derlemek gerekir.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   İstediğiniz çalıştırarak sürümü de belirtebilirsiniz `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Betik parametresi olmadan çalıştırırsanız, komut dosyası yüklü python sürümü otomatik olarak algılar (arama sırasını 2.7 -> 3.4 3.5 ->). Oluşturma ve çalıştırma sırasında Python sürümünüzü tutarlı tutar emin olun. 
   
   > [!NOTE] 
   Daha az en az 1 GB RAM içeren Linux cihazlarda oluşturma Python istemci kitaplığına (iothub_client.so) görebilirsiniz %98 aşağıda gösterildiği gibi iothub_client_python.cpp oluşturulurken takılmış derleme `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Bu sorunu yaşayıp çalıştırırsanız, kullanarak aygıt bellek tüketimini denetleme `free -m command` bu süre boyunca başka bir terminal penceresi içinde. Bellek yetersiz iothub_client_python.cpp dosyası derlenirken çalıştırıyorsanız, geçici olarak başarıyla Python istemci-tarafı cihaz SDK'sı kitaplığı oluşturmak için daha fazla kullanılabilir bellek almak için takas alanı artırmanız gerekebilir.
   
1. Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kopyalama cihaz bağlantı dizesi tek tırnak yapıştırma emin olun. Ve python 3 kullanın, sonra komutu kullanabilirsiniz, `python3 app.py '<your Azure IoT hub device connection string>'`.


   Algılayıcı verilerini ve IOT hub'ına gönderilen iletileri gösterir aşağıdaki çıktı görmeniz gerekir.

   ![Çıktı - Raspberry Pi'yi IOT hub'ına gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ınıza göndermek için örnek bir uygulamayı çalıştırdıktan. Bir komut satırı arabirimi içinde Raspberry Pi'yi, IOT hub'ı veya gönderme iletileri Raspberry Pi'yi tarafından gönderilen iletileri görmek için bkz: [iothub-explorer eğitici Mesajlaşma Yönet bulut cihaz](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
