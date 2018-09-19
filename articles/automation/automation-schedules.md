---
title: Azure automation'da zamanlamaları
description: Otomasyon zamanlamaları runbook'ları otomatik olarak başlayacak şekilde, Azure automation'da zamanlamak için kullanılır. Oluşturmak ve belirli bir zamanda veya yinelenen bir zamanlamaya göre otomatik olarak bir runbook başlatabilirsiniz bir zamanlama yönetmek açıklar.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3d8492d2a8982c9c85bfc91867f7eb6c2da04e58
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294773"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure Otomasyonu’nda runbook zamanlama

Belirli bir zamanda başlatmak için Azure Otomasyonu'nda runbook zamanlama için bir veya daha fazla zamanlamaya bağlayın. Bir zamanlama için runbook'ları Azure portalında çalışma bir kez veya bir yeniden oluşmasını saatlik veya günlük zamanlama için yapılandırılabilir. Ayrıca bunları için haftalık, aylık, haftanın belirli gün veya ayın veya ayın belirli zamanlayabilirsiniz. Bir runbook için birden çok zamanlama bağlanabilir ve bir zamanlama birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamaları, şu anda Azure Automation DSC yapılandırmaları desteklemez.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell cmdlet'leri

Aşağıdaki tabloda yer alan cmdlet'ler oluşturup Azure automation'da Windows PowerShell ile zamanlamaları yönetmek için kullanılır. Bunlar parçası olarak gönderilen [Azure PowerShell Modülü](/powershell/azure/overview).

