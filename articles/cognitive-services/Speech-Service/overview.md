---
title: Konuşma Tanıma Hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: "Konuşma hizmeti, Azure Bilişsel Hizmetler'in bir parçası, ayrı olarak önceden kullanılabilen çeşitli konuşma Hizmetleri sahip: Bing konuşma (metin okuma ve konuşma tanıma kapsayan), özel konuşma tanıma ve konuşma çevirisi."
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: e86adfd4e832e6b9514e4813ddd4a942b07ca624
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336603"
---
# <a name="what-is-speech-services"></a>Konuşma Hizmetleri nedir?

Diğer Azure konuşma Hizmetleri gibi konuşma Hizmetleri, Cortana ve Microsoft Office gibi ürünlerinde kullanılan konuşma teknolojileri tarafından desteklenir.

Konuşma Hizmetleri Azure konuşma tanıma özelliği ile önceden kullanılabilen birleştiren [Bing konuşma API'si](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Translator konuşma çevirisi](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [özel konuşma](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home), ve [özel Ses](http://customvoice.ai/) Hizmetleri. Artık bu özelliklerin tümüne tek bir abonelikle erişim sağlanabilir.

## <a name="main-speech-services-functions"></a>Ana konuşma hizmetleri işlevleri

Birincil işlevler konuşma Hizmetleri, Konuşmayı metne dönüştürme (Ayrıca çağrılan konuşma tanıma veya döküm), metin okuma (konuşma sentezi) ve konuşma çevirisi içindedir.

|İşlev|Özellikler|
|-|-|
|[Konuşmayı metne dönüştürme](speech-to-text.md)| <li>Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<li>Ses kayıtlarından toplu konuşma dökümü alabilir. <li>Ara sonuçların yanı sıra konuşma sonu algılama, otomatik metin biçimlendirme ve küfür maskeleme özelliklerini destekler. <li>Dökümü alınmış bir konuşmadan kullanıcının amacını anlamak üzere [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/)'de (LUIS) çağrı yapabilir.\*|
|[Metin okuma](text-to-speech.md)| <li>**YENİ**: Metin okuma sinir seslerini İnsan konuşma (İngilizce) neredeyse ayırt sağlar. <li>Metni, doğal sesli konuşmaya dönüştürür. <li>Desteklenen birçok dil için birden fazla cinsiyet ve/veya diyalekt sunar. <li>Düz metin girişini veya Konuşma Birleştirme İşaretleme Dilini (SSML) destekler. |
|[Konuşma çevirisi](speech-translation.md)| <li>Ses akışını neredeyse gerçek zamanlı olarak çevirir.<li> Kaydedilen konuşmaları da işleyebilir.<li>Sonuçları metin veya birleştirilmiş konuşma olarak sunar. |


## <a name="customize-speech-features"></a>Konuşma tanıma özelliklerini özelleştirme

Konuşma tanıma hizmetinin Konuşmayı Metne Dönüştürme ve Metin Okuma özellikleri için temel alınan modelleri eğitirken kendi verilerinizi kullanabilirsiniz.

|Özellik|Model|Amaç|
|-|-|-|
|Konuşmayı Metne Dönüştürme|[Akustik model](how-to-customize-acoustic-models.md)|Belirli konuşmacıların ve ortamların (arabalar veya fabrikalar gibi) konuşma dökümünü almanıza yardımcı olur.|
||[Dil modeli](how-to-customize-language-model.md)|Alana özgü sözlük ve dil bilgisi (tıp veya BT jargonu) transkriptini almanıza yardımcı olur.|
||[Söyleniş modeli](how-to-customize-pronunciation.md)|Kısaltmaların (örneğin, "Avrupa Birliği" için "AB") transkriptini almanıza yardımcı olur. |
|Metin okuma|[Ses tipi](how-to-customize-voice-font.md)|Modeli insan konuşması örneklerine dayalı olarak eğiterek uygulamanızın kendisine özgü bir sese sahip olmasını sağlar.|

Özel modellerinizi, uygulamanızın Konuşmayı Metne Dönüştürme veya Metin Okuma işlevleriyle standart modelleri kullandığınız her yerde kullanabilirsiniz.

## <a name="use-the-speech-service"></a>Konuşma tanıma hizmetini kullanma

Microsoft, konuşma özellikli uygulamaların geliştirilmesini basitleştirmek için Konuşma tanıma hizmetiyle kullanılmak üzere [Konuşma SDK'sını](speech-sdk.md) kullanıma sunmuştur. Konuşma SDK'sı C#, C++ ve Java için tutarlı, yerel Konuşmayı Metne Dönüştürme ve Konuşma Çevirisi API'leri sağlar. Bu dillerden biriyle geliştirme yapıyorsanız, Konuşma SDK'sı ağ ayrıntılarını sizin yerinize ele alarak geliştirmeyi daha kolay hale getirir.

Konuşma hizmetleri de sahip bir [REST API](rest-apis.md) HTTP isteği yapabilen programlama dili ile çalışır. REST arabirimi SDK'nın akıcı, gerçek zamanlı işlevselliğini sunmaz.

|<br>Yöntem|Konuşma<br>Metne Dönüştürme|Metin<br>Konuşma|Konuşma<br>Çeviri|<br>Açıklama|
|-|-|-|-|-|
|[Konuşma SDK'sı](speech-sdk.md)|Evet|Hayır|Evet|Geliştirme işlemini basitleştirmeye yönelik yerel C#, C++ ve Java API'leri.|
|[REST API'ler](rest-apis.md)|Evet|Evet|Hayır|Uygulamalarınıza konuşma eklemeyi kolay hale getiren HTTP tabanlı, basit bir API.|

### <a name="websockets"></a>WebSockets

Konuşma Hizmetleri WebSocket Protokolü akış konuşma metin ve konuşma çevirisi için de destekler. Konuşma SDK'ları, Konuşma tanıma hizmetiyle iletişim kurmak için bu protokolleri kullanır. Konuşma tanıma hizmetiyle kendi WebSocket iletişiminizi uygulamaya çalışmak yerine Konuşma SDK'sını kullanın.

Bing konuşma veya Translator konuşma çevirisi WebSockets üzerinden kullanan kodu zaten varsa, konuşma hizmetleri kullanacak şekilde güncelleştirebilirsiniz. WebSocket protokolleri uyumludur, ancak uç noktalar farklıdır.

### <a name="speech-devices-sdk"></a>Konuşma Cihazları SDK’sı

[Konuşma Cihazları SDK'sı](speech-devices-sdk.md), konuşma özellikli cihazlar üzerinde çalışan geliştiriciler için tümleşik bir donanım ve yazılım platformudur. Donanım iş ortağımız, başvuru tasarımları ve geliştirme birimleri sağlar. Microsoft, donanım özelliklerinden tam olarak yararlanan, cihaz için iyileştirilmiş bir SDK sunar.


## <a name="speech-scenarios"></a>Konuşma tanıma senaryoları

Konuşma Hizmetleri için kullanım örnekleri şunlardır:

> [!div class="checklist"]
> * Ses ile tetiklenen uygulamalar oluşturma
> * Çağrı merkezi kayıtlarının dökümünü alma
> * Ses botu uygulama

### <a name="voice-user-interface"></a>Ses kullanıcı arabirimi

Ses girişi; uygulamanızı esnek, eller serbest özellikli ve kullanımı kolay hale getirmenin harika bir yoludur. Ses özellikli uygulamalarla, kullanıcılar doğrudan aradıkları bilgileri isteyebilirler.

Uygulamanız genel kullanıma yönelikse, varsayılan konuşma tanıma modellerini kullanabilirsiniz. Bunlar genel ortamlarda çok çeşitli konuşmacıları tanır.

Uygulamanız belirli bir etki alanında (tıp veya BT gibi) kullanılıyorsa, [dil modeli](how-to-customize-language-model.md) oluşturabilirsiniz. Bu model, uygulamanız tarafından kullanılan özel terimleri hakkında konuşma Hizmetleri öğretmeyi kullanabilirsiniz.

Uygulamanız gürültülü bir ortamda örneğin fabrikada kullanılıyorsa, özel bir [akustik modeli](how-to-customize-acoustic-models.md) oluşturabilirsiniz. Bu model, konuşma paraziti ayırt etmek için konuşma Hizmetleri yardımcı olur.

### <a name="call-center-transcription"></a>Çağrı merkezi transkripsiyonu

Çağrı merkezi kayıtlarına genellikle yalnızca, bir aramayla ilgili sorun ortaya çıktığında başvurulur. Konuşma tanıma hizmeti ile her bir kayıt kolayca metne dönüştürülebilir. Metinleri [tam metin araması](https://docs.microsoft.com/azure/search/search-what-is-azure-search) için dizinleyebilir veya yaklaşımı, dili ve anahtar ifadeleri algılamak için [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) uygulayabilirsiniz.

Çağrı merkezi kayıtlarınızı ürün adları veya BT terminolojisinin özel terimleri kapsıyorsa oluşturabileceğiniz bir [dil modeli](how-to-customize-language-model.md) sözlük konuşma Hizmetleri öğretmeyi. Özel bir [akustik model](how-to-customize-acoustic-models.md) Yardım konuşma Hizmetleri daha az-en iyi telefon bağlantıları anlayabilirsiniz.

Bu senaryoyla ilgili olarak daha fazlasını öğrenmek için Konuşma tanıma hizmeti ile [toplu iş transkripsiyonu](batch-transcription.md) hakkında daha fazla bilgi edinin.

### <a name="voice-bots"></a>Ses botları

[Botlar](https://dev.botframework.com/) kullanıcıları istedikleri bilgilerle ve müşterileri sevdikleri işletmelerle buluşturmanın popüler bir yoludur. Web sitenize veya uygulamanıza konuşmaya dayalı bir kullanıcı arabirimi eklediğinizde, işlevler daha kolay bulunabilir ve bunlara hızla erişilebilir. Konuşma tanıma hizmeti ile bu konuşma, sözlü sorgulara aynı şekilde yanıt vererek akıcılığa yeni bir boyut kazandırır.

Ses özellikli botunuza benzersiz bir kişilik eklemek için kendisine ait bir sese sahip olmasını sağlayabilirsiniz. Özel ses oluşturma iki adımlı bir işlemdir. İlk olarak, kullanmak istediğiniz sesin [kayıtlarını oluşturun](record-custom-voice-samples.md). Ardından, Konuşma tanıma hizmetinin [ses özelleştirme portalına](https://cris.ai/Home/CustomVoice) [bu kayıtları gönderin](how-to-customize-voice-font.md) (bir metin transkripti ile birlikte). Diğer işlemleri portal gerçekleştirir. Özel sesinizi oluşturduktan sonra, bu sesi uygulamanızda kullanma adımları gayet basittir.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma Hizmetleri için bir abonelik anahtarı edinirler.

> [!div class="nextstepaction"]
> [Konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md)
