---
title: API Konsolu - Content Moderator içerik denetleme işleri çalıştırma
titlesuffix: Azure Cognitive Services
description: Azure Content Moderator, görüntü veya metin içeriği için uçtan uca içerik denetleme işleri başlatmak için gözden geçirme API iş işlemlerini kullanın.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 4d1f96cbf7a94c59476f077cc4e72a26ee9c8296
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259130"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>API konsolundan denetimi işi başlatın

Gözden geçirme API'nin kullanın [iş işlemleri](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) Azure Content Moderator, görüntü veya metin içeriği için uçtan uca içerik denetleme işleri başlatmak için. 

Denetimi işi içeriği Moderator resim denetimi API'si veya metin denetimi API'si kullanarak içeriğinizi tarar. Ardından, denetimi işi gözden geçirmeleri gözden geçirme Aracı'nda oluşturmak için iş akışları (İnceleme aracında tanımlanır) kullanır. 

İnsan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini gözden geçirmeleri ve son denetimi karar gönderdikten sonra gözden geçirme API, API uç noktanıza tüm bilgileri gönderir.

## <a name="prerequisites"></a>Önkoşullar

Gidin [İnceleme aracında](https://contentmoderator.cognitive.microsoft.com/). Henüz yapmadıysanız, kaydolun. İnceleme aracını içinde [özel bir iş akışını tanımlayan](Review-Tool-User-Guide/Workflows.md) bu kullanılacak `Job` işlemi.

## <a name="use-the-api-console"></a>API Konsolu
API çevrimiçi Konsolu aracılığıyla dediğini konsoluna girmek için bazı değerler gerekir:
    
- `teamName`: Kullanım `Id` gözden geçirme aracının kimlik bilgileri ekran alanını. 
- `ContentId`: Bu dize API için geçirilen ve geri döndürdü. **ContentID** iç tanımlayıcılar veya meta veri denetimi iş sonuçları ile ilişkilendirmek için kullanışlıdır.- `Workflowname`: Adını [oluşturduğunuz iş akışı](Review-Tool-User-Guide/Workflows.md) önceki bölümde.
- `Ocp-Apim-Subscription-Key`: Bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

Erişim API Konsolu geldiği **kimlik bilgilerini** penceresi.

### <a name="navigate-to-the-api-reference"></a>API başvuruya Git
İçinde **kimlik bilgilerini** penceresinde [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  `Job.Create` Sayfası açılır.

### <a name="select-your-region"></a>Bölgenizi seçin
İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.
  ![İş - kayıt seçimi sayfası oluşturma](images/test-drive-job-1.png)

  `Job.Create` API konsolu açılır. 

### <a name="enter-parameters"></a>Parametreleri girin

Gerekli sorgu parametreleri ve abonelik anahtarınız için değerleri girin. İçinde **istek gövdesi** kutusunda, taramak istediğiniz bilgilerin konumunu belirtin. Bu örnekte, bu kullanalım [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Proje - konsol sorgu parametreleri, üst bilgileri ve istek gövdesi kutusu oluşturma](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>İsteğinizi gönderin
**Gönder**’i seçin. Bir iş kimliği oluşturulur. Bu sonraki adımlarda kullanılacak kopyalayın.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Alma işi Ayrıntılar sayfasını açın
Seçin **alma**ve API bölgenizi eşleşen düğmesini seçerek açın.

  ![İş oluşturma - konsol Get sonuçları](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Gözden geçirme yanıtı

İçin değerler girin **teamName** ve **JobId**. Abonelik anahtarınızı girin ve ardından **Gönder**. Örnek İş durumu ve ayrıntıları şu yanıtı gösterir.

```
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": 
        [
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
```

## <a name="navigate-to-the-review-tool"></a>Gözden geçirme aracına gidin
Content Moderator Panoda seçin **gözden geçirme** > **görüntü**. Taranan görüntü görünür, insan tarafından İnceleme için hazır.

  ![Gözden geçirme aracı üç cyclists görüntüsü](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [işleri .NET Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
