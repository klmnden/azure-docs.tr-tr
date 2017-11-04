---
title: "Azure SQL veri eşitleme en iyi uygulamalar | Microsoft Docs"
description: "Yapılandırma ve Azure SQL veri eşitleme çalıştırmak için en iyi yöntemleri öğrenin"
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6101dfa4bc74acf5045975f6513886fa135fe833
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="best-practices-for-sql-data-sync"></a>SQL veri eşitleme için en iyi yöntemler 

Bu makalede SQL veri eşitleme (Önizleme) için en iyi uygulamaları açıklar.

## <a name="security-and-reliability"></a>Güvenlik ve güvenilirlik

### <a name="client-agent"></a>İstemci Aracısı

-   Ağ hizmeti erişim ile en az ayrıcalık hesabı kullanarak istemci aracısı yükleyin.

-   İstemci aracısının şirket içi SQL Server bilgisayardan ayrı bir bilgisayara yüklenirse en iyisidir.

-   Bir şirket içi veritabanına birden fazla aracı ile kaydetmez.

-   Farklı eşitleme grupları için farklı tablolara eşitleniyor olsa bile.

-   Bir şirket içi veritabanına birden çok istemci aracıları üzerinden zorluklar eşitleme gruplarından birini silerken kaydediliyor.

### <a name="database-accounts-with-least-required-privilege"></a>En az gerekli ayrıcalığa sahip veritabanı hesapları

-   **Eşitleme Kurulumu için**. Tablo Oluştur/Değiştir, Alter veritabanı, yordam, Seç / Alter Schema, kullanıcı tanımlı tür oluşturun.

-   **Devam eden eşitleme için**. Seçin / ekleme / güncelleştirme / meta verileri Eşitle ve tabloları izleme ve eşitleme için seçilen tablolar üzerinde silin, kullanıcı tanımlı tablo türlerinde Execute izni hizmeti tarafından oluşturulan saklı yordamları çalıştırma izni.

-   **XML'deki sağlama**. ALTER eşitleme parçası tablolar üzerinde seçin / meta verileri Eşitle tablolarda, eşitleme izleme tabloları, saklı yordamları ve kullanıcı tanımlı türler izleme eşitleme denetiminde Sil.

**Yalnızca tek bir kimlik eşitleme gruptaki bir veritabanı için olduğunda bu bilgileri nasıl kullanabileceğiniz?**

-   Farklı aşamalarında için kimlik bilgilerini değiştirin (örneğin, credential1 kurulumu ve credential2 için devam eden).

-   Değiştirme izni kimlik bilgilerinin (eşitleme ayarlandıktan sonra başka bir deyişle, değiştirme izni).

## <a name="locate-hub"></a>Hub veritabanı yerleştireceğinizi

### <a name="enterprise-to-cloud-scenario"></a>Kurumsal bulut senaryosu

Gecikmeyi en aza indirmek için eşitleme grubun veritabanı trafiğini en büyük yoğunluğu yakın hub veritabanı tutun.

### <a name="cloud-to-cloud-scenario"></a>Bulut Bulut senaryosu

-   Bir eşitleme grubundaki tüm veritabanlarının bir veri merkezinde olduğunda hub aynı veri merkezinde bulunmalıdır. Bu yapılandırma, gecikme süresi ve veri merkezleri arasında veri aktarımı maliyeti azaltır.

-   Eşitleme grubunu veritabanları birden çok veri merkezlerinde olduğunda hub veritabanları ve veritabanı trafiğinin çoğu aynı veri merkezinde bulunmalıdır.

### <a name="mixed-scenarios"></a>Karma senaryolar

Yukarıdaki yönergeleri daha karmaşık eşitleme grubu yapılandırmaları için geçerlidir.

## <a name="database-considerations-and-constraints"></a>Veritabanı konuları ve kısıtlamaları

### <a name="sql-database-instance-size"></a>SQL Database örnek boyutu

Yeni bir SQL veritabanı örneği oluşturduğunuzda, en büyük boyutu her zaman dağıttığınız veritabanından daha büyük olduğu şekilde ayarlayın. En büyük boyutu dağıtılan veritabanı büyük ayarlamayın eşitleme başarısız olur. Hiçbir otomatik büyüme - varken bir ALTER oluşturulduktan sonra veritabanı boyutunu artırmak için veritabanı yapabilirsiniz. SQL veritabanı örneği boyutu sınırları içinde kalmasını sağlayın.

> [!IMPORTANT]
> SQL veri eşitleme her veritabanı ile ek meta verileri depolar. Bu meta veriler için gereken alanı hesaplarken hesap emin olun. Miktarını eklenen ek yükü tabloları genişliği tarafından yönetilir (örneğin, daha fazla ek yükü dar tablolarda gereklidir) ve trafik miktarı.

## <a name="table-considerations-and-constraints"></a>Tablo konuları ve kısıtlamaları

### <a name="selecting-tables"></a>Tabloları seçme

