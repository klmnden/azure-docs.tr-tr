---
title: "Bir Log Analytics Uyarısından Azure Otomasyonu Runbook’u Çağırma | Microsoft Docs"
description: "Bu makalede bir Microsoft OMS Log Analytics uyarısından Otomasyon runbook’u çağırma işlemine genel bakış sunulmaktadır."
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
ms.openlocfilehash: 0c0b15f33a177afc70a3662c5bd008eb236ed0d6
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Bir OMS Log Analytics uyarısından Azure Otomasyonu runbook’u çağırma

Log Analytics'te, sonuçların işlemci kullanımında uzun süren bir ani değişiklik ya da bir iş uygulamasının işlevselliği için kritik olan bir uygulama işleminin başarısız olması ve Windows olay günlüğüne karşılık gelen bir olay yazması gibi ölçütlerle eşleşmesi durumunda uyarı kaydı oluşturmaya yönelik bir uyarı yapılandırıldığında bu uyarı, sorunu otomatik olarak düzeltme girişimiyle bir Otomasyon runbook'unu otomatik olarak çalıştırabilir.  

Uyarı yapılandırırken runbook'u çağırmak için iki seçenek vardır:

1. Web kancası kullanma.
   * OMS çalışma alanınız bir Otomasyon hesabına bağlı değilse kullanılabilecek tek seçenek budur.
   * OMS çalışma alanına bağlı bir Otomasyon hesabınız varsa bu seçenek yine de kullanılabilir.  

2. Doğrudan bir runbook seçme.
   * Bu seçenek yalnızca OMS çalışma alanı bir Otomasyon hesabına bağlı olduğunda kullanılabilir.

## <a name="calling-a-runbook-using-a-webhook"></a>Web kancası kullanarak bir runbook çağırma

Web kancası tek bir HTTP isteği ile Azure Otomasyonu’nda belirli bir runbook başlatmanıza olanak tanır. Web kancası kullanarak bir uyarı eylemi olarak runbook'u çağırmak üzere [Log Analytics uyarısını](../log-analytics/log-analytics-alerts.md#alert-rules) yapılandırmadan önce, ilk olarak bu yöntem kullanılarak çağrılacak runbook için bir web kancası oluşturmanız gerekir. [Web kancası oluşturma](automation-webhooks.md#creating-a-webhook) makalesindeki adımları uygulayın ve uyarı kuralını yapılandırırken başvurabilmeniz için web kancası URL'sini kaydetmeyi unutmayın.   

## <a name="calling-a-runbook-directly"></a>Doğrudan bir runbook çağırma

OMS çalışma alanınızda yüklü ve yapılandırılmış bir Otomasyon ve Denetim teklifiniz varsa, uyarı için Runbook eylemleri seçeneğini yapılandırırken tüm runbook'ları **Runbook seç** açılır listesinden görüntüleyebilir ve uyarıya yanıt olarak çalıştırmak istediğiniz runbook'u seçebilirsiniz. Seçili runbook, Azure bulutundaki bir çalışma alanında veya bir karma runbook çalışanında çalışabilir. Runbook seçeneği kullanılarak uyarı oluşturulduktan sonra, runbook için bir web kancası oluşturulur. Otomasyon hesabına gidip seçili runbook'un web kancası bölmesine ulaştığınızda web kancasını görebilirsiniz. Uyarıyı silerseniz web kancası silinmez, ancak kullanıcı web kancasını el ile silebilir. Web kancasının silinmemesi sorun değildir; yalnızca Otomasyon hesabının düzenini korumak için sonunda silinmesi gereken yalnız bırakılmış bir öğedir.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Bir runbook’un özellikleri (her iki seçenek için)

Log Analytics uyarısından runbook çağırmaya yönelik her iki yöntem de uyarı kurallarınızı yapılandırmadan önce anlaşılması gereken farklı davranış özelliklerine sahiptir. Uyarı verileri JSON biçimindedir ve **SearchResult** adlı tek bir özellik içinde yer alır. Bu biçim, standart yüke sahip runbook ve web kancası eylemlerine yöneliktir. **RequestBody** içinde **IncludeSearchResults:True** dahil olan özel yüklere yönelik web kancası işlemleri için özellik, **SearchResults**’tur.

