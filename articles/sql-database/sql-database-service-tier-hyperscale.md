---
title: Azure SQL veritabanı hiper ölçekli genel bakış | Microsoft Docs
description: Bu makalede Azure SQL veritabanı'ndaki sanal çekirdek tabanlı satın alma modeli hiper ölçekli hizmet katmanında ve genel amaçlı ve iş açısından kritik hizmet katmanı arasından farklı nasıl açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 04/04/2019
ms.openlocfilehash: 5e323b28913e0ba259654d39f97e0436e6bff2db
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786043"
---
# <a name="hyperscale-service-tier-preview-for-up-to-100-tb"></a>En fazla 100 TB için hiper ölçekli hizmet Katmanı (Önizleme)

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için ayarlanmış bir SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan üç Mimari modeli vardır:

- Genel amaçlı/standart 
- Kritik iş/Premium
- Hiper ölçeklendirme

Azure SQL veritabanı'nda hiper ölçekli hizmet katmanı sanal çekirdek tabanlı satın alma modeli en yeni hizmet katmanında ' dir. Bu hizmet katmanında yüksek düzeyde ölçeklenebilir depolama ve Azure mimarisini depolama ve önemli ölçüde kullanılabilir sınırları aşan bir Azure SQL veritabanı için genel amaçlı ve iş için bilgi işlem kaynaklarını yararlanır işlem performans katmanı olan Kritik hizmet katmanları.

> [!IMPORTANT]
> Hiper ölçekli hizmet katmanı genel önizlemeye sunuldu ve şu anda sınırlı Azure bölgelerinde kullanılabilir. Bölge tam listesi için bkz [hiper ölçekli hizmet katmanı kullanılabildiği bölgeler](#available-regions). Tüm üretim iş yüklerini hiper ölçekli veritabanlarında henüz çalıştıran önerilmemektedir. Diğer hizmet katmanları için bir hiper ölçekli veritabanı güncelleştirilemiyor. Test amacıyla geçerli veritabanınızın bir kopyasını alın ve kopyasını hiper ölçekli hizmet katmanına güncelleştirmek öneririz.
> [!NOTE]
> Sanal çekirdek tabanlı satın alma modeli, genel amaçlı ve iş açısından kritik hizmet katmanları hakkında daha fazla bilgi için bkz: [genel amaçlı](sql-database-service-tier-general-purpose.md) ve [iş açısından kritik](sql-database-service-tier-business-critical.md) hizmet katmanları. Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-purchase-models.md).

## <a name="what-are-the-hyperscale-capabilities"></a>Hiper ölçekli özellikleri nelerdir

Azure SQL veritabanı'nda hiper ölçekli Hizmet katmanını, aşağıdaki özellikleri sağlar:

- Veritabanı boyutu en fazla 100 TB için destek
- Neredeyse anında işlem GÇ etkilemeden boyutundan bağımsız olarak veritabanı yedeklemeleri (Azure Blob depolamada depolanan dosya anlık görüntüleri göre)   
- Veritabanını geri yükler (dosya anlık görüntülerine dayalı) dakika yerine saatler veya günler hızlı (değil veri işleminin boyutu)
- Daha fazla günlük performans ve veri birimlerini bağımsız olarak daha hızlı hareket işleme süreleri nedeniyle yüksek genel performansı
- Hızlı ölçeği genişletme - bir veya daha fazla salt okunur düğüm, okuma iş yükü boşaltma için ve kullanımına yönelik sık erişimli-yedek sağlayabilirsiniz
- Hızlı ölçeği artırma -, Sabit sürede işlem kaynaklarınızı gerektiğinde gerekli yoğun iş yüklerine uyum sağlamak üzere ölçeklendirmek ve ardından işlem kaynaklarını değil gerektiğinde ölçeği tekrar.

Hiper ölçekli hizmet katmanı, birçok geleneksel bulut veritabanlarında görülen pratik sınırlarına kaldırır. Tek bir düğüm kullanılabilir kaynaklar tarafından en diğer veritabanları burada sınırlıdır hiper ölçekli hizmet katmanındaki veritabanları gibi bir sınırları vardır. Esnek depolama mimarisinin gerektiğinde depolama büyür. Aslında, Hiper ölçekli veritabanları ile tanımlanan en fazla boyutu oluşturulmaz. Bir hiper ölçekli veritabanı gerektiğinde - büyüdükçe ve yalnızca kullandığınız kapasitesi için faturalandırılırsınız. Okuma açısından yoğun iş yükleri için hiper ölçekli Hizmet katmanını, yük boşaltma okuma iş yükleri için gerektiği gibi ek salt okunur çoğaltmalar sağlayarak hızlı ölçeklendirme sağlar.

