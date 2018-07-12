---
title: Bulut (Python) - Azure IOT hub'a bağlanma Raspberry Pi'yi raspberry Pi | Microsoft Docs
description: Kurulum ve Raspberry Pi Raspberry Pi, bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT hub'a bağlanma hakkında bilgi edinin.
author: rangv
manager: ''
keywords: Azure IOT raspberry pi, raspberry pi IOT hub, bulut verilerini raspberry pi göndermek, buluta raspberry pi
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 15b917d811dff2d70b638ab82c808bdc7fa1012b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38235738"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-python"></a>Azure IOT hub'a (Python) Raspberry Pi bağlama

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspbian çalıştıran Raspberry Pi çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra kullanarak cihazlarınızı buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](iot-hub-what-is-iot-hub.md). Windows 10 IoT Core örnekleri için Git [Windows Dev Center](http://www.windowsondevices.com/).

Bir paket henüz yok mu? Deneyin [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md). Veya yeni bir paket satın [burada](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Neler

* IOT hub oluşturun.
* Bir cihaz IOT hub'ına PI sayısı için kaydedin.
* Raspberry Pi ayarlayın.
* Pi sensör verilerini IOT hub'ınıza gönderilecek örnek uygulamayı çalıştırın.

Raspberry Pi'yi oluşturduğunuz IOT hub'a bağlayın. Ardından BME280 algılayıcıdan sıcaklık ve nem verileri toplamak için Pi üzerinde bir örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizesini almak nasıl.
* Pi BME280 algılayıcısı ile bağlanma.
* Pi üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* Raspberry Pi 2 veya Raspberry Pi 3 Pano.
* Etkin bir Azure aboneliği. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir izleyici, bir USB klavye ve fare Pi bağlanan.
* Bir Mac veya Windows veya Linux çalıştıran bir bilgisayar.
* Internet bağlantısı.
* 16 GB veya üzeri microSD kartı.
* İşletim sistemi görüntüsünü microSD kartı üzerine yazmak için bir USB SD bağdaştırıcı veya microSD kartı.
* 5-volt 2 amp power 6 ft mikro USB kablosu ile sağlayın.

Aşağıdaki öğeler isteğe bağlıdır:

* Bir birleştirilmiş Adafruit BME280 sıcaklık, baskısı ve nem algılayıcı.
* Bir breadboard.
* 6 F/dk anahtar kablolarına.
* Yayılmış 10 mm LED.


> [!NOTE] 
Kod örneği desteği sensör verilerini benzetimli çünkü bu öğeler isteğe bağlıdır.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi ' ayarlayın

### <a name="install-the-raspbian-operating-system-for-pi"></a>PI sayısı için Raspbian işletim sistemini yükleme

MicroSD kartı Raspbian görüntüyü yüklemesi için hazırlayın.

1. Raspbian indirin.
   1. [Masaüstü ile Raspbian Jessie indirme](https://www.raspberrypi.org/downloads/raspbian/) (.zip dosyasında).
   1. Raspbian görüntünün bilgisayarınızdaki bir klasöre ayıklayın.
1. Raspbian microSD kartı yükleyin.
   1. [Etcher SD kart yazıcı yardımcı programını yükleyip](https://etcher.io/).
   1. Etcher çalıştırın ve 1. adımda ayıkladığınız Raspbian görüntüsünü seçin.
   1. MicroSD kartı sürücüyü seçin. Etcher zaten doğru sürücü seçmiş olabilirsiniz olduğunu unutmayın.
   1. MicroSD kartı Raspbian yüklemek için Flash'ı tıklatın.
   1. Yükleme tamamlandığında microSD kartı bilgisayarınızdan kaldırın. Etcher otomatik olarak çıkarır veya tamamlandıktan sonra microSD kartı çıkarır çünkü microSD kartı doğrudan kaldırmak güvenlidir.
   1. MicroSD kartı PI yerleştirin.

### <a name="enable-ssh-and-i2c"></a>SSH ve I2C etkinleştir

1. Pi monitör, klavye ve fare bağlayın, PI başlatın ve ardından içinde Raspbian kullanarak oturum `pi` kullanıcı adı ve `raspberry` parolası.
1. Raspberry simgesine tıklayın > **tercihleri** > **Raspberry Pi yapılandırma**.

   ![Raspbian tercihleri menüsü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Üzerinde **arabirimleri** sekmesinde, belirleyin **I2C** ve **SSH** için **etkinleştirme**ve ardından **Tamam**. Yoksa ve fiziksel algılayıcınız varsa sanal sensör verilerini kullanmak istiyorsanız, bu adım isteğe bağlıdır.

   ![I2C ve Raspberry Pi üzerinde SSH'ı etkinleştir](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
SSH ve I2C etkinleştirmek için daha fazla başvuru belgeleri üzerinde bulabilirsiniz [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Pi algılayıcı bağlanma

Bir LED'i ve bir BME280 Pi gibi bağlanmak için breadboard ve anahtar kablo kullanın. Algılayıcı yoksa [bu bölümü atlayın](#connect-pi-to-the-network).

![Raspberry Pi ve algılayıcı bağlantı](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

BME280 algılayıcı sıcaklık ve nem veri toplayabilir. Ve varsa cihaz ve bulut arasındaki iletişimi LED yanıp sönme. 

Algılayıcı sabitlemek için aşağıdaki bağlantı kullanın:

| Başlangıç (sensör ve LED)     | Bitiş (Pano)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 5G)             | 3, 3v PWR (PIN 1)       | Beyaz kablosu   |
| GND (PIN 7G)             | GND (PIN 6)            | Kahverengi kablosu   |
| SDI (PIN 10G)            | I2C1 SDA (PIN 3)       | Kırmızı kablosu     |
| SCK (PIN 8G)             | I2C1 SCL (PIN 5)       | Turuncu kablosu  |
| LED VDD (PIN 18F)        | Bir GPIO'yu 24 (PIN 18)       | Beyaz kablosu   |
| LED GND (PIN 17F)        | GND (PIN 20)           | Siyah kablo   |

Görüntülemek için tıklayın [Raspberry Pi 2 ve 3 PIN eşlemeleri](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referans.

Raspberry Pi'yi BME280 başarıyla bağlandıktan sonra aşağıdaki resim gibi olmalıdır.

![Bağlı Pi ve BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi ağa bağlanma

Pi üzerinde mikro USB kablosu ve güç kaynağı kullanarak etkinleştirin. Pi, kablolu ağa bağlanacak veya takip için Ethernet kablosu kullanın [yönergeleri Raspberry Pi Foundation'dan](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi, kablosuz ağa bağlanmak için. Bir not gerek Pi'yi ağa başarıyla bağlandı sonra [Pi'yi IP adresini](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Kablolu ağa bağlı](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Pi bilgisayarınızı ile aynı ağa bağlı olduğundan emin olun. Örneğin, bilgisayarınız kablosuz ağa bağlıysa, PI kablolu bir ağa bağlıyken devdisco çıkış IP adresini göremeyebilirsiniz.

## <a name="run-a-sample-application-on-pi"></a>Pi üzerinde bir örnek uygulamayı çalıştırma

### <a name="install-the-prerequisite-packages"></a>Önkoşul paketleri yükleme

Raspberry Pi'yi bağlanmak için ana bilgisayarınız aşağıdaki SSH istemcilerden birini kullanın.
   
   **Windows kullanıcıları**
   1. İndirme ve yükleme [PuTTY](http://www.putty.org/) Windows için. 
   1. IP adresini, ana bilgisayar adı (veya IP adresi) PI bölümüne kopyalayın ve SSH bağlantı türü olarak seçin.
   
   
   **Mac ve Ubuntu kullanıcıları**
   
   Yerleşik bir SSH istemcisi, Ubuntu veya Macos'ta kullanın. Çalıştırmanız gerekebilir `ssh pi@<ip address of pi>` Pi SSH aracılığıyla bağlanmak için.
   > [!NOTE] 
   Varsayılan kullanıcı adı `pi` , ve parola `raspberry`.


### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Aşağıdaki komutu çalıştırarak örnek uygulamayı kopyalayın:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Yapılandırma dosyası, aşağıdaki komutları çalıştırarak açın:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   5 makroları vardır. Bu dosyada configurate olabilir. İlki `MESSAGE_TIMESPAN`, buluta gönderin, iki ileti zaman aralığını (milisaniye cinsinden) tanımlar. İkincisi `SIMULATED_DATA`, olduğu için veya sanal sensör verilerini kullanıp kullanmayacağınızı bir Boolean değeri. `I2C_ADDRESS` BME280 algılayıcı bağlı I2C adresidir. `GPIO_PIN_ADDRESS` IŞIĞI için GPIO adresidir. Sonuncu ise `BLINK_TIMESPAN`, milisaniye cinsinden, ışığı açıldığında timespan tanımlanmış.

   Varsa, **algılayıcı yok**ayarlayın `SIMULATED_DATA` değerini `True` örnek uygulama oluşturun ve sanal sensör verilerini kullanın.

1. Kaydet ve Çık denetimi-O tuşlarına basarak > Enter > X denetimi.

### <a name="build-and-run-the-sample-application"></a>Örnek uygulaması derleme ve çalıştırma

1. Örnek uygulama, aşağıdaki komutu çalıştırarak oluşturun. Azure IOT SDK'ları Python için Azure IOT cihaz C SDK'sı üzerinde sarmalayıcıları olduğundan, istediğiniz veya kaynak kodundan Python kitaplıkları oluşturmak için gereksinim duyduğunuz C kitaplıklarını derlemeniz gerekir.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   Çalıştırarak istediğiniz sürüm belirtebilirsiniz `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Betik parametresi olmadan çalıştırırsanız, betiğin python'un yüklü sürümünü otomatik olarak algılar (arama sırası 2.7 -> 3.4 3.5 ->). Oluşturma ve çalıştırma sırasında Python sürümünüzdür tutarlı tutar emin olun. 
   
   > [!NOTE] 
   Görebileceğiniz Python istemci Kitaplığı'nı (iothub_client.so) 1 GB'tan az RAM'i olan Linux cihazlarında oluşturmaya, aşağıda gösterildiği gibi iothub_client_python.cpp oluşturulurken %98 takılı derleme `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Bu sorunu çalıştırırsanız, cihaz kullanarak bellek tüketimini denetleyin `free -m command` bu süre boyunca başka bir terminal penceresinde. Bellek yetersiz iothub_client_python.cpp dosyası derlenirken çalıştırıyorsanız, geçici olarak başarıyla Python istemci tarafı cihaz SDK'sı kitaplığı oluşturmak için daha fazla kullanılabilir bellek almak için takas alanı artırmak olabilir.
   
1. Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kopyalama cihaz bağlantı dizesini tek tırnak yapıştırma emin olun. Python 3 kullandığınız sonra komutunu kullanabilirsiniz `python3 app.py '<your Azure IoT hub device connection string>'`.


   Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir.

   ![Çıkış - IOT hub'ınıza Raspberry Pi'dan gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama çalıştırdınız. Raspberry Pi'yi Raspberry Pi'yi bir komut satırı arabirimi, IOT hub'ı veya gönderme iletilerini gönderilen iletileri görmek için bkz: [Yönet bulut cihaz iothub-explorer öğreticiyle Mesajlaşma](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
