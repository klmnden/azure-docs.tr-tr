---
title: 'Bulut için IOT DevKit: Azure IOT paketi Uzaktan izleme v2 IOT DevKit AZ3166 bağlanma | Microsoft Docs'
description: Bu öğreticide, algılayıcılar durumunu IOT DevKit AZ3166 üzerinde Uzaktan izleme v2 ile Azure IOT paketi izleme ve görselleştirme için nasıl göndereceğinizi öğrenin.
services: iot-hub
documentationcenter: ''
author: isabelcabezasm
manager: ''
tags: ''
keywords: ''
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: 667e51acd5ac1367e185fb9bf9a7949a13e061af
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="connect-mxchip-iot-devkit-az3166-to-azure-iot-suite-for-remote-monitoring-v2"></a>MXChip IOT DevKit AZ3166 Azure IOT paketi Uzaktan izleme v2 için Bağlan


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, Azure IOT paketi algılayıcı verileri göndermek için DevKit üzerinde bir örnek uygulamayı çalıştırma öğrenin.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre birimleri ve algılayıcılar panosudur. Bunun için kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile gelir [projeleri katalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Microsoft Azure hizmetlerini yararlanmak prototip nesnelerin interneti (IOT) çözümleri size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* Wi-Fi bağlı, DevKit sahip
* Geliştirme ortamını hazırlama


## <a name="open-the-remotemonitoring-sample"></a>RemoteMonitoring örneği açın

1. Bağlandıysa DevKit bilgisayarınızdan bağlantısını kesin.

2. VS Code'u başlatın.

3. DevKit bilgisayarınıza bağlayın. VS Code otomatik olarak, DevKit algılar ve aşağıdaki sayfalarda açar:
  * DevKit giriş sayfası.
  * Arduino örnekler: DevKit ile çalışmaya başlamak için uygulamalı örnekleri.

4. Sol tarafta genişletin **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 örnekler > AzureIoT**seçip **RemoteMonitoringv2**. Bu proje klasöründe ile yeni bir VS Code penceresi açar.

  ![Uzaktan izleme Proje Aç](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Bölmesini kapatmak için görülüyorsa yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="add-a-new-physical-device"></a>Yeni bir fiziksel aygıt ekleme

Portalı'nda gidin **aygıtları** bölümünde ve orada tıklatın **+ yeni cihaz** düğmesi. 

![Yeni bir cihaz ekleme](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

*Yeni cihaz form* dolu olması gerekir.
1. Tıklatın **fiziksel** içinde *aygıt türü* bölümü.
2. Kendi cihaz kimliği tanımlayın (örneğin *MXChip* veya *AZ3166*).
3. Seçin **otomatik oluştur anahtarları** içinde *kimlik doğrulama anahtarı* bölümü.
4. Tıklatın *Uygula* düğmesi.

![Yeni bir cihaz form ekleme](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Yeni aygıt sağlama portalı tamamlanana kadar bekleyin.

![Yeni bir cihaz sağlama ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Ardından yeni aygıt yapılandırma gösterilecek.
Kopya **bağlantı dizesi** oluşturulur.

![Cihaz bağlantı dizesi](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Bu bağlantı dizesi bir sonraki bölümde kullanılır.





## <a name="build-and-upload-the-device-code"></a>Derleme ve aygıt kodu karşıya yükle

Visual Studio Code dönün: 

1. Kullanım `Ctrl+P` (macOS: `Cmd + P`) ve türü **görev config cihaz bağlantısı**.

  ![Azure aboneliğinizi ve IOT Hub'ı seçin](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. Terminal kullanmak istediğiniz IOT cihaz bağlantı dizesi kullanmak isteyip istemediğinizi sorar. Seçin *Yeni Oluştur*, şimdi kopyalayıp yapıştırın.

  ![bağlantı dizesini yapıştırın](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. Terminal bazen yapılandırma modu girmenizi ister. Bunu yapmak için A düğmesini basılı tutun sonra anında ve Sıfırla düğmesini bırakın ve ardından düğmesini A. bırakın Ekran DevKit kimliği ve 'Configuration' görüntüler.

  ![Cihaz DevKit ekranı](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > Bu öğreticinin son Kısım izlediyseniz bağlantı dizesi panonuza kaydedilmesi gerekir. Aksi durumda, Azure Portalı'na gidin ve Uzaktan izleme kaynak grubunuz için IOT Hub arayın. Burada, IOT Hub cihazları bağlı ve cihaz bağlantı dizesini kopyalayın görebilirsiniz.

  ![bağlantı dizesi arayın](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Artık, yeni fiziksel Cihazınızı "Azure IOT Hub cihazları" VS kod bölümünde görebilirsiniz:

![Yeni IOT Hub cihaz dikkat edin](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Projeyi test

Örnek uygulama çalıştırıldığında DevKit algılayıcı verilerini Wi-Fi Azure IOT paketi gönderir. Sonuç görmek için aşağıdaki adımları izleyin:

1. Azure IOT paketi gidin ve tıklayın **PANO**.

2. Azure IOT paketi çözümünün konsolda DevKit algılayıcı durumunuzu görürsünüz. 

![Azure IOT paketi algılayıcı verileri](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Algılayıcı adını (AZ3166) tıklatırsanız MX yonga algılayıcılar grafik gerçek zamanlı görebileceğiniz panonun sağ tarafında bir sekmesini açar.


## <a name="send-a-c2d-message"></a>C2D ileti gönderme
Uzaktan izleme v2 cihazda uzak yöntem çağırma olanak tanır.
MX yonga örnek kod algılayıcı seçildiğinde yöntemi bölümünde görebilirsiniz üç yöntem yayımlar.

![Yöntemleri MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

"LedColor" yöntemini kullanarak MX yonga LED'leri birini rengini değiştirebilirsiniz. Bunu yapmak için cihazın onay kutusunu seçin ve zamanlama düğmesine tıklayın. 

![Yöntemleri MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Burada tüm yöntemleri görünür, bir ad yazın ve uygulama açılır listede ChangeColor çağrılan yöntemi seçin.

![Aşağı açılan MX yonga](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Birkaç saniye içinde fiziksel MX yonga öncülük RGB rengi değiştirmeniz gerekir (aşağıda bir düğme)

![MX yonga neden](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)


## <a name="change-device-id"></a>Değişiklik cihaz kimliği

IOT hub cihaz kimliği izleyerek değiştirebileceğiniz [bu kılavuzda](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).


## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanaldan bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT paketi DevKit aygıt bağlanmak ve algılayıcı verileri görselleştirmek öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Paketi'ne Genel Bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Microsoft IoT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)
