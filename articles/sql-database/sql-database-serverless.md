---
title: Azure SQL veritabanı sunucusuz (Önizleme) | Microsoft Docs
description: Bu makalede yeni sunucusuz bilgi işlem katmanı ve var olan sağlanan işlem katmanı ile karşılaştırır
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 06/12/2019
ms.openlocfilehash: b740b49e2decabd5f104d1db5d38b48f2bc2111c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116198"
---
# <a name="azure-sql-database-serverless-preview"></a>Azure SQL veritabanı sunucusuz (Önizleme)

Saniye başına işlem iş yükü isteğe bağlı ve faturalar işlem miktarı göre otomatik olarak ölçeklenen bir işlem katman tek veritabanları için sunucusuz (Önizleme) olan Azure SQL veritabanı kullanılır. Sunucusuz bilgi işlem katmanı, veritabanları, yalnızca depolama faturalandırılır ve bir etkinlik döndürdüğünde veritabanları otomatik olarak sürdürür etkin olmayan dönemlerde da otomatik olarak duraklatır.

## <a name="serverless-compute-tier"></a>Sunucusuz işlem katmanı

Tek bir veritabanı için sunucusuz bilgi işlem katmanı autopause gecikme ve işlem otomatik ölçeklendirme aralığı ile parametreli olup.  Bu parametre yapılandırma veritabanı performans deneyimi şekil ve işlem maliyeti.

