---
author: ggailey777
ms.service: billing
ms.topic: include
ms.date: 05/09/2019
ms.author: glenga
ms.openlocfilehash: 8f30d9fb2fcfe8f55af13d7726aa8458f8733b3f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236011"
---
| Resource | [Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan) | [Premium planı](../articles/azure-functions/functions-scale.md#premium-plan-public-preview) | [App Service planı](../articles/azure-functions/functions-scale.md#app-service-plan)<sup>1</sup> |
| --- | --- | --- | --- |
| Ölçeği genişletme | Aktivita typu EventDriven | Aktivita typu EventDriven | [El ile/otomatik ölçeklendirme](../articles/app-service/web-sites-scale.md) | 
|Varsayılan [zaman aşımı süresi](../articles/azure-functions/functions-scale.md#timeout) (min) |5 | 30 |30<sup>2</sup> |
|En fazla [zaman aşımı süresi](../articles/azure-functions/functions-scale.md#timeout) (min) |10 | sınırsız | Sınırsız<sup>3</sup> |
| En fazla giden bağlantı (örnek başına) | 600 etkin (1200 toplam) | sınırsız | sınırsız |
| En fazla istek boyutu (MB)<sup>4</sup> | 100 | 100 | 100 |
| Maksimum sorgu dizesi uzunluğu<sup>4</sup> | 4096 | 4096 | 4096 |
| En fazla istek URL uzunluğu<sup>4</sup> | 8192 | 8192 | 8192 |
| [ACU](../articles/virtual-machines/windows/acu.md) örnek başına | 100 | 210-840 | 100-840 |
| En fazla bellek (GB başına örneği) | 1,5 | 3.5-14 | 1.75-14 |
| Plan başına işlev uygulamaları |100 |100 |Sınırsız<sup>5</sup> |
| [App Service planları](../articles/app-service/overview-hosting-plans.md) | başına 100 [bölge](https://azure.microsoft.com/global-infrastructure/regions/) |kaynak grubu başına 100 |kaynak grubu başına 100 |
| Depolama<sup>6</sup> |1 GB |250 GB |50-1000 GB |
| Uygulama başına özel etki alanları</a> |500<sup>7</sup> |500 |500 |
| Özel etki alanı [SSL desteği](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |Desteklenen değil için joker karakter sertifikası *. azurewebsites.net varsayılan olarak kullanılabilir| Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir |Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir | 

<sup>1</sup>çeşitli App Service planı seçenekleri için belirli sınırları için bkz: [App Service planı sınırları](../articles/azure-subscription-service-limits.md#app-service-limits).  
<sup>2</sup>varsayılan olarak, App Service planı işlevler 1.x çalışma zamanı için zaman aşımını sınırsızdır.  
<sup>3</sup>App Service planı ayarlanması gerekir [Always On](../articles/azure-functions/functions-scale.md#always-on). Standart ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup> bu limitleri [konak kümesi](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config).  
<sup>5</sup> etkinlik uygulamaları, makine örnekleri ve karşılık gelen kaynak kullanımı, barındırabilirsiniz işlev uygulamaları gerçek sayısına bağlıdır.   
<sup>6</sup>depolama sınırı toplam içerik boyutu geçici bir depolama tüm uygulamalar arasında aynı App Service planında alanıdır. Tüketim planı, Azure dosyaları için geçici depolama kullanır.  
<sup>7</sup>, işlev uygulamanızı barındırılan bir [tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan), yalnızca CNAME seçeneği desteklenmiyor. İşlev uygulamaları için bir [Premium planı](../articles/azure-functions/functions-scale.md#premium-plan-public-preview) veya [App Service planı](../articles/azure-functions/functions-scale.md#app-service-plan), bir CNAME ya da bir A kaydı kullanarak özel bir etki alanını eşleyebilirsiniz. 
