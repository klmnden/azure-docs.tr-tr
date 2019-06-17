---
title: Azure veritabanı, Azure portalını kullanarak MariaDB sunucuyu yeniden başlatın
description: Bu makalede, Azure portalı kullanarak MariaDB için Azure veritabanı nasıl yeniden açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 2/7/2019
ms.openlocfilehash: 232037562c4a84ee9217e2e89a0da2ffdc37d560
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60745800"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Azure veritabanı, Azure portalını kullanarak MariaDB sunucuyu yeniden başlatın
Bu konuda, MariaDB server için Azure veritabanı nasıl yeniden açıklanmaktadır. Sunucu işlemi gerçekleştirirken, kısa bir kesintiye neden sunucunuzun bakım nedeniyle yeniden başlatmanız gerekebilir.

Sunucunun yeniden başlatılması, Hizmet meşgul olduğunda engellenir. Örneğin, hizmet sanal çekirdekler ölçeklendirme gibi daha önce istenen bir işlemin işliyor olabilir.

Yeniden başlatma tamamlamak için gereken süreyi MariaDB kurtarma işlemi bağlıdır. Yeniden başlatma süresini azaltmak için yeniden başlatma öncesinde sunucusunda gerçekleşen etkinliği miktarını en aza öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [MariaDB için Azure veritabanı](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucunun yeniden başlatılmasını gerçekleştirme

Aşağıdaki adımlar, MariaDB sunucuyu yeniden başlatın:

1. Azure portalında MariaDB için Azure veritabanı sunucunuza seçin.

2. Sunucunun araç **genel bakış** sayfasında **yeniden**.

   ![MariaDB - genel bakış - yeniden Başlat düğmesi için Azure veritabanı](./media/howto-restart-server-portal/2-server.png)

3. Tıklayın **Evet** sunucuyu yeniden başlatmadan onaylamak için.

   ![-MariaDB için Azure veritabanı, yeniden başlatma işlemini onaylayın](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumu "Yeniden için" değiştiğini gözlemleyin.

   ![MariaDB - yeniden başlatma durumu için Azure veritabanı](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucunun yeniden başlatılması başarılı olduğunu onaylayın.

   ![MariaDB - yeniden başlatma başarılı için Azure veritabanı](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Azure portalını kullanarak MariaDB sunucusu için Azure veritabanı oluşturma](./quickstart-create-mariadb-server-database-using-azure-portal.md)