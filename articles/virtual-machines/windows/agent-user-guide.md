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
ms.date: 11/17/2016
ms.author: danis
ms.openlocfilehash: a5da5cfb6cda940f68171e42d4512b304fc5f114
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915682"
---
# <a name="azure-virtual-machine-agent-overview"></a>Azure sanal makine aracısını genel bakış

Microsoft Azure sanal makine Aracısı (VM Aracısı) Azure yapı denetleyicisi VM etkileşim yöneten güvenli ve basit bir işlemdir. VM Aracısı'nı etkinleştirme ve Azure sanal makine uzantıları yürütme birincil bir rolü var. VM uzantıları etkinleştirme, yükleme ve yazılım yapılandırma gibi sanal makine dağıtım yapılandırması gönderin. Sanal makine uzantıları gibi bir sanal makine yönetici parolasını sıfırlama kurtarma özellikleri de sağlar. Azure VM Aracısı sanal makine uzantıları çalıştırılamaz.

Bu belge yükleme, algılama ve kaldırma Azure sanal makine Aracısı'nın ayrıntıları.

## <a name="install-the-vm-agent"></a>VM Aracısı yükleme

### <a name="azure-gallery-image"></a>Azure galeri görüntüsü

Azure VM Aracısı, Azure Galerisi görüntüden dağıtılmış tüm Windows sanal makine üzerinde varsayılan olarak yüklenir. Azure galerisinde görüntüden Portal, PowerShell, komut satırı arabirimini veya bir Azure Resource Manager şablonu dağıtırken Azure VM aracısı olan de yüklü olmalıdır. 

### <a name="manual-installation"></a>El ile yükleme

Windows VM Aracısı'nı el ile bir Windows Installer paketi kullanılarak yüklenebilir. Bir özel sanal makine görüntüsü oluşturma Azure'da dağıtılacak zaman el ile yükleme gerekli olabilir. Windows VM Aracısı'nı el ile yüklemek için bu konumdan VM aracı yükleyicisini indirin [Windows Azure VM Aracısı indirme](http://go.microsoft.com/fwlink/?LinkID=394789). 

VM Aracısı, windows Installer dosyasını çift tıklatarak yüklenebilir. Bir otomatik olarak veya katılımsız yükleme VM Aracısı'nın için aşağıdaki komutu çalıştırın.

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>VM Aracısı Algıla

### <a name="powershell"></a>PowerShell

Azure Resource Manager PowerShell modülü, Azure sanal makineler hakkında bilgi almak için kullanılabilir. Çalışan `Get-AzureRmVM` oldukça biraz Azure VM aracısı için sağlama durumu bilgilerini döndürür.

```PowerShell
Get-AzureRmVM
```

Yalnızca bir alt kümesini aşağıdadır `Get-AzureRmVM` çıktı. Bildirim `ProvisionVMAgent` içe özelliği içinde `OSProfile`, bu özellik, VM Aracısı sanal makineye dağıtılıp dağıtılmadığını belirlemek için kullanılabilir.

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Aşağıdaki komut dosyası, sanal makine adları ve VM aracısının durumunu kısa listesini almak için kullanılabilir.

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>El ile algılama

Bir Windows Azure VM oturum açıldığında, Görev Yöneticisi'ni çalışan işlemler incelemek için kullanılabilir. Azure VM aracısı için denetlemek için Görev Yöneticisi'ni açın > Ayrıntılar sekmesini tıklatın ve bir işlem adı arayın `WindowsAzureGuestAgent.exe`. VM aracısının yüklü olduğundan bu işlem varlığını gösterir.

## <a name="upgrade-the-vm-agent"></a>VM Aracısı yükseltme

Windows için Azure VM Aracısı otomatik olarak yükseltilir. Yeni sanal makineleri Azure'da dağıtılan gibi en son VM Aracısı alırlar. Özel VM görüntüleri el ile yeni VM Aracısı içerecek şekilde güncelleştirilmelidir.
