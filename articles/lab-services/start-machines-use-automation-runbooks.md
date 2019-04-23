---
title: Azure DevTest Labs'de Otomasyon runbook'ları kullanarak makineleri başlangıç | Microsoft Docs
description: Azure Otomasyonu runbook'larını kullanarak sanal makineleri Azure DevTest labs'deki bir laboratuvara başlatmak öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.openlocfilehash: 8d3885ba25e479316f97ecbb0681a1680650fc09
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996671"
---
# <a name="start-virtual-machines-in-a-lab-in-order-by-using-azure-automation-runbooks"></a>Azure Otomasyonu runbook'ları kullanarak sırasıyla bir laboratuvarda sanal makinelerini başlatma
[Autostart](devtest-lab-set-lab-policy.md#set-autostart) özelliği DevTest Labs, Vm'leri belirli bir zamanda otomatik olarak başlayacak şekilde yapılandırmanıza olanak tanır. Ancak, bu özellik, belirli bir sırayla başlatmak için makineleri desteklemez. İse, bu tür bir Otomasyon yararlı olacağı birkaç senaryo mevcuttur.  Sıçrama kutusu diğer vm'lere erişim noktası olarak kullanılmak üzere bir Sıçrama kutusu VM bir laboratuvar içindeki diğer Vm'lere önce ilk olarak başlatılması gereken yere bir senaryodur.  Bu makalede, bir betik yürütür bir PowerShell runbook'u ile bir Azure Otomasyonu hesabını ayarlama işlemini göstermektedir. Komut dosyası etiketleri betik değiştirmek zorunda kalmadan başlama sırasını denetlemesine olanak tanımak için laboratuvar Vm'lerde kullanır.

## <a name="setup"></a>Kurulum
Bu örnekte, Vm'leri Laboratuvardaki etiket gerek **StartupOrder** uygun değeriyle eklendi (0,1,2, vs.). -1 başlatılması gerekmez herhangi bir makineye atayın.

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma
Konusundaki yönergeleri izleyerek bir Azure Otomasyonu hesabını oluşturmak [bu makalede](../automation/automation-create-standalone-account.md). Seçin **farklı çalıştır hesapları** hesabı oluştururken seçeneği. Otomasyon hesabı oluşturulduktan sonra açmak **modülleri** sayfasında ve seçin **Azure modüllerini güncelleştirme** menü çubuğundaki. Eski ve güncelleştirme komut dosyası olmadan çeşitli sürümlerinden çalışmayan varsayılan modüllerdir.

## <a name="add-a-runbook"></a>Runbook Ekle
Şimdi bir runbook Otomasyon hesabına eklemek için seçin **runbook'ları** sol menüsünde. Seçin **runbook Ekle** menü ve aşağıdaki talimatlarda [PowerShell runbook'u oluşturma](../automation/automation-first-runbook-textual-powershell.md).

## <a name="powershell-script"></a>PowerShell betiği
Aşağıdaki komut dosyası, abonelik adı, parametre olarak Laboratuvar adı alır. Komut akışını, tüm Vm'leri Laboratuvardaki alın ve ardından sanal makine adları ve bunların başlatma sırası listesini oluşturmak için etiket bilgileri ayrıştırılamıyor sağlamaktır. Betik, sanal makineleri sırasına göre kılavuzluk ve Vm'leri başlatır. Belirli bir numarasının içinde birden fazla VM varsa, zaman uyumsuz olarak PowerShell işleri kullanılarak başlatılır. Bir etiketi (10), son olarak başlangıç değer kümesi yoksa bu sanal makineler için bunlar varsayılan olarak son olarak, başlatılacak.  Laboratuvar otomatik olarak olmasını VM istememektedir, etiket değeri 11'e ayarlayın ve göz ardı edilir.

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

param
(
    [Parameter(Mandatory=$false, HelpMessage="Name of the subscription that has the lab")]
    [string] $SubscriptionName,

    [Parameter(Mandatory=$false, HelpMessage="Lab name")]
    [string] $LabName
)

