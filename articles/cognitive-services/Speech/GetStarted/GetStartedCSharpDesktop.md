---
title: C# Masaüstü kitaplığını kullanarak Bing konuşma tanıma API'sini kullanmaya başlama | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Konuşmayı metne dönüştürmek için Bing konuşma tanıma API'si kullanan temel Windows uygulamaları geliştirin.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5f3b70a2dd9816210ed61280be38504a3980d205
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515378"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-c35-for-net-on-windows"></a>Hızlı Başlangıç: C'de Bing konuşma tanıma API'si kullanan&#35; Windows üzerinde .NET için

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bu sayfa, Konuşmayı metne dönüştürme için konuşma tanıma API'si kullanan temel bir Windows uygulaması geliştirme işlemi gösterilmektedir. İstemci kitaplığını kullanarak gerçek zamanlı akış için hangi istemci uygulamanızı hizmete ses gönderdiğinde, aynı anda ve zaman uyumsuz kısmi tanıma sonuçları alır anlamına sağlar.

Tüm cihazlarda çalışan uygulamalar konuşma hizmeti kullanmak isteyen geliştiriciler C# Masaüstü kitaplığını kullanabilirsiniz. Kitaplığı kullanmak için yükleyin [NuGet paketini Microsoft.ProjectOxford.SpeechRecognition x86](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x86/) bir 32-bit platformu ve [NuGet paketini Microsoft.ProjectOxford.SpeechRecognition x64](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x64/) için bir 64-bit platformu. İstemci Kitaplığı için API Başvurusu bkz [Microsoft konuşma C# Masaüstü Kitaplığı](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html).

Aşağıdaki bölümlerde, yükleme, oluşturma ve C# Masaüstü kitaplığını kullanarak C# örnek uygulamayı çalıştırma açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Aşağıdaki örnek Windows 8 + ve .NET Framework 4.5 + kullanarak geliştirilmiştir [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Örnek uygulaması edinir