Ayrıca, zaman ölçeği büyütün veya veritabanı yedeklemelerini oluşturmak için gerekli veya aşağı artık veritabanında veri hacmine bağlıdır. Hiper ölçekli veritabanları neredeyse anında yedeklenebilir. Ayrıca bir veritabanında terabayt onlarca yukarı veya aşağı dakikalar içinde ölçeklendirebilirsiniz. Bu özellik ilk yapılandırma seçimlerinizi Kutu ilgili endişeleriniz öğesinden kurtarır.

Hiper ölçekli hizmet katmanı için işlem boyutları hakkında daha fazla bilgi için bkz. [hizmet katmanı özellikleri](sql-database-service-tiers-vcore.md#service-tier-characteristics).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Kimin hiper ölçekli Hizmet katmanını göz önünde bulundurmalıyım

Hizmet katmanı, öncelikle büyük veritabanları olan müşteriler için tasarlanmıştır hiper ölçekli ya da şirket içi ve buluta veya bulutta durumda ve tarafından en büyük veritabanı boyutu sınırlı olduğundan müşteriler için taşıyarak uygulamalarını modernleştirin istiyorsanız kısıtlamalar (1-4 TB). Ayrıca, yüksek performanslı ve yüksek ölçeklenebilirlik için depolama arama ve işlem müşteriler için de tasarlanmıştır.

Tüm SQL Server iş yüklerini hiper ölçekli Hizmet katmanını destekler, ancak temelde OLTP için iyileştirilmiştir. Hiper ölçekli hizmet katmanı karma de destekler ve analitik (veri reyonu) iş yükleri.

> [!IMPORTANT]
> Elastik havuzlar, Hiper ölçekli Hizmet katmanını desteklemez.

## <a name="hyperscale-pricing-model"></a>Hiper ölçekli fiyatlandırma modeli

Hiper ölçekli hizmet katmanında bulunan yalnızca [vCore modeli](sql-database-service-tiers-vcore.md). Yeni mimariyi hizalamak için aşağıdaki fiyatlandırma modeli genel amaçlı veya iş açısından kritik hizmet katmanlarına biraz farklıdır:

- **İşlem**:

  Büyük ölçekli işlem birim fiyatı bir çoğaltmadır. [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) ölçek çoğaltmaları otomatik olarak okumak için fiyat uygulanır. Genel Önizleme sürümünde varsayılan olarak iki çoğaltmaları hiper ölçekli veritabanı başına oluştururuz.

- **Depolama**:

  Maks. veri boyutu bir hiper ölçekli veritabanı yapılandırırken belirtmeniz gerekmez. Hiper ölçeklendirme katmanında, veritabanınıza yönelik depolama alanı için gerçek kullanıma göre ücret alınır. Depolama alanı 1 GB artışlarla 5 GB ve 100 TB arasında dinamik şekilde ayrılır.  

Hiper ölçekli fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/single/)

## <a name="distributed-functions-architecture"></a>Dağıtılmış işlevleri mimarisi

Tüm veri yönetim işlevlerinin bir işlemde konum / (hatta bugün üretimde çağrılan dağıtılmış veritabanları tek parçalı veri altyapısı birden çok kopyasını böylece) merkezi geleneksel veritabanı altyapıları, bir hiper ölçekli veritabanı ayırır. Burada çeşitli veri altyapıları semantiği, uzun vadeli depolama ve dayanıklılık için veri sağlayan bileşenlerden ayırmak sorgu işleme altyapısı. Bu şekilde, depolama kapasitesini sorunsuz gerektiği kadar genişletilebilir (ilk hedef değer 100 TB). Yeni bir okunabilir çoğaltma dönmesi için hiçbir veri kopyalama gerekli şekilde salt okunur çoğaltmalar aynı işlem bileşenleri paylaşın. Önizleme sırasında yalnızca 1 salt okunur çoğaltma desteklenir.

Aşağıdaki diyagram, farklı türde bir hiper ölçekli veritabanında düğümleri gösterir:

![architecture](./media/sql-database-hyperscale/hyperscale-architecture.png)

Bir hiper ölçekli veritabanı aşağıdaki farklı tür düğüm içerir:

### <a name="compute-node"></a>İşlem düğümü

İşlem düğümü nerede ilişkisel altyapıda yaşıyor, tüm dil öğeleri, sorgu işleme ve benzeri ortaya olduğundan. Sorun bu pencerelerde hiper ölçekli veritabanına sahip tüm kullanıcı etkileşimlerine işlem düğümleri. Düğüm ağ sayısını en aza indirmek için SSD tabanlı önbellekler (etiketli RBPEX - önceki diyagramdaki dayanıklı arabellek havuzu genişletme) sahip işlem gidiş dönüş bir sayfa veri getirmek için gerekli. Tüm okuma-yazma iş yükleri ve işlemleri işleneceği bir birincil işlem düğümü vardır. (Bu işlevselliği isteniyorsa) iş yükleri okuma boşaltma için salt okunur işlem düğümleri olarak davranmak yanı sıra yük devretme amacıyla sık erişimli bekleme düğümleri olarak davranacak bir veya daha fazla ikincil işlem düğümleri vardır.

