---
title: include dosyası
description: include dosyası
services: virtual-machines
author: changov
ms.service: virtual-machines
ms.topic: include
ms.date: 09/10/2018
ms.author: vashan, rajraj, changov
ms.custom: include file
ms.openlocfilehash: 5a9e0c86273895ccb26fab2a24239ce7aefd7eac
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724219"
---
Azure işlem istekleri, abonelik ve hizmetin genel performansını ile yardımcı olmak için bölge başına temelinde kısıtlanmış. Tüm çağrılar için Azure işlem kaynak sağlayıcısı (Microsoft.Compute ad alanı kaynaklarınıza yöneten CRP) izin verilen en fazla API istek hızı aşmamak emin oluruz. Bu belge, azaltma sorunları gidermeye ilişkin ayrıntıları azaltma API'sini açıklayan ve aşarak önlemek için en iyi yöntemler.  

## <a name="throttling-by-azure-resource-manager-vs-resource-providers"></a>Azure Resource Manager vs kaynak sağlayıcıları tarafından azaltma  

Ön kapısı Azure, Azure Resource Manager kimlik doğrulama ve ilk sırada doğrulama ve tüm gelen API isteklerinin azaltma yapar. Azure Resource Manager çağrı hız sınırları ve ilgili tanılama yanıt HTTP üstbilgileri açıklanmıştır [burada](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-request-limits).
 
