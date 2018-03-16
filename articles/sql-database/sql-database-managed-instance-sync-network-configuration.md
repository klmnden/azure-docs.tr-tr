---
title: "Azure uygulama hizmeti - eşitleme ağ yapılandırması | Microsoft Docs"
description: "Bu makalede, Azure App Service barındırma planı için ağ yapılandırmanızı eşitlemenin nasıl anlatılmaktadır."
ms.service: sql-database
author: srdjan-bozovic
manager: craigg
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 5b139b1279776acfca63def25a9fdae0f627a727
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Ağ yapılandırması Azure App Service barındırma planı için eşitleme

Bu, olsa da gerçekleşebilir, [uygulamanız bir Azure sanal ağı ile tümleşik](../app-service/web-sites-integrate-with-vnet.md), yönetilen örneğine bağlantı kuramıyor. Deneyebileceğiniz bir hizmeti planınız için ağ yapılandırmasını yenilemek için şeydir. 

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>App Service barındırma planı için eşitleme ağ yapılandırması

Bunu yapmak için şu adımları uygulayın:  

1. Web uygulamalarınızı App Service planı gidin.
 
   ![App Service planı](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Tıklatın **ağ** ve ardından **Yönet burayı**.
 
   ![hizmet planı yönetme](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Seçin, **VNet** tıklatıp **eşitleme ağ**. 
 
   ![Eşitleme ağ](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Eşitleme tamamlanıncaya kadar bekleyin.
  
   ![Eşitleme tamamlandı](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Şimdi, yönetilen Örneğiniz için yeniden bağlantı kurmayı denemeye hazır olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

- Sanal ağınızı yönetilen örneği için yapılandırma hakkında daha fazla bilgi için bkz: [yönetilen örneği VNet yapılandırmasını](sql-database-managed-instance-vnet-configuration.md).
