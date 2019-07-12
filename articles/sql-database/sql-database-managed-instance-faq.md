---
title: SQL veritabanı yönetilen örneği hakkında SSS | Microsoft Docs
description: SQL veritabanı yönetilen örneği sık sorulan sorular (SSS)
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 07/08/2019
ms.openlocfilehash: c3a070eb7e1435055b47b39985cf8cb0b182a514
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798072"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>SQL veritabanı yönetilen örneği sık sorulan sorular (SSS)

Bu makalede birçok yaygın soruların hakkında içerir [SQL veritabanı yönetilen örneği](sql-database-managed-instance.md).

## <a name="where-can-i-find-a-list-of-features-that-are-supported-on-managed-instance"></a>Yönetilen örneğinde desteklenen özelliklerin bir listesini nerede bulabilirim?

Yönetilen örneğinde desteklenen özelliklerin bir listesi için bkz. [SQL Server yerine Azure SQL veritabanı](sql-database-features.md).

Azure SQL veritabanı yönetilen örneği ile şirket içi SQL Server arasındaki söz dizimi ve davranış farklılıkları için bkz [T-SQL farklılıkları SQL Server'dan](sql-database-managed-instance-transact-sql-information.md).


## <a name="where-can-i-find-technical-characteristics-and-resource-limits-for-managed-instance"></a>Yönetilen örnek için teknik özellikler ve kaynak sınırları nerede bulabilirim?

Kullanılabilir donanım oluşturma özellikleri için bkz: [donanım Nesilleri teknik farklılıkları](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
Kullanılabilir hizmet katmanları ve onların özellikleri için bkz. [hizmet katmanları teknik farklılıkları](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="where-can-i-find-known-issues-and-bugs"></a>Bilinen sorunlar ve hatalar nerede bulabilirim?

Hatalar ve bilinen sorunlar için bkz. [davranış değişiklikleri](sql-database-managed-instance-transact-sql-information.md#Changes) ve [bilinen sorunlar](sql-database-managed-instance-transact-sql-information.md#Issues).


## <a name="can-a-managed-instance-have-the-same-name-as-on-premises-sql-server"></a>Yönetilen örnek, şirket içi SQL Server ile aynı ada sahip olabilir?

Yönetilen örnek ile sonlanan adlara sahip olmalıdır *database.windows.net*. Varsayılan yerine, örneğin, başka bir DNS bölgesini kullanacak şekilde **mı başka bir-ad**. contoso.com: 
- Bir diğer ad tanımlamak için CliConfig kullanın (Bu da yapılabilir yalnızca bir kayıt defteri ayarları sarmalayıcı aracı olduğundan Grup İlkesi veya betik kullanarak).
- Kullanım *CNAME* ile *TrustServerCertificate = true* seçeneği.


## <a name="how-can-i-move-database-from-managed-instance-back-to-sql-server-or-azure-sql-database"></a>Nasıl veritabanı yönetilen örneği geri SQL Server veya Azure SQL veritabanı için taşıyabilirim?

Yapabilecekleriniz [veritabanını bacpac için dışarı aktarma](sql-database-export.md) ardından [bacpac dosyasını içeri aktarma]( sql-database-import.md). Veritabanınızı 100 GB'den küçükse, bu yaklaşım önerilir.

İşlem çoğaltma, veritabanındaki tüm tabloların birincil anahtarları varsa kullanılabilir.

Yerel `COPY_ONLY` yönetilen örneğinden alınan yedeklemeler yönetilen örnek SQL Server'a kıyasla daha yüksek bir veritabanı sürümü olduğundan SQL sunucusuna yüklenemez.

## <a name="how-can-i-migrate-my-instance-database-to-a-single-azure-sql-database"></a>Örnek veritabanını tek bir Azure SQL veritabanına nasıl geçirebilirim?

Bir seçenek olduğu için [veritabanını dışarı aktarma için bir bacpac](sql-database-export.md) ardından [bacpac dosyasını içeri aktarma]( sql-database-import.md). 

Veritabanınızı 100 GB'den küçükse, önerilen yaklaşımdır. İşlem çoğaltma, veritabanındaki tüm tabloların birincil anahtarları varsa kullanılabilir.

## <a name="how-do-i-choose-between-gen-4-and-gen-5-hardware-generation-for-managed-instance"></a>Gen 4 ve 5. nesil donanım oluşturma yönetilen örnek için arasında nasıl seçerim?

Bazı donanım oluşturma diğerinden daha iyi belirli türdeki iş yükleri için olduğu gibi bu, iş yüküne bağlıdır. Performans konusu basitleştirmek için çok karmaşık bir olsa da, aşağıdaki iş yükü performansını etkileyen donanım Nesilleri arasında farklılıklar:
- 4\. nesil sanal çekirdek işlemci üzerinde alan 5. nesil ve fiziksel işlemcilerin şekilde daha iyi bir işlem desteği temel alır sağlar. Bu işlem gücü kullanımlı iş yükleri için daha yararlı olabilir.
- 5\. nesil desteklediği ağ daha iyi bir g/ç bant genişliği uzak depoya kaynaklanan hızlandırılmış. Bu, g/ç kullanımlı iş yükleri üzerinde genel amaçlı hizmet katmanları için avantajlı olabilir. 5\. nesil Gen 4'e kıyasla daha hızlı SSD yerel diskler kullanır. Bu, iş kritik hizmet katmanlarına üzerinde g/ç kullanımlı iş yükleri için avantajlı olabilir.

Müşterilerin önerilir performansını test etmek için üretim çalıştırılmadan önce yönelik gerçek iş yükü, donanım belirlemek için nesil daha iyi sizin durumunuzda çalışır.

## <a name="can-i-switch-my-managed-instance-hardware-generation-between-gen-4-and-gen-5-online"></a>My yönetilen örnek donanımı oluşturma Gen 4 ve 5. nesil arasında çevrimiçi geçiş yapabilir miyim? 

Otomatik çevrimiçi donanım Nesilleri arasında geçiş, hem donanım Nesilleri yönetilen Örneğinize burada sağlandığında aynı bölgede kullanılabiliyorsa mümkündür. Bu durumda, donanım Nesilleri arasında geçiş yapmak için Azure portal'ın fiyatlandırma katmanı bölümündeki bir seçeneğiniz vardır.

Bu uzun, eski ve yeni bir örneği arasında aktarılan yeni yönetilen örnek arka uç ve veritabanı otomatik olarak sağlanacak şekilde işlem çalışıyor. Bu işlem, müşteriler için sorunsuz olacaktır.

Her iki donanım Nesilleri aynı bölgede desteklenmiyorsa, donanım oluşturma değiştirmek mümkündür ancak el ile yapılmalıdır. Bu bölgede yeni bir örneğini sağlamak için istenen donanım oluşturma kullanılabildiği el ile yedekleme ve eski ve yeni örnek arasındaki veri geri yükleme gerektirir.


## <a name="how-do-i-tune-performance-of-my-managed-instance"></a>Yönetilen Örneğim performansını nasıl ayarlama? 

Yönetilen örnek genel amaçlı Performans nedeniyle, veri ve günlük dosyalarının boyutunu önemli Uzaktan Depolama kullanıyor. Genel amaçlı hizmet katmanı performansı ayarlamak için bu blog gönderisinde yönergeleri izleyin.

G/ç kullanımlı iş yükleri için 4. nesil işlem gücü kullanımlı iş yükleri için kullanarak ve 5. nesil donanım kullanmayı düşünün. Daha fazla bilgi, donanım Nesilleri arasında seçme SSS bölümüne bakın.

İş yükünüz küçük hareketlerin çok sayıda oluşuyorsa, yeniden yönlendirme modu için proxy bağlantı türü göz önünde bulundurun.

## <a name="what-is-the-maximum-storage-size-for-managed-instance"></a>Yönetilen örnek için en fazla depolama boyutunu nedir? 

Yönetilen örnek için depolama boyutu, seçilen hizmet Katmanı (genel amaçlı veya iş açısından kritik) bağlıdır. Bu hizmet katmanları depolama sınırlamaları için bkz. [hizmet katmanı özelliği](sql-database-service-tiers-general-purpose-business-critical.md).

## <a name="is-the-backup-storage-deducted-from-my-managed-instance-storage"></a>Yedekleme depolama Alanım yönetilen örnek depolama alanından çıkarılır? 

Hayır, yedekleme depolama yönetilen Örnek Depolama alanınızdan değil düşülür. Yedekleme depolama örneği depolama alanından bağımsızdır ve boyutu sınırlı değildir. Yedekleme depolama alanı için 35 gün 7'den yapılandırılabilir, örnek veritabanları yedeğini tutmak için zaman sınırlıdır. Ayrıntılar için bkz [otomatik yedeklemeler](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
  
## <a name="how-can-i-set-inbound-nsg-rules-on-management-ports"></a>Yönetim bağlantı noktalarına gelen NSG kuralları nasıl ayarlayabilirim?

Yerleşik güvenlik duvarı, yalnızca Microsoft Yönetim/dağıtım makinesi ve etkili bir şekilde önleme güvenli yönetim iş istasyonları için ilişkili IP aralığından gelen bağlantılara izin vermek için kümedeki tüm sanal makineler üzerinde Windows Güvenlik Duvarı'nı yapılandırır. yetkisiz ağ katmanı aracılığıyla.

İşte hangi bağlantı noktaları için kullanılır:

9000 ve 9003 bağlantı noktaları, Service Fabric altyapısı tarafından kullanılır. Service Fabric birincil sanal küme durumunun iyi kalmasını sağlamak ve hedef durumu bileşen çoğaltmaların sayısı bakımından tutmak için rolüdür.

Düğüm aracısı tarafından kullanılan bağlantı noktaları 1438 1440 ve 1452. Düğüm Aracısı kümesi içinde çalışan ve yönetimi komutları yürütmek için Denetim düzlemi tarafından kullanılan bir uygulamadır.

Ek olarak yerleşik güvenlik duvarında ağ katmanı iletişim sertifikaları ile de korunur.
  
Daha fazla bilgi ve Güvenlik Duvarı'nı yerleşik doğrulama için bkz: [Azure SQL veritabanı yönetilen örneği yerleşik güvenlik duvarı](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).


## <a name="how-can-i-mitigate-networking-risks"></a>Ağ riskleri azaltmak ne? 

Tüm ağ riskleri azaltmak için müşterilerin bir dizi güvenlik ayarları ve denetimler uygulamak için önerilir:

- Tüm veritabanlarında saydam veri şifrelemesi (TDE) üzerinde açın.
- Ortak dil çalışma zamanı (CLR) devre dışı bırakın. Bu, şirket içi de önerilir.
- Yalnızca Azure AD hesapları kullanın.
- Düşük ayrıcalıklı DBA hesabıyla SQL mı erişim.
- JIT Sıçrama kutusu erişimini sysadmin hesabını yapılandırın.
- SQL denetimi açın ve uyarı mekanizmalarına ile tümleştirin.
- ATS paketinden tehdit algılamayı etkinleştirme.


## <a name="where-can-i-find-use-cases-and-resulting-cost-savings-with-managed-instance"></a>İle yönetilen örnek kullanım durumları ve sonuçta elde edilen maliyet tasarrufu nerede bulabilirim?

Yönetilen örnek olay incelemeleri:

- [Komatsu](http://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [powerdetails](http://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](http://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Daha iyi bir avantajlar, maliyetler ve Azure SQL veritabanı yönetilen örneği dağıtımıyla ilişkili risklerin'ın anlayış almak için de mevcuttur Forrester'ın olay incelemesi: [Toplam ekonomik etkisi mı](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="can-i-do-dns-refresh"></a>DNS yenileme yapabilirim? 
  
Şu anda şu yönetilen örnek için DNS sunucusu yapılandırmasını yenilemek için bir özellik sunmaz.

DNS yapılandırması sonunda yenilenir:

- DHCP kiralama süresi dolduğunda.
- Platform yükseltme sonrasında.

Geçici bir çözüm olarak yönetilen örneğe 4 sanal çekirdek sürümüne düşürürseniz ve daha sonra yeniden yükseltin. Bu DNS yapılandırmasını yenileme bir yan etkisi vardır.


## <a name="can-a-managed-instance-have-a-static-ip-address"></a>Yönetilen örnek, bir statik IP adresi olabilir mi?

Nadir ancak gereken durumlarda, biz bir yönetilen örneğin yeni bir sanal küme için bir çevrimiçi geçiş yapmanız gerekebilir. Gerekirse, bu güvenliği ve hizmet güvenilirliğini artıran amaçlayan bizim teknoloji yığını'daki değişiklikler nedeniyle geçiştir. Yönetilen örnek konak adıyla eşlenmiş IP adresini değiştirmek, yeni bir sanal küme sonuçları geçirme. Yönetilen örnek hizmeti, statik IP adresi destek talebi değil ve düzenli bakım döngüsü bir parçası olarak bir bildirim olmadan değiştirme hakkını saklı tutar.

Bu nedenle, biz kesinlikle gereksiz kesintilerin neden olabileceğinden IP adresinin değiştirilemezlik üzerinde bağlı olan önerilmemektedir.


## <a name="can-i-change-the-time-zone-for-an-existing-managed-instance"></a>Mevcut bir yönetilen örnek için saat dilimini değiştirebilirim?

Yönetilen örnek için ilk kez sağlandığında saat dilimi yapılandırmasına ayarlanabilir. Mevcut yönetilen örneğinin saat dilimini değiştirme desteklenmiyor. Ayrıntılar için bkz [saat dilimi sınırlamaları](sql-database-managed-instance-timezone.md#limitations).

Geçici Çözümler dahil doğru saat dilimi ile yeni bir yönetilen örnek oluşturma ve daha sonra el ile yedekleme gerçekleştirin ve geri yükleme veya önerilen, bir [arası örnek zaman içinde nokta geri yükleme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/).


## <a name="how-do-i-resolve-performance-issues-with-my-managed-instance"></a>Yönetilen Örneğim ile performans sorunlarını nasıl giderebilirim

Yönetilen örnek SQL Server arasındaki performans karşılaştırması için iyi bir başlangıç noktası olan [en iyi uygulamalar için Azure SQL yönetilen örneği SQL Server arasındaki performans karşılaştırma](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210).

Veri yüklerini zorunlu tam kurtarma modeli nedeniyle SQL Server yönetilen örneği üzerinde daha yavaş genellikle ve [sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) işlem günlüğüne yazma aktarım hızı. Bazı durumlarda, bu geçici bir çözüm yerine bir kullanıcı veritabanı tempdb geçici veri yükleme veya kümelenmiş columnstore veya bellek için iyileştirilmiş tablolar'ı kullanarak çalışılabilmesi.


## <a name="can-i-restore-my-encrypted-database-to-managed-instance"></a>Şifrelenmiş veritabanı yönetilen örneğine geri yükleyebilirim?

Evet, yönetilen örneğe geri getirebilmeniz için veritabanınızı şifresini gerek yoktur. Şifreleme anahtar koruyucusu şifrelenmiş yedekleme dosyasından verileri okuyabilir kaynak sistemindeki yönetilen örnek için kullanılan bir sertifika/anahtarınızı sağlamanız gerekir. Bunu yapmak için olası iki yolu vardır:

- Yönetilen örnek için koruyucu sertifika yükleyin. Bu yapılabilir yalnızca PowerShell kullanarak. Örnek betik, tüm işlemi açıklanmaktadır.
- Asimetrik anahtar koruyucu Azure Key Vault (AKV) karşıya yükleyin ve yönetilen örnek yönlendirebilirsiniz. Bu yaklaşım ayrıca AKV tümleştirme, şifreleme anahtarı depolamak için kullandığı Getir your-kendi-anahtar (BYOK) TDE kullanım örneği benzer. Yalnızca yönetilen örneği, anahtar şifrelemesi anahtar koruyucusu gerçekten kullanmadan şifrelenmiş veritabanlarını geri yükleme için kullanılabilir AKV yüklenen anahtar istiyorsanız, BYOK TDE ' ayarlamak için yönergeleri izleyin ve onay kutusu seçili anahtar yapma seçeneğini işaretlemeyin Varsayılan TDE koruyucusu.

Şifreleme koruyucusu yönetilen örnek için kullanılabilir hale sonra standart veritabanı geri yükleme yordamı ile devam edebilirsiniz.

## <a name="how-can-i-migrate-from-azure-sql-database-single-or-elastic-pool-to-managed-instance"></a>Yönetilen örnek için Azure SQL veritabanı tek veya esnek havuzdan nasıl geçirebilirim? 

Örneği teklifler işlem ve depolama boyutu başına aynı performans düzeyleri Azure SQL veritabanı'nın diğer dağıtım seçenekleri olarak yönetilebilir. Tek bir örneğinde veri birleştirmek istediğiniz veya yalnızca yönetilen örneğinde desteklenen bir özellik yeterlidir, içeri/dışarı aktarma (BACPAC) işlevini kullanarak verilerinizi geçirebilirsiniz.
