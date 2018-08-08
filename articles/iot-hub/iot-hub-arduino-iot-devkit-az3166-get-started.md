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
ms.openlocfilehash: 6d5b0036bb44f301ea0b11e5d984fcd5b4bfac71
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599838"
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

Geliştirme ortamını ayarlama artık: araçlar ve göz alıcı IOT uygulamaları oluşturmanıza olanak paketleri. Windows veya macOS sürümü, işletim sisteminize göre seçebilirsiniz.

### <a name="windows"></a>Windows

Geliştirme ortamınızı hazırlama için yükleme paketi kullanmanızı öneririz. Herhangi bir sorunla karşılaşırsanız, izleyebilirsiniz [el ile IOT DevKit yükleme yönergelerini](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) bitti edinilir.

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
> Ortamınıza bağlı olarak bazen hata Arduino IDE yüklerken alırsınız. Bu durumda, deneyebilirsiniz [Arduino IDE ayrı ayrı yükleme](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) Install.cmd yeniden çalıştırın. Aksi takdirde Lütfen izleyin [el ile IOT DevKit yükleme yönergelerini](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) tüm gerekli araçlara ve paketleri yüklemek için.

#### <a name="install-drivers"></a>Sürücüleri yükleyin

Arduino uzantısı için VS Code, Arduino IDE üzerinde kullanır. Arduino IDE yüklediğiniz ilk kez buysa, ilgili sürücüleri yüklemeniz istenir.

![alma başlatıldı-sürücü](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Yükleme, internet hızına bağlı olarak yaklaşık 10 dakika sürer. Yükleme tamamlandıktan sonra masaüstünüzde Visual Studio Code ve Arduino IDE kısayolları bakın.

> [!NOTE] 
> Bazen, VS Code başlattığınızda, Arduino IDE veya ilgili Pano paketi bulunamıyor hatayla istenir. Bunu çözmek için VS Code kapatın ve Arduino IDE yeniden başlatın. VS kodu ardından bulun Arduino IDE yolun doğru.

### <a name="macos"></a>macOS

Geliştirme ortamınızı hazırlama için tek tıklamayla yükleme deneyimi kullanmanızı öneririz. Herhangi bir sorunla karşılaşırsanız, izleyebilirsiniz [el ile IOT DevKit yükleme yönergelerini](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) bitti edinilir.

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

Terminal uygulamasını başlatın, .zip dosyasını ayıklayın ve aşağıdaki adımları çalıştırın klasörü bulun:

```bash
./install.sh
```

![macOS yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-install-sh.png)

> [!NOTE] 
> Homebrew izin hatası karşılıyorsanız, çalıştırma `brew doctor` sorunu düzeltmesi için. Denetleme ["homebrew error" IOT DevKit SSS bölümünü](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#homebrew-permission-error-on-macos) daha fazla ayrıntı için.

Artık tüm gerekli araçlara ve macOS için yüklü paketleri vardır.

## <a name="open-the-project-folder"></a>Proje klasörünü açın

Proje klasörünü açarak işleme başlayın. 

### <a name="start-vs-code"></a>VS Code'u başlatın

DevKit değil bağlı olduğundan emin olun. VS Code'u başlatın ve ardından DevKit bilgisayarınıza bağlayın. VS Code, otomatik olarak DevKit bulur ve bir giriş sayfası açılır.

![Giriş sayfası](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

> [!NOTE] 
> Bazen, VS Code başlattığınızda, Arduino IDE veya ilgili Pano paketi bulunamıyor hatayla istenir. VS Code kapatın ve Arduino IDE yeniden başlatın. VS kodu ardından bulun Arduino IDE yolun doğru.

### <a name="open-the-arduino-examples-folder"></a>Arduino örnekler klasörü açın

Üzerinde **Arduino örnekler** sekmesinde, Gözat **MXCHIP AZ3166 için örnek** > **AzureIoT**seçip **GetStarted**.

![Arduino örnekler sekmesi](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

Bölmesini kapatmak için MSDN aboneliğine, yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="provision-azure-services"></a>Azure hizmetleri sağlama

Çözüm penceresinde göreviniz çalışmasını `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision`.

VS Code terminalde, etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size yol gösterir.

![Etkileşimli komut satırı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-the-arduino-sketch"></a>Derleme ve Arduino taslak karşıya yükleyin

Ardından, derleme ve Arduino taslak karşıya yükleyin.

### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırılacak `task device-upload`.

2. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı anında iletme ve Sıfırla düğmesini bırakın. Ekran DevKit kimliğine ve 'Configuration' görüntüler.

   Alır bağlantı dizesini ayarlayalım budur `task cloud-provision` adım.

   Daha sonra VS Code doğrulanıyor ve karşıya Arduino taslak başlatır.

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
   > Bazen, hata alırsınız "hata: AZ3166: Bilinmeyen Paket". Bu panonun nedeniyle, paket dizinini yenilenmez. Adımları iade [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) çözmek için.


## <a name="test-the-project"></a>Test projesi

VS Code'da açın ve seri İzleyicisi ayarlamak için aşağıdaki adımları izleyin:

1. Tıklayın `COM[X]` doğru COM bağlantı noktası için durum çubuğuna word `STMicroelectronics`.

   ![COM bağlantı noktası](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/com-port.png)

2. Seri İzleyicisi'ni açmak için durum çubuğunu Power Tak simgesine tıklayın.

   ![Seri İzleyicisi](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution//connect-iothub/serial-monitor.png)

3. Durum çubuğunda Baud hızı temsil eden bir sayıya tıklayın ve değerini `115200`.

   ![baud hızı](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/baud-rate.png)

4. Örnek uygulama, aşağıdaki sonuçları görmeniz başarıyla çalışıyor:

   * Seri İzleyici, aşağıdaki ekran görüntüsünde gösterilen içerik olarak aynı bilgileri görüntüler.
   
   * Üzerinde MXChip IOT DevKit RGB LED'i yanıp sönen.

   ![VS code'da son çıkış](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, bir çözümde denetleyebilirsiniz [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/). De bize geri bildirim bu sayfada bir yorum bırakarak tanıyabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza bir MXChip IOT DevKit başarıyla bağlandınız ve IOT hub'ınıza yakalanan sensör verilerini gönderdik.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
