---
title: Azure SQL veritabanı hiper ölçekli genel bakış | Microsoft Docs
description: Bu makalede Azure SQL veritabanı'ndaki sanal çekirdek tabanlı satın alma modeli hiper ölçekli hizmet katmanında ve genel amaçlı ve iş açısından kritik hizmet katmanı arasından farklı nasıl açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 05/06/2019
ms.openlocfilehash: 4aeda5612b2b3e9e2073a65320b238266c8bb33a
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537863"
---
# <a name="hyperscale-service-tier-for-up-to-100-tb"></a>En fazla 100 TB için hiper ölçekli hizmet katmanı

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için ayarlanmış bir SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan üç Mimari modeli vardır:
- Genel amaçlı/standart 
-  Hiper Ölçek
-  Kritik iş/Premium

Azure SQL veritabanı'nda hiper ölçekli hizmet katmanı sanal çekirdek tabanlı satın alma modeli en yeni hizmet katmanında ' dir. Bu hizmet katmanında yüksek düzeyde ölçeklenebilir depolama ve Azure mimarisini depolama ve önemli ölçüde kullanılabilir sınırları aşan bir Azure SQL veritabanı için genel amaçlı ve iş için bilgi işlem kaynaklarını yararlanır işlem performans katmanı olan Kritik hizmet katmanları.

> 
> [!NOTE]
> Sanal çekirdek tabanlı satın alma modeli, genel amaçlı ve iş açısından kritik hizmet katmanları hakkında daha fazla bilgi için bkz: [genel amaçlı](sql-database-service-tier-general-purpose.md) ve [iş açısından kritik](sql-database-service-tier-business-critical.md) hizmet katmanları. Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-service-tiers.md).


## <a name="what-are-the-hyperscale-capabilities"></a>Hiper ölçekli özellikleri nelerdir

Azure SQL veritabanı'nda hiper ölçekli Hizmet katmanını, aşağıdaki özellikleri sağlar:

- Veritabanı boyutu en fazla 100 TB için destek
- Neredeyse anında işlem kaynakları üzerinde herhangi bir GÇ etkisi boyutundan bağımsız olarak veritabanı yedeklemeleri (Azure Blob depolamada depolanan dosya anlık görüntüleri göre)  
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

  Büyük ölçekli işlem birim fiyatı bir çoğaltmadır. [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) ölçek çoğaltmaları otomatik olarak okumak için fiyat uygulanır. Birincil çoğaltma ve salt okunur bir çoğaltması hiper ölçekli veritabanı başına varsayılan olarak oluştururuz.  Kullanıcıların toplam sayısı 1-5 birincil dahil olmak üzere çoğaltmaları ayarlayabilirsiniz.

- **Depolama**:

  Maks. veri boyutu bir hiper ölçekli veritabanı yapılandırırken belirtmeniz gerekmez. Hiper ölçeklendirme katmanında, veritabanınıza yönelik depolama alanı için gerçek kullanıma göre ücret alınır. Depolama alanı, 40 GB ve 10 GB arasında dinamik olarak ayarlanmış artışlarla 10 GB ve 100 TB arasında otomatik olarak ayrılır.  

Hiper ölçekli fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/single/)

## <a name="distributed-functions-architecture"></a>Dağıtılmış işlevleri mimarisi

Tüm veri yönetim işlevlerinin bir işlemde konum / (hatta bugün üretimde çağrılan dağıtılmış veritabanları tek parçalı veri altyapısı birden çok kopyasını böylece) merkezi geleneksel veritabanı altyapıları, bir hiper ölçekli veritabanı ayırır. Burada çeşitli veri altyapıları semantiği, uzun vadeli depolama ve dayanıklılık için veri sağlayan bileşenlerden ayırmak sorgu işleme altyapısı. Bu şekilde, depolama kapasitesini sorunsuz gerektiği kadar genişletilebilir (ilk hedef değer 100 TB). Yeni bir okunabilir çoğaltma dönmesi için hiçbir veri kopyalama gerekli şekilde salt okunur çoğaltmalar aynı depolama bileşenleri paylaşın. 

Aşağıdaki diyagram, farklı türde bir hiper ölçekli veritabanında düğümleri gösterir:

