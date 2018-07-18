---
title: İzleme ve performans ayarlama - Azure SQL veritabanı | Microsoft Docs
description: Performans değerlendirmesi ve geliştirme ile Azure SQL veritabanı'nda ayarlama için ipuçları.
services: sql-database
author: v-shysun
manager: craigg
editor: ''
keywords: sql performansı ayarlama, veritabanı performans ayarlama, sql performans ayarlama ipuçları, sql veritabanı performansını ayarlama
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: v-shysun
ms.openlocfilehash: 79f41ab133cba539e5f855b3ab8fa21723694acb
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092571"
---
# <a name="monitoring-and-performance-tuning"></a>İzleme ve performans ayarlama

Azure SQL veritabanı otomatik olarak yönetilir ve burada kolayca izleyebilir, kullanım, ekleyebilir veya kaynakları (CPU, bellek, g/ç) kaldırmak esnek veri hizmeti bulun, veritabanınızın performansını veya veritabanı iş yükünüze uyum sağlar. öneriler ve otomatik olarak performansı iyileştirin.

Bu makalede izleme ve performans ayarlama, Azure SQL veritabanı'nda kullanılabilir olan seçenekleri genel bakış sağlar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>İzleme ve veritabanı performans sorunlarını giderme

Azure SQL veritabanı kolayca veritabanı kullanımınızı izleyin ve performans sorunlarına neden sorguları belirlemenize olanak sağlar. Azure portalı veya sistem görünümlerini kullanarak veritabanı performansını izleyebilirsiniz. İzleme ve sorun giderme veritabanı performans için aşağıdaki seçenekleriniz:

1. İçinde [Azure portalında](https://portal.azure.com), tıklayın **SQL veritabanları**, veritabanını seçin ve ardından aramak için en fazla yaklaşan kaynakları için izleme grafiği kullanın. DTU tüketimi, varsayılan olarak gösterilir. Tıklayın **Düzenle** gösterilen değerler ve zaman aralığını değiştirmek için.
2. Kullanım [sorgu performansı İçgörüleri](sql-database-query-performance.md) kaynaklarının en çok harcama sorguları tanımlamak için.
3. Dinamik Yönetim görünümlerini (Dmv'ler) genişletilmiş olaylar kullanabilirsiniz (`XEvents`) ve gerçek zamanlı olarak performans parametrelerini alın ssms'de Query Store.

Bkz: [performans Kılavuzu konu](sql-database-performance-guidance.md) bu raporları ve görünümleri kullanarak bazı sorunu olduğunu belirlerseniz, Azure SQL veritabanı performansını artırmak için kullanabileceğiniz teknikleri bulunacak.

> [!IMPORTANT] 
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı sürümde olmak için her zaman en güncel Management Studio sürümünü kullanmanız önerilir. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Veritabanı performansını artırmak için en iyi duruma getirme

Azure SQL veritabanı sayesinde artırmak ve kaynakları değiştirmeden geçirerek sorgu performansını iyileştirmek için fırsatlarını belirlemek [performans ayarlama önerilerinde](sql-database-advisor.md). Veritabanı performansının düşük olmasına yol açan yaygın nedenler, dizinlerin eksik olması ve sorguların hatalı bir şekilde iyileştirilmesidir. İş yükünüzün performansını artırmak için bu ayar önerileri uygulayabilirsiniz.
Let Azure SQL veritabanı'na ayrıca [otomatik olarak, sorguların performansını en iyi duruma getirme](sql-database-automatic-tuning.md) uygulayarak tüm öneriler ve veritabanı performansını artırmak doğrulama belirledik. Veritabanınızın performansını artırmak için aşağıdaki seçenekleri kullanabilirsiniz:

1. Kullanım [SQL veritabanı Danışmanı](sql-database-advisor-portal.md) oluşturmak ve dizinleri bırakmayı, sorguları kümesini parametreleştirme ve şema sorunlarını giderme önerileri görüntüleyebilirsiniz.
2. [Otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md) ve Azure SQL tanımlanan düzeltme performans sorunlarını otomatik olarak veritabanı sağlar.

## <a name="improving-database-performance-with-more-resources"></a>Daha fazla kaynak ile veritabanı performansı iyileştirme

Son olarak, veritabanınızın performansını iyileştirebilir hiçbir eyleme dönüştürülebilir öğe varsa, Azure SQL veritabanı'nda kullanılabilir kaynakların miktarını değiştirebilirsiniz. Daha fazla kaynak değiştirerek atayabilirsiniz [DTU hizmet katmanı](sql-database-service-tiers-dtu.md) bir tek başına veritabanı veya elastik havuz edtu'ları dilediğiniz zaman artırın. Alternatif olarak, kullanıyorsanız [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md), hizmet katmanını değiştirebilir veya veritabanınız için ayrılan kaynakları artırın. 
1. Tek başına veritabanları için yapabilecekleriniz [hizmet katmanlarını değiştirme](sql-database-service-tiers-dtu.md) veya [işlem kaynaklarını](sql-database-service-tiers-vcore.md)veritabanı performansını artırmak için isteğe bağlı.
2. Birden fazla veritabanı için kullanmayı [elastik havuzlar](sql-database-elastic-pool-guidance.md) kaynakları otomatik olarak ölçeklendirmek için.

## <a name="tune-and-refactor-application-or-database-code"></a>Ayarlayın ve yeniden düzenleme uygulama veya veritabanı kod

Uygulama kodunun daha verimli veritabanı kullanmak, dizinleri değiştirin, plan zorlama veya iş yükünüz için veritabanını el ile uyum ipuçlarını kullanma değiştirebilirsiniz. El ile ayarlama ve kodu yeniden yazma için bazı yönergeler ve ipuçları bulabilirsiniz [performans Kılavuzu konu](sql-database-performance-guidance.md) makalesi.


## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı'nda Otomatik ayarlamayı etkinleştirme ve otomatik ayarlama özelliğini tam olarak iş yükünüzü yönetmenize izin vermek için bkz: [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).
- El ile ayarlama kullanmak için gözden geçirebilirsiniz [ayarlama önerileri Azure portalında](sql-database-advisor-portal.md) ve el ile sorgularınızı performansını olanları uygulayın.
- Değiştirerek, veritabanında mevcut olan kaynakları değiştirmek [Azure SQL veritabanı hizmet katmanları](sql-database-performance-guidance.md)
