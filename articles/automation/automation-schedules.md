---
title: Azure Otomasyon zamanlamaları
description: Otomasyon zamanlamaları runbook'ları otomatik olarak başlatmak için Azure automation'da zamanlamak için kullanılır. Oluşturma ve belirli bir zamanda veya yinelenen bir zamanlamaya göre otomatik olarak bir runbook başlatabilirsiniz bir zamanlama yönetme açıklar.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6c7bd4d4249d304ee7c1df4ae4b8fc0af476b99c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure Otomasyonu’nda runbook zamanlama

Belirli bir zamanda başlatmak için Azure automation'da bir runbook'u zamanlamak için bir veya daha fazla zamanlamaya bağlayın. Azure portalında runbook'lar için bir kez veya bir yinelenmeye saatlik çalıştırma ya da günlük zamanlama için bir zamanlama yapılandırılabilir. Ayrıca bunları için haftalık, aylık, haftanın belirli gün veya ayın günleri veya ayın belirli zamanlayabilirsiniz. Bir runbook için birden çok zamanlama bağlanabilir ve bir zamanlama birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamaları, şu anda Azure Otomasyonu DSC yapılandırmaları desteklemez.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell cmdlet'leri

Aşağıdaki tabloda yer alan cmdlet'ler oluşturmak ve zamanlamaları Azure automation'da Windows PowerShell ile yönetmek için kullanılır. Bir parçası olarak sevk [Azure PowerShell Modülü](/powershell/azure/overview).

