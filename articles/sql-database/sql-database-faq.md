---
title: "Azure SQL veritabanı ile ilgili SSS | Microsoft Docs"
description: "Ortak sorular müşteriler yanıtlarını bulutta bir hizmet olarak bulut veritabanları ve Azure SQL Database, Microsoft'un ilişkisel veritabanı yönetim sistemine (RDBMS) ve veritabanı hakkında isteyin."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 1da12abc-0646-43ba-b564-e3b049a6487f
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 02/12/2018
ms.author: carlrab
ms.openlocfilehash: 4efa053afd26bde208441c4b841c5d02142a2d18
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="sql-database-faq"></a>SQL Veritabanı SSS

## <a name="what-is-the-current-version-of-sql-database"></a>SQL veritabanı'nın geçerli sürümü nedir?
Geçerli SQL veritabanı V12 sürümüdür. Sürüm V11 devre dışı bırakılmış.

## <a name="what-is-the-sla-for-sql-database"></a>SQL veritabanı için SLA nedir?
Müşterilerin, tek ya da esnek Temel, Standart veya Premium Microsoft Azure SQL Veritabanı ile İnternet ağ geçidimiz arasında en az %99,99 düzeyinde bağlantıya sahip olacağını garanti ediyoruz. Daha fazla bilgi için bkz: [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Sunucu Yöneticisi parolasını sıfırlama nasıl?
İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL sunucuları**, listeden sunucuyu seçin ve ardından **parola sıfırlama**.

## <a name="how-do-i-manage-databases-and-logins"></a>Veritabanlarını ve oturum açma bilgileri nasıl yönetebilirim?
Bkz: [veritabanları ve oturum açma bilgileri yönetme](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Yalnızca yetkili IP adreslerini bir sunucuya erişim izni nasıl emin?
Bkz: [nasıl yapılır: SQL veritabanında güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>SQL veritabanı'nın kullanımını faturamı üzerinde nasıl göstermez?
SQL veritabanı faturaları tahmin edilebilir bir saatlik hızı üzerinde temel her iki hizmet katmanında + tek veritabanları veya esnek havuz başına Edtu için performans düzeyi. Gerçek kullanım hesaplanan ve saatlik, faturanızı bir saat kesirlerini gösterebilir. böylece Faturalaması. Örneğin, 12 saat içindeki bir ay için bir veritabanı zaten varsa, faturanıza 0,5 gün kullanımını gösterir. Ayrıca, hizmet katmanları + performans düzeyini ve havuz başına Edtu her bir ay içinde kullanılan veritabanı gün sayısını görmek kolaylaştırmak için fatura içinde bölünmüştür.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Ne tek bir veritabanı değerinden bir saat için etkin veya daha az bir saatten daha yüksek bir hizmet katmanı kullanır?
Faturalandırılır her bir veritabanı yüksek hizmet katmanı kullanarak mevcut saat + kullanım veya veritabanı değerinden bir saat için etkin olup olmadığını bağımsız olarak bu saatte uygulanan performans düzeyi için. Örneğin, tek bir veritabanı oluşturun ve daha sonra beş dakika silerseniz faturanızı bir veritabanı saat için bir ücret yansıtır. 

Örnekler:

* Temel bir veritabanı oluşturun ve ardından hemen standart S1 yükseltin, standart S1 oranı ilk saat için sizden ücret kesilir.
* Bir veritabanı Basic'ten Premium 10: 00'da yükseltirseniz, ve yükseltme 1: 35'da tamamlar 1: 00'da başlayan Premium oranı ücretlendirilen aşağıdaki günü 
* 11: 00'da Premium bir veritabanından temel düşürmek varsa ve 2: 15'da tamamlandıktan sonra veritabanı Premium oranı 3: 00'a kadar geçmesi temel oranlarda doludur doludur.

## <a name="how-does-elastic-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Nasıl esnek Havuz Kullanım faturamı ve değiştirdiğimde ne olacağını havuz başına Edtu görünmesini?
Esnek havuz ücretlendirilen Göster yukarı faturanızda esnek Dtu'lar (Edtu'lar) havuz başına Edtu altında gösterilen artışlarla [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/sql-database/). Esnek havuzlar için veritabanı başına ücret ödemeden yoktur. Bir havuz en yüksek eDTU kullanımı veya havuza değerinden bir saat için etkin olup olmadığını bakılmaksızın bulunduğu her saat için faturalandırılır. 

Örnekler:

* 11: 18'da ile 200 Edtu'lar standart bir esnek havuz oluşturursanız, beş veritabanları havuzuna ekleme, 200 Edtu'lar için tüm saat için 11'da başlayan ücretlendirilirsiniz Günlük geri kalanı ile.
* Gün 2, 5: 05'da, veritabanı 1 50 edtu'larını kullanmaya başladığında ve gün sürekli tutar. Veritabanları 2-5 dalgalanma gösteren 0 ile 80 Edtu'lar arasında. Gün içinde gün boyunca değişen Edtu'lar tüketen beş veritabanları ekleyin. Gün 2 200 eDTU faturalandırılır tam bir günüdür. 
* Sabah üzerinde 3, 5 başka bir 15 veritabanları ekleyin. Burada Edtu'lar 200 havuzundan 400 için 20: 05'da artırmaya karar noktasına gün boyunca veritabanı kullanımı artırır 200 eDTU düzeyinde ücretleri 8 pm kadar yürürlükte olan ve kalan dört saat için 400 Edtu'lar artırır. 

## <a name="elastic-pool-billing-and-pricing-information"></a>Esnek havuz faturalama ve fiyatlandırma bilgileri
Esnek havuzları aşağıdaki özelliklere faturalandırılır:

* Olduğunda bile hiçbir veritabanı havuzundaki bir esnek havuz oluşturulduktan sonra faturalandırılır.
* Bir esnek havuz saatlik faturalandırılır. Bu mod tek veritabanlarının performans düzeyleri için olduğu gibi aynı Ölçüm sıklığıdır.
* Bir esnek havuz Edtu yeni bir sayıya boyutlandırılır, yeniden boyutlandırma işlemi tamamlanana kadar ardından havuz Edtu yeni miktarına göre faturalandırılır değil. Bu, tek veritabanları performans düzeyini değiştirme olarak aynı düzeni izler.
* Bir esnek havuz fiyat havuzunun edtu'larını sayısına bağlıdır. Bir esnek havuz fiyat sayısına ve esnek veritabanları içindeki kullanımını bağımsızdır.
* Fiyat (havuz Edtu sayısı tarafından) hesaplanan x (eDTU fiyatı birimi).

Esnek havuzlar için fiyat eDTU aynı hizmet katmanının tek bir veritabanı için birim DTU fiyatı daha yüksektir. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 

Edtu ve hizmet katmanları anlamak için bkz: [SQL Database seçenekleri ve performansı](sql-database-service-tiers.md).

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Aktif coğrafi çoğaltma faturamı üzerinde bir esnek havuz gösterisini içinde kullanımını nasıl yapar?
Kullanarak tek veritabanlarını aksine [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) esnek veritabanları ile doğrudan bir faturalama etkisi yoktur.  Yalnızca her havuzları (birincil havuzu ve ikincil havuzu) için sağlanan edtu'ları için ücretlendirilirsiniz

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Denetim özelliğinin kullanılması faturamı nasıl etkiler?
Denetim içinde oluşturulan SQL Database hizmeti hiçbir ek maliyet ve temel, standart ve Premium veritabanlarına kullanılabilir. Ancak, denetim günlüklerini depolamak için bir Azure Storage hesabı ve tablolar ve Kuyruklar Azure Storage'da yer Fiyatlardan geçerli denetim özelliğini kullanır, Denetim günlüğü boyutuna göre.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-pools"></a>Tek veritabanları ve esnek havuzlar için doğru hizmet katmanını ve performans düzeyini nasıl bulurum?
Kullanabileceğiniz birkaç araç vardır: 

* Şirket içi veritabanları için [DTU boyutlandırma Danışmanı](http://dtucalculator.azurewebsites.net/) veritabanları ve gerekli Dtu'lar önerilir ve esnek havuzlar için birden çok veritabanı değerlendirmek için.
* Tek bir veritabanı için bir havuzda olmaktan yararlı kullanıyorsanız, garanti eder bir geçmiş kullanımı desen görürse Azure'un akıllı altyapısı bir esnek havuz önerir. Bkz: [İzleyici ve Azure portal ile bir esnek havuzunu yönetme](sql-database-elastic-pool-manage-portal.md). Matematik işini kendiniz yapmak hakkında ayrıntılı bilgi için bkz: [esnek havuzlar için fiyat ve performans konuları](sql-database-elastic-pool.md)
* Tek bir veritabanı yükseltmenize veya düşürmenize gerek olup olmadığını görmek için bkz: [tek veritabanları için performans rehberi](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Tek bir veritabanının Hizmet katmanını veya performans düzeyi ne sıklıkta değiştirebilir miyim?
Hizmet Katmanı (arasında temel, standart ve Premium) veya bir hizmet katmanına (örneğin, S2 S1) içindeki performans düzeyi istediğiniz sıklıkta değiştirebilirsiniz. Önceki sürüm veritabanları için 24 saatlik bir dönemde dört kez toplam hizmet katmanı veya performans düzeyini değiştirebilirsiniz.

## <a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Havuz başına edtu'larını ne sıklıkta göre ayarlayabilirsiniz?
Sıklıkta istiyor.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Ne kadar tek bir veritabanının Hizmet katmanını veya performans düzeyini değiştirmek veya bir veritabanını bir esnek havuz ve bu moddan taşımak için sürer?
Bir veritabanının Hizmet katmanını değiştirmeyi ve bir havuz ve bu moddan taşıma arka plan işlemi olarak platformunda kopyalanacak veritabanı gerektirir. Hizmet katmanını değiştirmeyi birkaç dakika arasında veritabanlarını boyutuna bağlı olarak birkaç saat sürebilir. Her iki durumda da veritabanları taşıma işlemi sırasında çevrimiçi ve kullanılabilir olmaya devam eder. Tek veritabanları değiştirme hakkında daha fazla bilgi için bkz [bir veritabanının Hizmet katmanını değiştirme](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Tek bir veritabanının esnek veritabanları ve ne zaman kullanmalıyım?
Genel olarak, esnek havuzlar için tipik bir tasarlanmış [hizmet olarak yazılım (SaaS) uygulama düzeni](sql-database-design-patterns-multi-tenancy-saas-applications.md), müşteri veya Kiracı başına tek bir veritabanı olduğu. Veritabanlarına göre değişen maksimum gereksinimleri karşılamak için ayrı ayrı veritabanları satın alıp bu veritabanlarını tekrar tekrar sağlamak, ekonomik açıdan çok verimli bir yöntem değildir. Havuzlarıyla, havuzun toplu performansı yönetme ve veritabanlarını yukarı ve aşağı otomatik olarak ölçeklendirin. Kullanım deseniyle garanti eder, azure'nın akıllı altyapısı veritabanları için bir havuz önerir. Ayrıntılar için bkz [esnek havuz Kılavuzu](sql-database-elastic-pool.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Yedekleme depolama alanı için en fazla sağlanan veritabanı alanınızın %200'üne kadar alana sahip olmak ne demektir?
Yedekleme depolama için kullanılan otomatik veritabanı yedeklerinizi ile ilişkili depolama olan [noktası-içinde--geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore) ve [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL Veritabanı herhangi bir ek ücretlendirme olmadan en fazla sağlanan veritabanı depolama alanınızın %200'üne kadar yedekleme alanı sağlar. Örneğin, bir standart DB örneğini sağlanan bir veritabanı boyutu ile 250 GB varsa, 500 GB ek ücret ödemeden yedekleme depolama ile sağlanır. Sağlanan yedekleme depolama veritabanınızı aşarsa, Azure desteği ile iletişim kurarak bekletme süresini azaltın veya standart okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) oranı faturalandırılır fazladan Yedekleme depolaması için ödeme seçebilirsiniz. RA-GRS faturalama hakkında daha fazla bilgi için depolama fiyatlandırma ayrıntıları bakın.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>I Web/iş yeni hizmet katmanları için taşıma 'M ı bilmeniz gerekenleri?
Azure SQL Web ve işletme veritabanları artık kullanımdan kaldırıldı. Temel, standart ve Premium ve esnek katmanları retiring Web ve işletme veritabanları değiştirin. 

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Coğrafi çoğaltma yapılırken bir beklenen çoğaltma gecikmesi nedir aynı Azure Coğrafya içinde iki bölgeler arasında bir veritabanı?
Çoğaltma gecikmesi olmuştur ve biz şu anda beş saniyede bir RPO desteklenmesi değerinden coğrafi ikincil Azure zaman barındırılan eşleştirilmiş bölge önerilen ve aynı hizmet katmanı.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Birincil veritabanı ile aynı bölgede coğrafi ikincil oluşturulduğunda beklenen çoğaltma gecikmesi nedir?
Ampirik verilerini temel alarak yok içi bölge ve arası bölge çoğaltma gecikmesi çok fazla birbirinden eşleştirilmiş bölge kullanılan Azure önerilen olduğunda. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>İki bölgeler arasında bir ağ hatası varsa, coğrafi çoğaltma ayarlarken yeniden deneme mantığı nasıl çalışır?
Bir bağlantıyı kes ise, biz 10 saniyede bağlantıları yeniden oluşturmak için yeniden deneyin.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Birincil veritabanı üzerinde önemli bir değişiklik çoğaltılır güvence altına almak için ne yapabilirim?
Coğrafi ikincil bir zaman uyumsuz çoğaltma ve biz birincil ile tam eşitleme tutmak deneyin değil. Ancak biz önemli değişiklikler (örneğin, parola güncelleştirmeleri) çoğaltma emin olmak için eşitlenmesini zorlamak için bir yöntem sağlar. Tüm kaydedilmiş işlemleri çoğaltılana çağıran iş parçacığı engellediğinden zorlanmış eşitleme performansını etkiler. Ayrıntılar için bkz [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Hangi Araçlar birincil veritabanı arasındaki coğrafi ikincil çoğaltma gecikmesi izlemek kullanılabilir var mı?
Biz, birincil veritabanı ile coğrafi-ikincil bir DMV aracılığıyla arasında gerçek zamanlı çoğaltma gecikmesi kullanıma sunar. Ayrıntılar için bkz [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Bir veritabanını aynı abonelik içinde farklı bir sunucuya taşımak için
İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL veritabanları**, listeden bir veritabanı seçin ve ardından **kopya**. Bkz: [bir Azure SQL veritabanını kopyalama](sql-database-copy.md) daha fazla ayrıntı için.

## <a name="to-move-a-database-between-subscriptions"></a>Bir veritabanını abonelikler arasında taşıma
İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL sunucuları** ve ardından listeden, veritabanını barındıran sunucuyu belirleyin. Tıklatın **taşıma**ve taşımak istediğiniz kaynakları ve taşımak için abonelik seçin.
