---
title: "Öğretici: Oluşturmak, eğitmek ve özel işleme REST API'si ile bir modelini dışarı aktarma"
titlesuffix: Azure Cognitive Services
description: Özel görüntü işleme modeli oluşturmak, eğitmek, test etmek ve dışarı aktarmak için REST API kullanın.
services: cognitive-services
author: blackmist
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: larryfr
ms.openlocfilehash: 54b5f7bb16803adf91a0a8ea60cfa68d1e322d07
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351108"
---
# <a name="tutorial-create-train-and-export-a-model-with-rest"></a>Öğretici: Oluşturmak, eğitmek ve REST ile bir modelini dışarı aktarma

Bu belgedeki bilgiler, Özel Görüntü İşleme hizmetini eğitmek için REST API ile çalışmak üzere REST istemcisinin nasıl kullanılacağını göstermektedir. Örneklerde, Windows PowerShell’deki `curl`ve bir bash ortamındaki yardımcı program `Invoke-WebRequest` kullanılarak API’nin nasıl kullanılacağı gösterilmektedir.

> [!div class="checklist"]
> * Anahtar alma
> * Proje oluşturma
> * Etiket oluşturma
> * Görüntü ekleme
> * Modeli eğitme ve test etme
> * Modeli dışarı aktarma

## <a name="prerequisites"></a>Önkoşullar

