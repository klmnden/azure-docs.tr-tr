---
title: "Bulut için IOT DevKit: IOT DevKit AZ3166 Azure IOT Hub'ına bağlanmak | Microsoft Docs"
description: "Bu öğreticide, ayarlamak ve Azure bulut platformu veri gönderebilmesi IOT DevKit AZ3166 Azure IOT Hub'ına bağlanmak öğrenin."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2017
ms.author: xshi
ms.openlocfilehash: 6a9d5e029e48c1bb62ad4731c7413f023b97c8c9
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>Bulutta Azure IOT Hub'ına IOT DevKit AZ3166 Bağlan

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Kullanabileceğiniz [MXChip IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) geliştirmek için ve Microsoft Azure hizmetlerini yararlanmak prototip nesnelerin interneti (IOT) çözümler. Zengin çevre birimleri ve algılayıcılar, bir açık kaynak tablosu paketi ve bir büyüyen bir Arduino uyumlu panosuna içeren [projeleri katalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Neler
Bağlantı [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) , oluşturduğunuz bir Azure IOT hub'ına algılayıcılar sıcaklık ve nem verileri toplamak ve IOT hub'ına verileri gönderin.

Bir DevKit henüz yok mu? Deneyin [DevKit simulator](https://azure-samples.github.io/iot-devkit-web-simulator/) veya [edinebileceğinizi](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Nasıl IOT DevKit bir kablosuz erişim noktasına bağlanmak ve geliştirme ortamınızı hazırlayın.
* Nasıl IOT hub oluşturma ve bir cihaz için MXChip IOT DevKit kaydetme.
* Örnek bir uygulama üzerinde MXChip IOT DevKit çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza algılayıcı verileri göndermek nasıl.

## <a name="what-you-need"></a>Ne gerekiyor

* MXChip IOT DevKit kartı mikro USB kablosu ile. [Şimdi Al](https://aka.ms/iot-devkit-purchase).
* Windows 10 veya macOS 10.10 + çalıştıran bir bilgisayar.
* Etkin bir Azure aboneliği. [Ücretsiz 30 günlük deneme Microsoft Azure hesabı etkinleştirme](https://azureinfo.microsoft.com/us-freetrial.html).
  

## <a name="prepare-your-hardware"></a>Donanımınızın hazırlama

Donanım bilgisayarınıza bağlayın.

Bu donanım gerekir:

* DevKit Panosu
* Mikro USB kablosu

![Gerekli donanım](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

Bilgisayarınıza DevKit bağlanmak için:

1. USB son bilgisayarınıza bağlayın.
2. Mikro USB son DevKit bağlayın.
3. Güç için yeşil LED bağlantı onaylar.

![Donanım bağlantıları](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wi-fi"></a>Wi-Fi yapılandırma

IOT projelerinin Internet bağlantısı kullanır. Wi-Fi bağlanmak için DevKit yapılandırmak için aşağıdaki yönergeleri kullanın.

### <a name="enter-ap-mode"></a>AP modu girin

Düğmesini B, anında iletme basılı ve Sıfırla düğmesini bırakın ve düğmesini B. bırakın DevKit Wi-Fi yapılandırma için AP moduna girer. Ekran DevKit yapılandırma portalı IP adresini ve hizmet kümesi tanımlayıcısını (SSID) görüntüler.

![Düğme, düğmesi B ve SSID Sıfırla](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>DevKit AP Bağlan

Şimdi (önceki görüntüde vurgulanan) DevKit SSID bağlanmak için başka bir Wi-Fi etkin aygıt (bilgisayar veya cep telefonu) kullanın. Parola boş bırakın.

![Ağ bilgi ve Bağlan düğmesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Wi-Fi DevKit için yapılandırma

Open bilgisayarınıza veya cep telefonu tarayıcı DevKit ekranda gösterilen IP adresi, DevKit bağlanmak istediğiniz Wi-Fi ağı seçin ve ardından parolayı yazın. **Bağlan**’ı seçin.

![Parola kutusu ve Bağlan düğmesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Bağlantı başarılı olduğunda DevKit birkaç saniye içinde yeniden başlatır. Daha sonra Wi-Fi adı ve IP adresi ekranda görürsünüz:

![Wi-Fi adı ve IP adresi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
> Fotoğrafın görüntülenen IP adresi atanır ve DevKit ekranda görüntülenen gerçek bir IP adresi eşleşmeyebilir. Wi-Fi IP'leri dinamik olarak atamak için DHCP kullandığından bu, normaldir.

Wi-Fi yapılandırıldıktan sonra aygıtın takılı olsa bile kimlik bilgilerinizi bu bağlantı için cihazda korunur. Örneğin, DevKit Wi-Fi için evinizde yapılandırın ve ardından DevKit ofise uygulayın, AP modu ("Girin AP modu" bölümündeki adım başlayarak), office Wi-Fi DevKit bağlanmak için yeniden yapılandırmanız gerekir. 

## <a name="start-using-the-devkit"></a>DevKit kullanmaya başlama

DevKit üzerinde çalışan varsayılan uygulama, en son bellenim sürümünü denetler ve sizin için bazı algılayıcı tanılama verilerini görüntüler.

### <a name="upgrade-to-the-latest-firmware"></a>En son bellenim yükseltme

> [!NOTE] 
> V1.1 itibaren DevKit ST güvenli yükleyicisinde sağlar. Büyük olasılıkla çalışması için v1.1 altında çalıştırıyorsanız bellenimini yükseltmeniz gerekir.

Bellenim yükseltmesi gerekiyorsa, ekran geçerli ve en son bellenim sürümleri gösterir. Yükseltme için izleyin [yükseltme bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) Kılavuzu.

![Geçerli ve en son bellenim sürümleri görüntüleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
> Bir kerelik çaba budur. Üzerinde DevKit geliştirmeye başlayın ve uygulamanız karşıya sonra en son üretici yazılımına uygulamanızla birlikte gelir.

### <a name="test-various-sensors"></a>Çeşitli algılayıcılar test

Algılayıcılar test etmek için B düğmesine basın. Tuşuna basarak ve her algılayıcı arasında geçiş yapmak için düğmesi B bırakarak devam edin.

![Düğmesi B ve algılayıcı görüntüleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Geliştirme ortamını ayarlama şimdi: araçlar ve etkileyici IOT uygulamalar oluşturmanızı paketleri. MacOS sürümü ve Windows işletim sisteminizi göre seçebilirsiniz.

### <a name="windows"></a>Windows

Geliştirme ortamı hazırlamak için yükleme paketi kullanmanızı öneririz. Herhangi bir sorunla karşılaşırsanız, takip edebilirsiniz [el ile yapılacak adımlar](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) bitti elde edin.

#### <a name="download-the-latest-package"></a>En son paketini indirin

Yüklediğiniz .zip dosyasını tüm gerekli araçları ve DevKit geliştirme için paketleri içerir.

> [!div class="button"]
[İndir](https://aka.ms/devkit/prod/installpackage/latest)

.Zip dosyasını aşağıdaki araçları ve paketleri içerir. Bazı bileşenleri yüklü zaten varsa, betik algılamak ve onları atlayın.

* Node.js ve Yarn: otomatik görevler ve Kurulum komut dosyası için çalışma zamanı.
* [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows): Azure kaynaklarını yönetmek için platformlar arası komut satırı deneyimi. MSI bağımlı Python ve PIP içerir.
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code): DevKit geliştirme için basit kod düzenleyicisini.
* [Visual Studio Code uzantısı Arduino için](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Visual Studio Code Arduino geliştirme sağlar uzantısı.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): Arduino uzantısı dayanan aracı.
* DevKit Panosu paketi: zincirleri, kitaplıklar ve projeler için DevKit aracı.
* ST bağlantı yardımcı programı: Temel araçlar ve sürücüleri.

#### <a name="run-the-installation-script"></a>Yükleme komut dosyasını çalıştır

Windows dosya Gezgini'nde, .zip dosyasını bulun ve ayıklayın. Bul `install.cmd`sağ tıklatın ve seçin **yönetici olarak çalıştır**.

![Dosya Gezgini](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Yükleme sırasında her aracı veya paket ilerlemesini bakın.

![Yükleme ilerleme durumu](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

> [!NOTE] 
> Ortamınıza bağlı olarak, bazen hata Arduino IDE yüklerken alırsınız. Bu durumda, deneyebilirsiniz [Arduino IDE ayrı ayrı yükleme](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) ve Install.cmd yeniden çalıştırın. Aksi halde, lütfen izleyin [el ile yapılacak adımlar](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) tüm gerekli araçları ve paketleri yüklemek için.

#### <a name="install-drivers"></a>Sürücüleri yükleyin

VS Code'da Arduino uzantısı Arduino IDE üzerinde kullanır. Arduino IDE yüklediğiniz ilk kez kullanıyorsanız, ilgili sürücüleri yüklemeniz istenir:

![alma başlatıldı-sürücü](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Yükleme Internet hızınıza bağlı olarak yaklaşık 10 dakika sürer. Yükleme tamamlandıktan sonra masaüstünüzde Visual Studio Code ve Arduino IDE kısayolları görmeniz gerekir.

> [!NOTE] 
> Bazen, VS Code başlattığınızda, onu Arduino IDE veya ilgili Panosu paketi bulamıyor hatayla istenir. Bunu çözmek için VS Code kapatın ve Arduino IDE yeniden başlatın. VS kodu sonra bulun Arduino IDE yolun doğru.

### <a name="macos"></a>macOS

Geliştirme ortamı hazırlamak için tek tıklamayla yükleme deneyimi kullanmanızı öneririz. Herhangi bir sorunla karşılaşırsanız, takip edebilirsiniz [el ile yapılacak adımlar](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) bitti elde edin.

#### <a name="install-homebrew"></a>Homebrew yükleyin

> [!NOTE] 
> Homebrew yüklediyseniz, bu adımı atlayabilirsiniz.

İzleyin [Homebrew yükleme yönergeleri](https://docs.brew.sh/Installation.html) yükleyin.

#### <a name="download-the-latest-package"></a>En son paketini indirin
Yüklediğiniz .zip dosyasını tüm gerekli araçları ve DevKit geliştirme için paketleri içerir.

> [!div class="button"]
[İndir](https://aka.ms/devkit/prod/installpackage/mac/latest)

.Zip dosyasını aşağıdaki araçları ve paketleri içerir. Bazı bileşenleri yüklü zaten varsa, betik algılamak ve onları atlayın.

* Node.js ve Yarn: otomatik görevler ve Kurulum komut dosyası için çalışma zamanı.
* [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest#a-namemacosinstall-on-macos): Azure kaynaklarını yönetmek için platformlar arası komut satırı deneyimi.
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code): DevKit geliştirme için basit kod düzenleyicisini.
* [Visual Studio Code uzantısı Arduino için](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Visual Studio Code Arduino geliştirme sağlar uzantısı.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): Arduino uzantısı dayanan aracı.
* DevKit Panosu paketi: zincirleri, kitaplıklar ve projeler için DevKit aracı.
* ST bağlantı yardımcı programı: Temel araçlar ve sürücüleri.

#### <a name="run-the-installation-script"></a>Yükleme komut dosyasını çalıştır

Bir Bulucu .zip bulun ve ayıklayın:

![macOS Bulucu](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-finder.png)

Terminal uygulamasını başlatın, .zip dosyasını ayıklayın ve Çalıştır klasörü bulun:

```bash
./install.sh
```

![macOS yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-install-sh.png)

> [!NOTE] 
> Homebrew izin hatası karşılıyorsa çalıştırmak `brew doctor` sabit elde edin. Denetleme [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#homebrew-permission-error-on-macos) daha fazla ayrıntı için.

Artık tüm gerekli araçları ve macOS için yüklü olan paketleri vardır.


## <a name="open-the-project-folder"></a>Proje klasörünü açın

### <a name="start-vs-code"></a>VS Code'u başlatın

DevKit bağlı emin olun. İlk VS Code'u başlatın ve DevKit bilgisayarınıza bağlayın. VS Code otomatik olarak DevKit bulur ve bir giriş sayfasını açar:

![Giriş sayfası](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

> [!NOTE] 
> Bazen, VS Code başlattığınızda, onu Arduino IDE veya ilgili Panosu paketi bulamıyor hatayla istenir. VS Code kapatıp Arduino IDE yeniden başlatın. VS kodu sonra bulun Arduino IDE yolun doğru.


### <a name="open-the-arduino-examples-folder"></a>Arduino örnekler klasörünü açın

Üzerinde **Arduino örnekler** sekmesinde, Gözat **örnekler MXCHIP AZ3166 için** > **AzureIoT**seçip **GetStarted**.

![Arduino örnekler sekmesi](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

Bölmesini kapatmak için görülüyorsa yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="provision-azure-services"></a>Azure hizmetlerini hazırlamanız

Çözüm penceresinde göreviniz çalıştırın `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision`.

VS Code terminal etkileşimli bir komut satırı aracılığıyla gerekli Azure hizmetleri sağlama sırasında size kılavuzluk eder:

![Etkileşimli komut satırı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-the-arduino-sketch"></a>Derleme ve Arduino taslak karşıya yükle

### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırmak için `task device-upload`.
2. Terminal yapılandırma modu girmenizi ister. Bunu yapmak için A düğmesini basılı tutun sonra push ve Sıfırla düğmesini bırakın. Ekran DevKit kimliği ve 'Configuration' görüntüler.

Bu alır bağlantı dizesini belirlemek için olduğundan `task cloud-provision` adım.

Ardından VS Code doğrulama ve Arduino taslak karşıya yükleme başlar:

![Doğrulama ve Arduino taslağın karşıya yükle](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE] 
> Bazen, alma hatası "hata: AZ3166: Bilinmeyen Paket". Bu panosudur nedeniyle paket dizini yenilenmedi. Bu kontrol [SSS adımları](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) çözmek için.

### <a name="macos"></a>macOS

1. DevKit yapılandırma moduna: düğmesi A, sonra anında iletme ve yayın Sıfırla düğmesini basılı tutun. Ekran 'Configuration' görüntüler.
2. Kullanım `Cmd+P` çalıştırmak için `task device-upload`.

Bu alır bağlantı dizesini belirlemek için olduğundan `task cloud-provision` adım.

Ardından VS Code doğrulama ve Arduino taslak karşıya yükleme başlar:

![cihaz karşıya yükleme](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE] 
> Bazen, alma hatası "hata: AZ3166: Bilinmeyen Paket". Bu panosudur nedeniyle paket dizini yenilenmedi. Bu kontrol [SSS adımları](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) çözmek için.


## <a name="test-the-project"></a>Projeyi test

VS açıp seri İzleyicisi ayarlamak için aşağıdaki adımları izleyerek kod içinde:

1. Tıklatın `COM[X]` sağ COM bağlantı noktası ile ayarlamak için durum çubuğunda word `STMicroelectronics`: ![com bağlantı noktası](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/com-port.png)

2. Seri İzleyicisi'ni açmak için durum çubuğunda güç Tak simgesine tıklayın: ![seri İzleyicisi](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution//connect-iothub/serial-monitor.png)

3. Durum çubuğu Baud hızı temsil eden sayı tıklatın ve kümesine `115200`: ![baud hızı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/baud-rate.png)

Aşağıdaki sonuçları gördüğünüzde örnek uygulama başarıyla çalıştırma:

* Seri İzleyicisi, aşağıdaki ekran görüntüsünde içerik olarak aynı bilgiyi görüntüler.
* LED MXChip IOT DevKit üzerinde yanıp sönen.

![VS code'da son çıktı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, bulabileceğiniz [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/). Ayrıca bize geri bildirim bu sayfada bir yorum bırakarak verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza yakalanan algılayıcı verilerini gönderilen ve IOT hub'ınıza MXChip IOT DevKit başarıyla bağlandı.

Azure IOT Hub ile çalışmaya başlama devam etmek için ve diğer IOT senaryolarını keşfetmeye bakın:

- [iothub-explorer ile bulut cihaz mesajlaşmasını yönetme](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [IoT Hub iletilerini Azure veri depolamaya kaydetme](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Azure IOT hub'ı gerçek zamanlı algılayıcı verilerini görselleştirmek için Power BI kullanın](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Azure IOT hub'ı gerçek zamanlı algılayıcı verilerini görselleştirmek için Web uygulamaları kullanma](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [IOT hub'ınızı algılayıcı verilerini Azure Machine Learning kullanarak tahmin hava durumu](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [iothub-explorer ile cihaz yönetimi](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Logic Apps ile uzaktan izleme ve bildirimler](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
