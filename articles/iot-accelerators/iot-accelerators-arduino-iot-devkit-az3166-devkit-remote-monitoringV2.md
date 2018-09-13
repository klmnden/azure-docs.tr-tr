---
title: Buluta--IOT DevKit Uzaktan izleme IOT çözüm hızlandırıcısına IOT DevKit AZ3166 bağlayın | Microsoft Docs
description: Bu öğreticide, IOT DevKit AZ3166 üzerinde Uzaktan izleme IOT Çözüm Hızlandırıcısı izlenmesi ve görselleştirilmesi için sensörlerden durumunu göndermek nasıl öğrenin.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: isacabe
ms.openlocfilehash: 32742b2a680370f443051e2d86f90d94e8632850
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720591"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IOT DevKit AZ3166 IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Çözüm hızlandırıcınız sensör verilerini göndermek için IOT DevKit örnek uygulamayı çalıştırma öğreneceksiniz.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Azure IOT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) Visual Studio code'da. Ve büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip nesnelerin interneti (IOT) çözümleri, Microsoft Azure hizmetlerinden yararlanan size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Git aracılığıyla [Başlarken Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) ve **aşağıdaki bölümlerde yalnızca son**:

* Donanım hazırlama
* Wi-Fi yapılandırma
* DevKit kullanmaya başlayın
* Geliştirme ortamını hazırlama

## <a name="open-the-remote-monitoring-sample-in-vs-code"></a>Uzaktan izleme örnek VS Code'da açın

1. Kendi IOT DevKit olduğundan emin olun **bağlı** bilgisayarınıza. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

2. Tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: örnekler**. Ardından **IOT DevKit** tablosu olarak.

3. Bulma **Uzaktan izleme** tıklatıp **açık örnek**. Proje klasöründe ile yeni bir VS Code penceresinin açılır.
  ![IOT Workbench, select Uzaktan izleme örneği](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-iot-hub-device-connection-string"></a>IOT Hub cihaz bağlantı dizesini yapılandırma

1. IOT DevKit içine geçiş **yapılandırma modunu**. Bunu yapmak için:
   * Düğmesini basılı **A**.
   * Anında iletme ve yayın **sıfırlama** düğmesi.

2. Ekran DevKit Kimliğine ve 'Configuration' görüntüler.
   
  ![IOT DevKit yapılandırma modu](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

3. Tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: cihaz > yapılandırma cihaz ayarlarından**.

4. Az önce kopyaladığınız tıklatın bağlantı dizesini yapıştırın `Enter` yapılandırmak için.

## <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

1. Tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: cihaz > cihaz yükleme**.
  ![IOT Workbench: Cihaz - > karşıya yükle](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-upload.png)

1. VS Code, ardından derlemeyi ve kod, Devkit'e karşıya yükleme başlar.
  ![IOT Workbench: Cihaz - > karşıya yüklendi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

## <a name="test-the-project"></a>Test projesi

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Uzaktan izleme çözümüne gönderilen telemetri görüntüleme

Örnek uygulama çalıştığında, Uzaktan izleme çözümünüzü DevKit sensör verilerini Wi-Fi gönderir. Sonuçları görmek için aşağıdaki adımları izleyin:

1. Çözüm panonuza gidin ve tıklatın **cihazları**.

2. ' A tıklayın sağ taraftaki sekmesinde, cihaz adı DevKit gerçek zamanlı sensör durumunu görebilirsiniz.
  ![Azure IOT paketi sensör verisi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="send-a-c2d-message"></a>C2D ileti gönderme

Uzaktan izleme çözümü, uzak yöntemi cihazda çağırmak sağlar. Sxample kod görebilirsiniz üç yöntem yayımlar **yöntemi** algılayıcı seçildiğinde bölümü.

![IOT DevKit yöntemleri](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

Bize DevKit "LedColor" yöntemiyle LED'lerini birinin rengini değiştirmek deneyin.

1. Cihaz listesinden cihaz adını seçin ve tıklayın **işleri**.

  ![Bir iş oluşturma](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

2. İşleri aşağıdaki gibi yapılandırın ve tıklayın **Uygula**.
  * Select iş: **Run yöntemi**
  * Yöntem adı: **LedColor**
  * İş adı: **ChangeLedColor**
  
  ![Proje ayarları](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

Birkaç saniye içinde DevKit RGB LED'i (Aşağıda, bir düğme) rengini değiştirmeniz gerekir.

![IOT DevKit kırmızı yönlendirdi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilere devam etmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın.

Çözüm Hızlandırıcısını artık ihtiyacınız kalmadığında, seçerek ve ardından silme çözüm tıklayarak sağlanan çözümleri sayfasından silin:

![Çözümü sil](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [IOT DevKit SSS'leri](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini Görselleştirme ve DevKit cihaz, Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Kullanıcı arabirimini özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [IOT DevKit Azure IOT Central uygulamanızı bağlayın](../iot-central/howto-connect-devkit.md)