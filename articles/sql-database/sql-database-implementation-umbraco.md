---
title: Azure SQL veritabanı Azure örnek olay incelemesi - Umbraco | Microsoft Docs
description: SQL veritabanı Umbraco hızlıca sağlamak için nasıl kullandığını ve bulutta kiracılar binlerce ölçek hizmetleri hakkında bilgi edinin
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: cd3952e92d09eef7c8b6fd8ec9352bd54dde0389
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649984"
---
# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco, kiracılar bulutta binlerce için hızlı sağlama ve ölçek Hizmetleri için Azure SQL veritabanı kullanır.
![Umbraco logosu](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco herhangi bir şey küçük kampanya ya da broşür sitelerinden karmaşık uygulamaları için Fortune 500 şirketleri ve genel medya Web siteleri için çalıştırılabilir bir popüler açık kaynaklı içerik yönetim sistemidir (CMS) ' dir. 

> "Umbraco çalıştıran 100. 000'den fazla geliştiricilere forumlarımıza ve canlı, birden fazla 350,000 siteleri sistemi kullanan geliştiriciler oldukça büyük bir topluluğu sahibiz."
> 
> — Mahir Christensen, teknik sağlama, Umbraco
> 
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-Umbraco/player]
> 
> 

Müşteri dağıtımları basitleştirmek için Umbraco-bir hizmet olarak (UaaS) Umbraco eklendi: şirket içi dağıtımlar için ihtiyacını ortadan kaldıran bir yazılım olarak-hizmet (SaaS) sunumu yerleşik ölçeklendirme sağlar ve yönetim ek yükü ürün çözüm yerine yenilik Yönetimi odaklanmaya geliştiriciler etkinleştirerek kaldırır. Umbraco, Microsoft Azure tarafından sunulan esnek hizmet olarak platform (PaaS) modelinde bağlı olarak bu avantajlar sağlayabilir.

UaaS SaaS müşterilerin daha önce kendi ulaşma dışında olan Umbraco CMS işlevlerini kullanmanıza olanak verir. Bu müşterilerden üretim veritabanını içeren çalışan CMS ortamı ile sağlanır. Müşteriler, geliştirme ve gereksinimlerine bağlı olarak hazırlama ortamları için en fazla iki ek veritabanları ekleyebilirsiniz. Otomatik bir işlem o müşteri bir veritabanına yeni bir ortam istendiğinde, otomatik olarak atar. Veritabanı zaten Umbraco kullanılabilir veritabanlarının (bkz: Şekil 1) bir Azure esnek havuzdan tarafından önceden sağlanmış olduğundan yeni bir veritabanı saniye cinsinden hazırdır.

![Umbraco sağlama yaşam döngüsü](./media/sql-database-implementation-umbraco/figure1.png)

Şekil 1 '. Hizmet (UaaS) olarak Umbraco için sağlama yaşam döngüsü

## <a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure esnek havuzlar ve Otomasyon dağıtımları basitleştirin
Azure SQL veritabanı ve diğer Azure hizmetleriyle, Umbraco müşteriler ortamlarını otomatik olarak sağlayabilir ve Umbraco kolayca izleyebilir ve sezgisel bir iş akışının bir parçası veritabanları yönetebilirsiniz:

1. Sağlama
   
   Umbraco 200 kullanılabilir önceden sağlanan veritabanlarından esnek havuz kapasitesi tutar. Yeni bir müşteri için UaaS kaydolduğunda Umbraco kullanılabilirlik havuzundan bir veritabanı atayarak yakın gerçek zamanlı yeni bir CMS ortamıyla müşteri sağlar.
   
   Bir kullanılabilirlik havuzu kendi eşiğe ulaştığında yeni bir esnek havuz oluşturulur ve yeni veritabanlarını müşterilere gerekirse atanması önceden hazırlanır.
   
   Uygulama, tamamen C# yönetim kitaplıklarını ve Azure Service Bus kuyruklarını kullanarak otomatik hale getirilmiştir.
