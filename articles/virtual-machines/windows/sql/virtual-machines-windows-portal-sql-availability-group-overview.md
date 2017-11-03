---
title: "SQL Server kullanılabilirlik gruplarını - Azure sanal makineleri - genel bakış | Microsoft Docs"
description: "Bu makalede, Azure sanal makinelerde SQL Server kullanılabilirlik gruplarını tanıtılır."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
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
ms.openlocfilehash: 2cbb9ff3b2d13996b1b8dc64aa833807c264c877
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerde Tanıtımı #

Bu makalede, SQL Server kullanılabilirlik gruplarını Azure sanal makineler üzerinde tanıtılır. 

Always On kullanılabilirlik grupları Azure sanal makineler üzerinde Always On kullanılabilirlik grupları şirket içi benzerdir. Daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx). 

Diyagram tam SQL Server kullanılabilirlik grubu Azure Virtual Machines'de parçalarını gösterir.

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

En önemli fark, Azure sanal makineleri bir kullanılabilirlik grubu için Azure sanal makinelerini gerekmesidir bir [yük dengeleyici](../../../load-balancer/load-balancer-overview.md). Yük Dengeleyici için kullanılabilirlik grubu dinleyici IP adreslerini tutar. Birden çok kullanılabilirlik grubu varsa, her grubun bir dinleyici gerektirir. Bir yük dengeleyici birden çok dinleyici destekleyebilir.

Azure Virtual Machines'de SQL Server kullanılabilirlik aroup oluşturmak hazır olduğunuzda bu öğreticileri bakın.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Otomatik olarak bir şablondan bir kullanılabilirlik grubu oluşturun

[Always On kullanılabilirlik grubu Azure VM'de otomatik olarak - Resource Manager yapılandırın.](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Azure portalında bir kullanılabilirlik grubu el ile oluşturma

Sanal makineleri kendiniz olmadan şablon oluşturabilirsiniz. İlk olarak, önkoşulları tamamlamanız sonra kullanılabilirlik grubu oluşturun. Aşağıdaki konulara bakın: 

- [Azure Virtual Machines'de SQL Server Always On kullanılabilirlik grupları için önkoşulları yapılandırma](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Her zaman üzerinde kullanılabilirlik kullanılabilirlik ve olağanüstü durum kurtarma artırmak için grubu oluştur](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Sonraki adımlar

[Bir SQL Server kullanılabilirlik grubu de farklı bölgelerdeki Azure sanal makinelerde her zaman yapılandırmasına](virtual-machines-windows-portal-sql-availability-group-dr.md).
