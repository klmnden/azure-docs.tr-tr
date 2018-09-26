---
title: Azure SQL veritabanı ölçek kaynakları | Microsoft Docs
description: Bu makalede, veritabanınızı ekleyerek veya kaldırarak ayrılan kaynakları ölçeklendirme açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: cd0653cf1920bd62621b89410b8cd2de2570fae3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162927"
---
# <a name="scale-database-resources"></a>Ölçek veritabanı kaynakları

Azure SQL veritabanı, veritabanı en düşük kapalı kalma süresi ile dinamik olarak daha fazla kaynak eklemek sağlar.

## <a name="overview"></a>Genel Bakış

Uygulamanıza yönelik talep cihaz ve müşteriye müşteriden milyonlarca kullanıcıya çıkması durumunda, Azure SQL veritabanı en az kapalı kalma süresiyle uygulamanız çalışırken ölçeklendirir. Ölçeklenebilirlik dinamik olarak gerektiğinde, hizmetinizin daha fazla kaynak eklemenize olanak sağlayan paas en önemli özelliklerinden biridir. Azure SQL veritabanı, veritabanları için ayrılan kaynakları (CPU gücü, bellek, g/ç aktarım hızı ve depolama) kolayca değiştirmenize olanak tanır.  
Dizin oluşturma kullanarak düzeltilemeyen veya yeniden yazma yöntemleri sorgu uygulamanızın artan kullanım nedeniyle performans sorunlarını azaltabilirsiniz. Daha fazla kaynak eklemek, veritabanınızı geçerli kaynak sınırı ziyaret sayısı ve gelen iş yükünü işlemek için daha fazla güç gerektiğinde hızlı bir şekilde tepki vermek sağlar. Azure SQL veritabanı, kaynakları maliyetlerini düşürmeye değil gerektiğinde ölçeği olanak sağlar.
Donanım satın alma ve altyapının değiştirme hakkında endişelenmeniz gerekmez. Veritabanı ölçeklendirme Azure portalında bir Kaydırıcıyı kullanarak kolayca yapılabilir.

![Veritabanı performansı ölçeklendirin](media/sql-database-scalability/scale-performance.svg)

Azure SQL veritabanı sunan bir [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). 
-   [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) işlem, bellek ve hafif ve ağır veritabanı iş yüklerini desteklemek için üç hizmet katmanı g/ç kaynakları karışımını sunar: temel, standart ve Premium. Her katman içindeki performans düzeyleri, bu kaynakların farklı bir karışımını sağlar ve bunlara ek depolama kaynakları da eklenebilir.
-   [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) sanal çekirdek miktarı veya bellek ve miktarını sayısını ve depolama hızını seçmenize olanak sağlar. Bu satın alma modeli, üç hizmet katmanı sunar: genel amaçlı, iş açısından kritik ve hiper ölçekli (Önizleme).
Düşük bir maliyetle genel amaçlı hizmet katmanındaki aylık küçük, tek bir veritabanı üzerinde ilk uygulamanızı oluşturun ve ardından Hizmet katmanını elle veya programlama yoluyla herhangi bir zamanda iş kritik hizmet katmanına çözümünüzün gereksinimlerini karşılayacak şekilde değiştirebilirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

> [!IMPORTANT]
> Genel amaçlı veya iş açısından kritik hizmet katmanından bir hiper ölçekli hizmet katmanına ölçeklendirilemez. Ancak, Hiper ölçekli bir hizmet katmanında performans düzeylerini değiştirebilirsiniz.

> [!NOTE]
> Dinamik ölçeklenebilirlik, otomatik ölçeklendirmeden farklıdır. Bir hizmet ölçütlere dayalı olarak otomatik şekilde ölçeklendirildiğinde otomatik ölçeklendirme oluşurken, dinamik ölçeklenebilirlik ise kesinti süresi olmadan el ile ölçeklendirmeye olanak sağlar.

Tek Azure SQL Veritabanı, el ile dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirmeyi desteklemez. Daha *otomatik* bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın.
Ancak, tek bir Azure SQL veritabanı için ölçeklenebilirlik otomatikleştirilmesine yardımcı olur betik vardır. Örnek için bkz. [PowerShell kullanarak tek bir SQL Veritabanını izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-database-powershell.md).

Değiştirebileceğiniz [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) veya [sanal çekirdek özelliklere](sql-database-vcore-resource-limits-single-databases.md) (genellikle ortalama dört saniyenin altında) uygulamanızda çok az kesinti ile dilediğiniz zaman. Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir sayıda Edtu havuzunda birden fazla veritabanı arasında paylaşılan ile elastik havuz kullanın.

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Azure SQL veritabanı üç türdeki tüm veritabanlarınızın dinamik olarak ölçeklendirmeniz mi bazı olanaklar sunar:
-   İçinde [tek Azure SQL veritabanı](sql-database-single-database-scale.md), kullanabilirsiniz [DTU](sql-database-dtu-resource-limits-single-databases.md) veya [sanal çekirdek](sql-database-vcore-resource-limits-single-databases.md) maksimum miktarı her bir veritabanı için atanan kaynakları tanımlamak için modeller.
-   [Azure SQL yönetilen örneği](sql-database-managed-instance.md) kullanan [sanal çekirdekler](sql-database-managed-instance.md#vcore-based-purchasing-model) modu ve maksimum CPU Çekirdeği ve en fazla depolama, örneğe ayrılan tanımlamanızı sağlar. Örneği içindeki tüm veritabanlarına örneğine ayrılan kaynaklar paylaşır.
-   [Azure SQL elastik havuzları](sql-database-elastic-pool-scale.md) , havuzda veritabanı başına en fazla kaynak sınırı tanımlamanıza olanak sağlar.

## <a name="alternative-scale-methods"></a>Alternatif ölçek yöntemleri
Kaynakları ölçeklendirme, kolay ve veritabanı veya uygulama kodunu değiştirmeden veritabanınızın performansını geliştirmek için en etkili yolu değildir. Bazı durumlarda bile en yüksek hizmet katmanları, bilgi işlem boyutlarına ve performans iyileştirmeleriyle iş yükünüze dayalı başarılı ve uygun maliyetli bir şekilde işlememesi. Bu durumda, veritabanınızı ölçeklendirmek için bu ek seçeneğiniz vardır:
-   [Okuma ölçeği genişletme](sql-database-read-scale-out.md) bir özellik, burada bir salt okunur çoğaltma, verilerinizin nerede salt okunur sorguların raporlar gibi zorlu yürütebilir aldığınıza kullanılabilir. Kırmızı-yalnızca çoğaltma salt okunur iş yükü kaynak kullanımını, birincil veritabanında etkilemeden işler.
-   [Veritabanı parçalama](sql-database-elastic-scale-introduction.md) verilerinizi birkaç veritabanlarına bölme ve bağımsız olarak ölçeklendirme sağlayan teknikleri kümesidir.

## <a name="next-steps"></a>Sonraki adımlar
- Veritabanı kod değiştirerek veritabanı performansını iyileştirme hakkında daha fazla bilgi için bkz: [bulun ve performans önerilerini uygulama](sql-database-advisor-portal.md).
- Veritabanınızı iyileştirin veritabanı yerleşik zekası izin vererek hakkında daha fazla bilgi için bkz: [otomatik ayarlama](sql-database-automatic-tuning.md).
- Azure SQL veritabanı hizmetinde okuma ölçeği genişletilmiş hakkında daha fazla bilgi için bkz. nasıl [salt okunur çoğaltmaların salt okunur sorgu dengelemeye sayfanızdaki](sql-database-read-scale-out.md).
- Bir veritabanı parçalama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).

