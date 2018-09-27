---
title: Content Moderator’ı kullanmaya başlama
titlesuffix: Azure Cognitive Services
description: Content Moderator'ı kullanmaya başlamak nasıl.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: c2ac0ccd89b5f1436a151e3d69c5d7423090f244
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225303"
---
# <a name="get-started-with-content-moderator"></a>Content Moderator’ı kullanmaya başlama

Aşağıdaki yollarla Content Moderator'ı kullanmaya başlayın:

- [İle gözden geçirme Aracı'nı başlatın](#start-with-the-review-tool) API anahtarı alma ve bir gözden geçirme ekibi oluşturun. İçerik ve İnceleme API'lerini incelemeleri, ek adımlar olmadan oluşturmak için taramak için yönetim API'lerini çağırmak için API anahtarını kullanabilirsiniz avantajdır.
- [Content Moderator için abone](#start-with-the-apis) API anahtarını almak için Azure. Kullanıma [API Başvurusu](api-reference.md) ve [SDK'ları](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Yine de bir gözden geçirme ekibi oluşturmak için çevrimiçi kaydolmanız gerekir.
- [Akış Bağlayıcısı ve şablonları](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) çok çeşitli kullanımı kolay Tasarımcısı ile tümleştirmeler görmek için.

Belirlediğiniz seçeneğe bakılmaksızın bkz [kimlik bilgilerinin yönetimiyle](review-tool-user-guide/credentials.md) API kimlik bilgilerinizi bulmak için makale.

## <a name="start-with-the-review-tool"></a>Gözden geçirme aracı ile Başlat
[Kaydolun](http://contentmoderator.cognitive.microsoft.com/) Content Moderator gözden geçirme aracı web sitesinde.

![İçerik Moderator giriş sayfası](images/homepage.PNG)

### <a name="create-a-review-team"></a>Bir gözden geçirme takım oluştur
Ekibiniz için bir ad verin. İş arkadaşlarınızı davet etmek istiyorsanız, e-posta adreslerini girerek bunu yapabilirsiniz.

![Takım üyesi davet et](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Görüntüleri karşıya yükleme veya metin girin
Tıklayın **deneyin > Görüntü** veya **deneyin > metin**. En fazla beş örnek görüntüleri karşıya yükleyebilir veya denetimi için örnek metni girin.

![Görüntü veya metin denetimi deneyin](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Otomatik denetimden için gönderme
İçeriğiniz için otomatik denetimden gönderin. Dahili olarak İnceleme aracını yönetim içeriğinizi taranacak API'lerini çağırır. Tarama tamamlandığında, gözden geçirmeniz için bekleyen sonuçlarıyla ilgili bildiren bir ileti görürsünüz.

![Orta dosyaları](images/submitted.png)

### <a name="review-and-confirm-results"></a>Gözden geçirin ve sonuçları onaylayın
Kullanarak otomatik olarak aracılı etiketleri gözden geçirin ve gerekirse değiştirin **sonraki** düğmesi. Moderator API'leri, sıraya alma etiketli içerik başlatır, iş uygulaması çağrısı insan tarafından İnceleme ekipleri tarafından incelenmesi için hazır. İçerik bu yaklaşımı kullanarak büyük hacimlerdeki hızlı bir şekilde gözden geçirin.

![Sonuçları gözden geçirme](images/reviewresults.png)

Tüm kullanmayı öğrenin [Aracı'nın özellikleri gözden geçirin](Review-Tool-User-Guide/human-in-the-loop.md) veya API'ler hakkında bilgi edinmek için sonraki bölüme devam edin. Kayıt adımı sizin için gözden geçirme Aracı'nda gösterildiği gibi sağlanan API anahtarı olduğundan atlamak [kimlik bilgilerinin yönetimiyle](review-tool-user-guide/credentials.md) makalesi.

### <a name="use-the-apis"></a>API'leri kullanma

Content Moderator İş uygulamalarınızı ile tümleştirmeyi öğrenin. Kullanıma [API Başvurusu](api-reference.md) ve [SDK'ları](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net).

## <a name="subscribe-in-the-azure-portal"></a>Azure portalında abone olun

[Content Moderator için abone](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) Azure portalında. Aşağıdaki API'leri biriyle başlayın:

### <a name="image-moderation"></a>Görüntü denetimi

İle başlayan [API Konsolu](try-image-api.md) veya [.NET Hızlı Başlangıç](image-moderation-quickstart-dotnet.md) görüntülerini taramak ve olası yetişkinlere yönelik ve müstehcen içeriğin etiketleri, güven puanları ve diğer kullanarak algılamak için bilgi ayıklanır.

### <a name="text-moderation"></a>Metin denetimi

İle başlayan [API Konsolu](try-text-api.md) veya [.NET Hızlı Başlangıç](text-moderation-quickstart-dotnet.md) olası küfür makine destekli istenmeyen metin sınıflandırma (Önizleme), metin içeriğini taramak ve kişisel bilgileri (PII). 


### <a name="video-moderation"></a>Video denetimi

İle başlayan [.NET Hızlı Başlangıç](video-moderation-api.md) videoları taramak ve olası yetişkinlere yönelik ve müstehcen içerikleri algılama için. 


### <a name="review-apis"></a>API’leri inceleme

Buradan iş, gözden geçirme ve iş akışı API'leri seçerek başlayın.

- [İş API](try-review-api-job.md) yönetim API'lerini kullanarak içeriğinizi tarar ve gözden geçirmeleri gözden geçirme Aracı'nda oluşturur. 
- [Gözden geçirme API](try-review-api-review.md) doğrudan görüntü, metin veya görüntü incelemeleri İnsan Moderatörler için içerik taramadan oluşturur. 
- [İş akışı API](try-review-api-workflow.md) oluşturur, güncelleştirir ve ekibinizin oluşturduğu özel iş akışlarını ayrıntılarını alır.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [API Başvurusu](api-reference.md) ve [SDK'ları](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Hızlı bir başlangıç yapmak, tümleştirme ile [.NET SDK'sı örnekleri](sdk-and-samples.md#net-sdk-samples), [C# REST API örnekleri](https://github.com/sanjeev3/azure-docs-pr/blob/master/articles/cognitive-services/Content-Moderator/sdk-and-samples.md#rest-api-samples-in-c) ve [öğreticiler](sdk-and-samples.md#tutorials).
