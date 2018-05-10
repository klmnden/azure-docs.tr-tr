---
title: SendGrid hizmeti ve Azure işlevleri kullanılarak kapısı açıldığında bir e-posta alıyorsunuz | Microsoft Docs
description: Bir kapısı açıldığında algılamak ve bir e-posta bildirimi göndermek için Azure işlevleri kullanmak için manyetik algılayıcı izleyin.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: 4eb13a588f0ffd1377caf3ce9bac4886f052a22f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="door-monitor"></a>Kapı İzleyicisi          

MXChip IOT DevKit yerleşik manyetik algılayıcı içerir. Bu projede, varlığının veya yokluğunun yakındaki güçlü manyetik alanının--bu durumda, küçük bir gelen algıla. Sabit mıknatıs.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede, öğreneceksiniz:
- MXChip IOT DevKit'ın manyetik algılayıcı yakındaki mıknatıs hareketini algılamak için nasıl kullanılacağını.
- SendGrid hizmet e-posta adresinizi bildirim göndermek için nasıl kullanılacağını.

> [!NOTE]
> Bu proje pratik kullanımı için:
> - Mıknatıs bir kapı köşesine bağlayın.
> - Kapı jamb mıknatıs yakın üzerinde DevKit bağlayın. Açma veya kapatma kapı alma bir e-posta bildirimi olayın kaynaklanan algılayıcı tetikler.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Getting Started Guide]({{"/docs/get-started/" | absolute_url }}) için:

