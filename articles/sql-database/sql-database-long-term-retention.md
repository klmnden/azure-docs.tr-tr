---
title: Azure SQL veritabanı yedeklemeleri için 10 yıla kadar Store | Microsoft Docs
description: Azure SQL veritabanı depolama tam veritabanı yedeklemeleri için 10 yıla kadar nasıl desteklediğini öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 85757ace20501bea1db22ecfdd2fdb63284038d5
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58108755"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL veritabanı yedeklemeleri için 10 yıla kadar Store

Birçok uygulama yasal yoksa, uyumluluk veya diğer iş amaçlı veritabanı yedeklemelerini Azure SQL veritabanı tarafından sağlanan 7-35 gün dışında tutulacak gerektiren [otomatik yedeklemeler](sql-database-automated-backups.md). Uzun süreli saklama (LTR) özelliğini kullanarak, belirtilen SQL veritabanı tam yedeklemelerde depolayabilirsiniz [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blob depolama için 10 yıla kadar. Daha sonra herhangi bir yedeklemeyi yeni bir veritabanı olarak geri yükleyebilirsiniz.

> [!NOTE]
> LTR tek ve havuza alınmış veritabanları için etkinleştirilebilir. Mevcut örneği için yönetilen örnekleri'nde veritabanlarını değil henüz bu değil. SQL Aracısı işleri zamanlamak için kullanabileceğiniz [yalnızca kopya yedekleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) 35 gün ötesinde LTR alternatif olarak.
> 

## <a name="how-sql-database-long-term-retention-works"></a>SQL veritabanı uzun süreli saklama nasıl çalışır?

Uzun süreli yedek saklama (LTR) yararlanır, tam veritabanı yedeklemeleri [otomatik olarak oluşturulan](sql-database-automated-backups.md) belirli bir noktaya geri yükleme (PITR) etkinleştirmek için. LTR İlkesi yapılandırdıysanız, bu yedeklemeler için farklı depolama BLOB'ları kopyalanır.
LTR İlkesi her SQL veritabanı için yapılandırmak ve ne sıklıkla yedeklemeleri uzun vadeli depolama BLOB'ları için kopyalamanız gerekir belirtin. Bu esneklik etkinleştirmek için dört parametre birleşimi kullanarak ilkesi tanımlayabilirsiniz: Haftalık yedekleme bekletme (W), aylık yedekleme bekletme (M), yıllık yedekleme bekletme (Y) ve (WeekOfYear) yılın haftası. Belirtirseniz W, haftada bir yedekleme uzun süreli depolama alanına kopyalanır. M belirtirseniz, her ayın ilk haftasında sırasında bir yedek uzun süreli depolama alanına kopyalanacak. Y belirtirseniz, bir yedekleme WeekOfYear tarafından belirtilen bir hafta sırasında uzun süreli depolama alanına kopyalanacak. Her yedekleme Bu parametreler tarafından belirtilen süre boyunca uzun vadeli depolamada tutulacak. 

Örnekler:

-  W = 0, M = 0 ise, Y = 5, WeekOfYear = 3

   5 yıl boyunca her yıl 3 tam yedeğini tutulacak.
- W = 0, M = 3, Y = 0

   Her ayın ilk tam yedekleme için 3 ay tutulacak.

- W=12, M=0, Y=0

   Her haftalık tam yedekleme için 12 haftaya tutulacak.

- W = 6, M = 12, Y = 10 WeekOfYear = 16

   Her haftalık tam yedekleme için 6 hafta tutulacak. 12 ay boyunca ilk tam yedeklemede her ayın hangi tutulacak. Geçen yılın 16 haftası tam yedekleme dışında olduğu için 10 yıla tutulacak. 

Aşağıdaki tabloda, temposu ve şu ilkeye için uzun vadeli yedekleme sona erme gösterilmektedir:

W = 12 Hafta (84 gün), M = 12 ay (365 gün), Y = 10 yıl (3650 gün) WeekOfYear = 15 (hafta sonra 15 Nisan)

   ![LTR örneği](./media/sql-database-long-term-retention/ltr-example.png)



Yukarıdaki ilkesini değiştirmek için açtığınız ve yedek kopyaların W = 0 (hiçbir haftalık yedeklemeler), temposu kümesi olarak değiştirir, yukarıdaki tabloda vurgulanmış tarihlere göre gösterilir. Buna göre bu yedeklemeler tutmak için gerekli depolama miktarını azaltır. 

> [!NOTE]
> 1. Kopyalama işlemi, mevcut bir veritabanının üzerine herhangi bir performans etkisi yoktur dolayısıyla LTR kopyalarını Azure depolama hizmeti tarafından oluşturulur.
> 2. İlke, gelecekteki yedeklemeler için geçerlidir. Örneğin Belirtilen WeekOfYear zaman İlkesi yapılandırılır geçmişte ise, sonraki yılın ilk LTR yedekleme oluşturulur. 
> 3. LTR depolamadan bir veritabanını geri yüklemek için zaman damgası üzerinde dayalı belirli bir yedekleme seçebilirsiniz.   Veritabanını özgün veritabanıyla aynı abonelik altında var olan herhangi bir sunucuya geri yüklenebilir. 

## <a name="geo-replication-and-long-term-backup-retention"></a>Coğrafi çoğaltma ve uzun süreli yedek saklama

Son yük devretme işlemleri için hazırlamak ve coğrafi-ikincil veritabanı'nda aynı LTR ilkesi yapılandırma, iş sürekliliği çözümünüz olarak etkin coğrafi çoğaltma veya yük devretme grubu kullanıyorsanız. Yedeklemeler, ikincil veritabanı ' oluşturulmaz gibi LTR depolama maliyetlerinizi artırmaz. İkincil birincil olduğunda yalnızca yedekleme oluşturulur. Böylece yük devretme tetiklendiğinde LTR yedekleme kesintiye nesil garanti eder ve birincil, ikincil bölgeye taşır. 

> [!NOTE]
> Özgün birincil veritabanını yük devretme için neden olabilir kesinti kurtarıldığında, yeni ikincil olur. Bu nedenle, yedekleme oluşturma değil sürdürür ve birincil yeniden olana kadar mevcut LTR ilke etkili olmaz. 

## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure portalı veya PowerShell kullanarak uzun süreli saklama yapılandırma konusunda bilgi için bkz: [uzun süreli yedek saklama yapılandırma](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı yedeklerinin yanlışlıkla ya da silme verileri korumak için önemli bir bölümü herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejiniz oldukları. Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