### <a name="page-server-node"></a>Sunucu düğümü sayfası

Sayfa sunucuları genişletilmiş depolama altyapısı temsil eden sistemleridir.  Her bir sayfası sunucusu, veritabanındaki sayfaları kümesini sorumludur.  Olup, her sayfa sunucusu 1 terabayt veri denetler. Veri yok (dışında yedeklilik ve kullanılabilirlik tutulur çoğaltmalar) birden fazla sayfa sunucudaki paylaşılır. Bir sayfa sunucusunun isteğe bağlı işlem düğümlerine veritabanlarının sunmak ve veri hareketlerini güncelleştir güncel sayfaları tutmak işidir. Günlük hizmeti günlük kayıtları yürüterek sayfası sunucular güncel tutulur. Sayfa sunucuları da performansı geliştirmek için SSD tabanlı önbellekler bulundurur. Uzun vadeli depolaması ve veri sayfalarını ek güvenilirlik için Azure depolama alanında tutulur.

### <a name="log-service-node"></a>Günlük hizmeti düğümü

Günlük hizmeti düğümü birincil işlem düğümü günlük kayıtları kabul eder, bunları dayanıklı bir önbellekte kalıcı ve (, önbelleklerini güncelleştirebileceğiniz) ve böylece veriler, güncelleştirilmiş t olabilir günlük kayıtlarını geri kalanı için işlem düğümlerine ilgili sayfaya sunucuları yanı sıra iletir Burada. Bu şekilde, tüm veri değişikliklerini birincil işlem düğümünden tüm sayfa sunucuları ve ikincil işlem düğümleri için günlük hizmeti aracılığıyla dağıtılır. Son olarak, günlük kayıtları sonsuz depolama havuzu bir Azure depolama, uzun vadeli depolama için atılır. Bu mekanizma, sık sık günlük kesme için yeterlilikte kaldırır. Günlük hizmeti erişimi hızlandırmak üzere yerel önbelleği de vardır.

### <a name="azure-storage-node"></a>Azure Depolama düğümü

Azure depolama, veri sayfası sunucularından son hedefi düğümüdür. Bu depolama, Azure bölgeleri arasında çoğaltmayı hem de yedekleme amacıyla kullanılır. Yedekleme anlık görüntüsünü veri dosyalarının oluşur. Geri yükleme işlemi bu anlık görüntülerden hızlı olan ve veri geri zaman içinde herhangi bir noktasına yükleyebilirsiniz.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Temel dosya anlık görüntüsü yedekleri olan ve bu nedenle neredeyse anında. Depolama ve işlem ayrımı yedekleme/geri yükleme işlemi, birincil bir işlem düğümünde işleme yükünü azaltmak depolama katmanına gönderme etkinleştirin. Sonuç olarak, büyük bir veritabanının yedeğini birincil işlem düğümü performansını etkilemez. Benzer şekilde, geri yüklemeler dosya anlık görüntüsü kopyalayarak yapılır ve bu nedenle bir veri işlemi boyutunu değildir. Aynı depolama hesabında geri yüklemeler için geri yükleme işlemi hızlıdır.

## <a name="scale-and-performance-advantages"></a>Ölçek ve performans avantajları

Hızlı bir şekilde ek salt okunur işlem düğümleri Yukarı/Aşağı Döndür olanağı, mimari önemli sağlayan hiper ölçekli ölçeklendirme özelliklerinden okuma ve ayrıca daha fazla yazma isteklerine hizmet için birincil işlem düğümü boş. Ayrıca, işlem düğümlerine yukarı/aşağı hızla hiper ölçekli mimarisi paylaşılan depolama mimarisi nedeniyle ölçeklendirilebilir.

## <a name="create-a-hyperscale-database"></a>Hiper ölçekli bir veritabanı oluşturma

