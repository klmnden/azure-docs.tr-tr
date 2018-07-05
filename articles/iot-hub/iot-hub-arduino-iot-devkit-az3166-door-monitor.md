---
title: SendGrid service ve Azure işlevleri'ni kullanarak kapısı açıldığında bir e-posta alırsınız. | Microsoft Docs
description: Bir kapısı açıldığında ne zaman gerçekleştiğini algılayın ve bir e-posta bildirimi göndermek için Azure işlevleri'ni kullanmak için manyetik algılayıcıyı izleme.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: 25cb3ba53c663a642f0871becbfbcab39d521c67
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437724"
---
# <a name="door-monitor"></a>Kapı İzleyicisi          

MXChip IOT DevKit yerleşik manyetik algılayıcıyı içerir. Bu projede, varlığı veya yokluğu yakındaki güçlü manyetik alanının--bu durumda, küçük bir gelen algılayın. Sabit mıknatıs.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede şunları öğrenirsiniz:
- MXChip IOT DevKit'ın manyetik algılayıcıyı yakındaki mıknatıs hareketini algılamak için nasıl kullanılacağını.
- E-posta adresinizi bildirim göndermek için SendGrid hizmetini kullanma

> [!NOTE]
> Bu proje için bir pratik Kullanım:
> - Bir mıknatıs bir kapısının köşesine bağlayın.
> - Kapı jamb mıknatıs yakın üzerinde DevKit bağlayın. Açma veya kapatma kapı alma olayın bir e-posta bildirimi kaynaklanan algılayıcı tetikler.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](iot-hub-arduino-iot-devkit-az3166-get-started.md) için:

