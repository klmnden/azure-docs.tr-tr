---
title: Uygulama yayımlama
titleSuffix: Azure Cognitive Services
description: Oluşturma ve etkin LUIS uygulamanızı test etme bitirdikten sonra istemci uygulamanız için kullanılabilir uç noktaya yayımlayarak kolaylaştırır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: diberry
ms.openlocfilehash: 22bed877d853c7023f8efe6bfb3dd21b4aa4c8df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60194469"
---
# <a name="publish-your-active-trained-app-to-a-staging-or-production-endpoint"></a>Bir hazırlık veya üretim uç noktası için etkin, eğitilen uygulamanızı yayımlayın

Oluşturma ve etkin LUIS uygulamanızı test etme bitirdikten sonra istemci uygulamanız için kullanılabilir uç noktaya yayımlayarak kolaylaştırır. 

<a name="publish-your-trained-app-to-an-http-endpoint"></a>

## <a name="publishing"></a>Yayımlama

Uç noktaya yayımlamak için seçin **Yayımla** üst, sağ paneli. 

![Üst, sağ gezinti çubuğunda](./media/luis-how-to-publish-app/publish-top-nav-bar.png)

Açılır pencerede görüntülendiğinde, doğru yuvayı seçin: Hazırlık veya üretim. İki yayımlama yuvaları kullanarak, bu iki farklı Uç noktalara yayımlanan uç noktaları ile iki farklı sürümlerini veya aynı sürüme sahip olmanızı sağlar. 

Uygulama LUIS Portalı'nda eklenen LUIS kaynaklarla ilişkili tüm bölgeler için yayımlanır. Örneğin, üzerinde oluşturulan bir uygulama için [www.luis.ai](https://www.luis.ai)LUIS kaynak oluşturma, **westus** ve uygulamaya bir kaynak olarak eklemek için uygulama o bölgenin yayımlanır. LUIS bölgeleri hakkında daha fazla bilgi için bkz. [bölgeleri](luis-reference-regions.md).
 
![Açılır pencere yayımlama](./media/luis-how-to-publish-app/publish-pop-up.png)

Uygulamanız başarıyla yayımlandığında bir yeşil bir başarı bildirim tarayıcı üst kısmında görüntülenir. Yeşil bildirim çubuğu, uç noktalarına bağlantıyı da içerir. 

![Bağlantı uç noktası ile yayımlama açılır penceresi](./media/luis-how-to-publish-app/publish-success.png)

Uç nokta URL'sini gerekiyorsa, bağlantıyı seçin. Uç nokta URL'leri seçerek de sahip olabilirsiniz **Yönet** üst menüden seçip **anahtarları ve uç noktaları** soldaki menüde. 

## <a name="configuring-publish-settings"></a>Yayımlama ayarlarını yapılandırma

Yapılandırma seçerek yayımlama ayarları **Yönet** Gezinti üst, sağ, ardından seçerek **yayımlama ayarları**. 

![Yayımlama ayarları](./media/luis-how-to-publish-app/publish-settings.png)

### <a name="publish-after-enabling-sentiment-analysis"></a>Yaklaşım analizi etkinleştirdikten sonra yayımlayın

<a name="enable-sentiment-analysis"></a>

Yaklaşım Analizi ile tümleştirmek LUIS sağlayan [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) yaklaşımını ve anahtar tümcecik analiz sağlamak için. 

Metin analizi anahtarı belirtmeniz gerekmez ve Azure hesabınızda bu hizmet için fatura ücret alınmaz. Bu ayarı işaretleyin, sonra kalıcıdır. 

Yaklaşım verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif yaklaşım veri. Yaklaşım etiketinin `positive`, `neutral`, ve `negative` desteklenen kültürdür. Şu anda yalnızca İngilizce yaklaşım etiketlerini destekler. 

Yaklaşım Analizi ile JSON uç yanıtı hakkında daha fazla bilgi için bkz. [yaklaşım analizi](luis-concept-data-extraction.md#sentiment-analysis)

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [anahtarları Yönet](./luis-how-to-azure-subscription.md) anahtarları için Azure aboneliği ekleme LUIS anahtarına ve Bing yazım denetimi yapma anahtar ve tüm hedefleri sonuçlarına dahil.
* Bkz: [eğitme ve uygulamanızı test](luis-interactive-test.md) yayımlanan uygulamanızı test konsolunda test etmek yönergeler.

