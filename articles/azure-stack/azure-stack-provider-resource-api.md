---
title: Sağlayıcı kaynak kullanım API'si | Microsoft Docs
description: Başvuru için kaynak kullanım API'si, Azure Stack kullanım bilgilerini alır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: c6f17fd4cc225b7d4ce60d38bf2abcabf12a40c5
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945595"
---
# <a name="provider-resource-usage-api"></a>Sağlayıcı kaynak kullanım API’si
Terim *sağlayıcısı* Hizmet Yöneticisi ve herhangi bir sağlayıcı temsilcisi için geçerlidir. Azure Stack operatörleri ve sağlayıcı temsilcisi sağlayıcı kullanım API'si doğrudan kiracıları kullanımını görüntülemek için kullanabilirsiniz. Örneğin, diyagramda gösterildiği gibi P1'ın kullanım bilgileri için API sağlayıcısı P0 çağırabilir ve P1 ve P2'ın doğrudan kullanım P3 ve P4 kullanım bilgileri için çağırabilirsiniz.

![Sağlayıcı hiyerarşinin kavramsal model](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>API çağrısı başvurusu
### <a name="request"></a>İstek
İstek tüketim ayrıntılarını ve istenen zaman çerçevesi için istenen abonelikleri alır. Hiçbir istek gövdesi yok.

Bu kullanım API'si bir API sağlayıcısı olduğundan, çağıran bir sağlayıcının abonelik sahibi, katkıda bulunan veya okuyucu rol atanması gerekir.

| **Yöntem** | **İstek URI'si** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity} & subscriberId {sub1.1} = & API-version = 2015-06-01-preview & continuationToken {belirteci-value} = |

### <a name="arguments"></a>Bağımsız Değişkenler
| **Bağımsız değişken** | **Açıklama** |
| --- | --- |
| *armendpoint* |Azure Stack ortamınıza Azure Resource Manager uç noktası. Azure Resource Manager uç nokta adı biçiminde olduğunu Azure Stack kuraldır `https://adminmanagement.{domain-name}`. Etki alanı adıdır, örneğin, Geliştirme Seti için *local.azurestack.external*, Resource Manager uç noktasını ise `https://adminmanagement.local.azurestack.external`. |
| *subId* |Çağrıyı yapan kullanıcının abonelik kimliği. |
| *reportedStartTime* |Başlangıç saati sorgu. Değeri *DateTime* (UTC) Eşgüdümlü Evrensel Saat ve saat, örneğin, 13:00 başında olması gerekir. Günlük toplama için UTC gece yarısı ile bu değeri ayarlayın. Biçim *kaçış* ISO 8601. Örneğin, *2015-06-%16T18 %3a53 %3a11 %2b00 3a00Z*, iki nokta üst üste için kaçış burada *% 3a* ve artı için kaçış *% 2b* URI kolay olmasını sağlayın. |
| *reportedEndTime* |Sorgu bitiş saati. Uygulanan kısıtlamaları *reportedStartTime* bu bağımsız değişken için de geçerlidir. Değeri *reportedEndTime* ya da geçerli tarihi gelecekte olamaz. İse, sonuç "tam işleme yok." ayarlanır |
| *aggregationGranularity* |İki ayrı olası değerlere sahip isteğe bağlı bir parametre: günlük ve saatlik. Değerleri Öner gibi günlük ayrıntı düzeyi, verileri döndürür ve diğeri ise saatlik bir çözüm. Günlük varsayılan seçenektir. |
| *subscriberId* |Abonelik kimliği Filtrelenmiş veri almak için sağlayıcının doğrudan bir kiracının abonelik kimliği gereklidir. Abonelik kimliği parametre belirtilmediğinde arama sağlayıcısı'nın doğrudan kiracılar için kullanım verilerini döndürür. |
| *API sürümü* |Bu isteği yapmak için kullanılan protokol sürümü. Bu değeri şuna ayarlı *2015-06-01-preview*. |
| *continuationToken* |Belirteç sağlayıcı kullanım API'si son çağrısından alınan. Bu belirteç, yanıt 1.000 satır büyük olduğunda ve ilerleme için yer işareti olarak davranan gereklidir. Belirteç mevcut değilse, verileri günün başlangıcından itibaren alınır veya saat ayrıntı düzeyi üzerinde göre geçirilen. |

### <a name="response"></a>Yanıt
/Subscriptions/sub1/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime Al = 2015-06-%01T00 %3a00 %3a00 %2b00 3a00 & aggregationGranularity günlük & subscriberId = = sub1.1 & api sürümü 1.0 =

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce.Admin/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce.Admin/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Yanıt Ayrıntıları
| **Bağımsız değişken** | **Açıklama** |
| --- | --- |
| *Kimliği* |Kullanım toplama benzersiz kimliği. |
| *Adı* |Kullanım toplama adı. |
| *type* |Kaynak tanımı. |
| *Subscriptionıd* |Azure Stack kullanıcı abonelik tanımlayıcısı. |
| *usageStartTime* |UTC başlangıç zamanı, bu kullanım toplama ait olduğu kullanım demeti.|
| *usageEndTime* |Bu kullanım toplama ait olduğu kullanım demeti bitiş saati UTC. |
| *instanceData* |Örnek ayrıntıları (yeni biçimde) anahtar-değer çiftleri:<br> *resourceUri*: tam kaynak kimliği, kaynak grupları ve örnek adını içerir. <br> *Konum*: Bu hizmeti çalıştırıldığı bölge. <br> *etiketleri*: kullanıcı tarafından belirtilen kaynak etiketleri. <br> *Additionalınfo*: işletim sistemi sürümü veya görüntü türü daha fazla ayrıntı, örneğin, tüketilen kaynak hakkında. |
| *Miktar* |Bu zaman çerçevesinde gerçekleşen kaynak tüketimi miktarı. |
| *meterId* |Tüketilen kaynak için benzersiz kimlik (olarak da adlandırılan *ResourceId*). |


## <a name="retrieve-usage-information"></a>Kullanım bilgilerini alma

### <a name="powershell"></a>PowerShell

Kullanım verilerini oluşturmak için çalışan ve sistem örneğin etkin olarak kullanan kaynaklar, etkin bir sanal makineye veya bazı veri vb. içeren bir depolama hesabı olmalıdır. Dikey emin olmak için izleme olup Azure Stack Market'te çalıştıran herhangi bir kaynağa sahip, bir sanal makine (VM) dağıtma ve VM doğrulayın konusunda emin değilseniz çalıştığı. Kullanım verilerini görüntülemek için aşağıdaki PowerShell cmdlet'lerini kullanın:

1. [Azure Stack için PowerShell yükleyin.](azure-stack-powershell-install.md)
2. [Azure Stack kullanıcının yapılandırma](user/azure-stack-powershell-configure-user.md) veya [Azure Stack operatörü'nın](azure-stack-powershell-configure-admin.md) PowerShell ortamı 
3. Kullanım verilerini almak için kullanın [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell cmdlet:
```powershell
Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
```
### <a name="rest-api"></a>REST API

Microsoft.Commerce.Admin hizmet çağırarak silinen abonelikler için kullanım bilgilerini toplayabilirsiniz. 

**Tüm Kiracı kullanımı için döndürülecek etkin kullanıcıları için silinir:**

| **Yöntem** | **İstek URI'si** |
| --- | --- |
| GET | https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&api-version= 2015-06-01-Önizleme |

**Silinmiş veya etkin Kiracı kullanım döndürmek için:**

| **Yöntem** | **İstek URI'si** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&subscriberId={ Abonelik-kimliği} & API-version = 2015-06-01-Önizleme |


## <a name="next-steps"></a>Sonraki adımlar
[Kiracı kaynak kullanım API'si başvurusu](azure-stack-tenant-resource-usage-api.md)

[Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)
