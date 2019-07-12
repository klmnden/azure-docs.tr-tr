---
title: Azure'da bir şablondan Windows VM oluşturma | Microsoft Docs
description: Kolayca yeni bir Windows VM oluşturmak için bir Resource Manager şablonu ve PowerShell kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70a9680b9f8696cd40cb5631217861dc6d1d7ad8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719907"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Bir Resource Manager şablonundan bir Windows sanal makine oluşturma

Azure Cloud shell'den bir Azure Resource Manager şablonu ve Azure PowerShell kullanarak bir Windows sanal makine oluşturmayı öğrenin. Bu makalede kullanılan şablon yeni bir sanal ağda tek bir alt ağ ile Windows Server çalıştıran tek bir sanal makine dağıtır. Linux sanal makinesi oluşturmak için bkz: [Azure Resource Manager şablonları ile Linux sanal makinesi oluşturma](../linux/create-ssh-secured-vm-from-template.md).

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bir Azure sanal makinesi oluşturma genellikle iki adımları içerir:

- Bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır.
- Sanal makine oluşturur.

Aşağıdaki örnek, bir VM oluşturur. bir [Azure Hızlı Başlangıç şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json). Şablonun bir kopyasını şu şekildedir:

[!code-json[create-windows-vm](~/quickstart-templates/101-vm-simple-windows/azuredeploy.json)]

PowerShell betiğini çalıştırmak için seçin **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter an unique DNS name for the public IP"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" `
    -adminUsername $adminUsername `
    -adminPassword $adminPassword `
    -dnsLabelPrefix $dnsLabelPrefix

 (Get-AzVm -ResourceGroupName $resourceGroupName).name

```

Azure Cloud shell Powershell'den yerine yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için.

Önceki örnekte, Github'da depolanmış bir şablon belirtildi. Ayrıca indirin veya bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

Bazı ek kaynaklar aşağıda verilmiştir:

- Resource Manager şablonları geliştirme hakkında bilgi edinmek için [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).
- Azure sanal makine şemaları görmek için bkz: [Azure şablon başvurusu](/azure/templates/microsoft.compute/allversions).
- Daha fazla sanal makine şablonu örnekleri görmek için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Önceki komut dosyasından son PowerShell komutu, sanal makine adı gösterir. Sanal makineye bağlanmak için bkz: [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](./connect-logon.md).

## <a name="next-steps"></a>Sonraki Adımlar

- Dağıtımla ilgili sorunlar varsa, göz atın [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](../../resource-manager-common-deployment-errors.md).
- Oluşturma ve içinde bir sanal makineyi yönetmeyi öğrenin [oluşturun ve Azure PowerShell modülü ile Windows Vm'leri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Şablonları oluşturma hakkında daha fazla bilgi için JSON söz dizimi ve dağıttığınız kaynak türleri için özellikleri görüntüleyin:

- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
