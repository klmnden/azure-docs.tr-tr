---
title: Azure işlevleri ile bir Twitter iletisini alma | Microsoft Docs
description: Hareket algılayıcısı kullanın sallayabilir algılamak ve belirttiğiniz bir diyez etiketi ile rastgele bir tweet bulmak için Azure işlevleri'ni kullanın
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: dc4ff35ff04680e8635d54c25212c8ae639ae472
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60779908"
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Sallama, bir Tweet için sallayın--Azure işlevleri ile bir Twitter iletisini alma

Bu projede hareket algılayıcısı Azure işlevleri'ni kullanarak bir olayı tetiklemek için nasıl kullanılacağını öğrenin. Uygulama, Arduino taslak yapılandırdığınız bir #hashtag ile rastgele bir tweet alır. Tweet DevKit ekranda görüntüler.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* Wi-Fi, DevKit bağlandınız.
* Geliştirme ortamınızı hazırlama.

Etkin bir Azure aboneliği. Yoksa, aşağıdaki yöntemlerden birini kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azure.microsoft.com/free/)
* Talep, [Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abonesi iseniz

## <a name="open-the-project-folder"></a>Proje klasörünü açın

Proje klasörünü açarak işleme başlayın. 

### <a name="start-vs-code"></a>VS Code'u başlatın

* DevKit bilgisayarınıza bağlı olduğundan emin olun.

* VS Code'u başlatın.

* DevKit bilgisayarınıza bağlayın.

   > [!NOTE]
   > VS Code başlatırken Arduino IDE veya ilgili Pano paket edilemeyecek bir hata iletisi alabilirsiniz bulunamadı. Bu hata oluşursa VS Code kapatın ve Arduino IDE yeniden başlatın. VS Code artık Arduino IDE yolun doğru bulun.

### <a name="open-the-arduino-examples-folder"></a>Arduino örnekler klasörü açın

Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > AzureIoT**seçip **ShakeShake**. Proje klasörünü görüntüleyen yeni bir VS Code penceresinin açılır. MXCHIP AZ3166 bölümü göremiyorsanız, cihazınız düzgün bağlandığından ve Visual Studio Code yeniden emin olun.  
![mini solution örnekleri](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

Komut paletinden da örnek projeyi açabilirsiniz. Tıklayın `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: Örnekler**.

## <a name="provision-azure-services"></a>Azure hizmetleri sağlama

Çözüm penceresinde göreviniz çalışmasını `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision`.

VS Code terminalde, etkileşimli bir komut satırı, gerekli Azure hizmetleri sağlama aracılığıyla size kılavuzluk eder:

![Bulut sağlama](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Sayfa yükleme durumu Azure'da oturum açmaya çalışırken kilitleniyor alıyorsa, [IOT DevKit SSS "oturum açma sayfasının yanıt vermemeye başlıyor" adımda](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure).
 
## <a name="modify-the-hashtag"></a>#Hashtag değiştirme

Açık `ShakeShake.ino` ve bu kod satırı bulun:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Dize değiştirin `iot` , tercih edilen bir diyez etiketi ile küme ayraçları içinde. DevKit daha sonra bu adımda belirttiğiniz diyez etiketi içeren rastgele bir tweet alır.

## <a name="deploy-azure-functions"></a>Azure İşlevleri’ni dağıtma

Kullanım `Ctrl+P` (macOS: `Cmd+P`) çalıştırılacak `task cloud-deploy` Azure işlevleri kodu dağıtmaya başlamak için:

![Bulutu dağıtma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> Bazen, Azure işlevi düzgün çalışmayabilir. Bu durumda, bu sorunu çözmek için işaretleyin ["derleme hatası" IOT DevKit SSS bölümünü](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Derleme ve cihaz kodu yükleyin

Ardından, derleme ve cihaz kodu yükleyin.

### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırılacak `task device-upload`.

2. Terminal yapılandırma modunu girmenizi ister. Bunu yapmak için:

   * A tuşunu basılı tutun

   * Anında iletme ve Sıfırla düğmesini bırakın.

3. Ekran DevKit Kimliğine ve 'Configuration' görüntüler.

### <a name="macos"></a>macOS

1. DevKit yapılandırma moduna:

   A tuşunu basılı tutun, sonra anında iletme ve Sıfırla düğmesini bırakın. Ekran 'Configuration' görüntüler.

2. Kullanım `Cmd+P` çalıştırılacak `task device-upload` hizmetinden alınan bağlantı dizesini ayarlamak için `task cloud-provision` adım.

### <a name="verify-upload-and-run"></a>Doğrulayın, karşıya yükleyin ve çalıştırın

Bağlantı dizesini ayarlayalım artık doğrular ve uygulamayı yükler ardından çalıştırır. 

1. VS Code, doğrulama ve Arduino Taslak, Devkit'e karşıya yükleme başlar:

   ![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)

2. DevKit yeniden başlatır ve kod çalışmaya başlar.

Alabilirsiniz bir "hata: AZ3166: Bilinmeyen Paket"hata iletisi. Pano paket dizinini doğru şekilde yenilenmez bu hata oluşur. Bu sorunu çözmek için denetleme ["Bilinmeyen Paket" hatası IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Test projesi

Uygulama başlatmadan sonra tıklatın ve düğmesi A release ve yavaşça DevKit panonun sallayın. Bu eylem, daha önce belirtilen diyez etiketi bulunduğu rastgele bir tweet alır. Birkaç saniye içinde bir tweet DevKit ekranınızda görüntüler:

### <a name="arduino-application-initializing"></a>Arduino uygulama başlatılıyor...

![Arduino uygulama başlatılıyor](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Sallama için A tuşlarına basın...

![Sallama için ENTER tuşuna A](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Sallama hazır...

![Hazır sallama](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>İşleniyor...

![İşleniyor](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Okunacak B tuşuna basın...

![Okuma için ENTER tuşuna B](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Rastgele bir tweet görüntüle...

![Görüntüle-a-rastgele-tweet](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Bir yeniden düğmesine basın ve ardından için yeni bir tweet sallayın.
- Tweetin rest üzerinden ilerlemek için B düğmesine basın.

## <a name="how-it-works"></a>Nasıl çalışır?

![Diyagramı](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

Arduino taslak Azure IOT Hub'ına bir olay gönderir. Azure işlev uygulaması bu olay tetiklenir. Azure işlev uygulaması, Twitter'ın API'sine bağlanmak ve tweet almak için mantığı içerir. Daha sonra bir C2D tweet metni kaydırmıyor (bulut-cihaz) ileti ve cihaza geri gönderir.

## <a name="optional-use-your-own-twitter-bearer-token"></a>İsteğe bağlı: Kendi Twitter taşıyıcı belirteci kullanın

Test amacıyla, bu örnek proje, önceden yapılandırılmış bir Twitter taşıyıcı belirteci kullanır. Ancak, bir [hız sınırı](https://dev.twitter.com/rest/reference/get/search/tweets) her Twitter hesabı. Kendi belirteç kullanmayı göz önünde bulundurun istiyorsanız şu adımları izleyin:

1. Git [Twitter Geliştirici Portalı](https://dev.twitter.com/) yeni bir Twitter uygulamayı kaydedin.

2. [Tüketici anahtarı ve tüketici gizli dizilerini alma](https://support.yapsody.com/hc/en-us/articles/360003291573-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) uygulamanızın.

3. Kullanım [bazı yardımcı programı](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) iki Bu anahtarları bir Twitter taşıyıcı belirteci oluşturmak için.

4. İçinde [Azure portalında](https://portal.azure.com/){: hedef = "_blank"}, içine alma **kaynak grubu** ve Azure işlevi bulun (tür: App Service) "Sallayın, sallama" projeniz için. Ad, her zaman 'dize... sallayın' içeriyor.

   ![Azure işlevi](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Kodu güncelleştirme `run.csx` içinde **işlevleri > shakeshake cs** kendi belirteç ile:

   ```csharp
   string authHeader = "Bearer " + "[your own token]";
   ```
  
   ![twitter belirteci](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Dosyayı kaydedin ve tıklayın **çalıştırma**.

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Geri bildirim sağlamak veya sorunları gidermek nasıl. 

### <a name="problems"></a>Sorunları

Her adım başarıyla çalıştırıldı 'No Tweetleri' ekranı görüntüler varsa bir sorun, görebilirsiniz. Bu durum normalde, ilk kez dağıtma ve işlev uygulamasını herhangi bir yere birkaç saniye kadar bir dakika hazırlıksız başlatma için uygulamanın gerektirdiğinden örneği çalıştırmak gerçekleşir. 

Veya kodu çalıştırırken, uygulamanın yeniden başlatılması neden bazı blips vardır. Bu durum gerçekleştiğinde, cihaz uygulamasını tweet yakalama için bir zaman aşımı alabilirsiniz. Bu durumda, bir veya sorunu çözmek için bu yöntemlerin ikisi de deneyebilirsiniz:

1. Cihaz uygulamasını yeniden çalıştırmak için DevKit Sıfırla düğmesine tıklayın.

2. İçinde [Azure portalında](https://portal.azure.com/), oluşturduğunuz Azure işlevleri uygulamayı bulun ve yeniden başlatın:

   ![Azure işlevi yeniden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Geri Bildirim

Diğer sorunlar yaşıyorsanız, başvurmak [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

DevKit cihaz, Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı ve tweet almak öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözüm hızlandırıcısına genel bakış](https://docs.microsoft.com/azure/iot-suite/)