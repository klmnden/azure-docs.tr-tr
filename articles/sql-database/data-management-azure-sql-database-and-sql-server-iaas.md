<properties
    pageTitle="SQL (PaaS) Veritabanı ile VM'lerdeki bulutta bulunan SQL Server (IaaS) karşılaştırması | Microsoft Azure"
    description="Hangi bulut SQL Server seçeneğinin uygulamanıza uygun olduğunu öğrenin: Azure SQL (PaaS) Veritabanı veya Azure Virtual Machines'deki bulutta bulunan SQL Server."
    services="sql-database, virtual-machines"
    keywords="SQL Server cloud, SQL Server in the cloud, PaaS database, cloud SQL Server, DBaaS"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/25/2016"
    ms.author="carlrab"/>

# Bir bulut SQL Server seçeneği seçin: Azure SQL (PaaS) Veritabanı ya da Azure VM'lerde SQL Server (IaaS)

Azure, SQL Server iş yüklerini bulutta barındırmaya yönelik iki seçenek içerir:

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): Hizmet olarak yazılım (SaaS) uygulama geliştirme işlemi için iyileştirilmiş olan, hizmet olarak platform (PaaS) veritabanı veya hizmet olarak veritabanı (DBaaS) olarak da bilinen, bulut için yerel olan bir SQL database. SQL Server özelliklerinin çoğunluğu ile uyumluluk sağlar.
* [Azure Virtual Machines'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/): Azure üzerinde çalışan Windows Server Virtual Machines'deki (VM'ler) bulutta yüklü olan ve barındırılan, aynı zamanda hizmet olarak altyapı (IaaS) olarak da bilinen SQL Server.

Her bir seçeneğin Microsoft veri platformuna nasıl uyduğunu öğrenin ve iş gereksinimleriniz için doğru seçeneği bulma konusunda yardım alın. Sizin için maliyet tasarrufu ve minimum yönetim tüm diğer unsurlardan önce geliyorsa bu makale, hangi yaklaşımın en fazla önem verdiğiniz iş gereksinimleri açısından en iyi sonucu verdiği konusunda karar vermenize yardımcı olabilir.


## Microsoft'un veri platformu

Azure ile şirket içi SQL Server veritabanlarının karşılaştırmasına yönelik herhangi bir tartışmada anlaşılması gereken ilk unsur, bunların tümünü kullanabileceğinizdir. Microsoft'un veri platformu, SQL Server teknolojisini kullanır ve fiziksel şirket içi makineler, özel bulut ortamları, üçüncü taraflarca barındırılan özel bulut ortamları ve genel bulutta kullanılabilir kılar. Bu, söz konusu ortamlar genelinde aynı sunucu ürünleri, geliştirme araçları ve uzmanlık kümesin kullanırken, şirket içi ve bulut üzerinde barındırılan dağıtımların bir birleşimi yoluyla benzersiz ve çeşitli iş gereksinimlerini karşılamanıza olanak sağlar.

   ![Bulut SQL Server seçenekleri: IaaS üzerinde SQL sunucusu veya bulutta SaaS veritabanı.](./media/data-management-azure-sql-database-and-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Diyagramda görüldüğü gibi, her bir teklif, altyapı üzerinde sahip olduğunuz yönetim düzeyine (X ekseni üzerinde) ve veritabanı düzeyi birleştirme ve otomasyon işlemleri ile elde edilen maliyet verimliliği düzeyine göre belirlenebilir.

Bir uygulama tasarlarken, uygulamanın SQL Server kısmını barındırmak için dört temel seçenek mevcuttur:

- Sanallaştırılmamış fiziksel makinelerde SQL Server
- Şirket içi sanallaştırılmış makinelerde SQL Server (özel bulut)
- Azure Sanal Makine'de SQL Server (genel bulut)
- Azure SQL Database (genel bulut)

Aşağıdaki bölümlerde, genel bulutta SQL Server ile ilgili bilgi edineceğiz: Azure SQL Database ve Azure VM'leri üzerinde SQL Server. Ayrıca, hangi seçeneğin uygulamanız için en uygun olduğunu belirlemek üzere şu ortak iş teşviklerini inceleyeceğiz.

## Azure SQL Database ve Azure VM'lerinde SQL Server'a daha ayrıntılı bir bakış

**Azure SQL Database**, Azure bulutunda barındırılan, *Hizmet olarak Yazılım (SaaS)* ve *Hizmet olarak Platform (PaaS)* endüstri kategorileri kapsamında bulunan hizmet olarak ilişkisel veritabanıdır (DBaaS). SQL Database, Microsoft'un sahibi olduğu, barındırdığı ve bakımını yaptığı standartlaştırılmış donanım ve yazılım üzerinde geliştirilmiştir. SQL Database ile yerleşik özellikleri ve işlevleri kullanarak doğrudan hizmet üzerinde geliştirme yapabilirsiniz. SQL Database'i kullandığınızda, kesinti olmadan daha fazla güç için ölçeğinizi artırmaya veya genişletmeye yönelik seçeneklerle kullandıkça ödersiniz.

**Azure Virtual Machines'de (VM'ler) SQL Server**, *Hizmet olarak Altyapı (IaaS)* endüstri kategorisine girer ve buluttaki bir sanal makine içinde SQL Server'ı çalıştırmanıza olanak sağlar. SQL Database'e benzer şekilde,bu da Microsoft'un sahibi olduğu, barındırdığı ve bakımını yaptığı standartlaştırılmış donanım üzerinde geliştirilmiştir. VM üzerinde SQL Server'ı kullanırken, kendi SQL Server lisansınızı getirebilir veya Azure portalından önceden lisanslanmış bir SQL Server görüntüsü kullanabilirsiniz.

Genellikle, bu iki SQL seçeneği farklı amaçlar için en iyi hale getirilmiştir:

- **SQL Database**, çok sayıda veritabanının hazırlanması ve yönetilmesi için maliyetleri minimum düzeye indirmek üzere en iyi hale getirilmiştir. Herhangi bir sanal makineyi, işletim sistemini veya veritabanı yazılımını yönetmeniz gerekmediğinden, bu, devam eden yönetim maliyetlerini azaltır. Bu yükseltme, yüksek kullanılabilirlik ve yedeklemeler içerir. Genellikle, Azure SQL Database tek bir BT veya geliştirme kaynağı tarafından yönetilen veritabanlarının sayısını önemli ölçüde artırabilir.
- **Azure VM'leri üzerinde çalışan SQL Server**, var olan şirket içi SQL Server uygulamalarını bir karma senaryoda buluta genişletmek veya bir geçiş veya geliştirme/test senaryosunda var olan bir uygulamayı Azure'a dağıtmak için en iyi hale getirilmiştir. Karma senaryo örneği olarak, [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) kullanımı aracılığıyla Azure'da ikincil veritabanı kopyalarının tutulması verilebilir. Azure VM'lerinde SQL Server ile, adanmış bir SQL Server örneği ve bulut tabanlı bir VM üzerinde tam yönetici haklarına sahip olursunuz. Bu, bir kuruluşun halihazırda sanal makinelerin bakımını yapmak için kullanılabilir BT kaynaklarına sahip olması durumunda ideal bir seçenektir. VM'lerde SQL Server ile, uygulamanızın belirli performans ve kullanılabilirlik gereksinimlerini karşılamak için yüksek düzeyde özelleştirilmiş bir sistem oluşturabilirsiniz.

Aşağıdaki tabloda, SQL Database ve Azure VM'lerinde SQL Server'ın temel özellikleri özetlenmektedir:

<table cellspacing="0" border="1">
<tr>
   <th align="left" valign="middle"></th>
   <th align="left" valign="middle">SQL Database</th>
   <th align="left" valign="middle">Azure VM'de SQL Server</th>

</tr>
<tr>
   <td valign="middle"><p><b>En iyi kullanım alanı:</b></p></td>
   <td valign="middle">
          <ul>
          <li type=round>Geliştirme ve pazarlama alanında zaman kısıtlamaları olan yeni bulut tasarımlı uygulamalar.
          <li type=round>Yerleşik yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yükseltme mekanizmalarına gereksinim duyan uygulamalar.
          <li type=round>Altta yatan işletim sistemi ve yapılandırma ayarlarını yönetmek istemeyen ekipler.
         <li type=round>Ölçek genişletme modellerini kullanan uygulamalar.
         <li type=round>Boyutu 1 TB'ye kadar olan veritabanları.
         <li type=round>Hizmet olarak Yazılım (SaaS) uygulamaları oluşturma.
  </ul>
</td>
   <td valign="middle">
      <ul>
      <li type=round>Minimum değişiklikle buluta hızlı geçiş gerektiren var olan uygulamalar.
      <li type=round>Güvenli bir tünel üzerinden Azure'dan şirket içi kaynaklara (Active Directory gibi) erişim gerektiren SQL Server uygulamaları.
      <li type=round>Tam yönetici haklarına sahip bir özelleştirilmiş BT ortamına ihtiyaç duyuyorsanız.
      <li type=round>Şirket içi üretim dışı SQL Server donanımı satın almak istemediğinizde hızlı geliştirme ve test senaryoları.
      <li type=round>[Azure Storage'a yedekleme](http://msdn.microsoft.com/library/jj919148.aspx) veya Azure VM'lerle AlwaysOn kopyaları kullanılarak şirket içi SQL Server uygulamaları için olağanüstü durum kurtarma.
      <li type=round>Boyutu 1 TB'nin üzerinde olan büyük veritabanları.
      </ul></td>
</tr>
<tr>
   <td valign="middle"><p><b>Kaynaklar</b></p></td>
   <td valign="middle">
       <ul>
       <li type=round>Altyapının destek ve bakımı için BT kaynaklarını kullanmak istemiyorsanız.
       <li type=round>Uygulama katmanına odaklanmak istiyorsanız.
       </ul></td>
   <td valign="middle"><ul><li type=round>Destek ve bakım için BT kaynaklarına sahipseniz.</ul></td>

</tr>
<tr>
   <td valign="middle"><p><b>Toplam sahip olma maliyeti</b></p></td>
   <td valign="middle"><ul><li type=round>Donanım maliyetlerini ortadan kaldırır. Yönetimsel maliyetleri azaltır.</ul></td>
   <td valign="middle"><ul><li type=round>Donanım maliyetlerini ortadan kaldırır. </ul></td>

</tr>
<tr>
   <td valign="middle"><p><b>İş sürekliliği</b></p></td>
   <td valign="middle"><ul><li type=round>Azure SQL Database, yerleşik hata toleransı altyapı özelliklerine ek olarak, iş sürekliliğini artırmak üzere Belirli Bir Noktaya Geri Yükleme, Coğrafi Geri Yükleme ve Coğrafi Çoğaltma gibi özellikler içerir. Daha fazla bilgi için bkz. [SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md).</ul></td>
   <td valign="middle"><ul><li type=round>Azure VM'lerinde SQL Server, veritabanınızın belirli gereksinimleri için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü ayarlamanıza olanak sağlar. Böylece, uygulamanız için en iyi hale getirilmiş bir sisteme sahip olabilirsiniz. Yük devretme işlemlerini ihtiyaç duyulduğunda kendi kendinize test edebilir ve çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).</ul></td>

</tr>
<tr>
   <td valign="middle"><p><b>Karma bulut</b></p></td>
   <td valign="middle"><ul><li type=round>Şirket içi uygulamanız, Azure SQL Database'deki verilere erişebilir.</ul></td>
   <td valign="middle"><ul>
      <li type=round>Azure VM'lerinde SQL Server ile kısmen bulutta ve kısmen şirket içinde çalıştırılan uygulamalara sahip olabilirsiniz. Örneğin, şirket içi ağınızı ve Active Directory Etki Alanı'nı [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) üzerinden buluta genişletebilirsiniz. Ek olarak, [Azure'da SQL Server Veri Dosyaları](http://msdn.microsoft.com/library/dn385720.aspx)'nı kullanarak şirket içi veri dosyalarını Azure Storage'da depolayabilirsiniz. Daha fazla bilgi için bkz. [SQL Server 2014 Karma Bulutu'na giriş](http://msdn.microsoft.com/library/dn606154.aspx).
      <li type=round>[Azure Blob Storage ile SQL Server Yedekleme ve Geri Yükleme](http://msdn.microsoft.com/library/jj919148.aspx)'yi veya [Azure VM'de AlwaysOn kopyalarını](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) kullanarak şirket içi SQL Server uygulamalarına yönelik olağanüstü durum kurtarmayı destekler.
      </ul></td>

</tr>
</table>

## Azure VM'lerinde Azure SQL Database'in veya SQL Server'ın seçilmesine yönelik iş faydaları

### Maliyet

İster nakit ihtiyacında olan yeni bir girişim ister sıkı bütçe kısıtlamaları altında işletilen yerleşik bir şirket bünyesindeki bir ekip olun, sınırlı fon genellikle veritabanlarınızın barındırılmasına yönelik kararların verilmesinde en önemli etmendir. Bu bölümde, ilk olarak şu iki ilişkisel veritabanı seçeneğine istinaden Azure'da faturalama ve lisanslama ile ilgili temel bilgileri öğreneceğiz: SQL Database ve Azure VM'lerde SQL Server. Ardından, toplam uygulama maliyetini nasıl hesaplamamız gerektiğini öğreneceğiz.

#### Faturalama ve lisanslama ile ilgili temel bilgiler

Azure VM'lerin SQL Server için geleneksel SQL Server lisanslaması gerekirken, **SQL Database** müşterilere lisansla değil, bir hizmet olarak satılır.

Şu anda **SQL Database** birkaç hizmet katmanında sunulmaktadır ve bu katmanların her biri, seçtiğiniz hizmet katmanında ve performans düzeyi temelinde sabit bir ücretle saatlik olarak faturalandırılır. Buna ek olarak, giden Internet trafiği için faturalandırılırsınız. Temel, Standart ve Premium hizmet katmanları, uygulamanızın en üst sıradaki gereksinimlerini karşılamak üzere birden çok performans düzeyine sahip tahmin edilebilir bir performans sağlamak üzere tasarlanmıştır. Uygulamanızın değişken verimlilik ihtiyaçlarını karşılamak üzere, hizmet katmanları ve performans düzeyleri arasında geçiş yapabilirsiniz. Veritabanınızın işlem hacmi yüksekse ve çok sayıda eşzamanlı kullanıcıyı desteklemesi gerekiyorsa Premium hizmet katmanını öneririz. Desteklenen geçerli hizmet katmanlarına yönelik en son bilgiler için, bkz. [Azure SQL Database Hizmet Katmanları](sql-database-service-tiers.md).

**SQL Database** ile, veritabanı yazılımına Microsoft tarafından otomatik olarak yapılandırma, düzeltme eki ve yükseltme uygulanır; bu da yönetim maliyetlerinizi azaltır. Ayrıca, [yerleşik yedekleme](sql-database-business-continuity.md) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur.

**Azure VM'lerinde SQL Server** ile, geleneksel SQL Server lisansını kullanırsınız. Platform tarafından sağlanan SQL Server görüntüsünü (lisans içerir) kullanabilir veya kendi SQL Server lisansınızı getirebilirsiniz. Azure tarafından sağlanan görüntüler kullanıldığında, işletim maliyeti, VM boyutunun yanı sıra, seçtiğiniz SQL Server sürümüne bağlıdır. VM boyutundan veya SQL Server sürümünden bağımsız olarak, VM diskleri için Azure Storage maliyetinin yanı sıra, SQL Server ve Windows Server için dakika başına lisanslama ücreti ödersiniz. Dakika başına faturalandırma seçeneği, SQL Server'ı ek SQL Server lisansları satın almadan istediğiniz süreyle kullanmanıza olanak sağlar. Azure'a kendi SQL Server lisansınızı getirirseniz yalnızca Windows Server ve depolama maliyetleri için sizden ücret tahsil edilir. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/).

#### Toplam uygulama maliyetini hesaplama

Bir bulut platformunu kullanmaya başladığınızda, uygulamanızı çalıştırmanın maliyeti birincil olarak geliştirme ve yönetim maliyetlerinin yanı sıra, genel bulut platformunun gerektirdiği hizmet maliyetlerini içerir.

SQL Database ve Azure VM'lerinde SQL Server çalıştıran uygulamanız için ayrıntılı maliyeti hesaplaması şu şekildedir:

**Azure SQL Database kullanıldığında:**

*Toplam uygulama maliyeti = Yüksek oranda küçültülmüş yönetim maliyetleri + yazılım geliştirme maliyetleri + SQL Database hizmet maliyetleri*

**Azure VM'lerinde SQL Server kullanıldığında:**

*Toplam uygulama maliyeti = Küçültülmüş yazılım geliştirme/değişiklik maliyetleri + yönetim maliyetleri + SQL Server ve Windows Server lisanslama maliyetleri + Azure Storage maliyetleri*

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL Database Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)
- [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows) için [Virtual Machine Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] SQL Server'daki özelliklerin küçük bir kısmı, SQL Database için geçerli veya kullanılabilir değildir. Daha fazla bilgi için bkz. [SQL Database Genel Sınırlamaları ve Yönergeleri](sql-database-general-limitations.md) ve [SQL Database Transact-SQL Bilgileri](sql-database-transact-sql-information.md). Var olan bir SQL Server çözümünü buluta taşıyorsanız bkz. [Bir SQL Server veritabanının Azure SQL Database'e geçişini sağlama](sql-database-cloud-migrate.md). Var olan bir şirket içi SQL Server uygulamasının SQL Database'e geçişini sağladığınızda, bulut hizmetlerinin sunduğu özelliklerden faydalanmak üzere uygulamayı güncelleştirme seçeneğini değerlendirin. Örneğin, maliyet avantajlarını artırmak için uygulama katmanınızı barındırmak üzere [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/)'i veya [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/)'i kullanma fikrini değerlendirebilirsiniz.

### Yönetim

Birçok işletme için, bir bulut hizmetine geçiş yapma kararı, maliyetle ilgili olduğu kadar, yönetim karmaşıklığı yükünü gidermekle de ilgilidir. **SQL Database** ile Microsoft, altta yatan donanımı yönetir, tüm verileri yüksek kullanılabilirlik sağlamak üzere otomatik olarak çoğaltır, veritabanı yazılımını yapılandırır ve yükseltir, yük dengelemeyi yönetir ve bir sunucu hatası olması durumunda saydam yük devretme işlemi gerçekleştirir. Veritabanınızı yönetmeye devam edebilirsiniz ancak artık veritabanı altyapısını, sunucu işletim sistemini veya donanımı yönetmeniz gerekmez.  Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir.

Öte yandan, şirket içi uzmanlığa sahip olabilir ve disk konumuna kadar ayrıntılı bir şekilde veritabanı konumu üzerinde denetimi koruma isteğinde olabilirsiniz. **Azure VM'lerinde çalışan SQL Server** ile, işletim sistemi ve SQL Server örneği yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirileceğine/yükseltileceğine ve virüsten koruma ve yedekleme araçları gibi herhangi bir ek yazılımın ne zaman yükleneceğine ilişkin karar size aittir. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz.  Örneğin, Azure bir VM'nin boyutunu gereken şekilde değiştirmenize izin verir. Bilgi için bkz. [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](../virtual-machines/virtual-machines-linux-sizes.md).

### Hizmet Düzeyi Sözleşmesi (SLA)

Birçok BT departmanı için, bir Hizmet Düzeyi Sözleşmesi'nin çalışma süresi yükümlülüklerinin yerine getirilmesi birincil önceliğe sahiptir. Bu bölümde, her bir veritabanı barındırma seçeneği için geçerli olan SLA'yı ele alacağız.

**SQL Database** Temel, Standart ve Premium hizmet katmanları için Microsoft, %99,99 oranında kullanılabilirlik SLA'sı sağlar. En son bilgiler için bkz. [Hizmet Düzeyi Sözleşmesi](https://azure.microsoft.com/support/legal/sla/sql-database/). SQL Database hizmet katmanlarına ve desteklenen iş sürekliliği planlarına yönelik en son bilgiler için bkz. [Hizmet Katmanları](sql-database-service-tiers.md).

**Azure VM'lerinde çalıştırılan SQL Server** için Microsoft, yalnızca Sanal Makine'yi kapsayan %99,95 oranında bir kullanılabilirlik SLA'sı sağlar. Bu SLA, VM'de çalıştırılan işlemleri (SQL Server gibi) kapsamaz ve bir kullanılabilirlik kümesinde en az iki VM örneğini barındırmanızı gerektirir. En son bilgiler için bkz. [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). VM'lerde veritabanı yüksek kullanılabilirliği (HA) için, [AlwaysOn Kullanılabilirlik Grupları](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx) gibi, SQL Server'da desteklenen yüksek kullanılabilirlik seçeneklerinden birini gibi yapılandırmanız gerekir.

### <a name="market"></a>Pazarlama süresi

**SQL Database**, geliştirici üretkenliği ve hızlı pazarlama süresi kritik önem taşıdığında bulut tasarımlı uygulamalar için doğru çözümdür. Programlama DBA benzeri işlevsellik ile, altta yatan işletim sistemini ve veritabanını yönetmeye yönelik ihtiyacı azalttığından, bulut mimarları ve geliştiricileri için idealdir. Örneğin, [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) ve [PowerShell Cmdlet'lerini](http://msdn.microsoft.com/library/azure/dn546726.aspx) binlerce veritabanının yönetimsel işlemlerini otomatik hale getirmek ve yönetmek için kullanabilirsiniz. [Elastic Database Havuzları](sql-database-elastic-pool.md), uygulama katmanına odaklanmanıza ve çözümünüzü pazara daha hızlı bir şekilde sunmanıza olanak sağlar.

**Azure VM'lerinde çalıştırılan SQL Server**, var olan veya yeni uygulamalarınızın bir SQL Server örneğinin tüm özelliklerine yönelik erişim ve denetim gerektirmesi durumunda idealdir. Bu aynı zamanda, var olan şirket içi uygulamaların ve veritabanlarının oldukları gibi Azure'a geçişini sağlamak istediğinizde iyi bir tercihtir. Sunumu, uygulamayı ve veri katmanlarını değiştirmeniz gerekmediği için, var olan çözümünüzü yeniden yapılandırma konusunda zamandan ve bütçeden tasarruf sağlarsınız. Bunun yerine, tüm çözümlerinizin Azure'a geçişini sağlamaya ve Azure platformu tarafından gerekli kılınabilen bazı performans iyileştirmelerini gerçekleştirmeye odaklanabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için En İyi Performans Uygulamaları](../virtual-machines/virtual-machines-windows-sql-performance.md).

## Özet

Bu makalede, SQL Database ve Azure Virtual Machines'de (VM'ler) SQL Server işlenmiş ve kararınızı etkileyebilecek genel iş teşvikleri ele alınmıştır. Aşağıda, dikkate almanız için önerilerin bir özeti sağlanmıştır:

**Azure SQL Database**'i aşağıdaki koşullar geçerli olduğunda tercih edin:

- Yeni bulut tabanlı uygulamalar oluşturuyorsanız veya bulut hizmetlerinin sağladığı maliyet tasarruflarından ve performans iyileştirmesinden yararlanmak için var olan SQL Server çözümünüzün geçişini yapmak istiyorsanız. Bu yaklaşım, tam olarak yönetilen bir bulut hizmetinin avantajlarını sağlar, daha kısa bir ilk pazarlama süresi elde edilmesine yardımcı olur ve uzun vadeli maliyet iyileştirmesi sağlayabilir.

- Microsoft'un veritabanlarınız üzerinde genel yönetim işlemlerini gerçekleştirmesini ve veritabanları için daha güçlü kullanılabilirlik SLA'larını gerekli kılmasını istiyorsanız.



**Azure VM'lerinde SQL Server**'ı aşağıdaki koşullar geçerli olduğunda tercih edin:

- Var olan şirket içi uygulamalarınız mevcutsa ve kendi donanımınızın bakımını yapmaya devam etmek istemiyorsanız veya karma çözümleri değerlendiriyorsanız. Bu yaklaşım, güvenli bir tünel üzerinden şirket içi uygulamalarınıza bağlantıyı kolaylaştırırken, yüksek veritabanı kapasitesine daha kısa sürede erişmenizi sağlar.

- Var olan BT kaynaklarınız mevcutsa SQL Server üzerinde yönetici haklarına ihtiyacınız varsa ve şirket içi SQL Server ile tam uyumluluğu gerekli kılıyorsanız. Bu yaklaşım, çoğu uygulamayı çalıştırma esnekliğiyle birlikte var olan uygulamaların geliştirilme veya değiştirilme maliyetlerini en aza indirmenize olanak sağlar. Ayrıca, VM, işletim sistemi ve veritabanı yapılandırması üzerinde tam denetim sağlar.



> [AZURE.NOTE] SQL Server 2016 CTP2'yi denemek mi istiyorsunuz? Microsoft Azure'a kaydolun ve ardından [buraya](http://aka.ms/sql2016vm "buraya") giderek SQL Server 2016 CTP2'nin önceden yüklenmiş olduğu bir Virtual Machine oluşturun.

## Sonraki adımlar
- SQL Database'i kullanmaya başlamak için bkz. [SQL Database öğreticisi: Azure portalını kullanarak birkaç dakika içinde bir SQL veritabanı oluşturma](sql-database-get-started.md).
- Bkz. [SQL Database fiyatlandırması] (https://azure.microsoft.com/pricing/details/sql-database/)
- Azure VM'lerinde SQL Server'ı kullanmaya başlamak için bkz. [Azure'da SQL Server sanal makinesi sağlama](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md).



<!---HONumber=Jun16_HO2-->


