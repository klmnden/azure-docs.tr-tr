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
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 06/25/2019
ms.openlocfilehash: d8949f63dfa9b409cc14fe9c3bbed70f23a73c86
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357143"
---
# <a name="dynamically-scale-database-resources-with-minimal-downtime"></a>Dinamik olarak ölçek en düşük kapalı kalma süresi ile veritabanı kaynakları

Azure SQL veritabanı sayesinde veritabanınızı en düşük ile dinamik olarak daha fazla kaynak eklemek [kapalı kalma süresi](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/); ancak olduğundan geçiş dönemi boyunca bağlantı olduğu için veritabanı kayıp bir kısa sürede olabilen, yeniden deneme mantığı kullanılarak azaltılabilir.

## <a name="overview"></a>Genel Bakış

Uygulamanıza yönelik talep cihaz ve müşteriye müşteriden milyonlarca kullanıcıya çıkması durumunda, Azure SQL veritabanı en az kapalı kalma süresiyle uygulamanız çalışırken ölçeklendirir. Ölçeklenebilirlik dinamik olarak gerektiğinde, hizmetinizin daha fazla kaynak eklemenize olanak sağlayan paas en önemli özelliklerinden biridir. Azure SQL veritabanı, veritabanları için ayrılan kaynakları (CPU gücü, bellek, g/ç aktarım hızı ve depolama) kolayca değiştirmenize olanak tanır.

Dizin oluşturma kullanarak düzeltilemeyen veya yeniden yazma yöntemleri sorgu uygulamanızın artan kullanım nedeniyle performans sorunlarını azaltabilirsiniz. Daha fazla kaynak eklemek, veritabanınızı geçerli kaynak sınırı ziyaret sayısı ve gelen iş yükünü işlemek için daha fazla güç gerektiğinde hızlı bir şekilde tepki vermek sağlar. Azure SQL veritabanı, kaynakları maliyetlerini düşürmeye değil gerektiğinde ölçeği olanak sağlar.

Donanım satın alma ve altyapının değiştirme hakkında endişelenmeniz gerekmez. Veritabanı ölçeklendirme Azure portalında bir Kaydırıcıyı kullanarak kolayca yapılabilir.

![Veritabanı performansı ölçeklendirin](media/sql-database-scalability/scale-performance.svg)

