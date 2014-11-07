<properties umbracoNaviHide="0" pageTitle="Uygulama Modeli" metaKeywords="Azure, Azure, application model, Azure application model, development model, Azure development model, hosted service, Azure hosted service, web role, worker role" description="Learn about the Azure hosted service application model. Understand core concepts, design considerations, defining and configuring your application, and scaling." urlDisplayName="Application Model" headerExpose="" footerExpose="" disqusComments="1" title="Application Model" authors="robb" manager="johndaw" />

<tags ms.service="multiple" ms.workload="multiple" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="robb" />



# Azure Yürütme Modelleri

Azure, uygulamaları çalıştırmak için farklı yürütme modelleri sağlar. Her biri farklı bir hizmet kümesi sağlar ve bu yüzden hangisini seçtiğiniz, tam olarak ne yapmaya çalıştığınıza bağlıdır. Bu makalede üç tanesini ele alır, her bir teknolojiyi açıklar ve hangi durumlarda kullanacağınızla ilgili örnekler verir.

## İçindekiler

- [Sanal Makineler](#VMachine)
- [Web Siteleri](#WebSites)
- [Bulut Hizmetleri](#CloudServices)
- [Ne Kullanmalıyım? Seçim Yapma](#WhatShouldIUse)

<h2><a id="VMachine"></a>Sanal Makineler</h2>
Azure Sanal Makineler geliştiricilerin, BT operasyon çalışanlarının ve diğerlerinin bulutta sanal makineler oluşturup kullanmasını sağlar. *Hizmet Olarak Altyapı (IaaS)* sağlayan bu teknoloji, çeşitli yollarla kullanılabilir. [Şekil 1](#Fig1), temel bileşenlerini gösterir.

<a name="Fig1"></a>![01_CreatingVMs][01_CreatingVMs]

**Şekil 1: Azure Sanal Makineler, Hizmet Olarak Altyapı sağlar.**

Şekilde gösterildiği gibi Azure Yönetim Portalı veya REST tabanlı Azure Hizmet Yönetimi API'si kullanarak sanal makineler oluşturabilirsiniz. Yönetim Portalı'na Internet Explorer, Mozilla ve Chrome gibi herhangi bir popüler tarayıcıdan erişilebilir. REST API'si için Microsoft; Windows, Linux ve Macintosh sistemlerine yönelik istemci betik oluşturma araçları sağlar. Aynı zamanda diğer yazılımlar da bu arabirimi kullanabilir.

Ancak, platforma eriştiğinizde yeni bir sanal makinenin oluşturulması için sanal makinenin görüntüsüne ait bir sanal sabit disk (VHD) seçilmesi gerekir. Bu VHD'ler Azure blob'larında depolanır. 

Başlangıç için iki seçeneğiniz vardır 

1. Kendi VHD'nizi yükleme 
2. Microsoft ve iş ortakları tarafından Azure Sanal Makineler galerisinde veya Microsoft açık kaynak [VMDepot](http://vmdepot.msopentech.com/) web sitesinde sunulan VHD'leri kullanma. 

Galerideki ve VMDepot üzerindeki VHD'ler, temiz Microsoft ve Linux işletim sistemi görüntülerinin yanı sıra bunların üzerine yüklü Microsoft ve diğer üçüncü taraf ürünleri içeren görüntülerdir.  Seçenekler sürekli olarak artmaktadır. Örnekler aşağıdakilerin çeşitli sürümlerini ve yapılandırmalarını içerir:
 
-	Windows Server 
-	Suse, Ubuntu ve CentOS gibi Linux sunucuları
-	SQL Server
-	BizTalk Server 
-	SharePoint Server



Bir VHD ile birlikte yeni sanal makinenizin boyutunu belirlersiniz.  Her boyut için tam istatistikler [Azure kitaplığında](http://msdn.microsoft.com/tr-tr/library/windowsazure/dn197896.aspx) listelenmiştir.  

-	**Çok Küçük**, paylaşılan bir çekirdeğe ve 768MB belleğe sahiptir.
-	**Küçük**, 1 çekirdeğe ve 1,75GB belleğe sahiptir.
-	**Orta**, 2 çekirdeğe ve 3,5GB belleğe sahiptir.
-	**Büyük**, 4 çekirdeğe ve 7GB belleğe sahiptir.
-	**Çok Büyük**, 8 çekirdeğe ve 14GB belleğe sahiptir.
-	**A6**, 4 çekirdeğe ve 28GB belleğe sahiptir.
-	**A7**, 8 çekirdeğe ve 56GB belleğe sahiptir.


Son olarak ABD'de, Avrupa'da veya Asya'da yeni sanal makinenizin hangi Azure veri merkezinde çalışacağını seçersiniz. 

Bir sanal makine çalıştırıldıktan sonra çalıştığı her saat için ücret ödersiniz ve ilgili sanal makineyi kaldırdığınızda ödemeyi bırakırsınız. Ödediğiniz tutar sanal makinenizin ne kadar kullanıldığına değil, yalnızca saat cinsinden süreye bağlıdır. Bir saat boyunca boşta kalan bir sanal makinenin maliyeti, ağır yüke sahip bir sanal makineyle aynıdır. 

Çalışan her sanal makinenin bir blob'da tutulan ilişkili bir *işletim sistemi diski* vardır. Bir galeri VHD'si kullanarak sanal makine oluşturduğunuzda ilgili VHD, sanal makinenizin işletim sistemi diskine kopyalanır. Çalışan sanal makinenizin işletim sisteminde yaptığınız tüm değişiklikler burada depolanır. Örneğin, Windows kayıt defterini değiştiren bir uygulama yüklerseniz bu değişiklik sanal makinenizin işletim sistemi diskinde depolanır. İlgili işletim sistemi diskinden bir sanal makine daha oluşturduğunuzda sanal makine bıraktığınız durumda çalışmaya devam eder. Galeriye depolanmış VHD'ler için Microsoft, gerektiğinde işletim sistemi düzeltme ekleri gibi güncelleştirmeler uygular. Ancak, kendi işletim sistemi disklerinizdeki VHD'ler için bu güncelleştirmeleri uygulamak sizin sorumluluğunuzdadır (Windows Update varsayılan olarak açık olsa da).

Çalışan sanal makineler ayrıca sysprep aracı kullanılarak değiştirilebilir ve ardından yakalanabilir. Sysprep, bir VHD görüntüsünün aynı genel yapılandırma ile başka sanal makineler oluşturmak üzere kullanılması için makine adı gibi özellikleri kaldırır. Bu görüntüler, ek sanal makineler için başlangıç noktası olarak kullanılabilmeleri için yüklenmiş görüntülerinizle birlikte Yönetim portalında depolanır. 

Sanal Makineler, aynı zamanda oluşturduğunuz her bir sanal makineyi barındıran donanımları da izler. Sanal makine çalıştıran bir fiziksel sunucu arıza yaparsa, platform bunu fark eder ve aynı sanal makineyi başka bir makinede başlatır. Doğru lisansa sahip olduğunuzu varsayarak, değiştirilmiş bir VHD'yi işletim sistemi diskinizin dışına kopyalayabilir, ardından bunu şirketi içi veri merkeziniz veya farklı bir bulut sağlayıcısı gibi başka bir yerde çalıştırabilirsiniz. 

Sanal makineniz, işletim sistemi diskinize ek olarak bir veya daha fazla veri diskine sahiptir. Bunların her biri sanal makinenize takılmış bir disk sürücüsü gibi görünse de her birinin içeriği aslında bir blob'da depolanır. Bir veri diskine yazılan her öğe, temel alınan blob'da kalıcı olarak depolanır. Tüm blob'larda olduğu gibi Azure, bunları tek bir veri merkezinde ve veri merkezleri arasında arızalara karşı korumak amacıyla çoğaltır.

Çalışan sanal makineler Yönetim Portalı, PowerShell ve diğer betik oluşturma araçları kullanılarak veya doğrudan REST API'si aracılığıyla yönetilebilir. (Aslında, Yönetim Portalı ile yapabildiğiniz her şey bu API ile programlı olarak yapılabilir.) RightScale ve ScaleXtreme gibi Microsoft ortakları da REST API'sine dayanan yönetim hizmetleri sağlar.

Azure Sanal Makineler çeşitli yollarla kullanılabilir. Microsoft'un hedeflediği birincil senaryolar şunlardır:

- **Geliştirme ve test amaçlı sanal makineler.** Geliştirme grupları, genel olarak uygulama oluşturmak için belirli yapılandırmalara sahip sanal makinelere ihtiyaç duyar. Azure Sanal Makineler bu sanal makineleri oluşturmak, kullanmak ve ihtiyaç kalmadığında kaldırmak için basit ve ekonomik bir yöntem sağlar.
- **Uygulamaları bulutta çalıştırma.** Bazı uygulamalar için genel bulutta çalışma ekonomik bakımdan mantıklıdır. Örneğin, büyük talep değişimlerine sahip bir uygulama düşünün. Bu uygulamayı çalıştırmak amacıyla kendi veri merkeziniz için yeterli makine almanız her zaman mümkündür, ancak bu makinelerin birçoğu çoğunlukla kullanılmadan kalır. Bu uygulamanın Azure üzerinde çalıştırılması, ek sanal makineler için yalnızca ihtiyaç duyduğunuzda ödeme yapmanızı ve talep değişimi sona erdiğinde bunları kapatmanızı sağlar. Ya da hızlıca ve bir taahhütte bulunmadan talep üzerine bilgi işlem kaynaklarına ihtiyaç duyan bir başlangıç düzeyinde olduğunuzu varsayalım. Bu durumda da Azure doğru seçim olabilir.
- **Kendi veri merkezinizi genel buluta genişletme.** Azure Sanal Ağ ile kuruluşunuz, bir Azure Sanal Makine grubunu kendi şirket içi ağınızın parçası olarak gösteren bir sanal ağ (VNET) oluşturabilir. Bu durum, Azure üzerinde SharePoint ve diğer uygulamaların çalıştırılmasını sağlar ve kendi veri merkezinizde çalıştırmaya kıyasla dağıtımı daha kolay ve/veya daha ucuz olabilecek bir yaklaşımdır.
- **Olağanüstü durum kurtarma.** Nadiren kullanılan bir yedek veri merkezi için sürekli ödeme yapmak yerine IaaS tabanlı bir olağanüstü durum kurtarma özelliği, yalnızca gerçekten ihtiyaç duyduğunuzda bilgi işlem kaynakları için ödeme yapmanızı sağlar.  Örneğin, birincil veri merkeziniz arıza yaparsa temel uygulamaları çalıştırmak için Azure'da sanal makineler oluşturabilir, daha sonra bunlara ihtiyacınız kalmadığında bu sanal makineleri kapatabilirsiniz. 

Azure Sanal Makineleri nasıl kullanabileceğinizle ilgili başka olasılıklar da mevcuttur, ancak bunlar iyi örneklerdir.  


### Sanal Makineleri Gruplandırma: Bulut Hizmetleri 

Azure Sanal Makineler ile yeni bir sanal makine oluştururken sanal makineyi tek başına çalıştırmayı veya bir *bulut hizmeti* içinde birlikte çalışan sanal makine grubunun bir parçası yapmayı seçebilirsiniz. (Benzer adlara sahip olmalarına rağmen bu kavramı Azure'un PaaS teknolojisinin adı olan Bulut Hizmetleri ile karıştırmayın; bu ikisi aynı değildir.)  Her bir bağımsız sanal makineye kendi ortak IP adresi atanırken, aynı bulut hizmetindeki tüm sanal makinelere tek bir ortak IP adresi üzerinden erişilir. [Şekil 2](#Fig2) bunun nasıl göründüğünü gösterir.
 
<a name="Fig2"></a>![02_CloudServices][02_CloudServices]

**Şekil 2: Her bağımsız sanal makine kendi ortak IP adresine sahipken, bir bulut hizmetinde gruplandırılan sanal makineler tek bir ortak IP adresi üzerinden kullanıma sunulur.**

Örneğin, basit bir uygulamayı oluşturmak ve test etmek için sanal makine oluşturuyorsanız muhtemelen tek başına bir sanal makine kullanırsınız. Web ön uç, veritabanı ve hatta belki de bir orta katman içeren çok katmanlı bir uygulama oluşturuyorsanız Şekil 2'de gösterildiği gibi büyük olasılıkla birden çok sanal makineyi bir bulut hizmetine bağlarsınız.

Sanal makinelerin bir bulut hizmetinde gruplandırılması, diğer seçenekleri de kullanmanızı sağlar. Azure, kullanıcı isteklerini aynı bulut hizmetindeki sanal makinelere dağıtarak bunlar için yük dengeleme sağlar. Bu şekilde bağlanan sanal makineler, bir Azure veri merkezindeki yerel ağ üzerinden de birbiriyle iletişim kurabilir. 

Aynı bulut hizmetindeki sanal makineler, bir veya daha fazla *kullanılabilirlik kümesinde* de gruplandırılabilir. Bunun neden önemli olduğunu anlamak için birden çok ön uç sanal makine çalıştıran bir web uygulaması düşünün. Bu sanal makinelerin tümü, aynı fiziksel makineye veya aynı makine rafına dağıtılırsa tek bir donanım hatası hepsini erişilemez hale getirebilir. Ancak, bu sanal makineler bir kullanılabilirlik kümesinde gruplandırılırsa Azure, bunları veri merkezine dağıtır ve böylece hiçbiri tek bir hata noktasını paylaşmaz.

### Senaryo: SQL Server ile Uygulama Çalıştırma

Azure Sanal Makinelerin nasıl çalıştığını daha iyi anlamak için birkaç senaryoya biraz daha ayrıntılı olarak bakılması faydalıdır. Örneğin, Azure üzerinde çalışan güvenilir ve ölçeklenebilir bir web uygulaması oluşturmak istediğinizi varsayalım. Bunu yapmanın bir yolu, uygulamanın mantığını bir veya daha fazla Azure sanal makinesinde çalıştırmak ve ardından SQL Server'ı veri yönetimi için kullanmaktır. [Şekil 3](#Fig3) bunun nasıl göründüğünü gösterir.

<a name="Fig3"></a>![03_AppUsingSQLServer][03_AppUsingSQLServer]

**Şekil 3: Azure Sanal Makineler içerisinde çalışan bir uygulama, depolama için SQL Server kullanabilir.**

Bu örnekte her iki sanal makine türü de galerideki standart VHD'lerden oluşturulur. Uygulamanın mantığı Windows Server 2008 R2'de çalışır, bu nedenle geliştirici bu VHD'den üç sanal makine oluşturur ve ardından uygulamasını her birine yükler. Bu sanal makinelerin tümü aynı bulut hizmetinde olduğundan, donanım yük dengelemesini bunlar arasında istekleri dağıtacak şekilde yapılandırabilir. Geliştirici ayrıca galerinin SQL Server 2012 içeren VHD'sinden iki sanal makine oluşturur. Bunlar çalıştıktan sonra geliştirici, veritabanı yansıtmasını otomatik yük devretmeyle birlikte kullanmak için her bir örnekte SQL Server'ı yapılandırır. Bunun yapılması zorunlu değildir; uygulama tek bir SQL Server örneği kullanabilir, ancak bu yaklaşımın belirlenmesi güvenilirliği artırır. 

### Senaryo: SharePoint Grubu Çalıştırma 

Bir kuruluşun bir SharePoint grubu oluşturmak istediğini, ancak grubu kendi veri merkezinde çalıştırmak istemediğini varsayalım. Şirket içi veri merkezi kaynak sıkıntısı çekiyor veya grubu oluşturan iş birimi bu dahili BT grubuyla ilgilenmek istemiyor olabilir. Bunun gibi durumlarda SharePoint'in Azure Sanal Makineler üzerinde çalıştırılması mantıklı olabilir. [Şekil 4](#Fig4) bunun nasıl göründüğünü gösterir.

<a name="Fig4"></a>![04_SharePointFarm][04_SharePointFarm]
 
**Şekil 4: Azure Sanal Makineler, bir SharePoint grubunun bulutta çalıştırılmasını sağlar.**

Bir SharePoint grubunda her biri farklı bir VHD'den oluşturulmuş bir Azure sanal makinesinde çalışan birkaç bileşen vardır. Aşağıdakiler gereklidir:

- Microsoft SharePoint. Galeride deneme görüntüleri vardır veya kuruluş kendi VHD'sini sağlar.
- Microsoft SQL Server. SharePoint bir SQL Server'a bağımlıdır, bu nedenle kuruluş standart bir galeri VHD'sinden SQL Server 2012 çalıştıran sanal makineler oluşturur.
- Windows Server Active Directory. SharePoint ayrıca Active Directory gerektirir, bu nedenle kuruluş, galerideki bir Windows Server görüntüsünü kullanarak bulutta etki alanı denetleyicileri oluşturur. Bunun yapılması kesin olarak zorunlu değildir; şirket içi etki alanı denetleyicileri de kullanılabilir; ancak SharePoint, Active Directory ile sıklıkla etkileşim kurar ve bu yüzden burada gösterilen yaklaşımın performansı daha iyi olur.

Şekilde gösterilmemesine rağmen bu SharePoint grubu büyük olasılıkla Azure Sanal Ağı kullanılarak bir şirket içi ağa bağlanır. Bu durum, sanal makinelerin (ve içerdikleri uygulamaların) ilgili ağı kullanan kişiler tarafından yerel olarak görünmesini sağlar.

Bu örneklerde gösterildiği gibi Azure Sanal Makineleri kullanarak bulutta yeni uygulamalar oluşturup kullanabilir, var olan uygulamaları çalıştırabilir veya başka işlemler yapabilirsiniz. Hangi seçeneği belirlediğinize bakılmaksızın teknolojinin amacı aynıdır: ortak bulut bilgi işlemi için genel amaçlı bir altyapı sağlamak.



<h2><a id="WebSites"></a>Web siteleri</h2>

İnsanlar web teknolojilerini birçok farklı şekilde kullanır. Bir şirket, haftada milyonlarca tıklamayı kaldırabilen ve dünya genelinde birkaç veri merkezine dağıtılabilen bir varlık web sitesini geçirmek ya da ayarlamak isteyebilir. Bir web tasarımı ajansı, binlerce kullanıcıyı yönetebilen özel bir web uygulaması oluşturmak üzere bir geliştirici ekibiyle birlikte çalışabilir. Bir şirket geliştiricisi, Bulut içerisindeki gider raporlarını şirket Active Directory'sinden izlemek amacıyla bir uygulama oluşturmak isteyebilir. Bir BT danışmanı, küçük bir işletme için bir içerik yönetimi sistemi oluşturmak üzere popüler bir açık kaynak uygulaması kullanabilir.
Tüm bunlar Azure Sanal Makineler kullanılarak gerçekleştirilebilir. Ancak, ham sanal makinelerin oluşturulması ve yönetilmesi biraz beceri ve çaba gerektirir. Bir web sitesi veya web uygulaması yürütmeniz gerekiyorsa daha kolay (ve ucuz) bir çözüm vardır: bu yaklaşım, yaygın olarak Hizmet Olarak Platform (PaaS) adıyla bilinir. Şekil 5'te gösterildiği gibi Azure bunu Web Siteleri ile sağlar.


<a name="Fig5"></a>![05_Websites][05_Websites]
 
**Şekil 5: Azure Web Siteleri statik web sitelerini, popüler web uygulamalarını ve çeşitli teknolojilerle oluşturulmuş özel web uygulamalarını destekler.** 

Azure Web Siteleri, web uygulamalarını çalıştırmak için iyileştirilmiş bir Hizmet Olarak Platform çözümü oluşturmak amacıyla Azure Bulut Hizmetleri'nin üzerine inşa edilmiştir. Şekilde gösterildiği gibi Web Siteleri, birden fazla kullanıcı tarafından oluşturulmuş birden çok web sitesi içeren bir dizi tek sanal makineler ya da tek bir kullanıcıya ait standart sanal makineler üzerinde çalışır. Sanal makineler Azure Web Siteleri tarafından yönetilen bir kaynak havuzunun parçasıdır ve bu yüzden yüksek güvenilirlik ve hataya dayanıklılık sağlar.
Kullanmaya başlamak kolaydır. Azure Web Siteleri ile kullanıcılar bir dizi uygulama, çerçeve ve şablon arasından seçim yapabilir ve saniyeler içinde bir web sitesi oluşturabilir. Daha sonra sürekli tümleştirme oluşturmak ve bir ekip olarak gelişmek amacıyla favori geliştirme araçlarını (WebMatrix, Visual Studio, başka bir düzenleyici) ve kaynak denetimi seçeneklerini kullanabilir. MySQL DB'ye bağımlı uygulamalar bir Microsoft ortağı olan ClearDB tarafından Azure için sağlanan bir MySQL hizmetini kullanabilir.
Geliştiriciler, Web Siteleri ile geniş ve ölçeklenebilir web uygulamaları oluşturabilir. Bu teknoloji ASP.NET, PHP, Node.js ve Python kullanarak uygulama oluşturmayı destekler. Uygulamalar, örneğin yapışkan oturumlar kullanabilir ve var olan web uygulamaları herhangi bir değişiklik olmadan bu bulut platformuna taşınabilir. Web Siteleri'nde oluşturulan uygulamalar Hizmet Veri Yolu, SQL Veritabanı ve Blob Depolama Birimi gibi diğer Azure özelliklerini kullanabilir. Ayrıca, Web Siteleri isteklerin yük dengelemesini otomatik olarak yaparken bir uygulamanın birden fazla kopyasını farklı sanal makinelerde çalıştırabilirsiniz. Üstelik yeni Web Siteleri örnekleri zaten var olan sanal makinelerde oluşturulduğundan, yeni bir uygulamanın başlatılması çok hızlı gerçekleşir; yeni bir sanal makinenin oluşturulmasını beklemekten çok daha hızlıdır.
[Şekil 5](#Fig5)'te gösterildiği gibi Web Siteleri'nde kod ve diğer web içeriklerini birkaç farklı şekilde yayımlayabilirsiniz. FTP, FTPS veya Microsoft WebDeploy teknolojisini kullanabilirsiniz. Web Siteleri ayrıca Git, GitHub, CodePlex, BitBucket, Dropbox, Mercurial, Team Foundation Server ve bulut tabanlı Team Foundation Hizmeti gibi kaynak denetimi sistemlerinden kod yayımlamayı destekler.


<h2><a id="CloudServices"></a>Bulut Hizmetleri</h2>

Azure Sanal makineler IaaS sağlarken Azure Web Siteleri web barındırma sağlar. Üçüncü hesaplama seçeneği olan Bulut Hizmetleri, *Hizmet Olarak Platform (PaaS)* sağlar. Bu teknoloji ölçeklenebilir, güvenilir ve çalıştırılması ucuz uygulamaları destekleyecek şekilde tasarlanmıştır. Ayrıca geliştiricilerin, kullandıkları platform konusunda endişelenmemelerini ve tamamen kendi uygulamalarına odaklanmalarını sağlar. [Şekil 6](#Fig6) içerisinde bu fikir gösterilir.

<a name="Fig6"></a>![06_CloudServices2][06_CloudServices2] 

**Şekil 6: Azure Bulut Hizmetleri, Hizmet Olarak Platform sağlar.**

Diğer Azure hesaplama seçenekleri gibi Bulut Hizmetleri de sanal makinelere bağımlıdır. Teknoloji, birbirinden biraz farklı iki sanal makine seçeneği sağlar: *web rolleri* örnekleri IIS ile bir Windows Server değişkeninde, *çalışan rolleri* örnekleri ise IIS olmadan aynı Windows Server değişkeninde çalışır. Bir Bulut Hizmetleri uygulaması, bu iki seçeneğin bazı birleşimlerine dayanır. 

Örneğin, basit bir uygulama yalnızca bir web rolü kullanabilirken daha karmaşık bir uygulama, kullanıcılardan gelen istekleri işlemek için bir web rolü kullanabilir ve ardından bu istekleri işleme amacıyla bir çalışan rolüne geçirir. (Bu iletişim Hizmet Veri Yolu veya Azure Sıraları'nı kullanır.)

Şekilde gösterildiği gibi, daha önce Azure Sanal Makineler'de açıklandığı üzere tek bir uygulamadaki tüm sanal makineler aynı bulut hizmetinde çalışır. Bu nedenle kullanıcılar, tek bir ortak IP adresinden uygulamaya erişebilir ve isteklerin yük dengelemesi, uygulamaya ait sanal makinelerde otomatik olarak gerçekleştirilir. Azure Sanal Makineler kullanılarak oluşturulan bulut hizmetlerinde olduğu gibi platform, sanal makineleri tek bir donanım hatası noktasından kaçınarak Bulut Hizmetleri uygulamasına dağıtır.

Uygulamalar sanal makinelerde çalışsa da Bulut Hizmetleri'nin IaaS değil, PaaS sağladığı anlaşılmalıdır. Bu konuda aşağıdaki gibi düşünülebilir: Azure Sanal Makineler gibi IaaS ile öncelikle uygulamanızın çalışacağı ortamı oluşturup yapılandırır, ardından uygulamanızı bu ortama dağıtırsınız. İşletim sisteminin yeni düzeltme ekli sürümlerini her bir sanal makineye dağıtmak gibi işlemleri gerçekleştirerek bu dünyanın büyük bölümünü yönetme sorumluluğunu üstlenirsiniz. PaaS ile bunun aksine ortam zaten varmış gibi hareket edersiniz. Tüm yapmanız gereken uygulamanızı dağıtmaktır. İşletim sisteminin yeni sürümlerinin dağıtılması dahil olmak üzere, üzerinde çalıştığı platformun yönetimi sizin için gerçekleştirilir.

Bulut Hizmetleri ile sanal makineler oluşturmazsınız. Bunun yerine, üç web rolü örneği ve iki çalışan rolü örneği gibi Azure'a her birinden kaç tane istediğinizi söyleyen bir yapılandırma dosyası belirtirsiniz ve platform bunu sizin yerinize oluşturur.  Bu sanal makinelerin hangi boyutta olduğunu yine siz belirlersiniz (seçenekler Azure Sanal Makineler ile aynıdır), ancak bunları doğrudan kendiniz oluşturmazsınız. Uygulamanızın daha büyük bir yük taşıması gerekiyorsa daha fazla sanal makine isteyebilirsiniz; Azure bu örnekleri oluşturur. Yük azalırsa bu örnekleri kapatabilir ve bunlar için ödeme yapmayı bırakabilirsiniz.

Bir Bulut Hizmetleri uygulaması, genellikle iki adımlı bir işlemle kullanıma sunulur. Bir geliştirici, öncelikle uygulamayı platformun hazırlık alanına yükler. Geliştirici, uygulamayı canlı hale getirmeye hazır olduğunda üretime sokulmasını istemek üzere Azure Yönetim Portalı'nı kullanır. Hazırlık ile üretim arasındaki bu geçiş, herhangi bir kapalı kalma süresi olmadan yapılabilir; bu durum, uygulamanın kullanıcılarını rahatsız etmeden yeni bir sürüme yükseltilmesini sağlar. 

Bulut Hizmetleri ayrıca izleme sağlar. Azure Sanal Makineler gibi arızalı bir fiziksel sunucuyu algılar ve bu sunucu üzerinde çalışan sanal makineleri yeni bir makinede yeniden başlatır. Ancak, Bulut Hizmetleri yalnızca donanım hatalarını değil, aynı zamanda arızalı sanal makineleri ve uygulamaları da algılar. Sanal Makineler'in aksine her bir web ve çalışan rolünün içinde bir aracı vardır ve bu sayede hatalar oluştuğunda yeni sanal makineleri ve uygulama örneklerini başlatabilir.

Bulut Hizmetleri'nin PaaS niteliği başka anlamlar da doğurur. En önemlilerinden biri, bu teknoloji üzerinde oluşturulan uygulamaların, herhangi bir web veya çalışan rolü örneği arıza yaptığında doğru şekilde çalışmak üzere yazılması gerektiğidir. Bunu sağlamak için bir Bulut Hizmetleri uygulamasının kendi sanal makine dosya sistemindeki durumu sürdürmemesi gerekir. Azure Sanal Makineler ile oluşturulan sanal makinelerin aksine, Bulut Hizmetleri sanal makinelerine yazılan öğeler kalıcı değildir; Sanal Makineler veri diski diye bir şey yoktur. Bunun yerine, Bulut Hizmetleri uygulaması tüm durumu doğrudan SQL Database, blob'lar, tablolar veya başka dış depolama birimlerine yazmalıdır. Uygulamaların bu şekilde oluşturulması, ölçeklendirilmelerini kolaylaştırır, hataya daha dayanıklı hale gelmesini sağlar ve bunların her ikisi de Bulut Hizmetleri'nin önemli amaçlarıdır.


<h2><a id="WhatShouldIUse"></a>Ne Kullanmalıyım? Seçim Yapma</h2>

Üç Azure yürütme modeli de bulutta ölçeklenebilir ve güvenilir uygulamalar oluşturmanızı sağlar. Bu temel benzerlik göz önünde bulundurulduğunda hangisini seçmelisiniz? Yanıt, ne yapmaya çalıştığınıza göre değişir.

Sanal Makineler en genel çözümü sağlar. Mümkün olan en büyük denetimi istiyorsanız veya geliştirme ve sınama gibi amaçlarla genel sanal makinelere ihtiyacınız varsa en iyi seçenek budur. Sanal Makineler ayrıca daha önce SharePoint örneğinde gösterildiği gibi hazır şirket içi uygulamaları bulutta çalıştırmak için en iyi seçenektir. Bu teknolojiyle oluşturduğunuz sanal makineler tıpkı şirket içi sanal makineleriniz gibi görünebileceğinden, olağanüstü durum kurtarma için de en iyi seçenek olabilir. Bunu dengeleyen şey, elbette güç arttıkça sorumluluğun artmasıdır; IaaS bazı yönetim işlerini üstlenmenizi gerektirir.  

Basit bir web sitesi oluşturmak istediğinizde, Web Siteleri doğru seçenektir. Siteniz Joomla, WordPress veya Drupal gibi var olan bir uygulamaya dayanacaksa bu durum özellikle geçerlidir. Web Siteleri ayrıca, yüksek oranda ölçeklenebilir olması gerekenler dahil düşük yönetimli bir web uygulaması oluşturmak veya var olan bir IIS web uygulamasını ortak buluta taşımak için iyi bir seçimdir. Aynı zamanda hızlı dağıtım da sağlar. Uygulamanızın yeni bir örneği neredeyse hemen çalışmaya başlayabilirken, Sanal Makineler veya Bulut Hizmetleri ile yeni bir sanal makinenin dağıtılması birkaç dakika sürebilir. 

Azure tarafından sağlanan ilk yürütme modeli olan Bulut Hizmetleri doğrudan bir PaaS yaklaşımıdır. PaaS ile web barındırma arasındaki çizgi belirgin olmasa da, Bulut Hizmetleri ile Web Siteleri arasında aşağıdaki gibi bazı önemli farklılıklar vardır:

- Web Siteleri'nin aksine Bulut Hizmetleri, uygulamanızın sanal makinelerine yönetim erişimi sağlar. Bu özellik, uygulamanızın ihtiyaç duyduğu isteğe bağlı yazılımı yüklemenizi sağlar ve Web Siteleri'nde bulunmayan bir özelliktir.
- Bulut Hizmetleri hem web rolleri hem de çalışan rolleri sağladığından, iş mantığı için ayrı sanal makinelere ihtiyaç duyan çok katmanlı uygulamalar için daha iyi bir seçenektir.
- Bulut Hizmetleri ayrı hazırlık ve üretim ortamları sağlar ve uygulama güncelleştirmelerini Web Siteleri'nden biraz daha kolay hale getirir. 
- Web Siteleri'nin aksine Azure Sanal Ağı ve Azure Connect gibi ağ teknolojilerini kullanarak şirket içi bilgisayarları Bulut Hizmetleri uygulamalarına tutturabilirsiniz. 
- Bulut Hizmetleri, Web Siteleri'nde mümkün olmayan bir özellik olarak uygulamanın sanal makinelerine doğrudan bağlanmak için Uzak Masaüstü'nü kullanmanızı sağlar.

PaaS olduğu için Bulut Hizmetleri ayrıca Azure Sanal Makineler karşısında da bazı avantajlar sunar. Örneğin, işletim sistemi güncelleştirmelerini dağıtma gibi daha fazla yönetim görevi sizin yerinize gerçekleştirilir ve böylece çalıştırma maliyetleriniz Azure Sanal Makineler'in IaaS yaklaşımından daha düşük olur.

Üç Azure yürütme modelinin tamamı olumlu ve olumsuz yönlere sahiptir. En iyi seçimin yapılması için bunların anlaşılması, ne yapmaya çalıştığınızın bilinmesi ve ardından en uygun seçeneğin belirlenmesi gerekir.

Bazı durumlarda hiçbir yürütme modeli tek başına doğru değildir. Bunun gibi durumlarda seçeneklerin birleştirilmesi tamamen uygundur. Örneğin, Bulut Hizmetleri web rollerinin yönetim faydalarına ihtiyaç duyduğunuz ve diğer yandan uyumluluk veya performans nedeniyle standart SQL Server kullanmak istediğiniz bir uygulama oluşturduğunuzu varsayalım. Bu durumda en iyi seçenek, [Şekil 7](#Fig7)'de gösterildiği gibi yürütme modellerinin birleştirilmesidir.

<a name="Fig7"></a>![07_CombineTechnologies][07_CombineTechnologies] 
 
**Şekil 7: Tek bir uygulama birden çok yürütme modeli kullanabilir.**

Şekilde gösterildiği gibi Bulut Hizmetleri sanal makineleri, Sanal Makineler sanal makinelerinden ayrı bir bulut hizmetinde çalışır. Yine de bu ikisi oldukça verimli bir şekilde iletişim kurabilir, bu nedenle uygulamanın bu şekilde oluşturulması en iyi seçenektir.

Bulut platformlarının birçok farklı senaryoyu desteklemesi gerektiğinden Azure, farklı yürütme modelleri sunar. Bu platformu etkili biçimde kullanmak isteyen herkes (buraya kadar okuduysanız siz de) bunların her birini anlamalıdır.

[01_CreatingVMs]: ./media/fundamentals-application-models/ExecModels_01_CreatingVMs.png
[02_CloudServices]: ./media/fundamentals-application-models/ExecModels_02_CloudServices.png
[03_AppUsingSQLServer]: ./media/fundamentals-application-models/ExecModels_03_AppUsingSQLServer.png
[04_SharePointFarm]: ./media/fundamentals-application-models/ExecModels_04_SharePointFarm.png
[05_Websites]: ./media/fundamentals-application-models/ExecModels_05_Websites.png
[06_CloudServices2]: ./media/fundamentals-application-models/ExecModels_06_CloudServices2.png
[07_CombineTechnologies]: ./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png


