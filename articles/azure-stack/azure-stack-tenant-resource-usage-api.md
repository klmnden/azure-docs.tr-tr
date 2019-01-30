---
title: Kiracı kaynak kullanım API'si | Microsoft Docs
description: Azure Stack kullanım bilgilerini almak için kaynak kullanım API'si, başvuru.
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
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: alfredop
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: db0cdfce5903b8866985b0545494351e2796d8e2
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244632"
---
# <a name="tenant-resource-usage-api"></a>Kiracı kaynak kullanım API'si

Bir kiracı, Kiracı API'si, kiracının kendi kaynak kullanım verilerini görüntülemek için kullanabilirsiniz. Bu API (şu anda özel Önizleme) ile Azure kullanım API'si tutarlıdır.

Windows PowerShell cmdlet'ini kullanabilirsiniz **Get-UsageAggregates** Azure'da gibi kullanım verilerini almak için.

## <a name="api-call"></a>API çağrısı
### <a name="request"></a>İstek
İstek tüketim ayrıntılarını ve istenen zaman çerçevesi için istenen abonelikleri alır. Hiçbir istek gövdesi yok.

| **Yöntem** | **İstek URI'si** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version= 2015-06-01-preview & continuationToken {belirteci-value} = |

### <a name="arguments"></a>Bağımsız Değişkenler
| **Bağımsız değişken** | **Açıklama** |
| --- | --- |
| *armendpoint* |Azure Stack ortamınıza Azure Resource Manager uç noktası. Azure Resource Manager uç nokta adı biçiminde olduğunu Azure Stack kuraldır `https://management.{domain-name}`. Örneğin, Geliştirme Seti için local.azurestack.external etki alanı adıdır ve sonra Resource Manager uç nokta `https://management.local.azurestack.external`. |
| *subId* |Çağrıyı yapan kullanıcının abonelik kimliği. Tek bir aboneliğin kullanım için sorgu yalnızca bu API'yi kullanabilirsiniz. Sağlayıcıları, sağlayıcı kaynak kullanım API'si sorgu kullanımı için tüm kiracılar için kullanabilirsiniz. |
| *reportedStartTime* |Başlangıç saati sorgu. Değeri *DateTime* UTC ile saat, örneğin, 13:00 başında olmalıdır. Günlük toplama için UTC gece yarısı ile bu değeri ayarlayın. Biçim *kaçış* ISO 8601, burada iki nokta üst üste kaçış % 3a için örneğin, 2015-06-%16T18 %3a53 %3a11 %2b00 3a00Z, ve ayrıca URI kolay olması için % 2b kaçırılmışsa. |
| *reportedEndTime* |Sorgu bitiş saati. Uygulanan kısıtlamaları *reportedStartTime* bu bağımsız değişken için de geçerlidir. Değeri *reportedEndTime* gelecekte olamaz. |
| *aggregationGranularity* |İki ayrı olası değerlere sahip isteğe bağlı bir parametre: günlük ve saatlik. Değerleri Öner gibi günlük ayrıntı düzeyi, verileri döndürür ve diğeri ise saatlik bir çözüm. Günlük varsayılan seçenektir. |
| *API sürümü* |Bu isteği yapmak için kullanılan protokol sürümü. 2015-06-01-preview kullanmanız gerekir. |
| *continuationToken* |Belirteç sağlayıcı kullanım API'si son çağrısından alınan. Bu belirteç, yanıt 1.000 satır büyük olduğunda ve ilerleme durumu için bir yer işareti olarak davranan gereklidir. Yoksa, verileri günün başlangıcından itibaren alınır veya saat ayrıntı düzeyi üzerinde göre geçirilen. |

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
| *id* |Kullanım toplamanın benzersiz kimliği |
| *Adı* |Kullanım toplamanın adı |
| *type* |Kaynak tanımı |
| *Subscriptionıd* |Azure kullanıcı abonelik tanımlayıcısı |
| *usageStartTime* |Başlangıç ait olduğu için bu kullanım toplama kullanım demeti zamanı UTC |
| *usageEndTime* |Bu kullanım toplama ait olduğu kullanım demeti bitiş zamanı UTC |
| *instanceData* |Örnek ayrıntıları (yeni biçimde) anahtar-değer çiftleri:<br>  *resourceUri*: Kaynak grupları ve örnek adı dahil olmak üzere, tam kaynak kimliği <br>  *Konum*: Bu hizmetin çalıştırıldığı bölge <br>  *Etiketleri*: Kullanıcının belirttiği kaynak etiketleri <br>  *Additionalınfo*: Tüketilen, kaynak işletim sistemi sürümü veya görüntü hakkında daha fazla ayrıntı girin |
| *Miktar* |Bu zaman çerçevesinde gerçekleşen kaynak tüketimi miktarı |
| *meterId* |Tüketilen kaynak için benzersiz kimlik (olarak da adlandırılan *ResourceId*) |


## <a name="next-steps"></a>Sonraki adımlar
[Sağlayıcı kaynak kullanım API’si](azure-stack-provider-resource-api.md)

[Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)

