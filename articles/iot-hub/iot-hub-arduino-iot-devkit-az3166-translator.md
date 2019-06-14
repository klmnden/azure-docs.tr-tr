---
title: Azure işlevleri ve Bilişsel hizmetler kullanarak bir IOT DevKit translator oluştur | Microsoft Docs
description: Mikrofon üzerinde bir IOT DevKit bir sesli ileti alırsınız ve ardından İngilizce çevrilmiş metin işleme için Azure Bilişsel hizmetler için kullanın.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 12/19/2018
ms.author: liydu
ms.openlocfilehash: df7e7b426a8c85c8051d7f588c706a6f8811e183
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60518913"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>IOT DevKit AZ3166 dil translator yapmak için Azure işlevleri ve Bilişsel hizmetler ile kullanın

Bu makalede, bir dil translator olarak IOT DevKit kullanarak hale getirmeyi öğrenin [Azure Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/). Sesinizi kaydeder ve İngilizce metni DevKit ekranda gösterilen çevirir.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Azure IOT cihaz Workbench](https://aka.ms/iot-workbench) veya [Azure IOT Araçları](https://aka.ms/azure-iot-tools) Visual Studio Code uzantısı paketinde. [Projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) prototip IOT çözümlerine yardımcı olmak için örnek uygulamalar içerir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* İçindeki adımları izleyerek, DevKit hazırlama [IOT DevKit AZ3166 bulutta Azure IOT hub'a bağlanma](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

## <a name="create-azure-cognitive-service"></a>Azure Bilişsel hizmet oluşturma

1. Azure portalında **kaynak Oluştur** araması **konuşma**. Konuşma hizmeti oluşturmak için formu doldurun.
  ![Konuşma hizmeti](media/iot-hub-arduino-iot-devkit-az3166-translator/speech-service.png)

1. Konuşma hizmeti için yeni oluşturduğunuz tıklatın Git **anahtarları** bölümü kopyalayın ve Not **Key1** DevKit ona erişmek için.
  ![Anahtarları kopyalayın](media/iot-hub-arduino-iot-devkit-az3166-translator/copy-keys.png)

## <a name="open-sample-project"></a>Açık örnek proje

1. Kendi IOT DevKit olduğundan emin olun **bağlı** bilgisayarınıza. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Örnek Aç...** . Ardından **IOT DevKit** tablosu olarak.

1. IOT Workbench örnekler sayfasında bulma **DevKit Translator** tıklatıp **açık örnek**. Ardından örnek kodu indirmek için varsayılan yolu seçer.
  ![Açık örnek](media/iot-hub-arduino-iot-devkit-az3166-translator/open-sample.png)

## <a name="use-speech-service-with-azure-functions"></a>Konuşma hizmeti ile Azure işlevlerini kullanma

1. VS Code'da tıklayın `F1`yazın ve seçin **Azure IOT cihaz Workbench: Azure hizmetleri sağlama...** . ![Azure hizmetleri sağlama](media/iot-hub-arduino-iot-devkit-az3166-translator/provision.png)

1. Azure IOT Hub ve Azure işlevleri'ni Hazırlama işleminin adımlarını izleyin.
   ![Hazırlama adımları](media/iot-hub-arduino-iot-devkit-az3166-translator/provision-steps.png)

   Oluşturduğunuz Azure IOT Hub cihaz adını not alın.

1. Açık `Functions\DevKitTranslatorFunction.cs` ve cihaz adı ve konuşma hizmeti anahtarı not ettiğiniz aşağı aşağıdaki kod satırlarını güncelleştirin.
   ```csharp
   // Subscription Key of Speech Service
   const string speechSubscriptionKey = "";

   // Region of the speech service, see https://docs.microsoft.com/azure/cognitive-services/speech-service/regions for more details.
   const string speechServiceRegion = "";

   // Device ID
   const string deviceName = "";
   ```

1. Tıklayın `F1`yazın ve seçin **Azure IOT cihaz Workbench: Azure'a Dağıt...** . VS Code, yeniden dağıtım için onay isterse, tıklayın **Evet**.
   ![Uyarı dağıtma](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-warning.png)

1. Dağıtım başarılı olduğundan emin olun.
   ![Başarı dağıtma](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-success.png)

1. Azure portalında Git **işlev uygulamaları** bölümünde, yeni oluşturduğunuz Azure işlevi uygulaması bulun. Tıklayın `devkit_translator`, ardından **<> / işlev URL'sini Al** URL'yi kopyalamak için.
   ![İşlev URL'sini kopyalama](media/iot-hub-arduino-iot-devkit-az3166-translator/get-function-url.png)

1. URL'sini yapıştırın `azure_config.h` dosya.
   ![Azure yapılandırma](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-config.png)

   > [!NOTE]
   > İşlev uygulamasının düzgün şekilde çalışmaz, bu denetleyin [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) bu sorunu çözmek için bölüm.

## <a name="build-and-upload-device-code"></a>Derleme ve cihaz kodu yükleyin

1. Devkit'e geçiş **yapılandırma modunu** tarafından:
   * Düğmesini basılı **A**.
   * Tuşuna basın ve yayın **sıfırlama** düğmesi.

   Ekran görüntüleri DevKit kimliği görürsünüz ve **yapılandırma**.

   ![DevKit yapılandırma modu](media/iot-hub-arduino-iot-devkit-az3166-translator/devkit-configuration-mode.png)

1. Tıklayın `F1`yazın ve seçin **Azure IOT cihaz Workbench: Cihaz ayarlarını yapılandırma > yapılandırma cihaz bağlantı dizesini**. Seçin **IOT Hub cihaz bağlantı dizesini seçin** Devkit'e yapılandırmak için.
   ![Bağlantı dizesini yapılandırma](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string.png)

1. Başarıyla tamamlandığında bildirim görürsünüz.
   ![Yapılandırma bağlantı dizesini başarılı](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string-success.png)

1. Tıklayın `F1` seçin ve yeniden yazın **Azure IOT cihaz Workbench: Cihaz kodu karşıya**. Bu derleme başlar ve kodu Devkit'e yükleyin.
   ![Cihaz yükleme](media/iot-hub-arduino-iot-devkit-az3166-translator/device-upload.png)

## <a name="test-the-project"></a>Test projesi

Uygulama başlatma DevKit ekrandaki yönergeleri izleyin. Varsayılan kaynak dili Çince.

Çeviri için başka bir dil seçmek için:

1. Kurulum moduna girmek için düğmesi A tuşlarına basın.

2. Desteklenen tüm kaynak dilleri kaydırmak için B düğmesine basın.

3. Kaynak dil seçiminizi onaylamak için düğmesi A tuşlarına basın.

4. Tuşuna basın ve konuşma çalışırken düğmesi B basılı tutarken çeviri başlatmak için B düğmesini bırakın.

5. Ekranda İngilizce gösterir, çevrilmiş metni.

![Dilini seçmek için kaydırma](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Çeviri sonucu](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

Çeviri sonucu ekranında, aşağıdakileri yapabilirsiniz:

- A ve B kaydırın ve kaynak dili seçmek için düğmeler tuşuna basın.

- Konuşma B düğmesine basın. Ses gönderip çeviri metni almak için B düğmesini serbest bırakın.

## <a name="how-it-works"></a>Nasıl çalışır?

![Mini-Solution-Voice-to-tweet-Diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

IOT DevKit sesinizi kayıtlar ardından tetikleyici Azure işlevleri için bir HTTP isteği gönderir. Azure işlevleri, bilişsel hizmet konuşma çevirmeni API'sini çeviri yapmak için çağırır. Azure işlevleri çeviri metin aldıktan sonra cihaza bir C2D iletisi gönderir. Ardından çeviri ekranında görüntülenir.

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri ve Bilişsel hizmetler kullanarak bir translator IOT DevKit kullanma öğrendiniz. Bu nasıl yapılır makalesinde öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Visual Studio Code görevi bulut hükümlerine otomatikleştirmek için kullanın
> * Azure IOT cihaz bağlantı dizesini yapılandırma
> * Azure işlevini dağıtma
> * Sesli ileti çeviri test

Bilgi edinmek için diğer öğreticiler için geçin:

> [!div class="nextstepaction"]
> [IOT DevKit AZ3166 Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
