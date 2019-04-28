---
title: Bir Azure SQL veri ambarı - REST API geri yükleme | Microsoft Docs
description: REST API'leri kullanarak bir Azure SQL veri ambarı geri yükleyin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: daf013472e5fa533912920e4c14a552905b5d333
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60935696"
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>REST API ile bir Azure SQL veri ambarını geri yükleme
REST API'leri kullanarak bir Azure SQL veri ambarı geri yükleyin.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** Varsayılan olan bir mantıksal SQL sunucusu tarafından (örn. myserver.database.windows.net) barındırılan her SQL veri ambarı [DTU kotası](../sql-database/sql-database-what-is-a-dtu.md).  SQL veri ambarı geri yüklemeden önce olduğunu doğrulayın. SQL server'ınızı geri yüklenen veritabanı için yeterli kalan DTU kotası vardır. Daha fazla DTU istemek için [bir destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Etkin ya da duraklatılmış veri ambarını geri yükleme
Bir veri ambarını geri yüklemek için:

1. Veritabanı geri yükleme noktalarını al işlemi kullanarak veritabanını geri yükleme noktaları listesini alın.
2. Geri yükleme kullanarak başlarsanız [Oluştur veritabanı geri yükleme isteği](https://msdn.microsoft.com/library/azure/dn509571.aspx) işlemi.
3. Kullanarak geri yükleme işleminizin durumunu izlemek [veritabanı işlem durumunu](https://msdn.microsoft.com/library/azure/dn720371.aspx) işlemi.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra kurtarılan veri Ambarınızı izleyerek yapılandırabileceğiniz [kurtarma işleminden sonra veritabanını yapılandırma](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Silinen bir veri ambarını geri yükleme
Silinen bir veri ambarını geri yüklemek için:

1. Tüm geri yüklenebilen silinmiş veri ambarınız kullanarak liste [veritabanları listesi geri yüklenebilen bırakılan](https://msdn.microsoft.com/library/azure/dn509562.aspx) işlemi.
2. [Get geri yüklenebilen bırakılan veritabanı] [Get geri yüklenebilen bırakılan veritabanı] işlemi kullanılarak geri yüklemek istediğiniz silinen veri ambarı için ayrıntıları öğrenin.
3. Geri yükleme kullanarak başlarsanız [Oluştur veritabanı geri yükleme isteği](https://msdn.microsoft.com/library/azure/dn509571.aspx) işlemi.
4. Kullanarak geri yükleme işleminizin durumunu izlemek [veritabanı işlem durumunu](https://msdn.microsoft.com/library/azure/dn720371.aspx) işlemi.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veri Ambarınızı yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı sürümlerini iş sürekliliği özellikleri hakkında bilgi edinmek için lütfen okuyun [Azure SQL Database iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md).