Azure SQL veritabanı sunar [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

- [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) işlem, bellek ve hafif ve ağır veritabanı iş yüklerini desteklemek için üç hizmet katmanı g/ç kaynakları karışımını sunar: Temel, Standart ve Premium. Her katman içindeki performans düzeyleri, bu kaynakların farklı bir karışımını sağlar ve bunlara ek depolama kaynakları da eklenebilir.
- [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) sanal çekirdek miktarı veya bellek ve miktarını sayısını ve depolama hızını seçmenize olanak sağlar. Bu satın alma modeli, üç hizmet katmanı sunmaktadır: Genel amaçlı, iş açısından kritik ve hiper ölçekli.

Düşük bir maliyetle temel, standart veya genel amaçlı bir hizmet katmanındaki aylık küçük, tek bir veritabanı üzerinde ilk uygulamanızı oluşturun ve ardından Hizmet katmanını elle veya programlama yoluyla herhangi bir zamanda ne karşılamak için Premium veya iş açısından kritik hizmet katmanına değiştirebilirsiniz çözümünüzün EDS. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

> [!NOTE]
> Dinamik ölçeklenebilirlik, otomatik ölçeklendirmeden farklıdır. En düşük kapalı kalma süresi ile el ile ölçeklendirme için dinamik ölçeklenebilirlik sağlar ancak bir hizmet ölçütlere göre otomatik olarak ölçeklenen otomatik ölçeklendirme olur.

Tek Azure SQL Veritabanı, el ile dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirmeyi desteklemez. Daha *otomatik* bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın.
Ancak, tek bir Azure SQL veritabanı için ölçeklenebilirlik otomatikleştirilmesine yardımcı olur betik vardır. Örnek için bkz. [PowerShell kullanarak tek bir SQL Veritabanını izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-database-powershell.md).

Değiştirebileceğiniz [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) veya [sanal çekirdek özelliklere](sql-database-vcore-resource-limits-single-databases.md) (genellikle ortalama dört saniyenin altında) uygulamanızda çok az kesinti ile dilediğiniz zaman. Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir sayıda Edtu havuzunda birden fazla veritabanı arasında paylaşılan ile elastik havuz kullanın.

![SQL Database'e giriş: Katmana ve düzeye göre tek veritabanı dtu'ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Azure SQL veritabanı üç türdeki tüm veritabanlarınızın dinamik olarak ölçeklendirmeniz mi bazı olanaklar sunar:

- İle bir [tek veritabanı](sql-database-single-database-scale.md), kullanabilirsiniz [DTU](sql-database-dtu-resource-limits-single-databases.md) veya [sanal çekirdek](sql-database-vcore-resource-limits-single-databases.md) maksimum miktarı her bir veritabanı için atanan kaynakları tanımlamak için modeller.
- A [yönetilen örneği](sql-database-managed-instance.md) kullanan [sanal çekirdekler](sql-database-managed-instance.md#vcore-based-purchasing-model) modu ve maksimum CPU Çekirdeği ve en fazla depolama, örneğe ayrılan tanımlamanızı sağlar. Örneği içindeki tüm veritabanlarına örneğine ayrılan kaynaklar paylaşır.
- [Elastik havuzlar](sql-database-elastic-pool-scale.md) , havuzda veritabanı başına en fazla kaynak sınırı tanımlamanıza olanak sağlar.

> [!NOTE]
> Ölçeği artırma/ölçek, işlemi bir kısa bağlantı kesme tamamlandı bekleyebilirsiniz. Uyguladıysanız [yeniden deneme mantığı standart geçici hatalar için](sql-database-connectivity-issues.md#retry-logic-for-transient-errors), yük devretme görürsünüz değil.

## <a name="alternative-scale-methods"></a>Alternatif ölçek yöntemleri

Kaynakları ölçeklendirme, kolay ve veritabanı veya uygulama kodunu değiştirmeden veritabanınızın performansını geliştirmek için en etkili yolu değildir. Bazı durumlarda bile en yüksek hizmet katmanları, bilgi işlem boyutlarına ve performans iyileştirmeleriyle iş yükünüze dayalı başarılı ve uygun maliyetli bir şekilde işlememesi. Bu durumda, veritabanınızı ölçeklendirmek için bu ek seçeneğiniz vardır:

- [Okuma ölçeği genişletme](sql-database-read-scale-out.md) bir özellik, burada bir salt okunur çoğaltma, verilerinizin nerede salt okunur sorguların raporlar gibi zorlu yürütebilir aldığınıza kullanılabilir. Salt okunur çoğaltma, birincil veritabanınızın üzerindeki kaynak kullanımının etkilemeden salt okunur iş yükünüz işleyecektir.
- [Veritabanı parçalama](sql-database-elastic-scale-introduction.md) verilerinizi birkaç veritabanlarına bölme ve bağımsız olarak ölçeklendirme sağlayan teknikleri kümesidir.

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı kod değiştirerek veritabanı performansını iyileştirme hakkında daha fazla bilgi için bkz: [bulun ve performans önerilerini uygulama](sql-database-advisor-portal.md).
- Veritabanınızı iyileştirin veritabanı yerleşik zekası izin vererek hakkında daha fazla bilgi için bkz: [otomatik ayarlama](sql-database-automatic-tuning.md).
- Azure SQL veritabanı hizmetinde okuma ölçeği genişletilmiş hakkında daha fazla bilgi için bkz. nasıl [salt okunur çoğaltmaların salt okunur sorgu dengelemeye sayfanızdaki](sql-database-read-scale-out.md).
- Bir veritabanı parçalama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).
