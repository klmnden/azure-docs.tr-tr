---
title: Azure Linux Sanal Makinelerinde SQL Server'a Genel Bakış | Microsoft Docs
description: Azure Linux sanal makinelerinde tam SQL Server sürümlerini çalıştırma hakkında bilgi edinin. Tüm Linux SQL Server VM görüntülerinin ve ilgili içeriklerin doğrudan bağlantılarını alın.
services: virtual-machines-linux
documentationcenter: ''
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.workload: iaas-sql-server
ms.date: 03/22/2018
ms.author: jroth
ms.openlocfilehash: e752ad844a6efe572564e7081ebac87193e9c2a7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Azure Sanal Makinelerinde SQL Server'a Genel Bakış (Linux)

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](sql-server-linux-virtual-machines-overview.md)

Bu konu başlığı, Azure Linux sanal makinelerinde (VM'ler) SQL Server çalıştırmaya yönelik seçeneklerle birlikte [portal görüntülerinin bağlantılarını](#create) içermektedir.

> [!NOTE]
> SQL Server'ı zaten biliyor ve yalnızca bir SQL Server Linux VM'sinin nasıl dağıtılacağını görmek istiyorsanız bkz. [Azure'da Linux SQL Server VM'si sağlama](provision-sql-server-linux-virtual-machine.md). SQL Server içeren bir Windows VM oluşturmak istiyorsanız bkz. [Azure'da Windows SQL Server VM'si sağlama](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md).

Veritabanı yöneticisi veya geliştiriciyseniz Azure VM’leri, şirket içi SQL Server iş yüklerinizi ve uygulamalarınızı Buluta taşımanız için bir yöntem sağlar.

## <a name="scenarios"></a>Senaryolar

Verilerinizi Azure’da barındırmayı seçmeniz için birçok neden vardır. Uygulamanızı Azure'da geliştirir veya oraya geçirirseniz Azure arka uç verilerini bulma performansı da iyileştirilir. Küresel bir erişim ağı ve olağanüstü durum kurtarma için birden fazla veri merkezine otomatik olarak erişebilirsiniz. Ayrıca veriler son derece güvenilir ve dayanıklı olur.

Azure VM’lerinde çalışan SQL Server, ilişkisel verilerinizi Azure’da depolamak için yararlanabileceğiniz bir seçenektir. Azure SQL Veritabanı hizmetini de kullanabilirsiniz. Sanal Makineler üzerinde SQL Server ile Azure SQL Veritabanı arasında seçim yapmanıza yardımcı olacak bilgiler için bkz. [Bulut SQL Server seçeneği belirleme: Azure SQL (PaaS) Veritabanı veya Azure VM'leri üzerinse SQL Server (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

## <a id="create"></a> Yeni bir SQL sanal makinesi oluşturma

Yeni SQL VM oluşturmaya yönelik adım adım yönergeleri, [Azure'da Linux SQL Server VM'si sağlama](provision-sql-server-linux-virtual-machine.md) öğreticisinde bulabilirsiniz.

Aşağıdaki tabloda sanal makine galerisindeki en son SQL Server görüntülerinin bir matrisi verilmektedir. Belirtilen sürüm, yayın ve işletim sisteminizle yeni bir SQL VM oluşturmaya başlamak için bağlantılardan birine tıklayın.

> [!TIP]
> Bu görüntülerin VM ve SQL fiyatlandırmasını anlamak için bkz. [Linux SQL Server VM'leri için fiyatlandırma kılavuzu](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

| Sürüm | İşletim Sistemi | Sürüm |
| --- | --- | --- |
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> Kullanabileceğiniz Windows SQL Server sanal makine görüntülerini görmek için bkz. [Azure Sanal Makinelerinde SQL Server'a Genel Bakış (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).

## <a id="packages"></a> Yüklü paketler

Linux üzerinde SQL Server yapılandırdıktan sonra veritabanı altyapısı paketini ve gereksinimlerinize bağlı olarak çeşitli isteğe bağlı paketleri yüklersiniz. SQL Server için Linux sanal makine görüntüleri birçok paketi otomatik olarak sizin için yükler. Aşağıdaki tabloda her dağıtımda yüklenen paketler gösterilmektedir.

| Dağıtım | [Veritabanı Altyapısı](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [Araçlar](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [SQL Server Agent](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [Tam Metin Araması](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [HA eklentisi](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![hayır](./media/sql-server-linux-virtual-machines-overview/no.png) |
| SLES | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![hayır](./media/sql-server-linux-virtual-machines-overview/no.png) | ![hayır](./media/sql-server-linux-virtual-machines-overview/no.png) |
| Ubuntu | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![evet](./media/sql-server-linux-virtual-machines-overview/yes.png) |

## <a name="next-steps"></a>Sonraki adımlar

Linux üzerinde SQL Server yapılandırma ve kullanma hakkında daha fazla bilgi için bkz. [Linux üzerinde SQL Server'a genel bakış](https://docs.microsoft.com/sql/linux/sql-server-linux-overview).
