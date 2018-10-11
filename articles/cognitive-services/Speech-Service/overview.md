---
title: Konuşma tanıma hizmeti nedir?
description: "Microsoft'un sunduğu Bilişsel Hizmetler'in bir parçası olan Konuşma tanıma hizmeti, daha önce ayrı olarak sunulan şu birkaç Azure konuşma hizmetini bir araya getirmektedir: Bing Konuşma (konuşma tanıma ve metin okuma), Özel Konuşma Tanıma ve Konuşma Çevirisi."
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 09/24/2018
ms.author: v-jerkin
ms.openlocfilehash: c12cd754d9979683fac4b819739d53e78ab9259c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990204"
---
# <a name="what-is-the-speech-service"></a>Konuşma tanıma hizmeti nedir?

Konuşma tanıma hizmeti, daha önce [Bing Konuşma API'si](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Translator Konuşma Çevirisi](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Özel Konuşma Tanıma](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) ve [Özel Ses](http://customvoice.ai/) hizmetleriyle kullanılabilen Azure konuşma tanıma özelliklerini bir araya getirmektedir. Artık bu özelliklerin tümüne tek bir abonelikle erişim sağlanabilir.

Konuşma tanıma hizmeti, diğer Azure konuşma tanıma hizmetleriyle aynı şekilde Cortana ve Microsoft Office gibi ürünlerde kullanılan konuşma tanıma teknolojileri ile kullanıma sunulmuştur. Sonuçların kalitesine ve bulut platformunun güvenilirliğine inanabilirsiniz.

## <a name="main-speech-service-functions"></a>Konuşma tanıma hizmetinin ana işlevleri

Konuşma tanıma hizmetinin birincil işlevleri şunlardır: Konuşmayı Metne Dönüştürme (konuşma tanıma veya transkripsiyon olarak da adlandırılır), Metin Okuma (konuşma birleştirme) ve Konuşma Çevirisi.

|İşlev|Özellikler|
|-|-|
|[Konuşmayı Metne Dönüştürme](speech-to-text.md)| <ul><li>Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<li>Ses kayıtlarından toplu konuşma dökümü alabilir. <li>Etkileşimli, konuşma ve dikte kullanım örnekleri için tanıma modları sunar.<li>Ara sonuçların yanı sıra konuşma sonu algılama, otomatik metin biçimlendirme ve küfür maskeleme özelliklerini destekler. <li>Dökümü alınmış bir konuşmadan kullanıcının amacını anlamak üzere [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/)'de (LUIS) çağrı yapabilir.\*|
|[Metin Okuma](text-to-speech.md)| <ul><li>Metni, doğal sesli konuşmaya dönüştürür. <li>Desteklenen birçok dil için birden fazla cinsiyet ve/veya diyalekt sunar. <li>Düz metin girişini veya Konuşma Birleştirme İşaretleme Dilini (SSML) destekler. |
|[Konuşma Çevirisi](speech-translation.md)| <ul><li>Ses akışını neredeyse gerçek zamanlı olarak çevirir.<li> Kaydedilen konuşmaları da işleyebilir.<li>Sonuçları metin veya birleştirilmiş konuşma olarak sunar. |

\* *Amaç tanıma için LUIS aboneliği gereklidir.*

## <a name="customize-speech-features"></a>Konuşma tanıma özelliklerini özelleştirme

Konuşma tanıma hizmetinin Konuşmayı Metne Dönüştürme ve Metin Okuma özellikleri için temel alınan modelleri eğitirken kendi verilerinizi kullanabilirsiniz.

|Özellik|Model|Amaç|
|-|-|-|
|Konuşmayı Metne Dönüştürme|[Akustik model](how-to-customize-acoustic-models.md)|Belirli konuşmacıların ve ortamların (arabalar veya fabrikalar gibi) konuşma dökümünü almanıza yardımcı olur.|
||[Dil modeli](how-to-customize-language-model.md)|Alana özgü sözlük ve dil bilgisi (tıp veya BT jargonu) transkriptini almanıza yardımcı olur.|
||[Söyleniş modeli](how-to-customize-pronunciation.md)|Kısaltmaların (örneğin, "Avrupa Birliği" için "AB") transkriptini almanıza yardımcı olur. |
|Metin Okuma|[Ses tipi](how-to-customize-voice-font.md)|Modeli insan konuşması örneklerine dayalı olarak eğiterek uygulamanızın kendisine özgü bir sese sahip olmasını sağlar.|

Özel modellerinizi, uygulamanızın Konuşmayı Metne Dönüştürme veya Metin Okuma işlevleriyle standart modelleri kullandığınız her yerde kullanabilirsiniz.

## <a name="use-the-speech-service"></a>Konuşma tanıma hizmetini kullanma

Microsoft, konuşma özellikli uygulamaların geliştirilmesini basitleştirmek için yeni Konuşma tanıma hizmetiyle kullanılmak üzere [Konuşma SDK'sını](speech-sdk.md) kullanıma sunmuştur. Konuşma SDK'sı C#, C++ ve Java için tutarlı, yerel Konuşmayı Metne Dönüştürme ve Konuşma Çevirisi API'leri sağlar. Bu dillerden biriyle geliştirme yapıyorsanız, Konuşma SDK'sı ağ ayrıntılarını sizin yerinize ele alarak geliştirmeyi daha kolay hale getirir.

Ayrıca Konuşma tanıma hizmeti, HTTP isteği oluşturabilen tüm programlama dilleriyle çalışan bir [REST API](rest-apis.md)'ye sahiptir. REST arabirimi SDK'nın akıcı, gerçek zamanlı işlevselliğini sunmaz.

|<br>Yöntem|Konuşma<br>Metne Dönüştürme|Metin<br>Konuşma|Konuşma<br>Çeviri|<br>Açıklama|
|-|-|-|-|-|
|[Konuşma SDK'sı](speech-sdk.md)|Yes|Hayır|Yes|Geliştirme işlemini basitleştirmeye yönelik yerel C#, C++ ve Java API'leri.|
|[REST](rest-apis.md)|Yes|Yes|Hayır|Uygulamalarınıza konuşma eklemeyi kolay hale getiren HTTP tabanlı, basit bir API.|

### <a name="websockets"></a>WebSockets

Konuşma tanıma hizmetinde, Konuşmayı Metne Dönüştürme ve Konuşma Çevirisi akışları için WebSocket protokolleri de vardır. Konuşma SDK'ları, Konuşma tanıma hizmetiyle iletişim kurmak için bu protokolleri kullanır. Konuşma tanıma hizmetiyle kendi WebSocket iletişiminizi uygulamaya çalışmak yerine Konuşma SDK'sını kullanın.

Zaten WebSockets aracılığıyla Bing Konuşma'yı veya Translator Konuşma Çevirisi'ni kullanan bir kodunuz varsa, bunu Konuşma tanıma hizmetini kullanacak şekilde güncelleştirebilirsiniz. WebSocket protokolleri uyumludur, yalnızca uç noktalar farklıdır.

### <a name="speech-devices-sdk"></a>Konuşma Cihazları SDK’sı

[Konuşma Cihazları SDK'sı](speech-devices-sdk.md), konuşma özellikli cihazlar üzerinde çalışan geliştiriciler için tümleşik bir donanım ve yazılım platformudur. Donanım iş ortağımız, başvuru tasarımları ve geliştirme birimleri sağlar. Microsoft, donanım özelliklerinden tam olarak yararlanan, cihaz için iyileştirilmiş bir SDK sunar.

## <a name="speech-scenarios"></a>Konuşma tanıma senaryoları

Konuşma tanıma hizmeti için kullanım örnekleri arasında şunlar yer almaktadır:

> [!div class="checklist"]
> * Ses ile tetiklenen uygulamalar oluşturma
> * Çağrı merkezi kayıtlarının dökümünü alma
> * Ses botu uygulama

### <a name="voice-user-interface"></a>Ses kullanıcı arabirimi

Ses girişi; uygulamanızı esnek, eller serbest özellikli ve kullanımı kolay hale getirmenin harika bir yoludur. Ses özellikli uygulamalarla, kullanıcılar doğrudan aradıkları bilgileri isteyebilirler.

Uygulamanız genel kullanıma yönelikse, varsayılan konuşma tanıma modellerini kullanabilirsiniz. Bunlar genel ortamlarda çok çeşitli konuşmacıları tanır.

Uygulamanız belirli bir etki alanında (tıp veya BT gibi) kullanılıyorsa, [dil modeli](how-to-customize-language-model.md) oluşturabilirsiniz. Bu modeli kullanarak Konuşma tanıma hizmetini uygulamanız tarafından kullanılan özel terminoloji hakkında eğitebilirsiniz.

Uygulamanız gürültülü bir ortamda örneğin fabrikada kullanılıyorsa, özel bir [akustik modeli](how-to-customize-acoustic-models.md) oluşturabilirsiniz. Bu model Konuşma tanıma hizmetinin konuşmayı gürültüden ayırt etmesine yardımcı olur.

Kullanmaya kolayca başlayın. [Konuşma SDK'sını](speech-sdk.md) indirmeniz ve ilgili [Hızlı Başlangıç](quickstart-csharp-dotnet-windows.md) makalesini izlemeniz yeterli olur.

### <a name="call-center-transcription"></a>Çağrı merkezi transkripsiyonu

Çağrı merkezi kayıtlarına genellikle yalnızca, bir aramayla ilgili sorun ortaya çıktığında başvurulur. Konuşma tanıma hizmeti ile her bir kayıt kolayca metne dönüştürülebilir. Metinleri [tam metin araması](https://docs.microsoft.com/azure/search/search-what-is-azure-search) için dizinleyebilir veya yaklaşımı, dili ve anahtar ifadeleri algılamak için [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) uygulayabilirsiniz.

Çağrı merkezi kayıtlarınız özel terminoloji (ürün adları veya BT jargonu gibi) içeriyorsa Konuşma tanıma hizmetine bu sözlüğü öğretmek için bir [dil modeli](how-to-customize-language-model.md) oluşturabilirsiniz. Özel bir [akustik model](how-to-customize-acoustic-models.md), Konuşma tanıma hizmetinin optimum seviyenin altındaki telefon bağlantılarını anlamasına yardımcı olabilir.

Bu senaryoyla ilgili olarak daha fazlasını öğrenmek için Konuşma tanıma hizmeti ile [toplu iş transkripsiyonu](batch-transcription.md) hakkında daha fazla bilgi edinin.

### <a name="voice-bots"></a>Ses botları

[Botlar](https://dev.botframework.com/) kullanıcıları istedikleri bilgilerle ve müşterileri sevdikleri işletmelerle buluşturmanın popüler bir yoludur. Web sitenize veya uygulamanıza konuşmaya dayalı bir kullanıcı arabirimi eklediğinizde, işlevler daha kolay bulunabilir ve bunlara hızla erişilebilir. Konuşma tanıma hizmeti ile bu konuşma, sözlü sorgulara aynı şekilde yanıt vererek akıcılığa yeni bir boyut kazandırır.

Ses özellikli botunuza benzersiz bir kişilik eklemek için kendisine ait bir sese sahip olmasını sağlayabilirsiniz. Özel ses oluşturma iki adımlı bir işlemdir. İlk olarak, kullanmak istediğiniz sesin [kayıtlarını oluşturun](record-custom-voice-samples.md). Ardından, Konuşma tanıma hizmetinin [ses özelleştirme portalına](https://cris.ai/Home/CustomVoice) [bu kayıtları gönderin](how-to-customize-voice-font.md) (bir metin transkripti ile birlikte). Diğer işlemleri portal gerçekleştirir. Özel sesinizi oluşturduktan sonra, bu sesi uygulamanızda kullanma adımları gayet basittir.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma tanıma hizmeti için bir abonelik anahtarı edinin.

> [!div class="nextstepaction"]
> [Konuşma tanıma hizmetini ücretsiz olarak deneyin](get-started.md)
