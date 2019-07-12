---
title: SQL Server sanal makine Azure'da SQL VM kaynak sağlayıcısı ile kaydetme | Microsoft Docs
description: SQL Server VM'nize yönetilebilirlik iyileştirmek için SQL VM kaynak sağlayıcısını kaydedin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 7d7d04299c2d6c7a498701df9fb39c8990484a79
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807265"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>SQL Server sanal makine Azure'da ile SQL VM kaynak sağlayıcısını kaydedin.

Bu makalede, Azure SQL Server sanal makinesi (VM) ile SQL VM kaynak sağlayıcısını kaydetme. 

SQL Server VM, Azure portalı üzerinden bir SQL Server VM Market görüntüsü otomatik olarak dağıtma ile kaynak sağlayıcısını kaydeder. SQL Server yerine Azure Marketi'nden bir görüntü seçerek Azure sanal makinesi üzerinde Self yüklemeyi seçerseniz, ancak daha sonra SQL Server VM'nize bugün kaynak sağlayıcısı ile kayıt işlemi:

-  **Uyumluluk** – kullanan müşteriler, Microsoft Ürün koşulları [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) Azure hibrit avantajı - kullanırken, Microsoft ve ile SQL VM kaynak sağlayıcısını kaydetmeniz gerekir Bunu yapmak için belirtmeniz gerekir. 

-  **Özellik avantajları** -kaynak sağlayıcısı ile SQL Server VM'nize kaydetme kilidini açarak [otomatik düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md), [otomatik yedekleme](virtual-machines-windows-sql-automated-backup-v2.md), yanısıra,izlemeveyönetilebilirliközellikleri[lisanslama](virtual-machines-windows-sql-ahb.md) ve [edition](virtual-machines-windows-sql-change-edition.md) esneklik. Daha önce bu yalnızca SQL Server VM görüntüleri Azure Marketi'nde kullanılabilir.

Azure VM'de SQL Server Self yükleme veya Azure VM'deki SQL Server ile özel bir VHD'den sağlama müşteriler ile SQL VM kaynak kaydederek kullanan Microsoft belirtmek koşul ile SQL Server için Azure hibrit avantajı ile uyumludur. Sağlayıcı. 

SQL VM kaynak sağlayıcısını kullanmak için Ayrıca, aboneliğiniz ile SQL VM kaynak sağlayıcısını kaydetmeniz gerekir. Bu, Azure portalı, Azure CLI ve PowerShell ile gerçekleştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar

SQL Server VM'nize kaynak sağlayıcısı ile kaydolmak için aşağıdakiler gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- A [SQL Server sanal makinesi](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [Azure CLI](/cli/azure/install-azure-cli) ve [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>SQL VM ile kaynak sağlayıcısını kaydetme
Varsa [SQL Iaas uzantısı](virtual-machines-windows-sql-server-agent-extension.md) ile SQL VM kaynak sağlayıcısı kaydediliyor yalnızca bir meta veri kaynağı türü Microsoft.SqlVirtualMachine/SqlVirtualMachines oluşturuyor sonra sanal makinede zaten yüklü. SQL Iaas uzantısı VM'de yüklü ise SQL VM kaynak sağlayıcısı ile kaydolmak için kod parçacığı aşağıdadır. SQL Server Lisans ile SQL sanal makinesi olarak ya da kaynak Sağlayıcısı kaydı sırasında istenen türünü sağlamanız gereken ' PAYG ' veya 'AHUB'. 

Aşağıdaki kod parçacığı ile PowerShell kullanarak SQL Server VM kaydedin:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'}  
  
  ```

SQL Iaas uzantısı VM'de yüklü değilse, basit SQL Yönetim modu belirterek SQL VM kaynak sağlayıcısı ile kaydedebilirsiniz. Basit SQL Yönetim modunda SQL VM kaynak sağlayıcısı otomatik olarak yüklemeniz SQL Iaas uzantısı'nda [basit mod](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) ve SQL Server doğrulama örnek meta veri; bu işlem, SQL Server hizmetini başlatın değil. SQL Server Lisans ile SQL sanal makinesi olarak ya da kaynak Sağlayıcısı kaydı sırasında istenen türünü sağlamanız gereken ' PAYG ' veya 'AHUB'. 

PowerShell ile aşağıdaki kod parçacığını kullanarak basit SQL Yönetimi modunda SQL Server VM kaydedin:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

SQL VM kaynak sağlayıcısı ile kaydetme [basit mod](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) uyumluluk sağlamak ve esnek Lisanslamayı etkinleştirmek yanı sıra yerinde SQL Server sürümü güncelleştirmeleri. Yük devretme kümesi örnekleri ve çok örnekli dağıtımları, yalnızca basit modda SQL VM kaynak sağlayıcısı ile kaydedilebilir. Yükseltmek için Azure portalında bulunan yönergeleri izleyerek [tam modu](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) ve yönetilebilirlik kapsamlı özellik kümesi bir SQL Server yeniden başlatma ile dilediğiniz zaman etkinleştirin. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>SQL Server 2008/R2'de Windows Server 2008 Vm'leri kaydedin

SQL kaynak sağlamak, VM ile SQL Server 2008 ve 2008 R2, Windows Server 2008'de yüklü kaydedilebilir [NoAgent](virtual-machines-windows-sql-server-agent-extension.md) modu. Bu seçenek, uyumluluk sağlar ve Azure portalında sınırlı işlevsellikle izlenmesi SQL Server VM sağlar.

Aşağıdaki tabloda, kayıt sırasında sağlanan parametreleri için kabul edilebilir değerler ayrıntıları:

| Parametre | Kabul edilebilir değerler                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `'AHUB'`, veya `'PAYG'`                    |
| **sqlImageOffer**  | `'SQL2008-WS2008'` veya `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


