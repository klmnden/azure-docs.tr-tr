---
title: Azure içerik denetleyici içerik yönetimini işleri çalıştırma | Microsoft Docs
description: API konsolunda içerik yönetimini işlerini çalıştırmak öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/03/2017
ms.author: sajagtap
ms.openlocfilehash: 6f741be1001ae70d5fdbf6f374204aaad1601abe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351592"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>API konsolundan denetleme işi Başlat

Gözden geçirme API'nin kullanmak [iş işlemleri](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) Azure içeriği denetleyici görüntü veya metin içeriği için uçtan uca içerik yönetimini işleri başlatmasını. 

Denetleme işi içerik denetleyici Görüntü Yönetimi API veya metin denetleme API kullanarak içeriğinizi tarar. Ardından, denetleme işi incelemeler gözden geçirme aracında oluşturmak için iş akışları (gözden geçirme aracında tanımlanan) kullanır. 

İnsan denetleyici otomatik atanan etiketleri ve öngörü verileri gözden geçirir ve son denetleme karar gönderdikten sonra gözden geçirme API, API uç tüm bilgileri gönderir.

## <a name="prerequisites"></a>Önkoşullar

Gidin [aracı gözden](https://contentmoderator.cognitive.microsoft.com/). Siz bunu henüz yapmadıysanız kaydolun. Gözden geçirme araç içinde [özel bir iş akışında tanımlayın](Review-Tool-User-Guide/Workflows.md) bu kullanmak için `Job` işlemi.

## <a name="use-the-api-console"></a>API Konsolu
Çevrimiçi konsolunu kullanarak API dediğini konsola girmek için birkaç değerleri gerekir:
    
- `teamName`: Kullanın `Id` gözden geçirme aracın kimlik bilgilerini ekranından alan. 
- `ContentId`: Bu dize API için geçirilen ve geri döndürüldü. **ContentID** iç tanımlayıcıları veya meta veri sonuçlarını bir denetleme işi ile ilişkilendirmek için yararlıdır- `Workflowname`: adını [oluşturduğunuz iş akışı](Review-Tool-User-Guide/Workflows.md) önceki bölümdeki.
- `Ocp-Apim-Subscription-Key`: Üzerinde bulunduğu **ayarları** sekmesi. Daha fazla bilgi için bkz: [genel bakış](overview.md).

Erişim API Konsolu olan **kimlik bilgileri** penceresi.

### <a name="navigate-to-the-api-reference"></a>API referansı gidin
İçinde **kimlik bilgileri** penceresinde, seçin [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  `Job.Create` Sayfası açılır.

### <a name="select-your-region"></a>Bölgenizi seçin
İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.
  ![Proje - Sayfası bölge seçimi oluşturma](images/test-drive-job-1.png)

  `Job.Create` API konsolu açılır. 

### <a name="enter-parameters"></a>Parametreleri girin

Gerekli sorgu parametrelerini ve abonelik anahtarınızı değerlerini girin. İçinde **istek gövdesinde** kutusunda, taramak istediğiniz bilgilerin konumunu belirtin. Bu örnekte, bu kullanalım [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Proje - konsol sorgu parametreleri, üstbilgi ve istek gövdesi kutusu oluşturma](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>İsteğinizi gönderin
**Gönder**’i seçin. Bir iş kimliği oluşturulur. Bu sonraki adımlarda kullanmak üzere kopyalayın.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Alma işi Ayrıntılar sayfasını açın
Seçin **almak**ve ardından API bölgenizi eşleşen düğmesini seçerek açın.

  ![Proje - konsol Get sonuçları oluşturma](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Yanıt gözden geçirin

İçin değerler girin **teamName** ve **JobId**. Abonelik anahtarınızı girin ve ardından **Gönder**. Aşağıdaki yanıtı örnek iş durumu ve ayrıntıları gösterir.

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

## <a name="navigate-to-the-review-tool"></a>Gözden geçirme Aracı'na gidin
İçerik denetleyici Panoda seçin **gözden geçirme** > **görüntü**. Taranan görüntü görünür, İnsan gözden geçirme için hazır.

  ![Üç cyclists aracı görüntüsünü gözden geçirin](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Sonraki adımlar

REST API kodunuzdaki kullanın veya başlayın [işleri .NET quickstart](moderation-jobs-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
