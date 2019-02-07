---
title: Azure'da bir SQL Server sanal makinesi için lisanslama modelini değiştirme | Microsoft Docs
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
ms.openlocfilehash: ff1281a249abf456176cffe2b02ef3c63b718d5a
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768005"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Azure'da bir SQL Server sanal makinesi için lisanslama modelini değiştirme
Bu makalede yeni kullanarak Azure'da bir SQL Server sanal makine için lisans modeli değiştirmek nasıl SQL VM kaynak sağlayıcısı - **Microsoft.SqlVirtualMachine**. İki lisans modelleri barındıran SQL Server - ödeme-başına kullanım, bir sanal makine (VM) ve kendi lisansınızı getirin (BYOL). Ve artık, PowerShell veya Azure CLI kullanarak kullanan SQL Server VM'nize hangi lisans modeli değiştirebilirsiniz. 

**Kullanım başına ödeme** (PAYG) modeli anlamına gelir saniye başına maliyeti Azure VM'de çalışan SQL Server Lisans maliyetini içerir.

**Getir-kendi lisansını** (KLG) modelidir olarak da bilinen [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/), ve SQL Server çalıştıran bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak sağlar. Fiyatlar hakkında daha fazla bilgi için bkz: [SQL Server VM fiyatlandırma Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

İki lisans modelleri arasında geçiş doğurur **kapalı kalma süresi olmadan**, VM başlatmaz, ekler **ek ücret ödemeden** (aslında AHB etkinleştirme *azaltır* maliyeti) ve **hemen etkili**. 

  >[!NOTE]
  > - Lisanslama modeli dönüştürme yeteneği şu anda yalnızca bir Kullandıkça Öde SQL Server VM görüntüsüyle başlatırken kullanılabilir. Portaldan Getir-kendi lisansını görüntüsü ile başlatırsanız, o yansıma Kullandıkça Öde yöntemine dönüştürmek mümkün olmayacaktır. 
  > - CSP müşterileri, öncelikle bir Kullandıkça Öde VM dağıtarak ve ardından getirin-kendi lisansını için dönüştürme AHB avantajı kullanabilir. 

## <a name="prerequisites"></a>Önkoşullar
SQL VM kaynak sağlayıcısı SQL Iaas uzantısı gerektirir. Bu nedenle, ile SQL VM kaynak sağlayıcısı yararlanmaya devam etmek için aşağıdakiler gerekir:
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- A [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) ile [SQL Iaas uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) yüklü. 


## <a name="register-existing-sql-server-vm-with-sql-resource-provider"></a>Mevcut SQL Server VM ile SQL kaynak sağlayıcısı kaydetme
Lisans modelleri arasında geçiş yapma özelliğini (Microsoft.SqlVirtualMachine) yeni SQL VM kaynak sağlayıcısı tarafından sağlanan bir özelliktir. SQL Server Vm'leri aralık 2018'e otomatik olarak kayıtlı sonra yeni bir kaynak sağlayıcısı ile dağıtılabilir. Ancak, bu tarihten önce dağıtılan mevcut sanal makinelerin, bir lisanslama modeli geçiş yönetmeden önce el ile kaynak sağlayıcısına kayıtlı gerekir. 

  > [!NOTE] 
  > SQL VM kaynağınızı sürüklerseniz, görüntü sabit kodlanmış lisans ayarına geri geçer. 

### <a name="register-sql-resource-provider-with-your-subscription"></a>Aboneliğinizde SQL kaynak sağlayıcısını kaydetme 

SQL Server VM'nize SQL kaynak sağlayıcısı ile kaydolmak için aboneliğinize kaynak sağlayıcısını kaydetmeniz gerekir. PowerShell veya Azure portalı ile bunu yapabilirsiniz. 

#### <a name="using-powershell"></a>PowerShell’i kullanma
Aşağıdaki kod parçacığı SQL kaynak sağlayıcısı, Azure aboneliğiniz ile kaydeder. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

#### <a name="using-azure-portal"></a>Azure portalını kullanma
Aşağıdaki adımlar, Azure portalını kullanarak Azure aboneliğinizde SQL kaynak sağlayıcısı kaydeder. 

1. Azure portalını açın ve gidin **tüm hizmetleri**. 
1. Gidin **abonelikleri** ve ilgi aboneliği seçin.  
1. İçinde **abonelikleri** dikey penceresinde gidin **kaynak sağlayıcıları**. 
1. Tür `sql` filtresindeki SQL ile ilgili kaynak sağlayıcıları taşıyın. 
1. Şunlardan birini seçin *kaydetme*, *yeniden kaydettirin*, veya *Unregister* için **Microsoft.SqlVirtualMachine** sağlayıcısına bağlı olarak, İstenen eylem. 

  ![Sağlayıcı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>SQL kaynak sağlayıcısı ile SQL Server sanal Makinesini kaydetme
SQL kaynak sağlayıcısı, aboneliğiniz ile kaydedildikten SQL Server VM'nize SQL kaynak sağlayıcısı ile kaydolmak için PowerShell kullanabilirsiniz. 


```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```


## <a name="use-powershell"></a>PowerShell kullanma 
Lisanslama modelinizin değiştirmek için PowerShell kullanabilirsiniz.  SQL Server VM'nize lisanslama modeli geçmeden önce yeni SQL kaynak sağlayıcısı ile kaydedilmiş olduğunu unutmayın. 

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
  > Lisansları arasında geçiş yapmak için yeni SQL VM kaynak sağlayıcısı kullanıyor gerekir. Yeni sağlayıcı ile SQL Server VM'nize kaydetmeden önce bu komutları çalıştırmayı denerseniz, bu hatayla karşılaşabilirsiniz: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Bu hatayı görürseniz, lütfen [SQL Server VM'nize yeni bir kaynak sağlayıcısı ile kaydolmak](#register-existing-sql-server-vm-with-sql-resource-provider). 
 

## <a name="use-azure-cli"></a>Azure CLI kullanma
Lisanslama modelinizin değiştirmek için Azure CLI'yı kullanabilirsiniz.  SQL Server VM'nize lisanslama modeli geçmeden önce yeni SQL kaynak sağlayıcısı ile kaydedilmiş olduğunu unutmayın. 

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
  >Lisansları arasında geçiş yapmak için yeni SQL VM kaynak sağlayıcısı kullanıyor gerekir. Yeni sağlayıcı ile SQL Server VM'nize kaydetmeden önce bu komutları çalıştırmayı denerseniz, bu hatayla karşılaşabilirsiniz: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Bu hatayı görürseniz, lütfen [SQL Server VM'nize yeni bir kaynak sağlayıcısı ile kaydolmak](#register-existing-sql-server-vm-with-sql-resource-provider). 

## <a name="view-current-licensing"></a>Geçerli görünümü lisanslama 

Aşağıdaki kod parçacığı için SQL Server VM'nize geçerli lisanslama modelinizin görüntülemenize olanak sağlar. 

```PowerShell
# View current licensing model for your SQL Server VM
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


