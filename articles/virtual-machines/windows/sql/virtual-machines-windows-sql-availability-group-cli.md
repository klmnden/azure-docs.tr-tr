---
title: Azure VM'de SQL Server Always On kullanılabilirlik grubu yapılandırmak için Azure SQL VM CLI'yı kullanın
description: "Azure'da bir SQL Server VM'si Windows Yük devretme kümesi, kullanılabilirlik grubu dinleyicisi ve iç yük dengeleyici oluşturmak için Azure CLI'yi kullanın. "
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
ms.date: 02/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 058ed349e1aeb17dea7d550b9760082b464453f1
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404140"
---
# <a name="use-azure-sql-vm-cli-to-configure-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Azure VM'de SQL Server Always On kullanılabilirlik grubu yapılandırmak için Azure SQL VM CLI'yı kullanın
Bu makalede nasıl kullanılacağını [Azure SQL VM CLI](https://docs.microsoft.com/mt-mt/cli/azure/ext/sqlvm-preview/sqlvm?view=azure-cli-2018-03-01-hybrid) Windows Yük devretme kümesi (WSFC) dağıtın ve SQL Server Vm'leri kümeye eklemek, hem de iç Load Balancer ve Always On kullanılabilirlik grubu dinleyicisi oluşturmak için.  Gerçek dağıtım Always On kullanılabilirlik grubu hala el ile SQL Server Management Studio (SSMS) aracılığıyla gerçekleştirilir. 

## <a name="prerequisites"></a>Önkoşullar
Azure SQL VM CLI kullanarak bir Always On kullanılabilirlik grubunun Kurulum otomatikleştirmek için aşağıdaki önkoşulları zaten olması gerekir: 
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Bir etki alanı denetleyicisi ile bir kaynak grubu. 
- Bir veya daha fazla etki alanına katılmış [Vm'leri Azure çalışan SQL Server 2016 (veya üzeri) Enterprise Edition'da](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) içinde *aynı kullanılabilirlik kümesine veya farklı kullanılabilirlik bölgelerine* bulunmuş olabilirsiniz [kayıtlı SQL VM kaynak sağlayıcısı ile](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).  
 
## <a name="create-storage-account-as-a-cloud-witness"></a>Bulut tanığı depolama hesabı oluşturma
Küme bulut tanığı olarak görev yapacak bir depolama hesabı gerekir. Var olan herhangi bir depolama hesabı kullanabilir veya yeni bir depolama hesabı oluşturabilirsiniz. Mevcut bir depolama hesabı kullanmak istiyorsanız, sonraki bölüme atlayabilirsiniz. 

Aşağıdaki kod parçacığı, depolama hesabı oluşturur: 
```cli
# Create the storage account
# example: az storage account create -n 'cloudwitness' -g SQLVM-RG -l 'West US' `
#  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true

az storage account create -n <name> -g <resource group name> -l <region ex:eastus> `
  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true
```

   >[!TIP]
   > Hatasıyla karşılaşabilirsiniz `az sql: 'vm' is not in the 'az sql' command group` Azure CLI'ın eski bir sürüm kullanıyorsanız. İndirme [Azure CLI'nin en son sürümünü](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) bu hatayı gidermek için.

