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
ms.openlocfilehash: 44aee447c7f935cd67ca55902c53c5f6f9eb6fda
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500417"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server Azure sanal makine sürüm notları

Azure SQL Server'ın yerleşik görüntü ile bir sanal makine dağıtmanıza olanak tanır. Bu makalede, dağıtılan Azure sanal makinesinde SQL Server'ın en son sürümünde bekleyebileceğiniz yenilikleri ve yeni özellikleri listeler. 


## <a name="november-2018"></a>Kasım 2018
- **Yeni SQL kaynak sağlayıcısı**: yeni bir kaynak sağlayıcısı için sanal makinenizin daha iyi yönetimine olanak SQL VM'ler için yoktur. VM'nizi kaydetme ile ilgili daha fazla bilgi için bkz: [eski bir SQL VM ile yeni kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-ahb.md#register-legacy-sql-vm-with-new-resource-provider).
- **Lisanslama modeli geçiş**: artık Azure CLI veya Powershell kullanarak SQL vm'nizin kullanım başına ödeme ve Getir kendi lisans modeli arasında geçiş yapabilirsiniz. Daha fazla bilgi için [bir SQL VM için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md)



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
