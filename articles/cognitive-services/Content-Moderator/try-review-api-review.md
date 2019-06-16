---
title: Moderation incelemeleri REST API Konsolu - Content Moderator ile oluşturma
titlesuffix: Azure Cognitive Services
description: Görüntü veya metin incelemeleri insan tarafından denetim için oluşturmak için Azure Content Moderator gözden geçirme API'leri kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 254269ccedc92b9dfc164cc4665a8a8513682773
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60607532"
---
# <a name="create-human-reviews-rest"></a>İncelemelere (REST) oluşturma

[Gözden geçirmeler](./review-api.md#reviews) depolamak ve değerlendirmek İnsan Moderatörler içeriğini görüntüler. Bir kullanıcı bir gözden geçirme tamamlandığında, sonuçları bir belirtilen geri çağırma uç noktasına gönderilir. Bu kılavuzda, API Konsolu aracılığıyla gözden geçirme REST API'lerini kullanarak incelemeleri ayarlama öğreneceksiniz. API'leri yapısını anladığınızda, herhangi bir REST uyumlu platform çağrıları kolayca bağlantı noktası.

## <a name="prerequisites"></a>Önkoşullar

- Oturum açma veya hesap üzerinde Content Moderator oluşturmak [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) site.

## <a name="create-a-review"></a>Bir gözden geçirmesi oluşturma

Bir gözden geçirme oluşturmak için Git **[gözden geçirin - oluşturma](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4)** API başvuru sayfası ve anahtar bölgenize ilişkin düğmeyi seçin (Bu uç nokta URL'sini bulabileceğiniz **kimlik bilgilerini** sayfası ' ın [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/)). Bu, kolayca oluşturun ve REST API çağrılarının API Konsolu başlatır.

![Gözden geçirme - Get kayıt seçimi](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST çağrısı parametreler girin

İçin değerler girin **teamName**, ve **Ocp-Apim-Subscription-Key**:

- **teamName**: Ayarlarken oluşturduğunuz takım kimliği, [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabı (bulunan **kimliği** gözden geçirme aracının kimlik bilgilerini ekranda alan).
- **Ocp-Apim-Subscription-Key**: Content Moderator anahtar. Bunu şirket bulabilirsiniz **ayarları** sekmesinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com).

### <a name="enter-a-review-definition"></a>Bir gözden geçirme tanımı girin

Düzen **istek gövdesi** kutusunu JSON isteği şu alanlara sahip girin:

- **meta veri**: Geri çağırma uç noktanıza döndürülmesi gereken özel anahtar-değer çiftleri. Anahtar tanımlanan kısa bir kod olup olmadığını [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com), bir etiket olarak görüntülenir.
- **İçerik**: Görüntü ve Video içerikleri söz konusu olduğunda içeriği işaret eden bir URL dize budur. Metin içeriği için bu gerçek bir metin dizesidir.
- **ContentID**: Bir özel kimlik dizesi. Bu dize API için geçirilen ve geri döndürdü. İç tanımlayıcılar veya meta veri denetimi iş sonuçları ile ilişkilendirmek için kullanışlıdır.
- **CallbackEndpoint**: (İsteğe bağlı) Gözden Geçirme tamamlandığında, geri çağırma bilgilerini almak için URL.

Varsayılan istek gövdesi, incelemeleri oluşturabileceğiniz farklı türde örneklerini gösterir:

```json
[Image]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Image",
    "Content": "<Content Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Text]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Text",
    "Content": "<Your Text Content>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Video]
[
  {
    "VideoFrames":[
      {
          "Id": "<Frame Id>",
          "Timestamp": "<Frame Timestamp",
          "FrameImage":"<Frame Image URL",
          "Metadata": [
            {
              "Key": "<Key>",
              "Value": "<Value"
            }
          ],
          "ReviewerResultTags": [
          ]
    ], 
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      },
      //For encrypted Videos
        {
          "Key": "protectedType",
          "Value": "AES or FairPlay or Widevine or Playready"
        },
        {
          "Key": "authenticationToken",
          "Value": "your viewtoken(In case of Video Indexer AES encryption type, this value is viewtoken from breakdown json)"
        },
      //For FairPlay encrypted type video include certificateUrl as well
        {
          "Key": "certificateUrl",
          "Value": "your certificate url"
        }
    ],
    "Type": "Video",
    "Content": "<Stream Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>",
    [Optional]
    "Timescale": "<Timescale of the video>
  }
]
```

### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusu İnceleme Kimliğini görüntüler. Aşağıdaki adımlarda kullanmak için bu kimliği kopyalayın.

![Gözden geçirme - Konsolu yanıt içerik kutusu İnceleme Kimliğini görüntüler oluşturma](images/test-drive-review-2.PNG)

### <a name="examine-the-new-review"></a>Yeni gözden inceleyin

İçinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com)seçin **gözden geçirme** > **görüntü**/**metin** / **Video** (hangi içeriğe bağlı olarak, kullandığınız). Karşıya yüklenen içerik görünmelidir, insan tarafından İnceleme için hazır.

![Gözden geçirme aracı bir futbol Top görüntüsü](images/test-drive-review-5.PNG)

## <a name="get-review-details"></a>Gözden geçirme ayrıntıları alın

Mevcut bir gözden geçirme hakkında ayrıntıları almak için şu adrese gidin [gözden geçirin: alma](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) API başvuru sayfası ve bölgenize ilişkin düğmeyi seçin (anahtarınızı yönetilir bölge).

![İş akışı - Get kayıt seçimi](images/test-drive-region.png)

Yukarıdaki bölümde olduğu gibi REST çağrısı parametrelerini girin. Bu adım için **reviewId** gözden oluştururken aldığınız benzersiz kimliği dizesi.

![Gözden geçirme - Get sonuçları konsol oluşturun](images/test-drive-review-3.PNG)
  
**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusu gözden geçirme ayrıntıları aşağıdaki gibi bir JSON biçiminde görüntüler:

```json
{  
  "reviewId":"201712i46950138c61a4740b118a43cac33f434",
  "subTeam":"public",
  "status":"Complete",
  "reviewerResultTags":[  
    {  
      "key":"a",
      "value":"False"
    },
    {  
      "key":"r",
      "value":"True"
    },
    {  
      "key":"sc",
      "value":"True"
    }
  ],
  "createdBy":"<teamname>",
  "metadata":[  
    {  
      "key":"sc",
      "value":"true"
    }
  ],
  "type":"Image",
  "content":"https://reviewcontentprod.blob.core.windows.net/<teamname>/IMG_201712i46950138c61a4740b118a43cac33f434",
  "contentId":"0",
  "callbackEndpoint":"<callbackUrl>"
}
```

Yanıt aşağıdaki alanlara dikkat edin:

- **status**
- **reviewerResultTags**: Bu, herhangi bir etiket el ile insan tarafından İnceleme ekibi tarafından eklenmişse görünür (gösterilen **oluşturan** alan).
- **meta veri**: Bu incelemede, insan tarafından İnceleme team yapılmış değişiklikler önce başlangıçta eklenmiş etiketler gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, REST API kullanarak içerik denetleme incelemeleri oluşturulacağını öğrendiniz. Ardından, incelemeleri gibi bir denetimi uçtan uca senaryo tümleştirme [E-ticaret denetimi](./ecommerce-retail-catalog-moderation.md) öğretici.