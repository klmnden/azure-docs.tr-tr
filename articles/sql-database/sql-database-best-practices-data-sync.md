---
title: En iyi uygulamalar için Azure SQL Data Sync | Microsoft Docs
description: Yapılandırma ve Azure SQL Data Sync çalıştırmak için en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.date: 07/03/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.openlocfilehash: c8b8455dac9aa1a9f7747cada4ce85644162e331
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445169"
---
# <a name="best-practices-for-sql-data-sync"></a>SQL Data Sync için en iyi deneyimler 

Bu makalede, Azure SQL Data Sync için en iyi uygulamaları açıklar.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

## <a name="security-and-reliability"></a> Güvenlik ve güvenilirlik

### <a name="client-agent"></a>İstemci Aracısı

-   İstemci Aracısı, ağ hizmeti erişimi olan en az ayrıcalıklı kullanıcı hesabı kullanarak yükleyin.  
-   İstemci Aracısı, şirket içi SQL Server bilgisayarı olmayan bir bilgisayara yükleyin.  
-   Bir şirket içi veritabanı ile birden fazla aracı kayıt ettirmezseniz.    
    -   Farklı eşitleme grupları için farklı tabloları eşitleme yapan olsa bile bu kaçının.  
    -   Eşitleme grubu sildiğinizde, bir şirket içi veritabanı ile birden çok istemci aracıları yürütmelisiniz zorluklarının kaydediliyor.

### <a name="database-accounts-with-least-required-privileges"></a>Gerekli en az ayrıcalığa sahip veritabanı hesapları

-   **Eşitleme Kurulumu için**. Tablo oluşturma/değiştirme; Veritabanı değiştirmek; Yordam oluşturma Seç / şema değiştirmek; Kullanıcı tanımlı tür oluşturun.

-   **Devam eden eşitleme**. Seç / ekleme / güncelleştirme / eşitleme meta verileri ve izleme tablolarını ve eşitleme için seçilen tabloları silin; Saklı yordamlar hizmet tarafından oluşturulan çalıştırma izni; Kullanıcı tanımlı tablo türlerinde çalıştırma izni.

-   **Yetkiyi kaldırmak için**. Eşitleme tablolar parçası üzerinde değiştirmek; Seç / eşitleme meta verileri tablolarda Sil; Tabloları, saklı yordamlar ve kullanıcı tanımlı türler izleme eşitleme denetler.

Azure SQL veritabanı kimlik bilgileri yalnızca tek bir kümesini destekler. Bu kısıtlama içinde bu görevleri gerçekleştirmek için aşağıdaki seçenekleri göz önünde bulundurun:

-   Kimlik bilgilerini değiştirmek için farklı aşamaları (örneğin, *credentials1* kurulumu ve *credentials2* için devam eden).  
-   Değiştirme izni kimlik bilgilerinin (eşitleme ayarlandıktan sonra diğer bir deyişle, değiştirme izni).

## <a name="setup"></a>Kurulum

### <a name="database-considerations-and-constraints"></a> Veritabanı önemli noktalar ve sınırlamalar

#### <a name="sql-database-instance-size"></a>SQL veritabanı örnek boyutu

Yeni bir SQL veritabanı örneği oluşturduğunuzda, böylece her zaman dağıtım veritabanından daha büyük boyut üst sınırını ayarlayın. Büyük dağıtılan bir veritabanı için en büyük boyutu ayarlamazsanız, eşitleme başarısız olur. SQL Data Sync otomatik büyüme sunulmaz ancak çalıştırabileceğiniz `ALTER DATABASE` oluşturulduktan sonra veritabanı boyutunu artırmak için komutu. SQL veritabanı örneği boyutu sınırları içinde kalmasını sağlayın.

> [!IMPORTANT]
> SQL Data Sync ile her veritabanı ek meta verileri depolar. Gerekli alan hesaplarken bu meta veriler için hesap emin olun. Miktarı eklenen yükü tabloların genişliğine ilgili (örneğin, dar tabloları daha fazla ek yük gerektirir) ve trafik miktarı.

