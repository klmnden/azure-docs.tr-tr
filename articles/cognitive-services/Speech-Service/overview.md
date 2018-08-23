---
title: Konuşma hizmeti (Önizleme) nedir?
description: 'Konuşma hizmeti, Microsoft Bilişsel hizmetler, parçası ayrı olarak önceden kullanılabilen çeşitli Azure konuşma Hizmetleri sahip: Bing konuşma (metin okuma ve konuşma tanıma kapsayan), özel konuşma tanıma ve konuşma çevirisi.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 60ff2f71766a14af17ebb1cb9d20825976471296
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "41988804"
---
# <a name="what-is-the-speech-service-preview"></a>Konuşma hizmeti (Önizleme) nedir?

Bir abonelikle konuşma hizmeti geliştiriciler uygulamalarına konuşma özellikli güçlü özellikler eklemek için kolay bir yol sağlar. Uygulamalarınız artık sesli komut, döküm, yazdırma, konuşma sentezi ve konuşma çevirisi özelliğini.

Konuşma hizmeti, Cortana ve Microsoft Office gibi diğer Microsoft ürünlerinde kullanılan teknolojileri tarafından desteklenir.

> [!NOTE]
> Konuşma hizmeti, şu anda genel Önizleme aşamasındadır. Burada düzenli olarak güncelleştirmeleri belgeler, ek kod örnekleri ve daha fazlası için döndürür.

## <a name="speech-service-features"></a>Konuşma hizmeti özellikleri