## <a name="define-windows-failover-cluster-metadata"></a>Windows Yük devretme kümesi meta verileri tanımlayan
Azure SQL VM CLI [az sql vm grubu](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest) komut grubu kullanılabilirlik grubunu barındıran Windows Yük devretme kümesi (WSFC) hizmeti meta verileri yönetir. Küme meta verileri, AD etki alanı, küme hesapları, bulut tanığı ve SQL Server sürümü kullanılacak depolama hesapları dahildir. Kullanım [az sql vm grubu oluştur](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest#az-sql-vm-group-create) ilk SQL Server VM eklendiğinde, meta veriler için WSFC tanımlamak için küme tanımlandığı şekilde oluşturulur. 

Aşağıdaki kod parçacığı, küme için meta verileri tanımlar:
```cli
# Define the cluster metadata
# example: az sql vm group create -n Cluster -l 'West US' -g SQLVM-RG `
#  --image-offer SQL2017-WS2016 --image-sku Enterprise --domain-fqdn domain.com `
#  --operator-acc vmadmin@domain.com --bootstrap-acc vmadmin@domain.com --service-acc sqlservice@domain.com `
#  --sa-key '4Z4/i1Dn8/bpbseyWX' `
#  --storage-account 'https://cloudwitness.blob.core.windows.net/'

az sql vm group create -n <cluster name> -l <region ex:eastus> -g <resource group name> `
  --image-offer <SQL2016-WS2016 or SQL2017-WS2016> --image-sku Enterprise --domain-fqdn <FQDN ex: domain.com> `
  --operator-acc <domain account ex: testop@domain.com> --bootstrap-acc <domain account ex:bootacc@domain.com> `
  --service-acc <service account ex: testservice@domain.com> `
  --sa-key '<PublicKey>' `
  --storage-account '<ex:https://cloudwitness.blob.core.windows.net/>'
```

## <a name="add-sql-server-vms-to-cluster"></a>SQL Server Vm'leri kümeye Ekle
İlk SQL Server VM kümeye ekleme, kümeyi oluşturur. [Az sql vm ekleme gruba](https://docs.microsoft.com/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-add-to-group) komutu daha önce verilen ada sahip bir küme oluşturur, SQL Server Vm'lerinde küme rolünü yükler ve bunları kümeye ekler. Sonraki kullanımları `az sql vm add-to-group` komutu, yeni oluşturulan kümeye ek SQL Server Vm'leri ekler. 

Aşağıdaki kod parçacığı, kümeyi oluşturur ve ilk SQL Server VM ekler: 

```cli
# Add SQL Server VMs to cluster
# example: az sql vm add-to-group -n SQLVM1 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!
# example: az sql vm add-to-group -n SQLVM2 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

az sql vm add-to-group -n <VM1 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
az sql vm add-to-group -n <VM2 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
```
Değiştirme yalnızca başka bir SQL Server Vm'leri, kümeye eklemek için bu komutu kullanın `-n` parametresi için SQL Server VM adı. 

## <a name="create-availability-group"></a>Kullanılabilirlik grubu oluşturun
Normalde, kullanarak yaptığınız gibi el ile kullanılabilirlik grubunu oluşturma [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell), veya [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

  >[!IMPORTANT]
  > Yapmak **değil** bu Azure CLI aşağıdaki bölümlerde aracılığıyla yapıldığından şu anda bir dinleyici oluşturun.  

## <a name="create-internal-load-balancer"></a>İç yük dengeleyici oluşturma

Always On kullanılabilirlik grubu (ağ) dinleyicisi, iç Azure yük dengeleyici (ILB) gerektirir. ILB daha hızlı yük devretme ve yeniden bağlanmayı sağlayan ağ dinleyicisi "kayan" IP adresi sunar. SQL Server Vm'leri bir kullanılabilirlik grubuna varsa aynı kullanılabilirlik kümesinin parçası ve ardından bir temel yük dengeleyici kullanabilirsiniz; Aksi takdirde, bir Standard Load Balancer'ı kullanmanız gerekir.  **ILB, SQL Server VM örnekleri ile aynı sanal ağda olmalıdır.** 

Aşağıdaki kod parçacığı, iç yük dengeleyici oluşturur:

```cli
# Create the Internal Load Balancer
# example: az network lb create --name sqlILB -g SQLVM-RG --sku Standard `
# --vnet-name SQLVMvNet --subnet default

az network lb create --name sqlILB -g <resource group name> --sku Standard `
  --vnet-name <VNet Name> --subnet <subnet name>
```

  >[!IMPORTANT]
  > Standart Load Balancer ile uyumlu olacak şekilde standart bir SKU her SQL Server VM için genel IP kaynağına sahip olmalıdır. Sanal makinenizin genel IP kaynağı SKU'su belirlemek için gidin, **kaynak grubu**seçin, **genel IP adresi** istenen SQL Server VM, kaynak ve değerin altında bulun **SKU**  , **genel bakış** bölmesi.  

## <a name="create-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi oluşturun
Kullanılabilirlik grubu el ile oluşturulduktan sonra kullanarak dinleyici oluşturabilirsiniz [az sql vm ag-listener](https://docs.microsoft.com/cli/azure/sql/vm/group/ag-listener?view=azure-cli-latest#az-sql-vm-group-ag-listener-create). 


- **Alt ağ kaynak kimliği** değeri `/subnets/<subnetname>` vNet kaynağın kaynak kimliğe. Alt ağ kaynak kimliği belirlemek için aşağıdakileri yapın:
   1. Kaynak grubunuzda gidin [Azure portalında](https://portal.azure.com). 
   1. Sanal ağ kaynağı seçin. 
   1. Seçin **özellikleri** içinde **ayarları** bölmesi. 
   1. Sanal ağın kaynak Kimliğini belirlemek ve ekleme `/subnets/<subnetname>`sonuna kadar alt ağ kaynak kimliği oluşturmak için Örneğin:
        - VNet kaynağım kimliği `/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet`.
        - Alt adım `default`.
        - Bu nedenle alt ağ kaynak Kimliğimi gereklidir `/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default`


Aşağıdaki kod parçacığı, kullanılabilirlik grubu dinleyicisi oluşturur:

```cli
# Create the AG listener
# example: az sql vm group ag-listener create -n AGListener -g SQLVM-RG `
#  --ag-name SQLAG --group-name Cluster --ip-address 10.0.0.27 `
#  --load-balancer sqlilb --probe-port 59999  `
#  --subnet /subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#  --sqlvms sqlvm1 sqlvm2

az sql vm group ag-listener create -n <listener name> -g <resource group name> `
  --ag-name <availability group name> --group-name <cluster name> --ip-address <ag listener IP address> `
  --load-balancer <lbname> --probe-port <Load Balancer probe port, default 59999>  `
  --subnet <subnet resource id> `
  --sqlvms <names of SQL VM’s hosting AG replicas ex: sqlvm1 sqlvm2>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [SQL Server sanal makinesi genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [SQL Server sanal makinesi için lisanslama modelleri değiştirme](virtual-machines-windows-sql-ahb.md)
* [Always On kullanılabilirlik grupları genel bakış &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)   
* [Sunucu örneği için Always On kullanılabilirlik grupları yapılandırmasını &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server)   
* [Bir kullanılabilirlik grubunun Yönetim &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server)   
* [Kullanılabilirlik gruplarını izleme &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server)
* [Always On kullanılabilirlik grupları için Transact-SQL deyimlerine genel bakış &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups)   
* [Always On kullanılabilirlik grupları için PowerShell cmdlet'leri bakış &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server)  