Örnekten kopyalama [konuşma C# Masaüstü kitaplık örneği](https://github.com/microsoft/cognitive-speech-stt-windows) depo.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olur ve ücretsiz deneme aboneliği anahtarını alma

Konuşma tanıma API'si, Bilişsel hizmetler (daha önce Project Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

> [!IMPORTANT]
> * Bir abonelik anahtarı edinirler. Konuşma istemci kitaplıkları kullanmadan önce olmalıdır bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
> * Abonelik anahtarınızı kullanın. Örneği çalıştırdığında sağlanan C# Masaüstü örnek uygulama ile birlikte abonelik anahtarınızı metin kutusuna yapıştırın. Daha fazla bilgi için [örnek uygulamayı çalıştırma](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>1\. adım: Örnek uygulamayı yüklemek

1. Visual Studio 2015'i başlatın ve **dosya** > **açık** > **proje/çözüm**.

2. İndirilen konuşma tanıma API'si dosyalarını kaydettiğiniz klasöre göz atın. Seçin **konuşma** > **Windows**ve ardından örnek WP klasörü seçin.

3. SpeechToText WPF Samples.sln adlı Visual Studio 2015 çözümü (.sln) dosyasını açmak için çift tıklayın. Çözüm, Visual Studio'da açılır.

## <a name="step-2-build-the-sample-application"></a>2\. adım: Örnek uygulaması oluşturma

1. Kullanmak istiyorsanız *tanıma amacıyla*, önce kaydolmanız gerekir [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). LUIS uygulamanızı uç nokta URL'sini anahtarının değerini ayarlayın kullanılacağını `LuisEndpointUrl` samples/SpeechRecognitionServiceExample klasöründeki app.config dosyasında. LUIS uygulaması uç nokta URL'sini hakkında daha fazla bilgi için bkz. [uygulamanızı yayımlayın](../../luis/luis-get-started-create-app.md#publish-your-app).

   > [!TIP]
   > Değiştirin `&` LUIS uç nokta URL'si ile içindeki karakter `&amp;` URL doğru XML ayrıştırıcısı tarafından yorumlanır emin olmak için.

2. Ctrl + Shift + B tuşuna basın veya **derleme** Şerit menüsünde. Ardından **Çözümü Derle**.

## <a name="step-3-run-the-sample-application"></a>3\. adım: Örnek uygulamayı çalıştırın

1. Derleme tamamlandıktan sonra F5 tuşuna basın veya seçin **Başlat** örneği çalıştırmak için Şerit menüsünde.

2. Git **Project Oxford konuşma metin örnek** penceresi. Abonelik anahtarınızı yapıştırın **başlatmak için abonelik anahtarınızı buraya yapıştırın** gösterildiği gibi metin kutusu. Abonelik anahtarınızı PC'NİZDE veya dizüstü kalıcı hale getirmek için seçin **anahtarı Kaydet**. Sistemden bir abonelik anahtarı silmek için işaretleyin **Delete tuşuna** PC ya da dizüstü kaldırmak için.

   ![Yapıştırma, konuşma tanıma anahtarı](../Images/SpeechRecog_paste_key.PNG)

3. Altında **konuşma tanıma kaynak**, iki ana giriş kategoriye ayrılır altı konuşma kaynaklardan birini seçin:

   * Bilgisayarınızın mikrofon veya iliştirilmiş bir mikrofon konuşma yakalamak için kullanın.
   * Bir ses dosyasını yürütün.

   Her kategorinin üç tanıma modu vardır:

    * **ShortPhrase modu**: Uzun bir utterance fazla 15 saniye. İstemci, veriler sunucuya gönderildiğinde gibi birden çok kısmı sonuç ve birden çok en iyi n seçim ile bir nihai sonucu alır.
    * **LongDictation modu**: Uzun bir utterance en fazla iki dakika. Veriler sunucuya gönderildiğinde gibi istemci, birden çok kısmı sonuç ve birden çok Nihai sonuç, sunucunun cümle duraklamaları burada gösterir temel alır.
    * **Hedefi olan algılama**: Sunucu giriş konuşma hakkında daha fazla yapılandırılmış bilgi döndürür. Hedefi olan algılama kullanmak için önce kullanarak bir model eğitip gerekir [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

Bu örnek uygulaması ile örnek ses dosyalarını kullanın. Bu örnek samples/SpeechRecognitionServiceExample klasörü altında ile indirdiğiniz deponun dosyaları bulun. Başka hiçbir dosya seçtiğinizde seçilirse, bu örnek ses dosyalarını otomatik olarak çalışacak **Shortphrase modu için wav dosyası kullan** veya **Longdictation modu için wav dosyası kullan** konuşmanızı giriş olarak. Şu anda yalnızca WAV ses biçimi desteklenir.

![Konuşma metin arabirimi](../Images/HelloJones.PNG)

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="recognition-events"></a>Tanıma olayları

* **Kısmi sonuçlar olayları**: Konuşma hizmeti bile Konuşmayı bitirmeden kişilerin, yorumlarını tahmin her başlatıldığında bu olay adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderen son (kullanırsanız `DataRecognitionClient`).
* **Hata olayları**: Hizmet bir hata algıladığında çağrılır.
* **Hedefi olayları**: Adlı istemcilerde "WithIntent" (yalnızca modunda ShortPhrase) son tanıma sonra sonuç yapılandırılmış bir JSON hedefi ayrıştırılır.
* **Neden olayların**:
  * İçinde `ShortPhrase` modu, bu olay adı verilir ve konuşma tamamladıktan sonra en iyi n sonuçlarını döndürür.
  * İçinde `LongDictation` modu, olay işleyicisi adlı birden çok kez bağlı hizmeti cümle duraklamaları burada tanımlar.
  * **Her en iyi n seçim**, güvenirlik değeri ve tanınan metin birkaç farklı biçimleri döndürülür. Daha fazla bilgi için [çıkış biçimi](../Concepts.md#output-format).

Olay işleyicileri zaten kod açıklamaları biçiminde kodda belirtildiği.

## <a name="related-topics"></a>İlgili konular

* [Microsoft Speech Masaüstü Kitaplığı Başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html)
* [Android'de Java Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedJavaAndroid.md)
* [İOS üzerinde Objective-C, Microsoft konuşma tanıma API'si ile başlama](Get-Started-ObjectiveC-iOS.md)
* [JavaScript içinde Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedJSWebsockets.md)
* [REST aracılığıyla Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedREST.md)
