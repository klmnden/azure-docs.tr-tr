---
title: Azure DevTest labs'deki bir VM için bir yapıt ekleyin | Microsoft Docs
description: Azure DevTest labs'deki bir laboratuvara içindeki bir sanal makineye bir yapıt eklemeyi öğrenin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 5e6a7cbc070d81de33fac07a89dabf2b469bd355
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58450168"
---
# <a name="add-an-artifact-to-a-vm"></a>Bir VM'ye bir yapıt ekleme
Bir VM oluştururken, mevcut yapıtı için ekleyebilirsiniz. Bu yapılar herhangi birinden olabilir [genel DevTest Labs Git deposu](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) veya kendi Git deposundan. Bu makalede Azure portalı ve Azure PowerShell kullanarak yapıt Ekle gösterilmektedir. 

Azure DevTest Labs *yapıtları* belirtmenizi sağlar *eylemleri* VM hazırlandığında Windows PowerShell komut dosyaları çalıştırmak, Bash komutları çalıştırma ve yazılım yükleme gibi gerçekleştirilir. Yapıt *parametreleri* belirli senaryonuz için yapıt özelleştirmenize olanak sağlar.

Özel yapıtlar oluşturma hakkında bilgi edinmek için bkz: [Özel yapıtlar oluşturma](devtest-lab-artifact-author.md).

