---
title: Azure Bilişsel hizmetler konuşma senaryoları | Azure Microsoft Docs
description: Senaryolar
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 07/02/2018
ms.author: panosper
ms.openlocfilehash: 1488f95296bcc11a55a45aff56cee83b7708a789
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072223"
---
# <a name="speech-scenarios"></a>Konuşma Senaryoları

Konuşma tanıma teknolojisini kullanarak yetkilendirilmiş birçok senaryo vardır. Biz, en sık kullanılan birkaç çözümleme ve ilgili özellikleri belgelerinde gelin. Çoğu içerik [SDK](speech-sdk.md) bu senaryolar etkinleştirirken taşımaktadır.

Sayfa açıklar nasıl yapılır:
> [!div class="checklist"]
> * Sesle tetiklenen uygulamalar oluşturun
> * Çağrı merkezi ses çağrıları özelliği
> * Ses Botlar

## <a name="voice-triggered-apps"></a>Ses uygulamaları tetiklendi

Çok sayıda kullanıcı, uygulamaları üzerinde ses girişini etkinleştirmek istiyorsanız. Ses giriş, bu bire bir (örneğin bir araba) ücretsiz kullanma veya çeşitli görevleri yönergeleri haberleri veya hava durumu bilgileri isteme gibi hızlandırma uygulamanızı esnek hale getirmek için harika bir yoludur. 

### <a name="voice-triggered-apps-with-baseline-models"></a>Ses tetiklenen uygulamalarla temel modelleri

Uygulamanız tarafından genel ortamlarda arka plan gürültüsü aşırı olmadığı yerde kullanılacak olacaksa, bunu yapmanın kolay ve en hızlı yolu yeterlidir indirme bizim [Speech SDK'sı](speech-sdk.md) ve ilgili aşağıdaki [ Örnekleri](quickstart-csharp-dotnet-windows.md). Tarafından desteklenen SDK'sı, [Azure abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/) geliştiricilerin Cortana ve Skype güç temel konuşma tanıma modelleri için ses karşıya olanak tanır. Mdoels teknoloji ve yukarıda sözü edilen ürünleri tarafından kullanılır. Dakikalar için kullanmaya başlayabilirsiniz.

### <a name="voice-triggered-apps-with-custom-models"></a>Ses tetiklenen uygulamaları ile özel modelleri

Uygulamanızı belirli bir etki alanı, (örneğin Kimya, biyolojisi veya özel dietary gereksinimleri) adresleri sonra uyarlamak için dikkate alınması gereken isteyebileceğiniz bir [dil modeli](how-to-customize-language-model.md). Dil modeli uyarlama en yaygın ifadeler ve uygulamanız tarafından kullanılan sözcükler hakkında kod çözücü sağlanır. Kod Çözücü, daha doğru bir şekilde temel modeli yerine belirli bir etki alanı için bir özel dil modeli ile giriş sesi konuşmaların mümkün olacaktır. Benzer şekilde, kullanılacak uygulamanızı nerede bulunacağını arka plan gürültüsü tanınmış ise akustik model uyarlama isteyebilirsiniz. Diğer durumlarda çalıştırılacağı belgelerini keşfedin [dil uyarlama](how-to-customize-language-model.md) ve [akustik uyarlama](how-to-customize-acoustic-models.md) değer sağlayın ve ziyaret bizim [uyarlama portalı](https://customspeech.ai) clark'i belirten için model oluşturma deneyimi. Benzer şekilde temel modelleri, özel modelleri aracılığıyla çağrılır bizim [Speech SDK'sı](speech-sdk.md) ve ilgili aşağıdaki [örnekleri](quickstart-csharp-dotnet-windows.md).

## <a name="transcribe-call-center-audio-calls"></a>Çağrı merkezi ses çağrıları özelliği

Çağrı merkezi ses büyük miktarlarda toplar. Döküm yine de alınabilir bu ses dosyaları kalıyor değeri içinde gizli. Çağrı süresi yaklaşımı, genel çağırana sağlanan arama değeri ve müşteri memnuniyetini görüşmesi dökümleri elde ederek bulunabilir.

En iyi başlangıç noktası olan [toplu transkripsiyonu API](batch-transcription.md) yanı sıra ilgili [örnek](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

İlk edinmeniz gerekir bir [Azure abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/) ve ardından danışmanız gerekir [belgeleri]([Batch transcription API](batch-transcription.md)).

### <a name="transcribe-call-center-audio-calls-with-baseline-models"></a>Çağrı merkezi ses çağrıları temel modelleri özelliği

Yapılması gereken iç temel modelleri mi kullanacağınızı transkripsiyonu taşımak için bir dil veya akustik model ya da her ikisini de uyarlayabilirsiniz kararıdır. Taban çizgisi modelini kullanmak için API anahtarı yalnızca API gerektirir. Dahili API çağırmak için verilerinizi en iyi modeli ve uyarlayabilirsiniz.

### <a name="transcribe-call-center-audio-calls-with-custom-models"></a>Çağrı merkezi ses çağrıları özel modelleri özelliği

Ardından özel bir model kullanmayı planlıyorsanız, API anahtarı ile birlikte bu modelin Kimliğini almanız gerekir. Model kimliği elde edilen [uyarlama portalı](https://customspeech.ai). Bu, şu 'Uç noktası Ayrıntıları' görünümünde bulma uç noktası kimliği, ancak 'Details' Bu modelin üzerinde tıkladığınızda, alabileceğiniz modeli kimliği değil.

## <a name="voice-bots"></a>Ses Botlar

Geliştirici, kendi uygulama ses çıkış güçlendirebilirsiniz. Konuşma hizmeti için bir dizi synthetize konuşma [dilleri](supported-languages.md) sağlar [uç noktaları](rest-apis.md) erişmek ve bu özelliği uygulamanıza eklemek için.

Ayrıca, daha fazla kişilik ve benzersizlik kendi botlar için eklemek istediğiniz kullanıcıları için konuşma hizmeti, geliştiricilerin benzersiz ses tipi özelleştirme olanak tanır. Konuşma tanıma modelleri ses tiplerini gerektiren kullanıcı verilerini özelleştirme benzer. Geliştiriciler, bu verileri karşıya yükleme olan bizim [ses uyarlama portalı](https://customspeech.ai) ve ses botunuza ilişkin benzersiz markanız oluşturmaya başlayın. Ayrıntılar açıklanmıştır [burada](how-to-text-to-speech.md) yanı sıra [SSS](faq-text-to-speech.md) sayfaları 

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Konuşma SDK ile Başlat](speech-sdk.md)
