---
title: Azure App Service - eşitleme ağı yapılandırması | Microsoft Docs
description: Bu makalede, Azure App Service barındırma planı için ağ yapılandırmanızı eşitleme anlatılmaktadır.
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
ms.openlocfilehash: 0d7920080fd61389741fbe785f5141003bef5251
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61314729"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Azure App Service barındırma planı için ağ yapılandırmayı eşitleyemedi

Bu, olsa da gerçekleşebilir, [uygulamanızı bir Azure sanal ağı ile tümleşik](../app-service/web-sites-integrate-with-vnet.md), yönetilen örnek bağlantı kuramıyor. Ağ yapılandırması, hizmet planınız için bir şey deneyebilirsiniz yenilemektir.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>App Service barındırma planı için eşitleme ağı yapılandırması

Bunu yapmak için şu adımları uygulayın:  

1. Web apps için App Service planı gidin.

   ![App service planı](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Tıklayın **ağ** ve ardından **burada Yönet**.

   ![Hizmet planını yönetme](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Seçin, **VNet** tıklatıp **eşitleme ağ**.

   ![ağı Eşitle](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Eşitleme işlemi tamamlanana kadar bekleyin.
  
   ![Eşitleme bitti](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Şimdi, yönetilen Örneğinize yeniden bağlantı kurmayı denemeye hazır olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnek için sanal ağ yapılandırma hakkında daha fazla bilgi için bkz: [mimarisi yönetilen örnek VNet](sql-database-managed-instance-connectivity-architecture.md) ve [mevcut bir VNet yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md).
