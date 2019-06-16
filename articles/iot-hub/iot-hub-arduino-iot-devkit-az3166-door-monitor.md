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
ms.openlocfilehash: a620b592a33f9de11de53d623d257f203da2157b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61370365"
---
# <a name="door-monitor"></a>Kapı İzleyicisi          

MXChip IOT DevKit yerleşik manyetik algılayıcıyı içerir. Bu projede, varlığı veya yokluğu yakındaki güçlü manyetik alanının--bu durumda, bir küçük, Sabit mıknatıs yakında algılayın.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede şunları öğrenirsiniz:
- MXChip IOT DevKit'ın manyetik algılayıcıyı yakındaki mıknatıs hareketini algılamak için nasıl kullanılacağını.
- E-posta adresinizi bildirim göndermek için SendGrid hizmetini kullanma

> [!NOTE]
> Bu proje hakkında pratik kullanım için aşağıdaki görevleri gerçekleştirin:
> - Bir mıknatıs bir kapısının köşesine bağlayın.
> - Kapı jamb mıknatıs yakın üzerinde DevKit bağlayın. Açma veya kapatma kapı alma olayın bir e-posta bildirimi kaynaklanan algılayıcı tetikler.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](iot-hub-arduino-iot-devkit-az3166-get-started.md) için:

