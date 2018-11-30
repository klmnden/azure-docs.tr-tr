---
title: Uzaktan izleme çözüm hızlandırıcısına - Azure IOT DevKit bağlayın | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda IOT DevKit AZ3166 cihazda sensörlerden alınan telemetri gönderecek şekilde öğrenmek için Uzaktan izleme çözüm hızlandırıcısının izlenmesi ve görselleştirilmesi için.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: isacabe
ms.openlocfilehash: 7f67868f6220ab2940aa8ac4d4bf24f82191cc22
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620260"
---
# <a name="connect-an-iot-devkit-device-to-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını için bir IOT DevKit cihazı bağlayın

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu nasıl yapılır kılavuzunda, IOT DevKit Cihazınızda bir örnek uygulamayı çalıştırma işlemini göstermektedir. Örnek kod telemetri çözüm hızlandırıcınız DevKit cihazda sensörlerden alınan gönderir.

[IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Azure IOT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) Visual Studio code'da. [Projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip IOT çözümlerine yardımcı olmak için örnek uygulamalar içerir.

## <a name="prerequisites"></a>Önkoşullar

İzleyin [IOT alma DevKet ile çalışmaya başlama Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) ve yalnızca aşağıdaki bölümleri tamamlayın:

* Donanım hazırlama
* Wi-Fi yapılandırma
* DevKit kullanmaya başlayın
* Geliştirme ortamını hazırlama

## <a name="open-the-sample"></a>Örnek Aç

Uzaktan izleme örnek VS Code'da açmak için:

1. Bilgisayarınıza, IOT DevKit olmadığından emin olun. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tıklayın `F1` komut paleti, türü ve select açmak için **IOT Workbench: örnekler**. Ardından **IOT DevKit** tablosu olarak.

1. Bulma **Uzaktan izleme** tıklatıp **açık örnek**. Proje klasörünü gösteren yeni bir VS Code penceresinin açılır:

  ![IOT Workbench, select Uzaktan izleme örneği](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-the-device"></a>Cihazı yapılandırma

DevKit cihazınızın IOT Hub cihaz bağlantı dizesini yapılandırmak için:

1. IOT DevKit içine geçiş **yapılandırma modunu**:

    * Düğmesini basılı **A**.
    * Anında iletme ve yayın **sıfırlama** düğmesi.

1. Ekran DevKit Kimliğini görüntüler ve `Configuration`.

    ![IOT DevKit yapılandırma modu](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

1. Tuşuna **F1** komut paleti, türü ve select açmak için **IOT Workbench: cihaz > yapılandırma cihaz ayarlarından**.

1. Daha önce kopyaladığınız bağlantı dizesini yapıştırın ve basın **Enter** cihaz yapılandırmak için.

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturmak ve cihaz kodu yüklemek için:

1. Tuşuna **F1**' ** komut paleti, türü ve select açmak için **IOT Workbench: cihaz > cihaz yükleme**:

    ![IOT Workbench: Cihaz - > karşıya yükle](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-upload.png)

1. VS Code, derler ve kod DevKit cihazınıza yükler:

    ![IOT Workbench: Cihaz - > karşıya yüklendi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

1. DevKit cihaz yeniden başlatılır ve karşıya yüklediğiniz kodunu çalıştırır.

## <a name="test-the-sample"></a>Örnek test

DevKit cihaza yüklediğiniz örnek uygulamayı çalışır durumda olduğunu doğrulamak için aşağıdaki adımları tamamlayın:

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Uzaktan izleme çözümüne gönderilen telemetri görüntüleme

Örnek uygulama çalıştırıldığında DevKit cihaz çözüm hızlandırıcınız Wi-Fi algılayıcı verilerini telemetri gönderir. Telemetri görmek için:

1. Çözüm panonuza gidin ve tıklatın **cihazları**.

1. DevKit cihazınızın cihaz adına tıklayın. sağ taraftaki sekmesinde DevKit gerçek zamanlı olarak gelen telemetriyi görebilirsiniz:

    ![Azure IOT paketi sensör verisi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="control-the-devkit-device"></a>DevKit cihazı denetleme

Uzaktan izleme çözüm Hızlandırıcısını Cihazınızı uzaktan denetlemenize olanak tanır. Örnek kod görebilirsiniz üç yöntem uygulayan **yöntemi** bölümünde cihaz seçtiğinizde **cihazları** sayfası:

![IOT DevKit yöntemleri](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

DevKit LED'lerini birinin rengini değiştirmek için kullanın **LedColor** yöntemi:

1. Cihaz listesinden cihaz adını seçin ve tıklayın **işleri**:

    ![Bir iş oluşturma](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

1. Aşağıdaki değerleri kullanarak işleri yapılandırmak ve tıklatın **Uygula**:

    * Select iş: **Run yöntemi**
    * Yöntem adı: **LedColor**
    * İş adı: **ChangeLedColor**

    ![İş ayarları](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

1. Birkaç saniye sonra üzerinde DevKit RGB LED (Aşağıda, bir düğme) rengini değiştirir:

    ![IOT DevKit kırmızı yönlendirdi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilere devam etmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın.

Çözüm Hızlandırıcısını artık ihtiyacınız kalmadığında, seçerek ve ardından silme çözüm tıklayarak sağlanan çözümleri sayfasından silin:

![Çözümü sil](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Tüm sorunlarla karşılaşırsanız, bakın [IOT DevKit SSS'leri](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan izleme çözüm Hızlandırıcısını için DevKit cihaz bağlayamama öğrendiniz, bazı önerilen sonraki adımlar şunlardır:

* [Azure IOT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-accelerators/)
* [Kullanıcı arabirimini özelleştirme](iot-accelerators-remote-monitoring-customize.md)
* [IOT DevKit Azure IOT Central uygulamanızı bağlayın](../iot-central/howto-connect-devkit.md)