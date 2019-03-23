---
title: Buluta--IOT DevKit IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Bu öğreticide, ayarlama ve Azure bulut platformuna veri gönderebilmesi IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma konusunda bilgi edinin.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/21/2019
ms.author: wesmc
ms.openlocfilehash: 941455e39a32405097563b043046866aeb5c7964
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351941"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>IOT DevKit AZ3166 Azure IOT hub'a bağlama

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Kullanabileceğiniz [MXChip IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) geliştirmek için ve Microsoft Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümleri. Zengin çevre ve algılayıcılar, açık kaynaklı Panosu paket ve büyüyen bir Arduino ile uyumlu Pano içerir [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Neler

DevKit oluşturduğunuz Azure IOT hub'a bağlayın. Ardından sensörden sıcaklık ve nem verileri toplamak ve IOT hub'ına verileri gönderin.

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

![AP modunu ayarlama](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-ap-mode.gif)

### <a name="connect-to-devkit-ap"></a>AP Devkit'e bağlanma

Şimdi (önceki görüntüde vurgulanan) DevKit SSID bağlanmak için başka bir Wi-Fi etkin cihaz (bilgisayar veya mobil telefon) kullanın. Parola boş bırakın.

![Ağ bilgileri ve Bağlan düğmesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Wi-Fi DevKit için yapılandırma

Bilgisayarınıza veya cep telefonu tarayıcı DevKit ekranda gösterilen IP adresiyle açın, DevKit bağlanmak istediğiniz Wi-Fi ağını seçin ve ardından parolayı yazın. **Bağlan**’ı seçin.

![Parola kutusu ve Bağlan düğmesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Bağlantı başarılı olduğunda DevKit birkaç saniye içinde yeniden başlatır. Ardından Wi-Fi adı ve IP adresi ekranda gördüğünüz:

![Wi-Fi adı ve IP adresi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE]
> 2.4 GHz ağ IOT DevKit çalışmak için ihtiyacınız olacak. IOT DevKit WiFi modüldeki 5 GHz ağıyla uyumlu değil. Denetleme [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#wi-fi-configuration) daha fazla ayrıntı için.

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

### <a name="install-azure-iot-tools"></a>Azure IOT araçlarını yükleme

Öneririz [Azure IOT Araçları](https://aka.ms/azure-iot-tools) DevKit üzerinde geliştirmek Visual Studio Code için uzantı paketi. Azure IOT araçları içeren [Azure IOT cihaz Workbench](https://aka.ms/iot-workbench) geliştirin ve çeşitli IOT devkit cihazlarda hata ayıklama ve [Azure IOT hub'ı Araç Seti](https://aka.ms/iot-toolkit) yönetmek ve Azure IOT Hub ile etkileşim kurmak için.

Bu izleme [Channel 9](https://channel9.msdn.com/) yaptıkları genel bakış için videolar:
* [Yeni IOT Workbench uzantısı için VS Code giriş](https://channel9.msdn.com/Shows/Internet-of-Things-Show/IoT-Workbench-extension-for-VS-Code)
* [VS Code için IOT Toolkit uzantısını yenilikler](https://channel9.msdn.com/Shows/Internet-of-Things-Show/Whats-new-in-the-IoT-Toolkit-extension-for-VS-Code)

Geliştirme ortamı için DevKit hazırlamak için aşağıdaki adımları izleyin:

1. Yükleme [Arduino IDE](https://www.arduino.cc/en/Main/Software). Derleme ve Arduino kod karşıya yükleme için gerekli araç zinciri sağlar.
    * **Windows**: Windows Installer sürümünü kullanın. App Store ' yüklemeyin.
    * **macOS**: Sürükle ve bırak ayıklanan **Arduino.app** içine `/Applications` klasör.
    * **Ubuntu**: Gibi bir klasöre çıkartın `$HOME/Downloads/arduino-1.8.8`

2. Yükleme [Visual Studio Code](https://code.visualstudio.com/), platformlar arası kaynak kod Düzenleyicisi, IntelliSense kod tamamlama gibi araçları ve hata ayıklama güçlü Geliştirici ile.

3. VS Code'u başlatın, Aranan **Arduino** uzantı Market'te ve yükleyin. Bu uzantı, Arduino platformda geliştirmeye yönelik gelişmiş deneyimler sağlar.
    ![Arduino yükleyin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. Aranacak **Azure IOT Araçları** uzantı Market'te ve yükleyin.
    ![Azure IOT araçlarını yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

    > [!div class="nextstepaction"]
    > [Azure IOT araçları uzantısı paketi yükleme](vscode:extension/vsciot-vscode.azure-iot-tools)

5. VS Code Arduino ayarlarla yapılandırın.

    Visual Studio Code'da tıklayın **Dosya > tercih > ayarları**. Ardından **...**  ve **settings.json açın**.
    ![Azure IOT araçlarını yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/user-settings-arduino.png)
    
    Satırları Arduino platformunuza bağlı olarak yapılandırmak için aşağıdaki ekleyin: 

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
    
        Değiştirin **{username}** adınızı içeren aşağıda yer tutucu.

        ```json
        "arduino.path": "/home/{username}/Downloads/arduino-1.8.8",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

6. Tıklayın `F1` komut paletini açın için girin ve seçin **Arduino: Pano Yöneticisi'ni**. Arama **AZ3166** ve en son sürümünü yükleyin.
    ![DevKit SDK'sını yükleyin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-az3166-sdk.png)

### <a name="install-st-link-drivers"></a>ST bağlantı sürücüleri yükleyin

[ST-bağlantı/V2](https://www.st.com/en/development-tools/st-link-v2.html) geliştirme makinenize ile iletişim kurmak için IOT DevKit kullanır USB arabirimidir. Derlenmiş cihaz kodu DevKit Flash Windows üzerinde yüklemeniz gerekir. Cihazınızı makine erişmesine izin vermek için işletim sistemine özgü adımları izleyin.

* **Windows**: USB sürücüsünden yükleyip [STMicroelectronics Web sitesi](https://www.st.com/en/development-tools/stsw-link009.html) için [doğrudan bağlantı](https://aka.ms/stlink-v2-windows).
* **macOS**: Sürücü, macOS için gereklidir.
* **Ubuntu**: Terminalde komutları çalıştırmak, oturumu kapatın ve grubu değişikliğin etkili olması için oturum açın:
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

### <a name="open-sample-code-from-sample-gallery"></a>Açık örnek kod örnek Galerisi

1. Kendi IOT DevKit olduğundan emin olun **bağlı** bilgisayarınıza. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Örnek Aç...** . Ardından **IOT DevKit** tablosu olarak.

1. IOT Workbench örnekler sayfasında bulma **Başlarken** tıklatıp **açık örnek**. Ardından örnek kodu indirmek için varsayılan yolu seçer.
    ![Açık örnek](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

### <a name="provision-azure-iot-hub-and-device"></a>Azure IOT Hub'ı sağlama ve cihaz

1. Yeni Proje açılan penceresinde tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Azure hizmetleri sağlama...** . Azure IOT Hub'ınıza sağlama ve IOT Hub cihazı oluşturmayı tamamlamak için adım adım kılavuzu izleyin.
    ![Sağlama komutu](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision.png)

    > [!NOTE]
    > Azure'da açmadıysanız. Oturum açmak için açılır bildirim izleyin.

1. Kullanmak istediğiniz aboneliği seçin.
    ![Sub seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-subscription.png)

1. Sonra seçin veya yeni bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#terminology).
    ![Kaynak grubu seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-resource-group.png)

1. Belirtilen kaynak grubunda seçin veya yeni bir Azure IOT Hub oluşturma kılavuzu izleyin.
    ![IOT hub'ı adımları seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provision.png)

    ![IOT hub'ını seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-hub.png)

    ![Seçili IOT hub'ı](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-selected.png)

1. Çıktı penceresinde, Azure IOT hub'da sağlanan görürsünüz ![IOT hub'ı sağlandı](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provisioned.png)

1. Seçin veya yeni bir cihaz sağladığınız Azure IOT Hub oluşturun.
    ![IOT cihaz adımları seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-device-provision.png)

    ![Sağlanan IOT cihazı seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-device.png)

1. Sağlanan Azure IOT Hub ve cihaz içinde oluşturulan elde edeceksiniz. Ayrıca IOT DevKit daha sonra yapılandırmak için cihaz bağlantı dizesini, VS Code'da'e kaydedilecek.
    ![Sağlama tamamlandı](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision-done.png)

### <a name="configure-and-compile-device-code"></a>Cihaz kodu düzenledikçe ve yapılandırma

1. Sağ alt durum çubuğunda denetleyin **MXCHIP AZ3166** Seçilen Pano ve seri bağlantı noktası ile gösterilen **STMicroelectronics** kullanılır.
    ![Pano ve COM seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-com.png)

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Cihaz ayarlarını yapılandır...** , ardından **yapılandırma cihaz bağlantı dizesi > IOT Hub cihaz bağlantı dizesini seçin**.

1. DevKit üzerinde basılı **bir düğme**, gönderme ve yayın **sıfırlama** düğmesini ve ardından sürüm **bir düğme**. DevKit yapılandırma moduna girer ve bağlantı dizesini kaydeder.
    ![Bağlantı dizesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Tıklayın `F1` seçin ve yeniden yazın **Azure IOT cihaz Workbench: Cihaz kodu karşıya**. Bu derleme başlar ve kodu Devkit'e yükleyin.
    ![Arduino karşıya yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Tüm hataları veya kesintilerinin ise, komutu yeniden çalıştırarak her zaman kurtarabilirsiniz.

## <a name="test-the-project"></a>Test projesi

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Azure IOT Hub'ına gönderilen telemetri görüntüleme

Seri İzleyicisi'ni açmak için durum çubuğunu power Tak simgesine tıklayın: ![Seri İzleyicisi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

Örnek uygulama, aşağıdaki sonuçları görmeniz başarıyla çalışıyor:

* IOT Hub'ına gönderilen ileti seri İzleyici görüntüler.
* LED MXChip IOT DevKit üzerinde yanıp sönen.

![Seri İzleme çıkışı](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Azure IOT Hub tarafından alınan telemetri görüntüleme

Kullanabileceğiniz [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) (D2C) iletileri IOT Hub CİHAZDAN buluta izlemek için.

1. Oturum [Azure portalında](https://portal.azure.com/), oluşturduğunuz IOT Hub'ı bulun.
    ![Azure portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. İçinde **paylaşılan erişim ilkeleri** bölmesinde tıklayın **iothubowner ilke**ve IOT hub'ınızın bağlantı dizesini yazın.
    ![Azure IOT hub'ı bağlantı dizesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. VS Code'da tıklayın `F1`yazın ve seçin **Azure IOT Hub: IOT Hub bağlantı dizesine ayarlayın**. Bağlantı dizesini buraya kopyalayın.
    ![Azure IOT Hub bağlantı dizesine ayarlayın](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-iothub-connection-string.png)

1. Genişletin **AZURE IOT HUB CİHAZLARI** sağ tıklayın, oluşturduğunuz ve seçin cihaz adına sağ bölmesinde **D2C iletisini İzlemeyi Başlat**.
    ![İzleyici D2C iletisini](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/monitor-d2c.png)

1. İçinde **çıkış** bölmesinde, IOT hub'ına gelen D2C iletileri görebilirsiniz.
    ![D2C iletisini](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/d2c-output.png)

## <a name="review-the-code"></a>Kodu gözden geçirin

`GetStarted.ino` Ana Arduino taslak dosyasıdır.

![D2C iletisini](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/code.png)

Azure IOT Hub'ına cihaz telemetrisi nasıl gönderileceğini görmek için `utility.cpp` dosya aynı klasörde yer alan. Görünüm [API Başvurusu](https://microsoft.github.io/azure-iot-developer-kit/docs/apis/arduino-language-reference/) sensör ve çevre birimleri üzerinde IOT DevKit kullanma hakkında bilgi edinmek için.

`DevKitMQTTClient` Kullanılır, bir sarmalayıcı **iothub_client** gelen [Microsoft Azure IOT SDK'ları ve kitaplıkları c](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client) Azure IOT Hub ile etkileşim kurmak için.

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, bir çözümde denetleyebilirsiniz [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya bizden ulaşın [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit). De bize geri bildirim bu sayfada bir yorum bırakarak tanıyabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza bir MXChip IOT DevKit başarıyla bağlandınız ve IOT hub'ınıza yakalanan sensör verilerini gönderdik.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]
