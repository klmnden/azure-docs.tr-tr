---
title: Azure SQL veri ambarı iş yükü önem | Microsoft Docs
description: Önem sorgular için Azure SQL veri ambarı'nda ayarlamaya yönelik yönergeler.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 05/01/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 0147977307ec22134777d6c3e8242a4191362ada
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66233832"
---
# <a name="azure-sql-data-warehouse-workload-importance"></a>Azure SQL veri ambarı iş yükü önem derecesi

Bu makalede, SQL veri ambarı istekler için yürütme sırasını iş yükü önem nasıl etkileyebilir açıklanmaktadır.

## <a name="importance"></a>Önem derecesi

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

İş gereksinimlerini, veri ambarı iş yükleriniz diğerlerinden daha önemli olmasını gerektirebilir.  Görev açısından kritik satış verilerini mali dönem kapatmadan önce yüklendiği bir senaryo düşünün.  Hava durumu verileri katı bir SLA yoktur gibi diğer kaynaklar için verileri yükler.   Satış verileri yüklemek bir istek için yüksek önem ve düşük öneme sahip bir istek veri satış verilerini Yük sağlar olsun ayarlama ilk erişim kaynakları alır ve daha hızlı tamamlanır.

## <a name="importance-levels"></a>Önem düzeyi

Beş önem düzeyi vardır: Düşük, below_normal, normal, above_normal ve yüksek.  Önem derecesi ayarlamamanız istekleri normal varsayılan düzeyi atanır.  Aynı önem düzeyine sahip istekler, bugün var olan planlama aynı davranışa sahip.

## <a name="importance-scenarios"></a>Önem derecesi senaryoları

Satış ve hava durumu verileri ile yukarıda açıklanan temel önem senaryosu dışında burada iş yükü önem veri işleme ve gereksinimlerini sorgulama karşılayacak diğer senaryolar vardır.

### <a name="locking"></a>Kilitleme

Kilitleri okumak üzere erişim ve yazma etkinliği bir doğal Çekişme alanıdır.  Gibi etkinlikler [bölüm değiştirme](/azure/sql-data-warehouse/sql-data-warehouse-tables-partition) veya [NESNEYİ Yeniden Adlandır](/sql/t-sql/statements/rename-transact-sql) yükseltilmiş kilitleri gerektirir.  İş yükü önem, aktarım hızı için SQL veri ambarı en iyi duruma getirir.  Kuyruğa alınan istekler, aktarım hızı çalıştırırken anlamına gelir ve kuyruğa alınan istekler aynı kilitleme gereksinimlerine sahip ve kaynaklar kullanılabilir en iyi duruma getirme, istek sırasında daha önce gelen ihtiyaçlarını daha yüksek kilitleme ile istekleri atlayabilirsiniz.  İş yükü önem derecesi yüksek kilitleme ile isteklerine uygulandıktan sonra gerekir. İstekle daha yüksek önem derecesi daha düşük önem derecesiyle istekten önce çalıştırılır.

Aşağıdaki örnek göz önünde bulundurun:

S1 etkin olarak çalıştığını ve SalesFact verileri seçme.
S2 S1 tamamlanması bekleniyor kuyruğa alınır.  Bu, 9'da gönderildi ve bölüm anahtarı yeni verileri SalesFact çalışıyor.
Q3 09:01:00 gönderilen ve SalesFact verileri seçmenizi ister.

S2 ve S3'ü aynı öneme sahip ve S1 hala Yürütülüyor iken, yürütme Q3 başlar. S2 için özel bir kilit üzerinde SalesFact beklemeye devam edecek.  S2, S3'dan daha yüksek önem varsa, S3 S2 yürütme başlamadan önce işlemi tamamlanana kadar bekler.

### <a name="non-uniform-requests"></a>Tekdüze olmayan istekler

Farklı kaynak sınıfları ile istekleri gönderildiğinde önem sorgulanırken karşılayın burada yardımcı olabilecek başka bir senaryodur.  SQL veri ambarı, daha önce aynı öneme altında belirtildiği gibi verimliliğini iyileştirir.  (Örneğin, smallrc veya mediumrc) karma boyutu istekler kuyruğa alınır, SQL veri ambarı içindeki kullanılabilir kaynakların en uygun erken gelen istek seçersiniz.  En yüksek önem derecesi isteği, iş yükü önem uygulanırsa, sonraki zamanlanır.
  
Aşağıdaki örnek DW500c üzerinde göz önünde bulundurun:

S1, S2, S3 ve S4 smallrc sorguları çalışıyor.
S5 mediumrc kaynak sınıfıyla 9'da gönderilir.
S6 smallrc kaynak sınıfı ile 09:01:00 gönderilir.

S5 mediumrc olduğundan, iki eşzamanlılık yuvası gerektirir.  S5 iki çalışan sorguların tamamlanması için beklemesi gerekir.  Ancak, bir çalışan sorguların (S1-4) tamamlandığında, sorguyu yürütmek için kaynaklar olduğundan S6 hemen zamanlanır.  S5 S6 daha yüksek önem varsa, S6 S5 yürütme başlamadan önce çalışan kadar bekler.

## <a name="next-steps"></a>Sonraki adımlar

- Sınıflandırıcı oluşturma hakkında daha fazla bilgi için bkz. [iş YÜKÜ SINIFLANDIRICI oluşturma (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql).  
- SQL veri ambarı iş yükü sınıflandırma hakkında daha fazla bilgi için bkz: [iş yükü sınıflandırma](sql-data-warehouse-workload-classification.md).  
- Bu hızlı başlangıçta bkz [iş yükü sınıflandırıcı oluşturma](quickstart-create-a-workload-classifier-tsql.md) iş yükü sınıflandırıcı oluşturmak için.
- Nasıl yapılır makalelerine bakın [iş yükü önem yapılandırma](sql-data-warehouse-how-to-configure-workload-importance.md) ve nasıl [yönetme ve izleme iş yükü yönetimi](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
- Bkz: [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) sorgular ve atanan önem görüntülemek için.
