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
ms.date: 05/07/2019
ms.openlocfilehash: 7f850f309034d128efef89ea842db41d35b8491e
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235749"
---
# <a name="sql-database-serverless-preview"></a>SQL veritabanı sunucusuz (Önizleme)

## <a name="what-is-the-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanı nedir

Sunucusuz SQL veritabanı (Önizleme) saniye başına temelinde tek bir veritabanı tarafından kullanılan süre boyunca faturalar hesaplayan bir işlem katmandır. Sunucusuz işlem Isınma bazı gecikme boşta kullanım dönemlerini gücünüze yükselen kullanım düzenlerini ile tek veritabanları için en iyi duruma getirilmiş fiyat-performans ' dir.
Buna karşılık, genel kullanıma sunar, SQL veritabanı süre boyunca bugün faturada saatlik olarak sağlanan işlem. Bu sağlanan işlem katmanı fiyat-performans tek veritabanı veya elastik için iyileştirilmiş olduğundan işlem Isınma kurtarmadaki gecikmeyi Aboneliklerde daha yüksek bir ortalama kullanım havuzlarıyla.

Sunucusuz bilgisayar katmanındaki bir veritabanı, işlem aralığın kullanabilmesi ve autopause gecikme tarafından parametreli olup.

![sunucusuz faturalandırma](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance"></a>Performans

- `MinVcore` ve `MaxVcore` veritabanı için kullanılabilir işlem kapasitesi aralığını tanımlamak yapılandırılabilir parametrelerdir. Bellek ve GÇ, sınırları, belirtilen sanal çekirdek aralığı orantılıdır.  
- Autopause gecikmeyi otomatik olarak duraklatılmadan önce veritabanı etkin olmaması gereken süre tanımlayan yapılandırılabilir bir parametredir. Sonraki oturum oluştuğunda veritabanı otomatik olarak sürdürülür.

### <a name="pricing"></a>Fiyatlandırma

- Toplam fatura sunucusuz veritabanı için depolama faturası ve işlem fatura toplamı olur.
İşlem için kullanılan sanal çekirdek ve saniye başına kullanılan bellek miktarı göre faturalandırılır.
- Faturalandırılan en düşük bilgi işlem, en düşük Vcore ve en az bellek dayanır.
- Veritabanı duraklatıldığında yalnızca depolama ücreti alınır.

## <a name="scenarios"></a>Senaryolar

Sunucusuz işlem Isınma bazı gecikme boşta kullanım dönemlerini gücünüze yükselen kullanım düzenlerini ile tek veritabanları için en iyi duruma getirilmiş fiyat-performans ' dir. Sağlanan işlem, fiyat-performans işlem Isınma kurtarmadaki gecikmeyi Aboneliklerde daha yüksek bir ortalama kullanım ile tek veya havuza alınmış veritabanları için en iyi duruma getirilmiş katmandır.

Aşağıdaki tabloda sağlanan işlem katmanı ile sunucusuz bilgi işlem katmanı karşılaştırılır:

||Sunucusuz işlem|Sağlanan işlem|
|---|---|---|
|**Tipik kullanım senaryosu**|Etkin nokta ile interspersed yükselen, tahmin edilemeyen kullanım veritabanlarıyla|Veritabanları ya da daha fazla normal kullanım ile elastik havuzları|
|**Performans yönetim çabası**|Daha düşük|Daha yüksek|
|**Ölçeklendirme işlem**|Otomatik|El ile|
|**İşlem yanıt hızı**|Etkin nokta sonra daha düşük|Hemen|
|**Faturalandırma ayrıntı düzeyi**|Saniye başına|Saatlik|
|

### <a name="scenarios-well-suited-for-serverless-compute"></a>Sunucusuz bilgi işlem için uygun olan senaryoları

- Tek veritabanlarını etkin olmadığı dönemler ile interspersed yükselen kullanım düzenlerini kullanılan işlem miktarı saniye başına faturalandırma göre fiyat tasarrufu yararlanabilir.
- Kaynak talebi tahmin etmek zordur ve işlem boyutlandırma hizmetine temsilci atamak tercih eden müşteriler ile tek veritabanları.
- Sağlanan işlem katmanında tek veritabanları, performans düzeyleri sık değiştirin.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Sağlanan işlem için uygun olan senaryoları

- Daha fazla normal ve daha fazla önemli ile tek veritabanları, kullanımın zaman işlem.
- Hakkında daha fazla kaynaklanan performans dengelemeler genişliğinin kullanılmasını veritabanları kırpma bellek sık veya duraklatılmış duruma gelen autoresuming içinde gecikme.
- Tek bir sunucu birleştirilebilir ve elastik havuzlar için daha iyi fiyat iyileştirme kullanan yükselen kullanım düzenlerine sahip birden çok veritabanı.


## <a name="purchasing-model-and-service-tier"></a>Model ve hizmet katmanı satın alma

Sunucusuz SQL veritabanı şu anda yalnızca genel amaçlı katmanında satın alma modeli sanal çekirdek, 5. nesil donanımlarda desteklenir.

## <a name="autoscaling"></a>Otomatik ölçeklendirme

### <a name="scaling-responsiveness"></a>Yanıt verme hızını ölçeklendirme

Veritabanları için herhangi bir işlem belirlenen sınırlar içinde istenen miktarda kesintisiz resource talebi karşılamak için yeterli kapasiteye sahip bir makinede genel olarak, çalıştırılan `maxVcores` değeri. Bazen, makineyi birkaç dakika içinde kaynak talebi karşılamak kuramazsa Yük Dengeleme otomatik olarak gerçekleşir. Veritabanı bağlantıları bırakıldığında dışında kısa bir süre sonunda işlemi, yük dengelemeyi sırasında çevrimiçi kalır.

### <a name="memory-management"></a>Bellek yönetimi

Sunucusuz veritabanları için bellek iadesi birden çok sık sağlanan veritabanları için. Bu davranış, sunucusuz, maliyetleri denetlemek için önemlidir. CPU veya önbellek kullanım düşük olduğunda sağlanan işlem farklı olarak, SQL veritabanından bellek sunucusuz bir veritabanından geri kazanılır.

## <a name="autopause-and-autoresume"></a>Autopause ve autoresume

### <a name="autopause"></a>Autopause

Autopause autopause gecikme süresi için aşağıdaki koşulların tümü doğru olması durumunda tetiklenir:

- Sayı oturumlar = 0
- CPU = 0 (için kullanıcı iş yükü kullanıcı havuzunda çalışan)

Autopause isterseniz devre dışı bırakmak için bir seçenek sağlanır.

### <a name="autoresume"></a>Autoresume

Aşağıdaki koşullardan herhangi biri herhangi bir zamanda doğruysa Autoresume tetiklenir:

|Özellik|Autoresume tetikleyicisi|
|---|---|
|Kimlik doğrulama ve yetkilendirme|Oturum Aç|
|Tehdit algılama|Tehdit algılama ayarları veritabanı veya sunucu düzeyinde etkinleştirme/devre dışı bırakma<br>Veritabanı veya sunucu düzeyinde tehdit algılama ayarları değiştirme|
|Veri bulma ve sınıflandırma|Ekleme, değiştirme, silme veya duyarlılık etiketleri görüntüleme|
|Denetim|Denetim kayıtlarını görüntüleme.<br>Denetim ilkesini görüntüleme veya güncelleştirme|
|Veri maskeleme|Ekleme, değiştirme, silme veya veri maskeleme kuralları görüntüleme|
|Saydam veri şifrelemesi|Görünüm durumu veya saydam veri şifrelemesi durumu|
|Veri deposu sorgu (performans)|Sorgu deposu ayarları görüntüleme veya değiştirdiğiniz; otomatik ayarlama|
|Bırakabilirsiniz|Uygulama ve bırakabilirsiniz önerileri otomatik dizin oluşturma gibi doğrulama|
|Veritabanı kopyalama|Kopya olarak veritabanı oluşturma<br>Bir BACPAC dosyasına aktarma|
|SQL data Sync'i|Yapılandırılabilir bir zamanlamaya göre çalıştırın ya da el ile gerçekleştirilen hub ve üye veritabanı arasında eşitleme|
|Belirli bir veritabanı meta verileri değiştirme|Yeni veritabanı etiketler ekleme<br>En yüksek Vcore, en düşük Vcore autopause gecikme değiştirme|
|SQL Server Management Studio (SSMS)|Sürüm 18 SSMS kullanarak ve herhangi bir veritabanı için yeni bir sorgu penceresi açıp sunucuda aynı sunucuya otomatik olarak duraklatıldı veritabanında devam edecek. IntelliSense ile SSMS sürümü 17.9.1 kullanarak devre dışı ise bu davranışı gerçekleşmez.|

### <a name="connectivity"></a>Bağlantı

Sunucusuz bir veritabanları duraklatıldı durumunda ilk oturum açma veritabanını sürdürmeniz ve veritabanı kullanılamaz olduğunu bildiren bir hata döndürür. Veritabanı sürdürülüyor sonra oturum açma bağlantısı kurmak için yeniden denenmelidir. Veritabanı bağlantısı yeniden deneme mantığı istemcilerle değiştirilmesi gerekmez.

### <a name="latency"></a>Gecikme

Autopause ya da sunucusuz veritabanı autoresume gecikme süresi, genellikle 1 dakika bazında ' dir.

### <a name="feature-support"></a>Özellik desteği

Aşağıdaki özellikler autopausing ve autoresuming desteklemez. Aşağıdaki özelliklerden herhangi birini kullanılıyorsa, diğer bir deyişle, sonra veritabanının veritabanı etkin olmama süresini bağımsız olarak çevrimiçi kalır:

- Coğrafi çoğaltma (etkin coğrafi çoğaltma ve otomatik yük devretme grupları)
- Uzun süreli yedek saklama (LTR)
- SQL veri eşitlemede kullanılan eşitleme veritabanı.


## <a name="on-boarding-into-the-serverless-compute-tier"></a>Kolaylaşmasına sunucusuz bilgi işlem katmanı içine

Yeni bir veritabanı oluşturmak veya bir sunucusuz bilgi işlem katmanı varolan bir veritabanını yeni bir veritabanı oluşturma olarak aynı deseni izler taşıma bilgi işlem katmanı sağlanan ve aşağıdaki iki adımdan oluşur:

1. Hizmet hedef adı belirtin. Aşağıdaki tabloda, şu anda genel Önizleme sürümünde kullanılabilir bilgi işlem boyutlarına ve kullanılabilir bir hizmet katmanı gösterir.

   |Hizmet katmanı|İşlem boyutu|
   |---|---|
   |Genel Amaçlı|GP_S_Gen5_1|
   |Genel Amaçlı|GP_S_Gen5_2|
   |Genel Amaçlı|GP_S_Gen5_4|

2. İsteğe bağlı olarak, varsayılan değerleri değiştirmek için en düşük Vcore ve autopause gecikme belirtin. Aşağıdaki tabloda kullanılabilir değerleri bu parametrelerin gösterilmektedir.

   |Parametre|Değer seçenekleri|Varsayılan değer|
   |---|---|---|---|
   |En Az vCore|Herhangi {0,5, 1, 2, 4}, en çok sanal çekirdek değerini aşmayan|0,5 sanal çekirdek|
   |Autopause gecikmesi|En küçük: 360 dakika (6 saat)<br>En fazla: süre 10080 dakikadır (7 gün)<br>Artış: 60 dakika<br>Autopause devre dışı bırak: -1|360 dakika|

> [!NOTE]
> Mevcut bir veritabanı içine sunucusuz taşımak veya işlem boyutunu değiştirmek için T-SQL kullanarak şu anda desteklenmiyor ancak Azure portal veya PowerShell yapılabilir.

### <a name="create-new-database-using-the-azure-portal"></a>Azure portalını kullanarak yeni veritabanı oluştur

Bkz: [hızlı başlangıç: Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma](sql-database-single-database-get-started.md).

### <a name="create-new-database-using-powershell"></a>PowerShell kullanarak yeni veritabanı oluşturma

Aşağıdaki örnek GP_S_Gen5_4 en düşük Vcore ve autopause gecikme için varsayılan değerlerle adlı hizmet hedefi tarafından tanımlanan sunucusuz bilgi işlem katmanı yeni bir veritabanı oluşturur.

Sunucusuz PowerShell şu anda galeride böylece çalıştırma olandan daha yeni bir sürümü gerektirir `Update-Module Az.Sql` en son etkin sunucusuz cmdlet'lerini alma.

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
  -AutoPauseDelay 720
```

### <a name="move-existing-database-into-the-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanı var olan veritabanına taşıma

Aşağıdaki örnekte mevcut bir tek veritabanı sağlanan işlem katmanından sunucusuz bilgi işlem Katmanı taşır. Bu örnekte, en düşük Vcore, en çok sanal çekirdek ve autopause gecikme için varsayılan değerleri kullanır.

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
  -AutoPauseDelay 1440
```

### <a name="move-a-database-out-of-the-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanı dışında bir veritabanını taşıma

Sunucusuz veritabanı sağlanan işlem katmanına, sağlanan işlem veritabanı bir sunucusuz bilgi işlem katmanı olarak aynı şekilde taşınabilir.

## <a name="modify-serverless-configuration-parameters"></a>Sunucusuz yapılandırma parametrelerini değiştirme

### <a name="maximum-vcores"></a>En fazla vCore

En yüksek sanal çekirdekler değiştirme işlemi gerçekleştirildiğinde kullanarak [kümesi AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell kullanarak komut `MaxVcore` bağımsız değişken.

### <a name="minimum-vcores"></a>En Az vCore

En yüksek sanal çekirdekler değiştirme işlemi gerçekleştirildiğinde kullanarak [kümesi AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell kullanarak komut `MinVcore` bağımsız değişken.

### <a name="autopause-delay"></a>Autopause gecikmesi

En yüksek sanal çekirdekler değiştirme işlemi gerçekleştirildiğinde kullanarak [kümesi AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell kullanarak komut `AutoPauseDelay` bağımsız değişken.

## <a name="monitor-serverless-database"></a>Sunucusuz veritabanı İzleyicisi

### <a name="resources-used-and-billed"></a>Faturalandırılır ve kullanılan kaynaklar

Aşağıdaki varlıklar tarafından sunucusuz veritabanı kaynaklarını kapsüllenir:

#### <a name="app-package"></a>Uygulama paketi

Uygulama, veritabanı sunucusuz veya sağlanan işlem katmanında olmasına bakılmaksızın, bir veritabanı için dış çoğu kaynak yönetimi sınır paketidir. Uygulama paketi SQL örneği içerir ve harici bir veritabanında bir SQL veritabanı tarafından kullanılan tüm kullanıcı ve sistem kaynakları birlikte, kapsamı Hizmetleri. Dış hizmetlere örnek olarak, R ve tam metin araması içerir. SQL örneği, genellikle uygulama paketi arasında genel kaynak kullanımı kaplamaktadır.

#### <a name="user-resource-pool"></a>Kullanıcı kaynak havuzu

Kullanıcı kaynak havuzuna iç veritabanı sunucusuz veya sağlanan işlem katmanında olmasına bakılmaksızın, bir veritabanı için çoğu kaynak yönetimi sınırı olan. Kullanıcı kaynak havuzuna DDL sorgular (örneğin, CREATE, ALTER, vb.) ve DML sorgularında (örneğin, seçme, ekleme, güncelleştirme, silme, vb.) tarafından oluşturulan kullanıcı iş yükü için CPU ve g/ç kapsamları. Bu sorguları genellikle uygulama paketi içinde kullanımı en önemli oranını temsil eder.

### <a name="metrics"></a>Ölçümler

|Varlık|Ölçüm|Açıklama|Birimler|
|---|---|---|---|
|Uygulama paketi|app_cpu_percent|Uygulama için izin verilen en yüksek Vcore göre uygulama tarafından kullanılan çekirdek yüzdesi.|Yüzde|
|Uygulama paketi|app_cpu_billed|İşlem için uygulama raporlama döneminde faturalandırılır miktarı. Bu süre boyunca ödeme tutarı, bu ölçüm ve sanal çekirdek birim fiyatı ürünüdür.<br>Bu ölçüm değerleri en fazla CPU kullanılan zaman içinde toplayarak belirlenir ve saniyede bellek kullanılır.<br>Kullanılan tutar en az bellek ve en düşük Vcore kümesi olarak sağlanan en düşük miktar altındaysa, sağlanan en düşük miktar ücreti alınır.  CPU ile bellek faturalandırma için karşılaştırmak için bellek, sanal çekirdekler birimlerine bellek miktarı GB olarak 3 GB sanal çekirdek ölçeklendirme tarafından normalleştirilmiştir.|Sanal çekirdek saniye|
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

## <a name="billing"></a>Faturalama

Saniyede faturalandırılan işlem en fazla kullanılan CPU ve saniyede kullanılan bellek miktarıdır. Kullanılan CPU miktarını ve her biri için sağlanan en düşük miktar daha az kullanılan bellek miktarı sağlanan faturalandırılır. CPU ile bellek faturalandırma için karşılaştırmak için bellek, sanal çekirdekler birimlerine bellek miktarı GB olarak 3 GB sanal çekirdek ölçeklendirme tarafından normalleştirilmiştir.

- **Kaynak faturalandırılır**: CPU ve bellek
- **Tutarı üzerinden faturalandırılırsınız ($)**: sanal çekirdek birim fiyatı * max (en düşük Vcore, kullanılan sanal çekirdek, en düşük bellek GB * 1/3 bellek kullanılan GB * 1/3) 
- **Faturalama sıklığı**: Saniye başına

Faturalandırılan işlem miktarını tarafından aşağıdaki ölçüm sunulur:

- **Ölçüm**: app_cpu_billed (sanal çekirdek saniye)
- **Tanımı**: en fazla (en düşük Vcore, kullanılan sanal çekirdek, en düşük bellek GB * 1/3 bellek kullanılan GB * 1/3) *
- **Raporlama sıklığını**: Dakika başına

> [!NOTE]
> \* Bu miktar saniyede hesaplanır ve 1 dakika içinde toplanır.

**Örnek**: GP_S_Gen5_4 ile aşağıdaki kullanımı bir saat diliminde kullanarak veritabanı göz önünde bulundurun:

|Süre (saat: dakika)|app_cpu_billed (sanal çekirdek saniye)|
|---|---|
|0:01|63|
|0:02|123|
|0:03|95|
|0:04|54|
|0:05|41|
|0:06 - 1:00|1255|
||Toplam: 1631|

İşlem birim fiyatı $0.2609/vCore/hour olduğunu varsayın. Sonra da bu bir saatlik süre için faturalandırılırsınız işlem aşağıdaki formül kullanılarak belirlenir: **$0.2609/vCore/hour * 1631 sanal çekirdek saniye * 1 saat/3600 saniye $0.1232 =**

## <a name="available-regions"></a>Kullanılabilen bölgeler

Sunucusuz bilgi işlem katmanı şu bölgeler dışındaki tüm bölgelerde kullanılabilir: Avustralya Orta, Çin Doğu, Kuzey Çin, Fransa Güney, Almanya Orta, Almanya Kuzeydoğu, Hindistan Batı, Kore Güney, Güney Afrika Batı, UK Kuzey, UK Güney, UK Batı ve Orta Batı ABD

## <a name="next-steps"></a>Sonraki adımlar

Kaynak limitleri için bkz. [sunucusuz bilgi işlem katmanı kaynak sınırları](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).