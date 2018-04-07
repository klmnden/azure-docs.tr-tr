---
title: En iyi uygulamalar için Azure SQL veri eşitleme (Önizleme) | Microsoft Docs
description: Yapılandırma ve Azure SQL veri eşitleme (Önizleme) çalıştırmak için en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.date: 04/01/2018
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7ce7830d853a77b54706201fa614e9f4bee637a4
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="best-practices-for-sql-data-sync-preview"></a>SQL veri eşitleme (Önizleme) için en iyi yöntemler 

Bu makale, Azure SQL veri eşitleme (Önizleme) için en iyi uygulamaları açıklar.

SQL veri eşitleme (Önizleme) genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme) ile](sql-database-sync-data.md).

## <a name="security-and-reliability"></a> Güvenlik ve güvenilirlik

### <a name="client-agent"></a>İstemci Aracısı

-   İstemci Aracısı, ağ hizmeti erişimine sahip en az ayrıcalıklı kullanıcı hesabı kullanarak yükleyin.  
-   İstemci Aracısı, şirket içi SQL Server bilgisayarı olmayan bir bilgisayara yükleyin.  
-   Bir şirket içi veritabanına birden fazla aracı ile kayıt yok.    
    -   Farklı eşitleme grupları için farklı tablolara eşitleniyor olsa bile bu kaçının.  
    -   Eşitleme grubu sildiğinizde bir şirket içi veritabanına birden çok istemci aracıları üzerinden zorluklar kaydediliyor.

### <a name="database-accounts-with-least-required-privileges"></a>En az gerekli ayrıcalıklara sahip veritabanı hesapları

-   **Eşitleme Kurulumu için**. Tablo Oluştur/Değiştir; ALTER Database; Yordam oluşturma; Seçin / şema Alter; Kullanıcı tanımlı tür oluşturun.

-   **Devam eden eşitleme için**. Seçin / ekleme / güncelleştirme / eşitlenmesi ve meta verileri Eşitle ve izleme tablolarını seçili tabloları silin; Hizmeti tarafından oluşturulan saklı yordamları çalıştırma izni; Kullanıcı tanımlı tablo türlerinde çalıştırma izni.

-   **Sağlamayı kaldırmayı için**. Eşitleme tabloları parçası alter; Seçin / eşitleme meta verileri tablolarda Sil; İzleme tabloları, saklı yordamları ve kullanıcı tanımlı türler eşitleme denetler.

Azure SQL veritabanı kimlik bilgileri, yalnızca tek bir kümesini destekler. Bu sınırlama içinde bu görevleri gerçekleştirmek için aşağıdaki seçenekleri göz önünde bulundurun:

-   Farklı aşamalarında için kimlik bilgilerini değiştirin (örneğin, *credentials1* kurulumu ve *credentials2* için devam eden).  
-   Değiştirme izni kimlik bilgilerinin (eşitleme ayarlandıktan sonra başka bir deyişle, değiştirme izni).

## <a name="setup"></a>Kurulum

### <a name="database-considerations-and-constraints"></a> Veritabanı konuları ve kısıtlamaları

#### <a name="sql-database-instance-size"></a>SQL Database örnek boyutu

Yeni bir SQL veritabanı örneği oluşturduğunuzda, en büyük boyutu her zaman dağıttığınız veritabanından daha büyük olduğu şekilde ayarlayın. Dağıtılan veritabanı büyük için en büyük boyutu ayarlamazsanız, eşitleme başarısız olur. SQL veri eşitleme (Önizleme) otomatik büyüme sunmaz rağmen çalıştırabilirsiniz `ALTER DATABASE` oluşturulduktan sonra veritabanı boyutunu artırmak için komutu. SQL veritabanı örneği boyutu sınırları içinde kalmasını sağlayın.

> [!IMPORTANT]
> SQL veri eşitleme (Önizleme) ile her veritabanı ek meta verileri depolar. Gerekli alan hesaplarken bu meta veriler için hesap emin olun. Miktarını eklenen ek yükü tabloları genişliğini ilgili (örneğin, daha fazla ek yükü dar tablolarda gereklidir) ve trafik miktarı.

