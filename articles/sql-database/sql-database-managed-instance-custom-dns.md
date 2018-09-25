---
title: Azure SQL veritabanı yönetilen örneği özel DNS | Microsoft Docs
description: Bu konuda, bir Azure SQL veritabanı yönetilen örneği ile özel bir DNS yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 09/23/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 2d1bb7e8522da32dd33933261ea41b578f8afac1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949494"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Özel DNS yapılandırma için Azure SQL veritabanı yönetilen örneği

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Yönetilen örnek çözülmesi özel konak adlarını gerektiren bazı senaryolar (yani, Bulut veya karma ortamınızdaki diğer SQL örneklerine bağlı sunucular) vardır. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. Yönetilen örneği için kendi iç işleyişini aynı DNS kullandığından, sanal ağ DNS yapılandırmasını yönetilen örneği ile uyumlu olması gerekir. 

Özel bir DNS yapılandırmasını yapmak için yönetilen örneği ile uyumludur, şunları yapmanız gerekir: 
- Genel etki alanı adlarını çözümlemek için bu nedenle özel DNS sunucusunu yapılandırma 
- Sanal ağ DNS listesinin sonuna 168.63.129.16 Azure özyinelemeli çözümleyici DNS IP adresi yerleştirin 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Özel DNS sunucuları yapılandırma ayarlama

1. Azure portalında sanal ağınızdaki özel DNS seçeneği bulun.

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Özel anahtar ve özel DNS sunucusu IP adresinizi ve bunun yanı sıra Azure'nın yinelemeli Çözümleyicileri IP adresi 168.63.129.16 girin. 

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Azure'nın yinelemeli çözümleyici DNS ayarları listesi hatalı durumuna girmeyi yönetilen örneğe neden olur. Gelen durum yeni bir örneğini bir sanal ağ ile uyumlu ağ ilkeleri oluşturmanıza olanak gerektirebilir kurtarma, örnek düzeyinde veri oluşturun ve veritabanlarınızı geri yüklemek. Bkz: [VNet yapılandırmasını](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren bir öğretici için bkz [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- Bir yönetilen örnek için bir VNet yapılandırma hakkında daha fazla bilgi için bkz: [yönetilen örnekleri için sanal ağ yapılandırması](sql-database-managed-instance-vnet-configuration.md)
