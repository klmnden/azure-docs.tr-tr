---
title: Azure SQL veritabanı ölçek kaynakları | Microsoft Docs
description: Bu makalede, veritabanınızı ekleyerek veya kaldırarak ayrılan kaynakları ölçeklendirme açıklanmaktadır.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/07/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: f55ce511f6ba90c27e149ac90bbd2c8aa0b3c742
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921517"
---
# <a name="scale-database-resources"></a>Ölçek veritabanı kaynakları

Azure SQL veritabanı, veritabanı en düşük kapalı kalma süresi ile dinamik olarak daha fazla kaynak eklemek sağlar.

## <a name="overview"></a>Genel Bakış

Uygulamanıza yönelik talep cihaz ve müşteriye müşteriden milyonlarca kullanıcıya çıkması durumunda, Azure SQL veritabanı en az kapalı kalma süresiyle uygulamanız çalışırken ölçeklendirir. Ölçeklenebilirlik dinamik olarak gerektiğinde, hizmetinizin daha fazla kaynak eklemenize olanak sağlayan paas en önemli özelliklerinden biridir. Azure SQL veritabanı, veritabanları için ayrılan kaynakları (CPU gücü, bellek, g/ç aktarım hızı ve depolama) kolayca değiştirmenize olanak tanır.  
Dizin oluşturma kullanarak düzeltilemeyen veya yeniden yazma yöntemleri sorgu uygulamanızın artan kullanım nedeniyle performans sorunlarını azaltabilirsiniz. Daha fazla kaynak eklemek, veritabanınızı geçerli kaynak sınırı ziyaret sayısı ve gelen iş yükünü işlemek için daha fazla güç gerektiğinde hızlı bir şekilde tepki vermek sağlar. Azure SQL veritabanı, kaynakları maliyetlerini düşürmeye değil gerektiğinde ölçeği olanak sağlar.
Donanım satın alma ve altyapının değiştirme hakkında endişelenmeniz gerekmez. Veritabanı ölçeklendirme Azure portalında bir Kaydırıcıyı kullanarak kolayca yapılabilir.

![Veritabanı performansı ölçeklendirin](media/sql-database-scalability/scale-performance.svg)

Azure SQL veritabanı sunan bir [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md). 
-   [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) işlem, bellek ve hafif ve ağır veritabanı iş yüklerini desteklemek için üç hizmet katmanı g/ç kaynakları karışımını sunar: temel, standart ve Premium. Her katman içindeki performans düzeyleri, bu kaynakların farklı bir karışımını sağlar ve bunlara ek depolama kaynakları da eklenebilir.
-   [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) (Önizleme) sanal çekirdek miktarı veya bellek ve miktarını sayısını ve depolama hızını seçmenize olanak sağlar.
Düşük aylık maliyetlerle küçük bir veritabanı üzerinde ilk uygulamanızı oluşturabilir, ardından istediğiniz zaman el ile veya programlama yoluyla hizmet katmanını değiştirebilirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.


> [!NOTE]
> Dinamik ölçeklenebilirlik, otomatik ölçeklendirmeden farklıdır. Bir hizmet ölçütlere dayalı olarak otomatik şekilde ölçeklendirildiğinde otomatik ölçeklendirme oluşurken, dinamik ölçeklenebilirlik ise kesinti süresi olmadan el ile ölçeklendirmeye olanak sağlar.
>


Tek Azure SQL Veritabanı, el ile dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirmeyi desteklemez. Daha *otomatik* bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın.
Ancak, tek bir Azure SQL veritabanı için ölçeklenebilirlik otomatikleştirilmesine yardımcı olur betik vardır. Örnek için bkz. [PowerShell kullanarak tek bir SQL Veritabanını izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-database-powershell.md).


Azure SQL veritabanı üç türdeki tüm veritabanlarınızın dinamik olarak ölçeklendirmeniz mi bazı olanaklar sunar:
-   İçinde [tek Azure SQL veritabanı](sql-database-single-database-scale.md), kullanabilirsiniz [DTU](sql-database-dtu-resource-limits-single-databases.md) veya [sanal çekirdek](sql-database-vcore-resource-limits-single-databases.md) maksimum miktarı her bir veritabanı için atanan kaynakları tanımlamak için modeller.
-   [Azure SQL yönetilen örneği](sql-database-managed-instance.md) kullanan [sanal çekirdekler](/azure/sql-database/sql-database-managed-instance#vcore-based-purchasing-model-preview) modu ve maksimum CPU Çekirdeği ve en fazla depolama, örneğe ayrılan tanımlamanızı sağlar. Örneği içindeki tüm veritabanlarına örneğine ayrılan kaynaklar paylaşır.
-   [Azure SQL elastik havuzları](sql-database-elastic-pool-scale.md) , havuzda veritabanı başına en fazla kaynak sınırı tanımlamanıza olanak sağlar.

## <a name="alternative-scale-methods"></a>Alternatif ölçek yöntemleri
Kaynakları ölçeklendirme, kolay ve veritabanı veya uygulama kodunu değiştirmeden veritabanınızın performansını geliştirmek için en etkili yolu değildir.
Bazı durumlarda, daha yüksek performans katmanları ve performans iyileştirmeleriyle iş yükünüze dayalı başarılı ve uygun maliyetli bir şekilde işlememesi. Bu durumda, veritabanınızı ölçeklendirmek için diğer seçeneğiniz vardır:
-   [Okuma ölçeği genişletme](sql-database-read-scale-out.md) bir özellik, burada bir salt okunur çoğaltma, verilerinizin nerede salt okunur sorguların raporlar gibi zorlu yürütebilir aldığınıza kullanılabilir. Kırmızı-yalnızca çoğaltma salt okunur iş yükü kaynak kullanımını, birincil veritabanında etkilemeden işler.
-   [Veritabanı parçalama](sql-database-elastic-scale-introduction.md) verilerinizi birkaç veritabanlarına bölme ve bağımsız olarak ölçeklendirme sağlayan teknikleri kümesidir.

## <a name="next-steps"></a>Sonraki adımlar
- Veritabanı kod değiştirerek veritabanı performansını iyileştirme hakkında daha fazla bilgi için bkz: [bulun ve performans önerilerini uygulama](sql-database-advisor-portal.md).
- Veritabanınızı iyileştirin veritabanı yerleşik zekası izin vererek hakkında daha fazla bilgi için bkz: [otomatik ayarlama](sql-database-automatic-tuning.md).
- Azure SQL veritabanı hizmetinde okuma ölçeği genişletilmiş hakkında daha fazla bilgi için bkz. nasıl [salt okunur çoğaltmaların salt okunur sorgu dengelemeye sayfanızdaki](sql-database-read-scale-out.md).
- Bir veritabanı parçalama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).

