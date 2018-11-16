---
title: Buluta--IOT DevKit IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Bu öğreticide, ayarlama ve Azure bulut platformuna veri gönderebilmesi IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma konusunda bilgi edinin.
author: rangv
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 08/27/2018
ms.author: rangv
ms.openlocfilehash: d6cbd2992968a57cfba99117e9f1fc1ab9b5b5b3
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711846"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>IOT DevKit AZ3166 Azure IOT hub'a bağlama

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Kullanabileceğiniz [MXChip IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) geliştirmek için ve Microsoft Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümleri. Zengin çevre ve algılayıcılar, açık kaynaklı Panosu paket ve büyüyen bir Arduino ile uyumlu Pano içerir [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Neler

Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) oluşturduğunuz bir Azure IOT hub'ına sensörden sıcaklık ve nem veri toplamak ve IOT hub'ına verileri gönder.

Bir DevKit henüz yok mu? Deneyin [DevKit simülatör](https://azure-samples.github.io/iot-devkit-web-simulator/) veya [bir DevKit satın](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Nasıl bir kablosuz erişim noktasına IOT DevKit bağlanın ve geliştirme ortamınızı hazırlama.
* Nasıl bir IOT hub'ı oluşturma ve MXChip IOT DevKit için bir cihazı kaydedin.
* MXChip IOT DevKit örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="what-you-need"></a>Ne gerekiyor

* MXChip IOT DevKit Pano Micro USB kablosu ile. [Şimdi edinin](https://aka.ms/iot-devkit-purchase).
* Windows 10 veya macOS 10.10 + çalıştıran bir bilgisayar.
* Etkin bir Azure aboneliği. [Ücretsiz 30 günlük deneme Microsoft Azure hesabı etkinleştirme](https://azureinfo.microsoft.com/us-freetrial.html).
  
## <a name="prepare-your-hardware"></a>Donanım hazırlama

Aşağıdaki donanım bilgisayarınıza denetime:

* DevKit Panosu
* Mikro USB kablosu

![Gerekli donanım](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

DevKit bilgisayarınıza bağlanmak için şu adımları izleyin:

1. USB son bilgisayarınıza bağlayın.

2. Mikro USB son Devkit'e bağlanma.

3. Yeşil LED gücü için bağlantıyı doğrular.

   ![Donanım bağlantıları](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wi-fi"></a>Wi-Fi yapılandırma

IOT projelerinin internet bağlantısını kullanır. Wi-Fi bağlanmak için DevKit yapılandırmak için aşağıdaki yönergeleri kullanın.

### <a name="enter-ap-mode"></a>AP moduna gir

Düğmesini B, anında iletme basılı Sıfırla düğmesini bırakın ve sonra b düğmesini bırakın DevKit, Wi-Fi yapılandırmak için AP moduna girer. Ekran DevKit yapılandırma portalı IP adresini ve hizmet kümesi tanımlayıcısını (SSID) görüntüler.

![Düğme, düğmesi B ve SSID Sıfırla](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>AP Devkit'e bağlanma

Şimdi (önceki görüntüde vurgulanan) DevKit SSID bağlanmak için başka bir Wi-Fi etkin cihaz (bilgisayar veya mobil telefon) kullanın. Parola boş bırakın.

![Ağ bilgileri ve Bağlan düğmesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Wi-Fi DevKit için yapılandırma

Bilgisayarınıza veya cep telefonu tarayıcı DevKit ekranda gösterilen IP adresiyle açın, DevKit bağlanmak istediğiniz Wi-Fi ağını seçin ve ardından parolayı yazın. **Bağlan**’ı seçin.

![Parola kutusu ve Bağlan düğmesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Bağlantı başarılı olduğunda DevKit birkaç saniye içinde yeniden başlatır. Ardından Wi-Fi adı ve IP adresi ekranda gördüğünüz:

![Wi-Fi adı ve IP adresi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
> Fotoğrafın görüntülenen IP adresinin DevKit ekranda görüntülenir ve atanan gerçek IP adresini eşleşmeyebilir. Wi-Fi IP'ler dinamik olarak atamak için DHCP kullandığından bu normal, budur.

Wi-Fi yapılandırdıktan sonra cihaza takılı olsa bile kimlik bilgilerinizi bu bağlantı için cihazda açık kalır. Örneğin, DevKit evinizde Wi-Fi yapılandırma ve sonra office DevKit gerçekleştirin, AP modu ("Girin AP modu" bölümüne adımda başlayarak) DevKit ofisiniz için Wi-Fi bağlanmak için yeniden yapılandırmanız gerekir. 

## <a name="start-using-the-devkit"></a>DevKit kullanmaya başlayın

DevKit üzerinde çalışan varsayılan uygulama, en son bellenim sürümünü denetler ve sizin için bazı sensör tanılama verilerini görüntüler.

### <a name="upgrade-to-the-latest-firmware"></a>En son üretici yazılımına için yükseltme

> [!NOTE]
> V1.1 beri DevKit yükleyicisinde ST güvenli sağlar. V1.1 önceki bir sürümü çalıştırıyorsanız bellenimini yükseltmeniz gerekir.

Üretici yazılımı yükseltme gerekiyorsa, ekranın geçerli ve en son bellenim sürümleri gösterilir. Yükseltmek için izleyin [yükseltme bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) Kılavuzu.

![Geçerli ve en son bellenim sürümleri görüntüle](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE]
> Bu tek seferlik bir işlemdir. Üzerinde DevKit geliştirmeye başlamanıza ve uygulamanızı karşıya sonra en son üretici yazılımına uygulamanızla birlikte gelir.

### <a name="test-various-sensors"></a>Çeşitli sensör test

Algılayıcılar test etmek için B düğmesine basın. Her algılayıcı geçiş yapmak için B tuşunu bırakarak ve tuşuna basarak devam edin.

![Düğmesi B ve algılayıcısı görüntüleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

### <a name="install-azure-iot-workbench"></a>Azure IOT Workbench'i yükleyin

Öneririz [Azure IOT Workbench](https://aka.ms/iot-workbench) DevKit üzerinde geliştirmek Visual Studio Code uzantısı.

Azure IOT Workbench IOT çözümleri geliştirmek için tümleşik bir deneyim sağlar. Bu, Azure IOT ve diğer hizmetleri kullanarak hem şirket cihaz ve bulut geliştirmesini yardımcı olur. Ne işe yaradığını genel bir bakış için bu Channel9 videoları izleyebilirsiniz.

Geliştirme ortamı için DevKit hazırlamak için aşağıdaki adımları izleyin:

1. İndirme ve yükleme [Arduino IDE](https://www.arduino.cc/en/Main/Software). Derleme ve Arduino kod karşıya yükleme için gerekli araç zinciri sağlar.
    * **Windows**: kullanım Windows Installer sürümü.
    * **macOS**: sürükle ve bırak ayıklanan **Arduino.app** içine `/Applications` klasör.
    * **Ubuntu**: gibi bir klasöre çıkartın `$HOME/Downloads/arduino-1.8.5`

2. Yükleme [Visual Studio Code](https://code.visualstudio.com/), platformlar arası kaynak kod Düzenleyicisi, IntelliSense kod tamamlama gibi araçları ve hata ayıklama güçlü Geliştirici ile.

3. Aranacak **Azure IOT Workbench** uzantı Market'te ve yükleyin.
    ![Azure IOT Workbench'i yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-workbench.png) bağımlı diğer uzantılar IOT Workbench ile birlikte yüklenir.

4. Açık **Dosya > tercih > ayarları** ve Arduino yapılandırmak için satırları ekleyin.
    * **Windows**:

    ```json
    "arduino.path": "C:\\Program Files (x86)\\Arduino",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

    * **macOS**:

    ```json
    "arduino.path": "/Applications",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

    * **Ubuntu**:

    ```json
    "arduino.path": "/home/{username}/Downloads/arduino-1.8.5",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

5. Tıklayın `F1` komut paleti, türü ve select açmak için **Arduino: Pano Yöneticisi'ni**. Arama **AZ3166** ve en son sürümünü yükleyin.
    ![DevKit SDK'sını yükleyin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-sdk.png)

### <a name="install-st-link-drivers"></a>ST bağlantı sürücüleri yükleyin

[ST-bağlantı/V2](http://www.st.com/en/development-tools/st-link-v2.html) geliştirme makinenize ile iletişim kurmak için IOT DevKit kullanır USB arabirimidir. Cihazınızı makine erişmesine izin vermek için işletim sistemine özgü adımları izleyin.

* **Windows**: USB sürücüsünden yükleyip [STMicroelectronics Web sitesi](http://www.st.com/en/development-tools/stsw-link009.html).
* **macOS**: sürücü macOS için gereklidir.
* **Ubuntu**: terminal ve oturum kapatma ve oturum açma grubu değişikliğin etkili olması aşağıdaki komutu çalıştırın:
    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

Artık geliştirme ortamınızı yapılandırma ve hazırlama ile hazırsınız. "Hello World" örneği için IOT bize oluşturmak: Azure IOT Hub'ına sıcaklık telemetri verileri göndermeye.

## <a name="build-your-first-project"></a>İlk projenizi oluşturun

1. Kendi IOT DevKit olduğundan emin olun **bağlı** bilgisayarınıza. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Alta doğru durum çubuğunda denetleyin **MXCHIP AZ3166** Seçilen Pano ve seri bağlantı noktası ile gösterilen **STMicroelectronics** kullanılır.
    ![Pano ve COM seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-board.png)

1. Tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: örnekler**. Ardından **IOT DevKit** tablosu olarak.

1. IOT Workbench örnekler sayfasında bulma **Başlarken** tıklatıp **açık örnek**. Ardından örnek kodu indirmek için varsayılan yolu seçer.
    ![Açık örnek](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

1. Arduino uzantısı yüklü VS Code'da yoksa, tıklayın **yükleme** bildirim bölmesinde.
    ![Arduino uzantısını yükle](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino-ext.png)

1. Yeni Proje açılan penceresinde tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: Bulut**, ardından **Azure sağlama**. Azure IOT Hub'ınıza sağlama ve cihaz oluşturmayı tamamlamak için adım adım kılavuzu izleyin.
    ![Bulut sağlama](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/cloud-provision.png)

1. Tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: cihaz**, ardından **yapılandırma cihaz Ayarları > yapılandırma cihaz bağlantı dizesi > seçin IOT Hub cihaz bağlantı dizesini**.

1. DevKit üzerinde basılı **bir düğme**, gönderme ve yayın **sıfırlama** düğmesini ve ardından sürüm **bir düğme**. DevKit yapılandırma moduna girer ve bağlantı dizesini kaydeder.
    ![Bağlantı dizesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Tıklayın `F1` seçin ve yeniden yazın **IOT Workbench: cihaz**, ardından **cihaz yükleme**.
    ![Arduino karşıya yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Hataları veya kesintilerinin ise, komutu yeniden çalıştırarak her zaman kurtarabilirsiniz.

## <a name="test-the-project"></a>Test projesi

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Azure IOT Hub'ına gönderilen telemetri görüntüleme

Seri İzleyicisi'ni açmak için durum çubuğunu power Tak simgesine tıklayın: ![seri İzleyicisi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

Örnek uygulama, aşağıdaki sonuçları görmeniz başarıyla çalışıyor:

* IOT Hub'ına gönderilen ileti seri İzleyici görüntüler.
* LED MXChip IOT DevKit üzerinde yanıp sönen.

![Seri İzleme çıkışı](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Azure IOT Hub tarafından alınan telemetri görüntüleme

Kullanabileceğiniz [Azure IOT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (D2C) iletileri IOT Hub CİHAZDAN buluta izlemek için.

1. Visual Studio Code'da Ara **Azure IOT Toolkit** uzantı Market'te ve yükleyin.

1. Oturum açma [Azure portalında](https://portal.azure.com/), oluşturduğunuz IOT Hub'ı bulun.
    ![Azure portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. İçinde **paylaşılan erişim ilkeleri** bölmesinde tıklayın **iothubowner ilke**ve IOT hub'ınızın bağlantı dizesini yazın.
    ![Azure IOT hub'ı bağlantı dizesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. Visual Studio Code'da genişletin **AZURE IOT HUB CİHAZLARI** sol alt köşedeki üzerinde tıklayın **IOT Hub bağlantı dizesine ayarlayın**.
    ![Azure IOT Hub bağlantı dizesine ayarlayın](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-toolkit-conn-string.png)

1. Tıklayın **IOT: D2C iletisini izlemeye başlama** bağlam menüsünde.

1. İçinde **çıkış** bölmesinde, IOT hub'ına gelen D2C iletileri görebilirsiniz.
    ![D2C iletisini](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-toolkit-console.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, bir çözümde denetleyebilirsiniz [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya bizden ulaşın [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit). De bize geri bildirim bu sayfada bir yorum bırakarak tanıyabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza bir MXChip IOT DevKit başarıyla bağlandınız ve IOT hub'ınıza yakalanan sensör verilerini gönderdik.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]
