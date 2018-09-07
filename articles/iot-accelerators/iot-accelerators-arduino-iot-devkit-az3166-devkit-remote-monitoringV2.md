---
title: Buluta--IOT DevKit Uzaktan izleme IOT çözüm hızlandırıcısına IOT DevKit AZ3166 bağlayın | Microsoft Docs
description: Bu öğreticide, IOT DevKit AZ3166 üzerinde Uzaktan izleme IOT Çözüm Hızlandırıcısı izlenmesi ve görselleştirilmesi için sensörlerden durumunu göndermek nasıl öğrenin.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: d3175290c1a7fca5e35f4438392f29324868f1a3
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054925"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IOT DevKit AZ3166 IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, DevKit çözüm hızlandırıcınız sensör verilerini göndermek için örnek uygulamayı çalıştırma konusunda bilgi edinin.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip nesnelerin interneti (IOT) çözümleri, Microsoft Azure hizmetlerinden yararlanan size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Git aracılığıyla [Başlarken Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) ve **aşağıdaki bölümlerde yalnızca son**:

* Donanım hazırlama
* Wi-Fi yapılandırma
* DevKit kullanmaya başlayın
* Geliştirme ortamını hazırlama


## <a name="open-the-remotemonitoring-sample-in-vs-code"></a>VS Code'da RemoteMonitoring örneği açın

1. Bağlı olup olmadığını MXChip DevKit bilgisayarınızdan bağlantısını kesin.

2. VS Code'u başlatın.

3. MXChip DevKit bilgisayarınıza bağlayın.

4. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

 5. Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > AzureIoT**seçip **RemoteMonitoringv2**. Bu proje klasöründe ile yeni bir VS Code penceresinin açılır.

  > [!NOTE]
  > Görmüyorsanız **örnekler için MXCHIP**, kullanın `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) türü ve komut paletini açın **Arduino Panosu Manager**. Dosyayı seçin ve ardından arama **AZ3166** Pano Yöneticisi'ni içinde. Ardından yukarıdaki 5. adımı yineleyin ve örneklere bakın olmalıdır.

  ![Uzaktan izleme Proje Aç](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)

  > [!NOTE]
  > Bölmesini kapatmak için MSDN aboneliğine, yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="build-and-upload-the-device-code-to-your-mxchip"></a>Derleme ve cihaz kodu, MXChip için karşıya yükleme

1. Kullanım `Ctrl+P` (macOS: `Cmd + P`) ve türü **görev yapılandırma cihaz bağlantısı**.

  ![Azure aboneliğinizi ve IOT Hub'ı seçin](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. Terminalde, kullanmak istediğiniz IOT cihaz bağlantı dizesini kullanmak isteyip istemediğinizi sorar. Seçin *Yeni Oluştur*, artık kopyalayıp yapıştırın.

  ![bağlantı dizesini yapıştırın](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. Terminal bazen yapılandırma modunu girmenizi ister. Bunu yapmak için basılı **bir düğme**, ardından gönderin ve yayın **Sıfırla düğmesi** ve A. düğmesini bırakın Ekran DevKit Kimliğine ve 'Configuration' görüntüler.

  ![Cihaz DevKit ekranı](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > Bu öğreticinin son bölümde izlediyseniz bağlantı dizesini panonuza kaydedilmesi gerekir. Aksi durumda, Azure portalına gidin ve Uzaktan izleme kaynak grubunuz için IOT Hub bakın. Burada, IOT hub'ı bağlı cihazlar ve cihaz bağlantı dizesini kopyalayın görebilirsiniz.

  ![bağlantı dizesini arayın](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)

  Artık bağlı ve başarıyla MXChip Cihazınızı IOT hub'ına doğrulandı. Yeni fiziksel Cihazınızı VS Code bölümünde "Azure IOT Hub cihazları" görmek için yüklemelidir [Azure IOT Toolkit uzantısını.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) 

  Artık, yeni fiziksel Cihazınızı VS Code bölümünde "Azure IOT Hub cihazları" görebilirsiniz:

  ![Yeni IOT Hub cihazı dikkat edin.](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

4. Son olarak, MxChip IOT çözüm hızlandırıcınız verileri göndermeye başlamak için üzerine RemoteMonitoringV2.ino kod yükleyeceksiniz. Kullanım `Ctrl + Shift + P` (macOS: `Cmd + Shift + P`) ve türü **Arduino karşıya**. VS Code, ardından kod, MXChip üzerine karşıya yüklemeyi başlatmak ve tamamlandığında size bildirir. 

## <a name="test-the-project"></a>Test projesi

Örnek uygulama çalıştırıldığında, MXChip DevKit sensör verilerini IOT çözüm hızlandırıcılarınız Wi-Fi gönderir. Sonuçları görmek için aşağıdaki adımları izleyin:

1. IOT çözüm hızlandırıcınız gidip tıklatın **PANO**.

2. IOT Çözüm Hızlandırıcısı konsolda MXChip DevKit algılayıcı durumunuzu görürsünüz. 

![IOT Çözüm Hızlandırıcıları sensör verisi](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Algılayıcı adı (AZ3166) tıklatırsanız, gerçek zamanlı MXChip sensörlerden grafik görebileceğiniz bir Pano sağ tarafında bir sekme açılır.


## <a name="send-a-c2d-message"></a>C2D ileti gönderme
Uzaktan izleme v2 cihazda uzak yöntem çağırma olanak sağlar.
MX yonga örnek kod, algılayıcı seçildiğinde yöntemi bölümünde görebileceğiniz üç yöntem yayımlar.

![MX yonga yöntemleri](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

MX yonga "LedColor" yöntemiyle LED'lerini birinin rengini değiştirebilirsiniz. Bunu yapmak, cihazın onay kutusunu seçin ve zamanlama düğmesine tıklayın. 

![MX yonga yöntemleri](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Burada tüm yöntemleri görünen bir ad yazın ve uygulama açılır menüde ChangeColor adlı yöntemi seçin.

![Açılan MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Birkaç saniye içinde fiziksel MX yonganız RGB LED'i rengini değiştirmek (aşağıda bir düğme)

![LED MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanal aracılığıyla bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

IOT çözüm hızlandırıcılarınız DevKit cihaz bağlanıp algılayıcı verilerini görselleştirme öğrendiniz, önerilen sonraki adımlar şunlardır:

* [IOT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [MXChip IOT DevKit cihaz Microsoft IoT Central uygulamanızı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
* [IOT Geliştirici Seti](https://microsoft.github.io/azure-iot-developer-kit/)
