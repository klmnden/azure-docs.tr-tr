---
title: Buluta--IOT DevKit IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Bu öğreticide, ayarlama ve Azure bulut platformuna veri gönderebilmesi IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma konusunda bilgi edinin.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/17/2019
ms.author: wesmc
ms.openlocfilehash: 2f86b74299b5d47a87ed0b8e89a992f0f91a84be
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64924641"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>IOT DevKit AZ3166 Azure IOT hub'a bağlama

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Kullanabileceğiniz [MXChip IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) geliştirmek için ve Microsoft Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümleri. Zengin çevre ve algılayıcılar, açık kaynaklı Panosu paket ve zengin bir Arduino ile uyumlu Pano içerir [örnek Galerisi](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Nasıl bir IOT hub'ı oluşturma ve MXChip IOT DevKit için bir cihazı kaydedin.
* IOT DevKit Wi-Fi'a bağlayın ve IOT Hub bağlantı dizesini yapılandırmak nasıl.
* IOT hub'ınıza DevKit algılayıcı telemetri verileri göndermek nasıl.
* Geliştirme ortamınızı hazırlama ve IOT DevKit için uygulama geliştirmek nasıl.

Bir DevKit henüz yok mu? Deneyin [DevKit simülatör](https://azure-samples.github.io/iot-devkit-web-simulator/) veya [bir DevKit satın](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-need"></a>Ne gerekiyor

* MXChip IOT DevKit Pano Micro USB kablosu ile. [Şimdi edinin](https://aka.ms/iot-devkit-purchase).
* Windows 10, macOS 10.10 + veya 18.04 + Ubuntu çalıştıran bir bilgisayar.
* Etkin bir Azure aboneliği. [Ücretsiz 30 günlük deneme Microsoft Azure hesabı etkinleştirme](https://azureinfo.microsoft.com/us-freetrial.html).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
  
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

## <a name="quickstart-send-telemetry-from-devkit-to-an-iot-hub"></a>Hızlı Başlangıç: Bir IOT Hub'ına DevKit telemetri gönderme

Hızlı Başlangıç, IOT Hub'ına telemetri göndermek için önceden derlenmiş DevKit bellenim kullanır. Çalıştırmadan önce IOT hub oluşturma ve hub'ı ile cihaz kaydetme.

### <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **MyNodeDevice**: Kaydettirmekte cihazın adı. Kullanım **MyNodeDevice** gösterildiği gibi. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında adı ve örnek uygulamalarda bunları çalıştırmadan önce cihaz adı gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyNodeDevice
    ```
   > [!NOTE]
   > Çalıştırılırken bir hata alırsanız `device-identity`, yükleme [Azure CLI için Azure IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension/blob/dev/README.md) daha fazla ayrıntı için.
  
1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyNodeDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

### <a name="send-devkit-telemetry"></a>DevKit telemetri gönderme

DevKit IOT hub'ınızdaki bir cihaza özel uç noktasına bağlanır ve sıcaklık ve nem telemetrisini gönderir.

1. En son sürümünü indirin [GetStarted bellenim](https://aka.ms/devkit/prod/getstarted/latest) IOT DevKit için.

1. USB üzerinden bilgisayarınıza bağlayın IOT DevKit emin olun. Adlı bir USB yığın depolama cihaz var. dosya Gezgini'ni açın **AZ3166**.
    ![Açık Windows Gezgini](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/az3166-usb.png)

1. Sürükle ve bırak üretici yazılımı yalnızca yığın depolama cihazı ve bu yüklenen otomatik olarak flash.
    ![Üretici yazılımı kopyalayın](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/copy-firmware.png)

1. DevKit üzerinde düğmesini basılı **B**, gönderme ve yayın **sıfırlama** düğmesini ve ardından yayın düğmesini **B**. DevKit AP moduna girer. Onaylamak için ekran DevKit yapılandırma portalı IP adresini ve hizmet kümesi tanımlayıcısını (SSID) görüntüler.
    ![Düğme, düğmesi B ve SSID Sıfırla](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ap.jpg)

    ![AP modunu ayarlama](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/set-ap-mode.gif)

1. Önceki adımda gösterilen IOT DevKit SSID bağlanmak cihaz (bilgisayar veya cep telefonu) kullanımı Web tarayıcısı üzerinde farklı bir Wi-Fi etkin. İçin bir parola isterse, bu alanı boş bırakın.
    ![SSID bağlanma](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/connect-ssid.png)

1. Açık **192.168.0.1** tarayıcıda. Wi-Fi parolayı yazın ve ardından, daha önce Not yapılan cihaz bağlantı dizesini yapıştırın IOT DevKit bağlanmak istediğiniz Wi-Fi'ı seçin. Daha sonra Kaydet'e tıklayın.
    ![Yapılandırma kullanıcı Arabirimi](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui.png)

    > [!NOTE]
    > IOT DevKit yalnızca 2,4 GHz ağ destekler. Denetleme [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#wi-fi-configuration) daha fazla ayrıntı için.

1. Sonuç sayfasını gördüğünüzde, WiFi bilgi ve cihaz bağlantı dizesini IOT DevKit depolanır.
    ![Yapılandırma sonucu](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui-result.png)

    > [!NOTE]
    > Wi-Fi yapılandırdıktan sonra cihaza takılı olsa bile kimlik bilgilerinizi bu bağlantı için cihazda açık kalır.

1. IOT DevKit birkaç saniye içinde yeniden başlatır. DevKit ekranda DevKit IP adresini sıcaklık dahil telemetri verilerini tarafından izler ve nem değeri ileti sayısı ile Azure IOT Hub'ına gönderme bakın.
    ![Wi-Fi IP](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ip.jpg)

    ![Veri gönderme](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/sending-data.jpg)

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Geliştirme ortamı için DevKit hazırlamak için aşağıdaki adımları izleyin:

### <a name="install-visual-studio-code-with-azure-iot-tools-extension-package"></a>Visual Studio Code ile Azure IOT araçları uzantısı paketi yükleme

1. Yükleme [Arduino IDE](https://www.arduino.cc/en/Main/Software). Derleme ve Arduino kod karşıya yükleme için gerekli araç zinciri sağlar.
    * **Windows**: Windows Installer sürümünü kullanın. App Store ' yüklemeyin.
    * **macOS**: Sürükle ve bırak ayıklanan **Arduino.app** içine `/Applications` klasör.
    * **Ubuntu**: Gibi bir klasöre çıkartın `$HOME/Downloads/arduino-1.8.8`

2. Yükleme [Visual Studio Code](https://code.visualstudio.com/), platformlar arası kaynak kod Düzenleyicisi ile güçlü IntelliSense kod tamamlama ve desteğinin yanı sıra zengin uzantılar hata ayıklama Market'ten yüklenebilir.

3. VS Code'u başlatın, Aranan **Arduino** uzantı Market'te ve yükleyin. Bu uzantı, Arduino platformda geliştirmeye yönelik gelişmiş deneyimler sağlar.
    ![Arduino yükleyin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. Aranacak [Azure IOT Araçları](https://aka.ms/azure-iot-tools) uzantı Market'te ve yükleyin.
    ![Azure IOT araçlarını yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

    Veya doğrudan bu bağlantıyı kullanın:
    > [!div class="nextstepaction"]
    > [Azure IOT araçları uzantısı paketi yükleme](vscode:extension/vsciot-vscode.azure-iot-tools)

    > [!NOTE]
    > Azure IOT araçları uzantısı paketi içeren [Azure IOT cihaz Workbench](https://aka.ms/iot-workbench) geliştirme ve çeşitli IOT devkit cihazlarda hata ayıklama için kullanılır. [Azure IOT hub'ı Araç Seti](https://aka.ms/iot-toolkit), ayrıca Azure IOT araçları uzantısı paketindeki, yönetmek ve Azure IOT hub'ları ile etkileşim kurmak için kullanılır.

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

* **Windows**: USB sürücüsünden yükleyip [STMicroelectronics Web sitesi](https://www.st.com/en/development-tools/stsw-link009.html) veya [doğrudan bağlantı](https://aka.ms/stlink-v2-windows).
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

Artık geliştirme ortamınızı yapılandırma ve hazırlama ile hazırsınız. Bize yalnızca çalıştırdığınız GetStarted örneği derleyin.

## <a name="build-your-first-project"></a>İlk projenizi oluşturun

### <a name="open-sample-code-from-sample-gallery"></a>Açık örnek kod örnek Galerisi

IOT DevKit içeren zengin bir Galerisine öğrenmek için kullanabileceğiniz örnekler DevKit çeşitli Azure hizmetlerine bağlanın.

1. Kendi IOT DevKit olduğundan emin olun **bağlı** bilgisayarınıza. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Örnek Aç...** . Ardından **IOT DevKit** tablosu olarak.

1. IOT Workbench örnekler sayfasında bulma **Başlarken** tıklatıp **açık örnek**. Ardından örnek kodu indirmek için varsayılan yolu seçer.
    ![Açık örnek](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

### <a name="provision-azure-iot-hub-and-device"></a>Azure IOT Hub'ı sağlama ve cihaz

Azure IOT Hub'ın sağlama ve cihaz Azure portalından yerine, geliştirme ortamı çıkmadan VS Code'da yapabilirsiniz.

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
