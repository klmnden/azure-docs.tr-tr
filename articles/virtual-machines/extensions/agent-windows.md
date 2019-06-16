---
title: Azure sanal makine Aracısı genel bakış | Microsoft Docs
description: Azure sanal makine Aracısı genel bakış
services: virtual-machines-windows
documentationcenter: virtual-machines
author: roiyz-msft
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
ms.author: roiyz
ms.openlocfilehash: d17d7c9d7b57e6ca040e4f81c9665789c8bc26e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60799780"
---
# <a name="azure-virtual-machine-agent-overview"></a>Azure sanal makine Aracısı genel bakış
Microsoft Azure sanal makine Aracısı (VM Aracısı) sanal makine (VM) etkileşim Azure yapı denetleyicisi tarafından yönetilen güvenli, hafif bir işlemdir. VM Aracısı, etkinleştirme ve Azure sanal makine uzantıları yürütme süreçlerinde birincil role sahiptir. Dağıtım sonrası yapılandırma, yükleme ve yazılım yapılandırma gibi sanal makine, VM uzantılarını etkinleştirin. VM uzantıları, bir sanal makinenin yönetici parola sıfırlama gibi kurtarma özellikleri de olanak sağlar. VM uzantıları Azure VM Aracısı, çalıştırılamaz.

Bu makalede, yükleme, algılama ve Azure sanal makine aracısı kaldırma ayrıntıları.

## <a name="install-the-vm-agent"></a>VM aracısını yükleyin

### <a name="azure-marketplace-image"></a>Azure Market görüntüsü

Azure VM Aracısı, bir Azure Market görüntü üzerinden dağıtılan herhangi bir Windows VM üzerinde varsayılan olarak yüklenir. Azure VM Aracısı, Azure Market görüntüsü portal, PowerShell, komut satırı arabirimi ya da bir Azure Resource Manager şablonu dağıtırken de yüklenir.

Windows Konuk Aracısı paketi iki bölüme ayrılır:

- Sağlama Aracısı (PA)
- Windows Konuk Aracısı (WinGA)

VM'de yüklü PA olmalıdır bir VM'yi önyüklemek için ancak WinGA yüklü olması gerekmez. Zaman VM dağıtma, WinGA yüklememeyi seçebilirsiniz. Aşağıdaki örnek nasıl seçileceği gösterilir *provisionVmAgent* seçeneği bir Azure Resource Manager şablonu ile:

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

Aracıların yüklü değilse, Azure yedekleme ya da Azure güvenlik gibi bazı Azure Hizmetleri kullanamazsınız. Bu hizmetler bir uzantı yüklü olmasını gerektirir. Bir VM WinGA olmadan dağıttıysanız, aracı daha sonra en son sürümünü yükleyebilirsiniz.

### <a name="manual-installation"></a>El ile yükleme
Windows VM aracısını el ile bir Windows Installer paketi ile yüklenebilir. Azure'a dağıtılan özel bir VM görüntüsü oluşturduğunuzda el ile yüklemesi gerekebilir. Windows VM aracısını el ile yüklemek için [VM aracı yükleyicisini indirmek](https://go.microsoft.com/fwlink/?LinkID=394789).

VM Aracısı, Windows Installer dosyasını çift tıklayarak yüklenebilir. VM Aracısı otomatik olarak veya katılımsız yüklemesini için aşağıdaki komutu çalıştırın:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>VM Aracısı algılayın

### <a name="powershell"></a>PowerShell

Azure Resource Manager PowerShell modülü, Azure sanal makineleri hakkında bilgi almak için kullanılabilir. Azure VM Aracısı sağlama durumu gibi bir VM hakkında bilgi için kullanın [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm):

```powershell
Get-AzVM
```

Aşağıdaki sıkıştırılmış örneğe çıktısı bunu gösterir *ProvisionVMAgent* özelliği iç içe geçmiş içinde *OSProfile*. Bu özellik, VM Aracısı VM dağıtıp dağıtmadığını belirlemek için kullanılabilir:

```powershell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Sanal makine adları ve VM aracısının durumunu kısa bir listesini döndürmek için aşağıdaki komut kullanılabilir:

```powershell
$vms = Get-AzVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>El ile algılama

Bir Windows VM'de oturum açtıktan sonra Görev Yöneticisi'ni çalışan işlemleri incelemek için kullanılabilir. Azure VM Aracısı, denetlemek için Görev Yöneticisi'ni açın, *ayrıntıları* sekmesini tıklatın ve bir işlem adı arayın **WindowsAzureGuestAgent.exe**. Bu işlemin varlığı, VM aracısı yüklü olduğunu gösterir.


## <a name="upgrade-the-vm-agent"></a>VM Aracısı'nı yükseltme
Windows için Azure VM aracısını otomatik olarak yükseltilir. Yeni sanal makineler, Azure'a dağıtılırken, VM sağlama zaman en son VM Aracısı alırlar. Özel VM görüntülerini el ile görüntü oluşturma zamanında yeni VM Aracısı içerecek şekilde güncelleştirilmelidir.


## <a name="next-steps"></a>Sonraki adımlar
VM uzantıları hakkında daha fazla bilgi için bkz: [Azure sanal makine uzantılarına ve özelliklerine genel bakış](overview.md).
