---
title: Bulut (C) - Intel Edison'u Azure IOT Hub'ına bağlanmak için Intel Edison'u | Microsoft Docs
description: Kurulum ve Intel Edison'u'nın bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT Hub Intel Edison'u bağlanma hakkında bilgi edinin.
services: iot-hub
documentationcenter: ''
author: shizn
manager: timlt
tags: ''
keywords: Azure IOT Intel edison'u, Intel edison'u IOT hub, Intel edison'u buluta, Intel veri Gönder edison'u bulut
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: edbdbe0230f742cd7228f04a4a83c9bd567527e8
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="connect-intel-edison-to-azure-iot-hub-c"></a>Intel Edison'u Azure IoT Hub (C) Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Intel Edison'u ile çalışmanın temelleri öğrenerek başlayın. Daha sonra sorunsuzca aygıtlarınızı buluta kullanarak bağlanmasına nasıl öğrenin [Azure IOT Hub](iot-hub-what-is-iot-hub.md).

Bir pakete henüz yok mu? Başlat [burada](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Neler

* Intel Edison'u Kurulum ve ve Grove modüller.
* IOT hub'ı oluşturun.
* Bir cihaz IOT hub'ınıza Edison'u için kaydetme.
* IOT hub'ınıza algılayıcı verileri göndermek için Edison'u bir örnek uygulamayı çalıştırın.

Intel Edison'u, oluşturduğunuz bir IOT hub'ına bağlanın. Ardından, örnek bir uygulama Grove sıcaklık algılayıcısı sıcaklık ve nem verileri toplamak için Edison'u çalıştırın. Son olarak, IOT hub'ınıza algılayıcı verileri gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizenizi elde etme.
* Edison'u Grove sıcaklık algılayıcısı ile bağlanma.
* Örnek bir uygulama üzerinde Edison'u çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza algılayıcı verileri göndermek nasıl.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* Intel Edison'u Panosu
* Arduino genişletme kartı
* Etkin bir Azure aboneliği. Bir Azure hesabınız yoksa [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Mac veya Windows veya Linux çalıştıran bir bilgisayar.
* Bir Internet bağlantısı.
* Tür bir USB kablosu mikro B
* Bir doğrudan geçerli (DC) güç kaynağı. Güç kaynağı gibi derecelendirilmiş:
  - 7-15V DC
  - En az 1500mA
  - Merkezi/iç PIN güç kaynağı, pozitif kutbu'na olmalıdır

Aşağıdaki öğeler isteğe bağlıdır:

* Grove temel kalkan V2
* Grove - sıcaklık algılayıcısı
* Grove kablosu
* Bir ayırıcı çubukları veya genişletme kartı ve vida ve plastik boşluk ekleyiciler dört kümelerinin modülü fasten iki Vida dahil olmak üzere paketinde bulunan Vida.

> [!NOTE] 
Kod örneği desteği algılayıcı verilerini benzetimli çünkü bu öğeler isteğe bağlıdır.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Setup Intel Edison

### <a name="assemble-your-board"></a>Panonuzu birleştirin

Bu bölümde Intel® Edison'u modülünüzün genişletme panonuz ekleme adımlarını içerir.

1. Beyaz Anahat içinde Intel® Edison'u modülü Modül delik genişletme panosunda Vida ile hizalama genişletme panonuzu yerleştirin.

2. Modül sözcükleri hemen altındaki aşağı basın `What will you make?` bir ek düşündüğünüz kadar.

   ![Pano 2 birleştirin](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. (Pakete dahil) iki onaltılık nasıl genişletme Panosu modülü güvenliğini sağlamak için kullanın.

   ![Pano 3 birleştirin](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. Genişletme panosunda dört köşe delik birinde bir Vidayı ekleyin. Bük ve Vidayı üzerine Beyaz Plastik boşluk ekleyiciler birini sıkılaştırabilirsiniz.

   ![Pano 4 birleştirin](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. Diğer üç köşe boşluk ekleyiciler için yineleyin.

   ![Pano 5 birleştirin](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

Panonuz artık derlenip.

   ![Birleştirilmiş Panosu](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Grove Bankası kalkan ve sıcaklık algılayıcısı

1. Panonuzu açın Grove temel kalkan yerleştirin. Tüm PIN'ler panonuzu sıkıca takılı olduğundan emin olun.
   
   ![Grove temel kalkan](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. Grove kablo Grove sıcaklık algılayıcısı Grove temel kalkan üzerine bağlanmanız **A0** bağlantı noktası.

   ![Sıcaklık algılayıcısı Bağlan](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Edison'u ve algılayıcı bağlantısı](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

Artık, algılayıcı hazırdır.

### <a name="power-up-edison"></a>Power up Edison

1. Güç kaynağı takın.

   ![Güç kaynağı bağlama](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. (DS1 Arduino * genişletme panosunda etiketli) yeşil bir LED açık ve aydınlatılmış olmak gerekir.

3. Önyükleme işlemi sonlandırın panosu için bir dakika bekleyin.

   > [!NOTE]
   > DC güç kaynağı yoksa, hala bir USB bağlantı noktası üzerinden Panosu kapatabilirsiniz. Bkz: `Connect Edison to your computer` ayrıntıları bölümü. Özellikle Wi-Fi kullanarak veya yürüten motors zaman Panonuzu bu şekilde destekleyen, karttan beklenmeyen davranışlara neden olabilir.

### <a name="connect-edison-to-your-computer"></a>Edison'u bilgisayarınıza bağlayın

1. Böylece Edison'u aygıt modunda mikro düğme iki mikro USB bağlantı noktası doğru aşağı Değiştir. Cihaz mod ve ana mod arasındaki farklar için lütfen başvuru [burada](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Mikro düğme Değiştir](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. Mikro USB kablosu üst mikro USB bağlantı noktasına takın.

   ![Üst mikro USB bağlantı noktası](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. Bilgisayarınıza bir USB kablosu sonuna takın.

   ![Bilgisayar USB](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

4. Bilgisayarınızın yeni bir sürücüye (çok SD kart bilgisayarınıza ekleme gibi) bağladığında panonuzu tam olarak başlatılmadan bilirsiniz.

## <a name="download-and-run-the-configuration-tool"></a>Karşıdan yükleme ve yapılandırma aracını çalıştırın.
En son yapılandırma aracından alma [bu bağlantıyı](https://software.intel.com/en-us/iot/hardware/edison/downloads) altında listelenen `Installers` başlığı. Aracını Yürüt ve izleyin, ekrandaki yönergeleri, gerektiğinde İleri tıklatıldığında

### <a name="flash-firmware"></a>Bellenim flash
1. Üzerinde `Set up options` sayfasında, `Flash Firmware`.
2. Aşağıdakilerden birini yaparak panonuzu Flash görüntüyü seçin:
   - Karşıdan yükle ve panonuzu Intel'in kullanılabilir en son bellenim görüntüsü ile flash için seçin `Download the latest image version xxxx`.
   - Panonuzu, önceden kaydettiğiniz bilgisayarınızda bir görüntüyle flash için seçin `Select the local image`. Göz atın ve panonuz için flash istediğiniz görüntüyü seçin.
3. Kurulum aracı panonuzu flash dener. Tüm yanıp sönen işlem 10 dakika kadar sürebilir.

### <a name="set-password"></a>Parola belirleyin
1. Üzerinde `Set up options` sayfasında, `Enable Security`.
2. Intel® Edison'u panonuz için bir özel ad ayarlayabilirsiniz. Bu isteğe bağlıdır.
3. Panonuz için bir parola yazın ve ardından `Set password`.
4. Daha sonra kullanılan parola işaretleyin.

### <a name="connect-wi-fi"></a>Wi-Fi Bağlan
1. Üzerinde `Set up options` sayfasında, `Connect Wi-Fi`. Kadar bekleyin, bilgisayarınızın olarak bir dakika için kullanılabilen Wi-Fi ağlarına tarar.
2. Gelen `Detected Networks` aşağı açılan listesinde, ağınızı seçin.
3. Gelen `Security` aşağı açılan listesinde, ağın güvenlik türü seçin.
4. Oturum açma ve parola bilgilerinizi sağlayın ve ardından `Configure Wi-Fi`.
5. Daha sonra kullanılan IP adresi işaretleyin.

> [!NOTE]
> Edison'u bilgisayarınızın aynı ağa bağlı olduğundan emin olun. Bilgisayarınız, Edison'u IP adresini kullanarak bağlanır.

   ![Sıcaklık algılayıcısı Bağlan](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

Tebrikler! Edison'u başarıyla yapılandırdıktan.

## <a name="run-a-sample-application-on-intel-edison"></a>Intel Edison'u üzerinde bir örnek uygulamayı çalıştırın

### <a name="prepare-the-azure-iot-device-sdk"></a>Azure IOT cihaz SDK'sı hazırlama

1. Intel Edison'u bağlanmak için ana bilgisayarınız aşağıdaki SSH istemcilerden birini kullanın. IP adresi yapılandırması aracını ve bu aracında ayarladınız bir paroladır.
    - [PuTTY](http://www.putty.org/) Windows için.
    - Ubuntu veya macOS üzerindeki yerleşik SSH istemcisi (çalıştırmak `ssh root@"the IP address"`).

2. Cihazınızı örnek istemci uygulamaya kopyalayın. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. Azure IOT SDK'sı oluşturmak için aşağıdaki komutu çalıştırmak için depodaki klasöre gidin

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Yapılandırma dosyası, aşağıdaki komutları çalıştırarak açın:

   ```bash
   nano config.h
   ```

   ![Yapılandırma dosyası](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   İki makroları vardır bu dosyada configurate olabilir. İlk sağlayıcıdır `INTERVAL`, bulut göndermek iki ileti arasındaki zaman aralığını tanımlar. İkincisi `SIMULATED_DATA`, benzetimli algılayıcı verilerini veya kullanıp kullanmayacağınızı için bir Boolean değeri değil.

   Varsa, **algılayıcı yok**ayarlayın `SIMULATED_DATA` değeri `1` oluşturma ve sanal algılayıcı verilerini kullanma örnek uygulamayı yapmak için.

2. Kaydedip çıkmak denetimi O tuşlarına basarak > Enter > Denetim X.

### <a name="build-and-run-the-sample-application"></a>Derleme ve örnek uygulamayı çalıştırma

1. Örnek uygulama, aşağıdaki komutu çalıştırarak oluşturun:

   ```bash
   cmake . && make
   ```
   ![Çıktı derleme](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kopyalama cihaz bağlantı dizesi tek tırnak yapıştırma emin olun.

Algılayıcı verilerini ve IOT hub'ına gönderilen iletileri gösterir aşağıdaki çıktı görmeniz gerekir.

![Çıktı - Intel Edison'u IOT hub'ına gönderilen algılayıcı verileri](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ınıza göndermek için örnek bir uygulamayı çalıştırdıktan.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