| Cmdlet'leri | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Bir zamanlama alır. |
| [Yeni-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Yeni bir zamanlama oluşturur. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Bir zamanlama kaldırır. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Mevcut bir zamanlamanın özelliklerini ayarlar. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Zamanlanan runbook'lar alır. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Runbook zamanlama ile ilişkilendirir. |
| [Kaydını AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Zamanlama runbook'tan dissociates. |

## <a name="creating-a-schedule"></a>Bir zamanlama oluşturma

Azure portalında veya Windows PowerShell ile runbook'lar için yeni bir zamanlama oluşturabilirsiniz.

> [!NOTE]
> Yeni bir zamanlanmış iş çalıştırıldığında azure Otomasyonu, Otomasyon hesabınızda en son modüllerini kullanır.  Runbook'larınızı ve bunlar otomatikleştirmek işlemleri etkilemesini önlemek için test etmek için ayrılmış bir Otomasyon hesabı ile bağlı zamanlamaları runbook'ları test etmeniz gerekir.  Bu, zamanlanmış doğrular runbook'ları doğru şekilde çalışmaya devam ve aksi takdirde, daha fazla sorun giderme ve güncelleştirilmiş runbook sürümü üretime geçmeden önce gerekli değişiklikleri uygular.
> Bunları el ile seçerek güncelleştirdikten sürece Otomasyon hesabınızı modüllerinin tüm yeni sürümlere otomatik olarak almaz [Azure modüllerini güncelleştirme](automation-update-azure-modules.md) seçeneğini **modülleri**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Azure portalında yeni bir zamanlama oluşturmak için

1. Azure portalında Otomasyon hesabınızı seçin **zamanlamaları** bölümünde **paylaşılan kaynakları** soldaki.
1. Tıklayın **zamanlama Ekle** sayfanın üstünde.
1. Üzerinde **yeni zamanlama** bölmesinde bir **adı** ve isteğe bağlı olarak bir **açıklama** yeni zamanlama için.
1. Zamanlama bir kez olmasından veya yinelenen bir zamanlamaya göre seçerek **kez** veya **yinelenen**. Seçerseniz **kez** belirtin bir **başlangıç zamanı**ve ardından **Oluştur**. Seçerseniz **yinelenen**, belirtin bir **başlangıç zamanı** ve **Yinele her**, ne sıklıkta runbook - göre yinelemek istediğiniz sıklığını seçin **saat**, **gün**, **hafta**, ya da **ay**.
    1. Seçerseniz **hafta**, aralarından seçim yapabileceğiniz haftanın günlerini listesi sağlanır. İstediğiniz kadar çok gün seçin. İlk çalıştırma zamanlama, seçilen başlangıç zamanından sonraki ilk gününde gerçekleşir.
    2. Seçerseniz **ay**, farklı seçenekler sunulur. İçin **aylık occurrances** seçeneğinde, ya da seçin **ayın günü** veya **haftanın günleri**. Seçerseniz **ayın günü** istediğiniz sayıda gün seçmenize olanak tanıyan bir takvim gösterilir. Geçerli ay içinde oluşmaz 31 gibi bir tarihi seçerseniz, zamanlama çalışmaz. Zamanlamanın son gününde çalıştırmak istiyorsanız seçin **Evet** altında **ayın son gününde Çalıştır**. Seçerseniz **haftanın günleri**, **Yinele her** seçeneği sunulur. Seçin **ilk**, **ikinci**, **üçüncü**, **dördüncü**, veya **son**. Son olarak üzerinde yinelemek için bir günü seçin.
1. Click bittiğinde **Oluştur**.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Windows PowerShell ile yeni bir zamanlama oluşturmak için

Kullandığınız [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) tabloları oluşturmak için cmdlet'i. Zamanlama ve çalıştırılması gereken sıklığı için başlangıç saati belirtirsiniz.

Aşağıdaki örnek komutlar, 15 ve bir Azure Resource Manager cmdlet'ini kullanarak her ayın 30 için bir zamanlama oluşturma işlemini gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
$scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
-DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
```

## <a name="linking-a-schedule-to-a-runbook"></a>Bir runbook'a zamanlama bağlama

Bir runbook için birden çok zamanlama bağlanabilir ve bir zamanlama birden çok runbook bağlı olabilir. Bir runbook'un parametreleri varsa, daha sonra değerleri için bunları sağlayabilirsiniz. Zorunlu parametreler için değerler sağlamanız gerekir ve isteğe bağlı parametreler için değerleri sağlayabilirsiniz. Bu zamanlamayı runbook her başlatıldığında bu değerler kullanılır. Aynı runbook başka bir plana eklemek ve farklı parametre değerlerini belirtin.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portalı ile bir runbook için bir zamanlama bağlamak için

1. Azure portalında Otomasyon hesabınızı seçin **runbook'ları** bölümünde **süreç otomasyonu** soldaki.
2. Zamanlamak için runbook'un adına tıklayın.
3. Runbook şu anda bir zamanlamaya bağlı değilse, yeni zaman çizelgesi veya mevcut bir zamanlamanın bağlantısına oluşturma seçeneği sunulur.
4. Runbook parametrelere sahipse, bu seçeneği seçebilirsiniz **çalıştırma ayarları (varsayılan: Azure) değiştirme** ve **parametreleri** bölmesinde, bilgileri uygun şekilde girebileceğiniz sunulur.

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Windows PowerShell ile bir runbook için bir zamanlama bağlamak için

Kullanabileceğiniz [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) zamanlama bağlamak için cmdlet'i. Parametreleri parametresiyle runbook'un parametreleri için değerleri belirtebilirsiniz. Parametre değerleri belirtme hakkında daha fazla bilgi için bkz. [Azure Automation'da bir Runbook başlatma](automation-starting-a-runbook.md).
Aşağıdaki örnek komutlar parametrelerle bir Azure Resource Manager cmdlet'ini kullanarak bir runbook'a zamanlama bağlama gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Daha sık runbook'ları zamanlama

Bir Azure Otomasyonu zamanlama için yapılandırılmış en sık rastlanan aralığı bir saattir. Zamanlamaları, birden daha sık çalıştırmak için gerekiyorsa, iki seçenek vardır:

* Oluşturma bir [Web kancası](automation-webhooks.md) kullanın ve runbook için [Azure Scheduler](../scheduler/scheduler-get-started-portal.md) Web kancası çağırma. Azure Zamanlayıcı, bir zamanlama tanımlarken daha ayrıntılı ayrıntı düzeyi sağlar.

* 15 dakika içinde birbiriyle saatte bir çalışan tüm başlayarak dört zamanlamaları oluşturun. Bu senaryo, runbook farklı zamanlamalar ile 15 dakikada bir çalıştırılacak sağlar.

## <a name="disabling-a-schedule"></a>Bir zamanlama devre dışı bırakma

Bir zamanlama devre dışı bıraktığınızda, artık bağlı herhangi bir runbook bu zamanlamaya göre çalıştırır. El ile bir zamanlama devre dışı bırakın veya bunları oluşturduğunuz bir sıklık ile zamanlama için sona erme süresi ayarlayın. Zaman aşımı süresine ulaşıldığında, zamanlama devre dışı bırakıldı.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure portalından bir zamanlama devre dışı bırakmak için

1. Azure portalında Otomasyon hesabınızı seçin **zamanlamaları** bölümünde **paylaşılan kaynakları** soldaki.
1. Ayrıntılar bölmesinde açmak için bir zamanlama adına tıklayın.
1. Değişiklik **etkin** için **Hayır**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Windows PowerShell ile bir zamanlama devre dışı bırakmak için

Kullanabileceğiniz [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet'i mevcut bir zamanlamanın özelliklerini değiştirmek için. Zamanlama devre dışı bırakmak için belirtin **false** için **IsEnabled** parametresi.

Aşağıdaki örnek komutlar bir Azure Resource Manager cmdlet'i kullanarak bir runbook'u için zamanlama devre dışı bırakma gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu runbook'ları kullanmaya başlamak için bkz: [Azure Automation'da bir Runbook başlatma](automation-starting-a-runbook.md)
