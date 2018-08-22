---
title: Kullanım özel görüntü işleme hizmeti REST API - Azure Bilişsel hizmetler | Microsoft Docs
description: Oluşturma, eğitme, test ve bir özel görüntü işleme modelini dışarı aktarma için REST API'sini kullanın.
services: cognitive-services
author: blackmist
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/07/2018
ms.author: larryfr
ms.openlocfilehash: 44fa4d45c33f3064c089724ee761a70d0a8421ab
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250274"
---
# <a name="tutorial-use-the-custom-vision-rest-api"></a>Öğretici: REST API özel görüntü işleme kullanın

Özel işleme REST API'si oluşturma, eğitme, test ve bir modelini dışarı aktarma için kullanmayı öğrenin.

Bu belgedeki bilgiler, özel görüntü işleme hizmeti eğitim için REST API ile çalışmak için bir REST istemcisi kullanmayı gösterir. API kullanarak nasıl örnekler `curl` yardımcı programı'ndan bir bash ortamı ve `Invoke-WebRequest` Windows powershell'den.

> [!div class="checklist"]
> * Anahtarları alma
> * Proje oluşturma
> * Etiketleri oluşturma
> * Görüntü ekleme
> * Eğitme ve modeli test etme
> * Modelini dışarı aktarma

## <a name="prerequisites"></a>Önkoşullar

* Temel bilgisi ile temsili durum aktarımı (REST). Bu belge HTTP fiilleri, JSON veya REST ile yaygın olarak kullanılan başka şeyler gibi şeyleri ayrıntılarına geçmez.

