---
title: Azure SQL veritabanı yönetilen örneği özel DNS | Microsoft Docs
description: Bu konuda, bir Azure SQL veritabanı yönetilen örneği ile özel bir DNS yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 12/13/2018
ms.openlocfilehash: bb5890b883b6062d834b928bff28a26a3664fb64
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60700438"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Özel DNS yapılandırma için Azure SQL veritabanı yönetilen örneği

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Yönetilen örnek çözülmesi özel konak adlarını gerektiren birkaç senaryo vardır (örneğin, db posta, Bulut veya karma ortamınızdaki diğer SQL örneklerine bağlı sunucular). Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. Yönetilen örneği için kendi iç işleyişini aynı DNS kullandığından, sanal ağ DNS yapılandırmasını yönetilen örneği ile uyumlu olması gerekir.

   > [!IMPORTANT]
   > Özel DNS bölgenizdeki olsalar bile her zaman posta sunucuları, SQL sunucuları ve diğer hizmetler için tam etki alanı adlarını (FQDN) kullanın. Örneğin `smtp.contoso.com` posta sunucusu için olduğundan basit `smtp` düzgün şekilde çözümlenen olmayacaktır.

Özel bir DNS yapılandırmasını yapmak için yönetilen örneği ile uyumludur, şunları yapmanız gerekir:

- Genel etki alanı adlarını çözümlemek için bu nedenle özel DNS sunucusunu yapılandırma
- Sanal ağ DNS listesinin sonuna 168.63.129.16 Azure özyinelemeli çözümleyici DNS IP adresi yerleştirin

## <a name="setting-up-custom-dns-servers-configuration"></a>Özel DNS sunucuları yapılandırma ayarlama

1. Azure portalında sanal ağınızdaki özel DNS seçeneği bulun.

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png)

2. Özel anahtar ve özel DNS sunucusu IP adresinizi ve bunun yanı sıra Azure'nın yinelemeli Çözümleyicileri IP adresi 168.63.129.16 girin.

   ![Özel dns seçeneği](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png)

   > [!IMPORTANT]
   > Azure'nın yinelemeli çözümleyici DNS listesinde ayarlanmaması, herhangi bir nedenden dolayı özel DNS sunucuları kullanılamaz bir hatalı durumuna geçilmesidir yönetilen örneğe neden olabilir. Gelen durum yeni bir örneğini bir sanal ağ ile uyumlu ağ ilkeleri oluşturmanıza olanak gerektirebilir kurtarma, örnek düzeyinde veri oluşturun ve veritabanlarınızı geri yüklemek. Tüm özel DNS sunucuları bile başarısız olursa, DNS listedeki son girişi sağlar olarak Azure'nın yinelemeli çözümleyici ayarlama, ortak adları hala çözülebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren bir öğretici için bkz [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- Bir yönetilen örnek için bir VNet yapılandırma hakkında daha fazla bilgi için bkz: [yönetilen örnekleri için sanal ağ yapılandırması](sql-database-managed-instance-connectivity-architecture.md)
