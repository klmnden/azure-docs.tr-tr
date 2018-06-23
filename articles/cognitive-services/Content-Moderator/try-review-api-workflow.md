---
title: Azure içerik aracı - içerik yönetimini iş akışları API konsolundan | Microsoft Docs
description: API konsolundan İçerik Yönetimi iş akışlarının nasıl kullanılacağını öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/05/2018
ms.author: sajagtap
ms.openlocfilehash: 700b2bea5e902141659266a94d61ceb810c1b802
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351773"
---
# <a name="workflows-from-the-api-console"></a>API konsolundan iş akışları

Kullanım [iş akışı işlemlerini](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) oluşturmak veya bir iş akışını güncelleştirme veya gözden geçirme API'sini kullanarak iş akışı ayrıntıları almak için Azure içerik denetleyicinin içinde. Bu API kullanarak, iş akışları için basit, karmaşık ve hatta iç içe geçmiş ifadeler tanımlayabilirsiniz. İş akışları kullanılacak ekibiniz için İnceleme aracı görünür. İş akışları da gözden geçirme API'nin iş işlemleri tarafından kullanılır.

## <a name="prerequisites"></a>Önkoşullar

1. Git [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/). Bunu henüz yapmadıysanız kaydolun. 
2. Gözden geçirme aracında altında **ayarları**seçin **iş akışları** sekmesinde, gözden geçirme aracının gösterildiği gibi [iş akışı Öğreticisi](Review-Tool-User-Guide/Workflows.md).

### <a name="browse-to-the-workflows-screen"></a>İş akışları ekranına gidin

İçerik denetleyici Panoda seçin **gözden geçirme** > **ayarları** > **iş akışları**. Varsayılan iş akışı bakın.

  ![Varsayılan iş akışı](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Varsayılan iş akışı JSON tanımını Al

Seçin **Düzenle** iş akışınız için seçeneğini ve ardından **JSON** sekmesi. JSON ifadesini görürsünüz:

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

## <a name="get-workflow-details"></a>İş akışı ayrıntıları alma

Kullanım **iş akışı - Get** , var olan varsayılan iş akışı ayrıntılarını alma işlemi.

Gözden geçirme Aracı'nda Git [kimlik bilgileri](Review-Tool-User-Guide/credentials.md#the-review-tool) bölümü.

### <a name="browse-to-the-api-reference"></a>API referansı Gözat

1. İçinde **kimlik bilgileri** görünümü, select [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Zaman **iş akışı - oluştur veya Güncelleştir** sayfası açılır, Git [iş akışı - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) başvuru.

### <a name="select-your-region"></a>Bölgenizi seçin

İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.

  ![İş akışı - Get bölge seçimi](images/test-drive-region.png)

  **İş akışı - Get** API konsolu açılır.

### <a name="enter-parameters"></a>Parametreleri girin

İçin değerler girin **takım**, **workflowname**, ve **Apim abonelik anahtar Ocp** (abonelik anahtarınızı):

- **Takım**: ayarlarken oluşturduğunuz takım kimliği, [gözden aracı hesabı](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: iş akışınızı adı. Kullanım `default`.
- **Ocp Apim abonelik anahtar**: bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz: [genel bakış](overview.md).

  ![Sorgu parametrelerinin ve üst bilgileri alma](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa, **yanıt durumu** olan `200 OK`ve **yanıt içeriği** kutusu aşağıdaki JSON iş akışı görüntüler:

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


## <a name="create-a-workflow"></a>İş akışı oluşturma

Gözden geçirme Aracı'nda Git [kimlik bilgileri](Review-Tool-User-Guide/credentials.md#the-review-tool) bölümü.

### <a name="browse-to-the-api-reference"></a>API referansı Gözat

İçinde **kimlik bilgileri** görünümü, select [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). **İş akışı - oluştur veya Güncelleştir** sayfası açılır.

### <a name="select-your-region"></a>Bölgenizi seçin

İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.

  ![İş akışı - oluşturma veya güncelleştirme sayfa bölge seçimi](images/test-drive-region.png)

  **İş akışı - oluştur veya Güncelleştir** API konsolu açılır.

### <a name="enter-parameters"></a>Parametreleri girin

İçin değerler girin **takım**, **workflowname**, ve **Apim abonelik anahtar Ocp** (abonelik anahtarınızı):

- **Takım**: ayarlarken oluşturduğunuz takım kimliği, [gözden aracı hesabı](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: yeni iş akışınızı adı.
- **Ocp Apim abonelik anahtar**: bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz: [genel bakış](overview.md).

  ![İş akışı - veya güncelleştirme konsol sorgu parametreleri ve üstbilgi oluşturma](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>İş akışı tanımı girin

1. Düzen **istek gövdesinde** ayrıntılarını JSON istekle girmek için kutusunu **açıklama** ve **türü** (görüntü veya metin). 
2. İçin **ifade**, aşağıda gösterildiği gibi önceki bölümünden varsayılan iş akışı ifade kopyalayın:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    İstek gövdesi aşağıdaki JSON istek gibi görünür:

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
  
**Gönder**’i seçin. İşlem başarılı olursa, **yanıt durumu** olan `200 OK`ve **yanıt içeriği** kutusunda görüntüler `true`.

### <a name="check-out-the-new-workflow"></a>Yeni iş akışını denetleme

Gözden geçirme Aracı'nda seçin **gözden geçirme** > **ayarları** > **iş akışları**. Yeni iş akışınızı görünür ve kullanıma hazırdır.

  ![İş akışları aracı listesini gözden geçirin](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Yeni iş akışı ayrıntılarını gözden geçirin

1. Seçin **Düzenle** iş akışınız için seçeneğini ve ardından **Tasarımcısı** ve **JSON** sekmeleri.

   ![Tasarımcı sekmesi seçili iş akışı](images/workflow-console-new-workflow-designer.PNG)

2. İş akışı JSON görünümünü görmek için seçin **JSON** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

* Daha karmaşık iş akışı örnekleri için bkz: [iş akışlarına genel bakış](workflow-api.md).
* İş akışları ile kullanmayı öğrenin [içerik yönetimini işleri](try-review-api-job.md).
