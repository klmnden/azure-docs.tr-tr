---
title: Azure'da bir SQL sanal makinesi için lisanslama modelini değiştirme | Microsoft Docs
description: Bir Azure SQL sanal makinesi için lisanslama nasıl değiştireceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/14/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c516bf9b48164f2ef8dc7fea6fb834bdae00a0d1
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332158"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Azure'da bir SQL Server sanal makinesi için lisanslama modelini değiştirme
Bu makalede yeni kullanarak Azure'da bir SQL Server sanal makine için lisans modeli değiştirmek nasıl SQL kaynak sağlayıcısı - **Microsoft.SqlVirtualMachine**. İki lisans modelleri barındıran SQL Server - ödeme-başına kullanım, bir sanal makine (VM) ve kendi lisansınızı getirin (BYOL). Ve artık, PowerShell veya Azure CLI kullanarak kullanan SQL sanal makinenizin hangi lisans modeli değiştirebilirsiniz. 

**Kullanım başına ödeme** modeli anlamına gelir saniye başına maliyeti Azure VM'de çalışan SQL Server Lisans maliyetini içerir.

**Getir-kendi lisansını** modelidir olarak da bilinen [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/), ve SQL Server çalıştıran bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak sağlar. Fiyatlar hakkında daha fazla bilgi için bkz: [SQL VM fiyatlandırma Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

İki lisans modelleri arasında geçiş doğurur **kapalı kalma süresi olmadan**, VM başlatmaz, ekler **ek ücret ödemeden** (aslında AHB etkinleştirme maliyeti azaltır) ve **hemen etkili**. 

## <a name="prerequisites"></a>Önkoşullar
SQL VM kaynak sağlayıcısı SQL Iaas uzantısı gerektirir. Bu nedenle, ile SQL VM kaynak sağlayıcısı yararlanmaya devam etmek için aşağıdakiler gerekir:
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- A [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) ile [SQL Iaas uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) yüklü. 

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme
Lisans modelleri arasında geçiş yapma özelliğini (Microsoft.SqlVirtualMachine) yeni SQL VM kaynak sağlayıcısı tarafından sağlanan bir özelliktir. Şu anda lisanslama modelinizin arasında geçiş yapabilmek için önce yeni sağlayıcısını aboneliğinize kaydetmeniz ve ardından, var olan bir VM ile yeni SQL VM kaynak sağlayıcısı kaydetme gerekecektir. SQL VM kaynak sağlayıcısını kullanmak için SQL Iaas uzantısı yüklemeniz gerekir. Bunun yapılması, dağıtılan bir sanal makine VHD ile kaydetmek izin verir. Daha fazla bilgi için [SQL Iaas uzantısı](virtual-machines-windows-sql-server-agent-extension.md). 

  >[!IMPORTANT]
  > SQL VM kaynağınızı sürüklerseniz, görüntü sabit kodlanmış lisans ayarına geri geçer. 
  

### <a name="powershell"></a>PowerShell


Aşağıdaki kod parçacığı, Azure'a bağlanmak ve kullanmakta olduğunuz hangi abonelik Kimliğini doğrulayın. 
```PowerShell
# Connect to Azure
Connect-AzureRmAccount
Account: <account_name>

# Verify your subscription ID
Get-AzureRmContext

# Set the correct Azure Subscription ID
Set-AzureRmContext -SubscriptionId <Subscription_ID>
```

Aşağıdaki kod parçacığı, önce yeni SQL kaynak sağlayıcısı, aboneliğiniz için kaydeder ve ardından var olan SQL VM'nizi yeni kaynak sağlayıcısı ile kaydeder. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine

# Register your existing SQL VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

### <a name="portal"></a>Portal
Portalı kullanarak yeni SQL VM kaynak sağlayıcısını da kaydedebilirsiniz. İçin aşağıdaki adımları izleyin:
1. Azure portalını açın ve gidin **tüm hizmetleri**. 
1. Gidin **abonelikleri** ve ilgi aboneliği seçin.  
1. İçinde **abonelikleri** dikey penceresinde gidin **kaynak sağlayıcısı**. 
1. Tür `sql` filtresindeki SQL ile ilgili kaynak sağlayıcıları taşıyın. 
1. Şunlardan birini seçin *kaydetme*, *yeniden kaydettirin*, veya *Unregister* için **Microsoft.SqlVirtualMachine** sağlayıcısına bağlı olarak, İstenen eylem. 

  ![Sağlayıcı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


## <a name="use-powershell"></a>PowerShell kullanma 
Lisanslama modelinizin değiştirmek için PowerShell kullanabilirsiniz.  SQL VM'nizi lisanslama modeli geçmeden önce yeni SQL kaynak sağlayıcısı ile kaydedilmiş olduğunu unutmayın. 

Aşağıdaki kod parçacığı KLG (veya Azure hibrit Avantajı'nı kullanarak), lisans kullanım başına ödeme modeli geçer: 
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzureRmResource -Force 
``` 

Aşağıdaki kod parçacığı KLG modelinizi ödeme başına kullanım için geçer:
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzureRmResource -Force 
```

  >[!NOTE]
  > Lisansları arasında geçiş yapmak için yeni SQL VM kaynak sağlayıcısı kullanıyor gerekir. Yeni sağlayıcı ile SQL sanal makinenizin kaydetmeden önce bu komutları çalıştırmayı denerseniz, bu hatayla karşılaşabilirsiniz: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Bu hatayı görürseniz, lütfen [yeni kaynak sağlayıcısı ile SQL sanal makinenize kaydedin](#register-existing-SQL-vm-with-new-resource-provider). 
 

## <a name="use-azure-cli"></a>Azure CLI kullanma
Lisanslama modelinizin değiştirmek için Azure CLI'yı kullanabilirsiniz.  SQL VM'nizi lisanslama modeli geçmeden önce yeni SQL kaynak sağlayıcısı ile kaydedilmiş olduğunu unutmayın. 

Aşağıdaki kod parçacığı KLG (veya Azure hibrit Avantajı'nı kullanarak), lisans kullanım başına ödeme modeli geçer:
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

Aşağıdaki kod parçacığı KLG modelinizi ödeme başına kullanım için geçer: 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >Lisansları arasında geçiş yapmak için yeni SQL VM kaynak sağlayıcısı kullanıyor gerekir. Yeni sağlayıcı ile SQL sanal makinenizin kaydetmeden önce bu komutları çalıştırmayı denerseniz, bu hatayla karşılaşabilirsiniz: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Bu hatayı görürseniz, lütfen [yeni kaynak sağlayıcısı ile SQL sanal makinenize kaydedin](#register-existing-SQL-vm-with-new-resource-provider). 

## <a name="view-current-licensing"></a>Geçerli görünümü lisanslama 

Aşağıdaki kod parçacığı SQL sanal Makineniz için geçerli lisanslama modelinizin görüntülemenize olanak sağlar. 

```PowerShell
# View current licensing model for your SQL VM
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="known-errors"></a>Bilinen hataları

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>Sanal makine üzerinde SQL Iaas uzantısı yüklü değil
SQL Iaas uzantısı, SQL Server VM'nize SQL VM kaynak sağlayıcısı ile kaydetmek için gerekli bir önkoşuldur. SQL Server VM'nize SQL Iaas uzantısı yüklemeden önce kaydettirmeye çalışırsanız, şu hatayla karşılaşırsınız:

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

Bu sorunu çözmek için SQL Server VM'nize kaydedilecek çalışmadan önce SQL Iaas uzantısı'nı yükleyin. 

  > [!NOTE]
  > Yükleme SQL Iaas uzantısı, SQL Server hizmetini yeniden başlatır ve yalnızca bir bakım penceresi sırasında yapılmalıdır. Daha fazla bilgi için [SQL Iaas uzantısı yükleme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>'Sku' parametresindeki bağımsız değişken doğrulanamıyor
SQL Server VM'nin lisanslama modelinizin, Azure PowerShell kullanırken değiştirmeye çalışırken bu hatayla karşılaşabilirsiniz > 4.0:

`Set-AzureRmResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Bu hatayı gidermek için lisanslama modelinizin geçiş yaparken yukarıda açıklanan PowerShell kod parçacığında bu satırı açıklamadan çıkarın: 
```PowerShell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Azure PowerShell sürümünü doğrulamak için aşağıdaki kodu kullanın:

```PowerShell
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server üzerindeki bir Windows VM ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Fiyatlandırma Kılavuzu bir Windows VM'de SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server Windows VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


