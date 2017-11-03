---
title: "İzleme ve performans ayarlama - Azure SQL veritabanı | Microsoft Docs"
description: "Azure SQL veritabanı'nda ayarlama değerlendirme ve geliştirme performans ipuçları."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: "sql performans ayarlama, veritabanı performans ayarlama, sql performans ipuçları, ayarlama sql veritabanı performans ayarlama"
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/13/2017
ms.author: v-shysun
ms.openlocfilehash: 4eb344bf9e87ef1a8f469268211588e51696164e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="monitoring-and-performance-tuning"></a>İzleme ve performans ayarlama

Azure SQL veritabanı otomatik olarak yönetilir ve burada kolayca kullanımını izlemek, ekleyin veya kaynakları (CPU, bellek, g/ç) kaldırmak esnek veri hizmeti, veritabanı performansını veya veritabanı, iş yüküne uyum ve otomatik olarak performansı en iyi duruma izin önerileri bulun.

Bu makalede izleme ve performans Azure SQL veritabanı'nda kullanılabilen seçenekleri ayarlama genel bakış sağlar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>İzleme ve veritabanı performans sorunlarını giderme

Azure SQL veritabanı, kolayca veritabanı kullanımınız izlemenize ve performans sorunlarına neden sorguları belirlemenize olanak sağlar. Azure portal veya sistem görünümleri kullanarak veritabanı performansını izleyebilirsiniz. İzleme ve veritabanı performans sorunlarını giderme için aşağıdaki seçenekleriniz vardır:

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL veritabanları**veritabanını seçin ve ardından izleme grafik için kendi maksimum yaklaşan kaynakları aramak için kullanın. DTU tüketimi varsayılan olarak gösterilir. Tıklatın **Düzenle** zaman aralığı ve gösterilen değerleri değiştirmek için.
2. Kullanım [sorgu performansı öngörüleri](sql-database-query-performance.md) kaynaklarının en çok harcama sorguları tanımlamak için.
3. Dinamik Yönetim görünümlerini (Dmv'leri), genişletilmiş olaylar kullanabilirsiniz (`XEvents`) ve gerçek zamanlı performans parametreleri almak için SSMS sorgu deposunda.

Bkz: [performans Kılavuzu konu](sql-database-performance-guidance.md) bu raporları veya görünümleri kullanarak bazı sorun belirlerseniz Azure SQL veritabanı performansını iyileştirmek için kullanabileceğiniz teknikler bulunamıyor.

> [!IMPORTANT] 
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı sürümde olmak için her zaman en güncel Management Studio sürümünü kullanmanız önerilir. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Performansı artırmak için veritabanını en iyi duruma getirme

Azure SQL veritabanı geliştirmek ve kaynakları gözden geçirerek değiştirmeden Sorgu performansını iyileştirmek için fırsatları belirlemenize olanak tanır [performans ayarlama önerileri](sql-database-advisor.md). Veritabanı performansının düşük olmasına yol açan yaygın nedenler, dizinlerin eksik olması ve sorguların hatalı bir şekilde iyileştirilmesidir. İş yükünüzün performansını artırmak için bu ayarlama önerileri uygulayabilirsiniz.
Let Azure SQL veritabanına kullanabileceğiniz [otomatik olarak Sorgularınızın performansını en iyi duruma](sql-database-automatic-tuning.md) uygulayarak tüm öneriler ve bunlar veritabanı performansını iyileştirmek doğrulama tanımlanmış. Veritabanı performansını iyileştirmek için aşağıdaki seçenekleri kullanabilirsiniz:

1. Kullanım [SQL veritabanı Danışmanı](sql-database-advisor-portal.md) oluşturma ve dizinleri bırakmayı, sorguları kümesini parametreleştirme ve şema sorunlarını giderme önerileri görüntülemek için.
2. [Otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md) ve Azure otomatik olarak tanımlanan düzeltme performans sorunlarını veritabanı SQL izin verin.

## <a name="improving-database-performance-with-more-resources"></a>Daha fazla kaynak ile veritabanı performansı iyileştirme

Son olarak, veritabanınızın performansını artırabilir hiçbir işlem yapılabilir öğeler varsa, Azure SQL veritabanı'nda kullanılabilen kaynakları miktarını değiştirebilirsiniz. Daha fazla kaynak değiştirerek atayabilirsiniz [hizmet katmanı](sql-database-service-tiers.md) artış bir esnek havuz edtu'larını herhangi bir zamanda veya tek başına veritabanı.
1. Tek başına veritabanları için şunları yapabilirsiniz [hizmet katmanları değiştirmek](sql-database-service-tiers.md) veritabanı performansını artırmak için isteğe bağlı.
2. Birden çok veritabanı için kullanmayı [esnek havuzlar](sql-database-elastic-pool-guidance.md) kaynaklarını otomatik olarak ölçeklendirebilirsiniz.

## <a name="tune-and-refactor-application-or-database-code"></a>Ayarlama ve düzenleme uygulama veya veritabanı kodu

Daha verimli veritabanını kullanmak, dizinleri değiştirmek, planlar zorla veya el ile İş yükünüzün veritabanına uyarlamak için ipuçları kullanın uygulama kodu değiştirebilirsiniz. El ile ayarlama ve kodu yeniden yazma işlemi için bazı yönergeler ve ipuçları bulmak [performans Kılavuzu konu](sql-database-performance-guidance.md) makalesi.


## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı'nda otomatik olarak ayarlamayı etkinleştirmek ve işleminizi iş yükü tam olarak yönetmek otomatik ayarlama özelliği sağlar için bkz: [otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md).
- El ile ayarlamayı kullanacak şekilde gözden geçirebilirsiniz [önerileri Azure portalında ayarlama](sql-database-advisor-portal.md) ve el ile sorgularınızı performansını olanları uygulayın.
- Değiştirme, değiştirerek, veritabanında mevcut olan kaynaklar [Azure SQL Database hizmet katmanları](sql-database-performance-guidance.md)