![architecture](./media/sql-database-hyperscale/hyperscale-architecture.png)

Bir hiper ölçekli veritabanı aşağıdaki farklı tür düğüm içerir:

### <a name="compute-node"></a>İşlem düğümü

İşlem düğümü nerede ilişkisel altyapıda yaşıyor, tüm dil öğeleri, sorgu işleme ve benzeri ortaya olduğundan. Sorun bu pencerelerde hiper ölçekli veritabanına sahip tüm kullanıcı etkileşimlerine işlem düğümleri. Düğüm ağ sayısını en aza indirmek için SSD tabanlı önbellekler (etiketli RBPEX - önceki diyagramdaki dayanıklı arabellek havuzu genişletme) sahip işlem gidiş dönüş bir sayfa veri getirmek için gerekli. Tüm okuma-yazma iş yükleri ve işlemleri işleneceği bir birincil işlem düğümü vardır. (Bu işlevselliği isteniyorsa) iş yükleri okuma boşaltma için salt okunur işlem düğümleri olarak davranmak yanı sıra yük devretme amacıyla sık erişimli bekleme düğümleri olarak davranacak bir veya daha fazla ikincil işlem düğümleri vardır.

### <a name="page-server-node"></a>Sunucu düğümü sayfası

Sayfa sunucuları genişletilmiş depolama altyapısı temsil eden sistemleridir.  Her bir sayfası sunucusu, veritabanındaki sayfaları kümesini sorumludur.  Her bir sayfası sunucusu olup, 128 GB ile 1 TB veri arasında kontrol eder. Veri yok (dışında yedeklilik ve kullanılabilirlik tutulur çoğaltmalar) birden fazla sayfa sunucudaki paylaşılır. Bir sayfa sunucusunun isteğe bağlı işlem düğümlerine veritabanlarının sunmak ve veri hareketlerini güncelleştir güncel sayfaları tutmak işidir. Günlük hizmeti günlük kayıtları yürüterek sayfası sunucular güncel tutulur. Sayfa sunucuları da performansı geliştirmek için SSD tabanlı önbellekler bulundurur. Uzun vadeli depolaması ve veri sayfalarını ek güvenilirlik için Azure depolama alanında tutulur.

### <a name="log-service-node"></a>Günlük hizmeti düğümü

Günlük hizmeti düğümü birincil işlem düğümü günlük kayıtları kabul eder, bunları dayanıklı bir önbellekte kalıcı ve (, önbelleklerini güncelleştirebileceğiniz) ve böylece veriler, güncelleştirilmiş t olabilir günlük kayıtlarını geri kalanı için işlem düğümlerine ilgili sayfaya sunucuları yanı sıra iletir Burada. Bu şekilde, tüm veri değişikliklerini birincil işlem düğümünden tüm sayfa sunucuları ve ikincil işlem düğümleri için günlük hizmeti aracılığıyla dağıtılır. Son olarak, günlük kayıtları sonsuz depolama havuzu bir Azure depolama, uzun vadeli depolama için atılır. Bu mekanizma, sık sık günlük kesme için yeterlilikte kaldırır. Günlük hizmeti erişimi hızlandırmak üzere yerel önbelleği de vardır.

### <a name="azure-storage-node"></a>Azure Depolama düğümü

Azure depolama, veri sayfası sunucularından son hedefi düğümüdür. Bu depolama, Azure bölgeleri arasında çoğaltmayı hem de yedekleme amacıyla kullanılır. Yedekleme anlık görüntüsünü veri dosyalarının oluşur. Geri yükleme işlemi bu anlık görüntülerden hızlı olan ve veri geri zaman içinde herhangi bir noktasına yükleyebilirsiniz.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Temel dosya anlık görüntüsü yedekleri olan ve bu nedenle neredeyse anında. Depolama ve işlem ayrımı yedekleme/geri yükleme işlemi, birincil bir işlem düğümünde işleme yükünü azaltmak depolama katmanına gönderme etkinleştirin. Sonuç olarak, büyük bir veritabanının yedeğini birincil işlem düğümü performansını etkilemez. Benzer şekilde, geri yüklemeler dosya anlık görüntüsü kopyalayarak yapılır ve bu nedenle bir veri işlemi boyutunu değildir. Aynı depolama hesabında geri yüklemeler için geri yükleme işlemi hızlıdır.