Bir Azure API istemcisini bir kısıtlama hatası girdiğinde, HTTP 429 çok fazla istek durumudur. İstek azaltma Azure Resource Manager veya CRP gibi temel bir kaynak sağlayıcısı tarafından yapıldığını anlamak için incelemek `x-ms-ratelimit-remaining-subscription-reads` GET istekleri için ve `x-ms-ratelimit-remaining-subscription-writes` olmayan GET istekleri için yanıt üstbilgileri. Kalan çağrısı sayısı 0 yaklaşıyorsa, Azure Resource Manager tarafından tanımlanan aboneliğin genel çağrı sınırına ulaşıldı. Etkinlikler tüm abonelik istemcileri tarafından birlikte sayılır. Aksi takdirde, azaltma hedef kaynak sağlayıcısından gelen (bir ele `/providers/<RP>` segment istek URL'si). 

## <a name="call-rate-informational-response-headers"></a>Çağrı hızı bilgilendirici bir yanıt üstbilgileri 

| Üst bilgi                            | Değeri biçimi                           | Örnek                               | Açıklama                                                                                                                                                                                               |
|-----------------------------------|----------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-ms-ratelimit-kalan-kaynak |```<source RP>/<policy or bucket>;<count>```| Microsoft.Compute/HighCostGet3Min;159 | Bu istek hedefi de dahil olmak üzere kaynak demetine veya işlem grubu kapsayan azaltma ilkesi için kalan API çağrısı sayısı                                                                   |
| x-ms-istek-ücretsiz               | ```<count>   ```                             | 1                                     | Bu geçerli ilkenin sınırı yönelik HTTP isteği için "dolu" çağrısı sayısını sayar. Bu çoğunlukla 1'dir. Toplu istekleri, örneğin bir sanal makine ölçek kümesi ölçeklendirme birden çok sayıları ücret. |


Bir API isteği birden fazla kısıtlama ilkelere tabi olduğunu unutmayın. Ayrı bir olacaktır `x-ms-ratelimit-remaining-resource` her ilke için başlığı. 

Bir sanal makine ölçek kümesi isteğinde bir VM'yi silmek için bir örnek yanıt burada verilmiştir.

```
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet3Min;107 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet30Min;587 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VMScaleSetBatchedVMRequests5Min;3704 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VmssQueuedVMOperations;4720 
```

##<a name="throttling-error-details"></a>Hata ayrıntılarını azaltma

429 HTTP durum, genellikle bir çağrı hızı sınırını aşması nedeniyle bir isteği reddetmek için kullanılır. İşlem kaynak sağlayıcısı tipik azaltma hata yanıtı aşağıdaki örnekteki gibi görünür (yalnızca ilgili üst bilgiler gösterilir):

```
HTTP/1.1 429 Too Many Requests
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet3Min;46
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet30Min;0
Retry-After: 1200
Content-Type: application/json; charset=utf-8
{
  "code": "OperationNotAllowed",
  "message": "The server rejected the request because too many requests have been received for this subscription.",
  "details": [
    {
      "code": "TooManyRequests",
      "target": "HighCostGet30Min",
      "message": "{\"operationGroup\":\"HighCostGet30Min\",\"startTime\":\"2018-06-29T19:54:21.0914017+00:00\",\"endTime\":\"2018-06-29T20:14:21.0914017+00:00\",\"allowedRequestCount\":800,\"measuredRequestCount\":1238}"
    }
  ]
}

```

Kalan çağrı sayısı 0 ile döndürülen azaltma hata nedeniyle bir ilkedir. Bu durumda olan `HighCostGet30Min`. Genel yanıt gövdesi genel Azure Resource Manager API'si hata (OData ile uyumlu) biçimdir. Ana hata kodu `OperationNotAllowed`, bir işlem kaynak sağlayıcısı azaltma hataları (arasında istemci hataları diğer türleri) bildirmek için kullanır. 

Yukarıda gösterildiği gibi her kısıtlama hatası içerir `Retry-After` saniye en az sayıda istemci sağlar üst bilgi isteği yeniden denemeden önce beklemesi gereken. 

## <a name="best-practices"></a>En iyi uygulamalar 

- Azure hizmet API hataları koşulsuz olarak yeniden denemeyin. Yeniden deneme mümkün olmayan bir hata ile karşılaşıldığında hızlı yeniden deneme döngüsünde almak için istemci kodu buna yaygın bir durumdur. Yeniden deneme sonunda hedef işlem grubu için izin verilen çağrısı sınırı tüketebilir ve diğer istemciler aboneliğin etkiler. 
- Yüksek hacimli API Otomasyon durumlarda, bir hedef işlem grubu için kullanılabilir çağrı sayısı düşük bazı eşiğinin altına düştüğünde proaktif istemci-tarafı Self kısıtlama uygulama göz önünde bulundurun. 
- Zaman uyumsuz işlemleri izleme sırasında Retry-After üst bilgisi ipuçları uyar. 
- İstemci kodu, VM kaynak grubu veya tüm abonelik içeren ve ardından istemci tarafında gereken VM çekme tüm Vm'leri listelemek yerine doğrudan belirli bir sanal makine, bir sorgu hakkında bilgi gerekip gerekmediğini. 
- İstemci kodu, VM'ler, diskler ve belirli bir Azure konumdan anlık görüntüleri gerekiyorsa, tüm abonelik Vm'leri sorgulama ve ardından istemci tarafında konuma göre filtreleme yerine konum temelli form sorgu kullanın: `GET /subscriptions/<subId>/providers/Microsoft.Compute/locations/<location>/virtualMachines?api-version=2017-03-30` ve `/subscriptions/<subId>/providers/Microsoft.Compute/virtualMachines` işlem için sorgu Kaynak sağlayıcı bölgesel uç. •, Oluşturma veya API kaynaklar özellikle güncelleştiriliyor, VM'ler ve sanal makine ölçek kümeleri olduğunda, döndürülen zaman uyumsuz işlem tamamlanana kadar kaynak URL'si üzerinde sorgulama yapın daha izlemek için çok daha verimli (temel `provisioningState`).

## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure Hizmetleri için yeniden deneme Kılavuzu hakkında daha fazla bilgi için bkz: [belirli hizmetlere yönelik yeniden deneme Kılavuzu](https://docs.microsoft.com/en-us/azure/architecture/best-practices/retry-service-specific)