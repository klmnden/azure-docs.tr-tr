---
title: Sorun giderme azaltma hataları azure'da | Microsoft Docs
description: Azaltma hataları, yeniden denemeler ve Azure işlem, geri alma.
services: virtual-machines
documentationcenter: ''
author: changov
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.devlang: na
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: vashan, rajraj, changov
ms.openlocfilehash: fa65b108f3aea79d4417e65d706d42f0bd819f54
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445392"
---
# <a name="troubleshooting-api-throttling-errors"></a>API azaltma hatalarının sorunlarını giderme 

Azure işlem istekleri, abonelik ve hizmetin genel performansını ile yardımcı olmak için bölge başına temelinde kısıtlanmış. İzin verilen en fazla API istek hızı Microsoft.Compute ad alanı altında kaynaklarını yöneten Azure bilgi işlem kaynak sağlayıcısı (CRP) yapılan tüm çağrılarda aşmamak emin oluruz. Bu belge, azaltma sorunları gidermeye ilişkin ayrıntıları azaltma API'sini açıklayan ve aşarak önlemek için en iyi yöntemler.  

## <a name="throttling-by-azure-resource-manager-vs-resource-providers"></a>Azure Resource Manager vs kaynak sağlayıcıları tarafından azaltma  

Ön kapısı Azure, Azure Resource Manager kimlik doğrulama ve ilk sırada doğrulama ve tüm gelen API isteklerinin azaltma yapar. Azure Resource Manager çağrı hız sınırları ve ilgili tanılama yanıt HTTP üstbilgileri açıklanmıştır [burada](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-request-limits).
 
