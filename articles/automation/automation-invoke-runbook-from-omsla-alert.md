---
title: "Bir Log Analytics uyarısından Azure Otomasyonu runbook’u çağırma | Microsoft Docs"
description: "Bu makalede, Operations Management Suite’te Log Analytics uyarısından Otomasyon runbook’u çağırma işlemine genel bakış sunulmaktadır."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 76d5ab23ef1962733ccb3b36640facdba9ab8acb
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="call-an-azure-automation-runbook-from-a-log-analytics-alert"></a>Log Analytics uyarısından Azure Otomasyonu runbook’u çağırma

Azure Log Analytics’te sonuçlar ölçütlerinizle eşleştiğinde uyarı kaydı oluşturmak üzere bir uyarı yapılandırabilirsiniz. Bu uyarı, sorunu otomatik olarak düzeltme girişimiyle bir Azure Otomasyonu runbook'unu otomatik olarak çalıştırabilir. 

Örneğin bir uyarı, işlemci kullanımında uzun süreli bir yükselmeyi gösterebilir. Veya bir iş uygulamasının işlevselliği için önemli olan bir uygulama işleminin başarısız olduğunu gösterebilir. Bir runbook daha sonra Windows olay günlüğüne karşılık gelen bir olay yazabilir.  

Uyarı yapılandırmasında runbook'u çağırmak için iki seçenek vardır:

* Web kancası kullanma.
   * Operations Management Suite çalışma alanınız bir Otomasyon hesabına bağlı değilse kullanılabilecek tek seçenek budur.
   * Operations Management Suite çalışma alanına bağlı bir Otomasyon hesabınız varsa bu seçenek yine de kullanılabilir.  

* Doğrudan bir runbook seçme.
   * Bu seçenek yalnızca Operations Management Suite çalışma alanı bir Otomasyon hesabına bağlı olduğunda kullanılabilir.

## <a name="calling-a-runbook-by-using-a-webhook"></a>Web kancası kullanarak bir runbook çağırma

Web kancası kullanarak tek bir HTTP isteği ile Azure Otomasyonu’nda belirli bir runbook başlatabilirsiniz. Web kancası kullanarak bir uyarı eylemi olarak runbook'u çağırmak üzere [Log Analytics uyarısını](../log-analytics/log-analytics-alerts.md#alert-rules) yapılandırmadan önce, bu yöntem kullanılarak çağrılacak runbook için bir [web kancası oluşturmanız](automation-webhooks.md#creating-a-webhook) gerekir. Uyarı kuralını yapılandırırken başvurabilmeniz için web kancası URL'sini kaydetmeyi unutmayın.   

## <a name="calling-a-runbook-directly"></a>Doğrudan bir runbook çağırma

Operations Management Suite çalışma alanınıza Automation and Control teklifini yükleyip yapılandırabilirsiniz. Uyarı için runbook eylemleri seçeneğini yapılandırırken tüm runbook'ları **Runbook seç** açılır listesinden görüntüleyebilir ve uyarıya yanıt olarak çalıştırmak istediğiniz runbook'u seçebilirsiniz. Seçili runbook, Azure çalışma alanında veya bir karma runbook çalışanında çalışabilir. 

Runbook seçeneğini kullanarak uyarıyı oluşturduktan sonra, runbook için bir web kancası oluşturulur. Otomasyon hesabını açıp seçili runbook'un web kancası bölmesine ulaştığınızda web kancasını görebilirsiniz. 

Uyarıyı silerseniz web kancası silinmez. Bu bir sorun değildir. Web kancası, Otomasyon hesabını düzenli tutmak için sonunda el ile silmeniz gereken, yalnız bırakılmış bir öğe haline gelir.  

## <a name="characteristics-of-a-runbook"></a>Bir runbook’un özellikleri

Log Analytics uyarısından runbook çağırmaya yönelik her iki yöntem de uyarı kurallarınızı yapılandırmadan önce anlamanız gereken farklı davranış özelliklerine sahiptir. 

Uyarı verileri JSON biçimindedir ve **SearchResult** adlı tek bir özellik içinde yer alır. Bu biçim, standart yüke sahip runbook ve web kancası eylemlerine yöneliktir. Özel yüklere (**RequestBody** içinde **IncludeSearchResults:True** dahil olmak üzere) yönelik web kancası işlemleri için özellik, **SearchResults** şeklindedir.

