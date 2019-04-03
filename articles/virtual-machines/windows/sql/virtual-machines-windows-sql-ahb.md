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
ms.date: 02/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: bc3e2955049188b0794367d5391762f5eb50b1c0
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58850184"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Azure'da bir SQL Server sanal makinesi için lisanslama modelini değiştirme
Bu makalede yeni kullanarak Azure'da bir SQL Server sanal makine için lisans modeli değiştirmek nasıl SQL VM kaynak sağlayıcısı - **Microsoft.SqlVirtualMachine**. İki sanal makine (VM) için model barındıran SQL Server - Kullandıkça Öde, lisanslama ve kendi lisansınızı getirin (BYOL). Ve artık, PowerShell veya Azure CLI kullanarak kullanan SQL Server VM'nize hangi lisans modeli değiştirebilirsiniz. 

**Kullandıkça Öde** (PAYG) modeli anlamına gelir saniye başına maliyeti Azure VM'de çalışan SQL Server Lisans maliyetini içerir.

**Getirin-kendi lisansını** (KLG) modelidir olarak da bilinen [Azure hibrit Avantajı'nı (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/), ve SQL Server çalıştıran bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak sağlar. Fiyatlar hakkında daha fazla bilgi için bkz: [SQL Server VM fiyatlandırma Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

İki lisans modelleri arasında geçiş doğurur **kapalı kalma süresi olmadan**, VM başlatmaz, ekler **ek ücret ödemeden** (aslında AHB etkinleştirme *azaltır* maliyeti) ve **hemen etkili**. 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="remarks"></a>Açıklamalar

 - Lisanslama modelini dönüştürme imkanı yalnızca kullandıkça öde SQL Server VM görüntüsünü kullanmaya başladığınızda sunulur. Portaldan kendi lisansını getir görüntüsüyle başlamanız durumunda ilgili görüntüyü kullandıkça öde modeline dönüştüremezsiniz.
 - CSP müşterileri, öncelikle bir Kullandıkça Öde VM dağıtarak ve ardından getirin-kendi lisansını için dönüştürme AHB avantajı kullanabilir. 
 - Şu anda bu özellik yalnızca genel bulut yüklemeleri için etkindir.
 - Özel bir SQL Server VM görüntüsü kaynak sağlayıcısı ile kaydederken, lisans türü = 'AHUB' olarak belirtin. Lisans bırakarak boş olarak yazın ya da 'PAYG' belirtilmesi, kayıt başarısız olmasına neden olur. 

## <a name="prerequisites"></a>Önkoşullar
SQL VM kaynak sağlayıcısı SQL Iaas uzantısı gerektirir. Bu nedenle, ile SQL VM kaynak sağlayıcısı yararlanmaya devam etmek için aşağıdakiler gerekir:
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- A *Kullandıkça Öde* [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) ile [SQL Iaas uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) yüklü. 


## <a name="register-sql-resource-provider-to-your-subscription"></a>Aboneliğinizde SQL kaynak sağlayıcısını kaydetme 

Lisans modelleri arasında geçiş yapma özelliğini (Microsoft.SqlVirtualMachine) yeni SQL VM kaynak sağlayıcısı tarafından sağlanan bir özelliktir. SQL Server Vm'leri aralık 2018'e otomatik olarak kayıtlı sonra yeni bir kaynak sağlayıcısı ile dağıtılabilir. Ancak, bu tarihten önce dağıtılan mevcut sanal makinelerin, bir lisanslama modeli geçiş yönetmeden önce el ile kaynak sağlayıcısına kayıtlı gerekir. 

  > [!NOTE] 
  > SQL Server VM kaynağınızı sürüklerseniz, görüntü sabit kodlanmış lisans ayarına geri geçer. 

SQL Server VM'nize SQL kaynak sağlayıcısı ile kaydolmak için aboneliğinize kaynak sağlayıcısını kaydetmeniz gerekir. Azure portalı, Azure CLI veya PowerShell ile bunu yapabilirsiniz. 

### <a name="with-the-azure-portal"></a>Azure portal ile
Aşağıdaki adımlar, Azure portalını kullanarak Azure aboneliğinizde SQL kaynak sağlayıcısı kaydeder. 

1. Azure portalını açın ve gidin **tüm hizmetleri**. 
1. Gidin **abonelikleri** ve ilgi aboneliği seçin.  
1. İçinde **abonelikleri** dikey penceresinde gidin **kaynak sağlayıcıları**. 
1. Tür `sql` filtresindeki SQL ile ilgili kaynak sağlayıcıları taşıyın. 
1. Şunlardan birini seçin *kaydetme*, *yeniden kaydettirin*, veya *Unregister* için **Microsoft.SqlVirtualMachine** sağlayıcısına bağlı olarak, İstenen eylem. 

   ![Sağlayıcı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="with-azure-cli"></a>Azure CLI ile
Aşağıdaki kod parçacığı, Azure aboneliğinizde SQL kaynak sağlayıcısını kaydeder. 

```azurecli
# Register the new SQL resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="with-powershell"></a>PowerShell ile
Aşağıdaki kod parçacığı, Azure aboneliğinizde SQL kaynak sağlayıcısını kaydeder. 

```powershell
# Register the new SQL resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```


## <a name="register-sql-server-vm-with-sql-resource-provider"></a>SQL kaynak sağlayıcısı ile SQL Server sanal Makinesini kaydetme
Aboneliğinizde SQL kaynak sağlayıcısı kaydedildikten sonra kaynak sağlayıcısı ile SQL Server VM'nize kaydedebilirsiniz. Azure CLI ve PowerShell kullanarak bunu yapabilirsiniz. 

### <a name="with-azure-cli"></a>Azure CLI ile

SQL Server aşağıdaki kod parçacığı ile Azure CLI kullanarak VM kaydedin: 

```azurecli
# Register your existing SQL Server VM with the new resource provider
az sql vm create -n <VMName> -g <ResourceGroupName> -l <VMLocation>
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki kod parçacığı ile PowerShell kullanarak SQL Server VM kaydedin: 

```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

## <a name="change-licensing-model"></a>Lisanslama modelini değiştirme

Kaynak sağlayıcısı ile SQL Server VM'nize kaydedildikten Azure portalı, Azure CLI veya PowerShell kullanarak bir lisanslama modeli değiştirebilirsiniz. 

### <a name="with-the-azure-portal"></a>Azure portal ile

Lisanslama modeli Portalı'ndan doğrudan değiştirebilirsiniz. 

1. SQL Server VM'nize içinde gidin [Azure portalında](https://portal.azure.com). 
1. Seçin **SQL Server Yapılandırması** içinde **ayarları** bölmesi. 
1. Seçin **Düzenle** içinde **SQL Server Lisansı** bölmesinde lisansı değiştirin. 

![Portalında AHB](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)

  >[!NOTE]
  > Bu seçenek getirin-kendi lisansını görüntüler için kullanılabilir değildir. 

### <a name="with-azure-cli"></a>Azure CLI ile

Lisanslama modelinizin değiştirmek için Azure CLI'yı kullanabilirsiniz.  

Aşağıdaki kod parçacığı, Kullandıkça Öde lisansı modelinizi KLG (veya Azure hibrit Avantajı'nı kullanarak) geçer:

```azurecli
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Aşağıdaki kod parçacığı, Kullandıkça Öde aboneliğine KLG modelinizi geçer: 

```azurecli
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

### <a name="with-powershell"></a>PowerShell ile 

Lisanslama modelinizin değiştirmek için PowerShell kullanabilirsiniz. 

Aşağıdaki kod parçacığı, Kullandıkça Öde lisansı modelinizi KLG (veya Azure hibrit Avantajı'nı kullanarak) geçer: 

```PowerShell
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest

$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

Aşağıdaki kod parçacığı, Kullandıkça Öde aboneliğine KLG modelinizi geçer:

```PowerShell
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest

$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
``` 


## <a name="view-current-licensing"></a>Geçerli görünümü lisanslama 

Aşağıdaki kod parçacığı için SQL Server VM'nize geçerli lisanslama modelinizin görüntülemenize olanak sağlar. 

```powershell
# View current licensing model for your SQL Server VM
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>

$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
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

`Set-AzResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Bu hatayı gidermek için lisanslama modelinizin geçiş yaparken yukarıda açıklanan PowerShell kod parçacığında bu satırı açıklamadan çıkarın: 
```powershell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Azure PowerShell sürümünü doğrulamak için aşağıdaki kodu kullanın:

```powershell
Get-Module -ListAvailable -Name Azure -Refresh
```

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>'< Resource-group >' kaynak grubu altında ' Microsoft.SqlVirtualMachine/SqlVirtualMachines/ < resource-group >' kaynağı bulunamadı. Bu nesne üzerinde ' % s'özelliği 'sqlServerLicenseType' bulunamıyor. Özelliği var ve ayarlanabilir doğrulayın.
SQL kaynak sağlayıcısına kayıtlı değil bir SQL Server VM üzerinde lisanslama modelini değiştirme girişimi olduğunda bu hata oluşur. İçin kaynak sağlayıcısını kaydetmeniz gerekir, [abonelik](#register-sql-resource-provider-to-your-subscription)ve ardından SQL Server VM'nize SQL ile kaydetme [kaynak sağlayıcısı](#register-sql-server-vm-with-sql-resource-provider). 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server üzerindeki bir Windows VM ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Fiyatlandırma Kılavuzu bir Windows VM'de SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server Windows VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


