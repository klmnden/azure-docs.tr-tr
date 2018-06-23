---
title: Azure içerik aracı - yönetimini işleri ve İnsan-içinde--döngü incelemeler | Microsoft Docs
description: İnsan gözetim makine destekli yönetimini en iyi sonuçlar için geçerlidir.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/21/2018
ms.author: sajagtap
ms.openlocfilehash: 35b3cdaa410712c3fd08d3df4ebe4c83e3955d50
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351767"
---
# <a name="moderation-jobs-and-reviews"></a>Denetleme işler ve gözden geçirme

Azure içerik denetleyici kullanarak makine destekli yönetimini insan-içinde--döngü özellikleri ile birleştirme [gözden geçirme API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) işletmeniz için en iyi sonuçları almak için.

Gözden geçirme API İnsan gözetim içerik yönetimini işleminize eklemek için aşağıdaki yöntemleri sunar:

* `Job` işlemler, makine destekli denetleme ve İnsan gözden geçirme oluşturma bir adım olarak başlatmak için kullanılır.
* `Review` işlemleri denetleme adım dışında İnsan gözden geçirme oluşturmak için kullanılır.
* `Workflow` işlemleri gözden geçirme oluşturma eşiklerle tarama otomatikleştiren iş akışlarını yönetmek için kullanılır.

`Job` Ve `Review` işlemleri durumunu ve sonuçları almak için geri çağırma noktalarınızı kabul edin.

Bu makalede yer almaktadır `Job` ve `Review` işlemleri. Okuma [iş akışlarına genel bakış](workflow-api.md) nasıl oluşturulacağı hakkında bilgi için düzenleme ve iş akışı tanımları alın.

## <a name="job-operations"></a>İş işlemleri

### <a name="start-a-job"></a>Bir işi Başlat
Kullanım `Job.Create` denetleme ve İnsan gözden geçirme oluşturma işi başlatmak için işlemi. İçerik denetleyici içeriğini tarar ve belirtilen iş akışı değerlendirir. İş akışı sonuçlarına bağlı olarak incelemeler oluşturur ya da adım atlar. Ayrıca, geri çağırma uç noktanızı sonrası denetleme ve sonrası gözden geçirme etiketleri gönderir.

Girdiler aşağıdaki bilgileri içerir:

- Gözden geçirme takım kimliği
- Aracılı içeriği.
- İş akışı adı. ("Varsayılan" iş akışı varsayılandır.)
- API geri bildirimleri için gelin.
 
Aşağıdaki yanıtı başlatıldı iş tanımlayıcısını gösterir. İş durumunu almak ve ayrıntılı bilgi almak için iş tanımlayıcısı kullanın.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>İş durumunu Al

Kullanım `Job.Get` işlemi ve çalışan ya da tamamlanmış bir iş ayrıntılarını almak için iş tanımlayıcısı. İşlemi hemen denetleme işi zaman uyumsuz olarak çalışırken döndürür. Sonuçları geri çağırma uç noktası aracılığıyla döndürülür.

Girişlerinizi aşağıdaki bilgileri içerir:

- Gözden geçirme Takım Kimliği: önceki işlem tarafından döndürülen iş tanımlayıcısı

Yanıt aşağıdaki bilgileri içerir:

- Oluşturulan gözden geçirme tanımlayıcısı. (Bu kimliği son gözden sonuçları almak için kullanın.)
- (Tamamlanmış veya devam eden) işinin durumu: atanan yönetimini etiketler (anahtar-değer çiftleri).
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
 
![İnsan araburucu için görüntü gözden geçirme](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Gözden geçirme işlemleri

### <a name="create-reviews"></a>Gözden geçirmeler oluşturma

Kullanım `Review.Create` İnsan incelemeler oluşturma işlemi. Başka bir yerde Orta ya da özel mantık denetleme etiketleri atamak için kullanın.

Bu işlem için girişlerinizi şunları içerir:

- Gözden geçirilecek içeriği.
- Gözden geçirme için İnsan araburucu tarafından atanan etiketler (anahtar değer çiftleri).

Aşağıdaki yanıtı gözden geçirme tanımlayıcısını gösterir:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Gözden geçirme durumunu Al
Kullanım `Review.Get` aracılı görüntü İnsan gözden tamamlandıktan sonra sonuçları alma işlemi. Sağlanan geri çağırma uç noktanızı bilgi edinin. 

İşlemi iki etiketlerin döndürür: 

* Denetleme hizmeti tarafından atanan etiketleri
* İnsan İnceleme tamamlandıktan sonra etiketleri

Girişlerinizi en azından aşağıdakileri içerir:

- Gözden geçirme ekip adı
- Önceki işlem tarafından döndürülen gözden geçirme tanımlayıcısı

Yanıt aşağıdaki bilgileri içerir:

- Durumunu gözden geçir
- İnsan Gözden Geçiren tarafından onaylanan etiketler (anahtar-değer çiftleri)
- Denetleme hizmeti tarafından atanan etiketler (anahtar-değer çiftleri)

Her iki İnceleme atanan etiketler konusuna bakın (**reviewerResultTags**) ve ilk etiketleri (**meta verileri**) aşağıdaki örnek yanıt:

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

* Test sürücü [iş API Konsolu](try-review-api-job.md)ve REST API kod örnekleri kullanır. Visual Studio ve C# ile sahibiyseniz, ayrıca kullanıma [işleri .NET quickstart](moderation-jobs-quickstart-dotnet.md). 
* İncelemeler için kullanmaya başlama [gözden geçirme API Konsolu](try-review-api-review.md)ve REST API kod örnekleri kullanır. Daha sonra bkz [incelemeler .NET quickstart](moderation-reviews-quickstart-dotnet.md).
* Video incelemeleri kullanın [Video gözden geçirme quickstart](video-reviews-quickstart-dotnet.md)ve öğrenin nasıl [video incelemeye dökümleri eklemek](video-transcript-reviews-quickstart-dotnet.md).