Kullanılarak bir hiper ölçekli veritabanı oluşturulabilir [Azure portalında](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create). Hiper ölçekli veritabanları yalnızca kullanılabilir [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

Aşağıdaki T-SQL komutu, Hiper ölçekli bir veritabanı oluşturur. Hem sürüm hem de hizmet hedef belirtmelisiniz `CREATE DATABASE` deyimi.

```sql
-- Create a HyperScale Database
CREATE DATABASE [HyperScaleDB1] (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen4_4');
GO
```

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanı için mevcut bir Azure SQL veritabanını geçirme

Hiper ölçekli kullanarak, mevcut Azure SQL veritabanlarınızı taşıyabilirsiniz [Azure portalında](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update). Genel önizlemede, tek yönlü bir geçiş budur. Başka bir hizmet katmanı için hiper ölçekli veritabanları taşıyamazsınız. Üretim veritabanlarınızı bir kopyasını alın ve (kanıtları) kavram kanıtı için hiper ölçekli için geçirme öneririz.

Aşağıdaki T-SQL komutu, Hiper ölçekli hizmet katmanına bir veritabanı taşır. Hem sürüm hem de hizmet hedef belirtmelisiniz `ALTER DATABASE` deyimi.

```sql
-- Alter a database to make it a HyperScale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen4_4');
GO
```

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Bir okuma ölçeği hiper ölçekli bir veritabanı çoğaltmasına bağlanmak

Hiper ölçekli veritabanlarında `ApplicationIntent` istemci tarafından sağlanan bağlantı dizesi bağımsız değişkenini belirleyen bağlantı yazma çoğaltmaya veya salt okunur bir ikincil çoğaltmaya yönlendirilir. Varsa `ApplicationIntent` kümesine `READONLY` ve veritabanı ikincil bir çoğaltmaya sahip değil, birincil çoğaltma ve varsayılan olarak bağlantı yönlendirilecek `ReadWrite` davranışı.

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="available-regions"></a>Kullanılabilen bölgeler

Hiper ölçekli hizmet katmanı şu anda genel önizlemede ve aşağıdaki Azure bölgelerinde kullanılabilir: 1 Doğu ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Kuzey CentralU S, Batı Avrupa, Kuzey Avrupa, Avustralya Doğu, Avustralya, Güneydoğu Avustralya, Güneydoğu Asya, Japonya Doğu ve Kore Orta

## <a name="known-limitations"></a>Bilinen sınırlamalar

| Sorun | Açıklama |
| :---- | :--------- |
| SQL server hiper ölçekli veritabanları, SQL Server'dan filtrelenir göstermez veritabanı yedekleri Yönet bölmesi ->  | Hiper ölçekli yedeklemeler yönetmek için ayrı bir yöntem vardır ve bu nedenle zaman yedek saklama ayarları noktasında ve uzun süreli saklama uygulanmaz / geçersiz kılınır. Buna göre hiper ölçekli veritabanlarını yedeklemeyi yönetme Bölmesi'nde görünmez. |
| Belirli bir noktaya geri yükleme | Bir veritabanı hiper ölçekli hizmet katmanına geçirildikten sonra bir-belirli bir noktaya geçişten önce geri yükleme desteklenmiyor.|
| Bir veritabanı dosyası etkin bir iş yükü nedeniyle geçiş sırasında artar ve dosya sınır başına 1 TB'ı aştığında, geçiş başarısız olur. | Risk azaltma işlemleri: <br> -Eğer Mümkünse, çalışan hiçbir güncelleştirme iş yükü olduğunda veritabanını geçirin.<br> -Geçiş işlemini yeniden deneyin, geçiş sırasında 1 TB sınır aşıldığında değil sürece başarılı olur.|
| Yönetilen örnek şu anda desteklenmiyor | Şu anda desteklenmiyor |
| Geçiş için hiper ölçekli şu anda bir tek yönlü işlem değil | Bir veritabanı için hiper ölçekli geçirildikten sonra doğrudan bir hiper ölçekli olmayan hizmet katmanına geçirilemez. Şu anda Hiperölçekli için olmayan hiper ölçekli bir veritabanını geçirme yalnızca kullanarak BACPAC dosyasına dışarı/içeri aktarma için yoludur.|
| Bellek içi nesneleri içeren bir veritabanı geçişi şu anda desteklenmiyor | Bellek içi nesneler bırakılan ve hiper ölçekli hizmet katmanına bir veritabanını geçirmeden önce bellek olmayan nesneler olarak yeniden oluşturulması gerekir.|
| Değişiklik izleme verileri şu anda desteklenmiyor. | Değişiklik izleme verileri ile hiper ölçekli databasess kullanmanız mümkün olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- Üzerinde hiper ölçekli bir SSS için bkz: [hiper ölçekli hakkında sık sorulan sorular](sql-database-service-tier-hyperscale-faq.md).
- Hizmet katmanları hakkında daha fazla bilgi için bkz: [hizmet katmanları](sql-database-purchase-models.md)
- Bkz: [kaynak bakış sınırlayan bir SQL veritabanı sunucusunda](sql-database-resource-limits-database-server.md) sunucu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
- Model sınırları tek bir veritabanı için satın almak için bkz: [Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sınırları tek bir veritabanı için](sql-database-vcore-resource-limits-single-databases.md).
- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
