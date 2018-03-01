---
title: "Bulut için IOT DevKit: IOT DevKit AZ3166 Azure IOT Hub'ına bağlanmak | Microsoft Docs"
description: "Bu öğreticide, algılayıcılar durumunu izleme ve görselleştirme için IOT DevKit AZ3166 üzerinde Azure IOT paketi için gönderme konusunda bilgi edinin."
services: iot-hub
documentationcenter: 
author: liydu
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: b43061f5af3e836ba3f0b37eb11b351a769890be
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-suite-for-remote-monitoring"></a>Azure IOT paketi Uzaktan izleme için IOT DevKit AZ3166 Bağlan

Bu öğreticide, Azure IOT paketi algılayıcı verileri göndermek için DevKit üzerinde bir örnek uygulamayı çalıştırma öğrenin.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre birimleri ve algılayıcılar panosudur. Bunun için kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile gelir [projeleri katalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Microsoft Azure hizmetlerini yararlanmak prototip nesnelerin interneti (IOT) çözümleri size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* Wi-Fi bağlı, DevKit sahip
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Bir yoksa, bu iki yöntemden biri kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azureinfo.microsoft.com/us-freetrial.html)
* Talep, [Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abone olduğunda

## <a name="create-an-azure-iot-suite"></a>Bir Azure IOT paketi oluşturma

1. Git [Azure IOT paketi site](https://www.azureiotsuite.com/) tıklatıp **yeni bir çözüm oluşturmak**.
  ![Azure IOT paketi türünü seçin](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)
  > [!NOTE]
  > Bir IOT paketi oluşturduktan sonra varsayılan olarak, bu örnek S2 IOT hub'ı oluşturur. Bu IOT hub cihaz yoğun sayısı kullanılmazsa, S1 için S2 düşürmek ve artık ihtiyacınız olduğunda ilgili IOT hub'ı aynı zamanda, silinebilir şekilde IOT paketi silmek öneririz. 

2. Seçin **Uzaktan izleme**.

3. Çözüm adı girin, bir abonelik ve bir bölge seçin ve ardından **çözüm oluşturma**. Çözüm sağlanacak biraz zaman alabilir.
  ![Çözüm oluşturma](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. Sağlama tamamlandıktan sonra tıklatın **başlatma**. Bazı sanal cihazlar çözüm için sağlama işlemi sırasında oluşturulur. Tıklatın **AYGITLARI** bunları kullanıma için. ![Pano](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  ![konsol](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. Tıklatın **bir CİHAZ ekleme**.

6. Tıklatın **yeni Ekle** için **özel cihaz**.
  ![Yeni aygıt ekleme](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. Tıklatın **kendi cihaz Kimliğimi tanımlamama izin ver**, girin `AZ3166`ve ardından **oluşturma**.
  ![Cihaz kimliği ile oluşturma](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. Not **IOT Hub ana bilgisayar adına**, tıklatıp **Bitti**.

## <a name="open-the-remotemonitoring-sample"></a>RemoteMonitoring örneği açın

1. Bağlandıysa DevKit bilgisayarınızdan bağlantısını kesin.

2. VS Code'u başlatın.

3. DevKit bilgisayarınıza bağlayın. VS Code otomatik olarak, DevKit algılar ve aşağıdaki sayfalarda açar:
  * DevKit giriş sayfası.
  * Arduino örnekler: DevKit ile çalışmaya başlamak için uygulamalı örnekleri.

4. Sol tarafta genişletin **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 örnekler > AzureIoT**seçip **RemoteMonitoring**. Bu proje klasöründe ile yeni bir VS Code penceresi açar.
  > [!NOTE]
  > Bölmesini kapatmak için görülüyorsa yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="provision-required-azure-services"></a>Gerekli Azure hizmetlerini hazırlamanız

Çözüm penceresinde göreviniz çalıştırın `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision` sağlanan metin kutusunda:

VS Code terminal etkileşimli bir komut satırı aracılığıyla gerekli Azure hizmetleri sağlama sırasında size kılavuzluk eder:

![Azure kaynaklarını hazırlama](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Derleme ve aygıt kodu karşıya yükle

1. Kullanım `Ctrl+P` (macOS: `Cmd + P`) ve türü **görev config cihaz bağlantısı**.

2. Terminal alır bağlantı dizesi kullanmak isteyip istemediğinizi sorar `task cloud-provision` adım. 'Yeni Oluştur...' i tıklatarak da kendi cihaz bağlantı dizesi bir giriş

3. Terminal yapılandırma modu girmenizi ister. Bunu yapmak için A düğmesini basılı tutun sonra push ve Sıfırla düğmesini bırakın. Ekran DevKit kimliği ve 'Configuration' görüntüler.
  ![Giriş bağlantı dizesi](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Sonra `task config-device-connection` bitirdiğinizde tıklatın `F1` VS Code komutları yüklemek ve seçmek için `Arduino: Upload`, VS Code doğrulama başlar ve Arduino karşıya taslak: ![doğrulama ve Arduino taslağın karşıya yükle](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

## <a name="test-the-project"></a>Projeyi test

Örnek uygulama çalıştırıldığında DevKit algılayıcı verilerini Wi-Fi Azure IOT paketi gönderir. Sonuç görmek için aşağıdaki adımları izleyin:

1. Azure IOT paketi gidin ve tıklayın **PANO**.

2. Azure IOT paketi çözümünün konsolda DevKit algılayıcı durumunuzu görürsünüz.

![Azure IOT paketi algılayıcı verileri](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>Değişiklik cihaz kimliği

IOT hub cihaz kimliği izleyerek değiştirebileceğiniz [bu kılavuzda](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/). Ve sabit kodlanmış değiştirmek istiyorsanız **AZ3166** özelleştirilmiş kodda cihaz kimliği için. [Burada](https://github.com/Microsoft/devkit-sdk/blob/master/AZ3166/src/libraries/AzureIoT/examples/RemoteMonitoring/RemoteMonitoring.ino#L23) değiştirebileceğiniz kod satırı vardır.

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanaldan bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT paketi DevKit aygıt bağlanmak ve algılayıcı verileri görselleştirmek öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Paketi'ne Genel Bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Microsoft IoT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)
