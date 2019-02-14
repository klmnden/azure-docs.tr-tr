---
title: SQL Server Azure VM sürüm notları | Microsoft Docs
description: Azure VM'de SQL Server'ın geliştirmeleri ve yeni özellikleri hakkında bilgi edinin
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/5/2019
ms.openlocfilehash: b59ac4798260381c11ab22adb7358ff63e5e1d77
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245431"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server Azure sanal makine sürüm notları

Azure SQL Server'ın yerleşik görüntü ile bir sanal makine dağıtmanıza olanak tanır. Bu makalede yeni özellikler ve geliştirmeler son sürümlerinde özetlenir [Azure sanal makineler'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/). Makale ayrıca doğrudan ilgili sürüme ancak aynı zaman çerçevesinde yayımlanan önemli içerik güncelleştirmeleri listelenir. Diğer Azure Hizmetleri için geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates)

## <a name="december-2018"></a>Aralık 2018

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL kümesine Grup kaynak sağlayıcısı** | Windows Yük devretme kümesi meta verileri tanımlayan yeni bir kaynak sağlayıcısı (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups). Bir SQL Server VM'si için birleştirme *SqlVirtualMachineGroups* Windows Yük devretme kümesi hizmeti bootstraps ve VM kümeye ekler.  |
|**Azure hızlı başlangıç şablonları ile bir kullanılabilirlik grubu dağıtımı ayarlama otomatikleştirin** |Artık, Windows Yük devretme kümesi oluşturma, SQL Server Vm'leri için katılın, dinleyiciyi oluşturun ve iç Load Balancer iki Azure hızlı başlangıç şablonları ile yapılandırmak mümkündür. Daha fazla bilgi için [WSFC oluşturmak, dinleyiciyi ve ILB Azure Hızlı Başlangıç şablonu ile bir SQL Server VM'de Always On kullanılabilirlik grubu için yapılandırma](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Otomatik SQL VM kaynak Sağlayıcısı kaydı** | SQL Server Vm'leri bu ay otomatik olarak kayıtlı sonra yeni SQL Server Kaynak sağlayıcısı ile dağıtılabilir. SQL Server bu ay önce dağıtılan Vm'leri yine de el ile kayıtlı olması gerekir. Daha fazla bilgi için [mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
|yok | |
| | |

## <a name="november-2018"></a>Kasım 2018

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM kaynak sağlayıcısı** |  SQL Server VM'nize daha iyi yönetimine olanak sağlayan SQL Server Vm'leri (Microsoft.SqlVirtualMachine) için yeni bir kaynak sağlayıcısı. VM'nizi kaydetme ile ilgili daha fazla bilgi için bkz: [mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-sql-resource-provider). |
|**Anahtar lisanslama modeli** |Şimdi Azure CLI veya Powershell kullanarak SQL vm'nizin kullanım başına ödeme ve Getir kendi lisans modeli arasında geçiş yapabilirsiniz. Daha fazla bilgi için [bir SQL VM için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
|yok | |
| | |

## <a name="additional-resources"></a>Ek kaynaklar

**Windows Vm'leri**:

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).
* [Bir SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Bir veritabanını Azure VM'deki SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md)
* [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makineler’de SQL Server için performansa yönelik en iyi uygulamalar](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux Vm'leri**:

* [Bir Linux sanal makinesinde SQL Server'a genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bir SQL Server Linux sanal makinesi sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)