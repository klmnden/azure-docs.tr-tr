---
title: Azure portalını kullanarak MySQL sunucusu için Azure veritabanını yeniden başlatın
description: Bu makalede Azure portalını kullanarak MySQL için Azure veritabanı nasıl yeniden açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 2/7/2019
ms.openlocfilehash: cd9275e9e112d24d9dbb3d1d5a60c871d7d8ae5c
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55890473"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Azure portalını kullanarak MySQL sunucusu için Azure veritabanını yeniden başlatın
Bu konuda, MySQL için Azure veritabanı nasıl yeniden açıklanmaktadır. Sunucu işlemi gerçekleştirirken, kısa bir kesintiye neden sunucunuzun bakım nedeniyle yeniden başlatmanız gerekebilir.

Sunucunun yeniden başlatılması, Hizmet meşgul olduğunda engellenir. Örneğin, hizmet sanal çekirdekler ölçeklendirme gibi daha önce istenen bir işlemin işliyor olabilir.

Yeniden başlatma tamamlamak için gereken süreyi MySQL kurtarma işlemi bağlıdır. Yeniden başlatma süresini azaltmak için yeniden başlatma öncesinde sunucusunda gerçekleşen etkinliği miktarını en aza öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [MySQL sunucusu ve veritabanı için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucunun yeniden başlatılmasını gerçekleştirme

Aşağıdaki adımları MySQL sunucusunu yeniden başlatın:

1. Azure portalında MySQL için Azure veritabanı sunucunuza seçin.

2. Sunucunun araç **genel bakış** sayfasında **yeniden**.

   ![MySQL - genel bakış - yeniden Başlat düğmesi için Azure veritabanı](./media/howto-restart-server-portal/2-server.png)

3. Tıklayın **Evet** sunucuyu yeniden başlatmadan onaylamak için.

   ![-MySQL için Azure veritabanı, yeniden başlatma işlemini onaylayın ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumu "Yeniden için" değiştiğini gözlemleyin.

   ![MySQL - yeniden başlatma durumu için Azure veritabanı ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucunun yeniden başlatılması başarılı olduğunu onaylayın.

   ![-Yeniden başlatma başarılı olan MySQL için Azure veritabanı ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Azure portalını kullanarak MySQL sunucusu için Azure veritabanı oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)