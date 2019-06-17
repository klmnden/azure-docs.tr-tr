---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineler - genel bakış | Microsoft Docs
description: Bu makalede, Azure sanal makinelerinde SQL Server kullanılabilirlik gruplarını tanıtılmaktadır.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
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
ms.openlocfilehash: b9977965dc076ec36aa90680a1732b6640b1e41a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60325819"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde ile tanışın #

Bu makalede, SQL Server kullanılabilirlik gruplarını Azure sanal Makineler'de tanıtılmaktadır. 

Always On kullanılabilirlik gruplarının Azure sanal makinelerinde Always On kullanılabilirlik grupları şirket içi benzerdir. Daha fazla bilgi için [Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Diyagram, tam SQL Server kullanılabilirlik grubu Azure sanal Makineler'de parçalarını gösterir.

![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Anahtar kullanılabilirlik grubu için Azure sanal Makineler'de Azure sanal makinelerini gerektiğini fark bir [yük dengeleyici](../../../load-balancer/load-balancer-overview.md). Yük dengeleyicinin IP adresleri için kullanılabilirlik grubu dinleyicisi tutar. Birden fazla kullanılabilirlik grubu varsa, her grubun bir dinleyici gerektirir. Bir yük dengeleyici birden çok dinleyici destekleyebilir.

Ayrıca, bir Azure Iaas VM Konuk yük devretme kümesinde sunucu (küme düğümü) başına tek bir NIC'ye ve tek bir alt ağ öneririz. Azure ağındaki fiziksel yedeklilik nedeniyle Azure IaaS VM konuk kümesinde ek NIC ve alt ağ kullanılmasına gerek yoktur. Küme doğrulama raporu, düğümlerin yalnızca tek bir ağ üzerinde erişilebilir durumda olduğuna dair bir uyarı gösterse de bu uyarı Azure IaaS VM konuk yük devretme kümelerinde güvenli bir şekilde yoksayılabilir. 

|  | Windows Server sürümü | SQL Server sürümü | SQL Server sürümü | WSFC çekirdek yapılandırma | Çok bölgeli ile DR | Birden çok alt ağ desteği | Mevcut bir AD desteği | DR ile aynı bölgede birden fazla bölge | Dist AG herhangi bir AD etki alanı desteği | Cluster dist AG desteği |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) | 2016 | 2017 </br>2016   | Üst | Bulut tanığı | Hayır | Evet | Evet | Evet | Hayır | Hayır |
| [Hızlı Başlangıç şablonları](virtual-machines-windows-sql-availability-group-quickstart-template.md) | 2016 | 2017</br>2016  | Üst | Bulut tanığı | Hayır | Evet | Evet | Evet | Hayır | Hayır |
| [Portal şablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) | 2016 </br>2012 R2 | 2016</br>2014 | Üst | Dosya paylaşımı | Hayır | Hayır | Hayır | Hayır | Hayır | Hayır |
| [El ile](virtual-machines-windows-portal-sql-availability-group-prereq.md) | Tümü | Tümü | Tümü | Tümü | Evet | Evet | Evet | Evet | Evet | Evet |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

Azure sanal Makineler'de SQL Server kullanılabilirlik grubu oluşturmak hazır olduğunuzda, aşağıdaki öğreticilere bakın.

## <a name="manually-with-azure-cli"></a>Azure CLI ile el ile
Basitlik ve dağıtım hızı açısından en iyi şekilde yapılandırmak ve bir kullanılabilirlik grubuna dağıtmak için Azure CLI kullanarak önerilen seçenektir. Azure CLI, SQL Server Vm'leri Kümeye katılan Windows Yük devretme küme oluşturma yanı sıra dinleyici ve iç Load Balancer oluşturma tüm 30 dakika içerisinde gerçekleştirilebilir. Bu seçenek yine de el ile bir kullanılabilirlik grubu oluşturulmasını gerektirir, ancak diğer tüm gerekli yapılandırma adımlarını otomatik hale getirir. 

Daha fazla bilgi için [Azure VM'deki SQL Server Always On kullanılabilirlik grubu yapılandırmak için Azure SQL VM CLI'yı kullanmak](virtual-machines-windows-sql-availability-group-cli.md). 

## <a name="automatically-with-azure-quickstart-templates"></a>Otomatik olarak Azure hızlı başlangıç şablonları ile
Azure hızlı başlangıç şablonları, Windows Yük devretme kümesi dağıtma, SQL Server Vm'leri için katılın, dinleyiciyi oluşturun ve iç Load Balancer yapılandırma için SQL VM kaynak sağlayıcısını kullanır. Bu seçenek yine de bir el ile bir kullanılabilirlik grubu ve iç yük dengeleyici (ILB) oluşturulmasını gerektirir ancak otomatikleştirir ve diğer tüm gerekli yapılandırma adımları (ILB yapılandırılması dahil olmak üzere) basitleştirir. 

Daha fazla bilgi için [kullanımı Azure hızlı başlangıç Azure VM'deki SQL Server Always On kullanılabilirlik grubu yapılandırmak için şablonu](virtual-machines-windows-sql-availability-group-quickstart-template.md).


## <a name="automatically-with-an-azure-portal-template"></a>Azure Portal şablonuyla otomatik olarak

[Always On kullanılabilirlik grubu Azure VM'de otomatik olarak - Kaynak Yöneticisi'ni yapılandırma.](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)


## <a name="manually-in-azure-portal"></a>Azure portalında el ile

Sanal makineleri kendiniz şablonu oluşturabilirsiniz. İlk olarak, önkoşulları tamamlayın ve ardından kullanılabilirlik grubunu oluşturun. Aşağıdaki konulara bakın: 

- [Azure sanal Makineler'de SQL Server Always On kullanılabilirlik grupları önkoşullarını yapılandırın](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Her zaman üzerinde kullanılabilirlik kullanılabilirlik ve olağanüstü durum kurtarma geliştirmek için grubu oluştur](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Sonraki adımlar

[Bir SQL Server Always On'u farklı bölgelerdeki Azure sanal makinelerinde kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md)