## <a name="use-azure-portal"></a>Azure portalı kullanma 
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Çalışmak istediğiniz sanal Makineyi içeren Laboratuvar labs listesinden seçin.  
1. Seçin **sanal makinelerim**.
1. İstediğiniz VM'yi seçin.
1. Seçin **yapıtları yönetme**. 
1. Seçin **yapıları uygulamak**.
1. Üzerinde **yapıları uygulamak** bölmesinde istediğiniz sanal Makineye eklemek için yapıt seçin.
1. Üzerinde **yapıt ekleme** bölmesinde gerekli parametre değerlerini ve ihtiyaç duyduğunuz herhangi bir isteğe bağlı parametreler girin.  
1. Seçin **Ekle** yapıt ekleme ve dönmek için **yapıları uygulamak** bölmesi.
1. Sanal Makineniz için gerektiği şekilde yapıtları eklemeye devam edebilirsiniz.
1. Yapıtları ekledikten sonra [yapıtlar çalıştırılma sırasını değiştirmek](#change-the-order-in-which-artifacts-are-run). Aynı zamanda geri gidebilirsiniz [görüntülemek veya değiştirmek yapı](#view-or-modify-an-artifact).
1. Ekleme yapıtları işiniz bittiğinde seçin **Uygula**

### <a name="change-the-order-in-which-artifacts-are-run"></a>Yapıtları çalıştırdığınız sırasını değiştirme
Varsayılan olarak, yapıları eylemleri VM'ye eklenen sırayla yürütülür. Aşağıdaki adımlar, yapılar çalıştığı sırasını değiştirme göstermektedir.

1. Üst kısmındaki **yapıları uygulamak** bölmesinde, sanal Makineye eklenmiş olan yapıların sayısını gösteren bağlantıyı seçin.
   
    ![VM'ye eklenen yapıların sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Üzerinde **seçili yapıtları** bölmesinde sürükleyip yapıtlar istenen sıralar. Yapıt sürükleyerek sorun yaşıyorsanız, yapıt sol taraftan sürükleyerek emin olun. 
1. Tamamladığınızda **Tamam**’ı seçin.  

### <a name="view-or-modify-an-artifact"></a>Görüntüleme veya bir yapıt değiştirme
Aşağıdaki adımlar, görüntülemek veya bir yapı parametrelerini değiştirme göstermektedir:

1. Üst kısmındaki **yapıları uygulamak** bölmesinde, sanal Makineye eklenmiş olan yapıların sayısını gösteren bağlantıyı seçin.
   
    ![VM'ye eklenen yapıların sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Üzerinde **seçili yapıtları** bölmesinde, görüntülemek veya düzenlemek istediğiniz yapıyı seçin.  
1. Üzerinde **yapıt ekleme** bölmesinde, tüm gereken değişiklikler ve seçin **Tamam** kapatmak için **yapıt ekleme** bölmesi.
1. Seçin **Tamam** kapatmak için **seçili yapıtları** bölmesi.

## <a name="use-powershell"></a>PowerShell kullanma
Aşağıdaki komut, belirtilen yapıt belirtilen VM için geçerlidir. [Invoke-AzureRmResourceAction](/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-6.13.0) işlemi gerçekleştiren bir komuttur.  

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

param
(
[Parameter(Mandatory=$true, HelpMessage="The ID of the subscription that contains the lab")]
   [string] $SubscriptionId,
[Parameter(Mandatory=$true, HelpMessage="The name of the lab containing the virtual machine")]
   [string] $DevTestLabName,
[Parameter(Mandatory=$true, HelpMessage="The name of the virtual machine")]
   [string] $VirtualMachineName,
[Parameter(Mandatory=$true, HelpMessage="The repository where the artifact is stored")]
   [string] $RepositoryName,
[Parameter(Mandatory=$true, HelpMessage="The artifact to apply to the virtual machine")]
   [string] $ArtifactName,
[Parameter(ValueFromRemainingArguments=$true)]
   $Params
)

# Set the appropriate subscription
Set-AzureRmContext -SubscriptionId $SubscriptionId | Out-Null
 
# Get the lab resource group name
$resourceGroupName = (Find-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' | Where-Object { $_.Name -eq $DevTestLabName}).ResourceGroupName
if ($resourceGroupName -eq $null) { throw "Unable to find lab $DevTestLabName in subscription $SubscriptionId." }

# Get the internal repo name
$repository = Get-AzureRmResource -ResourceGroupName $resourceGroupName `
                    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
                    -ResourceName $DevTestLabName `
                    -ApiVersion 2016-05-15 `
                    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
                    | Select-Object -First 1

if ($repository -eq $null) { "Unable to find repository $RepositoryName in lab $DevTestLabName." }

# Get the internal artifact name
$template = Get-AzureRmResource -ResourceGroupName $resourceGroupName `
                -ResourceType "Microsoft.DevTestLab/labs/artifactSources/artifacts" `
                -ResourceName "$DevTestLabName/$($repository.Name)" `
                -ApiVersion 2016-05-15 `
                | Where-Object { $ArtifactName -in ($_.Name, $_.Properties.title) } `
                | Select-Object -First 1

if ($template -eq $null) { throw "Unable to find template $ArtifactName in lab $DevTestLabName." }

# Find the virtual machine in Azure
$FullVMId = "/subscriptions/$SubscriptionId/resourceGroups/$resourceGroupName`
                /providers/Microsoft.DevTestLab/labs/$DevTestLabName/virtualmachines/$virtualMachineName"

$virtualMachine = Get-AzureRmResource -ResourceId $FullVMId

# Generate the artifact id
$FullArtifactId = "/subscriptions/$SubscriptionId/resourceGroups/$resourceGroupName`
                        /providers/Microsoft.DevTestLab/labs/$DevTestLabName/artifactSources/$($repository.Name)`
                        /artifacts/$($template.Name)"

# Handle the inputted parameters to pass through
$artifactParameters = @()

# Fill artifact parameter with the additional -param_ data and strip off the -param_
$Params | ForEach-Object {
   if ($_ -match '^-param_(.*)') {
      $name = $_.TrimStart('^-param_')
   } elseif ( $name ) {
      $artifactParameters += @{ "name" = "$name"; "value" = "$_" }
      $name = $null #reset name variable
   }
}

# Create structure for the artifact data to be passed to the action

$prop = @{
artifacts = @(
    @{
        artifactId = $FullArtifactId
        parameters = $artifactParameters
    }
    )
}

# Check the VM
if ($virtualMachine -ne $null) {
   # Apply the artifact by name to the virtual machine
   $status = Invoke-AzureRmResourceAction -Parameters $prop -ResourceId $virtualMachine.ResourceId -Action "applyArtifacts" -ApiVersion 2016-05-15 -Force
   if ($status.Status -eq 'Succeeded') {
      Write-Output "##[section] Successfully applied artifact: $ArtifactName to $VirtualMachineName"
   } else {
      Write-Error "##[error]Failed to apply artifact: $ArtifactName to $VirtualMachineName"
   }
} else {
   Write-Error "##[error]$VirtualMachine was not found in the DevTest Lab, unable to apply the artifact"
}

```

## <a name="next-steps"></a>Sonraki adımlar
Yapıları üzerine aşağıdaki makalelere bakın:

- [Laboratuvarınız için zorunlu yapıtları belirtin](devtest-lab-mandatory-artifacts.md)
- [Özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
- [Bir laboratuvara yapıt deposu ekleme](devtest-lab-artifact-author.md)
- [Yapıt hatalarını tanılama](devtest-lab-troubleshoot-artifact-failure.md)