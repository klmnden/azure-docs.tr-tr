---
title: Azure SQL veritabanı yönetilen örneği'nın yönetim uç noktası keşfedin | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği'nın yönetim uç noktası genel IP adresini alın ve doğrulayın, yerleşik bir güvenlik duvarı koruması hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 99ec559429d66becc20e038e43349f5369afac39
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856756"
---
# <a name="discover-azure-sql-database-managed-instance-management-endpoint"></a>Azure SQL veritabanı yönetilen örneği'nın yönetim uç noktasını Bul 

## <a name="overview"></a>Genel Bakış
Azure SQL yönetilen örneği [sanal küme](sql-database-managed-instance-connectivity-architecture.md) örneğini yönetmek için Microsoft'un kullandığı yönetim uç noktası içerir.  

Yönetim uç noktası, ağ düzeyinde ve karşılıklı sertifika doğrulamayı uygulama düzeyinde yerleşik güvenlik duvarı ile korunur. 

Ne zaman bağlantıları başlatılacağını gelen içinde yönetilen örneği (CLR, bağlantılı sunucu, yedekleme, Denetim günlüğü vb.) trafiği yönetim uç noktası genel IP adresinden kaynaklanan görünür. Güvenlik duvarı kuralları yalnızca bu IP adreslerine izin verecek şekilde ayarlayarak yönetilen örneğinden kamu hizmetleri için erişimi sınırlayabilirsiniz.

> [!NOTE]
> Bu, birlikte bulunan hizmetleri arasında giden trafik için iyileştirme platformu olduğu gibi yönetilen örneği ile aynı bölgede bulunan Azure Hizmetleri için güvenlik duvarı kurallarını ayarlamak geçerli değildir. 

Bu makalede, yerleşik bir güvenlik duvarı koruması doğrulamak için yönetim uç noktası genel IP adresinin yanı sıra nasıl alabilir açıklanmaktadır.

## <a name="finding-management-endpoint-public-ip-address"></a>Yönetim uç noktası genel IP adresi bulma
Konak mı olduğunu varsayalım _mı demo.xxxxxx.database.windows.net_. Çalıştırma _nslookup_ konak adını kullanarak.

![İç konak adı çözme](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Şimdi başka yapmak _nslookup_ removed_.vnet._segment ile vurgulanan adı. Bu komut yürütme sonucu olarak genel IP adresi elde edersiniz.

![Genel IP adresi çözümleme](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="verifying-managed-instance-built-in-firewall"></a>Yönetilen örnek yerleşik güvenlik duvarı doğrulanıyor
Yönetilen örnek [zorunlu gelen güvenlik kuralları](sql-database-managed-instance-vnet-configuration.md#mandatory-inbound-security-rules) 9003, yönetim bağlantı noktalarına 9000, gerekli 1438, 1440, yönetilen örneğe koruyan bir ağ güvenlik grubu üzerinde herhangi bir kaynaktan olmasını 1452 açın. Bu bağlantı noktaları NSG düzeyinde açık olmasına rağmen ağ düzeyinde yerleşik güvenlik duvarı tarafından korunur.

Bu bağlantı noktaları doğrulamak için bu bağlantı noktaları test etmek için herhangi bir güvenlik tarayıcısı aracını kullanabilirsiniz. Aşağıdaki ekran görüntüsünde Bu araçlardan birini kullanmayı gösterir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)
