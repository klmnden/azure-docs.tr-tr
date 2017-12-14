---
title: "Azure Otomasyon zamanlamaları | Microsoft Docs"
description: "Otomasyon zamanlamaları runbook'ları otomatik olarak başlatmak için Azure automation'da zamanlamak için kullanılır. Oluşturma ve belirli bir zamanda veya yinelenen bir zamanlamaya göre otomatik olarak bir runbook başlatabilirsiniz bir zamanlama yönetme açıklar."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 1c2da639-ad20-4848-920b-88e471b2e1d9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: c651ab70977367d0e41364120c89561a04a45cf4
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure Otomasyonu’nda runbook zamanlama
Belirli bir zamanda başlatmak için Azure automation'da bir runbook'u zamanlamak için bir veya daha fazla zamanlamaya bağlayın. Klasik Azure portalı runbook'larda ve runbook'ları Azure portalında bir kez veya bir yinelenmeye saatlik çalıştırma ya da günlük zamanlama için bir zamanlama yapılandırılabilir, ayrıca bunları için haftalık, aylık, haftanın belirli günlerinde veya gün ayın veya ayın belirli zamanlayabilirsiniz.  Bir runbook için birden çok zamanlama bağlanabilir ve bir zamanlama birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamaları, şu anda Azure Otomasyonu DSC yapılandırmaları desteklemez.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell cmdlet'leri
Aşağıdaki tabloda yer alan cmdlet'ler oluşturmak ve zamanlamaları Azure automation'da Windows PowerShell ile yönetmek için kullanılır. Bir parçası olarak sevk [Azure PowerShell Modülü](/powershell/azure/overview).