* DevKit Wi-Fi'a bağlandıktan
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Biri yoksa, aşağıdaki yöntemlerden birini kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azure.microsoft.com/free/).
* Talep, [Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abonesi olması durumunda.

## <a name="deploy-the-sendgrid-service-in-azure"></a>Azure'da SendGrid hizmetini dağıtma

[SendGrid](https://sendgrid.com/) bir bulut tabanlı e-posta teslim platformudur. Bu hizmet, e-posta bildirimleri göndermek için kullanılır.

> [!NOTE]
> SendGrid hizmetini dağıttıysanız, doğrudan devam [Azure IOT Hub'ı Dağıtma](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>SendGrid dağıtım

Azure hizmetleri sağlamak için kullanın **azure'a Dağıt** düğmesi. Bu düğme, Microsoft Azure açık kaynak projelerine hızla ve kolayca dağıtımını sağlar.

Tıklayın **azure'a Dağıt** düğmeye. 

[![Azure’a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Azure hesabınızda oturum zaten imzalanmamışsa şimdi oturum açın. 

SendGrid kayıt formunu göreceksiniz.

![SendGrid dağıtım](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

Kayıt formunu tamamlayın:

   * **Kaynak grubu**: SendGrid hizmetini barındırmak için bir kaynak grubu oluşturun veya var olanı kullanın. Bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/manage-resource-groups-portal.md).

   * **Ad**: SendGrid hizmetinizin adı. Bilgisayarınızda yüklü olmayabilir diğer hizmetlerden farklı olan benzersiz bir ad seçin.

   * **Parola**: Hizmet, bu projedeki her şey için kullanılmayacak bir parola gerektirir.

   * **e-posta**: SendGrid hizmetini doğrulama Bu e-posta adresine gönderir.

Denetleme **panoya Sabitle** gelecekte bulmak bu uygulamayı kolaylaştırmak için seçeneğini belirleyin, ardından tıklayın **satın alma** oturum açma formunu göndermek için.
 
### <a name="sendgrid-api-key-creation"></a>SendGrid API anahtarı oluşturma

Dağıtım tamamlandıktan sonra buna tıklayın ve ardından **Yönet** düğmesi. SendGrid hesabı sayfanıza e-posta adresinizi doğrulamak gerek duyduğunuz görünür.

![SendGrid yönetme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

SendGrid sayfasında tıklayın **ayarları** > **API anahtarları** > **API anahtarı oluştur**.

![SendGrid API ilk oluşturma](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

Üzerinde **API anahtarı oluştur** giriş sayfasında **API anahtar adı** tıklatıp **oluştur & görünümü**.

![SendGrid API ikinci oluşturma](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

API anahtarınız, yalnızca bir kez görüntülenir. Kopyalayın ve bir sonraki adımda kullanılan güvenli bir biçimde saklayın emin olun.

## <a name="deploy-iot-hub-in-azure"></a>Azure IOT Hub'ı dağıtma

Aşağıdaki adımlar, diğer Azure IOT sağlayacak ilgili hizmetler ve bu proje için Azure işlevleri'ni dağıtma.

Tıklayın **azure'a Dağıt** düğmeye. 

[![Azure’a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Kaydolma formu görüntülenir.

![Iothub dağıtım](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

Kayıt formunda alanları doldurun.

   * **Kaynak grubu**: SendGrid hizmetini barındırmak için bir kaynak grubu oluşturun veya var olanı kullanın. Bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/manage-resource-groups-portal.md).

   * **IOT hub'ı adı**: IOT hub'ınızın adı. Bilgisayarınızda yüklü olmayabilir diğer hizmetlerden farklı olan benzersiz bir ad seçin.

   * **IOT Hub Sku'sunun**: (Abonelik başına tek teklifle sınırlıdır) F1 ücretsiz olarak kullanılabilir. Diğer fiyatlandırma bilgileri gördüğünüz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/iot-hub/).

   * **E-postasındaki**: Bu alan, SendGrid hizmetini ayarlarken kullandığınız aynı e-posta adresi olmalıdır.

Denetleme **panoya Sabitle** gelecekte bulmak bu uygulamayı kolaylaştırmak için seçeneğini belirleyin, ardından tıklayın **satın alma** sonraki adıma devam etmeye hazır olduğunuzda.
 
## <a name="build-and-upload-the-code"></a>Derleme ve kodu yükleyin

Ardından, VS code'da örnek kod ve gerekli Azure hizmetleri sağlama.

### <a name="start-vs-code"></a>VS Code'u başlatın

- DevKit olduğundan emin olun **değil** bilgisayarınıza bağlı.
- VS Code'u başlatın.
- DevKit bilgisayarınıza bağlayın.

> [!NOTE]
> VS Code'u başlatın, bu Arduino IDE veya ilgili Pano paketi bulunamıyor belirten bir hata iletisi alabilirsiniz. Bu hatayı almaya devam ederseniz Kapat VS Code, Arduino IDE yeniden başlatma ve VS Code, Arduino IDE yolun doğru bulun.

### <a name="open-arduino-examples-folder"></a>Arduino örnekler Klasör Aç

Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > AzureIoT**seçip **DoorMonitor**. Bu eylem, proje klasöründe yeni bir VS Code penceresinin açılır.

![Mini solution örnekleri](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

Örnek uygulama komut paletinden da açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: Örnekler**.

### <a name="provision-azure-services"></a>Azure hizmetleri sağlama

Çözüm penceresinde görev sağlama bulut çalıştırın:
- Tür `Ctrl+P` (macOS: `Cmd+P`).
- Girin `task cloud-provision` sağlanan metin kutusunda.

VS Code terminalde, etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size yol gösterir. Tüm aynı öğeleri de daha önce sağlanan istendiğinde listeden seçin [Azure IOT Hub'ı Dağıtma](#deploy-iot-hub-in-azure).

![Bulut sağlama](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Sayfa yükleme durumu Azure'da oturum açmaya çalışırken kilitleniyor alıyorsa, [IOT DevKit SSS bölümünü "oturum açma sırasında eğişiklikler sayfasında"](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) bu sorunu çözmek için. 

### <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

Ardından, cihaz için kodu yükleyin.

#### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırılacak `task device-upload`.

2. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için A düğmesini basılı anında iletme ve Sıfırla düğmesini bırakın. Ekran DevKit kimlik numarasını ve word görüntüler *yapılandırma*.

#### <a name="macos"></a>Mac OS

1. DevKit yapılandırma moduna: A tuşunu basılı tutun, sonra anında iletme ve Sıfırla düğmesini bırakın. Ekran 'Configuration' görüntüler.

2. Tıklayın `Cmd+P` çalıştırılacak `task device-upload`.

#### <a name="verify-upload-and-run-the-sample-app"></a>Doğrulayın, karşıya yükleme ve örnek uygulamayı çalıştırma

Hizmetinden alınan bağlantı dizesini [sağlama Azure Hizmetleri](#provision-azure-services) adımı şimdi ayarlayın. 

VS Code sonra doğrulama ve Arduino karşıya yükleme başlar Devkit'e tasarlayın.

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Bazen, alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket"hata iletisi. Pano paket dizinini doğru şekilde yenilenmez bu hata oluşur. Bu hatayı gidermek için başvurmak [geliştirme IOT DevKit SSS bölümünü](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Test projesi

Program, önce DevKit bir kararlı manyetik alan olduğu durumda olduğunda başlatır.

Başlatma sonra `Door closed` ekranında görüntülenir. Manyetik alanında bir değişiklik olduğunda durumu değişir `Door opened`. Kapı durumu değişiklikleri her zaman, bir e-posta bildirimi alırsınız. (Bu e-posta iletilerini alınması beş dakika kadar sürebilir.)

![Algılayıcı yakın mıknatıs: Kapı kapalı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "algılayıcı yakın mıknatıs: Kapı kapalı")

![Algılayıcı uzağa mıknatıs taşındı: Kapak Açık](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "mıknatıs uzağa algılayıcı taşındı: Kapı açıldı")

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bağlanın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

DevKit cihaz, Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı öğrendiniz sahip ve bir e-posta göndermek için SendGrid hizmeti kullanılır. Önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözüm hızlandırıcısına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
