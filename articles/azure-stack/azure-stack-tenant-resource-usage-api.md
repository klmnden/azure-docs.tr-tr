---
title: "Kaynak kullanımı API Kiracı | Microsoft Docs"
description: "Azure yığın kullanım bilgileri almak başvuru API, kaynak kullanımı."
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: b9d7c7ee-e906-4978-92a3-a2c52df16c36
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2016
ms.author: alfredop
ms.openlocfilehash: f2eaf1c766d6c86741cf0fd561c131eacb34d782
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tenant-resource-usage-api"></a>Kiracı kaynak kullanım API'si
Bir kiracı Kiracı API'si, kiracının kendi kaynak kullanım verilerini görüntülemek için kullanabilirsiniz. Bu API (şu anda özel olarak incelenmektedir) ile Azure kullanım API tutarlıdır.

Windows PowerShell cmdlet'ini kullanabilirsiniz **Get-UsageAggregates** Azure'da gibi kullanım verilerini almak için.

## <a name="api-call"></a>API çağrısı
### <a name="request"></a>İstek
İstek tüketim ayrıntıları istenen zaman çerçevesi ve istenen abonelikler için alır. İstek gövdesinde yok.

| **Yöntemi** | **İstek URI'si** |
| --- | --- |
| AL |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version= 2015-06-01-Önizleme & continuationToken = {değer belirteci} |

### <a name="arguments"></a>Bağımsız Değişkenler
| **Bağımsız değişken** | **Açıklama** |
| --- | --- |
| *Armendpoint* |Azure yığın ortamınızın Azure Kaynak Yöneticisi uç noktası. Azure Resource Manager uç noktanın adı şu biçimdedir Azure yığın kuraldır `https://management.{domain-name}`. Örneğin, Geliştirme Seti için local.azurestack.external etki alanı adıdır ve ardından kaynak yöneticisi uç noktası `https://management.local.azurestack.external`. |
| *subId* |Çağrıyı yapan kullanıcının abonelik kimliği. Bu API sorgu için yalnızca tek bir aboneliğin kullanım için kullanabilirsiniz. Sağlayıcıları sağlayıcısı kaynak kullanım API sorgu kullanım için tüm kiracılar için kullanabilirsiniz. |
| *reportedStartTime* |Sorgunun başlarsınız. Değeri *DateTime* UTC ve saat, örneğin, 13:00 başında olması gerekir. Günlük toplama için bu değer UTC gece yarısına ayarlanmış. Biçim *kaçışlı* ISO 8601, burada iki nokta üst üste kaçış karakteri içermediği için % 3a Örneğin, 2015 06 %16T18 %3a53 %3a11 %2b00 3a00Z, ve artı URI kolay olmasını sağlamak için % 2b kaçışlı. |
| *reportedEndTime* |Sorgunun bitiş saati. Geçerli kısıtlamaları *reportedStartTime* bu bağımsız değişken için de geçerlidir. Değeri *reportedEndTime* gelecekte olamaz. |
| *aggregationGranularity* |İki ayrı olası değerlere sahip isteğe bağlı bir parametre: günlük ve saatlik. Değerleri önermek gibi veri yer günlük ayrıntı düzeyi döndürür ve diğeri saatlik bir çözüm. Günlük varsayılan seçenektir. |
| *API sürümü* |Bu isteği yapmak için kullanılan protokol sürümü. 2015-06-01-Önizleme kullanmanız gerekir. |
| *continuationToken* |Belirteci son çağrı kullanım API'si sağlayıcısına alınır. Bu belirteç yanıt 1.000 satırları büyüktür ve ilerleme durumu için bir yer işareti gibi davranır gereklidir. Yoksa, veriler gün baştan alınır veya saat üzerinde ayrıntı göre geçirilen. |

### <a name="response"></a>Yanıt
/Subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime Al = 2015-06-%01T00 %3a00 %3a00 %2b00 3a00 & aggregationGranularity günlük = & API sürümü 1.0 =

```json
{
"value": [
{

"id":
"/subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",
"name": "sub1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1",
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
| *Kimliği* |Kullanım toplama benzersiz kimliği |
| *adı* |Kullanım toplama adı |
| *türü* |Kaynak tanımı |
| *Subscriptionıd* |Azure kullanıcının abonelik tanımlayıcısı |
| *usageStartTime* |UTC başlangıç saatini bu kullanım toplama ait olduğu kullanım demet |
| *usageEndTime* |Bu kullanım toplama ait olduğu kullanım demet UTC bitiş saati |
| *instanceData* |Örnek ayrıntıları (yeni biçimde) anahtar-değer çiftleri:<br>  *resourceUri*: kaynak kimliği, kaynak grupları ve örnek adı dahil olmak üzere tam <br>  *Konum*: Bu hizmet çalıştırıldı bölge <br>  *Etiketler*: kullanıcının belirttiği kaynak etiketleri <br>  *additionalınfo alanına*: işletim sistemi sürümü veya görüntü türü ayrıntıları, örneğin, kullanılan kaynak hakkında daha fazla |
| *Miktar* |Bu zaman dilimi içinde oluştu kaynak tüketimi miktarı |
| *meterId* |Benzersiz kimliği için kullanılan kaynak (olarak da bilinir *ResourceId*) |

## <a name="next-steps"></a>Sonraki adımlar
[Sağlayıcı kaynak kullanım API’si](azure-stack-provider-resource-api.md)

[Kullanımı ile ilgili SSS](azure-stack-usage-related-faq.md)