### <a name="table-considerations-and-constraints"></a> Tablo konuları ve kısıtlamaları

#### <a name="selecting-tables"></a>Tabloları seçme

Bir veritabanında bir eşitleme grubundaki tüm tabloları eklemek zorunda değilsiniz. Bir eşitleme Grubu'na dahil tabloları verimliliği ve maliyetleri etkiler. Tabloları ve bunlar yalnızca iş gerektiren gerekiyorsa bir eşitleme grubundaki, bağlı tabloları içerir.

#### <a name="primary-keys"></a>Birincil anahtarlar

Her bir eşitleme grubu tablosunda birincil anahtar olması gerekir. SQL veri eşitleme (Önizleme) hizmeti, bir birincil anahtara sahip olmayan bir tablo eşitleyemiyor.

SQL veri eşitleme (Önizleme) üretimde kullanmadan önce ilk ve devam eden eşitleme performansını test edin.

### <a name="provisioning-destination-databases"></a> Hedef veritabanı sağlama

SQL veri eşitleme (Önizleme) önizleme temel veritabanı autoprovisioning sağlar.

Bu bölümde, SQL veri eşitleme (Önizleme) sağlama sınırlamaları anlatılmaktadır.

#### <a name="autoprovisioning-limitations"></a>Autoprovisioning sınırlamaları

SQL veri eşitleme (Önizleme) autoprovisioning üzerinde aşağıdaki sınırlamalara sahiptir:

-   Hedef tabloda oluşturulan sütunları seçin.  
    Eşitleme grubunu parçası olmayan herhangi bir sütundan hedef tablolarında sağlanan değil.
-   Dizinler yalnızca seçilen sütunlar için oluşturulur.  
    Kaynak tablo dizin eşitleme grubunun parçası olmayan sütunları varsa, bu dizinler hedef tabloda sağlanan değil.  
-   XML türü sütunlarındaki dizinler sağlanan değil.  
-   Denetim kısıtlamalarında sağlanan değil.  
-   Kaynak tablolarda varolan Tetikleyiciler sağlanan değil.  
-   Görünümleri ve saklı yordamları da hedef veritabanı üzerinde oluşturulmaz.

#### <a name="recommendations"></a>Öneriler

-   Yalnızca hizmetin ölçeğini çalışırken SQL veri eşitleme (Önizleme) autoprovisioning yetenek kullanın.  
-   Üretim için veritabanı şeması sağlayın.

### <a name="locate-hub"></a> Hub veritabanı yerleştireceğinizi

#### <a name="enterprise-to-cloud-scenario"></a>Kurumsal bulut senaryosu

Gecikmeyi en aza indirmek için eşitleme grubun veritabanı trafiğini en büyük yoğunluğu yakın hub veritabanı tutun.

#### <a name="cloud-to-cloud-scenario"></a>Bulut Bulut senaryosu

-   Bir eşitleme grubundaki tüm veritabanlarının bir veri merkezinde olduğunda, hub aynı veri merkezinde bulunmalıdır. Bu yapılandırma, gecikme süresi ve veri merkezleri arasında veri aktarımı maliyeti azaltır.
-   Eşitleme grubunu veritabanları birden çok veri merkezlerinde olduğunda hub veritabanları ve veritabanı trafiğinin çoğunluğu ile aynı veri merkezinde bulunmalıdır.

#### <a name="mixed-scenarios"></a>Karma senaryolar

Kurumsal Bulut ve Bulut Bulut senaryoları bir karışımını olanlar gibi karmaşık eşitleme grubu yapılandırmaları için yukarıdaki yönergeleri uygulayın.

## <a name="sync"></a>Sync

### <a name="avoid-a-slow-and-costly-initial-synchronization"></a> Yavaş veya pahalı ilk eşitleme kaçının

Bu bölümde, bir eşitleme grubundaki ilk eşitleme tartışın. Uzun ve gerekenden daha pahalı bir başlangıç eşitlemesi önlemeye yardımcı olmak öğrenin.

#### <a name="how-initial-sync-works"></a>Nasıl ilk eşitleme çalışır

