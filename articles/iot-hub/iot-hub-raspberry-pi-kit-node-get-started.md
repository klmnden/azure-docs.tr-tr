---
title: Bulut (Node.js) - Azure IOT hub'a bağlanma Raspberry Pi'yi raspberry Pi | Microsoft Docs
description: Kurulum ve Raspberry Pi Raspberry Pi, bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT hub'a bağlanma hakkında bilgi edinin.
author: rangv
manager: ''
keywords: Azure IOT raspberry pi, raspberry pi IOT hub, bulut verilerini raspberry pi göndermek, buluta raspberry pi
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 0db28fb8016176bdd66e5406a6f1c0a18cc5c3e8
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58847585"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Raspberry Pi (Node.js) Azure IOT hub'a bağlanma

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspbian çalıştıran Raspberry Pi çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra kullanarak cihazlarınızı buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](about-iot-hub.md). Windows 10 IoT Core örnekleri için Git [Windows Dev Center](https://www.windowsondevices.com/).

Bir paket henüz yok mu? Deneyin [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md). Veya yeni bir paket satın [burada](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Neler

* IOT hub oluşturun.
* Bir cihaz IOT hub'ına PI sayısı için kaydedin.
* Raspberry Pi ' ayarlayın.
* Pi sensör verilerini IOT hub'ınıza gönderilecek örnek uygulamayı çalıştırın.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizesini almak nasıl.
* Pi BME280 algılayıcısı ile bağlanma.
* Pi üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](./media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* Raspberry Pi 2 veya Raspberry Pi 3 Pano.
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Bir izleyici, bir USB klavye ve fare Pi bağlanan.
* Bir Mac veya Windows veya Linux çalıştıran bir bilgisayar.
* İnternet bağlantısı.
* 16 GB veya üzeri microSD kartı.
* İşletim sistemi görüntüsünü microSD kartı üzerine yazmak için bir USB SD bağdaştırıcı veya microSD kartı.
* 5-volt 2 amp power 6 ft mikro USB kablosu ile sağlayın.

Aşağıdaki öğeler isteğe bağlıdır:

* Bir birleştirilmiş Adafruit BME280 sıcaklık, baskısı ve nem algılayıcı.
* Bir breadboard.
* 6 F/dk anahtar kablolarına.
* Yayılmış 10 mm LED.

> [!NOTE] 
> İsteğe bağlı öğeleri yoksa sanal sensör verilerini kullanabilirsiniz.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi ' ayarlayın

### <a name="install-the-raspbian-operating-system-for-pi"></a>PI sayısı için Raspbian işletim sistemini yükleme

MicroSD kartı Raspbian görüntüyü yüklemesi için hazırlayın.

1. Raspbian indirin.

   a. [Raspbian Esnetme indirme](https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-07-05/) (.zip dosyasında).

   > [!WARNING]
   > Lütfen indirmek için yukarıdaki bağlantıya kullanın `raspbian-2017-07-5` zip görüntü. En son sürümünü Raspbian görüntüleri, sonraki adımlarda hatasına neden olabilecek kablo-Pi düğüm ile ilgili bazı bilinen sorunlar vardır.

   b. Raspbian görüntünün bilgisayarınızdaki bir klasöre ayıklayın.

2. Raspbian microSD kartı yükleyin.

   a. [Etcher SD kart yazıcı yardımcı programını yükleyip](https://etcher.io/).

   b. Etcher çalıştırın ve 1. adımda ayıkladığınız Raspbian görüntüsünü seçin.

   c. MicroSD kartı sürücüyü seçin. Etcher zaten doğru sürücü seçmiş olabilirsiniz.

   d. MicroSD kartı Raspbian yüklemek için Flash'ı tıklatın.

   e. Yükleme tamamlandığında microSD kartı bilgisayarınızdan kaldırın. Etcher otomatik olarak çıkarır veya tamamlandıktan sonra microSD kartı çıkarır çünkü microSD kartı doğrudan kaldırmak güvenlidir.

   f. MicroSD kartı PI yerleştirin.

### <a name="enable-ssh-and-i2c"></a>SSH ve I2C etkinleştir

1. Pi monitör, klavye ve fare bağlayın. 

2. Pi başlatın ve ardından içinde Raspbian kullanarak oturum `pi` kullanıcı adı ve `raspberry` parolası.

3. Raspberry simgesine tıklayın > **tercihleri** > **Raspberry Pi yapılandırma**.

   ![Raspbian tercihleri menüsü](./media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

4. Üzerinde **arabirimleri** sekmesinde, belirleyin **I2C** ve **SSH** için **etkinleştirme**ve ardından **Tamam**. Yoksa ve fiziksel algılayıcınız varsa sanal sensör verilerini kullanmak istiyorsanız, bu adım isteğe bağlıdır.

   ![I2C ve Raspberry Pi üzerinde SSH'ı etkinleştir](./media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
> SSH ve I2C etkinleştirmek için daha fazla başvuru belgeleri üzerinde bulabilirsiniz [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Pi algılayıcı bağlanma

Bir LED'i ve bir BME280 Pi gibi bağlanmak için breadboard ve anahtar kablo kullanın. Algılayıcı yoksa [bu bölümü atlayın](#connect-pi-to-the-network).

![Raspberry Pi ve algılayıcı bağlantı](./media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

BME280 algılayıcı sıcaklık ve nem veri toplayabilir. Cihaz buluta ileti gönderdiğinde LED yanıp. 

Algılayıcı sabitlemek için aşağıdaki bağlantı kullanın:

| Başlangıç (sensör ve LED)     | Bitiş (Pano)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN 5G)             | 3, 3v PWR (PIN 1)       | Beyaz kablosu   |
| GND (Pin 7G)             | GND (PIN 6)            | Kahverengi kablosu   |
| SDI (Pin 10G)            | I2C1 SDA (PIN 3)       | Kırmızı kablosu     |
| SCK (PIN 8G)             | I2C1 SCL (PIN 5)       | Turuncu kablosu  |
| LED VDD (PIN 18F)        | GPIO 24 (Pin 18)       | Beyaz kablosu   |
| LED GND (PIN 17F)        | GND (PIN 20)           | Siyah kablo   |

Görüntülemek için tıklayın [Raspberry Pi 2 ve 3 PIN eşlemeleri](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referans.

Raspberry Pi'yi BME280 başarıyla bağlandıktan sonra aşağıdaki resim gibi olmalıdır.

![Bağlı Pi ve BME280](./media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi ağa bağlanma

Pi üzerinde mikro USB kablosu ve güç kaynağı kullanarak etkinleştirin. Pi, kablolu ağa bağlanacak veya takip için Ethernet kablosu kullanın [yönergeleri Raspberry Pi Foundation'dan](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi, kablosuz ağa bağlanmak için. Bir not gerek Pi'yi ağa başarıyla bağlandı sonra [Pi'yi IP adresini](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Kablolu ağa bağlı](./media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Pi bilgisayarınızı ile aynı ağa bağlı olduğundan emin olun. Örneğin, bilgisayarınız kablosuz ağa bağlıysa, PI kablolu bir ağa bağlıyken devdisco çıkış IP adresini göremeyebilirsiniz.

## <a name="run-a-sample-application-on-pi"></a>Pi üzerinde bir örnek uygulamayı çalıştırma

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Örnek uygulamayı kopyalayın ve önkoşul paketleri yükleme

1. Raspberry Pi'yi aşağıdaki SSH istemcisi biriyle, ana bilgisayardan bağlanın:

   **Windows kullanıcıları**
  
   a. İndirme ve yükleme [PuTTY](https://www.putty.org/) Windows için. 

   b. IP adresini, ana bilgisayar adı (veya IP adresi) PI bölümüne kopyalayın ve SSH bağlantı türü olarak seçin.

   ![PuTTy](./media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)

   **Mac ve Ubuntu kullanıcıları**

   Yerleşik bir SSH istemcisi, Ubuntu veya Macos'ta kullanın. Çalıştırmanız gerekebilir `ssh pi@<ip address of pi>` Pi SSH aracılığıyla bağlanmak için.

   > [!NOTE] 
   > Varsayılan kullanıcı adı `pi` ve parola `raspberry`.

2. Node.js ve NPM için Pi'yi yükleyin.

   İlk Node.js sürümünüzü kontrol edin. 

   ```bash
   node -v
   ```

   Sürüm 4.x'ten düşük ise veya Pi'yi üzerinde hiçbir Node.js ise, en son sürümünü yükleyin.

   ```bash
   curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

3. Örnek uygulamayı kopyalayın.

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

4. Tüm paketleri örneği için yükleyin. Yükleme, Azure IOT cihaz SDK'sını ve BME280 algılayıcı kitaplığı PI bağlama kitaplığı içerir.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```

   > [!NOTE] 
   >İşlem, ağ bağlantısı bağlı olarak bu yükleme işleminin tamamlanması birkaç dakika sürebilir.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Yapılandırma dosyası, aşağıdaki komutları çalıştırarak açın:

   ```bash
   nano config.json
   ```

   ![Yapılandırma dosyası](./media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Yapılandırabileceğiniz bu dosyada iki öğe yok. İlki `interval`, buluta gönderilen iletileri arasındaki zaman aralığını (milisaniye cinsinden) tanımlar. İkincisi olan `simulatedData`, olduğu için veya sanal sensör verilerini kullanıp kullanmayacağınızı bir Boolean değeri.

   Varsa, **algılayıcı yok**ayarlayın `simulatedData` değerini `true` örnek uygulama oluşturun ve sanal sensör verilerini kullanın.

2. Kaydet ve Çık denetimi-O yazarak > Enter > X denetimi.

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   > Kopyalama cihaz bağlantı dizesini tek tırnak yapıştırma emin olun.


Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir.

![Çıkış - IOT hub'ınıza Raspberry Pi'dan gönderilen algılayıcı verileri](./media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub'ınıza tarafından alınan iletileri okuma

Cihazınızdan IOT hub tarafından alınan iletileri izlemeye yönelik bir yolu, Visual Studio Code için Azure IOT araçları kullanmaktır. Daha fazla bilgi için bkz. [göndermek ve IOT Hub ve cihaz arasında iletileri almak Visual Studio Code için Azure IOT Araçları](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Cihazınız tarafından gönderilen verileri işlemek daha fazla yolu için açın sonraki bölüme devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