2. Kullanma
   
   Müşteriler bir ile üç ortamlarda (üretim, hazırlama ve/veya geliştirme), her biri kendi veritabanı ile kullanın. Müşteri verimli aşırı sağlamak zorunda kalmadan ölçeklendirme sağlamak Umbraco sağlayan esnek havuzlarında veritabanlarıdır.
   
   ![Umbraco projeye genel bakış](./media/sql-database-implementation-umbraco/figure2.png)
   
   ![Umbraco Proje Ayrıntıları](./media/sql-database-implementation-umbraco/figure3.png)
   
   Şekil 2. Umbraco-bir hizmet olarak (UaaS) müşteri Web sitesi, projeye genel bakış ve ayrıntılarını gösterme
   
   Azure SQL veritabanı, veritabanı işlem birimleri (Dtu'lar) gerçek veritabanı işlemleri için gereken göreli güç göstermek için kullanır. UaaS müşteriler için veritabanları, genelde yaklaşık 10 Dtu'lar çalışır, ancak her ölçeğini isteğe bağlı olarak esnekliğine sahiptir. UaaS müşteriler gerekli kaynaklar, yoğun zamanlarında bile her zaman sahip olduğunuzdan emin olabilirsiniz anlamına gelir. Örneğin, bir son Pazar gece Spor olayı sırasında bir UaaS müşteri deneyimli veritabanı en fazla 100 Dtu'lar oyun süresince tarafı. Azure esnek havuzlar bu yüksek talep performans düşüşü olmadan desteklemek Umbraco yapılan.
3. İzleme
   
   Umbraco monitör etkinliği panolar Azure portalındaki özel e-posta uyarıları ile birlikte kullanarak veritabanı.
4. Olağanüstü durum kurtarma
   
   Azure iki olağanüstü durum kurtarma (DR) seçenekleri sağlar: etkin coğrafi çoğaltma ve coğrafi geri yükleme. Bir şirket seçmelisiniz DR seçeneği bağlıdır, [iş sürekliliği hedefleri](sql-database-business-continuity.md).
   
   Aktif coğrafi çoğaltma hızlı yanıt düzeyi kapalı kalma süresi durumunda sağlar. Aktif coğrafi çoğaltma kullanarak, farklı bölgelerdeki sunucular üzerinde en fazla dört okunabilir ikinciller oluşturabilirsiniz ve yük devretme ikincil kopya herhangi bir arıza olması durumunda sonra başlatabilirsiniz.
   
   Umbraco coğrafi çoğaltma gerektirmez, ancak Azure coğrafi en düşük kapalı kalma süresi bir kesinti durumunda sağlamaya yardımcı olmak için geri yükleme avantajlarından yararlanmak. coğrafi geri yükleme veritabanı yedeklemeleri coğrafi olarak yedekli Azure depolama alanında kullanır. Kullanıcıların birincil bölgede bir kesinti olduğunda bir yedek kopyadan geri olanak tanır.
5. Devre dışı bırakma sağlama
   
   Bir proje Ortamı silindiğinde, ilişkili tüm veritabanları (geliştirme, hazırlama ya da Canlı) Azure Service Bus kuyruğu temizleme sırasında kaldırılır. Bu otomatik bir işlem kullanılmayan veritabanları Umbraco'nun esnek veritabanı kullanılabilirlik havuzu, en iyi koruyarak gelecekteki sağlama için kullanılabilir hale getirme geri yükler.

## <a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Esnek havuzlar kolayca ölçeklendirmek UaaS izin ver
Azure esnek havuzlar yararlanarak, Umbraco eksik veya eksik sağlamak zorunda kalmadan, müşterilerinin performansını iyileştirebilirsiniz. Umbraco hemen 3000 veritabanlarını kolayca kendi mevcut 325,000 müşteriler veya bulutta bir CMS dağıtmaya hazır yeni müşteriler uyum sağlamak için gerektiği şekilde ölçekleyebilirsiniz olanağı 19 esnek havuzlar arasında şu anda yok.

Aslında, Mahir Christensen, teknik neden Umbraco, en uygun "UaaS şimdi günde yaklaşık 30 yeni müşteriler büyümesini yaşıyor. Müşterilerimizin saniye cinsinden yeni projeler sağlamak, güncelleştirmeleri Canlı sitelerinin 'tek tıklatmayla dağıtım' kullanarak geliştirme ortamından anında yayımlamak için kullanım mutlu ve hatalar bulursanız gibi hızlı bir şekilde değişiklik."

Bir müşteri ikinci ve/veya üçüncü ortamı artık gerektirmiyorsa, yalnızca o ortamları kaldırabilirsiniz. Diğer müşteriler için Umbraco esnek veritabanı kullanılabilirlik havuzu bir parçası olarak kullanılabilir kaynakları serbest bırakır.

![Umbraco dağıtım mimarisi](./media/sql-database-implementation-umbraco/figure4.png)

Şekil 3 '. Microsoft Azure üzerinde UaaS dağıtım mimarisi

## <a name="the-path-from-datacenter-to-cloud"></a>Bulut için veri merkezi yolundan
Umbraco geliştiriciler için bir SaaS modeli geçmeye karar başlangıçta yapıldığında, bunlar servisi oluşturmak için uygun maliyetli ve ölçeklenebilir bir yol gerekir biliyorduk.

> "esnek havuzlar kapasite yukarı ve aşağı gerektiğinde arama yapmadan çünkü SaaS sunumu için mükemmel var. Sağlama kolaydır ve bizim Kurulum'a biz kullanımı en tutabilirsiniz."
> 
> — Mahir Christensen, teknik sağlama, Umbraco
> 
> 

"Biz müşterilerimizin sorunlarını çözme hakkında bizim süre beklemesini altyapısını yönetmeye değil istedik. En yüksek değeri, almak müşterilerimizin kolaylaştırmak istedi"Niels Hartvi'den Umbraco kurucusu söyler. "Biz başlangıçta barındırma sunucuları kendisini denendi, ancak kapasite planlaması bir onarımı kabus olacaktı." Tesadüfen Umbraco herhangi bir veritabanı yönetici UaaS kullanmak için bir anahtar değer teklifinde, alt çizgi uygulamadığınız değil.

Kapasite sınırlamaları duymadan ve hızla ortamları sağlayacak UaaS müşteriler için bir yol sağlamak için önemli bir hedef Umbraco geliştiriciler için oluştu. Ancak, Umbraco veri merkezlerinde adanmış bir barındırılan hizmet sağlayan çok sayıda işlem WINS'e işlemek için fazlalık kapasite için şunlar gerekir. Düzenli olarak gereğinden az çok miktarda bilgi işlem altyapısı ekleme yapısındaki.

Ayrıca, Umbraco geliştirme ekibi bunları kendi var olan kodu mümkün olduğunca çok yeniden belirleyebilmesini bir çözüm istedik. Umbraco geliştirici olarak Mikkel Madsen durumları, "biz biz zaten Microsoft SQL Server, Microsoft Azure SQL veritabanı, ASP.net ve Internet Information Services (IIS) gibi bilmiyorsanız, Microsoft geliştirme araçları ile mutluluk. Bir Iaas veya bir PaaS yatırım yapmadan önce bulut çözümü, böylece biz yoğun bizim kod için temel değişiklik zorunda olmayacaktır, bizim Microsoft araçları ve platformları, desteği olduğundan emin olun istedik. "

Tüm ölçütlerini karşılamak üzere bulut iş ortağı ile aşağıdaki nitelikleri için Umbraco Aranan:

* Yeterli kapasitesi ve güvenilirliği
* Böylece Umbraco mühendisleri tamamen geliştirme ortamlarını reinvent zorunda kalabilirsiniz değil Microsoft geliştirme araçları için destek
* Tüm UaaS (bunlar verilerine hızlı bir şekilde erişebilmesini ve verilerini bölgesel yasal düzenleme gereksinimlerine uyan bir konumda depolandığını emin olmak için işletmelerin gerek duyduğu) rekabet coğrafi pazarda bulunması

## <a name="why-umbraco-chose-azure-for-uaas"></a>Neden Azure Umbraco UaaS için seçtiğiniz
"Bizim seçenekleri düşünüldükten sonra seçtik Azure tüm bizim ölçütlerden, yönetilebilirlik ve ölçeklenebilirlik benzerlik ve düşük maliyet karşılanır çünkü. Mahir Christensen according Biz Azure vm'lerinde ortamları ayarlama ve her bir ortama esnek havuzlar tüm örnekleri kendi Azure SQL veritabanı örneğine sahip. Veritabanları geliştirme, hazırlama ve canlı ortamlar arasında ayırarak sağlam performans yalıtımı eşleşen Ölçekle müşterilerimizin sunuyoruz — büyük kazanım. "

Mahir devam önce "biz web veritabanları için sunucuları el ile sağlamak zorunda kaldı. Şimdi, biz hakkında düşünmek yok. Her şeyi otomatik olarak — temizleme işlemine hazırlama alanından. "

Mahir de Azure tarafından sağlanan ölçeklendirme özelliklerine sahip memnun olur. "esnek havuzlar kapasite yukarı ve aşağı gerektiğinde arama yapmadan çünkü SaaS sunumu için mükemmel var. Sağlama kolaydır ve bizim Kurulum'a biz kullanımı en tutabilirsiniz." Mahir durumları, "hizmet katmanı tabanlı Dtu'lar güvencesi ile birlikte esnek havuzlar basitliği bize, isteğe bağlı yeni kaynak havuzları sağlamak için güç'sağlar. Yakın zamanda, büyük müşterilerimizin birini Canlı ortamında 100 Dtu'lar için si. Azure kullanarak, bizim esnek havuz DTU gereksinimlerini tahmin etmek zorunda kalmadan gerçek zamanlı olarak gereken kaynaklar ile müşterinin veritabanlarını sağlamıştır. Basitçe, müşterilerimizin bekledikleri çevirme süresinin alın ve biz bizim performansı hizmet düzeyi sözleşmelerinin karşılayabilir."

Mikkel Madsen onu toplar: "Biz yaygın bir SaaS senaryo (gerçek zamanlı ölçekte ekleme yeni müşteriler) bağlanan güçlü Azure algoritması bizim uygulama düzeni (veritabanları, her iki geliştirme önceden sağlama ve canlı) (Azure SQL Database ile birlikte Azure Service Bus kuyruklarını kullanan) temel alınan teknoloji üstünde embraced."

## <a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Azure ile müşteri beklentilerini UaaS aşıyor
Azure, bulut iş ortağı olarak seçerek bu yana Umbraco UaaS müşterilerin en iyi duruma getirilmiş içerik yönetimi performans ile kendini barındıran bir çözümden gerekli BT kaynak yatırım olmadan sağlayabilir olmuştur. Mahir diyor gibi "biz geliştiriciye kolaylık sağlamak ve Azure bize veriyor ölçeklenebilirlik memnuniyet ve müşterilerimizin özellikleri ve güvenilirliği ile thrilled. Genel olarak, bunu bize için harika bir win olmamıştı!"

## <a name="more-information"></a>Daha fazla bilgi
* Azure esnek havuzları hakkında daha fazla bilgi için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
* Azure Service Bus hakkında daha fazla bilgi için bkz: [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Sanal ağlar hakkında daha fazla bilgi için bkz: [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/).    
* Yedekleme ve kurtarma hakkında daha fazla bilgi için bkz: [iş sürekliliği](sql-database-business-continuity.md).    
* Ppols izleme hakkında daha fazla bilgi için bkz: [havuzları izleme](sql-database-elastic-pool-manage-portal.md).    
* Bir hizmet olarak Umbraco hakkında daha fazla bilgi için bkz: [Umbraco](https://umbraco.com/cloud).

