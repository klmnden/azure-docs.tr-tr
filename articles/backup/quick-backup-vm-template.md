---
title: Azure hızlı başlangıç - Resource Manager şablonu ile sanal makine yedekleme
description: Azure Resource Manager şablonu ile sanal makinelerinizi yedekleme hakkında bilgi edinin
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b144d7509562b8ca0bca6299caee4a7ce292f4a6
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481372"
---
# <a name="back-up-a-virtual-machine-in-azure-with-resource-manager-template"></a>Azure Resource Manager şablonu ile bir sanal makineyi yedekleme

[Azure yedekleme](backup-overview.md) şirket içi makineleri ve uygulamaları ve Azure Vm'leri yedekler. Bu makalede Azure PowerShell ve Resource Manager şablonu ile bir Azure VM'yi yedekleme gösterilmektedir. Bu hızlı başlangıçta bir kurtarma Hizmetleri kasası oluşturmak için Resource Manager şablonu dağıtma işlemini üzerinde odaklanır. Resource Manager şablonları geliştirme hakkında daha fazla bilgi için bkz. [Resource Manager belgeleri](/azure/azure-resource-manager/) ve [şablon başvurusu](/azure/templates/microsoft.recoveryservices/allversions).

Alternatif olarak, bir VM kullanarak yedekleyebilirsiniz [Azure PowerShell](./quick-backup-vm-powershell.md), [Azure CLI](quick-backup-vm-cli.md), veya [Azure portalında](quick-backup-vm-portal.md).

## <a name="create-a-vm-and-recovery-services-vault"></a>VM ve kurtarma Hizmetleri kasası oluşturma

A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) Azure Vm'leri gibi korunan kaynakları için yedekleme verilerini depolayan bir mantıksal kapsayıcıdır. Bir yedekleme işi çalıştığında kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

Bu hızlı başlangıçta kullanılan şablon dandır [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/). Bu şablon, basit Windows VM ve kurtarma Hizmetleri kasası DefaultPolicy koruma için yapılandırılmış dağıtmanıza olanak sağlar.

Şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açın ve aşağıdaki PowerShell betiğini shell penceresine yapıştırın. Kod yapıştırmak için shell penceresine sağ tıklayın ve ardından **yapıştırın**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name (limited to eight characters) that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username for the virtual machine"
$adminPassword = Read-Host -Prompt "Enter the administrator password for the virtual machine" -AsSecureString
$dnsPrefix = Read-Host -Prompt "Enter the unique DNS Name for the Public IP used to access the virtual machine"

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-recovery-services-create-vm-and-configure-backup/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -adminUsername $adminUsername -adminPassword $adminPassword -dnsLabelPrefix $dnsPrefix
```

Azure PowerShell, bu hızlı başlangıçta Resource Manager şablonu dağıtmak için kullanılır. [Azure portalında](../azure-resource-manager/resource-group-template-deploy-portal.md), [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md), ve [Rest API](../azure-resource-manager/resource-group-template-deploy-rest.md) şablonları dağıtmak için de kullanılabilir.

## <a name="start-a-backup-job"></a>Bir yedekleme işi başlatma

Şablon bir VM oluşturur ve sanal makinede sağlar. Şablonu dağıttıktan sonra yedekleme işini başlatmanız gerekir. Daha fazla bilgi için [bir yedekleme işi başlatma](./quick-backup-vm-powershell.md#start-a-backup-job).

## <a name="monitor-the-backup-job"></a>Yedekleme işini izleme

Yedekleme işi izlemek için bkz: [yedekleme işini izleme](./quick-backup-vm-powershell.md#monitor-the-backup-job).

## <a name="clean-up-the-deployment"></a>Dağıtımı temizleme

Artık sanal makineyi yedeklemek istiyorsanız, bunu temizleyebilirsiniz.

- VM geri kullanıma denemek istiyorsanız, yedekleme temiz atlayın.
- Varolan bir VM'yi kullandıysanız, en son atlayabilirsiniz [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini kaynak grubunu ve sanal makineyi bırakabilirsiniz.

Korumayı devre dışı bırakın, kasa ve geri yükleme noktalarını kaldırın. Ardından kaynak grubunu ve sanal makine kaynaklarıyla ilişkilendirilmiş, aşağıda belirtildiği gibi silin:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kurtarma Hizmetleri kasası oluşturdunuz, bir sanal makine için koruma özelliklerini etkinleştirdiniz ve ilk kurtarma noktasını oluşturdunuz.

- [Bilgi nasıl](tutorial-backup-vm-at-scale.md) Vm'leri Azure portalında yedekleme için.
- [Bilgi nasıl](tutorial-restore-disk.md) hızlıca bir VM'yi geri yüklemek için