| Cmdlet'leri | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Bir zamanlama alır. |
| [AzureRmAutomationSchedule yeni](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Yeni bir zamanlama oluşturur. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Bir zamanlama kaldırır. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Varolan bir zamanlamanın özelliklerini ayarlar. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Zamanlanmış runbook'lar alır. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Bir runbook zamanlama ile ilişkilendirir. |
| [Kaydı AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Zamanlama runbook'tan dissociates. |

## <a name="creating-a-schedule"></a>Bir zamanlama oluşturma

Azure portalında veya Windows PowerShell ile runbook'lar için yeni bir zamanlama oluşturabilirsiniz.

> [!NOTE]
> Yeni bir zamanlanan iş çalıştırdığınızda azure Automation Otomasyon hesabınızda son modülleri kullanır.  Runbook'larınızı ve bunlar otomatikleştirmek işlemlerini etkileyen önlemek için önce zamanlamaları test etmek için adanmış bir Otomasyon hesabı ile bağlantılı runbook'ları test etmeniz gerekir.  Bu, zamanlanmış doğrulayan runbook'lar düzgün çalışmaya devam eder ve değilse, daha fazla sorun giderme ve güncelleştirilmiş runbook sürümü üretime geçirmeden önce gerekli değişiklikleri uygulamak.
> Bunları el ile seçerek güncelleştirdikten sürece, Automation hesabınız modülleri yeni sürümlerini otomatik olarak almaz [Update Azure modülleri](automation-update-azure-modules.md) gelen seçeneği **modülleri**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Azure portalında yeni bir zamanlama oluşturmak için

1. Azure portalından automation hesabınızı seçin **zamanlamaları** bölümünün altında **paylaşılan kaynakları** soldaki.
1. Tıklatın **bir zamanlama Ekle** sayfanın üst kısmındaki.
1. Üzerinde **yeni zamanlama** bölmesinde, bir **adı** ve isteğe bağlı olarak bir **açıklama** yeni zamanlama için.
1. Bir kez çalıştırılacağı olup olmadığını veya bir yeniden zamanlamada seçerek **kez** veya **yineleme**. Seçerseniz **kez** belirtin bir **başlangıç zamanı**ve ardından **oluşturma**. Seçerseniz **yineleme**, belirtin bir **başlangıç zamanı** ve ne sıklıkta runbook - göre yinelemek istediğiniz sıklığını **saat**, **gün**, **hafta**, ya da **ay**. Seçerseniz **hafta** veya **ay** aşağı açılan listeden **yineleme seçeneği** bölmesinde ve seçime bağlı görünür **yineleme seçeneği** bölmesinde sunulur ve seçtiyseniz, haftanın günü seçebilirsiniz **hafta**. Seçtiyseniz **ay**, tarafından seçebilirsiniz **haftanın günlerini** veya takvim ayın belirli günlerine ve son olarak, çalıştırmak ayın son gününde veya ve ardından istiyor musunuz **Tamam**.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Windows PowerShell ile yeni bir zamanlama oluşturmak için

Kullandığınız [yeni AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) tabloları oluşturmak için cmdlet'i. Zamanlama ve çalışmalı sıklığı için başlangıç saatini belirtin.

Aşağıdaki örnek komutlar bir Azure Resource Manager cmdlet'i kullanılarak her ayın 30 ve 15'inden için bir zamanlama oluşturmak nasıl gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
$scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
-DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
```

## <a name="linking-a-schedule-to-a-runbook"></a>Bir runbook'a bir zamanlama bağlama

Bir runbook için birden çok zamanlama bağlanabilir ve bir zamanlama birden çok runbook bağlı olabilir. Bir runbook'un parametreleri varsa, daha sonra değerleri kendileri için sağlayabilirsiniz. Zorunlu parametreler için değer sağlamalısınız ve isteğe bağlı parametreler için değerler sağlayabilir. Bu zamanlama tarafından runbook her başlatıldığında bu değerler kullanılır. Aynı runbook başka bir plana bağlayın ve farklı parametre değerlerini belirtin.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portal ile bir runbook için bir zamanlama bağlamak için

1. Azure portalından automation hesabınızı seçin **Runbook'lar** bölümünün altında **işlem Otomasyonu** soldaki.
1. Zamanlamak için runbook'un adına tıklayın.
1. Ardından runbook şu anda bir zamanlamaya bağlı değilse yeni bir zamanlama ya da mevcut bir zamanlamayı bağlantısını oluşturmak için seçeneği sunulur.
1. Runbook parametrelere sahipse, seçeneğini seçebilmenize **çalıştırma ayarlarını (varsayılan: Azure) Değiştir** ve **parametreleri** bölmesinde bilgileri buna göre girebilecekleri sunulur.

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Windows PowerShell ile bir runbook için bir zamanlama bağlamak için

Kullanabileceğiniz [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet'ini bir zamanlama Bağla. Parametreleri parametresiyle runbook'un parametreleri için değerler belirtebilirsiniz. Parametre değerleri belirtme hakkında daha fazla bilgi için bkz: [Azure Otomasyonu Runbook başlatma](automation-starting-a-runbook.md).
Aşağıdaki örnek komutlar parametrelere sahip bir Azure Resource Manager cmdlet'i kullanılarak bir runbook'a bir zamanlama Bağla gösterilmektedir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Runbook'ları daha sık planlama

Bir Azure Otomasyonu zamanlama için yapılandırılmış en sık rastlanan aralığı bir saattir. Daha sık, yürütmek için zamanlama gerektiriyorsa, iki seçenek vardır:

* Oluşturma bir [Web kancası](automation-webhooks.md) kullanın ve runbook için [Azure Scheduler](../scheduler/scheduler-get-started-portal.md) Web kancası çağırmak için. Azure Zamanlayıcı, bir zamanlama tanımlarken daha ayrıntılı ayrıntı düzeyi sağlar.

* Dört zamanlamaları, tüm başlangıç 15 dakika içinde birbiriyle saatte çalıştıran oluşturun. Bu senaryo, runbook'un her 15 dakikada ile farklı zamanlama çalıştırılmasına izin verir.

## <a name="disabling-a-schedule"></a>Bir zamanlama devre dışı bırakma

Bir zamanlama devre dışı bıraktığınızda, artık bağlı herhangi bir runbook bu zamanlamaya göre çalışır. El ile bir zamanlama devre dışı bırakmak veya bunları oluşturduğunuzda bir sıklığı ile zamanlama için sona erme süresi ayarlayın. Sona erme zamanı geldiğinde, zamanlaması devre dışı bırakıldı.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure portalından bir zamanlama devre dışı bırakmak için

1. Azure portalından Automation hesabınızı seçin **zamanlamaları** bölümünün altında **paylaşılan kaynakları** soldaki.
1. Ayrıntılar bölmesini açmak için bir zamanlama adına tıklayın.
1. Değişiklik **etkin** için **Hayır**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Windows PowerShell ile bir zamanlama devre dışı bırakmak için

Kullanabileceğiniz [kümesi AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) varolan bir zamanlama özelliklerini değiştirmek için cmdlet. Zamanlama devre dışı bırakmak için belirtmek **false** için **IsEnabled** parametresi.

Aşağıdaki örnek komutlar bir Azure Resource Manager cmdlet'i kullanılarak bir runbook için bir zamanlama devre dışı bırakma göstermektedir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu runbook'ları kullanmaya başlamak için bkz: [Azure Otomasyonu Runbook başlatma](automation-starting-a-runbook.md)