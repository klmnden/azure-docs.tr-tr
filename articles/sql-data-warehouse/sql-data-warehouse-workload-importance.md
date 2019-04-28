---
title: İş yükü önem | Microsoft Docs
description: Önem sorgular için Azure SQL veri ambarı'nda ayarlamaya yönelik yönergeler.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 12e7d9bc22eff14bbf302aed50080412d04a40d3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61474711"
---
# <a name="sql-data-warehouse-workload-importance-preview"></a>SQL veri ambarı iş yükü önem (Önizleme)

Bu makalede, SQL veri ambarı istekler için yürütme sırasını iş yükü önem nasıl etkileyebilir açıklanmaktadır.

> [!Note]
> İş yükü Sınıflandırma, SQL veri ambarı Gen2'te önizlemesi için kullanılabilir. 9 Nisan 2019 veya sonraki bir sürüm tarihi yapılarla Önizleme iş yükü yönetimi sınıflandırma ve önem derecesi içindir.  Kullanıcılar, iş yükü yönetimi için test derlemeleri bu tarihten önceki kullanmaktan kaçınmanız gerekir.  Derleme iş yükü yönetimi özelliğine sahip olup olmadığını belirlemek için çalıştırdığınızda @ seçin@version SQL veri ambarı Örneğinize bağlandığında.

## <a name="importance"></a>Önem derecesi

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

İş gereksinimlerini, veri ambarı iş yükleriniz diğerlerinden daha önemli olmasını gerektirebilir.  Görev açısından kritik satış verilerini mali dönem kapatmadan önce yüklendiği bir senaryo düşünün.  Hava durumu verileri katı bir SLA yoktur gibi diğer kaynaklar için verileri yükler.   Satış verileri yüklemek bir istek için yüksek önem ve düşük öneme sahip bir istek veri satış verilerini Yük sağlar olsun ayarlama ilk erişim kaynakları alır ve daha hızlı tamamlanır.

## <a name="importance-levels"></a>Önem düzeyi

Beş önem düzeyi vardır: Düşük, below_normal, normal, above_normal ve yüksek.  Önem derecesi ayarlamamanız istekleri normal varsayılan düzeyi atanır.  Aynı önem düzeyine sahip istekler, bugün var olan planlama aynı davranışa sahip.

## <a name="importance-scenarios"></a>Önem derecesi senaryoları

Satış ve hava durumu verileri ile yukarıda açıklanan temel önem senaryosu dışında burada iş yükü önem veri işleme ve gereksinimlerini sorgulama karşılayacak diğer senaryolar vardır.

### <a name="locking"></a>Kilitleniyor

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

SQL veri ambarı iş yükü sınıflandırma hakkında daha fazla bilgi için bkz: [SQL veri ambarı iş yükü sınıflandırma](sql-data-warehouse-workload-classification.md) ve [iş yükü sınıflandırıcı oluşturma](quickstart-create-a-workload-classifier-tsql.md). Bkz: [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) sorgular ve atanan önem görüntülemek için.
