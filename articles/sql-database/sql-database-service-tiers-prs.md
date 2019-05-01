---
title: Azure SQL veritabanı Premium RS hizmet katmanı devre dışı bırakma | Microsoft Docs
description: Premium RS Hizmet katmanını devre dışı bırakılıyor ve desteği sona eriyor - geçiş seçenekleri bakın.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: e12b89d0469587d7d7326bbee30f6467ada06bd5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64574073"
---
# <a name="azure-sql-database-premium-rs-service-tier-preview-is-being-retired---options-for-migration"></a>Azure SQL veritabanı Premium RS hizmet Katmanı (Önizleme) kullanımdan - geçiş seçenekleri

Şubat 2018'de Microsoft Azure SQL veritabanı Premium RS hizmet katmanında genel kullanıma sunulduktan değil ve artık 31 Ocak 2019 sonra desteklenen duyurdu. Bu destek son sonuna 30 Haziran 2019 için genişletilmiştir. Bu makalede, başka bir hizmet katmanına Premium RS hizmet katmanından geçiş seçenekleriniz açıklanır. 30 Haziran 2019'dan sonra Microsoft Premium RS veritabanınızın performans gereksinimleriyle en yakından eşleşen bir genel kullanıma sunulan hizmet katmanı için Premium RS veritabanlarınızı otomatik olarak geçirir.

Geçiş hedefler ve Premium RS müşteriler için uygun bir fiyatlandırma seçenekleri aşağıda verilmiştir:

- Sanal çekirdek hizmet katmanları

  **Genel amaçlı** ve **iş açısından kritik** hizmet katmanlarında [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). Bu iki hizmet katmanı, genel kullanım aşamasındadır. Sanal çekirdek tabanlı satın alma modeli de sunar **hiper ölçekli** isteğe bağlı veritabanı başına en fazla 100 TB otomatik ölçeklendirme ile İş yükünüzün gereksinimlerine uyum sağlayan bir hizmet katmanındaki (genel Önizleme). G/ç performansı için Premium hizmet katmanında karşılaştırılabilir hiper ölçekli hizmet katmanı sunar [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) fiyattan daha yakın Premium RS hizmet katmanı için.
