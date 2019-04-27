---
title: Azure SQL veritabanı yönetilen örneği'nın yönetim uç noktası keşfedin | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği'nın yönetim uç noktası genel IP adresini alın ve doğrulayın, yerleşik bir güvenlik duvarı koruması hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: b7eb9ecd6b94aad263346ad6b5c45b694e0bd46f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60700030"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Yönetim uç noktanın IP adresine belirleme

Azure SQL veritabanı yönetilen örneği sanal küme yönetimi işlemleri için Microsoft'un kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde ve karşılıklı sertifika doğrulamayı uygulama düzeyinde yerleşik bir güvenlik duvarı ile korunur. Yönetim uç noktası IP adresini belirleyebilirsiniz, ancak bu uç noktaya erişilemiyor.

## <a name="determine-ip-address"></a>IP adresini belirlemek

Yönetilen örnek konak olduğunu varsayalım `mi-demo.xxxxxx.database.windows.net`. Çalıştırma `nslookup` konak adını kullanarak.

![İç konak adı çözme](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Şimdi başka yapmak `nslookup` vurgulanan adı kaldırma `.vnet.` kesimi. Bu komutu yürütürken genel IP adresini elde edersiniz.

![Genel IP adresi çözümleme](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı yönetilen örneği bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).