Bir Azure API istemcisini bir kısıtlama hatası girdiğinde, HTTP 429 çok fazla istek durumudur. İstek azaltma Azure Resource Manager veya CRP gibi temel bir kaynak sağlayıcısı tarafından yapıldığını anlamak için incelemek `x-ms-ratelimit-remaining-subscription-reads` GET istekleri için ve `x-ms-ratelimit-remaining-subscription-writes` olmayan GET istekleri için yanıt üstbilgileri. Kalan çağrısı sayısı 0 yaklaşıyorsa, Azure Resource Manager tarafından tanımlanan aboneliğin genel çağrı sınırına ulaşıldı. Etkinlikler tüm abonelik istemcileri tarafından birlikte sayılır. Aksi takdirde, azaltma hedef kaynak sağlayıcısından gelen (bir ele `/providers/<RP>` segment istek URL'si). 

## <a name="call-rate-informational-response-headers"></a>Çağrı hızı bilgilendirici bir yanıt üstbilgileri 

| Üst bilgi                            | Değeri biçimi                           | Örnek                               | Açıklama                                                                                                                                                                                               |
|-----------------------------------|----------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-ms-ratelimit-kalan-kaynak |```<source RP>/<policy or bucket>;<count>```| Microsoft.Compute/HighCostGet3Min;159 | Bu istek hedefi de dahil olmak üzere kaynak demetine veya işlem grubu kapsayan azaltma ilkesi için kalan API çağrısı sayısı                                                                   |
| x-ms-istek-ücretsiz               | ```<count>```                             | 1                                     | Bu geçerli ilkenin sınırı yönelik HTTP isteği için "dolu" çağrısı sayısını sayar. Bu çoğunlukla 1'dir. Toplu istekleri, örneğin bir sanal makine ölçek kümesi ölçeklendirme birden çok sayıları ücret. |


Bir API isteği birden fazla kısıtlama ilkelere tabi olduğunu unutmayın. Ayrı bir olacaktır `x-ms-ratelimit-remaining-resource` her ilke için başlığı. 

Sanal makine ölçek kümesi isteği silmek için bir örnek yanıt burada verilmiştir.

```
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet3Min;107 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet30Min;587 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VMScaleSetBatchedVMRequests5Min;3704 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VmssQueuedVMOperations;4720 
```

## <a name="throttling-error-details"></a>Hata ayrıntılarını azaltma

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

Kalan çağrı sayısı 0 ile döndürülen azaltma hata nedeniyle bir ilkedir. Bu durumda olan `HighCostGet30Min`. Genel yanıt gövdesi genel Azure Resource Manager API'si hata (OData ile uyumlu) biçimdir. Ana hata kodu `OperationNotAllowed`, bir işlem kaynak sağlayıcısı azaltma hataları (arasında istemci hataları diğer türleri) bildirmek için kullanır. `message` Özelliği iç hata, bir seri hale getirilmiş JSON yapısıyla kısıtlama ihlali ayrıntılarını içerir.

Yukarıda gösterildiği gibi her kısıtlama hatası içerir `Retry-After` saniye en az sayıda istemci sağlar üst bilgi isteği yeniden denemeden önce beklemesi gereken. 

## <a name="api-call-rate-and-throttling-error-analyzer"></a>API çağrısı oranı ve azaltma hata Çözümleyicisi
Bir sorun giderme özelliğini bir önizleme sürümünü işlem kaynak sağlayıcısı API'si için kullanılabilir. Bu PowerShell cmdlet'lerini işlemi her zaman aralığını ve azaltma ihlalleri her işlem grubu (ilke) başına API istek hızı hakkında istatistikler sağlar:
-   [Dışarı aktarma AzLogAnalyticRequestRateByInterval](https://docs.microsoft.com/powershell/module/az.compute/export-azloganalyticrequestratebyinterval)
-   [Dışarı aktarma AzLogAnalyticThrottledRequests](https://docs.microsoft.com/powershell/module/az.compute/export-azloganalyticthrottledrequests)

API çağrısı istatistikleri, bir aboneliğin istemci davranışını harika Öngörüler sağlar ve azaltma neden arama desenlerinin kolay tanımayı etkinleştirin.

Bir anda Çözümleyicisi (desteklemek üzere yönetilen diskler) disk ve anlık görüntü kaynak türleri için istekleri sayılmaz sınırlamasıdır. Bu yana alınan CRP'ın telemetri verileri toplar da ARM azaltma hataları tanımlanmasına yardımcı olamaz. Ancak, bunlar kolayca göre farklı ARM yanıt üstbilgilerini, daha önce bahsedildiği gibi tanımlanabilir.

PowerShell cmdlet'leri (desteğiyle hiç biçimsel ancak henüz) doğrudan istemcileri tarafından kolayca çağrılabilen REST hizmeti API'si kullanmaktadır. Cmdlet'leri HTTP istek biçimini görmek için bunların yürütme Fiddler ile hata ayıklama anahtarı veya gözetleme - ile çalıştırın.


## <a name="best-practices"></a>En iyi uygulamalar 

- Azure hizmet API hatalarını koşulsuz olarak ve/veya hemen yeniden denemeyin. Yeniden deneme mümkün olmayan bir hata ile karşılaşıldığında hızlı yeniden deneme döngüsünde almak için istemci kodu buna yaygın bir durumdur. Yeniden deneme sonunda hedef işlem grubu için izin verilen çağrısı sınırı tüketebilir ve diğer istemciler aboneliğin etkiler. 
- Yüksek hacimli API Otomasyon durumlarda, bir hedef işlem grubu için kullanılabilir çağrı sayısı düşük bazı eşiğinin altına düştüğünde proaktif istemci-tarafı Self kısıtlama uygulama göz önünde bulundurun. 
- Zaman uyumsuz işlemleri izleme sırasında Retry-After üst bilgisi ipuçları uyar. 
- İstemci kodu belirli bir sanal makine hakkında bilgi gerekiyorsa, bu VM'yi içeren kaynak grubunu veya tüm abonelik içindeki tüm sanal makineleri listeleme ve ardından istemci tarafında gereken VM çekme yerine doğrudan sorgu. 
- İstemci kodu, VM'ler, diskler ve belirli bir Azure konumdan anlık görüntüleri gerekiyorsa, tüm abonelik Vm'leri sorgulama ve ardından istemci tarafında konuma göre filtreleme yerine konum temelli form sorgu kullanın: `GET /subscriptions/<subId>/providers/Microsoft.Compute/locations/<location>/virtualMachines?api-version=2017-03-30` işlem kaynak sağlayıcısı bölge için sorgu uç noktaları. 
-   Oluşturma veya API'si kaynaklarına özellikle güncelleştirme VM'ler ve sanal makine ölçek kümeleri, döndürülen zaman uyumsuz işlem tamamlanana kadar kaynak URL'si üzerinde sorgulama yapın daha izlemek için çok daha verimli olur (temel `provisioningState`).

## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure Hizmetleri için yeniden deneme Kılavuzu hakkında daha fazla bilgi için bkz: [belirli hizmetlere yönelik yeniden deneme Kılavuzu](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)