**Object** türünde **WebhookData** adlı bir runbook girdi parametreniz olmalıdır. Zorunlu veya isteğe bağlı olabilir. Uyarı bu girdi parametresini kullanarak arama sonuçlarını runbook’a geçirir.

```powershell
param  
    (  
    [Parameter (Mandatory=$true)]  
    [object] $WebhookData  
    )
```
**WebhookData**’yı bir PowerShell nesnesine dönüştürmek için ayrıca bir kodunuz olmalıdır.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
```

**$SearchResult** bir nesne dizisidir. Her nesne bir arama sonucunun değerlerini içeren alanlardan oluşur.


## <a name="example-walkthrough"></a>Örnek kılavuz

Aşağıdaki örnek, bu işlemin nasıl çalıştığını gösteren grafiksel bir runbook ile ilgilidir. Bir Windows hizmetini başlatır.

![Bir Windows hizmeti başlatmaya yönelik grafiksel runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)

Runbook, **WebhookData** adlı **Object** türünde bir giriş parametresine sahiptir. **SearchResult** içeren uyarıdan geçirilmiş web kancası verilerini içerir.

![Runbook giriş parametreleri](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)

Bu örnek için Log Analytics’te iki özel alan oluşturduk: **SvcDisplayName_CF** ve **SvcState_CF**. Bu alanlar hizmet görünen adı ve sistem olay günlüğüne yazılan olayı hizmet durumudur (yani çalışıyor veya durduruldu). Ardından şu arama sorgusuyla bir uyarı kuralı oluşturduk: Böylece, Windows sisteminde Yazdırma Biriktiricisi hizmetinin ne zaman durdurulduğunu saptayabiliriz:

`Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` 

Bu, ilgilendiğiniz herhangi bir hizmet olabilir. Ancak bu örnekte Windows OS ile birlikte sunulan önceden var olan hizmetlerden birine başvurulmaktadır. Uyarı eylemi, bu örnekte kullanılan runbook'u yürütecek ve hedef sistemlerde etkinleştirilmiş karma runbook çalışanı üzerinde çalışacak şekilde yapılandırılmıştır.   

**LA’den Hizmet Adı Alma** runbook kodu etkinliği, JSON biçimindeki dizeyi bir nesne türüne dönüştürür ve **SvcDisplayName_CF** öğesiyle filtreler. Windows hizmetinin görünen adını ayıklar ve yeniden başlatmayı denemeden önce hizmetin durdurulduğunu doğrulamak üzere bu değeri sonraki etkinliğe geçirir. **SvcDisplayName_CF**, Log Analytics’te bu örneği göstermek üzere oluşturduğumuz bir [özel alandır](../log-analytics/log-analytics-custom-fields.md).

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

Hizmet durdurulduğunda, Log Analytics'teki uyarı kuralı bir eşleşme saptar ve runbook’u tetikleyip uyarı bağlamını runbook'a gönderir. Runbook, hizmetin durdurulduğunu doğrulamaya çalışır. Doğrulayabilirse, runbook hizmeti yeniden başlatmayı, doğru şekilde başlatıldığını doğrulamayı ve sonuçları göstermeyi dener.     

Alternatif olarak, Otomasyon hesabınız Operations Management Suite çalışma alanına bağlı değilse, uyarı kuralını bir web kancası eylemiyle yapılandırabilirsiniz. Web kancası eylemi runbook’u tetikler. Runbook’u ayrıca JSON biçimli dizeyi dönüştürecek ve daha önce bahsedilen yönergeleri izleyerek **SearchResult** ile filtreleyecek şekilde yapılandırır.    

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](../log-analytics/log-analytics-log-search-upgrade.md) yükseltilmişse ağ kancası yükü değiştirilmiştir. Biçimle ilgili ayrıntılar için bkz. [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Sonraki adımlar

* Log Analytics’teki uyarılar ve bir uyarı oluşturma hakkında daha fazla bilgi için bkz. [Log Analytics’teki Uyarılar](../log-analytics/log-analytics-alerts.md).

* Web kancası kullanarak runbook’ları tetikleme hakkında bilgi almak için bkz. [Azure Otomasyonu web kancaları](automation-webhooks.md).
