---
title: Azure App Service - eşitleme ağı yapılandırması | Microsoft Docs
description: Bu makalede, Azure App Service barındırma planı için ağ yapılandırmanızı eşitleme anlatılmaktadır.
ms.service: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 57c4dd523a5dffc48a2d2d403d2a440a8d6cde95
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257911"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Azure App Service barındırma planı için ağ yapılandırmayı eşitleyemedi

Bu, olsa da gerçekleşebilir, [uygulamanızı bir Azure sanal ağı ile tümleşik](../app-service/web-sites-integrate-with-vnet.md), yönetilen örnek bağlantı kuramıyor. Ağ yapılandırması, hizmet planınız için bir şey deneyebilirsiniz yenilemektir. 

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>App Service barındırma planı için eşitleme ağı yapılandırması

Bunu yapmak için şu adımları uygulayın:  

1. Web apps için App Service planı gidin.
 
   ![App Service planı](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Tıklayın **ağ** ve ardından **burada Yönet**.
 
   ![Hizmet planını yönetme](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Seçin, **VNet** tıklatıp **eşitleme ağ**. 
 
   ![ağı Eşitle](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Eşitleme işlemi tamamlanana kadar bekleyin.
  
   ![Eşitleme bitti](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Şimdi, yönetilen Örneğinize yeniden bağlantı kurmayı denemeye hazır olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnek için sanal ağ yapılandırma hakkında daha fazla bilgi için bkz: [yönetilen örnek sanal ağ yapılandırması](sql-database-managed-instance-vnet-configuration.md).
