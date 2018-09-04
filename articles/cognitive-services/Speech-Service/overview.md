---
title: Konuşma tanıma hizmeti (önizleme) nedir?
description: "Microsoft'un sunduğu Bilişsel Hizmetler'in bir parçası olan Konuşma tanıma hizmeti, daha önce ayrı olarak sunulan şu birkaç Azure konuşma hizmetini bir araya getirmektedir: Bing Konuşma (konuşma tanıma ve metin okuma), Özel Konuşma Tanıma ve Konuşma Çevirisi."
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 922320bb0b880e933b27025257e6a533fe257680
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091481"
---
# <a name="what-is-the-speech-service"></a>Konuşma tanıma hizmeti nedir?

Konuşma tanıma hizmeti, daha önce [Bing Konuşma API'si](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Translator Konuşma Çevirisi](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Özel Konuşma Tanıma](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) ve [Özel Ses](http://customvoice.ai/) hizmetleriyle kullanılabilen Azure konuşma tanıma özelliklerini bir araya getirmektedir. Artık bu özelliklerin tümüne tek bir abonelikle erişim sağlanabilir.

Konuşma tanıma hizmeti, diğer Azure konuşma tanıma hizmetleriyle aynı şekilde Cortana ve Microsoft Office gibi ürünlerde kullanılan, başarısı kanıtlanmış konuşma tanıma teknolojileri ile kullanıma sunulmuştur. Sonuçların kalitesine ve Azure bulutunun güvenilirliğine güvenebilirsiniz.

> [!NOTE]
> Konuşma tanıma hizmeti şu anda genel önizleme aşamasındadır. Belge güncelleştirmeleri, yeni kod örnekleri ve daha fazlası için burayı düzenli olarak ziyaret edin.

## <a name="main-speech-service-functions"></a>Konuşma tanıma hizmetinin ana işlevleri

Konuşma tanıma hizmetinin birincil işlevleri şunlardır: Konuşmayı Metne Dönüştürme (konuşma tanıma veya transkripsiyon olarak da adlandırılır), Metin Okuma (konuşma birleştirme) ve Konuşma Çevirisi.

|İşlev|Özellikler|
|-|-|
|[Konuşmayı Metne Dönüştürme](speech-to-text.md)| <ul><li>Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<li>Ses kayıtlarından toplu konuşma dökümü alabilir. <li>Etkileşimli, konuşma ve dikte kullanım örnekleri için tanıma modları sunar.<li>Ara sonuçların yanı sıra konuşma sonu algılama, otomatik metin biçimlendirme ve küfür maskeleme özelliklerini destekler. <li>Dökümü alınmış bir konuşmadan kullanıcının amacını anlamak üzere [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/)'de (LUIS) çağrı yapabilir.\*|
|[Metin Okuma](text-to-speech.md)| <ul><li>Metni, doğal sesli konuşmaya dönüştürür. <li>Desteklenen birçok dil için Birden Fazla cinsiyet ve/veya diyalekt sunar. <li>Düz metin girişini veya Konuşma Birleştirme İşaretleme Dilini (SSML) destekler. |
|[Konuşma Çevirisi](speech-translation.md)| <ul><li>Ses akışını neredeyse gerçek zamanlı olarak çevirir<li> Kaydedilen konuşmaları da işleyebilir<li>Sonuçları metin veya birleştirilmiş konuşma olarak sunar. |

\* *Amaç tanıma için LUIS aboneliği gereklidir.*


## <a name="customizing-speech-features"></a>Konuşma tanıma özelliklerini özelleştirme

Konuşma tanıma hizmeti, bu hizmetin Konuşmayı Metne Dönüştürme ve Metin Okuma özellikleri için temel alınan modelleri eğitmek üzere kendi verilerinizi kullanmanıza olanak sağlar. 

|Özellik|Model|Amaç|
|-|-|-|
|Konuşmayı Metne Dönüştürme|[Akustik model](how-to-customize-acoustic-models.md)|Belirli konuşmacıların ve ortamların (arabalar veya fabrikalar gibi) konuşma dökümünü almanıza yardımcı olur|
||[Dil modeli](how-to-customize-language-model.md)|Alana özgü sözlük ve dil bilgisi (tıp veya BT jargonu) dökümü almanıza yardımcı olur|
||[Söyleniş modeli](how-to-customize-pronunciation.md)|Kısaltmaların (örneğin, "Avrupa Birliği" için "AB") dökümünü almanıza yardımcı olur |
|Metin Okuma|[Ses tipi](how-to-customize-voice-font.md)|Modeli insan konuşması örneklerine dayalı olarak eğiterek uygulamanızın kendisine özgü bir sese sahip olmasını sağlar.|

Oluşturulan özel modellerinizi, uygulamanızın Konuşmayı Metne Dönüştürme veya Metin Okuma işlevleriyle standart modelleri kullandığınız her yerde kullanabilirsiniz.


## <a name="using-the-speech-service"></a>Konuşma tanıma hizmetini kullanma

Microsoft, konuşma özellikli uygulamaların geliştirilmesini basitleştirmek için yeni Konuşma tanıma hizmetiyle kullanılmak üzere [Konuşma SDK'sını](speech-sdk.md) kullanıma sunmuştur. Konuşma SDK'sı C#, C++ ve Java için tutarlı, yerel Konuşmayı Metne Dönüştürme ve Konuşma Çevirisi API'leri sağlar. Bu dillerden biriyle geliştirme yapıyorsanız Konuşma SDK'sı, ağ ayrıntılarını sizin yerinize ele alarak geliştirmeyi daha kolay hale getirir.

Ayrıca Konuşma tanıma hizmeti, HTTP isteği oluşturabilen tüm programlama dilleriyle çalışan bir [REST API](rest-apis.md)'ye sahiptir. Ancak REST arabirimi, SDK'nın akışlı, gerçek zamanlı işlevselliğini sunmaz.

|<br>Yöntem|Konuşmayı<br>Metne Dönüştürme|Metin<br>Okuma|Konuşma<br>Çevirisi|<br>Açıklama|
|-|-|-|-|-|
|[Konuşma SDK'sı](speech-sdk.md)|Yes|Hayır|Yes|Geliştirme işlemini basitleştirmeye yönelik yerel C#, C++ ve Java API'leri.|
|[REST](rest-apis.md)|Evet|Evet|Hayır|Uygulamalarınıza konuşma eklemeyi kolay hale getiren HTTP tabanlı, basit bir API.|

### <a name="websockets"></a>WebSockets

Konuşma tanıma hizmetinde, Konuşmayı Metne Dönüştürme ve Metin Çevirisi akışları için WebSockets protokolleri de bulunmaktadır. Konuşma SDK'ları, Konuşma tanıma hizmetiyle iletişim kurmak için bu protokolleri kullanır. Konuşma tanıma hizmetiyle kendi WebSockets iletişiminizi uygulamaya çalışmak yerine Konuşma SDK'sını kullanmanız gerekir.

Ancak, WebSockets aracılığıyla Bing Konuşma'yı veya Translator Konuşma Çevirisi'ni kullanan bir kodunuz varsa bunu Konuşma tanıma hizmetini kullanacak şekilde güncelleştirmek oldukça basittir. WebSockets protokolleri uyumludur; yalnızca uç noktalar farklıdır.

### <a name="speech-devices-sdk"></a>Konuşma Cihazları SDK’sı

[Konuşma Cihazları SDK'sı](speech-devices-sdk.md), konuşma özellikli cihazlar üzerinde çalışan geliştiriciler için tümleşik bir donanım ve yazılım platformudur. Donanım iş ortağımız, başvuru tasarımları ve geliştirme birimleri sağlar. Microsoft, donanım özelliklerinden tam olarak yararlanan, cihaz için iyileştirilmiş bir SDK sunar.


## <a name="speech-scenarios"></a>Konuşma tanıma senaryoları

Konuşma tanıma hizmeti için kullanım örnekleri arasında şunlar yer almaktadır:

> [!div class="checklist"]
> * Ses ile tetiklenen uygulamalar oluşturma
> * Çağrı merkezi kayıtlarının dökümünü alma
> * Ses botu uygulama

### <a name="voice-user-interface"></a>Ses kullanıcı arabirimi

Ses girişi; uygulamanızı esnek, eller serbest özellikli ve kullanımı kolay hale getirmenin harika bir yoludur. Ses özellikli uygulamalarda kullanıcıların, aradıkları bilgileri gezinerek bulmak yerine yalnızca istemeleri yeterlidir.

Uygulamanız genel kullanıma yönelikse, varsayılan konuşma tanıma modellerini kullanabilirsiniz. Bunlar genel ortamlardaki çeşitli konuşmacıları tanıma konusunda oldukça kullanışlıdır.

Uygulamanız belirli bir etki alanında (örneğin, tıp veya BT) kullanılacaksa Konuşma tanıma hizmetine, uygulamanız tarafından kullanılan özel terminolojiyi öğretmek için bir [dil modeli](how-to-customize-language-model.md) kullanabilirsiniz.

Uygulamanız fabrika gibi gürültülü bir ortamda kullanılacaksa Konuşma tanıma hizmetinin konuşmaları gürültüden ayırt etmesine daha fazla olanak sağlamak için özel bir [akustik model](how-to-customize-acoustic-models.md) oluşturabilirsiniz.

Bu hizmeti kullanmaya kolayca başlamak için [Konuşma SDK'sını](speech-sdk.md) indirmeniz ve ilgili bir [Hızlı Başlangıç](quickstart-csharp-dotnet-windows.md) makalesindeki adımları uygulamanız yeterlidir.

### <a name="call-center-transcription"></a>Çağrı merkezi transkripsiyonu

Çağrı merkezi kayıtlarına genellikle yalnızca, bir aramayla ilgili sorun ortaya çıktığında başvurulur. Konuşma tanıma hizmeti ile her bir kayıt kolayca metne dönüştürülebilir. Kayıtlar metne dönüştürüldükten sonra bunları [tam metin araması](https://docs.microsoft.com/azure/search/search-what-is-azure-search) için dizinleyebilir veya yaklaşımı, dili ve anahtar ifadeleri algılamak için [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) uygulayabilirsiniz.

Çağrı merkezi kayıtlarınız özel terminoloji (ürün adları veya BT jargonu gibi) içeriyorsa Konuşma tanıma hizmetine bu sözlüğü öğretmek için bir [dil modeli](how-to-customize-language-model.md) oluşturabilirsiniz. Özel bir [akustik model](how-to-customize-acoustic-models.md), Konuşma tanıma hizmetinin optimum seviyenin altındaki telefon bağlantılarını anlamasına yardımcı olabilir.

Bu senaryoyla ilgili olarak daha fazlasını öğrenmek için Konuşma tanıma hizmeti ile [toplu iş transkripsiyonu](batch-transcription.md) hakkında daha fazla bilgi edinin.

### <a name="voice-bots"></a>Ses botları

[Botlar](https://dev.botframework.com/) kullanıcıları istedikleri bilgilerle ve müşterileri sevdikleri işletmelerle buluşturmanın giderek popüler hale gelen bir yoludur. Web sitenize veya uygulamanıza konuşmaya dayalı bir kullanıcı arabirimi eklemek, işlevselliği daha kolay bulunabilir ve erişimi daha hızlı hale getirir. Konuşma tanıma hizmeti ile bu konuşma, sözlü sorgulara aynı şekilde yanıt vererek akıcılığa yeni bir boyut kazandırır.

Ses özellikli botunuza benzersiz bir kişilik eklemek (ve markanızı güçlendirmek) için kendisine ait bir sese sahip olmasını sağlayabilirsiniz. Özel ses oluşturma iki adımlı bir işlemdir. İlk olarak, kullanmak istediğiniz sesin [kayıtlarını oluşturursunuz](record-custom-voice-samples.md). Ardından, Konuşma tanıma hizmetinin [ses özelleştirme portalına](https://cris.ai/Home/CustomVoice) [bu kayıtları gönderirsiniz](how-to-customize-voice-font.md) (bir metin transkripti ile birlikte) ve portal, diğer işlemleri gerçekleştirir. Özel sesinizi oluşturduktan sonra bu sesi uygulamanızda kolayca kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma tanıma hizmeti için bir abonelik anahtarı edinin.

> [!div class="nextstepaction"]
> [Konuşma tanıma hizmetini ücretsiz olarak deneyin](get-started.md)