* Ya da bir bash (Uluç yeniden kabuğu) ile [curl](https://curl.haxx.se) yardımcı programı veya Windows PowerShell 3.0 (veya üstü).

* Bir özel görüntü işleme hesabı. Daha fazla bilgi için [sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) belge.

## <a name="get-keys"></a>Anahtarları alma

REST API kullanırken, bir anahtar kullanarak kimlik doğrulaması gerekir. Yönetim veya eğitim işlemleri gerçekleştirirken kullandığınız __eğitim anahtar__. Model tahminde bulunmak amacıyla kullanırken, kullandığınız __tahmin anahtar__.

İsteği yaparken, anahtarı bir istek üst bilgisi gönderilir.

Hesabınız için anahtarları almaya ziyaret [Custom Vision web sayfası](https://customvision.ai) seçip __dişli simgesini__ sağ üst köşedeki. İçinde __hesapları__ bölümünde, değerleri kopyalayın __eğitim anahtarı__ ve __tahmin anahtar__ alanları.

![UI anahtarları görüntüsü](./media/rest-api-tutorial/training-prediction-keys.png)

> [!IMPORTANT]
> Her isteğin kimliğini doğrulamak için kullanılan anahtarları olduğundan, bu belgedeki örneklerde anahtar değerlerinin ortam değişkenleri içerdiği varsayılır. Bu belgede diğer kod parçacıkları kullanmadan önce ortam değişkenlerine anahtarları depolamak için aşağıdaki komutları kullanın:
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

Aşağıdaki örnekler adlı yeni bir proje oluşturur `myproject` Custom Vision service Örneğinizdeki. Bu hizmet için varsayılan olarak `General` etki alanı:

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

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
> `id` Girdidir yanıt yeni projenin kimliği. Bu, diğer örneklerde bu belgenin ilerleyen bölümlerinde kullanılır.

Bu isteği hakkında daha fazla bilgi için bkz. [CreateProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8290).

### <a name="specific-domains"></a>Özel etki alanları

Belirli bir etki alanı için bir proje oluşturmak için sağlayabilirsiniz __etki alanı kimliği__ isteğe bağlı bir parametre olarak. Aşağıdaki örnekler, mevcut etki alanlarının bir listesini almak gösterilmektedir:

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

```json
[
    {
        "id": "ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
        "name": "General",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    {
        "id": "c151d5b5-dd07-472a-acc8-15d29dea8518",
        "name": "Food",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    {
        "id": "ca455789-012d-4b50-9fec-5bb63841c793",
        "name": "Landmarks",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    ...
]
```

Bu isteği hakkında daha fazla bilgi için bkz. [GetDomains](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a827d).

Aşağıdaki örnek, kullanan yeni bir proje oluşturmadan gösterir __yer işareti__ etki alanı:

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

## <a name="create-tags"></a>Etiketleri oluşturma

Etiket görüntüleri için bir etiket kimliği kullanmalıdır Aşağıdaki örnekte adlı yeni bir etiket oluşturmak nasıl gösterir `cat` ve bir etiket kimliğini Al Değiştirin `{projectId}` projenizi kimliği. Kullanım `name=` parametresi etiketin adını belirtmek için:

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

İstek yanıtı için JSON belgesini benzer: 

```json
{"id":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43","name":"cat","description":null,"imageCount":0}
```

Kaydet `id` görüntüleri etiketlenirken kullanıldıkça, değer.

Bu isteği hakkında daha fazla bilgi için bkz. [CreateTag](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829d).

## <a name="add-images"></a>Görüntü ekleme

Aşağıdaki örnekler, URL'den dosya eklemeyi gösterir. Değiştirin `{projectId}` projenizi kimliği. Değiştirin `{tagId}` görüntüsü için etiket kimliği:

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

```json
{
    "isBatchSuccessful": true,
    "images": [
        {
            "sourceUrl": "http://myimages/cat.jpg",
            "status": "OK",
            "image": {
                "id": "081adaee-a76b-4d94-a70e-e4fd0935a28f",
                "created": "2018-08-13T13:24:22.0815638",
                "width": 640,
                "height": 480,
                "imageUri": "https://linktoimage",
                "thumbnailUri": "https://linktothumbnail",
                "tags": [
                    {
                        "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
                        "tagName": null,
                        "created": "2018-08-13T13:24:22.104936"
                    }
                ],
                "regions": [
                    {
                        "regionId": "40f206a1-3f8a-4de7-a6c3-c7b4643117df",
                        "tagName": null,
                        "created": "2018-08-13T13:24:22.104936",
                        "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
                        "left": 119,
                        "top": 94,
                        "width": 240,
                        "height": 140
                    }
                ]
            }
        }
    ]
}
```

Bu isteği hakkında daha fazla bilgi için bkz. [CreateImagesFromUrls](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8287).

## <a name="train-the-model"></a>Modeli eğitme

Aşağıdaki örnekler modeli eğitmek nasıl ekleyebileceğiniz gösterilmektedir. Değiştirin `{projectId}` projenizi kimliği:

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

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

Kaydet `id` değerini test etmek ve modeli dışa aktarmak için kullanılır.

Daha fazla bilgi için [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8294).

## <a name="test-the-model"></a>Modeli test

Aşağıdaki örnekler, modelin bir test gerçekleştirmek nasıl ekleyebileceğiniz gösterilmektedir. Değiştirin `{projectId}` projenizi kimliği. Değiştirin `{iterationId}` modeli eğitmek öğesinden döndürülen Kimliğine sahip. Değiştirin `https://linktotestimage` yoluyla test görüntüsü.

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

```json
{
    "id": "369b010b-2a92-4f48-a918-4c1a0af91888",
    "project": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "iteration": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "created": "2018-08-16T17:39:20.7944508Z",
    "predictions": [
        {
            "probability": 0.8390652,
            "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "tagName": "cat"
        }
    ]
}
```

Daha fazla bilgi için [QuickTestImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a828d).

## <a name="export-the-model"></a>Modelini dışarı aktarma

Bir model verme iki adımlı bir işlemdir. İlk model biçimini belirtin ve ardından istek URL'si dışarı aktarılan modeli için gerekir.

### <a name="request-a-model-export"></a>Modeli dışarı aktarma isteği

Aşağıdaki örnekler nasıl dışarı aktarılacağını gösteren bir `coreml` modeli. Değiştirin `{projectId}` projenizi kimliği. Değiştirin `{iterationId}` modeli eğitmek öğesinden döndürülen Kimliğine sahip.

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

```json
{
    "platform": "CoreML",
    "status": "Exporting",
    "downloadUri": null,
    "flavor": null
}
```

Daha fazla bilgi için [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829b).

### <a name="download-the-exported-model"></a>Dışarı aktarılan modeli indirin.

Aşağıdaki örnekler, dışarı aktarılan modeli URL'sini almak nasıl ekleyebileceğiniz gösterilmektedir. Değiştirin `{projectId}` projenizi kimliği. Değiştirin `{iterationId}` modeli eğitmek öğesinden döndürülen Kimliğine sahip.

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

İstek yanıtı için aşağıdaki JSON belgesini benzer:

```json
[
    {
        "platform": "CoreML",
        "status": "Done",
        "downloadUri": "https://linktoexportedmodel",
        "flavor": null
    }
]
```

Daha fazla bilgi için [GetExports](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829a).