---
title: SQL Server Azure VM sürüm notları | Microsoft Docs
description: Azure VM'de SQL Server'ın geliştirmeleri ve yeni özellikleri hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2018
ms.author: mathoma
ms.openlocfilehash: 7c854759e9a0ba13b698dd07dabb25100895bc85
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766886"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server Azure sanal makine sürüm notları

Azure SQL Server'ın yerleşik görüntü ile bir sanal makine dağıtmanıza olanak tanır. Bu makalede, dağıtılan Azure sanal makinesinde SQL Server'ın en son sürümünde bekleyebileceğiniz yenilikleri ve yeni özellikleri listeler. 

## <a name="december-2018"></a>Aralık 2018

| **Değişiklik** | Ayrıntılar |
| --- | --- |
| **Yeni SQL kümesine Grup kaynak sağlayıcısı** | Windows Yük devretme kümesi hakkındaki meta verileri tanımlayan yeni bir kaynak sağlayıcısı (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) yoktur. Bir SQL Server VM'si için birleştirme *SqlVirtualMachineGroups* Windows Yük devretme kümesi hizmeti bootstraps ve VM kümeye ekler.  |
|**Azure hızlı başlangıç şablonları ile bir kullanılabilirlik grubu dağıtımı ayarlama otomatikleştirin** |Artık, Windows Yük devretme kümesi oluşturma, SQL Server Vm'leri için katılın, dinleyiciyi oluşturun ve iç Load Balancer iki Azure hızlı başlangıç şablonları ile yapılandırmak mümkündür. Daha fazla bilgi için [WSFC oluşturmak, dinleyiciyi ve ILB Azure Hızlı Başlangıç şablonu ile bir SQL Server VM'de Always On kullanılabilirlik grubu için yapılandırma](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Otomatik SQL VM kaynak Sağlayıcısı kaydı** | SQL Server Vm'leri bu ay otomatik olarak kayıtlı sonra yeni SQL Server Kaynak sağlayıcısı ile dağıtılabilir. SQL Server bu ay önce dağıtılan Vm'leri yine de el ile kayıtlı olması gerekir. Daha fazla bilgi için [mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Kasım 2018

| **Değişiklik** | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM kaynak sağlayıcısı** |  İçin SQL Server VM'nize daha iyi yönetimine olanak tanıyan SQL Server Vm'leri (Microsoft.SqlVirtualMachine) için yeni bir kaynak sağlayıcısı yok. VM'nizi kaydetme ile ilgili daha fazla bilgi için bkz: [mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-sql-resource-provider). |
|**Anahtar lisanslama modeli** |Şimdi Azure CLI veya Powershell kullanarak SQL vm'nizin kullanım başına ödeme ve Getir kendi lisans modeli arasında geçiş yapabilirsiniz. Daha fazla bilgi için [bir SQL VM için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


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
* [SQL Server Linux VM'si sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
