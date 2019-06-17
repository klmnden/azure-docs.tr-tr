---
title: Azure portalını kullanarak azure'da SQL Server Vm'leri yönetme | Microsoft Docs
description: Azure üzerinde barındırılan SQL Server VM için Azure portalında SQL sanal makine kaynağı erişmeyi öğrenin.
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
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: e2a807bbd6baeb2f14a6d36f5d98a28d48725449
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082722"
---
# <a name="manage-sql-server-vms-in-azure-using-the-azure-portal"></a>Azure portalını kullanarak azure'da SQL Server Vm'leri yönetme

Kullanarak Azure üzerinde SQL Server VM'nize yönetmek için yeni bir erişim noktası [Azure portalında](https://portal.azure.com). 

**SQL sanal makineleri** kaynak, artık tüm, SQL Server Vm'leri eşzamanlı olarak görüntüleyin ve adanmış SQL Server ayarlarını değiştirmenize olanak sağlayan bir bağımsız Yönetim Hizmeti: 

![SQL sanal makine kaynağı](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Açıklamalar

- **SQL sanal makineleri** görüntüleyin ve yönetin, SQL Server Vm'leri için önerilen yöntem bir kaynaktır. Ancak, şu anda **SQL sanal makineleri** kaynak yönetimini desteklemez [(EOS) destek sonu](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server Vm'leri. SQL Server sanal makineleriniz EOS ayarlarını yönetmek için kullanım dışı kullanın [SQL Server yapılandırma sekmesinde](#access-sql-server-configuration-tab) yerine. 
- **SQL sanal makineleri** kaynak, yalnızca SQL Server Vm'leri için olan kullanılabilir [SQL VM kaynak sağlayıcısına kayıtlı](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-the-sql-vm-resource-provider). 


## <a name="access-sql-virtual-machine-resource"></a>Erişim SQL sanal makine kaynağı
SQL sanal makineleri kaynağa erişmek için aşağıdakileri yapın:

1. [Azure portalı](https://portal.azure.com) açın. 
1. Seçin **tüm hizmetleri**. 
1. Tür `SQL virtual machines` arama kutusuna.
1. (İsteğe bağlı): Yanındaki yıldızı seçin **SQL sanal makineleri** , Sık Kullanılanlar menüsü için bu seçeneği eklenecek. 
1. Seçin **SQL sanal makineleri**. 

   ![SQL VM, sanal makineleri tüm hizmetlerin bulun.](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. Bu abonelik içinde kullanılabilir tüm SQL Server Vm'leri listeler. Başlatmak yönetmek istediğiniz seçmek **SQL sanal makineleri** kaynak. SQL Server VM'nize kolayca görünür değilse, arama kutusunu kullanın. 

![Tüm kullanılabilir SQL VM'ler](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

SQL Server VM'nize seçme açılır **SQL sanal makineleri** kaynak: 


![SQL sanal makine kaynağı](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

  > [!TIP]
  > **SQL sanal makineleri** adanmış SQL Server ayarlarını bir kaynaktır. VM adını seçin **sanal makine** alan belirli VM, ancak SQL Server için özel ayarları'na gidin. 

## <a name="access-sql-server-configuration-tab"></a>Erişim SQL Server yapılandırma sekmesi
SQL Server yapılandırma sekmesinde kullanım dışıdır. İsteğe bağlı olarak şu anda yönetmek için tek yöntem olan [(EOS) destek sonu](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server Vm'leri ve olmayan bir SQL Server Vm'leri [SQL VM kaynak sağlayıcısına kayıtlı](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-the-sql-vm-resource-provider).

Kullanım dışı SQL server yapılandırma sekmesine erişmek için giderek gerekecektir **sanal makineler** kaynak. Bunu yapmak için aşağıdakileri yapın:

1. [Azure portalı](https://portal.azure.com) açın. 
1. Seçin **tüm hizmetleri**. 
1. Tür `virtual machines` arama kutusuna.
1. (İsteğe bağlı): Yanındaki yıldızı seçin **sanal makineler** , Sık Kullanılanlar menüsü için bu seçeneği eklenecek. 
1. Seçin **sanal makineler**. 

   ![Sanal makineler için arama](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. Bu Abonelikteki tüm sanal makineleri listeler. Başlatmak yönetmek istediğiniz seçmek **sanal makine** kaynak. SQL Server VM'nize kolayca görünür değilse, arama kutusunu kullanın. 
1. Seçin **SQL Server Yapılandırması** içinde **ayarları** bölmesinde SQL sunucunuzu yönetmek için. 

![SQL Server yapılandırması](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server üzerindeki bir Windows VM ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Fiyatlandırma Kılavuzu bir Windows VM'de SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server Windows VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


