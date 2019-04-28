---
title: Buluta--IOT DevKit IOT MXChip DevKit Azure IOT Hub'ına bağlanma | Microsoft Docs
description: Bu öğreticide, algılayıcılar durumunu IOT DevKit AZ3166 için Azure IOT Uzaktan izleme çözüm Hızlandırıcısını göndereceğinizi öğrenin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: ae8dc263e08528c6e3b3bae8c779162c96d51f43
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61324637"
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-remote-monitoring-solution-accelerator"></a>MXChip IOT DevKit Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı

Bu öğreticide, Azure IOT Uzaktan izleme çözüm Hızlandırıcısını sensör verilerini göndermek için DevKit örnek uygulamayı çalıştırma konusunda bilgi edinin.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip nesnelerin interneti (IOT) çözümleri, Microsoft Azure hizmetlerinden yararlanan size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* DevKit Wi-Fi'a bağlandıktan
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Biri yoksa, bu iki yöntemden biri kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azure.microsoft.com/free/)

* Talep, [Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abonesi iseniz

## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Bir Azure IOT Uzaktan izleme çözüm Hızlandırıcısını oluşturma

1. Git [Azure IOT Çözüm Hızlandırıcıları site](https://www.azureiotsolutions.com/) tıklatıp **yeni çözüm oluşturma**.

   ![Azure IOT Çözüm Hızlandırıcısı türü seçin](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)

   > [!WARNING]
   > Bir IOT Uzaktan izleme çözüm Hızlandırıcısını oluşturduktan sonra varsayılan olarak, bu örnek bir S2 IOT hub'ı oluşturur. Bu IOT hub cihaz büyük sayısı kullanılmazsa, S2 ' S1'e sürümüne düşürürseniz ve artık ihtiyacınız olmadığında ilgili IOT Hub Ayrıca, silinebilir böylece IOT Uzaktan izleme çözüm Hızlandırıcısını Sil öneririz. 

2. Seçin **Uzaktan izleme**.

3. Çözüm adını girin, bir abonelik ve bir bölge seçin ve ardından **çözüm oluşturma**. Çözüm sağlanacak biraz sürebilir.
  
   ![Çözüm oluştur](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. Tamamlandığında sağladıktan sonra tıklayın **başlatma**. Bazı sanal cihazlar çözüm için sağlama işlemi sırasında oluşturulur. Tıklayın **CİHAZLARI** için bunları gözden geçirin.

   ![Pano](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  
   ![Konsol](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. Tıklayın **bir CİHAZ eklemek**.

6. Tıklayın **yeni Ekle** için **özel cihaz**.
  
   ![Yeni cihaz ekleme](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. Tıklayın **Kimliğimi kendi cihaz Kimliğimi tanımlamama izin ver**, girin `AZ3166`ve ardından **Oluştur**.
  
   ![Cihaz kimliği oluşturma](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. Not **IOT Hub ana bilgisayar**, tıklatıp **Bitti**.

## <a name="open-the-remotemonitoring-sample"></a>RemoteMonitoring örneği açın

1. Bağlı olup olmadığını DevKit bilgisayarınızdan bağlantısını kesin.

2. VS Code'u başlatın.

3. DevKit bilgisayarınıza bağlayın. VS Code, otomatik olarak, DevKit algılar ve aşağıdaki sayfa açılır:

   * DevKit giriş sayfası.
   * Arduino örnekler: İle DevKit kullanmaya başlamak için uygulamalı örnekler sağlar.

4. Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > AzureIoT**seçip **RemoteMonitoring**. Bu proje klasöründe ile yeni bir VS Code penceresinin açılır.

   > [!NOTE]
   > Bölmesini kapatmak için MSDN aboneliğine, yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: Örnekler**.

## <a name="provision-required-azure-services"></a>Gerekli Azure hizmetleri sağlama

Çözüm penceresinde göreviniz çalışmasını `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision` sağlanan metin kutusunda.

VS Code terminalde, etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size yol gösterir.

![Azure kaynaklarını hazırlama](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

1. Kullanım `Ctrl+P` (macOS: `Cmd + P`) ve türü **görev yapılandırma cihaz bağlantısı**.

2. Terminal alır, bir bağlantı dizesi kullanmak isteyip istemediğinizi soran `task cloud-provision` adım. Kendi cihaz bağlantı dizesi 'Yeni Oluştur...' tıklayarak de giriş

3. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı anında iletme ve Sıfırla düğmesini bırakın. Ekran DevKit Kimliğine ve 'Configuration' görüntüler.

   ![Giriş dizesi.](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Sonra `task config-device-connection` bittiğinde tıklayın `F1` VS Code komutları yüklemek ve seçmek için `Arduino: Upload`. VS Code, doğrulama ve Arduino taslak karşıya yükleme başlar.
  
   ![Doğrulama ve Arduino taslağın karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

## <a name="test-the-project"></a>Test projesi

Örnek uygulama çalıştırıldığında, Azure IOT Uzaktan izleme çözüm Hızlandırıcısını DevKit sensör verilerini WiFi üzerinden gönderir. Sonuçları görmek için aşağıdaki adımları izleyin:

1. Azure IOT Uzaktan izleme çözüm Hızlandırıcısını için gidin ve tıklayın **PANO**.

2. Uzaktan izleme çözüm Konsolu DevKit algılayıcı durumunuzu görürsünüz.

   ![Azure IOT Uzaktan izleme çözüm Hızlandırıcısını sensör verisi](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>Cihaz Kimliğini değiştirme

Sabit kodlanmış değiştirmek istiyorsanız **AZ3166** kodda bir özelleştirilmiş cihaz kimliği için görüntülenen kod satırının değiştirme [Uzaktan izleme örnek](https://github.com/Microsoft/devkit-sdk/blob/master/AZ3166/src/libraries/AzureIoT/examples/RemoteMonitoring/RemoteMonitoring.ino#L23).

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [IOT Geliştirici Seti SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini Görselleştirme ve DevKit cihaz, Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-suite/)

* [Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)

* [IOT Geliştirici Seti](https://microsoft.github.io/azure-iot-developer-kit/) 
