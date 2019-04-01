---
title: REST API Konsolu - Content Moderator ile denetimi iş akışlarını tanımlayın
titlesuffix: Azure Cognitive Services
description: Azure içeriği Moderator İnceleme API'lerini, özel iş akışları ve içerik, ilkelere dayalı eşikleri tanımlamak için kullanabilirsiniz.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: e150b1321f2fbd348e737222c752203281503643
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756593"
---
# <a name="define-and-use-moderation-workflows-rest"></a>Tanımlama ve denetleme iş akışları (REST) kullanma

İçeriği daha verimli bir şekilde işlemek için kullanabileceğiniz bulut tabanlı özelleştirilmiş filtreler iş akışlarıdır. İş akışları çeşitli içerik farklı yollarla filtreleyebilir ve ardından uygun eylemi gerçekleştirin için hizmetler için bağlanabilirsiniz. Bu kılavuzda iş akışları oluşturup kullanacağınızı API konsolu üzerinden iş akışı REST API'lerini kullanmayı gösterir. API'leri yapısını anladığınızda, herhangi bir REST uyumlu platform çağrıları kolayca bağlantı noktası.

## <a name="prerequisites"></a>Önkoşullar

- Oturum açma veya hesap üzerinde Content Moderator oluşturmak [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) site.

## <a name="create-a-workflow"></a>Bir iş akışı oluşturma

Oluşturun veya bir iş akışı güncelleştirmek için Git **[iş akışı - oluştur veya Güncelleştir](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59)** API başvuru sayfası ve anahtar bölgenize ilişkin düğmeyi seçin (Bu uç nokta URL'sini bulabileceğiniz **kimlik bilgileri**  sayfasının [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/)). Bu, kolayca oluşturun ve REST API çağrılarının API Konsolu başlatır.

![İş akışı - oluşturma veya güncelleştirme sayfa bölge seçimi](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST çağrısı parametreler girin

İçin değerler girin **takım**, **workflowname**, ve **Ocp-Apim-Subscription-Key**:

- **Takım**: Ayarlarken oluşturduğunuz takım kimliği, [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabı (bulunan **kimliği** gözden geçirme aracının kimlik bilgilerini ekranda alan).
- **workflowname**: Eklemek için yeni bir iş akışı (veya var olan adı mevcut bir iş akışı güncelleştirmek istiyorsanız) adı.
- **Ocp-Apim-Subscription-Key**: Content Moderator anahtar. Bunu şirket bulabilirsiniz **ayarları** sekmesinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com).

![İş akışı - güncelleştirme veya konsol Sorgu parametrelerinin ve üst bilgileri oluşturma](images/workflow-console-parameters.PNG)

### <a name="enter-a-workflow-definition"></a>Bir iş akışı tanımı girin

1. Düzen **istek gövdesi** JSON isteği ayrıntılarını girmek için kutusu **açıklama** ve **türü** (ya da `Image` veya `Text`).
2. İçin **ifade**, varsayılan iş akışı JSON ifadesini kopyalayın. Son JSON dizesi şöyle görünmelidir:

```json
{
  "Description":"<A description for the Workflow>",
  "Type":"Text",
  "Expression":{
    "Type":"Logic",
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isAdult",
      "Operator":"eq",
      "Value":"true",
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    }
  }
}
```

> [!NOTE]
> Bu API kullanarak iş akışlarınızı için basit, karmaşık ve hatta iç içe geçmiş deyimler tanımlayabilirsiniz. [İş akışı - oluşturma veya güncelleştirme](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) belgeleri sahip daha karmaşık mantık örnekleri.

### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusunda görüntüler `true`.

### <a name="examine-the-new-workflow"></a>Yeni iş akışını inceleyin

İçinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/)seçin **ayarları** > **iş akışları**. Yeni akışınız, listede görünmesi gerekir.

![İş akışlarının listesini gözden geçirme aracı](images/workflow-console-new-workflow.PNG)

Seçin **Düzenle** akışınız için seçenek ve Git **Tasarımcısı** sekmesi. Burada, sezgisel bir JSON mantıksal temsilini görebilirsiniz.

![Seçili bir iş akışı için Tasarımcı sekmesi](images/workflow-console-new-workflow-designer.PNG)

## <a name="get-workflow-details"></a>İş akışı ayrıntılarını Al

Varolan bir iş akışı ayrıntılarını almak için şu adrese gidin **[iş akışı - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58)** API başvuru sayfası ve bölgenize ilişkin düğmeyi seçin (anahtarınızı yönetilir bölge).

![İş akışı - Get kayıt seçimi](images/test-drive-region.png)

Yukarıdaki bölümde olduğu gibi REST çağrısı parametrelerini girin. Bu kez, emin **workflowname** varolan bir iş akışı adıdır.

![Sorgu parametrelerinin ve üst bilgileri Al](images/workflow-get-default.PNG)

**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusu iş akışı aşağıdaki gibi bir JSON biçiminde görüntüler:

```json
{
  "Name":"default",
  "Description":"Default",
  "Type":"Image",
  "Expression":{
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isadult",
      "Operator":"eq",
      "Value":"true",
      "AlternateInput":null,
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "Subteam":null,
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    },
    "Else":null,
    "Type":"Logic"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- İş akışlarıyla kullanmayı öğrenin [içerik denetimi işleri](try-review-api-job.md).