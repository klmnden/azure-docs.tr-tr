---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sını kullanarak bir tarayıcıda JavaScript dilinde konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sını kullanarak bir tarayıcıda JavaScript dilinde Konuşma tanımayı öğrenmesine
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: 75dcda643741e3aeb1238f82128e4c5b058be840
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883675"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-cognitive-services-speech-sdk"></a>Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sını kullanarak bir tarayıcıda JavaScript dilinde konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sı JavaScript bağlantısını kullanarak bir Web sitesi oluşturmayı öğreneceksiniz.
Uygulama üzerindeki Microsoft Bilişsel hizmetler konuşma SDK bağlıdır ([indirme sürüm 1.0.0](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* PC veya Mac, çalışma mikrofon ile.
* Bir metin düzenleyicisi.
* Chrome ya da Microsoft Edge geçerli sürümü.
* İsteğe bağlı olarak, bir web sunucusu, PHP komut barındırmayı destekler.

## <a name="create-a-new-website-folder"></a>Yeni bir Web sitesi klasörü oluşturun

Yeni, boş bir klasör oluşturun. Örnek bir web sunucusunda barındırmak istemeniz durumunda, web sunucusunun klasöre erişebildiğini doğrulayın.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Konuşma SDK için JavaScript bu klasöre paketini açın.

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Karşıdan yükleme Speech SDK'sı olarak bir [.zip paketi](https://aka.ms/csspeech/jsbrowserpackage) ve yeni oluşturduğunuz klasöre paketinden çıkarın. Bu, başka bir deyişle, açılmış iki dosyada sonuçlanmalıdır `microsoft.cognitiveservices.speech.sdk.bundle.js` ve `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
İkinci gerekiyorsa isteğe bağlıdır ve SDK kodu, hata ayıklama amacıyla kullanılan dosyasıdır.

## <a name="create-an-indexhtml-page"></a>Bir index.html sayfası oluşturma

Adlı klasörde yeni bir dosya oluşturmak `index.html` ve bu dosyayı bir metin düzenleyiciyle açın.

1. Aşağıdaki HTML çatıyı oluşturun:

  ```html
  <html>
  <head>
      <title>Microsoft Cognitive Service Speech SDK JavaScript Quickstart</title>
  </head>
  <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
  </body>
  </html>
  ```

1. İlk yorum aşağıda dosyanıza aşağıdaki kullanıcı Arabirimi kodu ekleyin:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Konuşma SDK'sına bir başvuru ekleyin

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Yukarı düğme tanıma, tanıma işleminin sonucu ve abonelik için işleyiciler UI kod tarafından tanımlanan ilgili alanları bağlayabilirsiniz:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Belirteç kaynağı oluşturma (isteğe bağlı)

Bir web sunucusunda web sayfasını barındırmak istediğiniz durumlarda, bir belirteç kaynağının tanıtım uygulamanız için isteğe bağlı olarak sağlayabilirsiniz.
Bu şekilde, abonelik anahtarınızı hiçbir zaman sunucunuzun herhangi bir yetkilendirme kodu kendilerini girmeden Konuşma özelliklerini kullanmak üzere kullanıcıların verirken bırakır.

1. `token.php` adlı yeni bir dosya oluşturun. Bu örnekte, web sunucunuza PHP komut dili destekleyen varsayıyoruz. Aşağıdaki kodu girin:

  [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Düzen `index.html` dosya ve dosyanıza aşağıdaki kodu ekleyin:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Yetkilendirme belirteçleri yalnızca sınırlı bir ömrü vardır.
> Bu basit örnek yetkilendirme belirteçleri otomatik olarak yenilenecek şekilde nasıl algılanacağını göstermez. El ile bir kullanıcı olarak, sayfayı yeniden yükleyin veya yenilemek için F5'e basın.

## <a name="build-and-run-the-sample-locally"></a>Derleme ve örneği yerel olarak çalıştırma

Uygulamayı başlatmak için index.html dosyasını çift tıklatın veya index.html ile sık kullandığınız web tarayıcınızı açın. Abonelik anahtarınızı girin olanak tanıyan basit bir GUI sunacaktır ve [bölge](regions.md) ve mikrofon kullanarak bir tanıma tetikleyin.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Derleme ve örnek bir web sunucusu çalıştırma

Uygulamanızı başlatın, sık kullandığınız web tarayıcınızı açın ve klasör barındırın genel URL'ye noktası için girin, [bölge](regions.md)ve mikrofon kullanarak bir tanıma tetikleyin. Yapılandırılmışsa, bir belirteç, belirteç kaynağınızdan alacaksınız.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/js-browser` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi Al](speech-sdk.md#get-the-samples)