Bir eşitleme grubu oluşturduğunuzda, yalnızca bir veritabanındaki verilere başlayın. Birden çok veritabanlarında veri varsa, SQL veri eşitleme (Önizleme) her satır çözülmesi gereken bir çakışma değerlendirir. Bu çakışma çözümü yavaş gitmek ilk eşitleme neden olur. Birden çok veritabanlarında veri varsa, ilk eşitleme birkaç gün ve veritabanı boyutuna bağlı olarak birkaç ay arasında sürebilir.

Veritabanları farklı veri merkezlerinde varsa, her satır arasında farklı veri merkezlerinde geçmelidir. Bu bir başlangıç eşitlemesi maliyetini artırır.

#### <a name="recommendation"></a>Öneri

Mümkünse, veri eşitleme grubun veritabanları yalnızca birinde başlayın.

### <a name="design-to-avoid-synchronization-loops"></a> Eşitleme döngüleri önlemek için Tasarım

Bir eşitleme döngüsü döngüsel başvuru bir eşitleme grubu içinde olduğunda oluşur. Bu senaryoda, her değişiklik bir veritabanında sonsuz ve döngüsel eşitleme grubunu veritabanları arasında çoğaltılır.   

Çünkü, performans azalmasına yol ve maliyetleri önemli ölçüde artabilir eşitleme döngüleri kaçının emin olun.

### <a name="handling-changes-that-fail-to-propagate"></a> Yayılmasına başarısız değişiklikler

#### <a name="reasons-that-changes-fail-to-propagate"></a>Değişiklikleri yaymak için başarısız nedenler

Değişiklikler, aşağıdaki nedenlerden birinden dolayı yaymak başarısız olabilir:

-   Şema/datatype uyumsuzluğu.
-   Null sütun null ekleniyor.
-   Yabancı anahtar kısıtlamaları ihlal etme.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Değişiklikleri yaymak başarısız olduğunda ne olur?

-   Eşitleme içinde grup gösterir bir **uyarı** durumu.
-   Ayrıntılar portal UI günlük Görüntüleyicisi'nde listelenir.
-   Sorunu 45 gün giderilmezse veritabanı eski haline gelir.

> [!NOTE]
> Bu değişiklikler hiçbir zaman yayılır. Bu senaryoda kurtarmak için tek yolu eşitleme grubunu yeniden oluşturmaktır.

#### <a name="recommendation"></a>Öneri

Portal ve günlük arabirimi üzerinden düzenli aralıklarla eşitleme grubu ve veritabanı durumunu izleyin.


## <a name="maintenance"></a>Bakım

### <a name="avoid-out-of-date-databases-and-sync-groups"></a> Güncel olmayan veritabanlarını önlemek ve grupları Eşitle

Bir eşitleme grubu veya bir eşitleme grubu veritabanında, eski haline gelebilir. Bir eşitleme grubun durum olduğunda **güncel**, çalışmayı durdurur. Bir veritabanının durumu olduğunda **güncel**, verileri kaybolabilir. Bu durumdan kurtulmanın denemek yerine bu senaryonun olmaması en iyisidir.

#### <a name="avoid-out-of-date-databases"></a>Güncel olmayan veritabanlarını kaçının

Bir veritabanının durumunu ayarlamak **güncel** zaman onu süredir çevrimdışı 45 gün veya daha fazla bilgi için. Önlemek için bir **güncel** durumu bir veritabanında veritabanlarının hiçbiri 45 gün veya daha fazla bilgi için çevrimdışı olduğundan emin olun.

#### <a name="avoid-out-of-date-sync-groups"></a>Güncel olmayan eşitleme grubu kaçının

Bir eşitleme grubun durumunu ayarlamak **güncel** eşitleme grubundaki herhangi bir değişiklik başarısız olduğunda 45 gün veya daha fazla bilgi için eşitleme grubunu kalanına yaymak. Önlemek için bir **güncel** bir eşitleme grubu durumunu düzenli aralıklarla eşitleme grubun geçmiş günlüğü'nü denetleyin. Tüm çakışmalar çözümlenir ve değişiklikler eşitleme grubu veritabanları başarıyla yayılır emin olun.

