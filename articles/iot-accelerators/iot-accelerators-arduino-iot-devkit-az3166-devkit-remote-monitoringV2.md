---
title: 'Buluta IOT DevKit: IOT DevKit AZ3166 bağlanmak için Uzaktan izleme IOT Çözüm Hızlandırıcısı | Microsoft Docs'
description: Bu öğreticide, IOT DevKit AZ3166 üzerinde Uzaktan izleme IOT Çözüm Hızlandırıcısı izlenmesi ve görselleştirilmesi için sensörlerden durumunu göndermek nasıl öğrenin.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: e900b952ab9bb2054b9e4174670894027cdd2618
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969461"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IOT DevKit AZ3166 IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, DevKit çözüm hızlandırıcınız sensör verilerini göndermek için örnek uygulamayı çalıştırma konusunda bilgi edinin.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip nesnelerin interneti (IOT) çözümleri, Microsoft Azure hizmetlerinden yararlanan size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* DevKit Wi-Fi'a bağlandıktan
* Geliştirme ortamını hazırlama


## <a name="open-the-remotemonitoring-sample"></a>RemoteMonitoring örneği açın

1. Bağlı olup olmadığını DevKit bilgisayarınızdan bağlantısını kesin.

2. VS Code'u başlatın.

3. DevKit bilgisayarınıza bağlayın. VS Code, otomatik olarak, DevKit algılar ve aşağıdaki sayfa açılır:
  * DevKit giriş sayfası.
  * Arduino örnekler: DevKit ile kullanmaya başlamak için uygulamalı örnekler.

4. Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > AzureIoT**seçip **RemoteMonitoringv2**. Bu proje klasöründe ile yeni bir VS Code penceresinin açılır.

  ![Uzaktan izleme Proje Aç](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Bölmesini kapatmak için MSDN aboneliğine, yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="add-a-new-physical-device"></a>Yeni fiziksel bir cihaz ekleyin

Portalda, Git **cihazları** , tıklayın ve bölüm **+ yeni cihaz** düğmesi. 

![Yeni bir cihaz ekleme](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

*Yeni cihaz formu* doldurulması gerekir.
1. Tıklayın **fiziksel** içinde *cihaz türü* bölümü.
2. Kendi cihaz Kimliğimi (örneğin *MXChip* veya *AZ3166*).
3. Seçin **anahtarları otomatik olarak oluştur** içinde *kimlik doğrulama anahtarı* bölümü.
4. Tıklayın *Uygula* düğmesi.

![Yeni bir cihaz form ekleme](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Portal yeni cihaz sağlama işleminin tamamlanmasını bekleyin.

![Yeni bir cihaz sağlama ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Ardından, yeni cihaz yapılandırmasını gösterilir.
Kopyalama **bağlantı dizesi** oluşturulur.

![Cihaz bağlantı dizesi](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Bu bağlantı dizesi, sonraki bölümde kullanılacaktır.





## <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

Visual Studio Code için geri dönün: 

1. Kullanım `Ctrl+P` (macOS: `Cmd + P`) ve türü **görev yapılandırma cihaz bağlantısı**.

  ![Azure aboneliğinizi ve IOT Hub'ı seçin](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. Terminalde, kullanmak istediğiniz IOT cihaz bağlantı dizesini kullanmak isteyip istemediğinizi sorar. Seçin *Yeni Oluştur*, artık kopyalayıp yapıştırın.

  ![bağlantı dizesini yapıştırın](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. Terminal bazen yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı sonra anında iletme Sıfırla düğmesini bırakın ve sonra A. düğmesini bırakın Ekran DevKit Kimliğine ve 'Configuration' görüntüler.

  ![Cihaz DevKit ekranı](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > Bu öğreticinin son bölümde izlediyseniz bağlantı dizesini panonuza kaydedilmesi gerekir. Aksi durumda, Azure portalına gidin ve Uzaktan izleme kaynak grubunuz için IOT Hub bakın. Burada, IOT hub'ı bağlı cihazlar ve cihaz bağlantı dizesini kopyalayın görebilirsiniz.

  ![bağlantı dizesini arayın](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Artık, yeni fiziksel Cihazınızı VS Code bölümünde "Azure IOT Hub cihazları" görebilirsiniz:

![Yeni IOT Hub cihazı dikkat edin.](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Test projesi

Örnek uygulamayı çalıştırdığında IOT çözüm hızlandırıcılarınız DevKit sensör verilerini Wi-Fi gönderir. Sonuçları görmek için aşağıdaki adımları izleyin:

1. IOT çözüm hızlandırıcınız gidip tıklatın **PANO**.

2. IOT Çözüm Hızlandırıcısı konsolda DevKit algılayıcı durumunuzu görürsünüz. 

![IOT Çözüm Hızlandırıcıları sensör verisi](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Algılayıcı adı (AZ3166) tıklatırsanız, gerçek zamanlı MX yonga sensörlerden grafik görebileceğiniz bir Pano sağ tarafında bir sekme açılır.


## <a name="send-a-c2d-message"></a>C2D ileti gönderme
Uzaktan izleme v2 cihazda uzak yöntem çağırma olanak sağlar.
MX yonga örnek kod, algılayıcı seçildiğinde yöntemi bölümünde görebileceğiniz üç yöntem yayımlar.

![MX yonga yöntemleri](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

"LedColor" yöntemiyle MX yonga LED'lerini birinin rengini değiştirebilirsiniz. Bunu yapmak, cihazın onay kutusunu seçin ve zamanlama düğmesine tıklayın. 

![MX yonga yöntemleri](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Burada tüm yöntemleri görünen bir ad yazın ve uygulama açılır menüde ChangeColor adlı yöntemi seçin.

![Açılan MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Birkaç saniye içinde fiziksel MX yonganız LED'i RGB rengi değiştirmeniz gerekir (aşağıda bir düğme)

![Yol MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)


## <a name="change-device-id"></a>Cihaz Kimliğini değiştirme

IOT hub cihaz kimliği izleyerek değiştirebilirsiniz [bu kılavuzda](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).


## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanal aracılığıyla bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

IOT çözüm hızlandırıcılarınız DevKit cihaz bağlanıp algılayıcı verilerini görselleştirme öğrendiniz, önerilen sonraki adımlar şunlardır:

* [IOT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [MXChip IOT DevKit cihaz Microsoft IoT Central uygulamanızı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
