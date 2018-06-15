---
title: Azure zaman uyumsuz işlemleri | Microsoft Docs
description: Azure içinde zaman uyumsuz işlemleri izlemek açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: f62212f0488e4d1be49b419615b3a16b80033fd9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358719"
---
# <a name="track-asynchronous-azure-operations"></a>Zaman uyumsuz Azure işlemleri izleme
Bazı Azure REST işlemlerini zaman uyumsuz olarak çalışır, çünkü işlem hızlı bir şekilde tamamlanamıyor. Bu konu, yanıtta döndürülen değerleri arasında zaman uyumsuz işlemleri durumunu izlemek açıklar.  

## <a name="status-codes-for-asynchronous-operations"></a>Zaman uyumsuz işlemleri için durum kodları
Zaman uyumsuz bir işlem, başlangıçta bir HTTP durum kodu ya da döndürür:

* 201 (oluşturuldu)
* 202 (kabul edilen) 

İşlem başarıyla tamamlandığında, ya da döndürür:

* 200 (TAMAM)
* 204 (içerik yok) 

Başvurmak [REST API belgeleri](/rest/api/) yürütme işlemi için yanıtlar görmek için. 

## <a name="monitor-status-of-operation"></a>İşlemin durumunu izleyin
Zaman uyumsuz REST işlemlerini işlemin durumunu belirlemek için kullanılan üstbilgi değerleri döndürür. İncelemek için büyük olasılıkla üç üstbilgi değerleri şunlardır:

* `Azure-AsyncOperation` -İşlemi devam eden durumunu denetlemek için URL. İşleminizi bu değer döndürürse, her zaman bu (konum yerine) işlemin durumunu izlemek için kullanın.
* `Location` -Ne zaman bir işlemin tamamlanmasını belirlemek için URL. Yalnızca Azure AsyncOperation alınmadı olduğunda bu değeri kullanın.
* `Retry-After` -Zaman uyumsuz işlemin durumunu denetlemeden önce beklenecek saniye sayısı.

Bununla birlikte, her zaman uyumsuz işlemi bu tüm değerleri döndürür. Örneğin, bir işlem için Azure AsyncOperation üstbilgisi değeri ve başka bir işlem için konum üstbilgisi değeri değerlendirmek gerekebilir. 

Bir istek için herhangi bir üstbilgi değerini almak gibi üstbilgi değerlerini alır. Örneğin, C# ' ta üstbilgi değeri aldığınız bir `HttpWebResponse` adlı nesne `response` aşağıdaki kod ile:

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure AsyncOperation istek ve yanıt

Zaman uyumsuz işlemin durumunu almak için Azure AsyncOperation üstbilgi değeri URL'SİNDE bir GET isteği gönderin.

Bu işlem gelen yanıt gövdesini işlemi hakkında bilgi içerir. Aşağıdaki örnek işleminden döndürülen olası değerleri gösterir:

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

Yalnızca `status` tüm yanıtlar için döndürülür. Başarısız veya iptal edildi durum olduğunda hata nesnesi döndürülür. Diğer tüm değerler isteğe bağlıdır; Bu nedenle, aldığınız yanıt örnek farklı görünebilir.

## <a name="provisioningstate-values"></a>provisioningState değerleri

Bir kaynak oluşturma, güncelleştirme veya silme (PUT, PATCH, Sil) işlemleri genellikle dönmek bir `provisioningState` değeri. Bir işlem tamamlandığında, aşağıdaki üç değerden birini verilir: 

* Başarılı oldu
* Başarısız
* İptal edildi

Diğer tüm değerler işlemi hala çalışıyor gösterir. Kaynak sağlayıcısı durumunu gösteren özelleştirilmiş bir değeri geri dönebilirsiniz. Örneğin, alabileceğiniz **kabul edilen** alınan ve çalışan istek olduğunda.

## <a name="example-requests-and-responses"></a>Örnek istekleri ve yanıtları

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Sanal makine (Azure AsyncOperation ile 202) Başlat
Bu örnek durumunu belirlemek nasıl gösterir **Başlat** sanal makineler için işlem. İlk istek aşağıdaki biçimdedir:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

Durum kodu 202 döndürür. Üstbilgi değerleri arasında görürsünüz:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Bu URL başka bir isteği gönderilirken zaman uyumsuz işlemin durumunu denetlemek için.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Yanıt gövdesi işlemin durumunu içerir:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Kaynakları (201 Azure AsyncOperation ile) dağıtma

Bu örnek durumunu belirlemek nasıl gösterir **dağıtımları** kaynakları Azure'a dağıtma işlemi. İlk istek aşağıdaki biçimdedir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

201 durum kodunu döndürür. Yanıtın gövdesini içerir:

```json
"provisioningState":"Accepted",
```

Üstbilgi değerleri arasında görürsünüz:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Bu URL başka bir isteği gönderilirken zaman uyumsuz işlemin durumunu denetlemek için.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Yanıt gövdesi işlemin durumunu içerir:

```json
{"status":"Running"}
```

Dağıtım tamamlandığında, yanıtın içerir:

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Depolama hesabı (202 konumla ve yeniden deneme sonrasında) oluşturma

Bu örnek durumunu belirlemek nasıl gösterir **oluşturma** işlem depolama hesapları için. İlk istek aşağıdaki biçimdedir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

Ve istek gövdesi depolama hesabı özellikleri içerir:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

Durum kodu 202 döndürür. Üstbilgi değerleri arasında aşağıdaki iki değerden görürsünüz:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Bekleyen sayısı için belirtilen yeniden deneme sonrasında onay zaman uyumsuz işlemin durumunu bu URL başka bir istek göndererek saniye sonra.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

İstek hala devam ediyorsa, bir durum kodu 202 alırsınız. İstek tamamladıysa, durum kodu 200'ü alırsınız ve yanıtın gövdesini oluşturulduktan sonra depolama hesabının özelliklerini içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Her REST işlemini ilgili belgeler için bkz: [REST API belgeleri](/rest/api/).
* Resource Manager REST API'si aracılığıyla şablonları dağıtma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları ve Resource Manager REST API kaynaklarla dağıtmak](resource-group-template-deploy-rest.md).