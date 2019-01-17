---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineler - genel bakış | Microsoft Docs
description: Bu makalede, Azure sanal makinelerinde SQL Server kullanılabilirlik gruplarını tanıtılmaktadır.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: 5f8ae6d9138a7413b0cca4cca7bcc47c13212674
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358060"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde ile tanışın #

Bu makalede, SQL Server kullanılabilirlik gruplarını Azure sanal Makineler'de tanıtılmaktadır. 

Always On kullanılabilirlik gruplarının Azure sanal makinelerinde Always On kullanılabilirlik grupları şirket içi benzerdir. Daha fazla bilgi için [Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Diyagram, tam SQL Server kullanılabilirlik grubu Azure sanal Makineler'de parçalarını gösterir.

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Anahtar kullanılabilirlik grubu için Azure sanal Makineler'de Azure sanal makinelerini gerektiğini fark bir [yük dengeleyici](../../../load-balancer/load-balancer-overview.md). Yük dengeleyicinin IP adresleri için kullanılabilirlik grubu dinleyicisi tutar. Birden fazla kullanılabilirlik grubu varsa, her grubun bir dinleyici gerektirir. Bir yük dengeleyici birden çok dinleyici destekleyebilir.

Ayrıca, bir Azure Iaas VM Konuk yük devretme kümesinde sunucu (küme düğümü) başına tek bir NIC'ye ve tek bir alt ağ öneririz. Azure ağı Ek NIC ve alt ağları gereksiz bir Azure Iaas VM Konuk kümede olmasını sağlayan fiziksel yedeklilik sahiptir. Küme doğrulama raporunu düğümlere yalnızca tek bir ağda erişilebilir durumda bir uyarı verir ancak Azure Iaas VM Konuk yük devretme kümelerinde bu uyarıyı güvenle yoksayılabilir. 

Azure sanal Makineler'de SQL Server kullanılabilirlik grubu oluşturmak hazır olduğunuzda, aşağıdaki öğreticilere bakın.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Otomatik olarak bir şablondan bir kullanılabilirlik grubu oluşturun

[Always On kullanılabilirlik grubu Azure VM'de otomatik olarak - Kaynak Yöneticisi'ni yapılandırma.](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Azure portalını kullanarak bir kullanılabilirlik grubuna el ile oluşturma

Sanal makineleri kendiniz şablonu oluşturabilirsiniz. İlk olarak, önkoşulları tamamlayın ve ardından kullanılabilirlik grubunu oluşturun. Aşağıdaki konulara bakın: 

- [Azure sanal Makineler'de SQL Server Always On kullanılabilirlik grupları önkoşullarını yapılandırın](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Her zaman üzerinde kullanılabilirlik kullanılabilirlik ve olağanüstü durum kurtarma geliştirmek için grubu oluştur](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Sonraki adımlar

[Bir SQL Server Always On'u farklı bölgelerdeki Azure sanal makinelerinde kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md).
