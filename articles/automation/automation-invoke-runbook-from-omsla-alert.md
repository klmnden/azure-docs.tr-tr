---
title: Log Analytics uyarısından Azure Otomasyonu runbook’u çağırma
description: Bu makalede Azure günlük analizi uyarıdan bir Otomasyon runbook'u çağırmak nasıl bir bakış sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/12/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 0094362ee083511b05027f22b37ed62d56d68d41
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36217145"
---
# <a name="call-an-azure-automation-runbook-from-a-log-analytics-alert"></a>Log Analytics uyarısından Azure Otomasyonu runbook’u çağırma

Azure Log Analytics’te sonuçlar ölçütlerinizle eşleştiğinde uyarı kaydı oluşturmak üzere bir uyarı yapılandırabilirsiniz. Bu uyarı, sorunu otomatik olarak düzeltme girişimiyle bir Azure Otomasyonu runbook'unu otomatik olarak çalıştırabilir. 

Örneğin bir uyarı, işlemci kullanımında uzun süreli bir yükselmeyi gösterebilir. Veya bir iş uygulamasının işlevselliği için önemli olan bir uygulama işleminin başarısız olduğunu gösterebilir. Bir runbook daha sonra Windows olay günlüğüne karşılık gelen bir olay yazabilir.  

Uyarı yapılandırmasında runbook'u çağırmak için iki seçenek vardır:

* Web kancası kullanma.
   * Günlük analizi çalışma alanınız için bir Otomasyon hesabı bağlanmamışsa kullanılabilir tek seçenek budur.
   * Günlük analizi çalışma alanına bağlı bir Otomasyon hesabınız zaten varsa, bu seçenek kullanılabilir durumda kalır.  

* Doğrudan bir runbook seçme.
   * Bu seçenek, yalnızca bir Otomasyon hesabı için günlük analizi çalışma alanı bağlıysa kullanılabilir.

## <a name="calling-a-runbook-by-using-a-webhook"></a>Web kancası kullanarak bir runbook çağırma

Web kancası kullanarak tek bir HTTP isteği ile Azure Otomasyonu’nda belirli bir runbook başlatabilirsiniz. Yapılandırmadan önce [günlük uyarılar için Web kancası eylem](../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md) bir uyarı eylem olarak bir Web kancası kullanarak runbook'u çağırmak için yapmanız [bir Web kancası oluşturma](automation-webhooks.md#creating-a-webhook) bu yöntemle adlı runbook için. Uyarı kuralını yapılandırırken başvurabilmeniz için web kancası URL'sini kaydetmeyi unutmayın.   

## <a name="calling-a-runbook-directly"></a>Doğrudan bir runbook çağırma

Yükleyin ve otomasyon ve denetim günlük analizi çalışma alanınızda sunumu yapılandırın. Uyarı için runbook eylemleri seçeneğini yapılandırırken tüm runbook'ları **Runbook seç** açılır listesinden görüntüleyebilir ve uyarıya yanıt olarak çalıştırmak istediğiniz runbook'u seçebilirsiniz. Seçili runbook, Azure çalışma alanında veya bir karma runbook çalışanında çalışabilir. 

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

Alternatif olarak, günlük analizi çalışma alanına bağlı Automation hesabınız yoksa, bir Web kancası eylemiyle uyarı kuralı yapılandırabilirsiniz. Web kancası eylemi runbook’u tetikler. Runbook’u ayrıca JSON biçimli dizeyi dönüştürecek ve daha önce bahsedilen yönergeleri izleyerek **SearchResult** ile filtreleyecek şekilde yapılandırır.    

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](../log-analytics/log-analytics-log-search-upgrade.md) yükseltilmişse ağ kancası yükü değiştirilmiştir. Biçimle ilgili ayrıntılar için bkz. [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure günlük arama özelliğini kullanarak uyarı oluşturma hakkında daha fazla bilgi için bkz: [uyarıları Azure'da oturum](../monitoring-and-diagnostics/monitor-alerts-unified-log.md).

* Web kancası kullanarak runbook’ları tetikleme hakkında bilgi almak için bkz. [Azure Otomasyonu web kancaları](automation-webhooks.md).