* **Object** türünde **WebhookData** adlı bir runbook girdi parametreniz olmalıdır. Zorunlu veya isteğe bağlı olabilir. Uyarı bu girdi parametresini kullanarak arama sonuçlarını runbook’a geçirir.

    ```powershell
    param  
        (  
        [Parameter (Mandatory=$true)]  
        [object] $WebhookData  
        )
    ```
*  WebhookData’yı bir PowerShell nesnesine dönüştürmek için kodunuz olmalıdır.

    ```powershell
    $SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
    ```

    *$SearchResults* bir nesne dizisidir; her nesne bir arama sonucunun değerlerini içeren alanlardan oluşur


## <a name="example-walkthrough"></a>Örnek kılavuz

Bir Windows hizmetini başlatan aşağıdaki örnek grafiksel runbook kullanılarak bu işlemin nasıl çalıştığı gösterilecektir.<br><br> ![Windows Hizmeti Grafiksel Runbook’unu Başlatma](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Runbook, **Object** türünde **WebhookData** adlı bir giriş parametresini ve \*.SearchResults\* öğesini içeren uyarıdan geçirilmiş web kancası verilerini içerir.<br><br> ![Runbook girdi parametreleri](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

Bu örnek için, hizmet görünen adını ve hizmetin durumunu (çalışıyor veya durduruldu) Sistem olay günlüğüne yazılmış olaydan ayıklamak amacıyla Log Analytics’te \*SvcDisplayName\_CF\* ve \*SvcState\_CF\* adlı iki özel alan oluşturulmuştur. Ardından şu arama sorgusuyla bir uyarı kuralı oluşturulur: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"`; böylece, Windows sisteminde Yazdırma Biriktiricisi hizmetinin ne zaman durdurulduğu saptanabilir. Bu herhangi bir ilgili hizmet olabilir; ancak bu örnekte Windows OS ile birlikte sunulan önceden var olan hizmetlerden birine başvurulmaktadır. Uyarı eylemi, bu örnekte kullanılan runbook'u yürütecek ve hedef sistemlerde etkinleştirilmiş Karma Runbook Çalışanı üzerinde çalışacak şekilde yapılandırılmıştır.   

**LA'dan Hizmet Adını Al** adlı runbook kod etkinliği, JSON ile biçimlendirilmiş dizeyi bir nesne türüne dönüştürür ve Windows hizmetinin görünen adını ayıklayıp hizmeti yeniden başlatmayı denemeden önce hizmetin durdurulduğunu doğrulayan sonraki etkinliğe geçirmek amacıyla \*SvcDisplayName\_CF\* öğesini filtreler. *SvcDisplayName_CF*, Log Analytics’te bu örneği göstermek üzere oluşturulmuş bir [özel alandır](../log-analytics/log-analytics-custom-fields.md).

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

Hizmet durdurulduğunda, Log Analytics'teki uyarı kuralı bir eşleşme saptar ve runbook’u tetikleyip uyarı bağlamını runbook'a gönderir. Runbook, hizmetin durdurulduğunu doğrulamak için işlem yapar ve durdurulmuşsa hizmeti yeniden başlatmayı deneyip doğru başlatıldığını onaylar ve sonuçları gönderir.     

Otomasyon hesabınız OMS çalışma alanınıza bağlı değilse alternatif olarak, runbook’u tetiklemenin yanı sıra JSON ile biçimlendirilmiş dizeyi dönüştürecek ve daha önce bahsedilen adımlarla \*.SearchResult\* öğesini filtreleyecek şekilde yapılandırmak için, uyarı kuralını bir web kancası ile yapılandırabilirsiniz.    

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](../log-analytics/log-analytics-log-search-upgrade.md) yükseltilmişse ağ kancası yükü değiştirilmiştir.  Biçimle ilgili ayrıntılar için bkz. [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Sonraki adımlar

* Log Analytics’teki uyarılar ve bir uyarı oluşturma hakkında daha fazla bilgi için bkz. [Log Analytics’teki Uyarılar](../log-analytics/log-analytics-alerts.md).

* Web kancası kullanarak runbook’ları tetikleme hakkında bilgi almak için bkz. [Azure Otomasyonu web kancaları](automation-webhooks.md).
