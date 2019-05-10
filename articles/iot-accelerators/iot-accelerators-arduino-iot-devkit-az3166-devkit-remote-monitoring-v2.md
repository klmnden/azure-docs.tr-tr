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
ms.openlocfilehash: 3551d088c1d02715bf9ace09d7eb0048bc10111e
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473484"
---
# <a name="connect-an-iot-devkit-device-to-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını için bir IOT DevKit cihazı bağlayın

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu nasıl yapılır kılavuzunda, IOT DevKit Cihazınızda bir örnek uygulamayı çalıştırma işlemini göstermektedir. Örnek kod telemetri çözüm hızlandırıcınız DevKit cihazda sensörlerden alınan gönderir.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Azure IOT cihaz Workbench](https://aka.ms/iot-workbench) veya [Azure IOT Araçları](https://aka.ms/azure-iot-tools) Visual Studio Code uzantısı paketinde. [Projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip IOT çözümlerine yardımcı olmak için örnek uygulamalar içerir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* İçindeki adımları izleyerek, DevKit hazırlama [IOT DevKit AZ3166 bulutta Azure IOT hub'a bağlanma](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

## <a name="open-sample-project"></a>Açık örnek proje

Uzaktan izleme örnek VS Code'da açmak için:

1. Bilgisayarınıza, IOT DevKit olmadığından emin olun. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Örnek Aç...** . Ardından **IOT DevKit** tablosu olarak.

1. Bulma **Uzaktan izleme** tıklatıp **açık örnek**. Proje klasörünü gösteren yeni bir VS Code penceresinin açılır:

   ![IOT Workbench, select Uzaktan izleme örneği](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-the-device"></a>Cihazı yapılandırma

DevKit cihazınızın IOT Hub cihaz bağlantı dizesini yapılandırmak için:

1. IOT DevKit içine geçiş **yapılandırma modunu**:

    * Düğmesini basılı **A**.
    * Anında iletme ve yayın **sıfırlama** düğmesi.

1. Ekran DevKit Kimliğini görüntüler ve `Configuration`.

    ![IOT DevKit yapılandırma modu](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

1. Tuşuna **F1** komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Cihaz ayarlarını yapılandırma > yapılandırma cihaz bağlantı dizesini**.

1. Daha önce kopyaladığınız bağlantı dizesini yapıştırın ve basın **Enter** cihaz yapılandırmak için.

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturmak ve cihaz kodu yüklemek için:

1. Tuşuna `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Cihaz kodu karşıya**:

1. VS Code, derler ve kod DevKit cihazınıza yükler:

    ![IOT Workbench: Cihaz - > karşıya yüklendi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

1. DevKit cihaz yeniden başlatılır ve karşıya yüklediğiniz kodunu çalıştırır.

## <a name="test-the-sample"></a>Örnek test

DevKit cihaza yüklediğiniz örnek uygulamayı çalışır durumda olduğunu doğrulamak için aşağıdaki adımları tamamlayın:

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Uzaktan izleme çözümüne gönderilen telemetri görüntüleme

Örnek uygulama çalıştırıldığında DevKit cihaz çözüm hızlandırıcınız Wi-Fi algılayıcı verilerini telemetri gönderir. Telemetri görmek için:

1. Çözüm panonuza gidin ve tıklatın **Device Explorer**.

1. DevKit cihazınızın cihaz adına tıklayın. sağ taraftaki sekmesinde DevKit gerçek zamanlı olarak gelen telemetriyi görebilirsiniz:

    ![Azure IOT paketi sensör verisi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="control-the-devkit-device"></a>DevKit cihazı denetleme

Uzaktan izleme çözüm Hızlandırıcısını Cihazınızı uzaktan denetlemenize olanak tanır. Örnek kod görebilirsiniz üç yöntem uygulayan **yöntemi** bölümünde cihaz seçtiğinizde **Device Explorer** sayfası:

![IOT DevKit yöntemleri](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

DevKit LED'lerini birinin rengini değiştirmek için kullanın **LedColor** yöntemi:

1. Cihaz listesinden cihaz adını seçin ve tıklayın **işleri**:

    ![Bir iş oluşturma](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

1. Aşağıdaki değerleri kullanarak işleri yapılandırmak ve tıklatın **Uygula**:

   * İşi seçin: **Run yöntemi**
   * Yöntem adı: **LedColor**
   * İş Adı: **ChangeLedColor**

     ![İş ayarları](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

1. Birkaç saniye sonra üzerinde DevKit RGB LED (Aşağıda, bir düğme) rengini değiştirir:

    ![IOT DevKit kırmızı yönlendirdi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilere devam etmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın.

Çözüm Hızlandırıcısını artık ihtiyacınız kalmadığında, seçerek ve ardından silme çözüm tıklayarak sağlanan çözümleri sayfasından silin:

![Çözümü sil](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Tüm sorunlarla karşılaşırsanız, bakın [IOT DevKit SSS'leri](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan izleme çözüm Hızlandırıcısını için DevKit cihaz bağlayamama öğrendiniz, bazı önerilen sonraki adımlar şunlardır:

* [Azure IOT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-accelerators/)
* [Kullanıcı arabirimini özelleştirme](iot-accelerators-remote-monitoring-customize.md)
* [IOT DevKit Azure IOT Central uygulamanızı bağlayın](../iot-central/howto-connect-devkit.md)