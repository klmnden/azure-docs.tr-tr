---
title: Azure SQL veritabanı yönetilen örneği özel DNS | Microsoft Docs
description: Bu konuda, bir Azure SQL veritabanı yönetilen örneği ile özel bir DNS yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: d5bb2f2f4b79c4b03e631fc844a712f76fc69109
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258176"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Özel DNS yapılandırma için Azure SQL veritabanı yönetilen örneği

Azure SQL veritabanı yönetilen örneği (Önizleme) içinde bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bazı senaryolarda, Bulut veya karma ortamınızdaki diğer SQL örneklerine bağlı sunucular vardır, yönetilen örneğinden çözülmesi özel ana bilgisayar adları gerektirir. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. Yönetilen örneği için kendi iç işleyişini aynı DNS kullandığından, sanal ağ DNS yapılandırmasını yönetilen örneği ile uyumlu olması gerekir. 

Özel DNS yapılandırma yönetilen örneği ile uyumlu hale getirmek için aşağıdaki adımları tamamlamanız gerekir: 
- Azure DNS istekleri iletmek için özel DNS yapılandırma 
- VNet için özel DNS olarak birincil ve ikincil olarak Azure DNS ayarlayın 
- Azure DNS ve özel DNS birincil olarak ikincil olarak kaydetme

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Azure DNS istekleri iletmek için özel DNS yapılandırma 

Windows Server 2016'da DNS iletme yapılandırmak için aşağıdaki adımları kullanın: 

1. İçinde **Sunucu Yöneticisi'ni**, tıklayın **Araçları**ve ardından **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Çift **ileticileri**.

   ![İleticileri](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. **Düzenle**’ye tıklayın. 

   ![İleticiler listesi](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Azure'nın yinelemeli Çözümleyicileri IP adresi gibi 168.63.129.16 girin.

   ![Özyinelemeli Çözümleyicileri IP adresi](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Özel DNS olarak birincil ve ikincil olarak Azure DNS ayarlama 
 
Bir Azure sanal ağı DNS yapılandırma, IP adreslerini girin, bu nedenle aşağıdaki sonraki adımlar kullanarak statik bir IP adresi ile DNS sunucusunu barındıran Azure VM'yi yapılandırmak gerektirir: 

1. Azure portalında özel DNS VM ağ arabirimini açın.

   ![Ağ arabirimi](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. IP yapılandırma bölümü. IP yapılandırması 

   ![IP yapılandırması](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Özel IP adresi, statik ayarlayın. IP adresi (Bu ekran üzerinde 10.0.1.5'i) azaltma 

   ![statik](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Birincil olarak özel DNS ve Azure DNS, ikincil olarak kaydetme 

1. Azure portalında sanal ağınızdaki özel DNS seçeneği bulun.

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Özel anahtar ve özel DNS sunucusu IP adresinizi ve bunun yanı sıra Azure'nın yinelemeli Çözümleyicileri IP adresi gibi 168.63.129.16 girin. 

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Azure'nın yinelemeli çözümleyici DNS ayarları listesi hatalı durumuna girmeyi yönetilen örneğe neden olur. Gelen durum yeni bir örneğini bir sanal ağ ile uyumlu ağ ilkeleri oluşturmanıza olanak gerektirebilir kurtarma, örnek düzeyinde veri oluşturun ve veritabanlarınızı geri yüklemek. Bkz: [VNet yapılandırmasını](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren bir öğretici için bkz [bir yönetilen örnek oluşturma](sql-database-managed-instance-create-tutorial-portal.md).
- Bir yönetilen örnek için bir VNet yapılandırma hakkında daha fazla bilgi için bkz: [yönetilen örnekleri için sanal ağ yapılandırması](sql-database-managed-instance-vnet-configuration.md)
