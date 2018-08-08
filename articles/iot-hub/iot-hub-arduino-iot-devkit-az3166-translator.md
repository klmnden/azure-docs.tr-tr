---
title: Azure işlevleri ve Bilişsel hizmetler kullanarak bir IOT DevKit translator oluştur | Microsoft Docs
description: Mikrofon üzerinde bir IOT DevKit bir sesli ileti alırsınız ve ardından İngilizce çevrilmiş metin işleme için Azure Bilişsel hizmetler için kullanın.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: acfff95afacfa1e5c75a799ba84d64cfa0579f66
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592100"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>IOT DevKit AZ3166 dil translator yapmak için Azure işlevleri ve Bilişsel hizmetler ile kullanın

Bu makalede, bir dil translator olarak IOT DevKit kullanarak hale getirmeyi öğrenin [Azure Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/). Sesinizi kaydeder ve İngilizce metni DevKit ekranda gösterilen çevirir.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre ve sensörlerden panosudur. Onu kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Microsoft Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümleri size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* DevKit Wi-Fi'a bağlandıktan
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Biri yoksa, bu iki yöntemden biri kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azure.microsoft.com/free/)
* Talep, [Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abonesi iseniz

## <a name="open-the-project-folder"></a>Proje klasörünü açın

İlk olarak, proje klasörü açın. 

### <a name="start-vs-code"></a>VS Code'u başlatın

- DevKit bilgisayarınıza bağlı olduğundan emin olun.

- VS Code'u başlatın.

- DevKit bilgisayarınıza bağlayın.

### <a name="open-the-arduino-examples-folder"></a>Arduino örnekler klasörü açın

Sol tarafındaki genişletme **ARDUINO ÖRNEKLER > MXCHIP AZ3166 için örnek > AzureIoT**seçip **DevKitTranslator**. Proje klasörünü görüntüleyen yeni bir VS Code penceresinin açılır. MXCHIP AZ3166 bölümü için örnekler göremiyorsanız, cihazınız düzgün bağlandığından ve VS Code'u yeniden başlatın emin olun.  

![IOT DevKit örnekleri](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

Komut paletinden bir örnek de açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="provision-azure-services"></a>Azure hizmetleri sağlama

Çözüm penceresinde yazın `Ctrl+P` (macOS: `Cmd+P`) girin `task cloud-provision`.

Terminal VS Code'da etkileşimli bir komut satırı gerekli tüm Azure hizmetleri sağlamak için size yol gösterir:

![Bulut sağlama görevi](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="deploy-the-azure-function"></a>Azure işlevini dağıtma

Kullanım `Ctrl+P` (macOS: `Cmd+P`) çalıştırılacak `task cloud-deploy` Azure işlevleri kodu dağıtmak için. Bu işlem, genellikle 2 ila 5 dakika sürer.

![Bulut dağıtım görevi](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Azure işlevini başarıyla dağıtıldıktan sonra işlev uygulamanızın adını azure_config.h dosyasıyla doldurun. Gidebilirsiniz [Azure portalında](https://portal.azure.com/) bulmak için:

![Azure işlev uygulaması adı bulunamadı](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Azure işlevi düzgün çalışmaz, denetleyin [IOT DevKit SSS sayfasındaki "Azure işlevi için karmaşıklık error"](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) bu sorunu çözmek için.

## <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

1. Kullanım `Ctrl+P` (macOS: `Cmd+P`) çalıştırılacak `task config-device-connection`.

2. Terminal alır bağlantı dizesini kullanmak isteyip istemediğinizi sorar `task cloud-provision` adım. Seçim yaparak kendi cihaz bağlantı dizesini girebilirsiniz **'Yeni Oluştur...'**

3. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı anında iletme ve Sıfırla düğmesini bırakın. Ekran DevKit Kimliğine ve 'Configuration' görüntüler.

   ![Doğrulama ve Arduino taslağın karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Sonra `task config-device-connection` bitirdiğinizde tıklatın `F1` VS Code komutları yüklemek ve seçmek için `Arduino: Upload`, daha sonra VS Code doğrulama başlatır ve Arduino karşıya tasarlayın.

   ![Doğrulama ve Arduino taslağın karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

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

Bir Azure işlevi tetiklemek için sesinizi ardından HTTP gönderileri Arduino taslak kayıtları isteyin. Bilişsel hizmet konuşma çevirmeni API'sini çeviri yapmak için Azure işlevi çağırır. Azure işlevi çeviri metin aldıktan sonra cihaza bir C2D (bulut-cihaz) iletisi gönderir. Ardından çeviri ekranında görüntülenir.

## <a name="change-device-id"></a>Cihaz Kimliğini değiştirme

Azure IOT hub'da kayıtlı varsayılan cihaz kimliği **AZ3166**. Cihaz kimliği değiştirmek için bkz. nasıl [DevKit için IOT cihaz kimliği özelleştirme](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki Adımlar

Azure işlevleri ve Bilişsel hizmetler kullanarak bir translator IOT DevKit kullanma öğrendiniz. Bu nasıl yapılır makalesinde öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Visual Studio Code görevi bulut hükümlerine otomatikleştirmek için kullanın
> * Azure IOT cihaz bağlantı dizesini yapılandırma
> * Azure işlevini dağıtma
> * Sesli ileti çeviri test

Bilgi edinmek için diğer öğreticiler için geçin:

> [!div class="nextstepaction"]
> [IOT DevKit AZ3166 Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