Veritabanındaki tabloların tümü olduğu için gereken bir [eşitleme grubu](#sync-group). Bir eşitleme grubuna eklemek için tablolar ve hariç tutma (veya farklı bir eşitleme grubuna eklemek için) seçimini verimliliği ve maliyetleri etkileyebilir. Yalnızca bu tablolar eşitleme iş isteğe bağlı ve bağlı bağımlı oldukları tabloları gereksinimleriniz grubunda içerir.

### <a name="primary-keys"></a>Birincil anahtarlar

Her bir eşitleme grubu tablosunda birincil anahtar olması gerekir. SQL veri eşitleme (Önizleme) hizmeti bir birincil anahtara sahip olmadığı herhangi bir tablo eşitleyemedi.

Üretime çalışırken önce ilk ve devam eden eşitleme performansını test edin.

## <a name="provisioning-destination-databases"></a>Hedef veritabanı sağlama

SQL veri eşitleme (Önizleme) önizleme temel veritabanı otomatik sağlama sağlar.

Bu bölümde, SQL veri eşitleme sınırlamaları anlatılmaktadır (Önizleme) sağlama.

### <a name="auto-provisioning-limitations"></a>Otomatik sınırlamaları sağlama

SQL veri eşitleme (Önizleme) otomatik sağlama sınırlamaları şunlardır:

-   Yalnızca seçili sütun hedef tabloda oluşturulur.
Bu nedenle, bazı sütunları eşitleme grubunun parçası değilse bu sütunları hedef tablolarında sağlanmayan.

-   Dizinler yalnızca seçilen sütunlar için oluşturulur.
Kaynak tablo dizin eşitleme grubunun parçası olmayan sütunları varsa bu dizinler hedef tabloda sağlanan değil.

-   XML türü sütunlarındaki dizinler sağlanmayan.

-   Denetim kısıtlamalarında sağlanmayan.

-   Kaynak tablolarda varolan Tetikleyiciler sağlanmayan.

-   Görünümleri ve saklı yordamlar hedef veritabanı oluşturulmadı.

### <a name="recommendations"></a>Öneriler

-   Otomatik sağlama yeteneği, yalnızca hizmet denemek için kullanın.

-   Üretim için veritabanı şeması hazırlamanız.

## <a name="avoid-a-slow-and-costly-initial-synchronization"></a>Yavaş ve pahalı bir ilk eşitleme kaçının

Bu bölümde, bir eşitleme grubu ve gerekenden daha fazla gerekli ve maliyetlendirme daha uzun süren bir ilk eşitleme önlemek için neler yapabileceğinizi ilk eşitleme anlatılmaktadır.

### <a name="how-initial-synchronization-works"></a>Nasıl ilk eşitleme çalışır

Bir eşitleme grubu oluşturduğunuzda, yalnızca bir veritabanındaki verilere başlayın. Birden çok veritabanlarında veri varsa, SQL veri eşitleme (Önizleme) her satır çözümlemesi gereken bir çakışma değerlendirir. Bu çakışma çözümü, veritabanı boyutuna bağlı olarak birkaç ay için birkaç gün alma yavaş gitmek ilk eşitleme neden olur.

Ayrıca, veritabanları farklı veri merkezlerinde varsa, her satır farklı veri merkezleri arasında geçmelidir bu yana ilk eşitleme maliyetlerini gerekenden daha yüksek.

### <a name="recommendation"></a>Öneri

Olası her başlattığınızda eşitleme grubun veritabanları yalnızca biri veriye sahip.

## <a name="design-to-avoid-synchronization-loops"></a>Eşitleme döngüleri önlemek için Tasarım

Bir eşitleme döngüsü olduğunda bir eşitleme grubu içinde döngüsel başvurulara böylece her değişiklik bir veritabanında eşitleme grubunu veritabanları arasında döngüsel ve sonsuz çoğaltılır sonuçlanır. Performansı düşebilir ve maliyetleri önemli ölçüde artırabilir eşitleme döngüleri önlemek istiyor.

## <a name="avoid-out-of-date-databases-and-sync-groups"></a>Güncel olmayan veritabanlarını önlemek ve grupları Eşitle

Bir eşitleme grubunu veya veritabanını bir eşitleme grubundaki güncel hale gelebilir. Bir eşitleme grubun durumu "süresi geçmiş" olduğunda, çalışmayı durdurur. Bir veritabanının durumu "süresi geçmiş" olduğunda, veriler kaybolmuş olabilir. Bu durumlarla karşılaşmamak yerine bunları geri yüklemeniz en iyisidir.

### <a name="avoid-out-of-date-databases"></a>Güncel olmayan veritabanlarını kaçının

Bunu 45 gün veya daha fazla bilgi için çevrimdışı kaldığında veritabanının durumu güncel ayarlanır. Bir veritabanı güncel değil durumu veritabanlarının hiçbiri 45 gün veya daha fazla bilgi için çevrimdışı olduğunu sağlayarak kaçının.

### <a name="avoid-out-of-date-sync-groups"></a>Güncel olmayan eşitleme grubu kaçının

Eşitleme grubunu geri kalanı için 45 gün veya daha fazla bilgi için yaymak eşitleme grubu dahilinde herhangi bir değişiklik başarısız olduğunda bir eşitleme grubun durum güncel ayarlanır. Eşitleme grubu güncel değil durumu, düzenli aralıklarla eşitleme grubun Geçmiş günlüğünü denetleyerek kaçının. Tüm çakışmalar çözülür ve değişiklikleri başarıyla eşitleme grubu veritabanları yayılan emin olun.

Eşitleme grubunu bir değişikliği uygulamak başlatılamayabilir nedenler şunlardır:

-   Tablolar arasında şema uyumsuzluğu.

-   Tablolar arasında veri uyumsuzluğu.

-   Null değerlere izin vermiyor bir sütunda null değerine sahip bir satır ekleme.

-   Bir satır yabancı anahtar kısıtlamasını ihlal eden bir değer ile güncelleştiriliyor.

Güncel olmayan eşitleme grubu tarafından engel olabilirsiniz:

-   Şemanın başarısız satırları bulunan değerlere izin verecek şekilde güncelleştirin.

-   Başarısız satırları bulunan değerleri dahil etmek için yabancı anahtar değerlerini güncelleştirin.

-   Veri değerlerini şeması ile uyumlu olacak şekilde başarısız satırda veya hedef veritabanındaki yabancı anahtarlar güncelleştirin.

## <a name="avoid-deprovisioning-issues"></a>Sorunları sağlamayı kaçının

Belirli koşullar altında bir istemci Aracısı ile bir veritabanı kaydını eşitlemeler başarısız olmasına neden olabilir.

### <a name="scenario"></a>Senaryo

1. Eşitleme Grubu A, bir SQL veritabanı örneği ve yerel Aracı 1 ile ilişkili bir şirket içi SQL Server veritabanı ile oluşturuldu.

2. (Bu aracı, herhangi bir eşitleme grubu ile ilişkili değil) yerel aracı 2 ile aynı şirket içi veritabanına kaydedilir.

3. Yerel aracı 2 şirket içi veritabanından kaydını, şirket içi veritabanı için eşitleme grubu a izleme/meta tabloları kaldırır.

4. Şimdi, şu hata ile eşitleme grubu A işlemler başarısız – "geçerli işlem eşitleme için veritabanı sağlanmayan veya eşitleme yapılandırması tablolar için izniniz yok olduğundan tamamlanamadı."

### <a name="solution"></a>Çözüm

Durum tamamen hiçbir zaman bir veritabanı ile birden fazla aracı kaydederek kaçının.

Bu durumdan kurtarmak için:

1. Veritabanına ait her eşitleme grubundan kaldırın.

2. Veritabanını geri bundan yalnızca kaldırılan her eşitleme Grubu'na ekleyin.

3. (Veritabanı sağlar) her etkilenen eşitleme grubunu dağıtın.

## <a name="handling-changes-that-fail-to-propagate"></a>Yayılmasına başarısız değişiklikleri işleme

### <a name="reasons-that-changes-fail-to-propagate"></a>Değişiklikleri yaymak için başarısız nedenler

Değişiklikler pek çok nedeni yaymak başarısız olabilir. Bazı nedenler olacaktır:

-   Şema/Datatype uyumsuzluğu.

-   Null sütun null eklemeye çalışıyor.

-   Yabancı anahtar kısıtlamaları ihlal etme.

### <a name="what-happens-when-changes-fail-to-propagate"></a>Değişiklikleri yaymak başarısız olduğunda ne olur?

-   Eşitleme grubu, bir uyarı durumunda gösterilir.

-   Portal UI Günlüğü Görüntüleyicisi'nde ayrıntılar verilmektedir.

-   Sorunu 45 gün giderilmezse veritabanı eski haline gelir.

> [!NOTE]
> Bu değişiklikler hiçbir zaman yayılır. Kurtarmak için yalnızca eşitleme grubunu yeniden oluşturmak için bir yoludur.

### <a name="recommendation"></a>Öneri

Portal ve günlük arabirimi üzerinden düzenli aralıklarla eşitleme grubu ve veritabanı durumunu izleyin.

## <a name="modifying-your-sync-group"></a>Eşitleme grubunu değiştirme

Bir veritabanını bir eşitleme grubundan kaldırmanız ve eşitleme grubu değişiklikleri dağıtma birincisini olmadan düzenlemek çalışmayın.

İlk olarak, bir veritabanı eşitleme grubundan kaldırın. Sonra değişikliğin dağıtılması ve tamamlamak için XML'deki sağlamak için bekleyin. Bu işlem tamamlandıktan sonra eşitleme grubunu düzenlemek ve değişiklikleri dağıtın.

Bir veritabanını kaldırın ve sonra ilk dağıtmadan eşitleme grubu düzenlemek denerseniz, değişiklikleri, bir veya başka bir işlem başarısız olur ve portal arabiriminde tutarsız bir duruma alabilirsiniz. Bu durumda, doğru duruma geri yüklemek için sayfayı yenileyin.