![sunucusuz faturalandırma](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance-configuration"></a>Performans yapılandırması

- **En düşük Vcore** ve **en yüksek Vcore** veritabanı için kullanılabilir işlem kapasitesi aralığını tanımlamak yapılandırılabilir parametrelerdir. Bellek ve GÇ, sınırları, belirtilen sanal çekirdek aralığı orantılıdır.  
- **Autopause gecikme** otomatik olarak duraklatılmadan önce veritabanı süreyi tanımlar, yapılandırılabilir bir parametre etkin olmalıdır. Sonraki oturum açma veya diğer etkinlik gerçekleştiğinde veritabanı otomatik olarak sürdürülür.  Alternatif olarak, autopausing devre dışı bırakılabilir.

### <a name="cost"></a>Maliyet

- Sunucusuz bir veritabanı için toplam depolama maliyeti ve işlem maliyeti maliyetidir.
- İşlem kullanımı en az ve yapılandırılmış maksimum sınırı arasında olduğunda işlem maliyeti sanal çekirdek ve bellek kullanılan bağlıdır.
- İşlem kullanımı yapılandırılan en düşük sınırları altında olduğunda işlem maliyetini en düşük Vcore ve yapılandırılan en düşük bellek dayanır.
- Veritabanı duraklatıldığında sıfır işlem maliyeti ve yalnızca depolama maliyetlerini uygulanır.
- Depolama maliyeti, sağlanan işlem katmanında olduğu gibi aynı şekilde belirlenir.

Daha fazla maliyet ayrıntıları için bkz. [faturalama](sql-database-serverless.md#billing).

## <a name="scenarios"></a>Senaryolar

Sunucusuz işlem Isınma bazı gecikme boşta kullanım dönemlerini gücünüze aralıklı, öngörülemeyen kullanım düzenlerine sahip tek veritabanları için en iyi duruma getirilmiş fiyat-performans ' dir. Buna karşılık, sağlanan işlem katmanı, tek veritabanları veya birden çok işlem Isınma kurtarmadaki gecikmeyi Aboneliklerde daha yüksek bir ortalama kullanım ile elastik havuzlardaki veritabanları için en iyi duruma getirilmiş fiyat-performans gösterir.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Sunucusuz bilgi işlem için uygun olan senaryoları

- Aralıklı, öngörülemeyen kullanım düzenlerine sahip tek veritabanları, etkin olmama zaman içinde daha düşük ortalama işlem kullanımı ve nokta ile interspersed.
- Sık boyutlandırılan sağlanan işlem katmanı ve temsilci atamak tercih eden müşteriler tek veritabanları için hizmet ölçeklendirme işlem.
- Yeni tek veritabanları işlem boyutlandırma zor veya SQL veritabanı'nda dağıtımından önce tahmin etmek mümkün olduğu Kullanım Geçmişi olmadan.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Sağlanan işlem için uygun olan senaryoları

- Daha fazla normal, öngörülebilir kullanım düzenlerini ve daha yüksek bir ortalama ile tek veritabanları, kullanımın zaman işlem.
- Hakkında daha fazla kaynaklanan performans dengelemeler genişliğinin kullanılmasını veritabanları kırpma bellek sık veya duraklatılmış duruma gelen autoresuming içinde gecikme.
- Elastik havuzlar için daha iyi fiyat-performans iyileştirme içine birleştirilebilir aralıklı, öngörülemeyen kullanım düzenlerine sahip birden çok veritabanı.

## <a name="comparison-with-provisioned-compute-tier"></a>Sağlanan işlem katmanı ile karşılaştırma

Aşağıdaki tabloda, sunucusuz bilgi işlem katmanı ve sağlanan işlem katmanı arasındaki farklılıklar özetlenmiştir:

| | **Sunucusuz bilgi işlem** | **Sağlanan işlem** |
|:---|:---|:---|
|**Veritabanı kullanım düzeninden**| Aralıklı, öngörülemeyen kullanım zaman içinde daha düşük ortalama işlem kullanımına sahip. |  Daha fazla normal kullanım düzenlerini daha yüksek bir ortalama ile kullanım süresi veya esnek havuzları kullanan birden çok veritabanı üzerinde işlem.|
| **Performans yönetim çabası** |Daha düşük|Daha yüksek|
|**Ölçeklendirme işlem**|Otomatik|Manual|
|**İşlem yanıt hızı**|Etkin nokta sonra daha düşük|Hemen|
|**Faturalandırma ayrıntı düzeyi**|Saniye başına|Saatlik|

## <a name="purchasing-model-and-service-tier"></a>Model ve hizmet katmanı satın alma

Sunucusuz SQL veritabanı şu anda yalnızca genel amaçlı katmanında satın alma modeli sanal çekirdek, 5. nesil donanımlarda desteklenir.

## <a name="autoscaling"></a>Otomatik ölçeklendirme

### <a name="scaling-responsiveness"></a>Yanıt verme hızını ölçeklendirme

Genel olarak, sunucusuz veritabanları için herhangi bir işlem tarafından en yüksek Vcore değeri şuna ayarlı sınırları içinde istenen miktarda kesintisiz resource talebi karşılamak için yeterli kapasiteye sahip bir makinede çalıştırılır. Bazen, makineyi birkaç dakika içinde kaynak talebi karşılamak kuramazsa Yük Dengeleme otomatik olarak gerçekleşir. Örneğin, 4 sanal çekirdek kaynak taleptir yoktur, ancak yalnızca 2 sanal çekirdek, daha sonra birkaç dakika 4 sanal çekirdek sağlanan önce yük dengelemek sürebilir. Veritabanı bağlantıları bırakıldığında dışında kısa bir süre sonunda işlemi, yük dengelemeyi sırasında çevrimiçi kalır.

### <a name="memory-management"></a>Bellek yönetimi

Sunucusuz veritabanları için bellek iadesi birden çok sık sağlanan işlem veritabanları için. Bu davranış, sunucusuz, maliyetleri denetlemek için önemlidir ve performansı etkileyebilir.

#### <a name="cache-reclamation"></a>Önbellek geri kazanma

CPU veya önbellek kullanım düşük olduğunda sağlanan işlem veritabanlarının aksine, SQL veritabanından bellek sunucusuz bir veritabanından geri kazanılır.

- Toplam boyutu en kısa süre önce bir süre için önbellek girişlerini eşiğin bir kullanılan önbellek kullanımı düşük olarak kabul edilir.
- Önbellek geri kazanma tetiklendiğinde hedef önbellek boyutu, bir önceki boyutuna kesir olarak artımlı olarak azaltılır ve tekrar kullanılabilir hale getirme yalnızca kullanım düşükse, devam eder.
- Önbellek geri kazanma ortaya çıktığında, yüksek bellek baskısı olduğunda çıkarmak için önbellek girişlerinin seçmek için sağlanan işlem veritabanları için aynı seçim ilke ilkedir.
- Önbellek boyutu, hiçbir zaman yapılandırılabilecek en düşük Vcore tarafından tanımlanan en düşük bellek sınırı aşağıdaki azaltılır.

Hem sunucusuz ve sağlanan veritabanları, tüm kullanılabilir bellek kullanıldığında girişleri çıkarılmış önbellek işlem.

#### <a name="cache-hydration"></a>Önbellek hidrasyonu

Verileri diskten aynı şekilde ile sağlanan veritabanları için aynı hızda getirildiğini gibi SQL önbellek büyür. Veritabanı meşgul olduğunda önbellek sınırlandırılmamış Maks bellek sınırı kadar büyümesine izin verilmez.

## <a name="autopausing-and-autoresuming"></a>Autopausing ve autoresuming

### <a name="autopausing"></a>Autopausing

Autopausing autopause gecikme süresi için aşağıdaki koşulların tümü doğru olması durumunda tetiklenir:

- Sayı oturumlar = 0
- CPU kullanıcı havuzunda çalışan kullanıcı iş yükü için 0 =

Bir seçenek autopausing isterseniz devre dışı bırakmak için sağlanır.

Aşağıdaki özellikler autopausing desteklemez.  Aşağıdaki özelliklerden herhangi birini kullanılıyorsa, diğer bir deyişle, sonra veritabanının veritabanı etkin olmama süresini bağımsız olarak çevrimiçi kalır:

- Coğrafi çoğaltma (etkin coğrafi çoğaltma ve otomatik yük devretme grupları).
- Uzun süreli yedek saklama (LTR).
- SQL veri eşitlemede kullanılan eşitleme veritabanı.

Autopausing veritabanının çevrimiçi olması gereken bazı hizmet güncelleştirmeleri dağıtımı sırasında geçici olarak engellenir.  Hizmet güncelleştirmesi tamamlandıktan sonra bu gibi durumlarda autopausing yeniden izin olur.

### <a name="autoresuming"></a>Autoresuming

Aşağıdaki koşullardan herhangi biri herhangi bir zamanda doğruysa Autoresuming tetiklenir:

|Özellik|Autoresume tetikleyicisi|
|---|---|
|Kimlik doğrulama ve yetkilendirme|Oturum Aç|
|Tehdit algılama|Veritabanı veya sunucu düzeyinde tehdit algılama ayarları etkinleştirme/devre dışı bırakma.<br>Veritabanı veya sunucu düzeyinde tehdit algılama ayarları değiştirme.|
|Veri bulma ve sınıflandırma|Ekleme, değiştirme, silme veya duyarlılık etiketleri görüntüleme|
|Denetim|Denetim kayıtlarını görüntüleme.<br>Güncelleştirme veya denetim ilkesini görüntüleme.|
|Veri maskeleme|Ekleme, değiştirme, silme veya veri maskeleme kuralları görüntüleme|
|Saydam veri şifrelemesi|Görünüm durumu veya saydam veri şifrelemesi durumu|
|Veri deposu sorgu (performans)|Sorgu deposu ayarları görüntüleme veya değiştirdiğiniz; otomatik ayarlama|
|Bırakabilirsiniz|Uygulama ve bırakabilirsiniz önerileri otomatik dizin oluşturma gibi doğrulama|
|Veritabanı kopyalama|Veritabanı kopya olarak oluşturun.<br>Bir BACPAC dosyasına dışarı aktarın.|
|SQL data Sync'i|Yapılandırılabilir bir zamanlamaya göre çalıştırın ya da el ile gerçekleştirilen hub ve üye veritabanı arasında eşitleme|
|Belirli bir veritabanı meta verileri değiştirme|Yeni veritabanı etiketler ekleme.<br>En çok sanal çekirdek, en düşük Vcore veya autopause gecikme değiştiriliyor.|
|SQL Server Management Studio (SSMS)|Sürüm 18 SSMS kullanarak ve herhangi bir veritabanı için yeni bir sorgu penceresi açıp sunucuda aynı sunucuya otomatik olarak duraklatıldı veritabanında devam edecek. IntelliSense ile SSMS sürümü 17.9.1 kullanarak devre dışı ise bu davranışı gerçekleşmez.|

Autoresuming ayrıca veritabanının çevrimiçi olması gereken bazı hizmet güncelleştirmeleri dağıtımı sırasında tetiklenir.

### <a name="connectivity"></a>Bağlantı

Sunucusuz veritabanı duraklatıldı durumunda ilk oturum açma veritabanını sürdürmeniz ve veritabanı hata kodu 40613 kullanılamaz olduğunu bildiren bir hata döndürür. Veritabanı sürdürülüyor sonra oturum açma bağlantısı kurmak için yeniden denenmelidir. Veritabanı bağlantısı yeniden deneme mantığı istemcilerle değiştirilmesi gerekmez.

### <a name="latency"></a>Gecikme süresi

Autoresume ve sunucusuz veritabanı autopause gecikme süresini genellikle 1 dakikalık autoresume ve 1-10 dakika için autopause sırasıdır.

## <a name="onboarding-into-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanı içine ekleme

Yeni bir veritabanı oluşturmak veya bir sunucusuz bilgi işlem katmanı varolan bir veritabanını yeni bir veritabanı oluşturma olarak aynı deseni izler taşıma bilgi işlem katmanı sağlanabilir ve aşağıdaki iki adımdan oluşur.

1. Hizmet hedef adı belirtin. Hizmet hedefi bir hizmet katmanı, donanım oluşturma ve en yüksek Vcore önerir. Aşağıdaki tabloda hizmet hedefi seçenekleri gösterir:

   |Hizmet hedef adı|Hizmet katmanı|Donanım oluşturma|En yüksek sanal çekirdekler|
   |---|---|---|---|
   |GP_S_Gen5_1|Genel Amaçlı|Gen5|1|
   |GP_S_Gen5_2|Genel Amaçlı|Gen5|2|
   |GP_S_Gen5_4|Genel Amaçlı|Gen5|4|

2. İsteğe bağlı olarak, varsayılan değerleri değiştirmek için en düşük Vcore ve autopause gecikme belirtin. Aşağıdaki tabloda kullanılabilir değerleri bu parametrelerin gösterilmektedir.

   |Parametre|Değer seçenekleri|Varsayılan değer|
   |---|---|---|---|
   |En düşük Vcore|Herhangi {0,5, 1, 2, 4}, en çok sanal çekirdek değerini aşmayan|0,5 sanal çekirdek|
   |Autopause gecikmesi|En az: 360 dakika (6 saat)<br>En fazla: süre 10080 dakikadır (7 gün)<br>Artış: 60 dakika<br>Autopause devre dışı bırak: -1|360 dakika|

> [!NOTE]
> Mevcut bir veritabanı içine sunucusuz taşımak veya işlem boyutunu değiştirmek için T-SQL kullanarak şu anda desteklenmiyor ancak Azure portal veya PowerShell yapılabilir.

### <a name="create-new-database-in-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanında yeni veritabanı oluşturun 

#### <a name="use-azure-portal"></a>Azure portalı kullanma

Bkz: [hızlı başlangıç: Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma](sql-database-single-database-get-started.md).

#### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki örnek, sunucusuz bilgi işlem katmanında yeni bir veritabanı oluşturur.  Bu örnekte, en düşük Vcore, en çok sanal çekirdek ve autopause gecikme açıkça belirtir.

```powershell
New-AzSqlDatabase `
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -ComputeModel Serverless `
  -Edition GeneralPurpose `
  -ComputeGeneration Gen5 `
  -MinVcore 0.5 `
  -MaxVcore 2 `
  -AutoPauseDelayInMinutes 720
```

### <a name="move-database-from-provisioned-compute-tier-into-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanı içinde sağlanan işlem katmanından veritabanı taşıma

#### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki örnek bir veritabanı sunucusuz bilgi işlem katmanı sağlanan işlem katmanından taşır. Bu örnekte, en düşük Vcore, en çok sanal çekirdek ve autopause gecikme açıkça belirtir.

```powershell
Set-AzSqlDatabase
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -Edition GeneralPurpose `
  -ComputeModel Serverless `
  -ComputeGeneration Gen5 `
  -MinVcore 1 `
  -MaxVcore 4 `
  -AutoPauseDelayInMinutes 1440
```

### <a name="move-database-from-serverless-compute-tier-into-provisioned-compute-tier"></a>Sunucusuz bilgi işlem katmanı içinde sağlanan işlem katmanı veritabanından taşıma

Sunucusuz veritabanı sağlanan işlem katmanına, sağlanan işlem veritabanı bir sunucusuz bilgi işlem katmanı olarak aynı şekilde taşınabilir.

## <a name="modifying-serverless-configuration"></a>Sunucusuz yapılandırmasını değiştirme

### <a name="maximum-vcores"></a>En fazla vCore

#### <a name="use-powershell"></a>PowerShell kullanma

En yüksek sanal çekirdekler değiştirme işlemi gerçekleştirildiğinde kullanarak [kümesi AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell kullanarak komut `MaxVcore` bağımsız değişken.

### <a name="minimum-vcores"></a>En Az vCore

#### <a name="use-powershell"></a>PowerShell kullanma

En düşük Vcore değiştirme işlemi gerçekleştirildiğinde kullanarak [kümesi AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell kullanarak komut `MinVcore` bağımsız değişken.

### <a name="autopause-delay"></a>Autopause gecikmesi

#### <a name="use-powershell"></a>PowerShell kullanma

Autopause gecikmesini değiştirme işlemi gerçekleştirildiğinde kullanarak [kümesi AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell kullanarak komut `AutoPauseDelayInMinutes` bağımsız değişken.

## <a name="monitoring"></a>İzleme

### <a name="resources-used-and-billed"></a>Faturalandırılır ve kullanılan kaynaklar

Sunucusuz veritabanı kaynakları uygulama paketi, SQL örneği ve kullanıcı kaynak havuzuna varlıkları kapsüllenir.

#### <a name="app-package"></a>Uygulama paketi

Uygulama, veritabanı sunucusuz veya sağlanan işlem katmanında olmasına bakılmaksızın, bir veritabanı için dış çoğu kaynak yönetimi sınır paketidir. Uygulama paketi SQL örneği içerir ve harici bir veritabanında bir SQL veritabanı tarafından kullanılan tüm kullanıcı ve sistem kaynakları birlikte, kapsamı Hizmetleri. Dış hizmetlere örnek olarak, R ve tam metin araması içerir. SQL örneği, genellikle uygulama paketi arasında genel kaynak kullanımı kaplamaktadır.

#### <a name="user-resource-pool"></a>Kullanıcı kaynak havuzu

Kullanıcı kaynak havuzuna iç veritabanı sunucusuz veya sağlanan işlem katmanında olmasına bakılmaksızın, bir veritabanı için çoğu kaynak yönetimi sınırı olan. Kullanıcı kaynak havuzuna kapsamlar CPU ve seçin, ekleme, güncelleştirme ve silme GÇ gibi DDL sorguları oluşturma ve değiştirme ve DML sorguları gibi tarafından oluşturulan kullanıcı iş yükü için. Bu sorguları genellikle uygulama paketi içinde kullanımı en önemli oranını temsil eder.

### <a name="metrics"></a>Ölçümler

|Varlık|Ölçüm|Açıklama|Birimler|
|---|---|---|---|
|Uygulama paketi|app_cpu_percent|Uygulama için izin verilen en yüksek Vcore göre uygulama tarafından kullanılan çekirdek yüzdesi.|Yüzde|
|Uygulama paketi|app_cpu_billed|İşlem için uygulama raporlama döneminde faturalandırılır miktarı. Bu süre boyunca ödeme tutarı, bu ölçüm ve sanal çekirdek birim fiyatı ürünüdür. <br><br>Bu ölçüm değerleri en fazla CPU kullanılan zaman içinde toplayarak belirlenir ve saniyede bellek kullanılır. Kullanılan tutar en az bellek ve en düşük Vcore kümesi olarak sağlanan en düşük miktar altındaysa, sağlanan en düşük miktar ücreti alınır. CPU ile bellek faturalandırma için karşılaştırmak için bellek, sanal çekirdekler birimlerine bellek miktarı GB olarak 3 GB sanal çekirdek ölçeklendirme tarafından normalleştirilmiştir.|Sanal çekirdek saniye|
|Uygulama paketi|app_memory_percent|Uygulama için izin verilen en fazla bellek göre uygulama tarafından kullanılan bellek yüzdesi.|Yüzde|
|Kullanıcı havuzu|cpu_percent|Kullanıcı iş yükü için izin verilen en yüksek Vcore göre kullanıcı iş yükü tarafından kullanılan çekirdek yüzdesi.|Yüzde|
|Kullanıcı havuzu|data_IO_percent|Kullanıcı iş yükü için veri maks. IOPS veri göre kullanıcı iş yükü tarafından kullanılan IOPS yüzdesi izin.|Yüzde|
|Kullanıcı havuzu|log_IO_percent|Günlük yüzdesi, kullanıcı iş yükü için en büyük günlük MB/sn göre kullanıcı iş yükü tarafından kullanılan MB/sn izin.|Yüzde|
|Kullanıcı havuzu|workers_percent|Kullanıcı iş yüküne göre kullanıcı iş yükü için izin verilen en fazla çalışan tarafından kullanılan çalışan yüzdesi.|Yüzde|
|Kullanıcı havuzu|sessions_percent|Kullanıcı iş yükü için izin verilen maksimum oturum göre kullanıcı iş yükü tarafından kullanılan oturumları yüzdesi.|Yüzde|
____

> [!NOTE]
> Azure portalında ölçümleri altında tek bir veritabanı için veritabanı bölmesinde kullanılabilir **izleme**.

### <a name="pause-and-resume-status"></a>Duraklatma ve sürdürme durumu

Azure portalında veritabanı durumu içerdiği veritabanlarını listeler sunucunun genel bakış bölmesinde görüntülenir. Veritabanı durumu de veritabanı için genel bakış bölmesinde görüntülenir.

Duraklatma sorgulamak ve bir veritabanı durumunu sürdürmek için aşağıdaki PowerShell komutunu kullanarak:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Kaynak sınırları

Kaynak limitleri için bkz. [sunucusuz bilgi işlem katmanı](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier)

## <a name="billing"></a>Faturalandırma

Faturalandırılan işlem en fazla kullanılan CPU ve saniyede kullanılan bellek miktarıdır. Kullanılan CPU miktarını ve her biri için sağlanan en düşük miktar daha az kullanılan bellek miktarı sağlanan faturalandırılır. CPU ile bellek faturalandırma için karşılaştırmak için bellek, sanal çekirdekler birimlerine bellek miktarı GB olarak 3 GB sanal çekirdek ölçeklendirme tarafından normalleştirilmiştir.

- **Kaynak faturalandırılır**: CPU ve bellek
- **Faturalandırılan miktar**: sanal çekirdek birim fiyatı * max (en düşük Vcore, kullanılan sanal çekirdek, en düşük bellek GB * 1/3 bellek kullanılan GB * 1/3) 
- **Faturalama sıklığı**: Saniye başına

Saniye başına sanal çekirdek başına maliyet sanal çekirdek birim fiyatı. Başvurmak [Azure SQL veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/single/) belirli birimi fiyatlar belirli bir bölge için.

Faturalandırılan işlem miktarını tarafından aşağıdaki ölçüm sunulur:

- **Ölçüm**: app_cpu_billed (sanal çekirdek saniye)
- **Tanımı**: en fazla (en düşük Vcore, kullanılan sanal çekirdek, en düşük bellek GB * 1/3 bellek kullanılan GB * 1/3)
- **Raporlama sıklığını**: Dakika başına

Bu miktar saniyede hesaplanır ve 1 dakika içinde toplanır.

1 dakika sanal çekirdek ve en fazla 4 sanal çekirdek ile yapılandırılmış bir sunucusuz veritabanı göz önünde bulundurun.  Bu, yaklaşık 3 GB en düşük bellek ve en fazla 12 GB bellek karşılık gelir.  Otomatik duraklatma gecikme 6 saat olarak ayarlanır ve veritabanı iş yükünüzü 24 saatlik bir dönemde ilk 2 saat boyunca etkin ve etkin olmayan Aksi takdirde varsayalım.    

Bu durumda, veritabanı işlem ve depolama için sırasında ilk 8 saat olarak faturalandırılır.  Veritabanı devre dışı sonra ikinci saatten itibaren olsa da, veritabanı çevrimiçi durumdayken sağlanan en düşük işlem üzerinde göre sonraki 6 saat içindeki işlem yine de faturalandırılır.  Veritabanı duraklatıldığında yalnızca depolama 24 saatlik dönemde geri kalanı faturalandırılır.

Bu örnekteki işlem faturada daha kesin bir şekilde aşağıdaki gibi hesaplanır:

|Zaman aralığı|saniyede kullanılan sanal çekirdekler|Saniyede kullanılan GB|İşlem boyutu faturalandırılır|Sanal çekirdek saniye süre faturalandırılır|
|---|---|---|---|---|
|0:00-1:00|4|9|kullanılan sanal çekirdekler|4 sanal çekirdek * 3600 saniye = 14400 sanal çekirdek saniye|
|1:00-2:00|1|12|Kullanılan bellek|12 GB * 1/3 * 14400 sanal çekirdek saniye = 3600 saniye|
|2:00-8:00|0|0|Sağlanan en düşük bellek|3 GB * 1/3 * 21600 saniye = 21600 sanal çekirdek saniye|
|8:00-24:00|0|0|Duraklatılmış faturalandırılırken işlem yok|0 sanal çekirdek saniye|
|Toplam sanal çekirdek saniye içinde 24 saat olarak faturalandırılır||||50400 sanal çekirdek saniye|

İşlem birim fiyatı $0.000073/vCore/second olduğunu varsayın.  Sonra bu 24 saatlik dönem için fatura işlem ürün işlem birim fiyatı ve sanal çekirdek saniyelik olarak faturalandırılır: $0.000073/vCore/second * $3.68 50400 sanal çekirdek saniye =

## <a name="available-regions"></a>Kullanılabilen bölgeler

Sunucusuz bilgi işlem katmanı, tüm dünyada dışında aşağıdaki bölgelerde kullanılabilir: Avustralya Orta, Çin Doğu, Kuzey Çin, Fransa Güney, Almanya Orta, Almanya Kuzeydoğu, Hindistan Batı, Kore Güney, Güney Afrika Batı, UK Kuzey, UK Güney, UK Batı ve Orta Batı ABD.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [hızlı başlangıç: Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma](sql-database-single-database-get-started.md).
- Kaynak limitleri için bkz. [sunucusuz bilgi işlem katmanı kaynak sınırları](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).
