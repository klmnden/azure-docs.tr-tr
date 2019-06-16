---
title: REST API Konsolu - Content Moderator denetimi işleri kullanma
titlesuffix: Azure Cognitive Services
description: Azure Content Moderator, görüntü veya metin içeriği için uçtan uca içerik denetleme işleri başlatmak için gözden geçirme API iş işlemlerini kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 7827cee2af2dfc0c1fddc407c1d146dc9a66c514
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60607637"
---
# <a name="define-and-use-moderation-jobs-rest"></a>Tanımlama ve denetleme işleri (REST) kullanma

İçerik denetleme, iş akışları ve incelemeleri işlevsellik için sarmalayıcı türü bir denetimi iş görür. Bu kılavuzda başlatmak ve içerik denetleme işleri denetlemek için iş REST API'lerini kullanmayı gösterir. API'leri yapısını anladığınızda, herhangi bir REST uyumlu platform çağrıları kolayca bağlantı noktası.

## <a name="prerequisites"></a>Önkoşullar

- Oturum açma veya hesap üzerinde Content Moderator oluşturmak [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) site.
- (İsteğe bağlı) [Özel bir iş akışını tanımlayan](./Review-Tool-User-Guide/Workflows.md) ; işinizi ile kullanılacak varsayılan iş akışı kullanabilirsiniz.

## <a name="create-a-job"></a>Bir iş oluşturma

Bir denetimi işi oluşturmak için Git [iş - oluşturma](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) API başvuru sayfası ve anahtar bölgenize ilişkin düğmeyi seçin (Bu uç nokta URL'sini bulabileceğiniz **kimlik bilgilerini** sayfasının [gözden geçirme Aracı](https://contentmoderator.cognitive.microsoft.com/)). Bu, kolayca oluşturun ve REST API çağrılarının API Konsolu başlatır.

![İş - kayıt seçimi sayfası oluşturma](images/test-drive-job-1.png)

### <a name="enter-rest-call-parameters"></a>REST çağrısı parametreler girin

REST araması oluşturmak için aşağıdaki değerleri girin:

- **teamName**: Ayarlarken oluşturduğunuz takım kimliği, [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabı (bulunan **kimliği** gözden geçirme aracının kimlik bilgilerini ekranda alan).
- **ContentType**: Bu, "Image", "Metin" veya "Görüntü" olabilir.
- **ContentID**: Bir özel kimlik dizesi. Bu dize API için geçirilen ve geri döndürdü. İç tanımlayıcılar veya meta veri denetimi iş sonuçları ile ilişkilendirmek için kullanışlıdır.
- **workflowname**: İş akışı, daha önce oluşturduğunuz (veya varsayılan iş akışı için "varsayılan") adı.
- **CallbackEndpoint**: (İsteğe bağlı) Gözden Geçirme tamamlandığında, geri çağırma bilgilerini almak için URL.
- **Ocp-Apim-Subscription-Key**: Content Moderator anahtar. Bunu şirket bulabilirsiniz **ayarları** sekmesinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com).

### <a name="fill-in-the-request-body"></a>İstek gövdesinde doldurun

Bir alan, REST çağrısı gövdesi içerir **ContentValue**. Metin yönetme, ham metin içeriği yapıştırın veya görüntü/video yönetme bir resim veya video URL'si girin. Aşağıdaki örnek resim URL'si kullanabilirsiniz: [https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg)

![Proje - konsol sorgu parametreleri, üst bilgileri ve istek gövdesi kutusu oluşturma](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>İsteğinizi gönderin

**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusu iş Kimliğini görüntüler. Aşağıdaki adımlarda kullanmak için bu kimliği kopyalayın.

![Gözden geçirme - Konsolu yanıt içerik kutusu İnceleme Kimliğini görüntüler oluşturma](images/test-drive-job-3.PNG)

## <a name="get-job-status"></a>İş durumunu Al

Çalışan veya tamamlanmış bir işin ayrıntılarını ve durumunu almak için Git [iş - Al](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c3) API başvuru sayfası ve bölgenize ilişkin düğmeyi seçin (anahtarınızı yönetilir bölge).

![İş - Get kayıt seçimi](images/test-drive-region.png)

Yukarıdaki bölümde olduğu gibi REST çağrısı parametrelerini girin. Bu adım için **JobId** , işi oluştururken aldığınız benzersiz kimliği dizesi. **Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusu işi aşağıdaki gibi bir JSON biçiminde görüntüler:

```json
{  
  "Id":"2018014caceddebfe9446fab29056fd8d31ffe",
  "TeamName":"some team name",
  "Status":"Complete",
  "WorkflowId":"OCR",
  "Type":"Image",
  "CallBackEndpoint":"",
  "ReviewId":"201801i28fc0f7cbf424447846e509af853ea54",
  "ResultMetaData":[  
    {  
      "Key":"hasText",
      "Value":"True"
    },
    {  
      "Key":"ocrText",
      "Value":"IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
    }
  ],
  "JobExecutionReport":[  
    {  
      "Ts":"2018-01-07T00:38:29.3238715",
      "Msg":"Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
    },
    {  
      "Ts":"2018-01-07T00:38:29.2928416",
      "Msg":"Job marked completed and job content has been removed"
    },
    {  
      "Ts":"2018-01-07T00:38:29.0856472",
      "Msg":"Execution Complete"
    },
    {  
      "Ts":"2018-01-07T00:38:26.7714671",
      "Msg":"Successfully got hasText response from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:26.4181346",
      "Msg":"Getting hasText from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:25.5122828",
      "Msg":"Starting Execution - Try 1"
    }
  ]
}
```

![Proje: REST çağrısı yanıt alma](images/test-drive-job-5.png)

### <a name="examine-the-new-reviews"></a>Yeni review(s) inceleyin

İçerik işinizi İnceleme oluşturulmasında oluştuysa, içinde görüntüleyebilirsiniz [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com). Seçin **gözden geçirme** > **görüntü**/**metin**/**Video** (bağlı olarak, içerik kullanılır). İçerik görünmelidir, insan tarafından İnceleme için hazır. İnsan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini gözden geçirmeleri ve son denetimi karar gönderdikten sonra işleri API tüm bu bilgileri atanan geri çağırma uç noktası uç noktasına gönderir.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, oluşturma ve içerik denetleme işleri REST API kullanarak sorgulama hakkında bilgi edindiniz. Ardından, işleri gibi bir denetimi uçtan uca senaryo tümleştirme [E-ticaret denetimi](./ecommerce-retail-catalog-moderation.md) öğretici.