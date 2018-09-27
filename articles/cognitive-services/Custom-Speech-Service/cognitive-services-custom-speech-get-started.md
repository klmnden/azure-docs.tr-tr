---
title: Özel konuşma hizmeti ile çalışmaya başlama
titlesuffix: Azure Cognitive Services
description: Özel konuşma hizmeti için abone ve hizmet etkinlikleri bir modeli eğitmek ve dağıtım yapmak için bir Azure aboneliğine bağlayabilirsiniz.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: ae72edd626bd91dea7cd2812a3ef821b905f59a4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225251"
---
# <a name="get-started-with-custom-speech-service"></a>Özel konuşma hizmeti ile çalışmaya başlama

Özel konuşma hizmeti ana özelliklerini ve oluşturmanızı, dağıtmanızı ve uygulama ihtiyaçlarınız için akustik ve dil modelleri kullanma hakkında bilgi edinin. Özel konuşma Hizmetleri portalda kaydolduktan sonra daha kapsamlı belgeler ve adım adım yönergeler bulunabilir.

## <a name="samples"></a>Örnekler  
Bulabileceğiniz koyulmanız için sağladığımız iyi bir örneği [burada](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Önkoşullar  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Özel konuşma hizmeti için abone ve bir abonelik anahtarı edinirler
Yukarıdaki yürütmeden önce örnek özel konuşma hizmeti için abone ve gerekir bir abonelik anahtarı, bkz: alma [abonelikleri](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) veya açıklamaları izleyin [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). Bu öğreticide, hem birincil ve ikincil anahtar kullanılabilir. API anahtarı gizli tutmak için en iyi uygulamaları takip ettiğinizden emin olun ve güvenliğini sağlayın.

### <a name="get-the-client-library-and-example"></a>İstemci Kitaplığı ve örnek alma
Bir istemci kitaplığı ve örnek aracılığıyla yükleyebilirsiniz [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). İndirilen ZIP dosyasını tercih ettiğiniz bir klasöre ayıklanmasını gerekiyor, çok sayıda kullanıcı Visual Studio 2015 klasöründe seçin.

## <a name="creating-a-custom-acoustic-model"></a>Özel akustik model oluşturma
Belirli bir etki alanı için akustik model özelleştirmek için bir konuşma veri koleksiyonu gereklidir. Bu koleksiyon, konuşma verilerinden oluşan ses dosyası kümesine ve her ses dosyasının transkripsiyonunu içeren bir metin dosyasından oluşur. Ses verisi tanıyıcı kullanmak istediğiniz senaryoyu temsilcisi olmalıdır

Örneğin: konuşma gürültülü fabrika ortamında daha iyi tanımak istiyorsanız, ses dosyalarını gürültülü factory'de Konuşmayı kişilerin oluşmalıdır.
Tek bir Konuşmacı için performansı en iyi duruma getirme ilgileniyorsanız, örneğin tüm FDR'ın Fireside sohbetleri konuşmaların istediğiniz sonra birçok örnekleri yalnızca bu konuşmacının ses dosyalarını oluşmalıdır.

Bir özel akustik model oluşturma konusunda ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Özel dil modeli oluşturma
Özel dil modeli oluşturma yordamı, yalnızca metin hiçbir ses verisi haricinde bir akustik model oluşturma işlemiyle benzerdir. Metin sorgular birçok örnekleri oluşmalıdır veya konuşma beklediğiniz söylemek kullanıcılar veya kullanıcılar oturum belirten (veya yazma), uygulamanızdaki.

Özel dil modeli oluşturma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Özel Konuşmayı metne uç noktası oluşturuluyor
Özel akustik modeller ve/veya dil modelleri oluştururken, özel bir konuşmayı metne uç noktası dağıtılabilir. Yeni özel uç nokta oluşturmak için sayfanın üstündeki "CRI" menüsünden "Dağıtım" tıklayın. Bu, geçerli özel uç noktalar "dağıtımları" adlı bir tabloya götürür. Uç nokta henüz oluşturmadıysanız, tablo boş olur. Geçerli yerel ayar, tablo başlığında gösterilir. Farklı bir dil için bir dağıtım oluşturmak istiyorsanız, "Yerel ayarını Değiştir üzerinde"'a tıklayın. Yerel ayar değiştirme bölümünde desteklenen diller hakkında daha fazla bilgi bulunabilir.

Bir özel metin konuşma uç noktası oluşturma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Özel konuşma tanıma uç noktası kullanma
İstekleri CRI metne dönüştürme konuşma uç noktası için varsayılan Azure Bilişsel hizmetler konuşma uç noktası olarak çok benzer bir biçimde gönderilebilir. Bu uç noktaları konuşma tanıma API'si varsayılan uç noktalar için işlevsel olarak eşdeğer olduğunu unutmayın. Bu nedenle, konuşma tanıma API'si için REST API veya istemci kitaplığı aracılığıyla kullanılabilen aynı işlevler ayrıca özel uç noktanız için kullanılabilir.

Özel Konuşmayı metne uç noktasını kullanma hakkında ayrıntılı bir açıklama bulabilirsiniz [burada](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


CRI oluşturulan uç noktaları farklı sayıda eş zamanlı istek aboneliğinin ilişkili olduğu katmana bağlı olarak işleyebilir unutmayın. Daha fazla tanıma daha istediyseniz, hata kodu 429 döndürür (çok fazla istek). Daha fazla bilgi için lütfen [fiyatlandırma bilgileri](https://www.microsoft.com/cognitive-services/en-us/pricing). Ayrıca, isteklerin ücretsiz katman için aylık kota yok. Size erişim durumlarda ücretsiz katmanda aylık kota hizmet yukarıdaki uç noktanızı hata kodu 403 döndürür Yasak.

Hizmet, ses gerçek zamanlı olarak iletilen varsayar. Daha hızlı gönderilir, isteği gerçek zamanlı süresi bitene kadar çalıştıran kabul edilir.

* [Genel Bakış](cognitive-services-custom-speech-home.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
