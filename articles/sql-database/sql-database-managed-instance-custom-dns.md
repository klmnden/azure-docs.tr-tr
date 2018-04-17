---
title: Azure SQL veritabanı örneği özel DNS yönetilen | Microsoft Docs
description: Bu konuda bir özel DNS ile yönetilen bir Azure SQL veritabanı örneği için yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 3175b99c0e41cedf313115043b09608496adfdca
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Örnek yönetilen Azure SQL veritabanı için özel bir DNS yapılandırma

Bir Azure SQL veritabanı yönetilen örneği'nı (Önizleme) bir Azure içinde dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bazı senaryolarda, diğer SQL örneklerine Bulut veya karma ortamınızdaki bağlantılı sunucular varsa, yönetilen örneğinden çözümlenmesi için özel ana bilgisayar adlarını gerektiren. Bu durumda, Azure içinde özel DNS yapılandırmanız gerekir. Yönetilen örneği kendi çalışmalar için aynı DNS kullandığından, sanal ağ DNS yapılandırmasını yönetilen örneği ile uyumlu olması gerekir. 

Özel bir DNS yapılandırması yönetilen örneği ile uyumlu hale getirmek için aşağıdaki adımları tamamlamanız gerekir: 
- Özel Azure DNS'ye isteklerini iletmek için DNS yapılandırma 
- Özel DNS olarak birincil ve ikincil olarak Azure DNS VNet için ayarlama 
- Azure DNS ve özel DNS birincil olarak ikincil olarak Kaydet

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Özel Azure DNS'ye isteklerini iletmek için DNS yapılandırma 

Windows Server 2016 DNS iletme yapılandırmak için aşağıdaki adımları kullanın: 

1. İçinde **Sunucu Yöneticisi'ni**, tıklatın **Araçları**ve ardından **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Çift **İleticiler**.

   ![İleticileri](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. **Düzenle**’ye tıklayın. 

   ![İleticiler listesi](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Azure'nın özyinelemeli çözümleyiciler IP adresi, 168.63.129.16 gibi girin.

   ![Özyinelemeli çözümleyiciler IP adresi](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Özel DNS olarak birincil ve ikincil olarak Azure DNS ayarlama 
 
DNS yapılandırması bir Azure sanal ağ üzerinde IP adreslerini girin, böylece aşağıdaki sonraki adımları kullanarak statik bir IP adresi ile DNS sunucusu barındıran Azure VM yapılandırma gerektirir: 

1. Azure Portal'da özel DNS VM ağ arabirimine açın.

   ![Ağ arabirimi](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. IP yapılandırma bölümü. IP yapılandırması 

   ![IP yapılandırması](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Özel IP adresi statik ayarlayın. IP adresi (Bu ekran üzerindeki 10.0.1.5) azaltma 

   ![statik](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Birincil olarak özel DNS ve Azure DNS ikincil olarak Kaydet 

1. Azure Portal'da sanal ağınızı için özel DNS seçeneği bulun.

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Özel anahtar ve Azure'nın özyinelemeli çözümleyiciler gibi IP adresi, 168.63.129.16 yanı sıra, özel DNS sunucusu IP adresi girin. 

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Azure'nın özyinelemeli çözümleyici DNS ayarları listesi hatalı durum girmek yönetilen örneği neden olur. Gelen durum, yeni örnek ilkeleriyle uyumlu ağ bir VNet oluşturmak gerektirebilir kurtarma, örnek düzeyinde veri oluşturun ve veritabanlarınızı geri yükleyin. Bkz: [VNet yapılandırmasını](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bir bakış için bkz: [yönetilen örneği nedir](sql-database-managed-instance.md)
- Yeni bir yönetilen örneğinin nasıl oluşturulacağını gösteren bir öğretici için bkz: [yönetilen örneği oluşturma](sql-database-managed-instance-create-tutorial-portal.md).
- Yönetilen bir örneği için bir sanal ağ yapılandırma hakkında daha fazla bilgi için bkz: [yönetilen örnekleri için sanal ağ yapılandırması](sql-database-managed-instance-vnet-configuration.md)
