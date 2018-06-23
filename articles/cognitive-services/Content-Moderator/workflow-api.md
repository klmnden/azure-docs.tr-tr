---
title: Azure içerik aracı - yönetimini iş akışları | Microsoft Docs
description: İş akışları ile içerik yönetimini kullanın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/04/2018
ms.author: sajagtap
ms.openlocfilehash: 079fcd119f1536f9e76ca57fccc76538b3c3ed78
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351863"
---
# <a name="moderation-workflows"></a>İş akışları denetleme

İçerik denetleyici araçları ve iş akışlarını yönetmek için API içerir. İş akışları ile kullandığınız [gözden API iş işlemlerini](review-api.md) , içerik ilkeleri ve eşikleri bağlı İnsan-içinde--döngü gözden geçirme oluşturmayı otomatikleştirmek için.

Gözden geçirme API İnsan gözetim içerik yönetimini işleminize eklemek için aşağıdaki yöntemleri sunar:

1. **İş** makine destekli denetleme ve İnsan başlangıç için işlem oluşturma bir adım olarak gözden geçirin.
1. **Gözden** İnsan işlemlerinde denetleme adım dışında oluşturma gözden geçirin.
1. **İş akışı** için eşikler ile tarama otomatikleştiren iş akışlarını yönetme için işlem oluşturma gözden geçirin.

Bu makalede yer almaktadır **iş akışı** işlemleri. Okuma [işler ve incelemeleri](review-api.md) içeriği denetleme hakkında bilgi edinmek için genel bakış işler ve gözden geçirir.

Kullanıma **varsayılan** içerik denetleyici anlama akışlarında kullanmaya başlamak için en iyi yolu iş akışıdır.

## <a name="your-first-workflow"></a>İlk iş akışınızın

