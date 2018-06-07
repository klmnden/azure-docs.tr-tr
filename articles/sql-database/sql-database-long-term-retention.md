---
title: 10 yılı aşkın Azure SQL veritabanı yedeklemelerini depolamak | Microsoft Docs
description: Azure SQL veritabanı depolanmasını tam veritabanı yedeklemeleri 10 yılı aşkın nasıl desteklediğini öğrenin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: b2f3c454ba84c7b892096cc42dcbe2706ab6159f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648301"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL veritabanı yedeklemelerini depolamak için en fazla 10 yıl

Birçok uygulama yasal varsa, uyumluluk ya da diğer iş amaçlı veritabanı yedeklemeleri Azure SQL veritabanı tarafından sağlanan 7-35 gün dışında tutulacak gerektiren [otomatik yedeklemeler](sql-database-automated-backups.md). Uzun vadeli bekletme (LTR) özelliğini kullanarak, belirtilen SQL veritabanı tam yedeklemelerin depolayabilir [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blob depolamada en fazla 10 yıldır. Daha sonra yeni bir veritabanı olarak herhangi bir yedekten geri yükleyebilirsiniz.

> [!IMPORTANT]
> Uzun vadeli bekletme şu anda önizlemede değil. Bu özellik önceki önizlemesini bir parçası olarak Azure kurtarma Hizmetleri kasasında depolanan var olan yedekleri SQL Azure depolama birimine geçirilir.<!-- and available in the following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.-->
>

## <a name="how-sql-database-long-term-retention-works"></a>SQL veritabanı uzun vadeli bekletme nasıl çalışır?

Uzun vadeli yedekleme bekletme yararlanır [otomatik SQL veritabanı yedeklemeleri](sql-database-automated-backups.md) oluşturulan zaman nokta geri yükleme (PITR) alır. Her bir SQL veritabanı için uzun vadeli bekletme ilkesi yapılandırın ve yedeklemeleri uzun süreli depolama alanına kopyalamanız gerekecektir ne sıklıkta belirtin. Bu esneklik sağlamak için dört parametre birleşimini kullanarak İlkesi tanımlayabilir: Haftalık yedekleme bekletme (W) aylık yedekleme bekletme (M), yıllık yedekleme bekletme (Y) ve (WeekOfYear) yılın haftası. Belirtirseniz W, bir yedekleme haftada uzun süreli depolama alanına kopyalanır. M belirtirseniz, her ayın ilk haftası bir yedeklemede uzun süreli depolama alanına kopyalanır. Y belirtirseniz, uzun vadeli depolama için bir yedekleme WeekOfYear tarafından belirtilen bir hafta sırasında kopyalanacak. Her yedekleme uzun vadeli depolama alanında bu parametreler tarafından belirtilen süre boyunca tutulur. 

Örnekler:

-  W = 0, M = 0, Y = 5, WeekOfYear = 3

   5 yıl boyunca her yıl 3 tam yedeklemesini tutulacak.

- W = 0, M = 3, Y = 0

   Her ayın ilk tam yedeklemede 3 aydan tutulacak.

- W=12, M=0, Y=0

   Her haftalık tam yedekleme için 12 haftaya tutulacak.

- W = 6, M = 12, Y = 10, WeekOfYear = 16

   Her haftalık tam yedekleme 6 hafta boyunca tutulur. Dışında ilk tam yedeklemede her ayın hangi 12 ay tutulacak. 16 yılın haftası üzerinde gerçekleştirilecek tam yedekleme dışında 10 yılı aşkın tutulacak. 

Aşağıdaki tabloda uzun vadeli yedeklemeleri aşağıdaki ilkesi için sona erme tarihi ve tempoyla gösterilmektedir:

W = 12 Hafta (84 gün), M = 12 ay (365 gün), Y = 10 yıl (3650 gün), WeekOfYear = 15 (hafta sonra Nisan 15)

   ![LTR örneği](./media/sql-database-long-term-retention/ltr-example.png)


 
Yukarıdaki tabloda yukarıdaki ilkesini değiştirmek için olan ve yedek kopyaları kümesini W = 0 (hiçbir haftalık yedekleri), tempoyla olarak değiştirecek vurgulanan tarihlerle gösterilir. Bu yedeklemeler tutmak için gerekli depolama miktarını uygun şekilde azaltın. 

> [!NOTE]
1. Kopyalama işlemi, mevcut bir veritabanının üzerine herhangi bir performans etkisi yoktur şekilde LTR kopyaları Azure depolama hizmeti tarafından oluşturulur.
2. İlke, gelecekteki yedeklemeler için geçerlidir. Örneğin Belirtilen WeekOfYear ilke zaman yapılandırılan geçmişte ise, bir sonraki yılın ilk LTR yedekleme oluşturulacak. 
3. LTR depolama biriminden bir veritabanını geri yüklemek için zaman damgasını temel alarak belirli bir yedeği seçebilirsiniz.   Veritabanını özgün veritabanını aynı abonelik altında var olan herhangi bir sunucuya geri yüklenebilir. 
> 

## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure portal veya PowerShell kullanarak uzun vadeli bekletme yapılandırma konusunda bilgi edinmek için [uzun vadeli yedekleme bekletme yapılandırma](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı Yedeklemeleri yanlışlıkla Bozulması veya silinmesi verileri korumak için herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejiniz temel parçası oldukları. Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