Eşitleme grubu şunlardan biri nedeniyle bir değişikliği uygulamak başarısız olabilir:

-   Tablolar arasında şema uyumsuzluğu.
-   Tablolar arasında veri uyumsuzluğu.
-   Null değerlere izin vermeyecek bir sütunda null değerine sahip bir satır ekleme.
-   Bir satır yabancı anahtar kısıtlamasını ihlal eden bir değer ile güncelleştiriliyor.

Güncel olmayan eşitleme grubu önlemek için:

-   Şemanın başarısız satırları bulunan değerlere izin verecek şekilde güncelleştirin.
-   Bulunan değerleri başarısız satırları dahil etmek için yabancı anahtar değerlerini güncelleştirin.
-   Şema veya hedef veritabanındaki yabancı anahtarlar ile uyumlu olacak şekilde başarısız satırda veri değerlerini güncelleştirin.

### <a name="avoid-deprovisioning-issues"></a> Sorunları sağlamayı kaçının

Bazı durumlarda, bir istemci Aracısı ile bir veritabanı kaydını eşitleme başarısız olmasına neden olabilir.

#### <a name="scenario"></a>Senaryo

1. Eşitleme Grubu A, bir SQL veritabanı örneği ve yerel Aracı 1 ile ilişkili bir şirket içi SQL Server veritabanı kullanılarak oluşturulmuş.
2. (Bu aracı, herhangi bir eşitleme grubu ile ilişkili değil) yerel aracı 2 ile aynı şirket içi veritabanına kaydedilir.
3. Şirket içi veritabanı kaydını yerel Aracısı'ndan izleme 2 kaldırır ve Grup A şirket içi veritabanı için meta tablolar için eşitleme.
4. Eşitleme grubu bu hata bir işlem başarısız olur: "geçerli işlem eşitleme için veritabanı sağlanmayan veya eşitleme yapılandırması tablolar için izniniz yok olduğundan tamamlanamadı."

#### <a name="solution"></a>Çözüm

Bu senaryonun olmaması için bir veritabanı ile birden fazla aracı kayıt yok.

Bu senaryodan kurtarmak için:

1. Veritabanına ait her eşitleme grubundan kaldırın.  
2. Veritabanı buradan kaldırdığınız her eşitleme grubuna geri ekleyin.  
3. (Bu işlem veritabanı sağlar) her etkilenen eşitleme grubu Dağıt.  

### <a name="modifying-your-sync-group"></a> Eşitleme grubunu değiştirme

Bir veritabanını bir eşitleme grubundan kaldırmanız ve eşitleme grubu değişiklikleri dağıtma birincisini olmadan Düzenle girişiminde yok.

Bunun yerine, önce bir veritabanı eşitleme grubundan kaldırın. Sonra değişikliğin dağıtılması ve tamamlanması sağlamayı için bekleyin. Sağlama kaldırma tamamlandığında, eşitleme grubunu düzenlemek ve değişiklikleri dağıtın.

Bir veritabanını kaldırın ve ardından eşitleme grubu değişiklikleri dağıtma birincisini olmadan düzenleyin çalışırsanız, bir veya başka bir işlem başarısız olur. Portal arabiriminde tutarsız hale gelebilir. Bu olursa, doğru duruma geri yüklemek için sayfayı yenileyin.

## <a name="next-steps"></a>Sonraki adımlar
SQL veri eşitleme (Önizleme) hakkında daha fazla bilgi için bkz:

-   [Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme)](sql-database-sync-data.md)
-   [Azure SQL veri eşitleme (Önizleme) ayarı](sql-database-get-started-sql-data-sync.md)
-   [Günlük analizi ile İzleyici Azure SQL veri eşitleme (Önizleme)](sql-database-sync-monitor-oms.md)
-   [Azure SQL veri eşitleme (Önizleme) ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)  
-   SQL veri eşitleme (Önizleme) yapılandırma Göster PowerShell örnekleri tamamlayın:  
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [SQL veri eşitleme (Önizleme) REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)  

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