- Geliştirme ve Test fiyatlandırması

  [Geliştirme ve test fiyatlandırması](https://azure.microsoft.com/pricing/dev-test/) Visual Studio aboneliğinizle %55 lisansın dahil edildiği oranları karşı varan tasarruf sağlar.
- Azure hibrit avantajı ve ayrılmış kapasite fiyatlandırması

  [Azure hibrit avantajı ve ayrılmış kapasite fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/) % 80'lisansın dahil edildiği oranları karşı tasarruf sağlayın. Bu seçenekler hakkında daha fazla bilgi için bkz. [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) ve [Azure SQL veritabanı ayrılan kapasite](sql-database-reserved-capacity.md).

## <a name="act-now-to-migrate-your-premium-rs-databases-to-alternative-sql-database-service-tiers"></a>Premium RS veritabanlarınızı alternatif SQL veritabanı hizmet katmanları için şimdi geçirmek için harekete geçin

Yanı sıra sunduğumuz fiyatlandırma ve belgeler Premium RS iş yükleriniz için doğru geçiş destination(s) belirlemek için bu makaledeki yönergeleri inceleyin.

## <a name="migrate-compute-intensive-workloads-and-save"></a>Yoğun işlem gücü kullanımlı iş yüklerini geçirin ve kaydedin

Yoğun işlem gücü kullanımlı Premium RS iş yükleriniz için SQL Server ve ayrılmış kapasite teklifler için Azure hibrit avantajı kullanılarak lisansın dahil edildiği oranları ve daha fazla tasarruf edin ve genel olarak kullanılabilir sanal çekirdek tabanlı genel amaçlı hizmet katmanımız geçirme öneririz. Bir DTU tabanlı satın alma seçeneği yerine kalır, standart hizmet katmanı için yoğun işlem gücü kullanımlı Premium RS veritabanlarınızı geçirme ve hala (bunu genel kullanıma geçmiş) Premium RS genel kullanılabilirlik fiyatlandırması ile kaydedin.

> [!WARNING]
> DTU tabanlı Premium hizmet katmanları için Premium RS iş yüklerinizin geçişini Premium RS geçerli fiyatlandırma karşı aylık maliyetlerinizi artırabilir. Azure hibrit avantajı ve ayrılmış kapasite Premium RS daha benzer veya daha düşük maliyetler korumak için fiyatlandırma ile hiper ölçekli veya iş açısından kritik katmanları dikkate öneririz.

### <a name="premium-rs-databases"></a>Premium RS veritabanları

|**Şu anda...**|**Benzer şekilde, sanal çekirdek tabanlı geçirme...**|**Karşılaştırılabilir için DTU tabanlı geçirme...**|
|---|---|---|
|Premium RS 1|Genel amaçlı 1 sanal çekirdek (4. nesil)|Standart 3|
|Premium RS 2|Genel amaçlı, 2 sanal çekirdek (4. nesil)|Standart 4|
|Premium RS 4|Genel amaçlı 4 sanal çekirdek (4. nesil)|Standart 6|
|Premium RS 6|Genel amaçlı 6 sanal çekirdek (4. nesil)|Standart 7|

### <a name="premium-rs-pools"></a>Premium RS havuzlar

|**Şu anda...**|**Benzer şekilde, sanal çekirdek tabanlı geçirme...**|**Karşılaştırılabilir için DTU tabanlı geçirme...**|
|---|---|---|
|Premium RS 125 DTU havuzu|Genel amaçlı 1 sanal çekirdek (4. nesil)|Standart havuz 100 edtu'ları|
|Premium RS 250 DTU havuzu|Genel amaçlı, 2 sanal çekirdek (4. nesil)|250 standart havuz edtu'ları|
|Premium RS 500 DTU havuzu|Genel amaçlı 4 sanal çekirdek (4. nesil)|500 standart havuz edtu'ları|
|Premium RS 1000 DTU havuzu|Genel amaçlı 8 sanal çekirdek (4. nesil)|Standart havuz 1000 edtu'ları|

## <a name="optimize-savings-and-performance-for-your-io-intensive-workloads"></a>Tasarruf ve performansı yoğun g/ç iş yükleriniz için en iyi duruma getirme

Bizim sanal çekirdek tabanlı hiper ölçekli katmanında, şu anda Önizleme ve en iyi performans ve maliyet birleşimi için genel kullanıma sunulan iş açısından kritik katmanımız, yoğun g/ç veritabanı havuzları, yoğun g/ç tek veritabanlarınızı geçirme öneririz.  Aşağıdaki sanal çekirdek tabanlı seçenekleri korumak veya geçerli performansınızı artırın ve Azure karma avantajı ile birlikte ve ayrılmış kapasite fiyatlandırması paradan tasarruf.

|**Şu anda...**|**Benzer şekilde, sanal çekirdek tabanlı geçirme...**|**Karşılaştırılabilir için DTU tabanlı geçirme...**|
|---|---|---|
|Premium RS 1|(Önizleme) Hiper ölçekli 1 sanal çekirdek (4. nesil) veya kritik iş 1 sanal çekirdek (4. nesil)|Premium 1|
|Premium RS 2|(Önizleme) Hiper ölçekli 2 sanal çekirdek (4. nesil) veya kritik iş 2 sanal çekirdek (4. nesil|Premium 2|
|Premium RS 4|(Önizleme) Hiper ölçekli 4 sanal çekirdek (4. nesil) ya da iş kritik 4 sanal çekirdek (4. nesil)|Premium 4
|Premium RS 6|(Önizleme) Hiper ölçekli 6 sanal çekirdek (4. nesil) ya da iş kritik 6 sanal çekirdek (4. nesil)|Premium 6|

|**Şu anda...**|**Benzer şekilde, sanal çekirdek tabanlı geçirme...**|**Karşılaştırılabilir için DTU tabanlı geçirme...**|
|---|---|---|
|Premium RS 125 DTU havuzu|İş kritik 2 sanal çekirdek (4. nesil)|Premium havuz 125 Edtu|
|Premium RS 250 DTU havuzu|İş kritik 2 sanal çekirdek (4. nesil)|Premium 250 havuz edtu'ları|
|Premium RS 500 DTU havuzu|İş kritik 4 sanal çekirdek (4. nesil)|Premium havuz 500 Edtu|
|Premium RS 1000 DTU havuzu|İş kritik 8 sanal çekirdek (4. nesil)|Premium havuz 1000 Edtu|

## <a name="take-advantage-of-our-new-offers"></a>Yeni tekliflerimizden yararlanın

Sanal çekirdek tabanlı satın alma modeli bizim hizmet katmanlarında % 80'lisans dahil fiyatlandırma karşı kaydedebilirsiniz özel teklifler için uygundur. En fazla 55 ile lisans dahil fiyatlandırma karşı % kaydetmek için etkin Yazılım Güvencesi içeren SQL Server Standard veya Enterprise sürümü lisanslarınızı kullanın [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/). Hibrit avantajı ile birleştirebilirsiniz [Azure SQL veritabanı ayrılan kapasite](sql-database-reserved-capacity.md) fiyatlandırma ve ön bir veya üç yıllık işleme % 80 tasarruf terim.  Bugün Azure portalından hem avantajları etkinleştirin.

Herhangi bir sorunuz veya bu ilgili endişeleriniz değiştirin ya da geçiş Yardım gerekiyorsa başvurun [Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="migration-from-a-premium-rs-service-tier-to-a-service-tier-in-either-the-dtu-or-the-vcore-model"></a>DTU ya da sanal çekirdek modeli bir hizmet katmanı için bir Premium RS hizmet katmanından geçiş

### <a name="migration-of-a-database"></a>Bir veritabanı geçişi

Premium RS hizmetinden bir veritabanını geçirme herhangi bir hizmet katmanı için DTU katmanı veya sanal çekirdek modeli yükseltme veya indirgeme Premium RS hizmet katmanındaki hizmet katmanları arasında benzerdir.

### <a name="using-database-copy-to-convert-a-premium-rs-database-to-a-dtu-based-or-vcore-based-database"></a>Premium RS veritabanı DTU veya sanal çekirdek tabanlı veritabanına dönüştürmek için veritabanı kopyalama kullanma

Premium RS işlem boyutu ile herhangi bir veritabanı DTU veya sanal çekirdek tabanlı işlem boyutu kısıtlamaları veya kaynak veritabanının en büyük veritabanı boyutu hedef işlem boyutu desteklediği sürece özel sıralaması olmayan bir veritabanına kopyalayabilirsiniz. Veritabanı kopyalama, kopyalama işleminin başlangıç tarihindeki verileri anlık görüntüsünü oluşturur ve kaynak ve hedef arasında veri eşitlemeye gerçekleştirmez.

## <a name="next-steps"></a>Sonraki adımlar

- Özel hakkında ayrıntılı bilgi işlem boyutları ve tek veritabanı için kullanılabilir depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md)
- Özel hakkında ayrıntılı bilgi işlem boyutları ve elastik havuzlar için kullanılabilir depolama boyutu seçenekleri için bkz: [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md).