SQL Server 2008 veya 2008 R2'de Windows Server 2008 örneğini kaydetmek için aşağıdaki Powershell kod parçacığını kullanın:  

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

## <a name="verify-registration-status"></a>Kayıt durumunu doğrulayın
SQL Server Azure portalı, Azure CLI veya PowerShell kullanarak SQL VM kaynak sağlayıcısı ile zaten kayıtlı olmadığını doğrulayabilirsiniz. 

### <a name="azure-portal"></a>Azure Portal 
Azure portalını kullanarak kayıt durumunu doğrulamak için aşağıdakileri yapın.

1. [Azure portal](https://portal.azure.com) oturum açın. 
1. Gidin, [SQL sanal makineleri](virtual-machines-windows-sql-manage-portal.md).
1. SQL Server VM'nize listeden seçin. SQL Server VM'nize burada listelenmiyorsa, SQL Server VM'nize SQL VM kaynak sağlayıcısına kayıtlı değil olasıdır. 
1. Değerin altında görüntülemek `Status`. Varsa `Status = Succeeded`, sonra da SQL Server VM ile SQL VM kaynak sağlayıcısı başarıyla kaydedildi. 

    ![SQL RP kaydı ile durumunu doğrulayın](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="az-cli"></a>Az CLI

Geçerli SQL Server VM kayıt durumu aşağıdaki AZ CLI komutu ile doğrulayın. `ProvisioningState` gösterecektir `Succeeded` kayıt başarılı olursa. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell cmdlet'i ile geçerli SQL Server VM kayıt durumunu doğrulayın. `ProvisioningState` gösterecektir `Succeeded` kayıt başarılı olursa. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
SQL Server VM kaynak sağlayıcısı ile kaydedilmemiş bir hata gösterir. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>Abonelik ile SQL VM kaynak sağlayıcısını kaydetme 

SQL Server VM'nize SQL VM kaynak sağlayıcısı ile kaydolmak için aboneliğinize kaynak sağlayıcısını kaydetmeniz gerekir. Azure portal veya Azure CLI ile bunu yapabilirsiniz.

### <a name="azure-portal"></a>Azure portal

Aşağıdaki adımlar, Azure portalını kullanarak Azure aboneliğinizde SQL VM kaynak sağlayıcısını kaydeder. 

1. Azure portalını açın ve gidin **tüm hizmetleri**. 
1. Gidin **abonelikleri** ve ilgi aboneliği seçin.  
1. Üzerinde **abonelikleri** sayfasında, gitmek **kaynak sağlayıcıları**. 
1. Tür `sql` filtresindeki SQL ile ilgili kaynak sağlayıcıları taşıyın. 
1. Şunlardan birini seçin *kaydetme*, *yeniden kaydettirin*, veya *Unregister* için **Microsoft.SqlVirtualMachine** sağlayıcısına bağlı olarak, İstenen eylem. 

   ![Sağlayıcı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="az-cli"></a>Az CLI
Aşağıdaki kod parçacığı, Azure aboneliğinizde SQL VM kaynak sağlayıcısını kaydeder. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell kod parçacığı, Azure aboneliğinizde SQL VM kaynak sağlayıcısını kaydeder.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Açıklamalar

 - SQL VM kaynak sağlayıcısı, yalnızca SQL Server ' Kaynak Yöneticisi ' kullanılarak dağıtılan Vm'leri destekler. 'Klasik modeli' desteklenmeyen'ı kullanarak SQL Server Vm'leri dağıtılabilir. 
 - SQL VM kaynak sağlayıcısı, yalnızca SQL Server genel buluta dağıtılan sanal makineleri destekler. Özel veya kamu bulut dağıtımları desteklenmez. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server üzerindeki bir Windows VM ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Fiyatlandırma Kılavuzu bir Windows VM'de SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server Windows VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


