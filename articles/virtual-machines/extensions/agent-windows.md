---
title: Azure sanal makine Aracısı genel bakış | Microsoft Docs
description: Azure sanal makine Aracısı genel bakış
services: virtual-machines-windows
documentationcenter: virtual-machines
author: danielsollondon
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.openlocfilehash: fb29f0f931715b8a6ba5b4528294eb61ef5762c8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-virtual-machine-agent-overview"></a>Azure sanal makine aracısını genel bakış
Microsoft Azure sanal makine Aracısı (VM Aracısı) Azure yapı denetleyicisi ile sanal makine (VM) etkileşimi yöneten güvenli ve basit bir işlemdir. VM Aracısı'nı etkinleştirme ve Azure sanal makine uzantıları yürütme birincil bir rolü var. VM uzantıları yükleme ve yazılım yapılandırma gibi VM Dağıtım sonrası yapılandırmasını etkinleştirin. VM uzantıları, VM yönetici parolasını sıfırlama gibi kurtarma özellikleri de sağlar. Azure VM Aracısı VM uzantıları çalıştırılamaz.

Bu makalede, yükleme, algılama ve kaldırma Azure sanal makine Aracısı'nın ayrıntıları.

## <a name="install-the-vm-agent"></a>VM Aracısı yükleme

### <a name="azure-marketplace-image"></a>Azure Market görüntüsü

Azure VM Aracısı, bir Azure Market görüntüsünden dağıtılan herhangi bir Windows VM üzerinde varsayılan olarak yüklenir. Portal, PowerShell, komut satırı arabirimini veya bir Azure Resource Manager şablonu Azure Market görüntüsünden dağıttığınızda, Azure VM Aracısı'nı da yüklenir.

Windows Konuk aracı paketi, iki bölüme ayrılır:

- Sağlama Aracısı (PA)
- Windows Konuk Aracısı (WinGA)

VM'de yüklü PA olmalıdır VM önyükleme, ancak WinGA yüklenmesi gerekmez. VM zaman dağıtabilir, WinGA yüklememeyi seçebilirsiniz. Aşağıdaki örnekte nasıl seçileceğini gösterir *provisionVmAgent* seçenek bir Azure Resource Manager şablonu ile:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Yüklü aracıları yoksa, Azure Backup veya Azure güvenliği gibi bazı Azure Hizmetleri kullanamazsınız. Bu hizmetler uzantı yüklü olmasını gerektirir. Bir VM WinGA olmadan dağıttıysanız, daha sonra Aracısı en son sürümünü yükleyebilirsiniz.

### <a name="manual-installation"></a>El ile yükleme
Windows VM Aracısı'nı el ile bir Windows Installer paketi ile yüklenebilir. Azure'a dağıtılan özel bir VM görüntüsü oluşturduğunuzda, el ile kurulması gerekebilir. Windows VM Aracısı'nı el ile yüklemek için [VM Aracısı yükleyici indirmek](http://go.microsoft.com/fwlink/?LinkID=394789).

VM Aracısı, Windows Installer dosyasını çift tıklatarak yüklenebilir. Bir otomatik olarak veya katılımsız yükleme VM Aracısı'nın için aşağıdaki komutu çalıştırın:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>VM Aracısı Algıla

### <a name="powershell"></a>PowerShell

Azure Resource Manager PowerShell modülü, Azure Vm'leri hakkında bilgi almak için kullanılabilir. Azure VM Aracısı'nı sağlama durumu gibi bir VM hakkındaki bilgileri görmek için kullanmak [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm):

'' 'powershell' Get-AzureRmVM
```

The following condensed example output shows the the *ProvisionVMAgent* property nested inside *OSProfile*. This property can be used to determine if the VM agent has been deployed to the VM:

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Aşağıdaki komut dosyası VM adları ve VM aracısının durumunu kısa listesini almak için kullanılabilir:

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>El ile algılama
Bir Windows Azure VM oturum açıldığında, Görev Yöneticisi'ni çalışan işlemler incelemek için kullanılabilir. Azure VM aracısı için denetlemek için Görev Yöneticisi'ni açın, *ayrıntıları* sekmesini tıklatın ve bir işlem adı arayın **WindowsAzureGuestAgent.exe**. VM aracısının yüklü olduğundan bu işlem varlığını gösterir.


## <a name="upgrade-the-vm-agent"></a>VM Aracısı yükseltme
Windows için Azure VM Aracısı otomatik olarak yükseltilir. Yeni sanal makineleri Azure'da dağıtılan gibi VM sağlama aynı anda en son VM Aracısı alırlar. Özel VM görüntüleri el ile yeni VM Aracısı görüntü oluşturma zamanında içerecek şekilde güncelleştirilmelidir.


## <a name="next-steps"></a>Sonraki adımlar
VM uzantıları hakkında daha fazla bilgi için bkz: [Azure sanal makine uzantıları ve özellikleri genel bakış](overview.md).