### <a name="table-considerations-and-constraints"></a> Tablo önemli noktalar ve sınırlamalar

#### <a name="selecting-tables"></a>Tabloları seçme

Bir veritabanında bir eşitleme grubunda olan tüm tabloları eklemek gerekmez. Bir eşitleme grubunda içeren tablolar, verimliliği ile maliyetleri etkiler. Tabloları ve bunlar yalnızca iş gerektiren gerekiyorsa bir eşitleme grubunda bağlı olarak, tabloları içerir.

#### <a name="primary-keys"></a>Birincil anahtarlar

Her bir eşitleme grubu tabloda bir birincil anahtarı olmalıdır. SQL Data Sync hizmet, birincil anahtarı olmayan tablo eşitleme yapılamıyor.

SQL Data Sync, üretim ortamında kullanmadan önce ilk ve devam eden eşitleme performansı test edin.

### <a name="provisioning-destination-databases"></a> Hedef veritabanı sağlama

SQL Data Sync temel veritabanı autoprovisioning sağlar.

Bu bölüm SQL Data Sync sağlanıyor kısıtlamaları açıklar.

#### <a name="autoprovisioning-limitations"></a>Autoprovisioning sınırlamaları

SQL Data Sync autoprovisioning üzerinde aşağıdaki sınırlamalara sahiptir:

-   Hedef tabloda oluşturulan sütunları seçin.  
    Eşitleme grubunun parçası olmayan tüm sütunları hedef tabloda sağlanan değildir.
-   Yalnızca Seçili sütunları için dizin oluşturulur.  
    Kaynak tablo dizin eşitleme grubunun parçası olmayan sütunları varsa, bu dizinler hedef tabloda sağlanan değildir.  
-   XML türü sütunlarındaki dizinler sağlanan değildir.  
-   Denetim kısıtlamalarında sağlanan değildir.  
-   Kaynak tablolarda varolan tetikleyicilerinizi sağlanan değildir.  
-   Görünüm ve saklı yordam hedef veritabanında oluşturulmaz.
-   UPDATE CASCADE ve ON DELETE CASCADE eylemleri yabancı anahtar kısıtlamaları hedef tabloda yeniden değildir.

#### <a name="recommendations"></a>Öneriler

-   Yalnızca hizmetin ölçeğini çalışırken SQL Data Sync autoprovisioning olanağını kullanın.  
-   Üretim için veritabanı şemasını sağlayın.

### <a name="locate-hub"></a> Hub veritabanı bulunacağı yere

#### <a name="enterprise-to-cloud-scenario"></a>Kurumsal bulut senaryosu

Gecikmeyi en aza indirmek için hub veritabanı eşitleme grubunun veritabanı trafiğini en yüksek yoğunluk yakın tutun.

#### <a name="cloud-to-cloud-scenario"></a>Bulut Bulut senaryosu

-   Bir eşitleme grubu içindeki tüm veritabanlarına merkezinde olduğunda, hub'ı aynı veri merkezinde bulunmalıdır. Bu yapılandırma, gecikme süresi ve veri merkezleri arasında veri aktarımı maliyeti azaltır.
-   Bir eşitleme grubu içindeki veritabanlarını birden çok veri merkezlerinde olduğunda hub veritabanlarının ve veritabanı trafiğinin çoğu olarak aynı veri merkezinde bulunmalıdır.

#### <a name="mixed-scenarios"></a>Karma senaryolar

Kurumsal Bulut ve Bulut Bulut senaryoları bir karışımını olanlar gibi karmaşık bir eşitleme grubu yapılandırmaları önceki yönergeleri uygulayın.

## <a name="sync"></a>Sync

### <a name="avoid-a-slow-and-costly-initial-synchronization"></a> Yavaş ve pahalı ilk eşitleme kaçının

Bu bölümde, bir eşitleme grubu ilk eşitlemesi ele alır. Uzun sürüyor ve gerekenden daha pahalı bir ilk eşitleme önlemeye yardımcı olmak konusunda bilgi edinin.