## <a name="scale-and-performance-advantages"></a>Ölçek ve performans avantajları

Hızlı bir şekilde ek salt okunur işlem düğümleri Yukarı/Aşağı Döndür olanağı, mimari önemli sağlayan hiper ölçekli ölçeklendirme özelliklerinden okuma ve ayrıca daha fazla yazma isteklerine hizmet için birincil işlem düğümü boş. Ayrıca, işlem düğümlerine yukarı/aşağı hızla hiper ölçekli mimarisi paylaşılan depolama mimarisi nedeniyle ölçeklendirilebilir.

## <a name="create-a-hyperscale-database"></a>Hiper ölçekli bir veritabanı oluşturma

Kullanılarak bir hiper ölçekli veritabanı oluşturulabilir [Azure portalında](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create). Hiper ölçekli veritabanları yalnızca kullanılabilir [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

Aşağıdaki T-SQL komutu, Hiper ölçekli bir veritabanı oluşturur. Hem sürüm hem de hizmet hedef belirtmelisiniz `CREATE DATABASE` deyimi. Başvurmak [kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases#hyperscale-service-tier) geçerli hizmet hedeflerini listesi.

```sql
-- Create a HyperScale Database
CREATE DATABASE [HyperScaleDB1] (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```
Bu, 4 çekirdek, 5. nesil donanımlarda hiper ölçekli bir veritabanı oluşturur.

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanı için mevcut bir Azure SQL veritabanını geçirme

Hiper ölçekli kullanarak, mevcut Azure SQL veritabanlarınızı taşıyabilirsiniz [Azure portalında](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update). Şu anda tek yönlü bir geçiş budur. Başka bir hizmet katmanı için hiper ölçekli veritabanları taşıyamazsınız. Üretim veritabanlarınızı bir kopyasını alın ve (kanıtları) kavram kanıtı için hiper ölçekli için geçirme öneririz.

Aşağıdaki T-SQL komutu, Hiper ölçekli hizmet katmanına bir veritabanı taşır. Hem sürüm hem de hizmet hedef belirtmelisiniz `ALTER DATABASE` deyimi.

```sql
-- Alter a database to make it a HyperScale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Bir okuma ölçeği hiper ölçekli bir veritabanı çoğaltmasına bağlanmak

Hiper ölçekli veritabanlarında `ApplicationIntent` istemci tarafından sağlanan bağlantı dizesi bağımsız değişkenini belirleyen bağlantı yazma çoğaltmaya veya salt okunur bir ikincil çoğaltmaya yönlendirilir. Varsa `ApplicationIntent` kümesine `READONLY` ve veritabanı ikincil bir çoğaltmaya sahip değil, birincil çoğaltma ve varsayılan olarak bağlantı yönlendirilecek `ReadWrite` davranışı.

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```
## <a name="disaster-recovery-for-hyperscale-databases"></a>Hiper ölçekli veritabanları için olağanüstü durum kurtarma
### <a name="restoring-a-hyperscale-database-to-a-different-geography"></a>Hiper ölçekli bir veritabanı için farklı bir coğrafi geri yükleme
Şu anda, olağanüstü durum kurtarma işlemi veya ayrıntıya, yeniden konumlandırma veya diğer herhangi bir nedenle bir parçası olarak barındırıldığı farklı bir bölge için bir Azure SQL veritabanı hiper ölçekli DB geri yüklemeniz gerekirse birincil veritabanının coğrafi geri yükleme bir yapmak için yöntemidir.  Bu, farklı bir bölgeye diğer bir AZURE SQL veritabanını geri yüklemek için kullanmak olarak tam olarak aynı adımları içerir:
1. Zaten uygun bir sunucu var olmayan varsa SQL veritabanı sunucusu hedef bölgede oluşturun.  Bu sunucu özgün (kaynak) sunucu ile aynı aboneliğe ait.
2. Bölümündeki yönergeleri [coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#geo-restore) Azure SQL veritabanları, otomatik yedeklemelerden geri yükleme sayfasının konu.

> [!NOTE]
> Farklı bölgelerde olduğundan, kaynak ve hedef veritabanını kaynak veritabanıyla olduğu son derece hızlı bir şekilde tamamlamak olmayan coğrafi geri yükleme gibi anlık görüntü depolama paylaşamazsınız.  Bir coğrafi geri yükleme durumunda bir hiper ölçekli veritabanının, hedef coğrafi çoğaltmalı depolama eşleştirilmiş bölgede olsa bile, bir veri boyutu işlemi olacaktır.  Bir coğrafi geri yükleme yaparsanız kurtarılan veritabanının boyutunu orantılı zaman süreceği anlamına gelir.  Hedef eşlenen bölgesi içinde ise, kopyalama internet üzerinden şehirlerarası kopyasını önemli ölçüde daha hızlı olacak, bir veri merkezi içinde olacaktır, ancak tüm bitleri kopyalayın devam.

## <a name=regions></a>Kullanılabilir bölgeler

Şu bölgelerde Azure SQL veritabanı hiper ölçekli katmanı şu anda kullanılabilir:

- Avustralya Doğu
- Avustralya Güneydoğu
- Güney Brezilya
- Orta Kanada
- Orta ABD
- Çin Doğu 2
- Çin Kuzey 2
- Doğu Asya
- East US
- Doğu ABD 2
- Fransa Orta
- Japonya Doğu
- Japonya Batı
- Kore Orta
- Kore Güney
- Orta Kuzey ABD
- Kuzey Avrupa
- Güney Afrika Kuzey
- Orta Güney ABD
- Güneydoğu Asya
- Birleşik Krallık Güney
- Birleşik Krallık Batı
- Batı Avrupa
- Batı ABD
- Batı ABD 2

Desteklenen belirtilmeyen bir bölgede hiper ölçekli veritabanı oluşturmak istiyorsanız, Azure portal aracılığıyla bir ekleme isteği gönderebilirsiniz. Listesini genişletmek için çalışıyoruz bu nedenle Lütfen onay son bölge listesi için desteklenen bölgeler.

Listede olmayan bölgelerde hiper ölçekli veritabanları oluşturma olanağı istemek için:

1. Gidin [Azure Yardım ve Destek dikey penceresi](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

2. Tıklayarak [ **yeni destek isteği**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

    ![Azure Yardım ve Destek dikey penceresi](media/sql-database-service-tier-hyperscale/whitelist-request-screen-1.png)

3. İçin **sorun türü**seçin **hizmet ve abonelik sınırlarını (kotalar)**

4. Veritabanları oluşturmak için kullanacağınız aboneliği seçin

5. İçin **kota türü**seçin **SQL veritabanı**

6. Tıklayın **sonraki: Çözümleri**

1. Tıklayın **ayrıntılarını sağlayın**

    ![Sorun ayrıntıları](media/sql-database-service-tier-hyperscale/whitelist-request-screen-2.png)

8. Seçin **SQL veritabanı kota türü**: **Diğer kota isteği**

9. Aşağıdaki şablon doldurun:

    ![Kota ayrıntıları](media/sql-database-service-tier-hyperscale/whitelist-request-screen-3.png)

    Şablonda, aşağıdaki bilgileri sağlayın

    > Yeni bölgede Azure Hiperölçekli SQL veritabanı oluşturma isteği<br/> Bölge: [istenen bölgenizde dolgu]  <br/>
    > SKU/toplam çekirdek okunabilir çoğaltma dahil olmak üzere işlem <br/>
    > Tahmini TB sayısı 
    >

10. **Önem Derecesi C**’yi seçin

11. Uygun iletişim yöntemini seçin ve ayrıntıları doldurun.

12. Tıklayın **Kaydet** ve **devam edin**

## <a name="known-limitations"></a>Bilinen sınırlamalar
Bunlar hiper ölçekli hizmet katmanına büyüyecek itibarıyla geçerli sınırlamalar  Etkin bir şekilde Bu sınırlamalar olabildiğince fazla sayıda kaldırmak için çalışıyoruz.

| Sorun | Açıklama |
| :---- | :--------- |
| Yedekleri Yönet bölmesinde bir mantıksal sunucu için hiper ölçekli veritabanları, SQL Server'dan filtrelenir göstermez  | Hiper ölçekli yedeklemeler yönetmek için ayrı bir yöntem vardır ve bu nedenle uzun vadeli bekletme ve saat yedekleme bekletme ayarlarını noktasında uygulanmaz / geçersiz kılınır. Buna göre hiper ölçekli veritabanlarını yedeklemeyi yönetme Bölmesi'nde görünmez. |
| Belirli bir noktaya geri yükleme | Bir veritabanı hiper ölçekli hizmet katmanına geçirildikten sonra bir-belirli bir noktaya geçişten önce geri yükleme desteklenmiyor.|
| Geri yükleme, olmayan - hiper ölçekli DB Hypserscale ve tersi | Bir hiper ölçekli veritabanı olmayan hiper ölçekli bir veritabanına geri yükleyemezsiniz ya da bir hiper ölçekli olmayan veritabanı hiper ölçekli bir veritabanına geri yükleyebilirsiniz.|
| Bir veritabanı dosyası etkin bir iş yükü nedeniyle geçiş sırasında artar ve dosya sınır başına 1 TB'ı aştığında, geçiş başarısız olur. | Risk azaltma işlemleri: <br> -Eğer Mümkünse, çalışan hiçbir güncelleştirme iş yükü olduğunda veritabanını geçirin.<br> -Geçiş işlemini yeniden deneyin, geçiş sırasında 1 TB sınır aşıldığında değil sürece başarılı olur.|
| Yönetilen Örnek | Azure SQL veritabanı yönetilen örneği, Hiper ölçekli veritabanlarıyla şu anda desteklenmiyor. |
| Esnek Havuzlar |  Esnek havuzlar ile SQL veritabanı Hiperölçekli şu anda desteklenmemektedir.|
| Geçiş için hiper ölçekli şu anda bir tek yönlü işlem değil | Bir veritabanı için hiper ölçekli geçirildikten sonra doğrudan bir hiper ölçekli olmayan hizmet katmanına geçirilemez. Şu anda Hiperölçekli için olmayan hiper ölçekli bir veritabanını geçirme yalnızca kullanarak BACPAC dosyasına dışarı/içeri aktarma için yoludur.|
| Bellek içi nesneleri içeren bir veritabanı geçişi | Bellek içi nesneler bırakılan ve hiper ölçekli hizmet katmanına bir veritabanını geçirmeden önce bellek olmayan nesneler olarak yeniden oluşturulması gerekir.|
| Değişiklik izleme verileri | Değişiklik izleme verileri hiper ölçekli veritabanlarıyla kullanmanız mümkün olmayacaktır. |
| Coğrafi Çoğaltma  | Coğrafi çoğaltma, henüz Azure SQL veritabanı için hiper ölçekli yapılandıramazsınız.  Coğrafi-geri yükler (DR veya diğer amaçlar için veritabanı farklı bir coğrafi geri yükleme) gerçekleştirebilirsiniz. |
| TDE/AKV tümleştirme | Hizmet tarafından yönetilen anahtarlar ile TDE tam olarak desteklenir ancak saydam veritabanı şifrelemesi Azure anahtar Kasası'nın (Getir--kendi-anahtarınız veya BYOK olarak yaygın olarak adlandırılır) kullanarak Azure SQL veritabanı Hiperölçekli için henüz desteklenmiyor. |
|Akıllı veritabanı özellikleri | 1. Dizin oluşturma, Drop Index Danışman modelleri için hiper ölçekli DBs eğitilir değil. <br/>2. Şema sorunu, DbParameterization - en son eklenen advisers hiper ölçekli veritabanı için desteklenmiyor.|



## <a name="next-steps"></a>Sonraki adımlar

- Üzerinde hiper ölçekli bir SSS için bkz: [hiper ölçekli hakkında sık sorulan sorular](sql-database-service-tier-hyperscale-faq.md).
- Hizmet katmanları hakkında daha fazla bilgi için bkz: [hizmet katmanları](sql-database-service-tiers.md)
- Bkz: [kaynak bakış sınırlayan bir mantıksal sunucuda](sql-database-resource-limits-logical-server.md) sunucusu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
- Model sınırları tek bir veritabanı için satın almak için bkz: [Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sınırları tek bir veritabanı için](sql-database-vcore-resource-limits-single-databases.md).
- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