* DevKit Wi-Fi'a bağlandıktan
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Biri yoksa, aşağıdaki yöntemlerden birini kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azure.microsoft.com/free/).
* Talep, [Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abonesi olması durumunda.

## <a name="deploy-sendgrid-service-in-azure"></a>SendGrid Azure hizmeti dağıtma

[SendGrid](https://sendgrid.com/) bir bulut tabanlı e-posta teslim platformudur. Bu hizmet, e-posta bildirimleri göndermek için kullanılır.

> [!NOTE]
> SendGrid hizmetini dağıttıysanız, doğrudan devam [Azure IOT Hub'ı Dağıtma](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>SendGrid dağıtım

Azure hizmetleri sağlamak için kullanın **azure'a Dağıt** düğmesi. Bu düğme, Microsoft Azure açık kaynak projelerine hızla ve kolayca dağıtımını sağlar.

Tıklayın **azure'a Dağıt** düğmesi, aşağıda. 

[![Azure’a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Sonra aşağıdaki sayfayı görürsünüz.

> [!NOTE]
> Şu sayfaya görmüyorsanız, öncelikle Azure hesabınızda oturum açmak gerekebilir.

Kayıt formunu tamamlayın:

  * **Kaynak grubu**: SendGrid hizmetini barındırmak için bir kaynak grubu oluşturun veya var olanı kullanın. Bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **Ad**: SendGrid hizmetinizin adı. Bilgisayarınızda yüklü olmayabilir diğer hizmetlerden farklı olan benzersiz bir ad seçin.

  * **Parola**: hizmeti bu projede herhangi bir şey için olmayacak bir parola gerektirir.

  * **E-posta**: SendGrid hizmet doğrulama Bu e-posta adresine gönderir.

  > [!NOTE]
  > Denetleme **panoya Sabitle** gelecekte bulmak bu uygulamayı kolaylaştırmak için seçeneği.
 
![SendGrid dağıtım](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

### <a name="sendgrid-api-key-creation"></a>SendGrid API anahtarı oluşturma

Dağıtım başarılı olduktan sonra buna tıklayın ve ardından **Yönet** düğmesi. SendGrid sayfanız alınır ve e-posta adresinizi doğrulamamız gerekiyor.

![SendGrid yönetme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

SendGrid sayfasında tıklayın **ayarları** > **API anahtarları** > **API anahtarı oluştur**. Giriş **API anahtar adı** tıklatıp **oluştur & görünümü**.

![SendGrid API ilk oluşturma](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

![SendGrid API ikinci oluşturma](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

API anahtarınız, yalnızca bir kez görüntülenir. Kopyalayın ve bir sonraki adımda kullanılan güvenli bir biçimde saklayın emin olun.

## <a name="deploy-iot-hub-in-azure"></a>Azure IOT Hub'ı dağıtma

Aşağıdaki adımlar, diğer Azure IOT sağlayacak ilgili hizmetler ve bu proje için Azure işlevleri'ni dağıtma.

Tıklayın **azure'a Dağıt** düğmesi, aşağıda. 

[![Azure’a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Sonra aşağıdaki sayfayı görürsünüz.

> [!NOTE]
> Şu sayfaya görmüyorsanız, öncelikle Azure hesabınızda oturum açmak gerekebilir.

Kayıt formunu tamamlayın:

  * **Kaynak grubu**: SendGrid hizmetini barındırmak için bir kaynak grubu oluşturun veya var olanı kullanın. Bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **IOT hub'ı adı**: IOT hub'ınızın adı. Bilgisayarınızda yüklü olmayabilir diğer hizmetlerden farklı olan benzersiz bir ad seçin.

  * **IOT Hub Sku'sunun**: F1 (sınırlı bir abonelik başına) ücretsizdir. Daha fazla fiyatlandırma bilgilerine gördüğünüz [fiyatlandırma ve ölçek katmanı](https://azure.microsoft.com/pricing/details/iot-hub/).

  * **E-postasındaki**: Bu SendGrid hizmetini ayarlarken kullandığınız aynı e-posta adresi olmalıdır.

  > [!NOTE]
  > Denetleme **panoya Sabitle** gelecekte bulmak bu uygulamayı kolaylaştırmak için seçeneği.
 
![Iothub dağıtım](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

## <a name="build-and-upload-the-code"></a>Derleme ve kodu yükleyin

### <a name="start-vs-code"></a>VS Code'u başlatın

- DevKit olduğundan emin olun **değil** bilgisayarınıza bağlı.
- VS Code'u başlatın.
- DevKit bilgisayarınıza bağlayın.

> [!NOTE]
> VS Code'u başlatın, bu Arduino IDE veya ilgili Pano paketi bulunamıyor belirten bir hata iletisi alabilirsiniz. Bu hatayı almaya devam ederseniz Kapat VS Code, Arduino IDE yeniden başlatma ve VS Code, Arduino IDE yolun doğru bulun.

### <a name="open-arduino-examples-folder"></a>Arduino örnekler Klasör Aç

Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > AzureIoT**seçip **DoorMonitor**. Bu eylem, proje klasöründe yeni bir VS Code penceresinin açılır.

![Mini solution örnekleri](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

> [!NOTE]
> Komut paletinden örnek de açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

### <a name="provision-azure-services"></a>Azure hizmetleri sağlama

Çözüm penceresinde görev sağlama bulut çalıştırın:
- Tür `Ctrl+P` (macOS: `Cmd+P`).
- Girin `task cloud-provision` sağlanan metin kutusunda.

VS Code terminalde, etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size yol gösterir. Tüm aynı öğeleri de daha önce sağlanan istendiğinde listeden seçin [Azure IOT Hub'ı Dağıtma](#deploy-iot-hub-in-azure).

![Bulut sağlama](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Sayfa yükleme durumu Azure'da oturum açmaya çalışırken kilitleniyor alıyorsa, [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) bu sorunu çözmek için. 

### <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

#### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırılacak `task device-upload`.
2. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı anında iletme ve Sıfırla düğmesini bırakın. Ekran DevKit kimlik numarasını ve word görüntüler *yapılandırma*.

Bu yordam hizmetinden alınan bağlantı dizesini ayarlar [sağlama Azure Hizmetleri](#provision-azure-services) adım.

VS Code sonra doğrulama ve Arduino karşıya yükleme başlar Devkit'e taslak:

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Bazen, alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Pano paket dizinini doğru şekilde yenilenmez bu hata oluşur. Bu hatayı gidermek için bu başvuru [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

#### <a name="macos"></a>macOS

1. Yapılandırma moduna DevKit: düğme A, sonra anında iletme ve yayın Sıfırla düğmesini basılı tutun. Ekran 'Configuration' görüntüler.
2. Kullanım `Cmd+P` çalıştırılacak `task device-upload`.

Bu yordam hizmetinden alınan bağlantı dizesini ayarlar [sağlama Azure Hizmetleri](#provision-azure-services) adım.

VS Code sonra doğrulama ve Arduino karşıya yükleme başlar Devkit'e taslak:

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Bazen, alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Pano paket dizinini doğru şekilde yenilenmez bu hata oluşur. Bu hatayı gidermek için bu başvuru [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Test projesi

Program, önce DevKit bir kararlı manyetik alan olduğu durumda olduğunda başlatır.

Başlatma sonra `Door closed` ekranında görüntülenir. Manyetik alanında bir değişiklik olduğunda durumu değişir `Door opened`. Kapı durumu değişiklikleri her zaman, bir e-posta bildirimi alırsınız. (Bu e-posta iletilerini alınması beş dakika kadar sürebilir.)

![Algılayıcı yakın mıknatıs: kapı kapalı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "algılayıcı yakın mıknatıs: kapı kapalı")

![Mıknatıs uzağa algılayıcı taşındı: kapı açılan](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "mıknatıs uzağa algılayıcı taşındı: kapı açıldı")

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bağlanın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

DevKit cihaz, Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı ve e-posta göndermek için SendGrid hizmetini kullanmak öğrendiniz. Önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözüm hızlandırıcısına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
