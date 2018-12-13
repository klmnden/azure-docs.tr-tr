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
ms.openlocfilehash: 45ddf1c75dd22f5074c2017185bc0ed3be0b2a80
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872705"
---
# <a name="discover-azure-sql-database-managed-instance-management-endpoint"></a>Azure SQL veritabanı yönetilen örneği'nın yönetim uç noktasını Bul

Azure SQL veritabanı yönetilen örneği [sanal küme](sql-database-managed-instance-connectivity-architecture.md) Microsoft yönetilen örneğe yönetmek için kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde ve karşılıklı sertifika doğrulamayı uygulama düzeyinde yerleşik güvenlik duvarı ile korunur.

Bağlantılar yönetilen örneğe içinde (Yedekleme, Denetim günlüğü) başlatılır, trafiği yönetim uç noktası genel IP adresinden kaynaklanan görünür. Güvenlik duvarı kuralları yalnızca yönetilen örnek IP adreslerine izin verecek şekilde ayarlayarak yönetilen örneğinden kamu hizmetleri için erişimi sınırlayabilirsiniz.

> [!NOTE]
> Bu, Azure platformu birlikte bulunan hizmetler arasında trafiğinin yönelik bir iyileştirme olarak, yönetilen örneği ile aynı bölgede bulunan Azure Hizmetleri için güvenlik duvarı kurallarını ayarlamak geçerli değildir.

Bu makalede, yerleşik bir güvenlik duvarı koruması doğrulamak için yönetim uç noktası genel IP adresinin yanı sıra nasıl alabilir açıklanmaktadır.

## <a name="finding-the-management-endpoint-public-ip-address"></a>Yönetim uç noktası genel IP adresi bulma

Yönetilen örnek konak olduğunu varsayalım `mi-demo.xxxxxx.database.windows.net`. Çalıştırma `nslookup` konak adını kullanarak.

![İç konak adı çözme](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Şimdi başka yapmak `nslookup` vurgulanan adı kaldırma `.vnet.` kesimi. Bu komut yürütme sonucu olarak genel IP adresi elde edersiniz.

![Genel IP adresi çözümleme](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="verifying-the-managed-instance-built-in-firewall"></a>Yönetilen örnek yerleşik güvenlik duvarı doğrulanıyor

Yönetilen örneğe [zorunlu gelen güvenlik kuralları](sql-database-managed-instance-vnet-configuration.md#mandatory-inbound-security-rules) 9003, yönetim bağlantı noktalarına 9000, gerekli 1438, 1440, buradan Aç olmasını 1452 **herhangi bir kaynağı** üzerinde ağ güvenlik grubu (yönetilen koruyan NSG) Örneği. Bu bağlantı noktaları NSG düzeyinde açık olsa da, bunlar ağ düzeyinde yerleşik güvenlik duvarı tarafından korunur.

Bu bağlantı noktaları doğrulamak için bu bağlantı noktaları test etmek için herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde Bu araçlardan birini kullanmayı gösterir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı yönetilen örneği bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).