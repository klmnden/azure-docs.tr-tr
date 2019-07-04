---
title: Azure Automation'da bir runbook başlatın
description: Azure Automation'da bir runbook başlatmak için kullanılan ve Azure portalı ve Windows PowerShell kullanarak Ayrıntılar sağlayan farklı yöntemlere özetler.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 13af62c52750b1a3684351156b981112b7f7b748
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477548"
---
# <a name="start-a-runbook-in-azure-automation"></a>Azure Automation'da bir runbook başlatın

Aşağıdaki tabloda kendi belirli senaryonuza en uygun Azure automation'da bir runbook başlatma yöntemi belirlemenize yardımcı olur. Bu makale, Azure portalı ve Windows PowerShell ile bir runbook başlatma hakkında bilgi içerir. Aşağıdaki bağlantılardan erişebileceğiniz diğer belgeler diğer yöntemler hakkında ayrıntılı bilgi sağlanır.

| **Yöntemi** | **Özellikleri** |
| --- | --- |
| [Azure portal](#start-a-runbook-with-the-azure-portal) |<li>En basit yöntem etkileşimli kullanıcı arabirimi.<br> <li>Basit parametre değerlerini sağlamak için formu.<br> <li>İş durumu kolayca izleyin.<br> <li>Azure oturum ile kimliği doğrulanmış erişim içinde. |
| [Windows PowerShell](/powershell/module/azurerm.automation/start-azurermautomationrunbook) |<li>Windows PowerShell cmdlet'lerle komut satırından çağırın.<br> <li>Birden çok adım ile otomatik çözüm eklenebilir.<br> <li>İstek kimliği doğrulanır ve sertifika veya OAuth kullanıcı asıl / hizmet sorumlusu.<br> <li>Basit ve karmaşık parametre değerlerini sağlayın.<br> <li>İş durumu izleyin.<br> <li>PowerShell cmdlet'leri desteklemek için gereken istemci. |
| [Azure Otomasyonu API](/rest/api/automation/) |<li>En esnek yöntem, ancak ayrıca en karmaşık.<br> <li>HTTP isteği yapabilen herhangi özel kodu çağırın.<br> <li>İstek doğrulanmış sertifika veya Oauth kullanıcı asıl / hizmet sorumlusu.<br> <li>Basit ve karmaşık parametre değerlerini sağlayın. *API kullanarak bir Python runbook'u arıyoruz, JSON yükü seri hale getirilmelidir.*<br> <li>İş durumu izleyin. |
| [Web kancaları](automation-webhooks.md) |<li>Tek HTTP isteğinden runbook'u başlatın.<br> <li>Güvenlik belirteci URL ile kimlik doğrulaması.<br> <li>İstemci Web kancasını oluşturduğunuzda belirtilen parametre değerleri geçersiz kılamaz. Runbook ile HTTP istek ayrıntılarını doldurulur tek bir parametre tanımlayabilirsiniz.<br> <li>Web kancası URL'si ile iş durumunu izlemek için özelliği yok. |
| [Azure uyarıya yanıt](../log-analytics/log-analytics-alerts.md) |<li>Azure uyarıya yanıt olarak bir runbook'u başlatın.<br> <li>Runbook ve sizi uyarmak için bağlantısı için Web kancası yapılandırın.<br> <li>Güvenlik belirteci URL ile kimlik doğrulaması. |
| [Zamanlama](automation-schedules.md) |<li>Otomatik olarak saatlik, günlük, haftalık veya aylık zamanlamaya göre runbook'u başlatın.<br> <li>Azure portalı, PowerShell cmdlet'leri ve Azure API aracılığıyla zamanlama işleyin.<br> <li>Zamanlama ile kullanılacak parametre değerlerini sağlayın. |
| [Başka bir Runbook'tan](automation-child-runbooks.md) |<li>Bir runbook başka bir runbook'taki bir etkinlik olarak kullanın.<br> <li>Birden çok runbook'ları tarafından kullanılan işlevselliği için kullanışlıdır.<br> <li>Alt runbook parametre değerlerini sağlayın ve çıkış üst runbook'ta kullanın. |

Aşağıdaki resimde bir runbook yaşam döngüsünü ayrıntılı adım adım işlemi gösterilmektedir. Azure Automation'da bir runbook başlatır farklı yolları içerir karma Runbook çalışanı'ı, Azure Otomasyonu runbook'ları ve farklı bileşenler arasındaki etkileşimler yürütmek gereken hangi bileşenleri. Veri merkezinizde Otomasyon runbook'ları çalıştırma hakkında bilgi edinmek için bkz [karma runbook çalışanları](automation-hybrid-runbook-worker.md)

![Runbook mimarisi](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="start-a-runbook-with-the-azure-portal"></a>Azure portalı ile bir runbook başlatın

1. Azure portalında **Otomasyon** ve ardından bir Otomasyon hesabının adına tıklayın.
2. Hub menüsünde **runbook'ları**.
3. Üzerinde **runbook'ları** sayfasında, bir runbook seçin ve ardından **Başlat**.
4. Runbook'un parametreleri varsa her parametre için bir metin kutusuyla birlikte değerler sağlamak için istenir. Parametreler hakkında daha fazla bilgi için bkz. [Runbook parametreleri](#runbook-parameters).
5. Üzerinde **iş** sayfasında, runbook işinin durumunu görüntüleyebilirsiniz.

## <a name="start-a-runbook-with-powershell"></a>PowerShell ile bir runbook başlatma

Kullanabileceğiniz [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) Windows PowerShell ile bir runbook'u başlatın. Aşağıdaki örnek kod, Test-Runbook adlı bir runbook başlatır.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Start-AzureRmAutomationRunbook runbook başlatıldıktan sonra durumunu izlemek için kullanabileceğiniz bir iş nesnesi döndürür. Ardından ile bu iş nesnesini kullanabilirsiniz [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) işin durumunu belirlemek için ve [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) çıktısını almak için. Aşağıdaki örnek kod, Test-Runbook, tamamlandı ve ardından çıktısını görüntüler kadar bekler adlı bir runbook başlatır.

```azurepowershell-interactive
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

Runbook parametre gerektiriyor sonra olarak sağlamanız gereken bir [hashtable](https://technet.microsoft.com/library/hh847780.aspx). Karma tablosu anahtarının parametre adıyla eşleşmelidir ve değerin parametre değeri olduğu. Aşağıdaki örnek, FirstName ve LastName, RepeatCount adlı bir tamsayı ve Show adlı bir Boole parametresi adlı iki dize parametre ile bir runbook başlatmak gösterilmektedir. Parametreler hakkında daha fazla bilgi için bkz. [Runbook parametreleri](#runbook-parameters) aşağıda.

```azurepowershell-interactive
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook parametreleri

Azure portalı ya da Windows PowerShell bir runbook'u başlattığınızda talimat Azure Otomasyonu web hizmeti aracılığıyla gönderilir. Bu hizmet, karmaşık veri türleriyle parametreleri desteklemiyor. Karmaşık bir parametre için bir değer sağlamanız gerekir. ardından, satır içi başka bir runbook'tan açıklandığı çağırmalısınız [Azure automation'da alt runbook'lar](automation-child-runbooks.md).

Azure Otomasyonu web hizmeti, aşağıdaki bölümlerde açıklandığı gibi belirli veri türlerini kullanarak parametreler için özel işlevleri sağlar:

### <a name="named-values"></a>Adlandırılmış değerler

Veri türü [object] parametresi olan sonra görünen değerlerin bir listesini buna göndermek için şu JSON biçimini kullanabilirsiniz: *{Name1: 'Value1', Name2: 'Value2', Name3: 'Value3'}* . Bu değerler basit türler olmalıdır. Runbook parametre olarak alan bir [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject) her görünen değere karşılık gelen özelliklerle birlikte.

Kullanıcı adlı bir parametreyi kabul eden aşağıdaki sınama runbook'unu göz önünde bulundurun.

```powershell
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

```json
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Bu, aşağıdaki çıktı olur:

```output
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Diziler

Parametre, [dizi] gibi bir dizi olup olmadığını veya [string []], değerlerin bir listesini buna göndermek için şu JSON biçimini kullanabilirsiniz: *[Value1, Value2, Value3]* . Bu değerler basit türler olmalıdır.

Adlı bir parametreyi kabul eden aşağıdaki sınama runbook'unu göz önünde bulundurun *kullanıcı*.

```powershell
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

```input
["Joe","Smith",2,true]
```

Bu, aşağıdaki çıktı olur:

```output
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Kimlik Bilgileri

Parametre veri türü ise **PSCredential**, bir Azure Otomasyonu adını sağlayabilirsiniz [kimlik bilgisi varlığı](automation-credentials.md). Runbook belirttiğiniz adla bir kimlik bilgisi alır.

Kimlik bilgisi adlı bir parametreyi kabul eden aşağıdaki sınama runbook'unu göz önünde bulundurun.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

Aşağıdaki metin kullanıcı parametresi adlı bir kimlik bilgisi varlığı olduğunu için kullanılabilecek *My kimlik bilgisi*.

```input
My Credential
```

Kullanıcı adı kimlik varsayılarak olan *jsmith*, bu aşağıdaki çıktı olur:

```output
jsmith
```

## <a name="next-steps"></a>Sonraki adımlar

* Geçerli makaledeki runbook mimarisi, Azure'da ve şirket içi karma Runbook çalışanı ile yönetme kaynakları runbook'ların üst düzey bir genel bakış sağlar. Veri merkezinizde Otomasyon runbook'ları çalıştırma hakkında bilgi edinmek için bkz [karma Runbook çalışanları](automation-hybrid-runbook-worker.md).
* Özel veya genel işlevler için diğer runbook'lar tarafından kullanılacak oluşturma modüler runbook'lar hakkında daha fazla bilgi için bkz [alt runbook'ları](automation-child-runbooks.md).