| Cmdlet'leri | Açıklama |
|:--- |:--- |
| **Azure Resource Manager cmdlet'leri** | |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Bir zamanlama alır. |
| [AzureRmAutomationSchedule yeni](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Yeni bir zamanlama oluşturur. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Bir zamanlama kaldırır. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Varolan bir zamanlamanın özelliklerini ayarlar. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Zamanlanmış runbook'lar alır. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Bir runbook zamanlama ile ilişkilendirir. |
| [Kaydı AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Zamanlama runbook'tan dissociates. |
| **Azure Hizmet Yönetimi cmdlet'leri** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |Bir zamanlama alır. |
| [AzureAutomationSchedule yeni](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |Yeni bir zamanlama oluşturur. |
| [Remove-AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |Bir zamanlama kaldırır. |
| [Set-AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |Varolan bir zamanlamanın özelliklerini ayarlar. |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Zamanlanmış runbook'lar alır. |
| [Register-AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Bir runbook zamanlama ile ilişkilendirir. |
| [Kaydı AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Zamanlama runbook'tan dissociates. |

## <a name="creating-a-schedule"></a>Bir zamanlama oluşturma
Azure portalında Klasik portalında veya Windows PowerShell ile runbook'lar için yeni bir zamanlama oluşturabilirsiniz. Ayrıca bir runbook'u Azure Klasik veya Azure portalını kullanarak bir zamanlamaya bağladığınızda, yeni bir zamanlama oluşturma seçeneğiniz vardır.

> [!NOTE]
> Yeni bir zamanlanan iş çalıştırdığınızda azure Automation Otomasyon hesabınızda son modülleri kullanır.  Runbook'larınızı ve bunlar otomatikleştirmek işlemlerini etkileyen önlemek için önce zamanlamaları test etmek için adanmış bir Otomasyon hesabı ile bağlantılı runbook'ları test etmeniz gerekir.  Bu, zamanlanmış doğrulayan runbook'lar düzgün çalışmaya devam eder ve değilse, daha fazla sorun giderme ve güncelleştirilmiş runbook sürümü üretime geçirmeden önce gerekli değişiklikleri uygulamak.  
>  Bunları el ile seçerek güncelleştirdikten sürece, Automation hesabınız modülleri yeni sürümlerini otomatik olarak almaz [Update Azure modülleri](automation-update-azure-modules.md) gelen seçeneği **modülleri** . 
>  

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Azure portalında yeni bir zamanlama oluşturmak için
1. Azure portalından automation hesabınızı seçin **zamanlamaları** bölümünün altında **paylaşılan kaynakları** soldaki. 
2. Tıklatın **bir zamanlama Ekle** sayfanın üst kısmındaki.
4. Üzerinde **yeni zamanlama** bölmesinde, bir **adı** ve isteğe bağlı olarak bir **açıklama** yeni zamanlama için.
5. Bir kez çalıştırılacağı olup olmadığını veya bir yeniden zamanlamada seçerek **kez** veya **yineleme**.  Seçerseniz **kez** belirtin bir **başlangıç zamanı** ve ardından **oluşturma**.  Seçerseniz **yineleme**, belirtin bir **başlangıç zamanı** ve ne sıklıkta runbook - göre yinelemek istediğiniz sıklığını **saat**, **gün**, **hafta**, ya da **ay**.  Seçerseniz **hafta** veya **ay** aşağı açılan listeden **yineleme seçeneği** bölmesinde ve seçime bağlı görünür **yineleme seçeneği** bölmesinde sunulur ve seçtiyseniz, haftanın günü seçebilirsiniz **hafta**.  Seçtiyseniz **ay**, tarafından seçebilirsiniz **haftanın günlerini** veya takvim ayın belirli günlerine ve son olarak, çalıştırmak ayın son gününde veya ve ardından istiyor musunuz **Tamam**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Klasik Azure portalında yeni bir zamanlama oluşturmak için
1. Azure Klasik portalında Otomasyon seçin ve ardından bir Otomasyon hesabının adını seçin.
2. Seçin **varlıklar** sekmesi.
3. Pencerenin alt kısmındaki tıklatın **ayar Ekle**.
4. Tıklatın **zamanlama Ekle**.
5. Tür a **adı** ve isteğe bağlı olarak bir **açıklama** yeni zamanlama için. Zamanlamanızı çalıştırabilirsiniz **bir kez**, **saatlik**, **günlük**, **haftalık**, veya **aylık**.
6. Belirtin bir **başlangıç saati** ve seçtiğiniz zamanlamanın türüne bağlı olarak diğer seçenekleri.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Windows PowerShell ile yeni bir zamanlama oluşturmak için
Kullanabileceğiniz [yeni AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) Azure Otomasyonu'nda Klasik runbook'lar için yeni bir zamanlama oluşturmak için cmdlet'i veya [yeni AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet'i Azure portalında runbook'lar için. Zamanlama ve çalışmalı sıklığı için başlangıç saatini belirtmeniz gerekir.

Aşağıdaki örnek komutlar bir Azure Resource Manager cmdlet'i kullanılarak her ayın 30 ve 15'inden için bir zamanlama oluşturmak nasıl gösterir.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Aşağıdaki örnek komutlar her gün 3:30 Şöyle bir Azure Hizmet Yönetimi cmdlet'iyle 20 Ocak 2015 tarihinde başlatma sırasında çalışan yeni bir zamanlama oluşturmak nasıl gösterir.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Bir runbook'a bir zamanlama bağlama
Bir runbook için birden çok zamanlama bağlanabilir ve bir zamanlama birden çok runbook bağlı olabilir. Bir runbook'un parametreleri varsa, daha sonra değerleri kendileri için sağlayabilirsiniz. Zorunlu parametreler için değer sağlamalısınız ve isteğe bağlı parametreler için değerler sağlayabilir.  Bu zamanlama tarafından runbook her başlatıldığında bu değerler kullanılır.  Aynı runbook başka bir plana bağlayın ve farklı parametre değerlerini belirtin.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portal ile bir runbook için bir zamanlama bağlamak için
1. Azure portalından automation hesabınızı seçin **Runbook'lar** bölümünün altında **işlem Otomasyonu** soldaki.
2. Zamanlamak için runbook'un adına tıklayın.
3. Ardından runbook şu anda bir zamanlamaya bağlı değilse yeni bir zamanlama ya da mevcut bir zamanlamayı bağlantısını oluşturmak için seçeneği sunulur.  
4. Runbook parametrelere sahipse, seçeneğini seçebilmenize **çalıştırma ayarlarını (varsayılan: Azure) Değiştir** ve **parametreleri** bölmesinde bilgileri buna göre girebilecekleri sunulur.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Klasik Azure portalı ile runbook için bir zamanlama bağlamak için
1. Klasik Azure portalında seçin **Otomasyon** ve ardından bir Otomasyon hesabı adını tıklatın.
2. Seçin **Runbook'lar** sekmesi.
3. Zamanlamak için runbook'un adına tıklayın.
4. Tıklatın **zamanlama** sekmesi.
5. Runbook şu anda bir zamanlamaya bağlı olmayan sonra seçeneği sunulur **yeni bir zamanlama Bağla** veya **varolan bir zamanlama Bağla**.  Runbook şu anda bir zamanlamaya bağlıysa, tıklatın **bağlantı** bu seçeneklerine erişmek için pencerenin altındaki.
6. Runbook'un parametreleri varsa, bunların değerleri istenir.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Windows PowerShell ile bir runbook için bir zamanlama bağlamak için
Kullanabileceğiniz [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) klasik bir runbook'a bir zamanlama bağlamak için veya [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet'i Azure portalında runbook'lar için.  Parametreleri parametresiyle runbook'un parametreleri için değerler belirtebilirsiniz. Bkz: [Azure Otomasyonu Runbook başlatma](automation-starting-a-runbook.md) parametre değerleri belirtme hakkında daha fazla bilgi için.

Aşağıdaki örnek komutlar parametrelere sahip bir Azure Resource Manager cmdlet'i kullanılarak bir runbook'a bir zamanlama Bağla gösterilmektedir.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Aşağıdaki örnek komutlar parametrelere sahip bir Azure Hizmet Yönetimi cmdlet'i kullanılarak bir zamanlama Bağla gösterilmektedir.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Bir zamanlama devre dışı bırakma
Bir zamanlama devre dışı bıraktığınızda, artık bağlı tüm runbook bu zamanlamaya göre çalışır. El ile bir zamanlama devre dışı bırakmak veya bunları oluşturduğunuzda bir sıklığı ile zamanlama için sona erme süresi ayarlayın. Sona erme zamanı geldiğinde, zamanlaması devre dışı bırakıldı.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure portalından bir zamanlama devre dışı bırakmak için
1. Azure portalından Automation hesabınızı seçin **zamanlamaları** bölümünün altında **paylaşılan kaynakları** soldaki.
2. Ayrıntılar bölmesini açmak için bir zamanlama adına tıklayın.
3. Değişiklik **etkin** için **Hayır**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Azure Klasik portalından bir zamanlama devre dışı bırakmak için
Zamanlama için Zamanlama Ayrıntıları sayfası, Klasik Azure portalından bir zamanlama devre dışı bırakabilirsiniz.

1. Azure Klasik portalında Otomasyon seçin ve ardından bir Otomasyon hesabının adını tıklatın.
2. Varlıklar sekmesini seçin.
3. Kendi ayrıntı sayfası açmak için bir zamanlama adına tıklayın.
4. Değişiklik **etkin** için **Hayır**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Windows PowerShell ile bir zamanlama devre dışı bırakmak için
Kullanabileceğiniz [kümesi AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) klasik bir runbook için varolan bir zamanlamanın özelliklerini değiştirmek için cmdlet veya [kümesi AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet'i Azure portalında runbook'lar için. Zamanlama devre dışı bırakmak için belirtmek **false** için **IsEnabled** parametresi.

Aşağıdaki örnek komutlar bir Azure Resource Manager cmdlet'i kullanılarak bir runbook için bir zamanlama devre dışı bırakma göstermektedir.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Aşağıdaki örnek komutlar Azure Hizmet Yönetimi cmdlet'ini kullanarak bir zamanlama devre dışı bırakma göstermektedir.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Sonraki adımlar
* Azure Otomasyonu runbook'ları kullanmaya başlamak için bkz: [Azure Otomasyonu Runbook başlatma](automation-starting-a-runbook.md) 

