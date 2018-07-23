---
title: Intel Edison buluta (Node.js) - Azure IOT hub'a bağlanma Intel Edison | Microsoft Docs
description: Kurulum ve Intel Edison Intel Edison, bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT hub'a bağlanma hakkında bilgi edinin.
author: rangv
manager: ''
keywords: Azure IOT Intel edison, Intel edison IOT hub, Intel edison Gönder verileri buluta, Intel edison buluta
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 7e4057db6c7e42755bbaf8d05f68809cca0bc257
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185746"
---
# <a name="connect-intel-edison-to-azure-iot-hub-nodejs"></a>Intel Edison (Node.js) Azure IOT hub'a bağlanma

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Intel Edison ile çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra kullanarak cihazlarınızı buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](about-iot-hub.md).

Bir paket henüz yok mu? Başlangıç [burada](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Neler

* Intel Edison Kurulum ve ve Grove modüller.
* IOT hub oluşturun.
* Bir cihaz IOT hub'ına Edison için kaydedin.
* Edison sensör verilerini IOT hub'ınıza gönderilecek örnek uygulamayı çalıştırın.

Intel Edison oluşturduğunuz IOT hub'a bağlayın. Ardından, Edison Grove sıcaklık algılayıcısı sıcaklık ve nem verileri toplamak için örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizesini almak nasıl.
* Edison Grove sıcaklık algılayıcısı ile bağlanma.
* Örnek bir uygulama üzerinde Edison çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* Intel Edison Panosu
* Arduino genişletme Panosu
* Etkin bir Azure aboneliği. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir Mac veya Windows veya Linux çalıştıran bir bilgisayar.
* Internet bağlantısı.
* Bir mikro B türü bir USB kablosu
* Doğrudan geçerli (DC) güç kaynağı. Güç kaynağı gibi derecelendirilmiş:
  - 7-15V DC
  - En az 1500mA
  - Güç kaynağı, pozitif kutbu center/iç PIN'i olmalıdır

Aşağıdaki öğeler isteğe bağlıdır:

* Grove temel kalkan V2
* Grove - sıcaklık algılayıcısı
* Grove kablosu
* Tüm ayırıcı çubukları veya modül genişletme panonun ve dört kümelerinin Vida ve plastik boşluk ekleyiciler fasten için iki Vida dahil olmak üzere, paketleme dahil Vida.

> [!NOTE] 
Kod örneği desteği sensör verilerini benzetimli çünkü bu öğeler isteğe bağlıdır.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Kurulum Intel Edison

### <a name="assemble-your-board"></a>Panonuzu birleştirin

Bu bölümde, Intel® Edison modül genişletme panonuza eklemek için adımları içerir.

1. Beyaz Anahat içinde Intel® Edison modül genişletme panosuna Vida modülü Delikli'kurmak hizalama genişletme panonuzu yerleştirin.

2. Modül sözcükleri hemen altındaki aşağı basın `What will you make?` bir ek düşündüğünüz kadar.

   ![Pano 2 birleştirin](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. Modül genişletme panosuna güvenli hale getirmek için (paket içerisine dâhil) iki onaltılık nuts kullanın.

   ![Pano 3 birleştirin](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. Bir Sarmal genişletme Panosu üzerindeki dört köşe boşluklarını birini ekleyin. Bük ve sarmal üzerine Beyaz Plastik boşluk ekleyiciler birini sıkılaştıran.

   ![Pano 4 birleştirin](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. Diğer üç köşe boşluk ekleyiciler için yineleyin.

   ![Pano 5 birleştirin](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

Artık panonuz derlendiğinden.

   ![Birleştirilmiş Pano](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Grove temel kalkan ve sıcaklık algılayıcısı

1. Panonuzu açın Grove temel kalkan yerleştirin. Tüm PIN'ler panonuzu sıkı bir şekilde takılı olduğundan emin olun.
   
   ![Grove temel koruma](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. Grove sıcaklık algılayıcısı Grove temel kalkan üzerine bağlamak için Grove kablosu kullanın **A0** bağlantı noktası.

   ![Bağlanmak için sıcaklık algılayıcısı](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Edison ve algılayıcı bağlantı](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

Artık, algılayıcı hazırdır.

### <a name="power-up-edison"></a>Güç Edison ayarlama

1. İçinde güç kaynağı takın.

   ![Takın güç kaynağı](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. (DS1 Arduino * genişletme panosunda etiketli) yeşil bir ışığı, vurgulamasında kullanılır ve aydınlatılmış haberdar olun.

3. Panonun önyükleme kurulumu tamamlamak için bir dakika bekleyin.

   > [!NOTE]
   > Bir DC güç kaynağı değilse, yine de panonun bir USB bağlantı noktası üzerinden kapatabilirsiniz. Bkz: `Connect Edison to your computer` ayrıntıları bölümü. Özellikle kullanarak Wi-Fi veya sürüş motors olduğunda bu şekilde Pano destekleyen panonuz, öngörülemeyen davranış neden olabilir.

### <a name="connect-edison-to-your-computer"></a>Edison bilgisayarınıza bağlayın

1. Böylece Edison cihaz modunda mikro düğme iki mikro USB bağlantı noktası doğru aşağı açın veya kapatın. Cihaz mod ve ana bilgisayar arasındaki farklar için lütfen başvuru [burada](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Mikro düğme Değiştir](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. Mikro USB kablosu üst mikro USB bağlantı noktasına takın.

   ![En iyi mikro USB bağlantı noktası](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. Bilgisayarınıza bir USB kablosu sonuna takın.

   ![Bilgisayar USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. Bilgisayarınızı yeni bir sürücü (çok bilgisayarınıza bir SD kart ekleme gibi) bağladığında panonuzu tam olarak başlatılmadan öğrenmiş olacaksınız.

## <a name="download-and-run-the-configuration-tool"></a>Yapılandırma aracını indirme ve çalıştırma
En son yapılandırma aracından alma [bu bağlantıyı](https://software.intel.com/en-us/iot/hardware/edison/downloads) altında listelenen `Installers` başlığı. Aracı yürütün ve izleyin, ekrandaki yönergeleri, sonraki gerektiğinde tıklayarak

### <a name="flash-firmware"></a>Flash üretici yazılımı
1. Üzerinde `Set up options` sayfasında `Flash Firmware`.
2. Aşağıdakilerden birini yaparak panonuzu Flash görüntüyü seçin:
   - İndirin ve Intel'in kullanılabilir en son bellenim görüntüsü ile panonuzu flash için `Download the latest image version xxxx`.
   - Panonuzu, önceden kaydettiğiniz bilgisayarınızda bir görüntüyle flash için seçin `Select the local image`. Bulun ve panonuza flash istediğiniz görüntüyü seçin.
3. Kurulum aracı panonuzu flash dener. İşlemin tümünü yanıp sönen 10 dakikaya kadar sürebilir.

### <a name="set-password"></a>Parola belirleyin
1. Üzerinde `Set up options` sayfasında `Enable Security`.
2. Intel® Edison panonuz için bir özel ad ayarlayabilirsiniz. Bu isteğe bağlıdır.
3. Panonuz için bir parola yazın ve ardından tıklayın `Set password`.
4. Daha sonra kullanılan parola işaretleyin.

### <a name="connect-wi-fi"></a>Wi-Fi bağlanma
1. Üzerinde `Set up options` sayfasında `Connect Wi-Fi`. En fazla bekleme bilgisayarınızı olarak bir dakika, kullanılabilir Wi-Fi ağlarını tarayacağını tarar.
2. Gelen `Detected Networks` aşağı açılan listesinde, ağınızın'ı seçin.
3. Gelen `Security` aşağı açılan listesinde, ağınızın güvenlik türü seçin.
4. Oturum açma ve parola bilgilerinizi sağlayın, sonra tıklayın `Configure Wi-Fi`.
5. Daha sonra kullanılan IP adresi işaretleyin.

> [!NOTE]
> Edison bilgisayarınızı ile aynı ağa bağlı olduğundan emin olun. Bilgisayarınız, Edison için IP adresini kullanarak bağlanır.

   ![Bağlanmak için sıcaklık algılayıcısı](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

Tebrikler! Edison başarıyla yapılandırdınız.

## <a name="run-a-sample-application-on-intel-edison"></a>Intel Edison hakkında bir örnek uygulamayı çalıştırma

### <a name="prepare-the-azure-iot-device-sdk"></a>Azure IOT cihaz SDK'sını hazırlama

1. İçin Intel Edison bağlanmak için ana bilgisayarınız aşağıdaki SSH istemcilerden birini kullanın. IP adresi yapılandırma aracını ve bu Aracı'nda belirlediğiniz bir paroladır.
    - [PuTTY](http://www.putty.org/) Windows için.
    - Ubuntu veya macOS üzerinde yerleşik SSH istemcisi.

2. Örnek istemci uygulaması cihazınıza çalıştıralım. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. Ardından tüm paketleri yüklemek için aşağıdaki komutu çalıştırmak için depo klasörüne gidin, serval dakika sürebilir.
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-the-sample-application"></a>Yapılandırma ve örnek uygulamayı çalıştırma

1. Yapılandırma dosyası, aşağıdaki komutları çalıştırarak açın:

   ```bash
   nano config.json
   ```

   ![Yapılandırma dosyası](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   İki makro vardır. Bu dosyada configurate olabilir. İlki `INTERVAL`, buluta gönderme iki ileti arasındaki zaman aralığını tanımlar. İkincisi `simulatedData`, olduğu için veya sanal sensör verilerini kullanıp kullanmayacağınızı bir Boolean değeri.

   Varsa, **algılayıcı yok**ayarlayın `simulatedData` değerini `true` örnek uygulama oluşturun ve sanal sensör verilerini kullanın.

1. Kaydet ve Çık denetimi-O tuşlarına basarak > Enter > X denetimi.


1. Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kopyalama cihaz bağlantı dizesini tek tırnak yapıştırma emin olun.

Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir.

![Çıkış - Intel Edison IOT hub'ına gönderilen algılayıcı verileri](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