İlk iş akışınızın ile ile birlikte gelen gelir, [aracı takım gözden](https://contentmoderator.cognitive.microsoft.com/). Siz bunu zaten yapmadıysanız kaydolun.

Gidin [aracın iş akışlarını gözden](Review-Tool-User-Guide/Workflows.md) ekran Ayarlar sekmesi altında. Gördüğünüz bir **varsayılan** aşağıdaki görüntüde gösterildiği gibi iş akışı:

![İçerik aracı iş akışları](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>Varsayılan iş akışı açın

Kullanım **Düzenle** aşağıdaki görüntüde gösterildiği gibi sayfa düzenleme iş akışı açmak için seçeneği: ![içerik denetleyici varsayılan iş akışı](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>Tasarımcı görünümü

Gördüğünüz **Tasarımcısı** sekmesi iş akışı için. Tasarımcı görünüm aşağıdaki adımları gösterir:

1. **Koşulu** değerlendirilecek iş akışı için. Bu durumda, iş akışı çağrıları içerik denetleyici ait API ve denetimleri olup görüntü `isAdult` çıkış eşittir `true`.
1. **Eylem** koşul karşılandığında gerçekleştirilecek. Bu durumda, iş akışını bir gözden geçirme gözden geçirme aracında oluşturur `isAdult` çıkış `true`.

![İçerik denetleyici varsayılan iş akışı - Tasarımcısı](images/default-workflow-designer.png)

### <a name="the-json-view"></a>JSON görünümü

Seçin **JSON** iş akışı JSON tanımını görmek için sekmesi.

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

İçerik denetleyici akışlarında yapılandırmak kolay ve esnek. Yerleşik Tasarımcısı gereksinimlerinizi karşılamıyorsa, iş akışı tanımı yazmak **JSON** biçimi. JSON tanımıyla kullanmak [iş akışı API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) oluşturmak ve iş akışı uygulamanızdan yönetmek için.

## <a name="define-a-custom-workflow"></a>Özel bir iş akışında tanımlayın

İçerik denetleyicinin iş akışı özellikleri tanımlama ve özel iş akışlarını kullanarak izin verir. Kullanım [aracı iş akışları yapılır gözden](Review-Tool-User-Guide/Workflows.md) makale özel bir iş akışında tanımlayın. Bu iş akışı içerik denetleyicinin OCR özelliğine bir örnek görüntüsünden metin ayıklamak için kullanır. Daha sonra bir gözden geçirme gözden geçirme aracında oluşturur.

### <a name="the-sample-image"></a>Örnek görüntü

Kaydet [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) yerel diskinize. Bu görüntü, alıştırma için gerekir.

### <a name="the-designer-view"></a>Tasarımcı görünümü

Seçin **Tasarımcısı** sekmesi ve [iş akışı oluşturma öğretici](Review-Tool-User-Guide/Workflows.md) özel bir iş akışı tanımlamak için. Aşağıdaki resimde tasarımcının gösterilmiştir **koşulu** görünümü. Kalan adımları görmek için Öğreticisine bakın.

![İçerik aracı - iş akışı durumu](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>JSON görünümü

Seçin **JSON** , özel iş akışı aşağıdaki JSON tanımını görmek için sekmesi. Bildirim nasıl **If Then** deyimleri JSON tanımında tanımlanan Tasarımcı görünümünü kullanma adımları karşılık gelir.

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

İş akışı iş akışları ekranından test ettikten sonra aşağıdaki inceleyin oluşturulur. Gidin **görüntü** altında sekmesinde **gözden** incelemeniz görmek için.
Birincil koşulu test için iş akışı gözden metin varlığını pozitif oluşturuldu. Ayrıca vurgulanan gözden geçirme **`a`** etiketinde görüntü gözden geçirin.

![İçerik aracı - basit iş akışı çıktısı](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Birleşimle Gelişmiş iş akışı

### <a name="the-sample-image"></a>Örnek görüntü

Aynı [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) önceki bölümde kullanıldı.

Ancak, birincil koşulunuz iki denetimleri bileşimi halinde bu sefer orada değiştirin. Metin için denetlemin yanı sıra, metni herhangi uygunsuz metin olup olmadığını denetleyin. Metin bulursa bir gözden geçirme iş akışını oluşturur **ve** uygunsuz metin algıladığı.

### <a name="the-designer-view"></a>Tasarımcı görünümü

Değiştirmek için **koşulu** için bir **birleşimi**, iş akışını değiştirin. Aşağıdaki resimde gördüğünüz Yeni Görünüm Tasarımcısı'nda gösterir.

![İçerik aracı - değiştirilmiş iş akışı durumu](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>JSON görünümü

Seçin **JSON** değiştirilen özel iş akışını aşağıdaki JSON tanımını görmek için sekmesi. Bildirim nasıl **If Then** deyimleri JSON tanımında iş akışına eklenen yeni adımları karşılık gelir.

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

İş akışını yeniden test ettikten sonra hiçbir gözden geçirme oluşturduğunuz bulun. Tüm gözden geçirme yokluğu onaylamak için gidin **görüntü** altında sekmesinde **gözden**.
Ayıklanan metinde uygunsuz metin algılamak başarısız olduğundan iş akışı gözden oluşturmadı.

![İçerik aracı - değiştirilmiş iş akışı çıktısı](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>İş akışı API

[İş akışı işlemlerini](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) iş akışı özelliklerine programlama arabirimi sağlar. İş akışları oluşturmak, iş akışı ayrıntıları almak ve iş akışı API kullanarak iş akışı tanımlarını güncelleştirin.

### <a name="get-all-workflow-details"></a>İş akışı ayrıntıları Al [All]

**İş akışı Get** işlemi aşağıdaki girişleri kabul eder:

- **Takım**: ayarlarken oluşturduğunuz takım kimliği, [aracı hesabı gözden](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: iş akışınızı adı. Kullanım `default` başından itibaren.
- **Ocp Apim abonelik anahtar**: bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz: [genel bakış](overview.md).

İşlem başarılı olursa, **yanıt durumu** olan `200 OK` ve **yanıt içeriği** kutusu iş akışı tanımı JSON biçiminde görüntüler.
Daha fazla bilgi edinmek için okuma [iş akışı API konsol quickstart](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>İş akışı güncelle

Oluşturma ve güncelleştirme işlemi API'SİNDEN iş akışı oluşturma sağlar.

**İş akışı oluşturma veya güncelleştirme** işlemi aşağıdaki girişleri kabul eder:

- **Takım**: ayarlarken oluşturduğunuz takım kimliği, [aracı hesabı gözden](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: iş akışınızı adı. Kullanım `default` başından itibaren.
- **Ocp Apim abonelik anahtar**: bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz: [genel bakış](overview.md).

İşlem başarılı olursa, **yanıt durumu** olan `200 OK` ve **yanıt içeriği** kutusunda görüntüler `true`. Daha fazla bilgi edinmek için [test sürücü `Create` işlemi](try-review-api-job.md).

## <a name="next-steps"></a>Sonraki adımlar

Özel iş akışları oluşturma konusunda bilgi almak için kullanıma [Aracı'nın iş akışı öğreticisini inceleyin](Review-Tool-User-Guide/Workflows.md). 

Test sürücü [iş akışı API konsol](try-review-api-job.md) ve REST API kod örnekleri kullanır. 

Son olarak, özel iş akışlarıyla kullanmak **iş** işlemleri içinde shon olarak [iş API konsol](try-review-api-job.md) ve [işleri .NET Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md).
