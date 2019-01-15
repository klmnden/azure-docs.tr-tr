---
title: Denetimi işleri ve İnsan içinde--döngüsü incelemeleri - Content Moderator
titlesuffix: Azure Cognitive Services
description: İşletmeniz için en iyi sonuçları almak için Azure Content Moderator İnceleme API'si kullanarak makine Yardımlı resim denetimi İnsan içinde--döngüsü özellikleri ile birleştirin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: a348b18d1ecc9c0e4405c54a8e554d932781ec92
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265354"
---
# <a name="content-moderation-jobs-and-reviews"></a>İçerik denetleme işler ve gözden geçirmeler

Azure Content Moderator'ı kullanarak makine Yardımlı resim denetimi İnsan içinde--döngüsü özellikleriyle birleştirerek [gözden geçirme API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) işletmeniz için en iyi sonuçları almak için.

Gözden geçirme API İnsan gözetiminin içerik denetleme işleminize dahil etmek için aşağıdaki yöntemleri sunar:

* `Job` işlemler, makine Yardımlı resim denetimi ve insan tarafından İnceleme oluşturulurken bir adım olarak başlatmak için kullanılır.
* `Review` işlemleri denetleme adım dışında insan tarafından İnceleme oluşturmak için kullanılır.
* `Workflow` işlem gözden geçirme oluşturma için eşikler ile tarama otomatik hale getiren iş akışlarını yönetmek için kullanılır.

`Job` Ve `Review` işlemleri kabul durumunu ve sonuçlarını almak için geri çağırma uç noktalarınızı.

Bu makalede ele alınmaktadır `Job` ve `Review` operations. Okuma [iş akışlarına genel bakış](workflow-api.md) nasıl oluşturulacağı hakkında bilgi için düzenleme ve iş akışı tanımları alın.

## <a name="job-operations"></a>İş işlemleri

### <a name="start-a-job"></a>Bir işi başlatma
Kullanım `Job.Create` denetleme ve insan tarafından İnceleme oluşturma işi başlatmak için işlemi. Content Moderator, içerik tarar ve belirtilen iş akışı değerlendirir. İş akışı sonuçlarına göre incelemeleri oluşturur ya da adım atlar. Ayrıca, geri çağırma uç noktanıza sonrası denetleme ve sonrası gözden geçirme etiketleri gönderir.

Girişler, aşağıdaki bilgileri ekleyin:

- Gözden geçirme takım kimliği.
- Denetlenen içeriği.
- İş akışının adı. ("Varsayılan" iş akışı varsayılandır.)
- API çağırmanıza için bildirimler noktası.
 
Başlatılan iş tanıtıcısı şu yanıtı gösterir. İşin durumunu alın ve ayrıntılı bilgi almak için iş tanımlayıcısı kullanın.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>İş durumunu Al

Kullanım `Job.Get` işlemi ve çalışan veya tamamlanmış bir işin ayrıntılarını almak için iş kimliği. İşlem, zaman uyumsuz olarak hemen denetimi iş çalışırken döndürür. Geri çağırma uç noktası aracılığıyla sonuçlar döndürülür.

Girişlerinizi aşağıdaki bilgileri ekleyin:

- Gözden geçirme Takım Kimliği: Önceki işlem tarafından döndürülen iş tanımlayıcısı

Yanıt aşağıdaki bilgileri içerir:

- Oluşturulan gözden geçirme tanımlayıcısı. (Son gözden geçirme sonuçlarını almak için bu kimliği kullanın.)
- (Tamamlanan veya devam eden) iş durumu: Atanan denetimi etiketleri (anahtar-değer çiftleri).
- İş yürütme raporu.
 
 
        {
            "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
            "TeamName": "some team name",
            "Status": "Complete",
            "WorkflowId": "OCR",
            "Type": "Image",
            "CallBackEndpoint": "",
            "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
            "ResultMetaData":[
            {
            "Key": "hasText",
            "Value": "True"
            },
            {
            "Key": "ocrText",
            "Value": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
            }
            ],
            "JobExecutionReport": [
            {
                "Ts": "2018-01-07T00:38:29.3238715",
                "Msg": "Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
                },
                {
                "Ts": "2018-01-07T00:38:29.2928416",
                "Msg": "Job marked completed and job content has been removed"
                },
                {
                "Ts": "2018-01-07T00:38:29.0856472",
                "Msg": "Execution Complete"
                },
            {
                "Ts": "2018-01-07T00:38:26.7714671",
                "Msg": "Successfully got hasText response from Moderator"
                },
                {
                "Ts": "2018-01-07T00:38:26.4181346",
                "Msg": "Getting hasText from Moderator"
                },
                {
                "Ts": "2018-01-07T00:38:25.5122828",
                "Msg": "Starting Execution - Try 1"
                }
            ]
        }
 
![İnsan denetimciler için görüntü incelemesi](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Gözden geçirme işlemleri

### <a name="create-reviews"></a>Gözden geçirmeler oluşturma

Kullanım `Review.Create` incelemelere oluşturma işlemi. Başka bir yerde Orta ya da denetimi etiketleri atamak için Özel mantık kullanın.

Bu işlem için girişlerinizi şunlardır:

- Gözden geçirilmesi içeriği.
- İnsan Moderatörler tarafından gözden geçirme için atanan etiketleri (anahtar-değer çiftlerinin).

Gözden geçirme tanımlayıcısı şu yanıtı gösterir:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Gözden geçirme durumu alma
Kullanım `Review.Get` aracılı görüntünün bir insan tarafından İnceleme tamamlandıktan sonra sonuçları almak için işlemi. Sağlanan geri çağırma uç noktanız ile bildirim alın. 

İşlem, iki etiket döndürür: 

* Denetimi hizmeti tarafından atanan etiketleri
* İnsan tarafından İnceleme tamamlandıktan sonra etiketleri

En azından girişlerinizi şunlardır:

- Gözden geçirme takım adı
- Önceki işlem tarafından döndürülen gözden geçirme tanımlayıcısı

Yanıt aşağıdaki bilgileri içerir:

- Gözden geçirme durumu
- İnsan tarafından inceleme tarafından onaylanan etiketleri (anahtar-değer çiftleri)
- Denetimi hizmeti tarafından atanan etiketleri (anahtar-değer çiftleri)

Her iki İnceleme atanan etiketleri görürsünüz (**reviewerResultTags**) ve başlangıç etiketleri (**meta verileri**) aşağıdaki örnek yanıt:

    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="next-steps"></a>Sonraki adımlar

* Sürücü test [iş API Konsolu](try-review-api-job.md)ve REST API kod örnekleri kullanın. Visual Studio ve C# ile ilgili bilgi sahibi değilseniz, ayrıca kullanıma [işleri .NET Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md). 
* İncelemeleri için kullanmaya başlama [gözden geçirme API Konsolu](try-review-api-review.md)ve REST API kod örnekleri kullanın. Daha sonra bkz [incelemeleri .NET Hızlı Başlangıç](moderation-reviews-quickstart-dotnet.md).
* Görüntü incelemeleri kullanın [Video gözden geçirme Hızlı Başlangıç](video-reviews-quickstart-dotnet.md)ve öğrenin nasıl [dökümleri video gözden geçirici ekleyin](video-transcript-reviews-quickstart-dotnet.md).
