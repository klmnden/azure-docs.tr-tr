---
title: "Azure işlevleri içeren bir Twitter ileti alma | Microsoft Docs"
description: "Hareket algılayıcısı sıkışması algılamak ve belirttiğiniz bir hashtag ile rastgele bir tweet bulmak için Azure işlevleri kullanmak için kullanın."
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
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: d9d03d35aa5d78d83e0f195c804cfe09fece3c07
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Sallama Tweet için sallama--Azure işlevleri içeren bir Twitter ileti almak!

Bu projede hareket algılayıcı Azure işlevleri kullanarak bir olay tetiklemek için nasıl kullanılacağını öğrenin. Uygulama, Arduino taslak yapılandırma #hashtag ile rastgele bir tweet alır. Tweet DevKit ekranda görüntüler.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* Wi-Fi, DevKit bağlandınız.
* Geliştirme ortamını hazırlayın.

Etkin bir Azure aboneliği. Yoksa, aşağıdaki yöntemlerden birini kaydedebilirsiniz:

* Etkinleştirme bir [Ücretsiz 30 günlük deneme Microsoft Azure hesabı](https://azureinfo.microsoft.com/us-freetrial.html)
* Talep, [Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) MSDN veya Visual Studio abone olduğunda

## <a name="open-the-project-folder"></a>Proje klasörünü açın

### <a name="start-vs-code"></a>VS Code'u başlatın

- DevKit olduğundan emin olun **değil** bilgisayarınıza bağlı.
- VS Code'u başlatın.
- DevKit bilgisayarınıza bağlayın.

VS Code otomatik olarak, DevKit bulur ve bir giriş sayfasını görüntüler:

![mini-solution-vscode](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_start.png)

> [!NOTE]
> VS Code başlatılırken Arduino IDE veya ilgili Panosu paketi olamaz bir hata iletisi alabilirsiniz bulundu. Bu hata oluşursa, VS Code kapatın ve Arduino IDE yeniden başlatın. VS Code şimdi Arduino IDE yolun doğru bulun.

### <a name="open-arduino-examples-folder"></a>Arduino örnekler Klasör Aç

Sol tarafta genişletin **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 örnekler > AzureIoT**seçip **ShakeShake**. Bu proje klasöründe içeren yeni bir VS Code pencere açılır.

![Mini solution örnekleri](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

Bölmesini kapatmak için görülüyorsa yeniden açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="provision-azure-services"></a>Azure hizmetlerini hazırlamanız

Çözüm penceresinde göreviniz çalıştırın `Ctrl+P` (macOS: `Cmd+P`) girerek `task cloud-provision`.

VS Code terminal etkileşimli bir komut satırı aracılığıyla gerekli Azure hizmetleri sağlama sırasında size kılavuzluk eder:

![Bulut sağlama](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Sayfa yükleme durumu için Azure oturum açmaya çalışırken askıda kalır, bunun için başvurun [SSS adım] ({{"/docs/faq/#page-hangs-when-log-in-azure" | 
 
## <a name="modify-the-hashtag"></a>#Hashtag değiştirme

Açık `ShakeShake.ino` ve bu kod satırı için bakın:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Dize Değiştir `iot` , tercih edilen diyez ile süslü ayraçlar içinde. DevKit daha sonra bu adımda belirttiğiniz diyez içeren rastgele bir tweet alır.

## <a name="deploy-azure-functions"></a>Azure İşlevleri’ni dağıtma

Kullanım `Ctrl+P` (macOS: `Cmd+P`) çalıştırmak için `task cloud-deploy` Azure işlevleri kod dağıtmaya başlamak için:

![Bulut dağıtımı](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> Bazen, Azure işlevi düzgün çalışmayabilir. Bu durumda bu sorunu çözmek için bu kontrol [SSS adım](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Derleme ve aygıt kodu karşıya yükle

### <a name="windows"></a>Windows

1. Kullanım `Ctrl+P` çalıştırmak için `task device-upload`.
2. Terminal yapılandırma modu girmenizi ister. Bunu yapmak için:

   * A düğmesini basılı tutun
   * Anında iletme ve Sıfırla düğmesini bırakın.

3. Ekran DevKit kimliği ve 'Configuration' görüntüler.
4. Bu hizmetinden alınan bağlantı dizesini ayarlar `task cloud-provision` adım.
5. VS Code'da sonra doğrulama ve Arduino karşıya yükleme, DevKit taslak başlatır: ![aygıt karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
6. DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Alabilirsiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Panosu paket dizinini doğru yenilendiğinde değilse bu hata oluşur. Bu sorunu çözmek için bu kontrol [SSS adım](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

### <a name="macos"></a>macOS

1. DevKit yapılandırma moduna: düğmesi A, sonra anında iletme ve yayın Sıfırla düğmesini basılı tutun. Ekran 'Configuration' görüntüler.
2. Kullanım `Cmd+P` çalıştırmak için `task device-upload` hizmetinden alınan bağlantı dizesini ayarlamak için `task cloud-provision` adım.
3. VS Code'da sonra doğrulama ve Arduino karşıya yükleme, DevKit taslak başlatır: ![aygıt karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
4. DevKit yeniden başlatır ve kod çalışmaya başlar.

> [!NOTE]
> Alabilirsiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Panosu paket dizinini doğru yenilendiğinde değilse bu hata oluşur. Bu sorunu çözmek için bu kontrol [SSS adım](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Projeyi test

Uygulama başlatma sonra ve düğmesi A bırakın ve ardından hafifçe DevKit Panosu sallama. Bu eylem, daha önce belirtilen diyez içeren rastgele bir tweet alır. Birkaç saniye içinde bir tweet DevKit ekranınızda görüntüler:

### <a name="arduino-application-initializing"></a>Arduino uygulama başlatılıyor...
![Arduino uygulama başlatma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Sallama A basın...
![Sallama için tuşuna A](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Sallama hazır...
![Hazır sallama](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>İşleniyor...
![İşleniyor](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Okunacak B tuşuna basın...
![Tuşuna B için okuma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Rastgele bir tweet görüntüle...
![Display-a-random-tweet](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Bir yeniden düğmesine basın, sonra için yeni bir tweet sallama.
- Tweet kullanılmadıkları kaydırmak için B düğmesine basın.

## <a name="how-it-works"></a>Nasıl çalışır?

![Diyagramı](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

Arduino taslak Azure IOT Hub'ına bir olay gönderir. Azure işlevleri uygulama bu olay tetiklenir. Azure işlevleri uygulaması Twitter'ın API'sine bağlanmak ve tweet almak için mantığını içerir. Ardından C2D tweet metni kaydırmıyor (bulut-cihaz) ileti ve aygıta geri gönderir.

## <a name="optional-use-your-own-twitter-bearer-token"></a>İsteğe bağlı: kendi Twitter taşıyıcı belirteci kullanın

Test amacıyla, bu örnek proje önceden yapılandırılmış bir Twitter taşıyıcı belirtecini kullanır. Ancak, bir [hızı sınırı](https://dev.twitter.com/rest/reference/get/search/tweets) her Twitter hesabı. Kendi belirteci kullanmayı istiyorsanız, aşağıdaki adımları izleyin:

1. Git [Twitter Geliştirici Portalı](https://dev.twitter.com/) yeni bir Twitter uygulamayı kaydedin.

2. [Tüketici anahtarı ve tüketici gizli alma](https://support.yapsody.com/hc/en-us/articles/203068116-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) , uygulamanızın.

3. Kullanım [bazı yardımcı programı](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) bu iki anahtarlarından Twitter taşıyıcı belirteci oluşturmak için.

4. İçinde [Azure portal](https://portal.azure.com/){: hedef = "_blank"}, almak **kaynak grubu** ve Bul Azure işlevi (tür: uygulama hizmeti) "Sallama, sallama" projeniz için. Adı, her zaman 'dizesi... sallama' içeriyor.
  ![azure-function](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Kodunu güncelleştirmesi `run.csx` içinde **İşlevler > shakeshake cs** kendi belirteci ile:
  ```csharp
  ...
  string authHeader = "Bearer " + "[your own token]";
  ...
  ```
  ![twitter-token](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Dosyayı kaydedin ve tıklatın **çalıştırmak**.


## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

### <a name="the-screen-displays-no-tweets-while-every-step-has-run-successfully"></a>Her adım başarıyla çalıştırıldı 'No Tweet'leri' ekran görüntüler

Bu durum normalde, dağıtabileceğiniz ve işlev uygulaması herhangi bir yere birkaç saniye kadar bir dakika soğuk başlatmak için uygulama gerektirdiğinden örneği çalıştırmak ilk kez gerçekleşir. Ya da kod çalıştırırken, uygulamasını yeniden başlatma işlemi neden bazı blips vardır. Bu durum gerçekleştiğinde, cihaz uygulaması tweet getirme için zaman aşımı elde edebilirsiniz. Bu durumda, bir ya da bu sorunu çözmek için bu yöntemlerin her ikisi de deneyebilirsiniz:

1. Cihaz uygulaması yeniden çalıştırmak için DevKit Sıfırla düğmesine tıklayın.

2. İçinde [Azure portal](https://portal.azure.com/), oluşturduğunuz Azure işlevleri uygulamayı bulun ve yeniden başlatın: ![azure işlevi yeniden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Geri Bildirim

Diğer sorunlar yaşıyorsanız başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanaldan bizimle iletişime geçin:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT paketi DevKit aygıt bağlanmak ve tweet almak öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Paketi'ne Genel Bakış](https://docs.microsoft.com/azure/iot-suite/)