#### <a name="how-initial-sync-works"></a>Nasıl ilk eşitleme çalışır

Bir eşitleme grubu oluşturma, tek bir veritabanındaki verilerle başlayın. SQL Data Sync, birden çok veritabanında veri varsa, her satır çözülmesi gereken bir çakışma değerlendirir. Bu çakışma yavaş gitmek ilk eşitleme neden olur. Birden çok veritabanında veri varsa, ilk eşitleme veritabanı boyutuna bağlı olarak birkaç ay arasındaki birkaç gün sürebilir.

Veritabanlarını farklı veri merkezlerinde varsa, her satır arasında farklı veri merkezlerinde geçmelidir. Bu, bir ilk eşitleme maliyetini artırır.

#### <a name="recommendation"></a>Öneri

Mümkünse, veri eşitleme grubunun veritabanları yalnızca birinde başlayın.

### <a name="design-to-avoid-synchronization-loops"></a> Eşitleme döngülerinden kaçınmak için Tasarım

Bir eşitleme döngüsü, bir eşitleme grubu içinde döngüsel başvurular olduğunda gerçekleşir. Bu senaryoda, bir veritabanındaki her değişiklik Kısıtlamasız bir şekilde ve döngüsel olarak eşitleme grubundaki veritabanları arasında çoğaltılır.   

Performans düşüşüne neden olur ve maliyetleri önemli ölçüde artabilir olduğundan eşitleme döngüleri önlemek emin olun.

### <a name="handling-changes-that-fail-to-propagate"></a> Yayılması başarısız değişiklikler

#### <a name="reasons-that-changes-fail-to-propagate"></a>Değişikliklerin yayılması başarısız nedenleri

Değişiklikler, aşağıdaki nedenlerden birinden dolayı yaymak başarısız olabilir:

-   Şema/datatype uyumsuzluğu.
-   Null atanamayan sütunlar ekleniyor.
-   Yabancı anahtar kısıtlamalarını ihlal ediyor.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Değişikliklerin yayılması başarısız olduğunda ne olur?

-   Eşitleme içinde grup gösterir bir **uyarı** durumu.
-   Ayrıntıları portal UI günlük Görüntüleyicisi'nde listelenmiştir.
-   45 gün sorun çözülmezse, veritabanını eski haline gelir.

> [!NOTE]
> Bu değişiklikleri hiçbir zaman aktarabilirsiniz. Bu senaryoda, kurtarılır tek yolu eşitleme grubunu yeniden oluşturmaktır.

#### <a name="recommendation"></a>Öneri

Eşitleme grubu ve veritabanı sistem durumu portalı ve günlük arabirimi aracılığıyla düzenli olarak izleyin.


## <a name="maintenance"></a>Bakım

### <a name="avoid-out-of-date-databases-and-sync-groups"></a> Güncel olmayan veritabanları önlemek ve grupları Eşitle

Bir eşitleme grubu ya da bir veritabanında bir eşitleme grubu güncel hale gelebilir. Bir eşitleme grubunun durumu olduğunda **güncel**, çalışmayı durdurur. Bir veritabanının durumu olduğunda **güncel**, verileri kaybolabilir. Bu durumdan kurtulmanın çalışırken yerine bu senaryonun olmaması idealdir.

#### <a name="avoid-out-of-date-databases"></a>Güncel olmayan veritabanları kaçının

Bir veritabanının durumu kümesine **güncel** zaman da meydana geldi çevrimdışı 45 gün veya daha fazla bilgi için. Önlemek için bir **güncel** durumu bir veritabanında veritabanlarının hiçbiri 45 gün veya daha fazla bilgi için çevrimdışı olduğundan emin olun.

#### <a name="avoid-out-of-date-sync-groups"></a>Güncel olmayan eşitleme grupları kaçının

Bir eşitleme grubunun durumu kümesine **güncel** eşitleme grubundaki herhangi bir değişiklik başarısız olduğunda 45 gün veya daha fazla eşitleme grubunu geri kalanı için yaymak. Önlemek için bir **güncel** bir eşitleme grubu durumunu düzenli olarak eşitleme grubunun Geçmiş günlüğünü denetleyin. Tüm çakışmaları çözümlenir ve değişiklikleri veritabanları eşitleme grubu başarıyla yayılana emin olun.