# Connect and add the appropriate subscription
$Conn = Get-AutomationConnection -Name AzureRunAsConnection

Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationId -Subscription $SubscriptionName -CertificateThumbprint $Conn.CertificateThumbprint

# Find the lab
$dtLab = Find-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $LabName

# Get the VMs
$dtlAllVms = New-Object System.Collections.ArrayList
$AllVMs = Get-AzResource -ResourceId "$($dtLab.ResourceId)/virtualmachines" -ApiVersion 2016-05-15

# Get the StartupOrder tag, if missing set to be run last (10)
ForEach ($vm in $AllVMs) {
    if ($vm.Tags) {
        if ($vm.Tags['StartupOrder']) {
            $startupValue = $vm.Tags['StartupOrder']
        } else {
            $startupValue = 10
        }
        } else {
            $startupValue = 10
        }
        $dtlAllVms.Add(@{$vm.Name = $startupValue}) > $null
}

# Setup for the async multiple vm start

# Save profile
$profilePath = Join-Path $env:Temp "profile.json"
If (Test-Path $profilePath){
    Remove-Item $profilePath
}
Save-AzContext -Path $profilePath

# Job to start VMs asynch
$startVMBlock = {
    Param($devTestLab,$vmToStart,$profilePath)
    Import-AzContext -Path ($profilePath)
    Invoke-AzResourceAction `
        -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vmToStart" `
        -Action Start `
        -Force
    Write-Output "Started: $vmToStart"
}

$current = 0
# Start in order from 0 to 10

While ($current -le 10) {
# Get the VMs in the current stage
    $tobeStarted = $null
    $tobeStarted = $dtlAllVms | Where-Object { $_.Values -eq $current}
    if ($tobeStarted.Count -eq 1) {
        # Run sync – jobs not necessary for a single VM
        $returnStatus = Invoke-AzResourceAction `
                -ResourceId "$($dtLab.ResourceId)/virtualmachines/$($tobeStarted.Keys)" `
                -Action Start `
                -Force
        Write-Output "$($tobeStarted.Keys) status: $($returnStatus.status)"
    } elseif ($tobeStarted.Count -gt 1) {
        # Start multiple VMs async
        $jobs = @()
        Write-Output "Start Jobs start: $(Get-Date)"
        
        # Jobs
        $jobs += Start-Job -ScriptBlock $startVMBlock -ArgumentList $dtLab, $($singlevm.Keys), $profilePath
        Write-Output "Start Jobs end: $(Get-Date)"
    }

    # Get results from all jobs
    if($jobs.Count -ne 0) {
        Write-Output "Receive Jobs start: $(Get-Date)"
        foreach ($job in $jobs){
            $jobResult = Receive-Job -Job $job -Wait | Write-Output
        }
        Remove-Job -Job $jobs -Force
    }
    else
    {
        Write-Output "Information: No jobs available"
    }
}
```

## <a name="create-a-schedule"></a>Bir zamanlama oluşturun
Bu betiği, günlük, yürütme için [bir zamanlama oluşturmak](../automation/shared-resources/schedules.md#creating-a-schedule) Otomasyon hesabında. Zamanlama oluşturulduktan sonra [runbook'a Bağla](../automation/shared-resources/schedules.md#linking-a-schedule-to-a-runbook). 

Büyük ölçekli bir durumda birden çok labs ile birden fazla aboneliğiniz olduğunda, parametre bilgilerini farklı labs dosyasına depolayabilir ve dosyanın komut dosyası bağımsız parametreleri yerine. Betik değiştirilmesi gerekebilir ancak çekirdek yürütme aynı olacaktır. Bu örnek PowerShell betiğini çalıştırmak için Azure Automation'ı kullanırken bir derleme/yayın işlem hattı, bir görev kullanma gibi diğer seçenekleri vardır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Otomasyonu hakkında daha fazla bilgi edinmek için şu makaleye bakın: [Azure Otomasyonu giriş](../automation/automation-intro.md).
