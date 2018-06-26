---
title: Azure işlevini ve Bilişsel hizmetler kullanarak IOT DevKit Çeviricisi | Microsoft Docs
description: Kullanım mikrofon sesli mesaj almak üzere IOT DevKit ve İngilizce çevrilmiş metne işlemek için Azure Bilişsel hizmetler
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: ba2325272552a13d6e464797b1fb523415393100
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945947"
---
# <a name="use-iot-devkit-az3166-with-azure-function-and-cognitive-services-to-make-a-language-translator"></a>IOT DevKit AZ3166 dil Çeviricisi yapmak için Azure işlevi ve Bilişsel hizmetler ile kullanma

Bu makalede, bir dil Çeviricisi gibi IOT DevKit kullanarak yapmak öğrenin [Azure Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/). Sesiniz kaydeder ve DevKit ekranda gösterilen İngilizce metne çevirir.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre birimleri ve algılayıcılar panosudur. Bunun için kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile gelir [projeleri katalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Microsoft Azure hizmetlerini yararlanmak prototip nesnelerin interneti (IOT) çözümleri size yol gösterecek.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* Wi-Fi bağlı, DevKit sahip
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Bir yoksa, bu iki yöntemden biri kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azure.microsoft.com/free/)
* Talep, [Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abone olduğunda

## <a name="step-1-open-the-project-folder"></a>1. Adım Proje klasörünü açın

### <a name="a-start-vs-code"></a>A. VS Code'u başlatın

- DevKit PC'nize bağlı olduğundan emin olun.
- VS Code'u başlatın
- DevKit bilgisayarınıza bağlayın.

### <a name="b-open-the-arduino-examples-folder"></a>B. Arduino örnekler klasörünü açın

Sol tarafta genişletin **ARDUINO ÖRNEKLER > örnekler MXCHIP AZ3166 için > AzureIoT**seçip **DevKitTranslator**. Bu DEVKITTRANSLATOR proje klasöründen ile yeni bir VS Code penceresi açar.  

> [!NOTE]
> Örnekler MXCHIP AZ3166 bölüm için göremiyorsanız, Cihazınızı düzgün bir şekilde bağlı Visual Studio Code sağlayın ve yeniden başlatın.  

![IOT DevKit örnekleri](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

> [!NOTE]
> Örnek komut paletinden da açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="step-2-provision-azure-services"></a>2. Adım Azure hizmetlerini hazırlamanız

Çözüm penceresinde yazın `Ctrl+P` (macOS: `Cmd+P`) ve girin `task cloud-provision`.

Terminal VS kodu'nda etkileşimli bir komut satırı, gerekli tüm Azure hizmetleri sağlamaya yardımcı olur:

![Bulut sağlama görevi](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="step-3-deploy-azure-functions"></a>3. Adım Azure İşlevleri’ni dağıtma

Kullanım `Ctrl+P` (macOS: `Cmd+P`) çalıştırmak için `task cloud-deploy` Azure işlevleri kodunu dağıtmak için. Bu işlem genellikle tamamlanması 2 ile 5 dakika sürer:

![Bulut dağıtım görevi](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Azure işlevi başarıyla dağıtıldıktan sonra azure_config.h dosyayı işlevi uygulama adı ile doldurun. Gidebilirsiniz [Azure portal](https://portal.azure.com/) bulmak için:

![Azure işlevi uygulaması adı bulunamadı](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Azure işlevi düzgün çalışmaz, bu denetleyin [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) bunu çözmek için bölüm.

## <a name="step-4-build-and-upload-the-device-code"></a>4. Adım. Derleme ve aygıt kodu karşıya yükle

1. Kullanım `Ctrl+P` (macOS: `Cmd+P`) çalıştırmak için `task config-device-connection`.

2. Terminal alır bağlantı dizesi kullanmak isteyip istemediğinizi sorar `task cloud-provision` adım. Seçerek kendi cihaz bağlantı dizesini de girebilirsiniz **'Yeni Oluştur...'**

3. Terminal yapılandırma modu girmenizi ister. Bunu yapmak için A düğmesini basılı tutun sonra push ve Sıfırla düğmesini bırakın. Ekran DevKit kimliği ve 'Configuration' görüntüler.
  ![Doğrulama ve Arduino taslağın karşıya yükle](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Sonra `task config-device-connection` bitirdiğinizde tıklatın `F1` VS Code komutları yüklemek ve seçmek için `Arduino: Upload`, VS Code doğrulama başlar ve Arduino karşıya taslak: ![doğrulama ve Arduino taslağın karşıya yükle](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

## <a name="test-the-project"></a>Projeyi test

Uygulama başlatma DevKit ekrandaki yönergeleri izleyin. Varsayılan kaynak dili Çince ' dir.

Çeviri için başka bir dil seçmek için:

1. Kurulum modunu girmek için düğmesi A basın.
2. Desteklenen tüm kaynak dilleri kaydırmak için B düğmesine basın.
3. Kaynak dil seçiminizi onaylamak için düğmesi A basın.
4. Tuşuna basın ve konuşurken düğmesi B tutun, ardından çeviri başlatmak için B düğmesini bırakın.
5. Ekranında İngilizce gösterir çevrilmiş metin.

![Dilini seçmek için kaydırma](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Çeviri sonucu](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

Çeviri sonuç ekranında şunları yapabilirsiniz:

- Düğme A ve B kaydırın ve kaynak dilini seçmek için basın.
- Konuşma, ses göndermek ve çeviri metni almak için yayın için B düğmesine basın

## <a name="how-it-works"></a>Nasıl çalışır?

![Mini-Solution-Voice-to-tweet-Diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

Arduino taslak kayıtları sesinizi sonra tetikleyici Azure işlevleri için bir HTTP isteği gönderir. Azure işlevlerini bilişsel hizmet konuşma Çeviricisi çeviri yapmak için API çağırır. Azure işlevleri çeviri metin aldıktan sonra cihaza bir C2D iletisi gönderir. Ardından çeviri ekranında görüntülenir.

## <a name="change-device-id"></a>Değişiklik cihaz kimliği

Azure IOT hub'da kayıtlı varsayılan cihaz kimliği **AZ3166**. Bunu değiştirmek istiyorsanız, izleyin [yönergeleri burada](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanaldan bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki Adımlar

Artık IOT DevKit bir Çeviricisi gibi Azure işlevi ve Bilişsel hizmetler kullanarak yapın. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Visual Studio Code görevi bulut hükümleri otomatikleştirmek için kullanın
> * Azure IOT cihaz bağlantı dizesini yapılandırma
> * Azure işlevi dağıtma
> * Sesli ileti çeviri test

Bilgi edinmek için diğer öğreticileri için ilerleyin:

> [!div class="nextstepaction"]
> [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı IOT DevKit AZ3166 Bağlan](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
