---
title: Buluta (Node.js) - Azure IOT Hub'ına bağlanmak Raspberry Pi'yi Böğürtlenli Pi | Microsoft Docs
description: Kurulum ve Raspberry Pi'yi'nın bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub ile Raspberry Pi'yi bağlanma hakkında bilgi edinin.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: Azure IOT Böğürtlenli PI Böğürtlenli PI IOT hub, bulut için Böğürtlenli pi gönderme, verileri buluta Böğürtlenli pi
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e7db2f78a5c1a942f64a2c0a40068fffe90749d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Azure IOT Hub (Node.js) Böğürtlenli Pi Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspbian çalıştıran Raspberry Pi'yi ile çalışmanın temelleri öğrenerek başlayın. Daha sonra sorunsuzca aygıtlarınızı buluta kullanarak bağlanmasına nasıl öğrenin [Azure IOT Hub](iot-hub-what-is-iot-hub.md). Windows 10 IOT Core örnekleri için Git [Windows Geliştirme Merkezi](http://www.windowsondevices.com/).

Bir pakete henüz yok mu? Deneyin [Raspberry Pi'yi çevrimiçi simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Veya yeni bir paket satın [burada](https://azure.microsoft.com/develop/iot/starter-kits).


## <a name="what-you-do"></a>Neler

* IOT hub'ı oluşturun.
* Bir cihaz IOT hub'ınıza Pi için kaydetme.
* Raspberry Pi'yi ayarlayın.
* Pi algılayıcı verilerini IOT hub'ınıza gönderilecek örnek bir uygulamayı çalıştırın.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizenizi elde etme.
* Pi BME280 algılayıcı ile bağlanma.
* Pi üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza algılayıcı verileri göndermek nasıl.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* Raspberry Pi 2 veya Raspberry Pi 3 Panosu.
* Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir izleyici, bir USB klavye ve Pi bağlanan fare.
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
İsteğe bağlı öğeleri yoksa, benzetimli algılayıcı verilerini kullanabilirsiniz.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi'yi ayarlayın

### <a name="install-the-raspbian-operating-system-for-pi"></a>Pi için Raspbian işletim sistemini yükleme

MicroSD kartı Raspbian görüntünün yüklenmesi için hazırlayın.

1. Raspbian indirin.
   1. [Raspbian Esnetme karşıdan](http://downloads.raspberrypi.org/raspbian/images/raspbian-2017-07-05/) (.zip dosyası).

   > [!WARNING]
   > Lütfen indirmek için yukarıdaki bağlantıyı kullanın `raspbian-2017-07-5` zip resmi. En son sürümünü Raspbian görüntüleri, sonraki adımlarda hatasına neden olabilen kablolama Pi düğüm bilinen bazı sorunlar vardır.
   1. Raspbian görüntünün bilgisayarınızdaki bir klasöre ayıklayın.

1. Raspbian microSD kartı yükleyin.
   1. [Etcher SD kart yazıcı yardımcı programı yükleyip](https://etcher.io/).
   1. Etcher çalıştırın ve 1. adımda ayıkladığınız Raspbian görüntüyü seçin.
   1. MicroSD kartı sürücü seçin. Etcher zaten doğru sürücü seçmiş olabilirsiniz.
   1. Raspbian microSD kartı yüklemek için Flash'ı tıklatın.
   1. Yükleme tamamlandığında microSD kartı bilgisayarınızdan kaldırın. Etcher otomatik olarak çıkarır veya tamamlanmasından sonra microSD kartı çıkarır çünkü microSD kartı doğrudan kaldırmak güvenlidir.
   1. MicroSD kartı Pi yerleştirin.

### <a name="enable-ssh-and-i2c"></a>SSH ve I2C etkinleştir

1. Pi monitör, klavye ve fare bağlayın. 
1. Pi başlatın ve ardından içinde Raspbian kullanarak oturum `pi` kullanıcı adı ve `raspberry` ve parola olarak.
1. Böğürtlenli simgesine tıklayın > **Tercihler** > **Raspberry Pi yapılandırma**.

   ![Raspbian Tercihler menüsü](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. Üzerinde **arabirimleri** sekmesinde, ayarlamak **I2C** ve **SSH** için **etkinleştirmek**ve ardından **Tamam**. Fiziksel algılayıcılar varsa ve benzetimli algılayıcı verilerini kullanmak istediğiniz yok, bu adım isteğe bağlıdır.

   ![I2C ve Böğürtlenli Pi üzerinde SSH etkinleştir](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
SSH ve I2C etkinleştirmek için daha fazla başvuru belgeleri bulabilirsiniz [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Pi algılayıcı Bağlan

Bir LED ve bir BME280 Pi şu şekilde bağlanmak için breadboard ve anahtar kablolarını kullanır. Algılayıcı yoksa [Bu bölüm atlayın](#connect-pi-to-the-network).

![Raspberry Pi'yi ve algılayıcı bağlantısı](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

BME280 algılayıcı sıcaklık ve nem verileri toplayabilir. Cihaz buluta ileti gönderdiğinde ışığı yanıp. 

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

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Örnek uygulama kopyalamak ve önkoşul yükleyin

1. Aşağıdaki SSH istemcilerini biri ile Raspberry Pi'yi ana bilgisayardan bağlanın:
   
   **Windows kullanıcıları**
   1. İndirme ve yükleme [PuTTY](http://www.putty.org/) Windows için. 
   1. Ana bilgisayar adı (veya IP adresi) içine Pi bölümünüzü IP adresini kopyalayın ve SSH bağlantı türü olarak seçin.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac ve Ubuntu kullanıcılar**
   
   Yerleşik SSH istemcisi Ubuntu veya macOS kullanın. Çalıştırmanız gerekebilir `ssh pi@<ip address of pi>` Pi SSH aracılığıyla bağlanmak için.
   > [!NOTE] 
   Varsayılan kullanıcı adı `pi` ve parola `raspberry`.

1. Node.js ve NPM, Pi yükleyin.
   
   İlk Node.js sürümünü denetleyin. 
   
   ```bash
   node -v
   ```

   Sürüm 4.x düşükse veya, Pi üzerinde hiçbir Node.js ise, en son sürümünü yükleyin.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. Örnek uygulama kopyalayın.

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. Tüm paketler örnek yükleyin. Yükleme, Azure IOT cihaz SDK'sı, BME280 algılayıcı kitaplığı ve geriye Pi kitaplığı içerir.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   Ağ bağlantınızın bağlı olarak bu yükleme işleminin tamamlanması birkaç dakika sürebilir.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Yapılandırma dosyası, aşağıdaki komutları çalıştırarak açın:

   ```bash
   nano config.json
   ```

   ![Yapılandırma dosyası](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Yapılandırabileceğiniz bu dosyada iki öğe yok. İlk sağlayıcıdır `interval`, buluta gönderilen iletileri arasındaki zaman aralığı (milisaniye cinsinden) tanımlar. İkincisi olan `simulatedData`, benzetimli algılayıcı verilerini veya kullanıp kullanmayacağınızı için bir Boolean değeri değil.

   Varsa, **algılayıcı yok**ayarlayın `simulatedData` değeri `true` oluşturma ve sanal algılayıcı verilerini kullanma örnek uygulamayı yapmak için.

1. Kaydedip çıkmak denetimi O yazarak > Enter > Denetim X.

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Kopyalama cihaz bağlantı dizesi tek tırnak yapıştırma emin olun.


Algılayıcı verilerini ve IOT hub'ına gönderilen iletileri gösterir aşağıdaki çıktı görmeniz gerekir.

![Çıktı - Raspberry Pi'yi IOT hub'ına gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ınıza göndermek için örnek bir uygulamayı çalıştırdıktan. Raspberry Pi'yi IOT hub'ınıza gönderdiği iletileri görmek veya bir komut satırı arabirimi içinde Raspberry Pi'yi iletileri göndermek için bkz: [iothub-explorer eğitici Mesajlaşma Yönet bulut cihaz](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
