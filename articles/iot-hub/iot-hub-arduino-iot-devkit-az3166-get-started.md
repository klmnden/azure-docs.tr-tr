---
title: Buluta--IOT DevKit IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Bu öğreticide, ayarlama ve Azure bulut platformuna veri gönderebilmesi IOT DevKit AZ3166 Azure IOT Hub'ına bağlanma konusunda bilgi edinin.
author: rangv
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: cf9ee5339c53eb4f9c74f6b5f251a7963555d676
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928758"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>Azure IOT hub'ı bulutta IOT DevKit AZ3166 bağlanma

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Kullanabileceğiniz [MXChip IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) geliştirmek için ve Microsoft Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümleri. Zengin çevre ve algılayıcılar, açık kaynaklı Panosu paket ve büyüyen bir Arduino ile uyumlu Pano içerir [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Neler
Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) oluşturduğunuz bir Azure IOT hub'ına sensörden sıcaklık ve nem veri toplamak ve IOT hub'ına verileri gönder.

Bir DevKit henüz yok mu? Deneyin [DevKit simülatör](https://azure-samples.github.io/iot-devkit-web-simulator/) veya [edinebileceğinizi](https://aka.ms/iot-devkit-purchase).

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

Donanım bilgisayarınıza bağlayın.

Bu donanım ihtiyacınız vardır:

* DevKit Panosu
* Mikro USB kablosu

![Gerekli donanım](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

DevKit bilgisayarınıza bağlanmak için:

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
> V1.1 beri DevKit yükleyicisinde ST güvenli sağlar. Büyük olasılıkla çalışması için v1.1 altında çalışıyorsa, bellenimini yükseltmeniz gerekir.

Üretici yazılımı yükseltme gerekiyorsa, ekranın geçerli ve en son bellenim sürümleri gösterilir. Yükseltmek için izleyin [yükseltme bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) Kılavuzu.

![Geçerli ve en son bellenim sürümleri görüntüle](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
> Bu tek seferlik bir işlemdir. Üzerinde DevKit geliştirmeye başlamanıza ve uygulamanızı karşıya sonra en son üretici yazılımına uygulamanızla birlikte gelir.

### <a name="test-various-sensors"></a>Çeşitli sensör test

Algılayıcılar test etmek için B düğmesine basın. Her algılayıcı geçiş yapmak için B tuşunu bırakarak ve tuşuna basarak devam edin.

![Düğmesi B ve algılayıcısı görüntüleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Geliştirme ortamını ayarlama artık: araçlar ve göz alıcı IOT uygulamaları oluşturmanıza olanak paketleri. Windows veya macOS sürümü, işletim sisteminize göre seçebilirsiniz.

### <a name="windows"></a>Windows

Geliştirme ortamınızı hazırlama için yükleme paketi kullanmanızı öneririz. Herhangi bir sorunla karşılaşırsanız, izleyebilirsiniz [el ile yapılacak adımlar](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) bitti edinilir.

#### <a name="download-the-latest-package"></a>En son paketini indirme

Yükleyebileceğiniz bir .zip dosyası, tüm gerekli araçlara ve DevKit geliştirme paketleri içerir.

> [!div class="button"]
[İndir](https://aka.ms/devkit/prod/installpackage/latest)

.Zip dosyası, aşağıdaki araçları ve paketleri içerir. Bazı bileşenleri yüklü zaten varsa, betik algılamak ve onları atlayın.

* Node.js ve Yarn: Kurulum betiğini ve otomatik görevler için çalışma zamanı.
* [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows): Azure kaynaklarını yönetmek için platformlar arası komut satırı deneyimi. MSI bağımlı Python ve pip uygulamalarını içerir.
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code): DevKit geliştirme için basit bir kod düzenleyici.
* [Arduino için Visual Studio Code uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Arduino geliştirme Visual Studio code'da uzantısı.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): Arduino uzantısı dayanan aracı.
* DevKit Panosu paket: zincirleri, kitaplıkları ve projeleri için DevKit aracı.
* ST bağlantı yardımcı program: Temel araçlar ve sürücüleri.

#### <a name="run-the-installation-script"></a>Yükleme betiğini çalıştırın

Windows dosya Gezgini'nde, .zip dosyasını bulun ve ayıklayın. Bulma `install.cmd`sağ tıklatın ve seçin **yönetici olarak çalıştır**.

![Dosya Gezgini](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Yükleme sırasında her bir aracı veya paket ilerlemesini bakın.

![Yükleme ilerleme durumu](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

> [!NOTE] 
> Ortamınıza bağlı olarak bazen hata Arduino IDE yüklerken alırsınız. Bu durumda, deneyebilirsiniz [Arduino IDE ayrı ayrı yükleme](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) Install.cmd yeniden çalıştırın. Aksi takdirde Lütfen izleyin [el ile yapılacak adımlar](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) tüm gerekli araçlara ve paketleri yüklemek için.

#### <a name="install-drivers"></a>Sürücüleri yükleyin

Arduino uzantısı için VS Code, Arduino IDE üzerinde kullanır. Arduino IDE yüklediğiniz ilk kez buysa, ilgili sürücüleri yüklemeniz istenir:

![alma başlatıldı-sürücü](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Yükleme, internet hızına bağlı olarak yaklaşık 10 dakika sürer. Yükleme tamamlandıktan sonra masaüstünüzde Visual Studio Code ve Arduino IDE kısayolları görmeniz gerekir.

> [!NOTE] 
> Bazen, VS Code başlattığınızda, Arduino IDE veya ilgili Pano paketi bulunamıyor hatayla istenir. Bunu çözmek için VS Code kapatın ve Arduino IDE yeniden başlatın. VS kodu ardından bulun Arduino IDE yolun doğru.

### <a name="macos"></a>macOS

Geliştirme ortamınızı hazırlama için tek tıklamayla yükleme deneyimi kullanmanızı öneririz. Herhangi bir sorunla karşılaşırsanız, izleyebilirsiniz [el ile yapılacak adımlar](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) bitti edinilir.

#### <a name="install-homebrew"></a>Homebrew yükleme

> [!NOTE] 
> Homebrew yüklediyseniz, bu adımı atlayabilirsiniz.

İzleyin [Homebrew yükleme yönergelerini](https://docs.brew.sh/Installation.html) yükleyin.

#### <a name="download-the-latest-package"></a>En son paketini indirme
Yükleyebileceğiniz bir .zip dosyası, tüm gerekli araçlara ve DevKit geliştirme paketleri içerir.

> [!div class="button"]
[İndir](https://aka.ms/devkit/prod/installpackage/mac/latest)

.Zip dosyası, aşağıdaki araçları ve paketleri içerir. Bazı bileşenleri yüklü zaten varsa, betik algılamak ve onları atlayın.

* Node.js ve Yarn: Kurulum betiğini ve otomatik görevler için çalışma zamanı.
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest#a-namemacosinstall-on-macos): Azure kaynaklarını yönetmek için platformlar arası komut satırı deneyimi.
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code): DevKit geliştirme için basit bir kod düzenleyici.
* [Arduino için Visual Studio Code uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Arduino geliştirme Visual Studio code'da uzantısı.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): Arduino uzantısı dayanan aracı.
* DevKit Panosu paket: zincirleri, kitaplıkları ve projeleri için DevKit aracı.
* ST bağlantı yardımcı program: Temel araçlar ve sürücüleri.

#### <a name="run-the-installation-script"></a>Yükleme betiğini çalıştırın

Finder .zip bulun ve ayıklayın:

![macOS Bulucu](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-finder.png)

Terminal uygulamasını başlatın, .zip dosyasını ayıklayın ve Çalıştır klasörü bulun:

```bash
./install.sh
```

![macOS yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-install-sh.png)

> [!NOTE] 
> Homebrew izin hatası karşılıyorsanız, çalıştırma `brew doctor` sorunu düzeltmesi için. Denetleme [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#homebrew-permission-error-on-macos) daha fazla ayrıntı için.

Artık tüm gerekli araçlara ve macOS için yüklü paketleri vardır.


## <a name="open-the-project-folder"></a>Proje klasörünü açın

### <a name="start-vs-code"></a>VS Code'u başlatın

DevKit değil bağlı olduğundan emin olun. VS Code ilk kez başlatın ve DevKit bilgisayarınıza bağlayın. VS Code, otomatik olarak DevKit bulur ve bir giriş sayfasını açar:

![Giriş sayfası](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

> [!NOTE] 
> Bazen, VS Code başlattığınızda, Arduino IDE veya ilgili Pano paketi bulunamıyor hatayla istenir. VS Code kapatın ve Arduino IDE yeniden başlatın. VS kodu ardından bulun Arduino IDE yolun doğru.


### <a name="open-the-arduino-examples-folder"></a>Arduino örnekler klasörü açın

Üzerinde **Arduino örnekler** sekmesinde, Gözat **MXCHIP AZ3166 için örnek** > **AzureIoT**seçip **GetStarted**.

![Arduino örnekler sekmesi](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

Bölmesini kapatmak için MSDN aboneliğine, yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="provision-azure-services"></a>Azure hizmetleri sağlama

Çözüm penceresinde göreviniz çalışmasını `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision`.

VS Code terminalde, etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size kılavuzluk eder:

![Etkileşimli komut satırı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-the-arduino-sketch"></a>Derleme ve Arduino taslak karşıya yükleyin

### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırılacak `task device-upload`.
2. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı anında iletme ve Sıfırla düğmesini bırakın. Ekran DevKit kimliğine ve 'Configuration' görüntüler.

Alır bağlantı dizesini ayarlayalım budur `task cloud-provision` adım.

Sonra VS Code, doğrulama ve Arduino taslak karşıya yükleme başlar:

![Doğrulama ve Arduino taslağın karşıya yükleme](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE] 
> Bazen, hata alırsınız "hata: AZ3166: Bilinmeyen Paket". Bu panonun nedeniyle, paket dizinini yenilenmez. Bunu denetlemek [SSS adımları](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) çözmek için.

### <a name="macos"></a>macOS

1. Yapılandırma moduna DevKit: düğme A, sonra anında iletme ve yayın Sıfırla düğmesini basılı tutun. Ekran 'Configuration' görüntüler.
2. Kullanım `Cmd+P` çalıştırılacak `task device-upload`.

Alır bağlantı dizesini ayarlayalım budur `task cloud-provision` adım.

Sonra VS Code, doğrulama ve Arduino taslak karşıya yükleme başlar:

![cihaz karşıya yükleme](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE] 
> Bazen, hata alırsınız "hata: AZ3166: Bilinmeyen Paket". Bu panonun nedeniyle, paket dizinini yenilenmez. Bunu denetlemek [SSS adımları](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) çözmek için.


## <a name="test-the-project"></a>Test projesi

VS Code'da açın ve seri İzleyicisi ayarlamak için aşağıdaki adımları izleyin:

1. Tıklayın `COM[X]` doğru COM bağlantı noktası ile ayarlamak için durum çubuğuna word `STMicroelectronics`: ![com bağlantı noktası](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/com-port.png)

2. Seri İzleyicisi'ni açmak için durum çubuğunu Power Tak simgesine tıklayın: ![seri İzleyicisi](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution//connect-iothub/serial-monitor.png)

3. Durum çubuğu Baud hızı temsil eden bir sayıya tıklayın ve kümesine `115200`: ![baud hızı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/baud-rate.png)

Örnek uygulama, aşağıdaki sonuçları görmeniz başarıyla çalışıyor:

* Seri İzleyici, aşağıdaki ekran görüntüsünde gösterilen içerik olarak aynı bilgileri görüntüler.
* Üzerinde MXChip IOT DevKit RGB LED'i yanıp sönen.

![VS code'da son çıkış](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, bulabilirsiniz [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/). De bize geri bildirim bu sayfada bir yorum bırakarak tanıyabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza bir MXChip IOT DevKit başarıyla bağlandınız ve IOT hub'ınıza yakalanan sensör verilerini gönderdik.

Azure IOT Hub ile çalışmaya başlama ve diğer IOT senaryolarını keşfetmek için bkz:

- [iothub-explorer ile bulut cihaz mesajlaşmasını yönetme](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [IoT Hub iletilerini Azure veri depolamaya kaydetme](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için Power BI'ı kullanma](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için Web Apps'ı kullanma](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Azure Machine Learning'de, IOT hub'ından sensör verilerini kullanarak hava durumu tahmini](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [iothub-explorer ile cihaz yönetimi](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Logic Apps ile uzaktan izleme ve bildirimler](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
