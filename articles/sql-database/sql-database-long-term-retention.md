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
ms.date: 05/18/2019
ms.openlocfilehash: 6549892bfd04065bf83ab50fa5f5b439c35c4238
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190539"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL veritabanı yedeklemeleri için 10 yıla kadar Store

Birçok uygulama yasal yoksa, uyumluluk veya diğer iş amaçlı veritabanı yedeklemelerini Azure SQL veritabanı tarafından sağlanan 7-35 gün dışında tutulacak gerektiren [otomatik yedeklemeler](sql-database-automated-backups.md). Uzun süreli saklama (LTR) özelliğini kullanarak, belirtilen SQL veritabanı tam yedeklemelerde depolayabilirsiniz [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blob depolama için 10 yıla kadar. Daha sonra herhangi bir yedeklemeyi yeni bir veritabanı olarak geri yükleyebilirsiniz.

> [!NOTE]
> LTR tek ve havuza alınmış veritabanları için etkinleştirilebilir. Mevcut örneği için yönetilen örnekleri'nde veritabanlarını değil henüz bu değil. SQL Aracısı işleri zamanlamak için kullanabileceğiniz [yalnızca kopya yedekleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) 35 gün ötesinde LTR alternatif olarak.
> 

## <a name="how-sql-database-long-term-retention-works"></a>SQL veritabanı uzun süreli saklama nasıl çalışır?

Uzun süreli yedek saklama (LTR) yararlanır, tam veritabanı yedeklemeleri [otomatik olarak oluşturulan](sql-database-automated-backups.md) belirli bir noktaya geri yükleme (PITR) etkinleştirmek için. LTR İlkesi yapılandırdıysanız, bu yedeklemeleri uzun vadeli depolama için farklı blobları kopyalanır. Kopyalama işlemi herhangi bir performans etkisi veritabanı iş yüküne sahip bir arka plan işi var. LTR yedekleme LTR İlkesi tarafından ayarlanan süreyi saklanır. Her bir SQL veritabanı LTR İlkesi de LTR yedeklemelerin ne sıklıkta oluşturulacağını belirtebilirsiniz. Bu esneklik etkinleştirmek için dört parametre birleşimi kullanarak ilkesi tanımlayabilirsiniz: Haftalık yedekleme bekletme (W), aylık yedekleme bekletme (M), yıllık yedekleme bekletme (Y) ve (WeekOfYear) yılın haftası. Belirtirseniz W, haftada bir yedekleme uzun süreli depolama alanına kopyalanır. M belirtirseniz, her ayın ilk haftasında sırasında bir yedek uzun süreli depolama alanına kopyalanacak. Y belirtirseniz, bir yedekleme WeekOfYear tarafından belirtilen bir hafta sırasında uzun süreli depolama alanına kopyalanacak. Her yedekleme Bu parametreler tarafından belirtilen süre boyunca uzun vadeli depolamada tutulacak. LTR İlkesi'nin herhangi bir değişiklik, gelecek yedeklemelerinde geçerlidir. Örneğin, belirtilen WeekOfYear zaman İlkesi yapılandırılır geçmişte ise, sonraki yılın ilk LTR yedekleme oluşturulur. 

LTR ilkesi örnekleri:

-  W = 0, M = 0 ise, Y = 5, WeekOfYear = 3

   Beş yıl boyunca her yıl üçüncü tam yedeğini tutulacak.
   
- W = 0, M = 3, Y = 0

   Üç ay boyunca her ay ilk tam yedeklemede tutulacak.

- W=12, M=0, Y=0

   Her haftalık tam yedekleme için 12 haftaya tutulacak.

- W = 6, M = 12, Y = 10 WeekOfYear = 16

   Altı hafta için her haftalık tam yedekleme tutulacak. 12 ay boyunca ilk tam yedeklemede her ayın hangi tutulacak. Geçen yılın 16 haftası tam yedekleme dışında olduğu için 10 yıla tutulacak. 

Aşağıdaki tabloda, temposu ve şu ilkeye için uzun vadeli yedekleme sona erme gösterilmektedir:

W = 12 Hafta (84 gün), M = 12 ay (365 gün), Y = 10 yıl (3650 gün) WeekOfYear = 15 (hafta sonra 15 Nisan)

   ![LTR örneği](./media/sql-database-long-term-retention/ltr-example.png)



Yukarıdaki ilkeyi değiştirin ve yedek kopyaların W = 0 (hiçbir haftalık yedeklemeler), temposu kümesi olarak değiştirir, yukarıdaki tabloda vurgulanmış tarihlere göre gösterilir. Buna göre bu yedeklemeler tutmak için gerekli depolama miktarını azaltır. 

> [!IMPORTANT]
> Tek tek LTR yedekleme zamanlamasını, Azure SQL veritabanı tarafından denetlenir. El ile LTR yedekleme oluşturamaz veya yedekleme oluşturma zamanlamasını denetlemek. LTR İlkesi yapılandırdıktan sonra bu ilk LTR yedekleme kullanılabilir yedekler Listesi penceresinde göstermeden önce en fazla 7 gün sürebilir.  
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Coğrafi çoğaltma ve uzun süreli yedek saklama

İş sürekliliği çözümünüz olarak etkin coğrafi çoğaltma veya yük devretme grupları kullanıyorsanız, nihai yük devretme işlemleri için hazırlama ve coğrafi-ikincil veritabanı üzerinde aynı LTR ilkesini yapılandırmanız gerekir. Yedeklemeler, ikincil veritabanı ' oluşturulmaz gibi LTR depolama maliyetlerinizi artırmaz. İkincil birincil olduğunda yalnızca yedekleme oluşturulur. Birincil, ikincil bölge'ye taşınır ve yük devretme tetiklenir LTR yedekleme kesintiye oluşturulmasını sağlar. 

> [!NOTE]
> Özgün birincil veritabanını kesintiden yük devretme nedeni kurtarıldığında, yeni ikincil olur. Bu nedenle, yedekleme oluşturma değil sürdürür ve birincil yeniden olana kadar mevcut LTR ilke etkili olmaz. 

## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure portal veya PowerShell kullanarak uzun süreli saklama yapılandırma konusunda bilgi için bkz: [yönetme Azure SQL veritabanı uzun süreli yedek saklama](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-database-from-ltr-backup"></a>Veritabanı LTR yedekten geri yükleyin.

LTR depolamadan bir veritabanını geri yüklemek için zaman damgası üzerinde dayalı belirli bir yedekleme seçebilirsiniz. Veritabanını özgün veritabanıyla aynı abonelik altında var olan herhangi bir sunucuya geri yüklenebilir. Veritabanınızı Azure portalı veya PowerShell kullanarak bir LTR yedekten geri yükleme hakkında bilgi edinmek için bkz. [yönetme Azure SQL veritabanı uzun süreli yedek saklama](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı yedeklerinin yanlışlıkla ya da silme verileri korumak için önemli bir bölümü herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejiniz oldukları. Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
