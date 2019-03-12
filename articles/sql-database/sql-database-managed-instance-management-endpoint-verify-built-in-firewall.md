---
title: Azure SQL veritabanı yönetilen örneği'nın yerleşik güvenlik duvarı keşfedin | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği'nde yerleşik güvenlik duvarı koruması doğrulayın öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: f059ac4149a385a12b440db0ad6394a343a1f810
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57761931"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Yönetilen örnek yerleşik güvenlik duvarı doğrulanıyor

Yönetilen örneğe [zorunlu gelen güvenlik kuralları](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) 9003, yönetim bağlantı noktalarına 9000, gerekli 1438, 1440, buradan Aç olmasını 1452 **herhangi bir kaynağı** üzerinde ağ güvenlik grubu (yönetilen koruyan NSG) Örneği. Bu bağlantı noktaları NSG düzeyinde açık olsa da, bunlar ağ düzeyinde yerleşik güvenlik duvarı tarafından korunur.

## <a name="verify-firewall"></a>Güvenlik Duvarı doğrulayın

Bu bağlantı noktaları doğrulamak için bu bağlantı noktaları test etmek için herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde Bu araçlardan birini kullanmayı gösterir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı yönetilen örneği bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).