---
title: Azure içerik denetleyici Başlarken | Microsoft Docs
description: Azure içerik Denetleyici ile çalışmaya nasıl başlayacağınız.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: ae4333047ebd95733c7baaed0323a0c2c477d323
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352565"
---
# <a name="get-started-with-content-moderator"></a>Content Moderator’ı kullanmaya başlama

Aşağıdaki yollarla içerik denetleyici API'leri ve gözden geçirme Aracı'nı kullanmaya başlama:

- [Başlatmak için İnceleme aracı ile](#start-with-the-review-tool) API anahtarları ve gözden geçirme ekip oluşturmak için. Gözden geçirme Aracı'nı keşfedin ve içerik yönetici API'ları kullanarak tümleştirme öğrenin.
- [İçerik denetleyiciye abone](#start-with-the-apis) Azure portalında. Hala bir gözden geçirme ekip oluşturmak için çevrimiçi kaydolmanız gerekir.
- [Akış Bağlayıcısı ve şablonları kullanmak](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) çok çeşitli kullanımı kolay Tasarımcısı ile tümleştirmeleri kullanıma için.

Belirlediğiniz seçenek ne olursa olsun, bkz: [kimlik bilgilerini yönetme](review-tool-user-guide/credentials.md) API kimlik bilgilerinizi bulmak için makale.

## <a name="start-with-the-review-tool"></a>Gözden geçirme aracıyla başlatın
[Kaydolun](http://contentmoderator.cognitive.microsoft.com/) içerik denetleyici gözden geçirme aracı web sitesinde.

![İçerik denetleyici giriş sayfası](images/homepage.PNG)

### <a name="create-a-review-team"></a>Gözden geçirme takım oluşturma
Ekibinizin bir ad verin. İş arkadaşlarınızı davet etmek istiyorsanız, e-posta adreslerini girerek bunu yapabilirsiniz.

![Bir ekip üyesi davet et](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Resimler yükleyin veya metni girin
Tıklatın **deneyin > Görüntü** veya **deneyin > metin**. En fazla beş örnek resimler yükleyin veya denetleme için örnek metin girin.

![Görüntü veya metin denetleme deneyin](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Otomatik denetleme için Gönder
İçeriğinizi otomatik denetleme için gönderin. Dahili olarak, gözden geçirme Aracı'nı denetleme içeriğinizi taramak için API çağırır. Tarama tamamlandığında, gözden geçirmeniz için bekleyen sonuçlarıyla ilgili bildiren bir ileti görür.

![Orta dosyaları](images/submitted.png)

### <a name="review-and-confirm-results"></a>Gözden geçirin ve sonuçları onaylayın
Otomatik aracılı etiketleri gözden geçirin, gerekirse değiştirin ve kullanarak gönderin **sonraki** düğmesi. Yönetici API ' ları, queuing etiketli içerik başlatır iş uygulamanızı çağırır gibi İnsan gözden geçirme ekipleri tarafından gözden geçirilmesi için hazır. Bu yaklaşımı kullanarak içeriğin büyük birimleri hızlı bir şekilde gözden geçirin.

![Sonuçları gözden geçirme](images/reviewresults.png)

Tüm kullanmayı öğrenin [aracın özellikleri gözden](Review-Tool-User-Guide/human-in-the-loop.md) veya API'ler hakkında bilgi edinmek için sonraki bölüme devam edin. Sizin için İnceleme aracı gösterildiği gibi sağlanan API anahtarı sahip için kaydolma adımı atlayın [kimlik bilgilerini yönetme](review-tool-user-guide/credentials.md) makalesi.

### <a name="use-the-apis"></a>API'leri kullanın

İçerik yönetimini incelediniz ve aracı deneyimi gözden geçirin, içerik denetleyici iş uygulamalarınızla tümleştirin öğrenin. Daha fazla bilgi edinmek ve örnekleri ve SDK'ları anlama Hızlı İzleme için aşağıdaki bölümü kullanın.

## <a name="start-with-the-apis"></a>API'leri ile Başlat

[İçerik denetleyiciye abone](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) Azure portalında. Aşağıdaki API'leri biriyle başlatın:

### <a name="image-moderation"></a>Görüntü denetimi

İle başlayan [API konsol](try-image-api.md) veya kullanın [.NET Hızlı Başlangıç](image-moderation-quickstart-dotnet.md) görüntüleri taramak ve etiketleri, güvenirlik puanları ve diğer kullanarak olası yetişkin ve saldırganlardan içeriği algılamak için bilgi ayıklanır.

### <a name="text-moderation"></a>Metin denetimi

İle başlayan [API konsol](try-text-api.md) veya [.NET Hızlı Başlangıç](text-moderation-quickstart-dotnet.md) olası uygunsuz metin, makine destekli istenmeyen metin sınıflandırma (Önizleme) için metin içeriği taramak için ve kişisel bilgileri (PII). 


### <a name="video-moderation"></a>Video denetimi

İle başlayan [.NET quickstart](video-moderation-api.md) videolar tarama ve olası yetişkin ve saldırganlardan içeriği algılayabilir. 


### <a name="review-apis"></a>API’leri inceleme

İş, gözden geçirme ve iş akışı API'leri seçerek buradan başlayın.

- [İş API](try-review-api-job.md) API'leri yönetimini kullanarak içeriğinizi tarar ve gözden geçirme aracında incelemeler oluşturur. 
- [Gözden geçirme API](try-review-api-review.md) doğrudan görüntü, metin ya da video incelemeler İnsan denetleyiciler için içerik taramadan oluşturur. 
- [İş akışı API](try-review-api-workflow.md) oluşturur, güncelleştirir ve ekibinizin oluşturduğu özel iş akışları ayrıntılarını alır.

## <a name="next-steps"></a>Sonraki adımlar

İçerik yönetimini başlayarak hakkında daha fazla bilgi [Görüntü Yönetimi API](image-moderation-api.md).