* Wi-Fi bağlı, DevKit sahip
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Bir yoksa, aşağıdaki yöntemlerden birini kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesap](https://azure.microsoft.com/free/).
* Talep, [Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) bir MSDN veya Visual Studio abone olması durumunda.

## <a name="deploy-sendgrid-service-in-azure"></a>Azure'da SendGrid hizmet dağıtma

[SendGrid](https://sendgrid.com/) bir bulut tabanlı e-posta teslim platformudur. Bu hizmet, e-posta bildirimleri göndermek için kullanılacak.

> [!NOTE]
> SendGrid hizmeti zaten dağıttıysanız, doğrudan devam [Azure IOT Hub'ı Dağıtma](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>SendGrid dağıtımı

Azure hizmetleri sağlamak için kullanmanız **Azure'a Dağıt** düğmesi. Bu düğme, açık kaynaklı proje Microsoft Azure için hızlı ve kolay dağıtımı sağlar.

Tıklatın **Azure'a Dağıt** düğmesi, aşağıda. 

[![Azure’a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Ardından aşağıdaki sayfayı görürsünüz.

> [!NOTE]
> Aşağıdaki sayfayı görmüyorsanız, ilk Azure hesabınızda oturum açmak gerekebilir.

Kayıt formunu tamamlayın:

  * **Kaynak grubu**: SendGrid hizmetini barındırmak için bir kaynak grubu oluşturabilir veya mevcut bir kullanabilirsiniz. Bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **Ad**: SendGrid hizmetiniz için ad. Yaptığınız diğer hizmetlerinden farklı benzersiz bir ad seçin.

  * **Parola**: Hizmet bu projede her şey için olmayacak bir parola gerektirir.

  * **E-posta**: SendGrid hizmet doğrulama Bu e-posta adresine gönderir.

  > [!NOTE]
  > Denetleme **panoya Sabitle** gelecekte bulmak bu uygulamayı kolaylaştırmak için seçeneği.
 
![SendGrid dağıtımı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

### <a name="sendgrid-api-key-creation"></a>SendGrid API anahtarı oluşturma

Dağıtım başarılı olduktan sonra onu tıklayın ve ardından **Yönet** düğmesi. SendGrid sayfasına yönlendirilir ve e-posta adresinizi doğrulamanız gerekir.

![SendGrid yönetme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

SendGrid sayfasında, tıklatın **ayarları** > **API anahtarları** > **API anahtarı oluştur**. Giriş **API anahtar adı** tıklatıp **oluşturma & görünümü**.

![SendGrid API önce oluşturun](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

![SendGrid API ikinci oluşturma](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

API anahtarınıza yalnızca bir kez görüntülenir. Kopyalayın ve sonraki adımda kullanılan gibi güvenli bir biçimde saklayın emin olun.

## <a name="deploy-iot-hub-in-azure"></a>Azure IOT hub'ı dağıtma

Aşağıdaki adımlar diğer Azure IOT hazırlayacağınız ilgili hizmetleri ve bu proje için Azure işlevleri dağıtın.

Tıklatın **Azure'a Dağıt** düğmesi, aşağıda. 

[![Azure’a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Ardından aşağıdaki sayfayı görürsünüz.

> [!NOTE]
> Aşağıdaki sayfayı görmüyorsanız, ilk Azure hesabınızda oturum açmak gerekebilir.

Kayıt formunu tamamlayın:

  * **Kaynak grubu**: SendGrid hizmetini barındırmak için bir kaynak grubu oluşturabilir veya mevcut bir kullanabilirsiniz. Bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **IOT hub'ı adı**: IOT hub'ınız için ad. Yaptığınız diğer hizmetlerinden farklı benzersiz bir ad seçin.

  * **IOT Hub Sku**: F1 (sınırlı bir abonelik başına) boş. Daha fazla fiyatlandırma bilgilerine bakın [fiyatlandırma ve ölçek katmanı](https://azure.microsoft.com/pricing/details/iot-hub/).

  * **E-posta gelen**: Bu SendGrid Servis ayarladığınızda kullandığınız aynı e-posta adresi olmalıdır.

  > [!NOTE]
  > Denetleme **panoya Sabitle** gelecekte bulmak bu uygulamayı kolaylaştırmak için seçeneği.
 
![Iothub dağıtımı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

## <a name="build-and-upload-the-code"></a>Derleme ve kod karşıya yükle

### <a name="start-vs-code"></a>VS Code'u başlatın

- DevKit olduğundan emin olun **değil** bilgisayarınıza bağlı.
- VS Code'u başlatın.
- DevKit bilgisayarınıza bağlayın.

> [!NOTE]
> VS Code'u başlatın, onu Arduino IDE veya ilgili Panosu paketi bulamıyor belirten bir hata iletisi alabilirsiniz. Bu hatayı alırsanız Kapat VS Code, Arduino IDE yeniden başlatma ve VS Code Arduino IDE yolu doğru bulun.

### <a name="open-arduino-examples-folder"></a>Arduino örnekler Klasör Aç

Sol tarafta genişletin **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 örnekler > AzureIoT**seçip **DoorMonitor**. Bu eylem, bu proje klasöründe ile yeni bir VS Code penceresi açar.

![Mini solution örnekleri](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

> [!NOTE]
> Örnek komut paletinden da açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

### <a name="provision-azure-services"></a>Azure hizmetlerini hazırlamanız

Çözüm penceresinde görev sağlama bulut çalıştırın:
- Tür `Ctrl+P` (macOS: `Cmd+P`).
- Girin `task cloud-provision` sağlanan metin kutusuna.

VS Code terminal etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size yol gösterir. Daha önce derlemenizde istendiğinde listesinden aynı öğelerin tümünü seçin [Azure IOT Hub'ı Dağıtma](#deploy-iot-hub-in-azure).

![Bulut sağlama](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Sayfa yükleme durumu için Azure oturum açmaya çalışırken askıda oluştuysa, [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) bu sorunu gidermek için. 

### <a name="build-and-upload-the-device-code"></a>Derleme ve aygıt kodu karşıya yükle

#### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırmak için `task device-upload`.
2. Terminal yapılandırma modu girmenizi ister. Bunu yapmak için A düğmesini basılı tutun sonra push ve Sıfırla düğmesini bırakın. Ekran DevKit kimlik numarasını ve word görüntüler *yapılandırma*.

Bu yordamı hizmetinden alınan bağlantı dizesini ayarlar [sağlama Azure Hizmetleri](#provision-azure-services) adım.

VS Code'da sonra doğrulama ve Arduino karşıya yükleme başlar DevKit tasarlayın:

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Bazen, alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Bu hata, Panosu paket dizinini doğru yenilenmemiş oluşur. Bu hatayı gidermek için bu başvuru [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

#### <a name="macos"></a>macOS

1. DevKit yapılandırma moduna: düğmesi A, sonra anında iletme ve yayın Sıfırla düğmesini basılı tutun. Ekran 'Configuration' görüntüler.
2. Kullanım `Cmd+P` çalıştırmak için `task device-upload`.

Bu yordamı hizmetinden alınan bağlantı dizesini ayarlar [sağlama Azure Hizmetleri](#provision-azure-services) adım.

VS Code'da sonra doğrulama ve Arduino karşıya yükleme başlar DevKit tasarlayın:

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Bazen, alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Bu hata, Panosu paket dizinini doğru yenilenmemiş oluşur. Bu hatayı gidermek için bu başvuru [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Projeyi test

DevKit varlığında kararlı bir manyetik alan olduğunda program ilk başlatır.

Başlatma sonra `Door closed` ekranında görüntülenir. Manyetik alanında bir değişiklik olduğunda, durum değişikliklerini `Door opened`. Kapı durum değişiklikleri her saat, bir e-posta bildirimi alırsınız. (Bu e-posta iletilerini alınması beş dakika kadar sürebilir.)

![Algılayıcı yakın mıknatıs: kapı kapalı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "algılayıcı yakın mıknatıs: kapı kapalı")

![Mıknatıs taşınmış çıktığınızda algılayıcı: kapı açılan](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "mıknatıs taşınmış çıktığınızda algılayıcı: kapı açılmış")

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanallar kullanarak bağlan:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı için DevKit aygıt bağlanmak ve bir e-posta göndermek için SendGrid hizmetini kullanmak nasıl öğrendiniz. Önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Azure IOT merkezi uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