* Temsili Durum Transferi (REST) konusunda temel bilgi. Bu belgede, HTTP fiilleri, JSON gibi şeylerin veya REST ile yaygın olarak kullanılan diğer şeylerin ayrıntısına inilmemektedir.
* [curl](https://curl.haxx.se) yardımcı programı veya Windows PowerShell 3.0 (ya da üzeri) ile bir bash (Bourne Again Shell).
* Bir Özel Görüntü İşleme hesabı. Daha fazla bilgi için [Sınıflandırıcı derleme](getting-started-build-a-classifier.md) belgesine bakın.

## <a name="get-keys"></a>Anahtar alma

REST API kullanırken, bir anahtar kullanarak kimlik doğrulaması yapmanız gerekir. Yönetim veya eğitim işlemleri gerçekleştirirken __eğitim anahtarını__ kullanırsınız. Tahminde bulunmak için model kullanırken __tahmin anahtarını__ kullanırsınız.

İstekte bulunurken anahtar bir istek üst bilgisi olarak gönderilir.

Hesabınızın anahtarlarını almak için [Özel Görüntü İşleme web sayfasını](https://customvision.ai) ziyaret edin ve sağ üst kısımdaki __dişli simgesini__ seçin. __Hesaplar__ bölümünde, __Eğitim Anahtarı__ ve __Tahmin Anahtarı__ alanlarından değerleri kopyalayın.

![Anahtarlar kullanıcı arabiriminin görüntüsü](./media/rest-api-tutorial/training-prediction-keys.png)

> [!IMPORTANT]
> Her isteğin kimliğini doğrulamak için anahtarlar kullanıldığından, bu belgedeki örneklerde, ortam değişkeninde anahtar değerlerinin yer aldığı varsayılır. Bu belgede diğer kod parçacıklarını kullanmadan önce ortam değişkenlerinde anahtarları depolamak için aşağıdaki komutları kullanın:
>
> ```bash
> read -p "Enter the training key: " TRAININGKEY
> read -p "Enter the prediction key: " PREDICTIONKEY
> ```
>
> ```powershell
> $trainingKey = Read-Host 'Enter the training key'
> $predictionKey = Read-Host 'Enter the prediction key'
> ```

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Aşağıdaki örnekler, Özel Görüntü İşleme hizmet örneğinizde `myproject` adlı yeni bir proje oluşturur. Bu hizmet varsayılan olarak `General` etki alanının değerini alır:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
{
  "id":"45d1b19b-69b6-4a22-8e7e-d1ca37504686",
  "name":"myproject",
  "description":"",
  "settings":{
    "domainId":"ee85a72c-405e-4adc-bb47-ffa8ca0c9f31",
    "useNegativeSet":true,
    "classificationType":"Multilabel",
    "detectionParameters":null
  },
  "created":"2018-08-10T17:39:02.5633333",
  "lastModified":"2018-08-10T17:39:02.5633333",
  "thumbnailUri":null
}
```

> [!TIP]
> Yanıttaki `id` girdisi, yeni projenin kimliğidir. Bu belgenin ilerleyen kısmındaki diğer örneklerde bu kullanılmaktadır.

Bu istek hakkında daha fazla bilgi için bkz. [CreateProject](https://go.microsoft.com/fwlink/?linkid=865446).

### <a name="specific-domains"></a>Belirli etki alanları

Belirli bir etki alanına yönelik bir proje oluşturmak için, isteğe bağlı parametre olarak __etki alanı kimliğini__ sağlayabilirsiniz. Aşağıdaki örneklerde, kullanılabilir etki alanlarının bir listesinin nasıl alınacağı gösterilmektedir:

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
[  
  {  
    "id":"ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
    "name":"General",
    "type":"Classification",
    "exportable":false,
    "enabled":true
  },
  {  
    "id":"c151d5b5-dd07-472a-acc8-15d29dea8518",
    "name":"Food",
    "type":"Classification",
    "exportable":false,
    "enabled":true
  },
  {  
    "id":"ca455789-012d-4b50-9fec-5bb63841c793",
    "name":"Landmarks",
    "type":"Classification",
    "exportable":false,
    "enabled":true
  },
  ...
]
```

Bu istek hakkında daha fazla bilgi için bkz. [GetDomains](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a827d).

Aşağıdaki örnekte, __Yer İşaretleri__ etki alanını kullanan yeni bir proje oluşturulması gösterilmektedir:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

## <a name="create-tags"></a>Etiket oluşturma

Görüntüleri etiketlemek için bir etiket kimliği kullanmanız gerekir. Aşağıdaki örnekte, `cat` adlı yeni bir etiketin nasıl oluşturulacağı ve bir etiket kimliğinin nasıl alınacağı gösterilmektedir. `{projectId}` değerini, projenizin kimliği ile değiştirin. Etiketin adını belirtmek için `name=` parametresini kullanın:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

İstek yanıtı, JSON belgesine benzer: 

```json
{"id":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43","name":"cat","description":null,"imageCount":0}
```

`id` değerini, görüntüleri etiketlerken kullanıldığı haliyle kaydedin.

Bu istek hakkında daha fazla bilgi için bkz. [CreateTag](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829d).

## <a name="add-images"></a>Görüntü ekleme

Aşağıdaki örneklerde, URL’den dosya ekleme işlemi gösterilmektedir. `{projectId}` değerini, projenizin kimliği ile değiştirin. `{tagId}` değerini, görüntü için etiketin kimliği ile değiştirin:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
{  
  "isBatchSuccessful":true,
  "images":[  
    {  
      "sourceUrl":"http://myimages/cat.jpg",
      "status":"OK",
      "image":{  
        "id":"081adaee-a76b-4d94-a70e-e4fd0935a28f",
        "created":"2018-08-13T13:24:22.0815638",
        "width":640,
        "height":480,
        "imageUri":"https://linktoimage",
        "thumbnailUri":"https://linktothumbnail",
        "tags":[  
          {  
            "tagId":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "tagName":null,
            "created":"2018-08-13T13:24:22.104936"
          }
        ],
        "regions":[  
          {  
            "regionId":"40f206a1-3f8a-4de7-a6c3-c7b4643117df",
            "tagName":null,
            "created":"2018-08-13T13:24:22.104936",
            "tagId":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "left":119,
            "top":94,
            "width":240,
            "height":140
          }
        ]
      }
    }
  ]
}
```

Bu istek hakkında daha fazla bilgi için bkz. [CreateImagesFromUrls](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8287).

## <a name="train-the-model"></a>Modeli eğitme

Aşağıdaki örneklerde, modelin nasıl eğitileceği gösterilmektedir. `{projectId}` değerini, projenizin kimliği ile değiştirin:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } 
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
{
    "id": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "name": "Iteration 1",
    "isDefault": false,
    "status": "Training",
    "created": "2018-08-10T17:39:02.5766667",
    "lastModified": "2018-08-16T17:15:07.5250661",
    "projectId": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "exportable": false,
    "domainId": null
}
```

Modeli test etmek ve dışarı aktarmak için kullanılan `id` değerini kaydedin.

Daha fazla bilgi için bkz. [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8294).

## <a name="test-the-model"></a>Modeli test etme

Aşağıdaki örneklerde, modelin bir testinin nasıl gerçekleştirileceği gösterilmektedir. `{projectId}` değerini, projenizin kimliği ile değiştirin. `{iterationId}` değerini, modelin eğitiminden döndürülen kimlik ile değiştirin. `https://linktotestimage` değerini, test görüntüsünün yolu ile değiştirin.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"url":"https://linktotestimage"}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"url":"https://linktotestimage"}'
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
{  
  "id":"369b010b-2a92-4f48-a918-4c1a0af91888",
  "project":"45d1b19b-69b8-4b22-8e7e-d1ca37504686",
  "iteration":"23de09d6-42a1-413e-839e-8db6ee6d3496",
  "created":"2018-08-16T17:39:20.7944508Z",
  "predictions":[  
    {  
      "probability":0.8390652,
      "tagId":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
      "tagName":"cat"
    }
  ]
}
```

Daha fazla bilgi için bkz. [QuickTestImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a828d).

## <a name="export-the-model"></a>Modeli dışarı aktarma

Bir modeli dışarı aktarma, iki adımlı bir işlemdir. İlk önce model biçimini belirtmeniz ve sonra dışarı aktarılan modelin URL’sini istemeniz gerekir.

### <a name="request-a-model-export"></a>Model dışarı aktarımı isteme

Aşağıdaki örneklerde, bir `coreml` modelinin nasıl dışarı aktarılacağı gösterilmektedir. `{projectId}` değerini, projenizin kimliği ile değiştirin. `{iterationId}` değerini, modelin eğitiminden döndürülen kimlik ile değiştirin.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
{  
  "platform":"CoreML",
  "status":"Exporting",
  "downloadUri":null,
  "flavor":null
}
```

Daha fazla bilgi için bkz. [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829b).

### <a name="download-the-exported-model"></a>Dışarı aktarılan modeli indirme

Aşağıdaki örneklerde, dışarı aktarılan modelin URL’sinin nasıl dışarı aktarılacağı gösterilmektedir. `{projectId}` değerini, projenizin kimliği ile değiştirin. `{iterationId}` değerini, modelin eğitiminden döndürülen kimlik ile değiştirin.

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

İstek yanıtı, aşağıdaki JSON belgesine benzer:

```json
[  
  {  
    "platform":"CoreML",
    "status":"Done",
    "downloadUri":"https://linktoexportedmodel",
    "flavor":null
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [GetExports](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829a).
