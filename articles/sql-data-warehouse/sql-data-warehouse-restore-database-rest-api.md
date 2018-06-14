---
title: Bir Azure SQL veri ambarı - REST API geri | Microsoft Docs
description: REST API'lerini kullanarak Azure SQL Data Warehouse geri yükleyin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 2e1874fdf7c11d98d369072739c5937caffe6e96
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31524433"
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>Bir Azure SQL veri ambarı REST API'leri ile geri yükleme
REST API'lerini kullanarak Azure SQL Data Warehouse geri yükleyin.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** Her SQL veri ambarı varsayılan olan bir mantıksal SQL sunucusu tarafından (örneğin myserver.database.windows.net) barındırılan [DTU kota](../sql-database/sql-database-what-is-a-dtu.md).  SQL Data Warehouse geri yükleyebilmeniz için önce doğrulayın yeterli kalan DTU kota geri yüklenen veritabanı için SQL server'ınızdaki sahiptir. Daha fazla DTU istemek için [destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Etkin ya da duraklatılmış veri ambarı geri yükle
Veri ambarı geri yüklemek için:

1. Get veritabanı geri yükleme noktaları işlemi kullanarak veritabanını geri yükleme noktaları listesini alın.
2. Geri yükleme kullanarak başlayın [oluşturma veritabanı geri yükleme isteği](https://msdn.microsoft.com/library/azure/dn509571.aspx) işlemi.
3. Kullanarak geri yükleme durumunu izlemek [veritabanı işlemi durumunu](https://msdn.microsoft.com/library/azure/dn720371.aspx) işlemi.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra izleyerek kurtarılan veri ambarınız yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Silinen veri ambarı geri yükle
Silinen veri ambarını geri yüklemek için:

1. Tüm geri yüklenebilen silinmiş veri ambarınız kullanarak liste [listesi geri yüklenebilen bırakılan veritabanları](https://msdn.microsoft.com/library/azure/dn509562.aspx) işlemi.
2. [Get geri yüklenebilen bırakılan veritabanı] [Get geri yüklenebilen bırakılan veritabanı] işlemi kullanarak geri yüklemek istediğiniz silinen veri ambarı için ayrıntıları öğrenin.
3. Geri yükleme kullanarak başlayın [oluşturma veritabanı geri yükleme isteği](https://msdn.microsoft.com/library/azure/dn509571.aspx) işlemi.
4. Kullanarak geri yükleme durumunu izlemek [veritabanı işlemi durumunu](https://msdn.microsoft.com/library/azure/dn720371.aspx) işlemi.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veri ambarı yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı sürümlerini iş sürekliliği özellikleri hakkında bilgi edinmek için lütfen okuyun [Azure SQL Database iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md).