|İşlev|Açıklama|
|-|-|
|[Konuşma metin](speech-to-text.md)| Ses akışları, uygulamanızın girdi olarak kabul edebilen bir metne dönüştürür. Ayrıca tümleşir [Language Understanding hizmeti](https://docs.microsoft.com/azure/cognitive-services/luis/) kullanıcının amacını konuşma türetme (LUIS).|
|[Metin okuma](text-to-speech.md)| Düz metin, ses dosyası uygulamanızda teslim görünen doğal konuşma dönüştürür. Birden çok ses, cinsiyet veya vurgu, değişen birçok desteklenen diller için kullanılabilir. |
|[Konuşma çevirisi](speech-translation.md)| Neredeyse gerçek zamanlı Akış ses çevirmek veya kayıtlı konuşma işlemek için kullanılabilir. |
|Özel Konuşmayı metne dönüştürme|Konuşmayı metne kendi oluşturarak özelleştirebileceğiniz [akustik](how-to-customize-acoustic-models.md) ve [dil](how-to-customize-language-model.md) modeller ve belirterek özel [telaffuz](how-to-customize-pronunciation.md) kuralları. |
|[Özel metin okuma](how-to-customize-voice-font.md)|Kendi seslerle metin okuma için oluşturabilirsiniz.|
|[Konuşma cihaz SDK'sı](speech-devices-sdk.md)| Birleşik konuşma hizmeti sunulmasıyla birlikte, Microsoft ve iş ortakları için geliştirme konuşma özellikli cihazlar için iyileştirilmiş bir tümleşik donanım/yazılım platformu sunar |

## <a name="access-to-the-speech-service"></a>Konuşma hizmeti erişimi

Konuşma hizmeti iki şekilde kullanılabilir. [SDK'sı](speech-sdk.md) ağ protokolleri için desteklenen platformları üzerinde daha kolay geliştirme ayrıntılarını dengelediği. [REST API](rest-apis.md) herhangi bir programlama dilinde çalışır, ancak SDK tarafından sunulan tüm işlevleri sağlamaz.

|<br>Yöntem|Konuşma<br>Metni|Metni<br>Konuşma|Konuşma<br>Çeviri|<br>Açıklama|
|-|-|-|-|-|
|[SDK'ları](speech-sdk.md)|Evet|Hayır|Evet|Belirli programlama dilleri, kitaplıklar, geliştirmeyi kolaylaştıran Websocket tabanlı alınırken kullanır.|
|[REST](rest-apis.md)|Evet|Evet|Hayır|Konuşma uygulamanıza eklemek kolaylaştıran bir basit HTTP tabanlı API'ler.|

## <a name="speech-scenarios"></a>Konuşma senaryoları

Konuşma birkaç ortak kullanımlarını kısaca aşağıda ele alınmıştır. [Speech SDK'sı](speech-sdk.md) çoğu bu senaryolar için önemlidir.

> [!div class="checklist"]
> * Ses tetiklenen uygulamalar oluşturun
> * Çağrı merkezi kayıtları özelliği
> * Ses botlar uygulayın

### <a name="voice-triggered-apps"></a>Ses tetiklenen uygulamaları

Ses giriş, esnek, eller serbest ve hızlı kullanmak uygulamanızı yapmak için harika bir yoludur. Sesli özellikli bir uygulamada, kullanıcılar yalnızca tıklayarak veya dokunarak gitmek zorunda yerine istedikleri bilgileri sorabilir.

Uygulamanıza herkes tarafından kullanıma yöneliktir, konuşma tanıma hizmeti tarafından sağlanan temel konuşma tanıma modeli kullanabilirsiniz. Bunu, çok çeşitli tipik ortamlarda konuşmacıları tanıma için iyi bir iş yapar.

Uygulamanızı belirli bir etki alanında kullanılacaksa (örneğin, TIP veya BT), oluşturabileceğiniz bir [dil modeli](how-to-customize-language-model.md) konuşma hizmeti hakkında uygulamanız tarafından kullanılan özel terimleri öğretmeyi.

Uygulamanızı bir Fabrika gibi gürültülü bir ortamda kullanılacaksa, özel bir oluşturabilirsiniz [akustik model](how-to-customize-acoustic-models.md) konuşma hizmeti konuşma paraziti ayırt etmek daha iyi izin vermek için.

Başlarken indiriliyor olarak kolayca [Speech SDK'sı](speech-sdk.md) ve ilgili aşağıdaki [hızlı](quickstart-csharp-dotnet-windows.md) makalesi.

### <a name="transcribe-call-center-recordings"></a>Çağrı merkezi kayıtları özelliği

Genellikle, kayıtları, yalnızca bir çağrı ile bir sorun oluşursa consulted merkezi çağırın. Konuşma hizmeti sayesinde her kayıt için metin konuşmaların kolaydır. Bunlar metin olduğunuzda bunları kolayca dizinleyebilirsiniz [tam metin araması](https://docs.microsoft.com/azure/search/search-what-is-azure-search) veya uygulama [metin analizi](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) yaklaşım, dil ve anahtar tümcecikleri algılamak için.

Çağrı merkezi kayıtlarınızı genellikle özel terimleri (örneğin, ürün adları veya BT terminolojisinin) içeriyorsa, oluşturabileceğiniz bir [dil modeli](how-to-customize-language-model.md) konuşma hizmeti bu sözlük öğretin. Özel bir [akustik model](how-to-customize-acoustic-models.md) Yardım konuşma hizmeti daha az-en iyi telefon bağlantıları anlayabilirsiniz.

Bu senaryo hakkında daha fazla bilgi için daha fazla bilgi edinin [toplu transkripsiyonu](batch-transcription.md) konuşma hizmeti.

### <a name="voice-bots"></a>Ses botlar

[Botlar](https://dev.botframework.com/) istedikleri giderek popüler bir yolu, kullanıcıların bilgileriyle bağlanma ve işletmelerin müşterilerle sevdikleri. Damıtarak konuşma bağlamında kullanılabilen kullanıcı arabirimi, Web sitesine veya uygulamanıza ekleme işlevselliğini bulmak daha kolay ve hızlı erişim sağlar. Konuşma hizmeti sayesinde, bu konuşma gerçekten Sentezlenen konuşma tanıma ile Konuşma sorgulara yanıt vererek fluency yeni bir boyutta alır.

Ses etkin botunuzun benzersiz kişilik ekleyin (ve markanızı güçlendirin için), bir ses kendi verebilirsiniz. Özel ses oluşturma iki adımlı bir işlemdir. İlk olarak, size [kayıt olun](record-custom-voice-samples.md) kullanmak istediğiniz ses. Sonra [bu kayıtları gönderme](how-to-customize-voice-font.md) (birlikte, bir metin dökümü) konuşma hizmetin için [ses özelleştirme portalı](https://cris.ai/Home/CustomVoice), geri kalanını yapar. Özel sesinizi oluşturduktan sonra uygulamanızda kullanmak daha kolaydır.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma hizmeti için bir abonelik anahtarı edinirler.

> [!div class="nextstepaction"]
> [Konuşma hizmeti ücretsiz olarak deneyin](get-started.md)
