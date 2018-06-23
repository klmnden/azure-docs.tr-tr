---
title: Azure özel konuşma hizmetini kullanmaya başlama | Microsoft Docs
description: Özel konuşma hizmete abone olmak ve hizmet etkinlikleri bir modeli eğitmek ve bir dağıtım yapmak için bir Azure aboneliğine bağlayın.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: fe7a140f5ba2d712014f03663a88d516958d188e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351743"
---
# <a name="get-started-with-custom-speech-service"></a>Özel konuşma hizmeti ile çalışmaya başlama

Özel konuşma hizmet ana özelliklerini keşfedin ve derleme, dağıtma ve uygulama gereksinimleriniz için kullanım ve dil modelleri kullanma öğrenin. Özel konuşma Hizmetleri portalında kaydolduktan sonra daha kapsamlı belgeler ve adım adım yönergeler bulunabilir.

## <a name="samples"></a>Örnekler  
Bulabileceğiniz giderek size sağladığımız iyi bir örneği [burada](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Önkoşullar  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Özel konuşma hizmete abone olmak ve bir abonelik anahtarı edinme
Yukarıdaki ile yürütmeden önce örnek özel konuşma hizmete abone olmak ve gerekir abonelik anahtar, bkz: get [abonelikleri](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) veya açıklamaları izleyin [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). Bu öğreticide, hem birincil ve ikincil anahtar kullanılabilir. API anahtar gizli tutmak için en iyi uygulamaları takip ettiğinizden emin olun ve güvenliğini sağlayın.

### <a name="get-the-client-library-and-example"></a>İstemci Kitaplığı ve örnek alın
Bir istemci kitaplığı ve örnek aracılığıyla karşıdan yüklenebilir [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). Tercih ettiğiniz bir klasöre ayıklanacak indirilen ZIP dosyasının gerekir, çok sayıda kullanıcı Visual Studio 2015 klasörü seçin.

## <a name="creating-a-custom-acoustic-model"></a>Özel akustik model oluşturma
Belirli bir etki alanındaki akustik modelini özelleştirmek için konuşma verilerinin bir koleksiyonunu gereklidir. Bu koleksiyon ses dosyalarını konuşma veri kümesi ve her ses dosyasının transcriptions bir metin dosyasından oluşur. Ses verilerini tanıyıcı kullanmak istediğiniz senaryo temsilcisi olmalıdır

Örneğin: gürültülü fabrika ortamında konuşma daha iyi bilmek istiyorsanız, ses dosyalarını gürültülü fabrikada konuşarak kişilerin oluşmalıdır.
Tek bir Konuşmacı için performansı en iyi duruma getirme ilgileniyorsanız, örneğin tüm Fireside FDR'ın sohbetleri transcribe istediğiniz sonra ses dosyaları yalnızca bu Konuşmacı birçok örnekleri oluşmalıdır.

Özel bir akustik modeli oluşturma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Özel dil model oluşturma
Özel dil modeli oluşturma yordamı, yalnızca metin hiçbir ses verisi aşağıdakiler dışında bir akustik modeli oluşturmak için benzer. Metin sorguları birçok örnekleri oluşmalıdır veya utterances beklediğiniz söylemek için kullanıcı veya kullanıcılar oturum belirten (veya yazarak), uygulamanızdaki.

Özel dil modeli oluşturma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Özel bir konuşma metin uç noktası oluşturma
Özel akustik modelleri ve/veya dil modelleri oluşturduğunuzda, özel bir konuşma metin uç noktası dağıtılabilir. Yeni bir özel uç noktası oluşturmak için sayfanın üst kısmında "CRI" menüsünden "Dağıtımları"'i tıklatın. Bu sizi, geçerli özel uç noktaları "dağıtımları" adlı bir tablo götürür. Uç nokta henüz oluşturmadıysanız, tablonun boş olur. Geçerli yerel tablo başlığı yansıtılır. Farklı bir dil için bir dağıtım oluşturmak istiyorsanız, "Yerel ayarını Değiştir üzerinde"'i tıklatın. Desteklenen diller hakkında ek bilgi değiştirme yerel bölümünde bulunabilir.

Bir özel konuşma metin uç noktası oluşturma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Bir özel konuşma uç noktası kullanma
İstekleri bir CRI konuşma metin uç noktası için varsayılan Microsoft Bilişsel hizmetler konuşma uç noktası olarak çok benzer bir şekilde gönderilebilir. Bu uç noktalar konuşma API varsayılan Uç noktalara işlevsel olarak özdeş olduğunu unutmayın. Bu nedenle, istemci kitaplığı veya konuşma API'si için REST API aracılığıyla kullanılabilir aynı işlevselliği de özel uç noktanız için kullanılabilir.

Özel bir konuşma metin uç nokta kullanma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Abonelik için ilişkili eş zamanlı istekleri katmanına bağlı olarak farklı sayıda uç nokta CRI oluşturuldu işleyebilir unutmayın. Daha fazla teşekkürler daha istediyseniz, hata kodu 429 döndürür (çok sayıda istek). Daha fazla bilgi için lütfen ziyaret [fiyatlandırma bilgileri](https://www.microsoft.com/cognitive-services/en-us/pricing). Ayrıca, bir aylık kota ücretsiz katmanı için isteklerin yoktur. Size erişim durumlarda ücretsiz katmanda aylık kotanızı hizmet yukarıdaki uç noktanızı 403 hata kodunu döndürür Yasak.

Hizmetin ses gerçek zamanlı olarak iletilir varsayar. Daha hızlı gönderilirse, istek gerçek zamanlı süresi geçene kadar çalıştıran kabul edilir.

* [Genel Bakış](cognitive-services-custom-speech-home.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
