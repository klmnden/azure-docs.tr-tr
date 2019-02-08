---
title: API Konsolu - Content Moderator içerik denetleme akışlarından
titlesuffix: Azure Cognitive Services
description: İş akışı işlemlerini Azure Content Moderator oluşturmak veya bir iş akışını güncelleştirme ya da gözden geçirme API'sini kullanarak iş akışı ayrıntıları almak için kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 1c18544a0fd135eb546660c442b865bf1249dfe5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883093"
---
# <a name="workflows-from-the-api-console"></a>API konsolu iş akışlarından

Kullanım [iş akışı işlemlerini](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) oluşturmak veya bir iş akışını güncelleştirme veya gözden geçirme API'sini kullanarak iş akışı ayrıntıları almak için Azure Content Moderator içinde. Bu API kullanarak basit, karmaşık ve hatta iç içe geçmiş ifadeler için iş akışlarınızı tanımlayabilirsiniz. İnceleme aracını kullanmak, takımınız için iş akışlarını görünür. İş akışları da gözden geçirme API'nin iş işlemleri tarafından kullanılır.

## <a name="prerequisites"></a>Önkoşullar

1. Git [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/). Henüz yapmadıysanız kaydolun. 
2. Gözden geçirme Aracı'nda altında **ayarları**seçin **iş akışları** sekmesinde, gözden geçirme aracının gösterildiği [iş akışı öğretici](Review-Tool-User-Guide/Workflows.md).

### <a name="browse-to-the-workflows-screen"></a>İş akışları ekrana Gözat

Content Moderator Panoda seçin **gözden geçirme** > **ayarları** > **iş akışları**. Varsayılan iş akışı görürsünüz.

  ![Varsayılan iş akışı](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Varsayılan iş akışı JSON tanımını Al

Seçin **Düzenle** akışınız için seçeneğini ve ardından **JSON** sekmesi. Aşağıdaki JSON ifadesini görürsünüz:

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

## <a name="get-workflow-details"></a>İş akışı ayrıntılarını Al

Kullanım **iş akışı - Get** var olan varsayılan iş ayrıntılarını almak için işlemi.

İnceleme aracında Git [kimlik bilgilerini](Review-Tool-User-Guide/credentials.md#the-review-tool) bölümü.

### <a name="browse-to-the-api-reference"></a>API Başvurusu için Gözat

1. İçinde **kimlik bilgilerini** görüntülenecek [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Zaman **iş akışı - oluşturma veya güncelleştirme** sayfası açıldıktan sonra Git [iş akışı - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) başvuru.

### <a name="select-your-region"></a>Bölgenizi seçin

İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.

  ![İş akışı - Get kayıt seçimi](images/test-drive-region.png)

  **İş akışı - Get** API konsolu açılır.

### <a name="enter-parameters"></a>Parametreleri girin

İçin değerler girin **takım**, **workflowname**, ve **Ocp-Apim-Subscription-Key** (abonelik anahtarınızı):

- **Takım**: Ayarlarken oluşturduğunuz takım kimliği, [gözden geçirme aracı hesabı](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: İş akışınızı adı. Kullanım `default`.
- **Ocp-Apim-Subscription-Key**: Bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

  ![Sorgu parametrelerinin ve üst bilgileri Al](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusu görüntüler aşağıdaki JSON iş akışı:

    {
        "Name": "default",
        "Description": "Default",
        "Type": "Image",
        "Expression": {
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isadult",
            "Operator": "eq",
            "Value": "true",
            "AlternateInput": null,
            "Type": "Condition"
            },
        "Then": {
            "Perform": [{
                "Name": "createreview",
                "Subteam": null,
                "CallbackEndpoint": null,
                "Tags": []
            }],
            "Type": "Actions"
            },
            "Else": null,
            "Type": "Logic"
            }
    }


## <a name="create-a-workflow"></a>Bir iş akışı oluşturma

İnceleme aracında Git [kimlik bilgilerini](Review-Tool-User-Guide/credentials.md#the-review-tool) bölümü.

### <a name="browse-to-the-api-reference"></a>API Başvurusu için Gözat

İçinde **kimlik bilgilerini** görüntülenecek [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). **İş akışı - oluşturma veya güncelleştirme** sayfası açılır.

### <a name="select-your-region"></a>Bölgenizi seçin

İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.

  ![İş akışı - oluşturma veya güncelleştirme sayfa bölge seçimi](images/test-drive-region.png)

  **İş akışı - oluşturma veya güncelleştirme** API konsolu açılır.

### <a name="enter-parameters"></a>Parametreleri girin

İçin değerler girin **takım**, **workflowname**, ve **Ocp-Apim-Subscription-Key** (abonelik anahtarınızı):

- **Takım**: Ayarlarken oluşturduğunuz takım kimliği, [gözden geçirme aracı hesabı](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: Yeni akışınız adı.
- **Ocp-Apim-Subscription-Key**: Bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

  ![İş akışı - güncelleştirme veya konsol Sorgu parametrelerinin ve üst bilgileri oluşturma](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>İş akışı tanımı girin

1. Düzen **istek gövdesi** JSON isteği ayrıntılarını girmek için kutusu **açıklama** ve **türü** (görüntü veya metin). 
2. İçin **ifade**, burada gösterildiği gibi varsayılan iş akışı ifadesi önceki bölümden kopyalayın:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    İstek gövdesi, aşağıdaki JSON istek gibi görünür:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": {
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
                    "Tags": [ ]
                }
                ],
                "Type": "Actions"
                }
            }
        }
 
### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa **yanıt durumu** olduğu `200 OK`ve **yanıt içeriği** kutusunda görüntüler `true`.

### <a name="check-out-the-new-workflow"></a>Yeni iş akışını denetleme

İnceleme aracında seçin **gözden geçirme** > **ayarları** > **iş akışları**. Yeni akışınız görüntülenir ve kullanıma hazır.

  ![İş akışlarının listesini gözden geçirme aracı](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Yeni iş akışı ayrıntılarını gözden geçirin

1. Seçin **Düzenle** akışınız için seçeneğini ve ardından **Tasarımcısı** ve **JSON** sekmeler.

   ![Seçili bir iş akışı için Tasarımcı sekmesi](images/workflow-console-new-workflow-designer.PNG)

2. İş akışı JSON görünümünü görmek için seçin **JSON** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

* Daha karmaşık iş akışı örnekleri için bkz. [iş akışlarına genel bakış](workflow-api.md).
* İş akışlarıyla kullanmayı öğrenin [içerik denetimi işleri](try-review-api-job.md).
