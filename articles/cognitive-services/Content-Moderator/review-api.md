---
title: İncelemeleri, iş akışları ve işleri kavramlar - Content Moderator
titlesuffix: Azure Cognitive Services
description: İncelemeleri, iş akışları ve işleri hakkında bilgi edinin
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: c1d4ef640e2ae072dacba7a665b6689e3224c55c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60607282"
---
# <a name="content-moderation-reviews-workflows-and-jobs"></a>İçerik denetleme incelemeleri, iş akışları ve işler

Content Moderator makine Yardımlı resim denetimi gerçek dünya senaryoları için bir en iyi denetleme işlemi oluşturmak için İnsan-de--döngü özellikleriyle birleştirir. Bunu bulut tabanlı yapar [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com). Bu kılavuzda, gözden geçirme Aracı'nın temel kavramlar hakkında bilgi edineceksiniz: incelemeleri, iş akışları ve işler.

## <a name="reviews"></a>İncelemeler

Bir gözden geçirme, içerik için gözden geçirme aracı yüklenir ve altında görünür **gözden** sekmesi. Buradan, kullanıcılar uygulanan etiketler alter ve uygun şekilde kendi özel etiketler. Bir kullanıcı bir gözden geçirme gönderdiğinde, sonuçlar belirtilen geri çağırma uç noktasına gönderilir ve içeriği siteden kaldırılır.

![Gözden geçirme sekmesinde bir tarayıcı gözden geçirme Aracı Web sitesi Aç](./Review-Tool-user-Guide/images/image-workflow-review.png)

Bkz: [gözden geçirme aracı Kılavuzu](./review-tool-user-guide/review-moderated-images.md) değerlendirmeleri oluşturmaya başlayın veya görmek için [REST API Kılavuzu](./try-review-api-review.md) programlamayla Bunun hakkında bilgi edinmek için.

## <a name="workflows"></a>İş Akışları

Bir iş akışı içeriği için bulut tabanlı özelleştirilmiş filtre kullanılıyor. İş akışları çeşitli içerik farklı yollarla filtreleyebilir ve ardından uygun eylemi gerçekleştirin için hizmetler için bağlanabilirsiniz. Content Moderator Bağlayıcısı ile bir iş akışı otomatik olarak denetleme etiketler ve gözden geçirmeleri ile gönderilen içerik oluşturun.

### <a name="view-workflows"></a>Görünüm iş akışları

Mevcut iş görüntülemek için Git [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) seçip **ayarları** > **iş akışları**.

![Varsayılan iş akışı](images/default-workflow-listed.PNG)

İş akışları tamamen bunları programlı olarak erişilebilir olmasını sağlayan JSON dizeler olarak açıklanabilir. Seçerseniz **Düzenle** akışınız için seçeneğini ve ardından **JSON** sekmesinde, aşağıdaki gibi bir JSON ifadesini görürsünüz:

```json
{
    "Type": "Logic",
    "If": {
        "ConnectorName": "moderator",
        "OutputName": "isAdult",
        "Operator": "eq",
        "Value": "true",
        "Type": "Condition"
        },
    "Then": {
    "Perform": [
    {
        "Name": "createreview",
        "CallbackEndpoint": null,
        "Tags": []
    }
    ],
    "Type": "Actions"
    }
}
```

Bkz: [gözden geçirme aracı Kılavuzu](./review-tool-user-guide/workflows.md) oluşturmaya ve iş akışlarını kullanarak başlayın veya görmek için [REST API Kılavuzu](./try-review-api-workflow.md) programlamayla Bunun hakkında bilgi edinmek için.

## <a name="jobs"></a>İşler

İçerik denetleme, iş akışları ve incelemeleri işlevsellik için sarmalayıcı türü bir denetimi iş görür. İş API veya metin denetim API'si Content Moderator görüntü denetimi kullanarak içeriğinizi tarar ve ardından, belirtilen iş akışı karşı denetler. İş akışının sonuçlarına göre bunu olabilir veya içeriği için bir gözden geçirme oluşturamaz [gözden geçirme aracı](./review-tool-user-guide/human-in-the-loop.md). İncelemeler hem de iş akışları oluşturulabilir ve bunların ilgili API'leri ile yapılandırılmış olsa da iş API (hangi belirtilen geri çağırma uç noktasına gönderilebilir) tüm işleminin ayrıntılı bir rapor almak sağlar.

Bkz: [REST API Kılavuzu](./try-review-api-job.md) işleri ile çalışmaya başlamak için.

## <a name="next-steps"></a>Sonraki adımlar

* Sürücü test [iş API Konsolu](try-review-api-job.md)ve REST API kod örnekleri kullanın. Visual Studio ve C# ile ilgili bilgi sahibi değilseniz, ayrıca kullanıma [işleri .NET Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md). 
* İncelemeleri için kullanmaya başlama [gözden geçirme API Konsolu](try-review-api-review.md)ve REST API kod örnekleri kullanın. Daha sonra bkz [incelemeleri .NET Hızlı Başlangıç](moderation-reviews-quickstart-dotnet.md).
* Görüntü incelemeleri kullanın [Video gözden geçirme Hızlı Başlangıç](video-reviews-quickstart-dotnet.md)ve öğrenin nasıl [dökümleri video gözden geçirici ekleyin](video-transcript-reviews-quickstart-dotnet.md).