Bir eşitleme grubu, aşağıdaki nedenlerden biri için bir değişikliği uygulamak başarısız olabilir:

-   Tablolar arasındaki bir şema uyumsuzluğu.
-   Tablolar arasında veri uyumsuzluğu.
-   Bir satır null değere sahip bir sütunda null değerlere izin vermeyecek ekleniyor.
-   Bir satır bir yabancı anahtar kısıtlaması ihlal eden bir değer ile güncelleştiriliyor.

Güncel olmayan eşitleme grupları engellemek için:

-   Şema başarısız satırlarda bulunan değerlere izin verecek şekilde güncelleştirin.
-   Yabancı anahtar değerleri içerdiği değerlerin başarısız satırları içerecek şekilde güncelleştirin.
-   Önceden şema veya hedef veritabanındaki yabancı anahtarlar ile uyumlu olduklarından başarısız satırındaki veri değerlerini güncelleştirin.

### <a name="avoid-deprovisioning-issues"></a> Sorunları sağlamayı kaldırma kaçının

Bazı durumlarda, bir veritabanı ile bir istemci Aracısı kaydını eşitleme başarısız olmasına neden olabilir.

#### <a name="scenario"></a>Senaryo

1. SQL veritabanı örneğinde ve yerel Aracı 1 ile ilişkili bir şirket içi SQL Server veritabanını kullanarak bir eşitleme grubu oluşturuldu.
2. Aynı şirket içi veritabanı, yerel Aracı (Bu aracı, herhangi bir eşitleme grubu ile ilişkili değil) 2 ile kaydedilmiştir.
3. Şirket içi veritabanı kaydını yerel Aracıdan izleme 2 kaldırır ve meta tablolar için şirket içi veritabanı için bir grup eşitleme.
4. Eşitleme grubu bu hata bir işlem başarısız olur: "geçerli işlem eşitleme yapılandırma tablolar için izniniz yok veya veritabanı eşitleme için sağlanmamış olduğundan tamamlanamadı."

#### <a name="solution"></a>Çözüm

Bu senaryonun olmaması için bir veritabanı ile birden fazla aracı kayıt ettirmezseniz.

Bu senaryodan kurtarmak için:

1. Veritabanı ait her bir eşitleme grubunu kaldırın.  
2. Buradan kaldırdığınız geri her eşitleme grubuna veritabanı ekleyin.  
3. (Bu işlem, veritabanı sağlar) her bir etkilenen eşitleme grubuna dağıtın.  

### <a name="modifying-your-sync-group"></a> Bir eşitleme grubu değiştirme

Bir veritabanını bir eşitleme grubundan kaldırmanız ve ardından değişikliklerin dağıtma ilki olmadan eşitleme grubu düzenlemek denemeyin.

Bunun yerine, önce bir veritabanı eşitleme grubundan kaldırın. Daha sonra değişikliğin dağıtılması ve tamamlanması sağlamayı kaldırma için bekleyin. Sağlamayı kaldırma işlemi tamamlandığında eşitleme grubu düzenleme ve değişiklikleri dağıtın.

Bir veritabanını kaldırın ve ardından değişikliklerin dağıtma ilki olmadan bir eşitleme grubu düzenlemek çalışırsanız, bir veya başka bir işlem başarısız olur. Portal arabirimi tutarsız hale gelebilir. Böyle bir durumda doğru duruma geri yüklemek için sayfayı yenileyin.

## <a name="next-steps"></a>Sonraki adımlar
SQL Data Sync hakkında daha fazla bilgi için bkz:

-   [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md)
-   [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)
-   [Azure SQL Data Sync’i Log Analytics ile izleme](sql-database-sync-monitor-oms.md)
-   [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)  
-   SQL Data Sync’in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri:  
    -   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [SQL Data Sync REST API belgelerini indirin](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)  

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanına genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
