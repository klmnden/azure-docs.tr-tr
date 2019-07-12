---
title: Azure'da bir SQL Server sanal makinesi için lisanslama modelini değiştirme
description: Azure 'Kullandıkça Öde' den bir SQL sanal makinesi için lisanslama nasıl değiştireceğinizi öğrenin 'getirin-kendi kullanarak Azure hibrit avantajı lisansını için'.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
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
ms.openlocfilehash: 78ad784a45d2b0063932791daedc9b1ec1aafd72
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786757"
---
# <a name="how-to-change-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Azure'da bir SQL Server sanal makinesi için lisanslama modelini değiştirme
Bu makalede yeni kullanarak Azure'da bir SQL Server sanal makine için lisans modeli değiştirmek nasıl SQL VM kaynak sağlayıcısı - **Microsoft.SqlVirtualMachine**.

İki lisanslama modelleri barındıran SQL Server - sanal makine (VM) için Kullandıkça Öde ve Azure hibrit Avantajı'nı (AHB) vardır. Ve şimdi, Azure portalı, Azure CLI veya PowerShell kullanarak SQL Server VM'nize lisanslama modelini değiştirebilirsiniz. 

**Kullandıkça Öde** (PAYG) modeli anlamına gelir saniye başına maliyeti Azure VM'de çalışan SQL Server Lisans maliyetini içerir.
[Azure hibrit Avantajı'nı (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) SQL Server çalıştıran bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak tanır. 

SQL Server için Microsoft Azure hibrit avantajı, Yazılım Güvencesi ("tam lisans") Azure Virtual Machines'de SQL Server lisanslarınızı kullanarak sağlar. SQL Server için Azure hibrit avantajı, müşterilerin VM üzerinde SQL Server lisans kullanımı için ücretlendirilmez, ancak bunlar yine de temel bulut bilgi işlem (taban ücretinin), depolama ve yedekler, hem de onların u ile ilişkili g/ç maliyeti için ödeme gerekir SE hizmetlerin (uygunsa gibi).

Microsoft Ürün Koşulları'nı göre "müşterileri, Azure SQL veritabanı (yönetilen örneği, elastik havuz ve tek veritabanı) kullanıyorsanız belirtmelisiniz Azure Data Factory, SQL Server Integration Services veya SQL Server sanal makineleri Azure hibrit altında SQL Server için avantajı iş yüklerini Azure üzerinde yapılandırırken."

Azure vm'lerde SQL Server için Azure hibrit avantajı kullanımını gösterir ve uyumlu olması için üç seçenek vardır:

1. Yalnızca Kurumsal anlaşma kapsamında olan müşteriler için kullanılabilir Azure Market'ten bir SQL Server KLG görüntüsü kullanarak bir sanal makine sağlayın.
1. Azure Market'ten PAYG SQL sunucu görüntüsü kullanarak bir sanal makine sağlama ve AHB etkinleştirin.
1. Bir Azure VM'de SQL Server Self el ile yükleme [kendi SQL Server sanal Makinesini kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md) AHB etkinleştirin.

SQL Server'ın lisans türü, VM hazırlandığında ve daha sonra dilediğiniz zaman değiştirilebilir olduğunda ayarlanır. Lisans modelleri arasında geçiş doğurur **kapalı kalma süresi olmadan**, VM başlatmaz, ekler **ek ücret ödemeden** (aslında AHB etkinleştirme *azaltır* maliyeti) ve **hemen etkili**. 

## <a name="prerequisites"></a>Önkoşullar

SQL VM kaynak sağlayıcısı SQL Iaas uzantısı gerektirir. Bu nedenle, ile SQL VM kaynak sağlayıcısı yararlanmaya devam etmek için aşağıdakiler gerekir:
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- A [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) kayıtlı [SQL VM kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md) yüklü. 


## <a name="change-license-for-vms-already-registered-with-resource-provider"></a>Kaynak sağlayıcısına zaten kayıtlı olan VM'ler için lisansını değiştirme 

# <a name="azure-portaltabazure-portal"></a>[Azure portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Lisanslama modeli Portalı'ndan doğrudan değiştirebilirsiniz. 

1. Açık [Azure portalında](https://portal.azure.com) ve başlatma [SQL sanal makineleri kaynak](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) için SQL Server VM'nize. 
1. Seçin **yapılandırma** altında **ayarları**. 
1. Seçin **Azure hibrit avantajı** seçenek ve Yazılım Güvencesi içeren SQL Server Lisansı sahip olduğunuzu onaylamak üzere onay kutusunu işaretleyin. 
1. Seçin **Uygula** kısmındaki **yapılandırma** sayfası. 

![Portalında AHB](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)

Lisanslama modelinizin değiştirmek için Azure CLI'yı kullanabilirsiniz.  

Aşağıdaki kod parçacığı, Kullandıkça Öde lisansı modelinizi KLG (veya Azure hibrit Avantajı'nı kullanarak) geçer:

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Aşağıdaki kod parçacığı, Kullandıkça Öde aboneliğine getirin-kendi lisansını modelinizi geçer: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Lisanslama modelinizin değiştirmek için PowerShell kullanabilirsiniz.

Aşağıdaki kod parçacığı, Kullandıkça Öde lisansı modelinizi KLG (veya Azure hibrit Avantajı'nı kullanarak) geçer:

```powershell-interactive
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

```powershell-interactive
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
---

## <a name="change-license-for-vms-not-registered-with-resource-provider"></a>Kaynak sağlayıcısına kayıtlı değil VM'ler için lisansını değiştirme

Ardından SQL Server VM PAYG Azure Market görüntülerinden sağladıysanız PAYG SQL lisans türü olacaktır. Ardından Azure Market'te KLG görüntüsü kullanarak bir SQL Server VM'si sağladıysanız AHUB lisans türü olacaktır. Varsayılan (PAYG) tüm SQL Server Vm'lerinin sağlanan ya da Azure Market'te KLG görüntüleri otomatik olarak kayıtlı SQL VM kaynak sağlayıcısı ile değiştirebilmeleri [lisans türü](#change-license-for-vms-already-registered-with-resource-provider)

Yalnızca kendi Azure hibrit avantajı aracılığıyla Azure vm'lerde SQL Server yüklemek uygun olan ve gerekir [bu Vm'leri ile SQL VM kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md) AHB kullanım göre göstermek için AHB olarak SQL Server Lisans ayarlayarak Microsoft Ürün koşulları.

SQL VM ile SQL VM kaynak sağlayıcısı kayıtlı değilse, yalnızca SQL Server VM PAYG veya AHB olarak lisans türünü değiştirebilirsiniz; ve tüm SQL VM'ler ile SQL VM RP lisans uyumluluğu için kaydedilmelidir.

## <a name="remarks"></a>Açıklamalar

 - Azure Cloud Solution Partner (CSP) müşterileri Azure hibrit avantajı, öncelikle bir Kullandıkça Öde VM dağıtma ve bunların etkin SA varsa getirin-kendi lisansını için dönüştürme kullanabilir.
 - SQL Server VM kaynağınızı sürüklerseniz, yeniden görüntü sabit kodlanmış lisans ayarına geçer. 
  - Lisanslama modelini değiştirme özelliği, SQL VM kaynak Sağlayıcısı'nın bir özelliğidir. Bir SQL Server VM, Azure portalı üzerinden bir Market görüntüsü otomatik olarak dağıtma ile kaynak sağlayıcısını kaydeder. Bununla birlikte, müşteriler, şirket içinde SQL Server yükleme el ile gerekecektir [kendi SQL Server sanal Makinesini kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md). 
- Bir SQL Server VM bir kullanılabilirlik kümesine ekleme, sanal Makineyi yeniden oluşturmayı gerektirir. Kullanılabilirlik eklenmiş gibi tüm Vm'leri olarak kümesi varsayılan bir Kullandıkça Öde lisans türü için geri gider ve AHB yeniden etkinleştirilmesi gerekir. 


## <a name="limitations"></a>Sınırlamalar

 - Lisanslama modelini değiştirme yalnızca Yazılım Güvencesine sahip müşteriler için kullanılabilir.
 - Lisanslama modelini değiştirme yalnızca SQL Server'ın standard ve enterprise Edition'da desteklenir. Lisans değişiklikleri Express ve Web geliştirme için desteklenmez. 
 - Lisanslama modelini değiştirme yalnızca Resource Manager modeli kullanılarak dağıtılan sanal makineler için desteklenir. Klasik modeli kullanarak dağıtılan Vm'leri desteklenmiyor. 
 - Lisanslama modelini değiştirme yalnızca genel bulut yüklemeleri için etkindir.
 - Lisanslama modelini değiştirme yalnızca tek bir NIC'ye (ağ arabirimi) sahip sanal makineler üzerinde desteklenir. Birden fazla NIC içeren sanal makineler üzerinde ilk birini NIC'ler (Azure portalı kullanarak) kaldırmanız, yordam denemeden önce. Aksi halde, aşağıdakine benzer bir hata çalışacaktır: `The virtual machine '\<vmname\>' has more than one NIC associated.` Lisanslama modu değiştirdikten sonra VM NIC eklemeniz mümkün olabilir, ancak Azure portalında, otomatik düzeltme eki uygulama ve yedekleme gibi SQL yapılandırma sayfası aracılığıyla yapılan işlemleri artık değerlendirilip onaylanır desteklenir.


## <a name="known-errors"></a>Bilinen hataları

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>Kaynak ' Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group >' kaynak grubu altında '\<resource-group >' bulunamadı. Bu nesne üzerinde ' % s'özelliği 'sqlServerLicenseType' bulunamıyor. Özelliği var ve ayarlanabilir doğrulayın.
SQL VM kaynak sağlayıcısına kayıtlı değil bir SQL Server VM üzerinde lisanslama modelini değiştirme girişimi olduğunda bu hata oluşur. İçin kaynak sağlayıcısını kaydetmeniz gerekir, [abonelik](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-vm-resource-provider-with-subscription)ve ardından SQL Server VM'nize SQL ile kaydetme [kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>'Sku' parametresindeki bağımsız değişken doğrulanamıyor
SQL Server VM'nin lisanslama modelinizin, Azure PowerShell kullanırken değiştirmeye çalışırken bu hatayla karşılaşabilirsiniz > 4.0: `Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Bu hatayı gidermek için lisanslama modelinizin geçiş yaparken yukarıda açıklanan PowerShell kod parçacığında bu satırı açıklamadan çıkarın:

  ```powershell-interactive
  # the following code snippet is necessary if using Azure Powershell version > 4
  $SqlVm.Kind= "LicenseChange"
  $SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
  $SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
  ```
  
Azure PowerShell sürümünü doğrulamak için aşağıdaki kodu kullanın:
  
  ```powershell-interactive
  Get-Module -ListAvailable -Name Azure -Refresh
  ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server üzerindeki bir Windows VM ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Fiyatlandırma Kılavuzu bir Windows VM'de SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server Windows VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


