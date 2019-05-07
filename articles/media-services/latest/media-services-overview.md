---
title: Azure Media Services v3’e genel bakış | Microsoft Docs
description: Bu makalede, Media Services’ın üst düzey genel bakışı ve daha fazla ayrıntı için makalelerin bağlantıları sağlanmaktadır.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 04/18/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 91297a02966000899ab79dfb86446890e9c4439a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148709"
---
# <a name="azure-media-services-v3-overview"></a>Azure Media Services v3 genel bakış

Azure Media Services, yayın kalitesinde video akışı elde etmenizi, erişilebilirlik ve dağıtımı iyileştirmenizi, içerikleri analiz etmenizi ve daha fazlasını yapmanızı sağlayan çözümler derlemenize olanak tanıyan bulut tabanlı bir platformdur. İster uygulama geliştiricisi, çağrı merkezi, devlet dairesi, isterse bir eğlence şirketi olun, Media Services günümüzün en popüler mobil cihazlarında ve tarayıcılarında büyük kitlelere olağanüstü kalitede medya deneyimi sunan uygulamalar oluşturmanıza yardımcı olur. 

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](developers-guide.md) birini kullanın.

## <a name="what-can-i-do-with-media-services"></a>Media Services ile ne yapabilirim?

Media Services, bulutta çeşitli medya iş akışı derlemenize olanak sağlar. Aşağıda, Media Services ile gerçekleştirilebileceklerin bazı örnekleri verilmiştir.  

* Videoları, çeşitli tarayıcılarda ve cihazlarda oynatılabilmesi için çeşitli biçimlerde sunma. Çeşitli istemcilere (mobil cihazlar, TV, PC vb.) hem isteğe bağlı hem de canlı akış sunmak için video ve ses içeriğinin uygun şekilde kodlanması ve paketlenmesi gerekir. Teslim etmek ve bu tür içerik akışı hakkında bilgi için bkz: [hızlı başlangıç: Kodlama ve akışını dosyaları](stream-files-dotnet-quickstart.md).
* Futbol, beyzbol, lise ve üniversite takım sporları vb. gibi spor etkinliklerini büyük bir çevrimiçi kitleye canlı akışa alın. 
* Belediye sarayı, şehir meclisi toplantıları ve yasama organları gibi kamu toplantılarını ve etkinliklerini yayınlayın.
* Kaydedilen videoları veya ses içeriğini analiz edin. Örneğin, daha yüksek müşteri memnuniyeti elde etmek için kuruluşlar, konuşmayı metne dönüştürebilir ve arama dizinleri ve panolar derleyebilir. Daha sonra genel şikayetlerden, şikayet kaynaklarından ve diğer ilişkili verilerden istihbarat çıkarabilir.
* Bir müşterinin (örneğin, bir filmi stüdyosu), telif hakkıyla korunan bir çalışmaya erişimi ve kullanım yetkisini kısıtlaması gerektiğinde bir abonelik video hizmeti oluşturun ve DRM korumalı içeriği akışa alın.
* Uçak, tren ve otomobillerde kayıttan yürütülmesi için çevrimdışı içerik sunun. Bir müşterinin, ağ bağlantısının kesileceğini tahmin ettiğinde kayıttan yürütülmesi için içeriği telefonuna veya tabletine indirmesi gerekebilir.
* Konuşmayı metne dönüştürme, birden fazla dile çevirme vb. için Azure Media Services ve [Azure Bilişsel Hizmetler API’leri](https://docs.microsoft.com/azure/#pivot=products&panel=ai) ile eğitim amaçlı bir e-öğrenme video platformu uygulayın. 
* Azure Media Services ile birlikte kullanmak [Azure Bilişsel hizmetler API'leri](https://docs.microsoft.com/azure/#pivot=products&panel=ai) videoları daha geniş bir kitleye (örneğin, işitme engelli kişiler veya isteyenler boyunca farklı bir okumak için uygun alt yazı ve açıklamalı alt yazı eklemek için Dil).
* Azure CDN, anlık yüksek yükleri (örneğin, bir ürün sunumu etkinliğinin başlangıcını) daha iyi işlemek için büyük ölçeklendirme elde etmek etkinleştirin. 

## <a name="how-can-i-get-started-with-v3"></a>v3’ü kullanmaya nasıl başlayabilirim? 

Media Services v3 ile içeriği kodlayıp paketleme, talep üzerine video akışı yapma, canlı yayın gerçekleştirme ve videolarınızı analiz etme hakkında bilgi edinin. Öğreticiler, API başvuruları ve diğer belgeler, güvenli bir biçimde milyonlarca kullanıcıya ölçeklendirilebilen, isteğe bağlı ve canlı video veya ses akışları sağlama ile ilgili bilgiler içerir.

Geliştirmeye başlamadan önce gözden [temel kavramlar](concepts-overview.md)<br/>

### <a name="quickstarts"></a>Hızlı Başlangıçlar  

Hızlı başlangıçlar, Media Services'ı hızlı şekilde denemenize yönelik yeni müşteriler için temel gün-1 yönergeler gösterir.

* [Stream video dosyaları - .NET](stream-files-dotnet-quickstart.md)
* [Stream video dosyaları - CLI](stream-files-cli-quickstart.md)
* [Stream video dosyaları - Node.js](stream-files-nodejs-quickstart.md)
    
### <a name="tutorials"></a>Öğreticiler 

Öğretici senaryo tabanlı yordamlar üst Media Services görevlerden bazılarını gösterir.

* [Uzak dosya ve akış videosu – REST kodlayın](stream-files-tutorial-with-rest.md)
* [Karşıya yüklenen dosya ve akış video - .NET kodlama](stream-files-tutorial-with-api.md)
* [Stream Canlı - .NET](stream-live-tutorial-with-api.md)
* [Videonuzu - .NET analiz etme](analyze-videos-tutorial-with-api.md)
* [Dinamik şifreleme AES-128 - .NET](protect-with-aes128.md)
    
### <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

Makaleler, bir görevin nasıl tamamlanacağını gösteren kod örnekleri içerir. Bu bölümde, birçok örnekler bulabilirsiniz, yalnızca birkaç tanesi şunlardır:

* [Bir hesap oluşturma - CLI](create-account-cli-how-to.md)
* [API'lere erişim - CLI](access-api-cli-how-to.md)
* [SDK'larla geliştirmeye başlama](developers-guide.md)
* [HTTPS ile giriş - iş olarak .NET kodlama](job-input-from-http-how-to.md)  
* [Olay İzleme - Portal](monitor-events-portal-how-to.md)
* [Birden çok DRM ile - .NET dinamik olarak şifreleyin](protect-with-drm.md) 
* [-CLI gibi özel bir dönüşüm ile kodlama](custom-preset-cli-howto.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

v3’ü kullanmaya nasıl başlayabilirim? 

> [!div class="nextstepaction"]
> [Temel kavramlar hakkında bilgi edinin](concepts-overview.md)<br/>
> [SDK'larını kullanarak Media Services v3 API ile geliştirme](developers-guide.md) 

