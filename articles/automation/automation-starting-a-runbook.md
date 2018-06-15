---
title: Azure Otomasyonu runbook başlatma
description: Azure portalı ve Windows PowerShell kullanma hakkında ayrıntılar verilmiştir ve Azure Otomasyon runbook'u başlatmak için kullanılır farklı yöntemleri özetler.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 0bc414d42acd665e52f3f76037dffe225344b23d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195303"
---
# <a name="starting-a-runbook-in-azure-automation"></a>Azure Otomasyonu runbook başlatma
Aşağıdaki tabloda, Azure automation'da belirli senaryonuza en uygun bir runbook'u başlatmak için bu yöntem belirlemenize yardımcı olur. Bu makalede Azure portal ile ve Windows PowerShell ile bir runbook'u başlatma hakkında bilgi içerir. Aşağıdaki bağlantılardan erişebilirsiniz diğer belgelerinde diğer yöntemler hakkında ayrıntılar verilmiştir.

| **YÖNTEMİ** | **ÖZELLİKLERİ** |
| --- | --- |
| [Azure Portal](#starting-a-runbook-with-the-azure-portal) |<li>En basit yöntem etkileşimli kullanıcı arabirimi ile.<br> <li>Basit parametre değerlerini sağlamak için form.<br> <li>İş durumu kolayca izleyin.<br> <li>İle Azure oturum açma kimliği doğrulanmış erişim. |
| [Windows PowerShell](https://msdn.microsoft.com/library/dn690259.aspx) |<li>Komut satırından Windows PowerShell cmdlet'leri ile çağırın.<br> <li>Birden çok adımı otomatik Çözümle eklenebilir.<br> <li>Sertifika veya OAuth kullanıcı asıl / hizmet isteği kimliği doğrulanır asıl.<br> <li>Basit ve karmaşık parametre değerlerini sağlayın.<br> <li>İş durumu izleyin.<br> <li>PowerShell cmdlet'leri desteklemek için gereken istemci. |
| [Azure Otomasyonu API](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li>En esnek yöntem, ancak ayrıca en karmaşık.<br> <li>HTTP isteği yapabilen dilediğiniz özel kodu çağırın.<br> <li>İstek sertifika ya da Oauth kullanıcı asıl / hizmet kimliği doğrulanmış sorumlu.<br> <li>Basit ve karmaşık parametre değerlerini sağlayın. *API kullanarak bir Python runbook arıyorsanız, JSON yükü seri hale getirilmesi gerekir.*<br> <li>İş durumu izleyin. |
| [Web kancaları](automation-webhooks.md) |<li>Runbook tek HTTP isteğinden başlatın.<br> <li>Güvenlik belirteci URL ile kimlik doğrulaması.<br> <li>İstemci Web kancası oluşturduğunuzda belirtilen parametre değerleri geçersiz kılamaz. Runbook ile HTTP istek ayrıntıları doldurulmuş tek bir parametre tanımlayabilirsiniz.<br> <li>Web kancası URL'si aracılığıyla iş durumu izleme yeteneği yok. |
| [Azure uyarısına yanıt](../log-analytics/log-analytics-alerts.md) |<li>Azure uyarı yanıtta bir runbook başlatın.<br> <li>Web kancası runbook ve uyarı için bağlantı için yapılandırın.<br> <li>Güvenlik belirteci URL ile kimlik doğrulaması. |
| [Zamanlama](automation-schedules.md) |<li>Saatlik, günlük, haftalık veya aylık zamanlamaya göre otomatik olarak runbook başlatın.<br> <li>Azure portal, PowerShell cmdlet'leri veya Azure API aracılığıyla zamanlama yönetme.<br> <li>Zamanlama ile kullanılacak parametre değerlerini sağlayın. |
| [Başka bir Runbook'tan](automation-child-runbooks.md) |<li>Bir runbook başka bir runbook'taki bir etkinlik olarak kullanın.<br> <li>Birden çok runbook tarafından kullanılan işlevselliği için kullanışlıdır.<br> <li>Alt runbook parametre değerlerini sağlayın ve çıktı üst runbook'ta kullanın. |

Aşağıdaki resimde bir runbook yaşam döngüsünü ayrıntılı adım adım işlemi gösterilmektedir. Bir runbook Azure Otomasyon karma Runbook çalışanı için gerekli bileşenleri Azure Otomasyon çalışma kitabı ve farklı bileşenler arasındaki etkileşimler yürütmek için başlatılan farklı yolları içerir. Otomasyon runbook'ları, veri merkezinizde çalıştırma hakkında bilgi edinmek için bkz [karma runbook çalışanları](automation-hybrid-runbook-worker.md)

![Runbook mimarisi](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-the-azure-portal"></a>Azure portalıyla bir runbook'u başlatma
1. Azure portalında seçin **Otomasyon** ve ardından bir Otomasyon hesabı adını tıklatın.
2. Hub menüsünde seçin **Runbook'lar**.
3. Üzerinde **Runbook'lar** sayfasında, bir runbook seçin ve ardından **Başlat**.
4. Runbook'un parametreleri varsa her parametre için bir metin kutusu değerlerini sağlamak için istenir. Bkz: [Runbook parametreleri](#Runbook-parameters) altında daha fazla ayrıntı için parametreleri.
5. Üzerinde **iş** sayfası, runbook işinin durumunu görüntüleyebilirsiniz.

## <a name="starting-a-runbook-with-windows-powershell"></a>Windows PowerShell ile bir runbook başlatılıyor
Kullanabileceğiniz [başlangıç AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) Windows PowerShell ile bir runbook'u başlatın. Aşağıdaki örnek kod, Test-Runbook adlı bir runbook başlatır.

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Başlangıç AzureRmAutomationRunbook runbook başlatıldıktan sonra durumunu izlemek için kullanabileceğiniz bir iş nesnesi döndürür. Bu iş nesnesi ile sonra kullanabileceğiniz [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) işin durumunu belirlemek için ve [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) çıktısını almak için. Aşağıdaki örnek kod, Test-Runbook, tamamlandı ve ardından çıktısını görüntüler tamamlanmasını bekler adlı bir runbook başlatır.

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Runbook'un parametreler gerektirmesi durumunda bunları sağlamanız gereken bir [hashtable](http://technet.microsoft.com/library/hh847780.aspx) burada karma tablosu anahtarının parametre adıyla eşleştiği ve değerin parametre değeridir. Aşağıdaki örnek, FirstName ve LastName, RepeatCount adlı bir tamsayı ve Show adlı bir boolean parametresiyle adlı iki dize parametresi ile bir runbook başlatın gösterilmektedir. Parametreler hakkında daha fazla bilgi için bkz: [Runbook parametreleri](#Runbook-parameters) aşağıda.

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook parametreleri
Azure portal ya da Windows PowerShell bir runbook'u başlattığınızda talimat Azure Otomasyonu web hizmeti aracılığıyla gönderilir. Bu hizmet, karmaşık veri türleriyle parametreleri desteklemiyor. Karmaşık bir parametre için bir değer sağlanması gerekiyor durumunda, satır içi başka bir runbook'tan açıklandığı gibi çağırmalısınız [alt runbook'ları Azure Automation](automation-child-runbooks.md).

Azure Otomasyonu web hizmeti aşağıdaki bölümlerde açıklandığı gibi belirli veri türlerini kullanarak parametreler için özel işlevler sağlar:

### <a name="named-values"></a>Adlandırılmış değerler
Veri türü [object] parametredir sonra adlandırılmış değerler listesini göndermek için şu JSON biçimini kullanabilirsiniz: *{Ad1: 'Değer1', ad2: 'Değer2', AD3: 'Değer3'}*. Bu değerler basit türler olmalıdır. Runbook parametre alan bir [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) her adlandırılmış değerine karşılık gelen özelliklere sahip.

Kullanıcı adında bir parametre kabul eden aşağıdaki sınama runbook'unu göz önünde bulundurun.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

Aşağıdaki metin kullanıcı parametresi için kullanılabilir.

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Bu durum şunlara sebep olur:

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Diziler
Parametresi [array] gibi bir dizi olup olmadığını veya [string []] değerlerinin listesini göndermek için şu JSON biçimini kullanabilirsiniz: *[Value1, Value2, Value3]*. Bu değerler basit türler olmalıdır.

Adlı bir parametre kabul eden aşağıdaki sınama runbook'unu göz önünde bulundurun *kullanıcı*.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

Aşağıdaki metin kullanıcı parametresi için kullanılabilir.

```
["Joe","Smith",2,true]
```

Bu durum şunlara sebep olur:

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Kimlik Bilgileri
Parametre veri türü ise **PSCredential**, bir Azure Otomasyonu adını sağlayın ve sonra [kimlik bilgisi varlığı](automation-credentials.md). Runbook Belirttiğiniz ada sahip kimlik bilgisi alır.

Kimlik bilgisi adlı bir parametre kabul eden aşağıdaki sınama runbook'unu göz önünde bulundurun.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

Aşağıdaki metin kullanıcı parametresi adlı bir kimlik bilgisi varlığı olduğunu varsayarak için kullanılabilecek *My kimlik bilgisi*.

```
My Credential
```

Kullanıcı kimlik bilgisi olduğu kabul *jsmith*, bu durum şunlara sebep olur:

```
jsmith
```

## <a name="next-steps"></a>Sonraki adımlar
* Geçerli makale runbook mimarisinde kaynakları yöneten runbook Azure ve şirket içi karma Runbook çalışanı ile üst düzey bir genel bakış sağlar. Otomasyon runbook'ları, veri merkezinizde çalıştırma hakkında bilgi edinmek için bkz [karma Runbook çalışanları](automation-hybrid-runbook-worker.md).
* Özel veya ortak işlevleri için diğer runbook'lar tarafından kullanılacak oluşturma modüler runbook'lar hakkında daha fazla bilgi için bkz [alt runbook'ları](automation-child-runbooks.md).

