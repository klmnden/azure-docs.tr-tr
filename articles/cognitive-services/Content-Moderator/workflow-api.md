---
title: Denetimi iş akışları - Content Moderator
titlesuffix: Azure Cognitive Services
description: İş akışlarını gözden geçirme API'nin iş işlemleri ile İnsan içinde--döngüsü incelemeleri, içerik ilkeleri ve eşiklere göre otomatikleştirmek için kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0a13d86afe3d395cb34f592b03c1eb9daa18076b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57454867"
---
# <a name="automate-moderation-reviews-with-workflows"></a>Denetimi incelemeleriyle iş akışlarını otomatikleştirin

Content Moderator, araçları ve iş akışları yönetmek için API içerir. İş akışları ile kullandığınız [gözden API'nin işe yönelik işlemleri](review-api.md) , içerik ilkeleri ve eşiklere göre İnsan içinde--döngüsü gözden geçirme oluşturmayı otomatikleştirmek için.

Gözden geçirme API İnsan gözetiminin içerik denetleme işleminize dahil etmek için aşağıdaki yöntemleri sunar:

1. **İş** makine Yardımlı resim denetimi ve İnsan başlatma işlemleri oluşturma bir adım olarak gözden geçirin.
1. **Gözden** İnsan işlemlerinde oluşturma, denetimi adım dışında gözden geçirin.
1. **İş akışı** eşikler ile tarama otomatik hale getiren iş akışlarını yönetme için işlem oluşturma gözden geçirin.

Bu makalede ele alınmaktadır **iş akışı** operations. Okuma [işler ve incelemeleri](review-api.md) içerik denetleme hakkında bilgi edinmek için genel bakış işler ve inceler.

Kullanıma **varsayılan** Content Moderator anlama akışlarında kullanmaya başlamak için en iyi yolu iş akışıdır.

## <a name="your-first-workflow"></a>İlk akışınızı

İle ilk iş akışınızı sunulur, [aracı takım gözden](https://contentmoderator.cognitive.microsoft.com/). Bunu zaten yapmadıysanız kaydolun.

Gidin [Aracı'nın iş akışlarını gözden geçirin](Review-Tool-User-Guide/Workflows.md) ekran Ayarlar sekmesi altında. Gördüğünüz bir **varsayılan** aşağıdaki görüntüde gösterildiği gibi iş akışı:

![Content Moderator iş akışları](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>Varsayılan iş akışı açın

Kullanım **Düzenle** seçeneği sayfasında aşağıdaki görüntüde gösterildiği gibi düzenleme iş akışı'nı açmak için: ![Content Moderator varsayılan iş akışı](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>Tasarımcı görünümü

Gördüğünüz **Tasarımcısı** iş akışı için sekmesinde. Tasarımcı görünümü, aşağıdaki adımları gösterir:

1. **Koşul** uyumluluğunun değerlendirilebilmesi iş akışı için. Bu durumda, iş akışı çağrıları Content Moderator'ın API ve denetimler olup görüntü `isAdult` çıkış eşittir `true`.
1. **Eylem** koşul karşılanıyorsa gerçekleştirilecek. Bu durumda, iş akışı bir gözden geçirme gözden geçirme Aracı'nda oluşturur `isAdult` çıktı `true`.

![Content Moderator varsayılan iş akışı - designer](images/default-workflow-designer.png)

### <a name="the-json-view"></a>JSON görünümü

Seçin **JSON** iş akışı JSON tanımını görmek için sekmesinde.

    {
        "Type": "Logic",
        If": {
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

### <a name="key-learning"></a>Anahtar öğrenme

Content Moderator akışlarında kolayca yapılandırılabilir ve esnek. Yerleşik Tasarımcı gereksinimlerinizi karşılamıyorsa, iş akışı tanımı yazmak **JSON** biçimi. Ardından ile JSON tanımı kullanması [iş akışı API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) oluşturup uygulamanızdan iş akışını yönetmek için.

## <a name="define-a-custom-workflow"></a>Özel bir iş akışında tanımlayın

Content Moderator'ın iş akışı özellikleri tanımlama ve özel iş akışlarını kullanarak izin verir. Kullanım [aracı iş akışları ile ilgili nasıl yapılır gözden](Review-Tool-User-Guide/Workflows.md) makalede özel iş akışı tanımlamak için. Bu iş akışı Content Moderator'ın OCR özelliğine bir örnek görüntüsünden metin ayıklamak için kullanır. Ardından İnceleme aracında bir inceleme oluşturur.

### <a name="the-sample-image"></a>Örnek görüntü

Kaydet [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) yerel sürücünüze. Bu görüntü, alıştırma için ihtiyacınız vardır.

### <a name="the-designer-view"></a>Tasarımcı görünümü

Seçin **Tasarımcısı** sekmesi ve [iş akışı oluşturma Öğreticisi](Review-Tool-User-Guide/Workflows.md) özel bir iş akışı tanımlamak için. Aşağıdaki resimde gösterilmektedir tasarımcının **koşul** görünümü. Adımların kalanını görmek için Öğreticisine bakın.

![Content Moderator - iş akışı durumu](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>JSON görünümü

Seçin **JSON** özel iş akışınızı aşağıdaki JSON tanımını görmek için sekmesinde. Bildirim nasıl **If-Then** ifadeleri JSON tanımında tanımlanan Tasarımcı görünümü kullanarak adımları karşılık gelir.

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="workflow-result"></a>İş akışı sonucu

İş akışı iş akışları ekranından test ettikten sonra aşağıdaki İnceleme oluşturulur. Gidin **görüntü** sekmesinde altında **gözden** incelemeniz görmek için.
Birincil koşul test çünkü iş akışını gözden metin varlığını pozitif oluşturuldu. Gözden geçirmeyi de vurgulanmış **`a`** resim incelemesi etiketi.

![Content Moderator - basit bir iş akışı çıkışı](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Gelişmiş iş akışı birleşimine sahip

### <a name="the-sample-image"></a>Örnek görüntü

Aynı [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) ve önceki bölümde kullanıldı.

Ancak, bu anda yaklaşık birincil koşulunuzu iki denetimleri'nın bir birleşimine değiştirin. Metin için denetlemenin yanı sıra, metnin herhangi küfür olup olmadığını denetleyin. Bu metin bulunursa, bir gözden geçirme iş akışı oluşturur **ve** küfür algıladığı.

### <a name="the-designer-view"></a>Tasarımcı görünümü

Değiştirilecek **koşul** için bir **birleşimi**, iş akışını değiştirin. Aşağıdaki görüntüde gördüğünüz Yeni Görünüm Tasarımcısı'nda gösterilmektedir.

![Content Moderator - değiştirilmiş iş akışı durumu](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>JSON görünümü

Seçin **JSON** değiştirilen özel iş akışınızın aşağıdaki JSON tanımını görmek için sekmesinde. Bildirim nasıl **If-Then** ifadeleri JSON tanımında iş akışına eklenen yeni adımları karşılık gelir.

    {
        "Type": "Logic",
        "If": {
        "Left": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Right": {
            "ConnectorName": "moderator",
            "OutputName": "text.HasProfanity",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition",
            "AlternateInput": "moderator.ocrText"
            },
        "Combine": "AND",
        "Type": "Combine"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

    
### <a name="workflow-result"></a>İş akışı sonucu

İş akışını yeniden test ettikten sonra hiçbir gözden geçirme oluşturduğunuz bulun. Herhangi bir gözden geçirme olmaması onaylamak için gidin **görüntü** sekmesinde altında **gözden**.
Ayıklanan küfürleri algılayın başarısız olduğundan iş akışını gözden oluşturmadınız.

![Content Moderator - değiştirilmiş iş akışı çıkışı](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>İş akışı API'si

[İş akışı işlemlerini](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) iş akışı özellikleri için programlama arabirimi sağlar. İş akışları oluşturun, iş akışı ayrıntılarını almak ve iş akışı API'sini kullanarak iş akışı tanımlarını güncelleştirin.

### <a name="get-all-workflow-details"></a>İş akışı ayrıntıları Al [All]

**İş akışı Get** işlemi aşağıdaki girişleri kabul eder:

- **Takım**: Ayarlarken oluşturduğunuz takım kimliği, [aracı panoya Sabitlenen](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: İş akışınızı adı. Kullanım `default` başlangıç.
- **Ocp-Apim-Subscription-Key**: Bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

İşlem başarılı olursa **yanıt durumu** olduğu `200 OK` ve **yanıt içeriği** kutusunda iş akışı tanımı JSON biçiminde görüntülenir.
Daha fazla bilgi edinmek için [iş akışı API Konsolu hızlı](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>Oluşturma veya iş akışını güncelleştirme

Oluşturma ve güncelleştirme işlemi, API'den iş akışı oluşturulmasını sağlar.

**İş akışı oluşturma veya güncelleştirme** işlemi aşağıdaki girişleri kabul eder:

- **Takım**: Ayarlarken oluşturduğunuz takım kimliği, [aracı panoya Sabitlenen](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: İş akışınızı adı. Kullanım `default` başlangıç.
- **Ocp-Apim-Subscription-Key**: Bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

İşlem başarılı olursa **yanıt durumu** olduğu `200 OK` ve **yanıt içeriği** kutusunda görüntüler `true`. Daha fazla bilgi edinmek için [test sürücü `Create` işlemi](try-review-api-job.md).

## <a name="next-steps"></a>Sonraki adımlar

Özel iş akışlarının nasıl oluşturulacağını öğrenmek için kullanıma [Aracı'nın iş akışı öğreticisini inceleyin](Review-Tool-User-Guide/Workflows.md). 

Sürücü test [iş akışı API Konsolu](try-review-api-job.md) ve REST API kod örnekleri kullanın. 

Son olarak, özel kullanarak iş akışlarınızı kullanın **iş** gösterildiği gibi işlemleri [iş API Konsolu](try-review-api-job.md) ve [işleri .NET Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md).
