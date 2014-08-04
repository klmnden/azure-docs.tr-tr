<properties  umbracoNaviHide="0" pageTitle="Application Model" metaKeywords="Azure, Azure, application model, Azure application model, development model, Azure development model, hosted service, Azure hosted service, web role, worker role" description="Learn about the Azure hosted service application model. Understand core concepts, design considerations, defining and configuring your application, and scaling." linkid="dev-net-fundamentals-application-model" urlDisplayName="Application Model" headerExpose="" footerExpose="" disqusComments="1" title="Application Model" authors="" />

# Azure Yürütme Modelleri

Azure, uygulamaları çalıştırmak için farklı yürütme modelleri sağlar. Her model farklı bir hizmet kümesi sağlar. Böylece yapmak istediğiniz işe uygun modeli seçebilirsiniz. Bu makalede üç modele yer verilmekte, modellerin teknolojileri ve kullanım şekilleri örneklerle açıklanmaktadır.

## İçindekiler

* [Sanal Makineler](#VMachine)
* [Web Siteleri](#WebSites)
* [Bulut Hizmetleri](#CloudServices)
* [Ne Kullanmalıyım? Seçim Yapma](#WhatShouldIUse)

<h2><a  id="VMachine" ></a>Sanal Makineler</h2>


Azure Sanal Makineleri geliştiricilerin, BT faaliyetlerini yürütenlerin ve diğer kişilerin bulutta sanal makineler oluşturmasına ve kullanmasına olanak tanır. Farklı şekillerde kullanılabilen *Hizmet olarak Altyapı (IaaS)* teknolojisini sağlar. [Şekil 1](#Fig1)'de bu teknolojinin temel bileşenleri gösterilmiştir.

<a name="Fig1"></a>![01_CreatingVMs](./media/fundamentals-application-models/ExecModels_01_CreatingVMs.png)

**Şekil 1: Azure Sanal Makineleri Hizmet olarak Altyapı sağlar.**

Şekilde gösterildiği gibi Azure Yönetim Portal'ını veya REST tabanlı Azure Servis Yönetimi API'sini kullanarak sanal makineler oluşturabilirsiniz. Internet Explorer, Mozilla ve Chrome gibi popüler tarayıcılarla Yönetim Portalı'na erişebilirsiniz. Microsoft, REST API için Windows, Linux ve Macintosh sistemlerine yönelik istemci komut dosyası araçları sağlar. Diğer yazılımlar da bu arabirimi ücretsiz olarak kullanabilir.

Platforma hangi şekilde erişirseniz erişin, yeni bir Sanal Makine oluşturmak için, Sanal Makinenin görüntüsünün depolanacağı bir sanal sabit disk (VHD) oluşturmanız gerekir. VHD'ler Azure blob'larında saklanır.

Kullanmaya başlamak için iki seçeneğiniz vardır.

1.  Kendi VHD'nizi yükleme 2.  Azure Sanal Makineleri galerisinde veya Microsoft açık kaynaklı
    [VMDepot][1] web sitesinde Microsoft ve iş ortakları tarafından
    sağlanan VHD'leri kullanma.

Galeride ve VMDepot'da bulunan VHD'ler, Microsoft ve diğer üçüncü taraf ürünlerinin de yüklü olduğu yeni Microsoft ve Linux işletim sistemi görüntüleri içerir. Seçenekler sürekli olarak artmaktadır. Örnekler, aşağıdaki işletim sistemlerinin ve yazılımların çeşitli sürümlerini ve yapılandırmalarını içermektedir:

* Windows Server
* Suse, Ubuntu ve CentOS gibi Linux sunucuları
* SQL Server
* BizTalk Server
* SharePoint Server

VHD'nin yanı sıra yeni sanal makinenizin boyutunu da belirlersiniz. Her boyuttaki sanal makineler için tüm özellikler [Azure kitaplığında][2] listelenmiştir.

* **Çok Küçük**, paylaşılan bir çekirdek ve 768 MB bellek.
* **Küçük**, 1 çekirdek ve 1,75 GB bellek.
* **Orta**, 2 çekirdek ve 3,5 GB bellek.
* **Büyük**, 4 çekirdek ve 7 GB bellek.
* **Çok Büyük**, 8 çekirdek ve 14 GB bellek.
* **A6**, 4 çekirdek ve 28 GB bellek.
* **A7**, 8 çekirdek ve 56 GB bellek.

Son olarak yeni sanal makinenizin çalıştırılacağı veri merkezini (ABD, Avrupa veya Asya) seçin.

Sanal makine çalıştırıldıktan sonra çalıştırılan her saat için ücret ödersiniz. Sanal makineyi kaldırdığınızda ödeme durdurulur. Ödediğiniz miktar, sanal makinenizin ne kadar kullanıldığına değil, yalnızca saate bağlıdır. Bir saat boyunca boş olarak bekleyen bir sanal makinenin ücreti, aşırı yüklü bir sanal makinenin ücretiyle aynıdır.

Çalışan her sanal makinenin blob'da ilgili bir *İşletim sistemi diski* bulunur. Galeri VHD'si kullanarak sanal makine oluşturursanız bu VHD, sanal makinenizin işletim sistemi diskine kopyalanır. Çalışan sanal makinenizin işletim sisteminde yaptığınız tüm değişiklikler burada saklanır. Örneğin, Windows kayıt defterini değiştiren bir uygulama yüklediğinizde bu değişiklik sanal makinenizin işletim sistemi diskinde saklanır. Gelecek sefer bu işletim sisteminden bir sanal makine oluşturduğunuzda sanal makine, bıraktığınız durumdan aynı şekilde çalışmaya devam eder. Microsoft, gerekli olduğunda galeride saklanan VHD'ler için işletim sistemi yamaları gibi güncelleştirmeleri uygular. Bununla birlikte, Windows Update varsayılan olarak açık olmasına rağmen kendi işletim sistemi disklerinizdeki VHD'ler için bu güncelleştirmeleri sizin yüklemeniz gerekir.

Çalışan sanal makineler üzerinde değişiklik yapılabilir ve ardından bu sistemler sysprep aracı ile yakalanabilir. VHD görüntüsünü kullanarak aynı genel yapılandırmaya sahip başka sanal makineler oluşturulabilmesi için Sysprep makine adı gibi belirli bilgileri kaldırır. Bu görüntüler, başka sanal makineler için bir başlangıç noktası olarak kullanılmak üzere karşıya yüklenmiş görüntülerinizle birlikte Yönetim portalında saklanır.

Sanal Makineler ayrıca, oluşturduğunuz her sanal makineyi barındıran donanımı da izler. Sanal makine çalıştıran bir fiziksel sunucu sorunla karşılaştığında platform bu durumu fark eder ve aynı sanal makineyi başka bir makinede başlatır. Doğru lisansa sahip olduğunuzda, değiştirilmiş bir VHD'yi işletim sistemi diskinizden kopyalayabilir ve şirket içindeki veri merkezinizde veya başka bir bulut sağlayıcısı gibi farklı bir yerde çalıştırabilirsiniz.

İşletim sistemi diskinin yanı sıra sanal makinenizde bir veya daha fazla veri diski bulunur. Bu disklerin her biri sanal makinenize bağlı disk sürücüleri gibi görünse de her diskin içeriği bir blob'da saklanır. Veri diski üzerindeki her yazma işlemi temel blob'da kalıcı olarak saklanır. Tüm blob'larda olduğu gibi Azure hatalara karşı korumak için bu blob'ları bir veri merkezinde veya farklı veri merkezlerinde çoğaltır.

Çalışan sanal makineler Yönetim Portalı, PowerShell ve diğer komut dosyası araçları ile veya doğrudan REST API ile yönetilebilir. (İşin aslı, Yönetim Portalı ile yapılabileceğiniz her işlem bu API aracılığıyla programlama yoluyla da gerçekleştirilebilir.) RightScale ve ScaleXtreme gibi Microsoft iş ortakları, REST API'ya bağlı olan yönetim hizmetleri de sağlar.

Azure Sanal Makineler farklı şekillerde kullanılabilir. Microsoft'un hedeflediği birincil senaryolar aşağıdakileri içerir:

* **Geliştirme ve test amaçlı Sanal Makine.** Geliştirme grupları,
  uygulama oluşturmak için genellikle belirli yapılandırmaya sahip sanal
  makinelere ihtiyaç duyar. Azure Sanal Makineleri, sanal makineleri
  kolay ve ekonomik bir şekilde oluşturmanızı, kullanmanızı ve ihtiyaç
  duyulmadığında kaldırmanızı sağlar.
* **Bulutta uygulama çalıştırma.** Bazı uygulamaları genel bulutta
  çalıştırmak daha ekonomiktir. Yoğun şekilde talep edilen bir
  uygulamayı düşünün. Bu uygulamayı çalıştırmak için veri merkezinize
  yeterli sayıda makine alabilirsiniz ancak bu makineler çoğu zaman
  kullanılmayacaktır. Bu uygulamayı Azure'da çalıştırdığınızda ek sanal
  makineler için yalnızca ihtiyaç duyduğunuzda ödeme yaparsınız ve yoğun
  talep ortadan kalkınca makineleri kapatabilirsiniz. Bir diğer örnek
  olarak, bilgi işlem kaynaklarına taahhütsüz ve hızlı bir şekilde,
  ihtiyacınız olduğunda ulaşmayı isteyen yeni kurulmuş bir şirket
  olduğunuzu varsayın. Bu durumda da Azure doğru seçimdir.
* **Veri merkezinizi genel buluta genişletme.** Kuruluşunuz Azure Sanal
  Ağ sayesinde, bir Azure sanal makine grubunun şirket içi ağınızın bir
  parçası olarak görünmesini sağlayan bir sanal ağ (VNET) oluşturabilir.
  Bu, SharePoint ve diğer uygulamaları Azure'da çalıştırmanızı ve kendi
  veri merkezinize göre daha kolay dağıtım ve/veya daha masrafsız
  çalıştırma imkanı sunabilen bir yaklaşım sağlar.
* **Olağanüstü durum kurtarma.** IaaS tabanlı olağanüstü durum kurtarma,
  sık kullanılmayan bir yedek veri merkezi için sürekli ödeme yapmak
  yerine gerekli bilgi işlem kaynaklarına yalnızca gerçekten ihtiyaç
  duyduğunuzda ödeme yapmanızı sağlar. Örneğin birincil veri
  merkezinizle ilgili bir sorun olduğunda Azure üzerinde gerekli
  uygulamaları çalıştıran sanal makineler oluşturabilir ve bu makinelere
  artık ihtiyaç duyulmadığında onları kapatabilirsiniz.

Gerçekleşebilecek olasılıklar bunlarla sınırlı değildir ancak bunlar, Azure Sanal Makinelerin kullanımıyla ilgili iyi örneklerdir.

### Sanal Makineleri Gruplama: Bulut Hizmetleri

Azure Sanal Makineleri ile yeni bir sanal makine oluşturduğunuzda, makineyi tek başına çalıştırabilir veya *bulut hizmetinde* çalışan bir sanal makine grubuna ekleyebilirsiniz. (Benzer isimlere sahip olabilirler ancak bu kavramı, Azure'un PaaS teknolojisinin adı olan Bulut Hizmetleri ile karıştırmayın. İkisi birbirinden farklıdır.) Aynı bulut hizmetinde yer alan sanal makinelere bir genel IP adresi üzerinden erişilebilirken, tek başına çalışan sanal makinelerin her birine, o makineye ait genel bir IP adresi atanmıştır. Bu durum [Şekil 2](#Fig2)'de gösterilmiştir.

<a name="Fig2"></a>![02_CloudServices](./media/fundamentals-application-models/ExecModels_02_CloudServices.png)

**Şekil 2: Bir bulut hizmetinde gruplanmış sanal makinelere bir genel IP adresi kullanılarak erişilebilirken, tek başına çalışan sanal makinelerin her biri, o makineye ait genel bir IP adresine sahiptir.**

Örneğin, basit bir uygulama oluşturma ve test etme amacıyla bir sanal makine oluştururken büyük olasılıkla tek bir sanal makine kullanırsınız. Web ön ucuna, veri tabanına hatta belki bir orta katmana sahip çok katmanlı bir uygulama oluştururken büyük olasılıkla birden fazla sanal makineyi Şekil 2'de gösterildiği gibi bir bulut hizmetine bağlarsınız.

Sanal makineleri bir bulut hizmetinde gruplandırmak, başka seçenekleri kullanmanıza da olanak sağlar. Azure, kullanıcı isteklerini makineler arasında dağıtarak aynı bulut hizmetindeki sanal makineler için yük dengeleme sağlar. Bu şekilde bağlanmış sanal makineler, Azure veri merkezindeki yerel ağ üzerinden birbirleriyle doğrudan iletişim kurabilirler.

Ayrıca aynı bulut hizmetinde bulunun sanal makineler, bir veya daha fazla *kullanılabilirlik kümesinde* gruplandırılabilir. Bu özelliğin önemini anlamak için birçok ön uç sanal makinesi çalıştıran bir web
uygulaması düşünün. Bu sanal makinelerin tümü aynı fiziksel makineye veya aynı raftaki makinelere dağıtıldığında, tek bir donanım hatası, tüm sanal makinelerin erişilmez hale gelmesine neden olabilir. Ancak sanal makineler kullanılabilirlik kümesinde gruplandırıldığında Azure bunları veri merkezine dağıtır. Böylece hiçbiri tek hata noktasını paylaşmaz.

### Senaryo: Bir Uygulamayı SQL Server ile Çalıştırma

Azure Sanal Makineleri'nin nasıl çalıştığını daha iyi anlamak için birkaç ayrıntılı senaryoya bakmak yararlı olacaktır. Azure'da çalışan güvenilir ve ölçeklendirilebilen bir web uygulaması oluşturmak istediğinizi düşünün. Bunu yapmanın bir yolu, uygulamanın mantığını bir veya daha fazla Azure sanal makinesinde çalıştırmak ve veri yönetimi için de SQL Server kullanmaktır. Bu durum [Şekil 3](#Fig3)'te gösterilmiştir.

<a name="Fig3"></a>![03_AppUsingSQLServer](./media/fundamentals-application-models/ExecModels_03_AppUsingSQLServer.png)

**Şekil 3: Azure Sanal Makineleri'nde çalışan bir uygulama, depolama için SQL Sunucusunu kullanabilir.**

Bu örnekte, galeride bulunan standart VHD'lerden her iki türde sanal makine oluşturulur. Uygulamanın mantığı Windows Server 2008 R2'de çalıştırılır. Böylece geliştirici bu VHD'den üç sanal makine oluşturur ve ardından uygulamasını bu makinelere yükler. Sanal makinelerin tümü aynı bulut hizmetinde bulunduğundan, geliştirici, yükün makineler arasında dağıtılması için donanım yük dengelemesini yapılandırılabilir. Geliştirici ayrıca içinde SQL Server 2012'yi barındıran galeri VHD'sini kullanarak iki sanal makine oluşturabilir. Makineleri çalıştırdıktan sonra her örnekteki SQL Server'ı, otomatik yük devretme özelliğine sahip veritabanı yansıtması özelliğini kullanacak şekilde yapılandırır. Bu işlem zorunlu değildir ve uygulama yalnızca bir SQL Server örneği kullanabilir ancak bu yaklaşım güvenilirliği artırır.

### Senaryo: SharePoint Grubu Çalıştırma

Bir kurumun SharePoint grubu oluşturmak istediğini ancak bu grubu kendi veri merkezinde çalıştırmak istemediğini düşünün. Şirket içi veri merkezinin kaynakları kısıtlı olabilir veya grubu oluşturan iş birimi, iç BT grubuyla ilgilenmek istemiyor olabilir. Buna benzer durumlarda Azure Sanal Makineleri'nde SharePoint çalıştırmak daha uygun olabilir. Bu durum [Şekil 4](#Fig4)'te gösterilmiştir.

<a name="Fig4"></a>![04_SharePointFarm](./media/fundamentals-application-models/ExecModels_04_SharePointFarm.png)

**Şekil 4: Azure Sanal Makineleri, bulutta SharePoint grubu çalıştırmaya izin verir.**

SharePoint grubu, her biri farklı VHD'lerden oluşturulmuş bir Azure sanal makinesinde çalışan çok sayıda bileşene sahiptir. Bunun için gerekli olanlar:

* Microsoft SharePoint. Galeride deneme görüntüleri bulunur veya kurum
  kendi VHD'sini sağlar.
* Microsoft SQL Server. SharePoint, SQL Server'a bağlıdır. Bu nedenle
  kurum, standart bir galeri VHD'sinden SQL Server 2012 çalıştıran sanal
  makineler oluşturur.
* Windows Server Active Directory. SharePoint ayrıca Active Directory
  gerektirir. Bu nedenle kurum, galerideki bir Windows Server
  görüntüsünü kullanarak etki alanı denetleyicileri oluşturur. Bu işlem
  zorunlu değildir ve şirket içi etki alanı denetleyicileri de
  kullanılabilir ancak SharePoint, Active Directory ile sıkça etkileşim
  kurduğundan burada gösterilen yaklaşım daha iyi performans sağlar.

Şekilde gösterilmese de bu SharePoint grubu büyük olasılıkla Azure Sanal Ağı kullanan bir şirket içi ağa bağlıdır. Böylece sanal makineler ve içerdikleri uygulamalar, bu ağı kullananlar için yerel ağda görünür.

Örneklerde görüldüğü gibi Azure Sanal Makineleri'ni bulutta yeni uygulamalar oluşturmak ve çalıştırmak, varolan uygulamaları çalıştırmak veya başka amaçlar için kullanabilirsiniz. Hangi seçeneği belirlerseniz belirleyin, teknolojinin hedefi aynıdır: Genel bulut işlemlerine yönelik genel amaçlı bir temel sağlamak.

<h2><a  id="WebSites" ></a>Web Siteleri</h2>


İnsanlar Web teknolojilerini farklı şekillerde kullanırlar. Bir şirket, haftada milyonlarca defa ziyaret edilecek ve dünyanın farklı yerlerindeki veri merkezlerine dağıtılacak bir varlık sitesini taşımak veya kurulumunu yapmak isteyebilir. Bir web tasarımı ajansı, binlerce kullanıcıyı yönetebilecek özel bir web uygulaması oluşturmak için bir geliştirici ekibiyle çalışabilir. Bir kurumsal geliştirici, Bulut'taki gider raporlarını yetkili kullanıcılar için kurumsal Active Directory ile izleyecek bir uygulama kurmak isteyebilir. Bir BT danışmanı, küçük işletmelere yönelik bir içerik yönetim sistemi kurmak için açık kaynaklı popüler bir uygulama kullanabilir. Bu işlerin tümü Azure Sanal Makineleri ile gerçekleştirilebilir. Ham sanal makineleri oluşturmak ve yönetmek bir miktar beceri ve çalışma gerektirir. Bir web sitesi veya web uygulaması kullanmak istiyorsanız daha kolay (ve ucuz) bir çözüm vardır. Bu da, genellikle Hizmet olarak Platform (PaaS) adıyla bilinen yaklaşımdır. Şekil 5'te gösterildiği gibi Azure, Web Sitelerine bu
özelliği sunmaktadır.

<a name="Fig5"></a>![05_Websites](./media/fundamentals-application-models/ExecModels_05_Websites.png)

**Şekil 5: Azure Web Siteleri; statik web sitelerini, popüler web uygulamalarını ve çeşitli teknolojilerle oluşturulmuş özel web uygulamalarını destekler.**

Azure Web Siteleri, web uygulamaları çalıştırmak üzere iyileştirilmiş Hizmet olarak Platform çözümü oluşturmak için Azure Bulut Hizmetleri üzerinde kurulmuştur. Web Siteleri, şekilde gösterildiği gibi farklı kullanıcılar tarafından oluşturulmuş birçok web sitesini içeren tek bir sanal makine kümesinde veya tek kullanıcıya ait standart sanal makineler üzerinde çalışır. Sanal makineler, Azure Web Siteleri tarafından yönetilen kaynak havuzunun bir parçasıdır. Bu nedenle yüksek güvenilirlik ve hataya dayanaklılık sağlar. Kullanması çok kolaydır. Kullanıcılar, Azure Web Siteleri sayesinde farklı uygulamalar, çerçeveler ve şablonlar arasından seçim yapabilir ve saniyeler içerisinde bir web sitesi oluşturabilir. Takım olarak sürekli tümleştirme ve geliştirme sağlamak için tercih ettikleri geliştirme araçlarını (WebMatrix, Visual Studio, vb.) ve kaynak kontrol seçeneklerini kullanabilir. MySQL DB'ye bağlı uygulamalar, bir Microsoft iş ortağı olan ClearDB tarafından Azure için sağlanan MySQL hizmetini kullanabilir. Geliştiriciler Web Siteleri ile büyük, ölçeklendirilebilir web uygulamaları oluşturabilir. Bu teknoloji; ASP.NET, PHP, Node.js ve Python kullanarak uygulama oluşturmayı desteklemektedir. Uygulamalar sabit oturumlar kullanabilir; örneğin mevcut web uygulamaları hiçbir değişiklik gerçekleştirilmeksizin bu bulut platformuna taşınabilir. Web Siteleri'nde oluşturulan uygulamalar Hizmet Veri Yolu, SQL Veritabanı ve Blob Depolama gibi diğer Azure özelliklerinden ücretsiz olarak yararlanabilir. Ayrıca bir uygulamanın kopyalarını farklı sanal makinelerde çalıştırabilirsiniz; Web Siteleri, yük dengeleme ile istekleri sanal makineler arasında otomatik olarak dağıtır. Zaten varolan sanal makinelerde yeni Web Siteleri örnekleri oluşturulduğundan yeni bir uygulama örneği başlatma işlemi çok hızlıdır. Yeni sanal makine oluşturmaktan daha kısa sürede tamamlanır. [Şekil 5](#Fig5)'te gösterildiği gibi kodları ve diğer web içeriklerini Web Siteleri'ne farklı şekillerde yayımlayabilirsiniz. FTP, FTPS veya Microsoft WebDeploy teknolojisini kullanabilirsiniz. Web Siteleri aynı zamanda Git, GitHub, CodePlex, BitBucket, Dropbox, Mercurial, Team Foundation Server, ve bulut tabanlı Team Foundation Hizmeti gibi kaynak kontrol sistemlerinden kod yayımlamayı da desteklemektedir.

<h2><a  id="CloudServices" ></a>Bulut Hizmetleri</h2>


Azure Sanal Makineleri IaaS, Azure Web Siteleri ise web barındırma sağlamaktadır. Üçüncü işlem seçeneği olan Bulut Hizmetleri, *Hizmet olarak Platform (PaaS)* sağlar. Bu teknoloji ölçeklendirilebilen, güvenilir ve ucuz bir şekilde çalıştırılabilen uygulamaları desteklemek üzere tasarlanmıştır. Bu aynı zamanda geliştiricilerin, kullandıkları platformun yönetimiyle ilgilenmelerine gerek kalmaksızın tümüyle uygulamalarına odaklanmalarına imkan tanır. [Şekil 6](#Fig6) bu fikri açıklamaktadır.

<a name="Fig6"></a>![06_CloudServices2](./media/fundamentals-application-models/ExecModels_06_CloudServices2.png)

**Şekil 6: Azure Bulut Hizmetleri Hizmet olarak Platform sağlar.**

Diğer Azure işlem seçenekleri gibi Bulut Hizmetleri de sanal makinelere bağlıdır. Bu teknoloji, çok az farka sahip iki sanal makine seçeneği sunar: *Web rollerinin* örnekleri IIS ile Windows Server'ın bir türünü çalıştırır, *çalışan rollerinin* örnekleri ise aynı Windows Server türünü, IIS olmadan çalıştırır. Bulut Hizmeti uygulaması bu iki seçeneğin birleşimine bağlıdır.

Örneğin basit bir uygulama yalnızca web rolü kullanırken, daha karmaşık bir uygulama gelen istekleri yönetmek için web rolü ve ardından bu isteklerin oluşturduğu işleri işlemek için çalışan rolü kullanabilir. (Bu iletişim, Hizmet Veri Yolu veya Azure Sıraları kullanabilir.)

Şekilde gösterildiği gibi tek bir uygulamadaki sanal makinelerin tümü (Azure Sanal Makineleri konusunda daha önce açıklanan şekilde) aynı bulut hizmetinde çalışır. Bu nedenle kullanıcılar uygulamaya tek bir genel IP adresi üzerinden erişirler ve istekler yük dengeleme ile uygulamanın sanal makineleri arasında otomatik olarak dağıtılır. Azure Sanal Makineleri ile oluşturulan bulut hizmetlerinde olduğu gibi platform sanal makineleri, Bulut Hizmetleri'nde tek bir donanım hata noktasını paylaşmayacak şekilde dağıtır.

Uygulamalar sanal makinelerde çalışsalar da Bulut Hizmetleri'nin IaaS değil PaaS sağladığını unutmayın. Bunu şu şekilde düşünebilirsiniz: Azure Sanal Makineleri gibi bir IaaS ile uygulamanızın çalıştırılacağı ortamı oluşturduktan ve yapılandırdıktan sonra uygulamanızı bu ortama dağıtırsınız. Bu ortamda, sanal makinelerdeki işletim sisteminin düzeltme sürümlerini dağıtma gibi işlerin büyük bir bölümünü yönetmeniz gerekir. Buna karşın PaaS, ortamın zaten var olduğunu kabul eder. Tüm yapmanız gereken uygulamanızı dağıtmaktır. Yeni işletim sistemi sürümlerinin dağıtımı da dahil olmak üzere, çalıştığı platformun yönetim işlemleri sizin yerinize gerçekleştirilir.

Bulut Hizmetleri ile sanal makineler oluşturmazsınız. Bunun yerine Azure'a her birinden kaç tane istediğinizi bildiren (üç web rolü örneği ve iki çalışan rolü örneği gibi) bir yapılandırma dosyası sağlarsınız ve platform da bunları sizin için oluşturur. Bu sanal makinelerin boyutlarını yine siz seçersiniz (seçenekler Azure Sanal Makineleri'ndeki seçeneklerin aynısıdır) ancak makineleri bizzat oluşturmazsınız. Uygulamanızın büyük boyutlarda bir yükü yönetmesi gerekiyorsa daha fazla sanal makine istersiniz ve Azure bu örnekleri oluşturur. Yük azalırsa bu örnekleri kapatırsınız ve ücret ödemeye devam etmezsiniz.

Kullanıcılar, Bulut Hizmetleri uygulamasını genellikle iki adımlı bir işlemle kullanabilirler. Geliştirici öncelikle uygulamayı platformun hazırlık alanına yükler. Geliştirici uygulamayı çalıştırmaya hazır olduğunda, üretime sokmak için Azure Yönetim Portalı'nı kullanır. Hazırlık ve üretim arasındaki geçiş sırasında uygulama kapalı kalmaz. Böylece çalışan bir uygulama, kullanıcılara kesinti yaşatmadan yeni sürüme yükseltilebilir.

Bulut Hizmetleri izleme hizmeti de sunar. Aynı Azure Sanal Makinelerinde olduğu gibi, fiziksel sunucuda bir hata algılandığında bu sunucuda çalışan sanal makineler, yeni bir makine üzerinde yeniden başlatılır. Bulut Hizmetleri, yalnızca donanım hatalarını değil, sanal makine ve uygulamalardaki hataları da algılar. Sanal Makineler'in aksine her web ve çalışan rolü içinde bir aracıya sahip olduğundan hata oluştuğunda yeni sanal makine ve uygulama başlatabilir.

Bulut Hizmetlerinin PaaS özelliğinin başka etkileri de vardır. En önemlilerinden biri, bu teknolojide oluşturulan uygulamaların, web veya çalışan rolü örneğinde hatayla karşılaşıldığında düzgün çalışacak şekilde yazılmış olmaları gerekliliğidir. Bunu başarmak için Bulut Hizmetleri uygulamasının, kendi sanal makinesinin dosya sistemi durumunu korumaması gerekir. Azure Sanal Makineleri ile oluşturulan sanal makinelerin aksine Bulut Hizmetleri sanal makinelerindeki yazma işlemi kalıcı değildir, Sanal Makinler veri diski gibi bir öğe yoktur. Bunun yerine Bulut Hizmetleri uygulaması, tüm durumu SQL Veritabanına, blob'lara, tablolara ve bir takım harici depolama alanlarına açık olarak yazmalıdır. Bu şekilde oluşturulan uygulamalar daha kolay
ölçeklendirilebilir ve daha az hatayla karşılaşır; bu iki özellik Bulut
Hizmetleri'nin temel hedeflerindendir.

<h2><a  id="WhatShouldIUse" ></a>Ne Kullanmalıyım? Seçim Yapma</h2>


Üç Azure yürütme modeli de bulutta ölçeklendirilebilen ve güvenilir uygulamalar oluşturmanıza imkan tanır. Birbirine benzeyen bu üç modelden hangisini kullanmalısınız? Yanıt, ne yapmak istediğinize göre değişir.

Sanal Makineler en genel çözümü sunar. En fazla denetimi sağlamak istiyorsanız veya geliştirme ve test amacıyla genel sanal makinelere ihtiyacınız varsa bu en iyi seçenektir. Sanal Makineler, daha önce SharePoint örneğinde açıklandığı gibi, rafta olmayan şirket içi uygulamaları bulutta çalıştırmak için en iyi seçimdir. Bu teknolojiyle oluşturduğunuz sanal makineler, şirket içi sanal makinelerinize benzediğinden olağanüstü durum kurtarma için en iyi seçimdir. Büyük güç büyük sorumluluk getirir; IaaS özelliği bir takım yönetim işlemleri yapmanızı gerektirir.

Web Siteleri'ni kullanarak basit bir web sitesi oluşturabilirsiniz. Siteniz Joomla, WordPress veya Drupal gibi varolan bir uygulamaya dayalıysa bu işlem daha basittir. Web Siteleri düşük seviyede yönetim gerektiren web uygulamaları (tam ölçeklendirilmesi gerekenleri bile) oluşturmak veya varolan bir IIS web uygulamasını genel buluta taşımak için kullanabilirsiniz. Aynı zamanda hızlı dağıtım imkanı tanır. Uygulamanızın yeni örneği neredeyse anında başlatılır, Sanal Makineler veya Bulut Hizmetleri ile yeni bir sanal makine dağıtmak birkaç dakika sürebilir.

Azure tarafından sağlanan ilk yürütme modeli olan Bulut Hizmetleri açık bir PaaS yaklaşımıdır. PaaS ve web barındırma arasında çok belirgin bir fark olmasa da Bulut Hizmetleri Web Siteleri'nden bazı önemli noktalarda ayrılır:

* Web Siteleri'nin aksine Bulut Hizmetleri, uygulamanızın sanal
  makinesine yönetim erişimi sağlar. Böylece uygulamanızın ihtiyacı
  olduğu kadar isteğe bağlı sayıda yazılım yükleyebilirsiniz, bu özellik
  Web Siteleri'nde yoktur.
* Bulut Hizmetleri hem web hem de çalışan rolleri sunduğundan iş mantığı
  için ayrı sanal makine gerektiren çok katmanlı uygulamalarda Web
  Siteleri'nden daha iyi bir tercihtir.
* Bulut Hizmetleri ayrı hazırlık ve üretim ortamları sağladığından
  uygulama güncelleştirmelerini Web Siteleri'ne göre daha sorunsuz
  gerçekleştirilir.
* Web Siteleri'nin aksine şirket içi bilgisayarları Bulut Hizmetleri
  uygulamalarına bağlamak için Azure Sanal Ağı ve Azure Connect gibi ağ
  teknolojilerini kullanabilirsiniz.
* Bulut Hizmetleri uygulamanın sanal makinesine doğrudan bağlanmak için
  Uzak Masaüstü'nü kullanmanıza izin verir, Web Siteleri'nde bu işlem
  mümkün değildir.

Bulut Hizmetleri PaaS nedeniyle bazı yönlerden Azure Sanal Makineleri'nden daha avantajlıdır. Sizin adınıza daha fazla yönetim görevi (işletim sistemi güncelleştirmelerini dağıtma vb.) gerçekleştirildiğinden işlem maliyetleriniz, Azure Sanal Makineleri'nin IaaS yaklaşımına göre daha düşük olur.

Üç Azure yürütme modelinin de iyi ve kötü yönleri vardır. En iyi seçimi yapmak için bunları anlamanız, amacınızı belirlemeniz ve en uygun olanı seçmeniz gerekir.

Bazen hiçbir yürütme modeli tek başına yeterli olmaz. Böyle durumlarda seçenekleri birleştirmek en uygun çözümdür. Bir uygulama oluştururken Bulut Hizmetleri web rollerinin sunduğu yönetim avantajından yararlanmak istediğinizi aynı zamanda uyumluluk veya performans unsurları nedeniyle standart SQL Server kullanmak istediğinizi düşünün. Bu durumda en iyi çözüm, [Şekil 7](#Fig7)'de gösterildiği gibi yürütme modellerini
birleştirmektir.

<a name="Fig7"></a>![07_CombineTechnologies](./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png)

**Şekil 7: Bir uygulamada birden fazla yürütme modeli kullanılabilir.**

Şekilde açıklandığı gibi Bulut Hizmetleri sanal makineleri, Sanal Makinler'in makinelerinden farklı bir bulut hizmetinde çalışır. Yine de bu iki model kolaylıkla iletişim kurabildiği için uygulamayı bu yöntemle oluşturmak en iyi çözümdür.

Bulut platformlarının birçok farklı senaryoyu desteklemesi gerektiğinden Azure, farklı yürütme modelleri sunar. Bu platformu verimli bir şekilde kullanmak isteyen geliştiricilerin (makaleyi buraya kadar okuduysanız büyük olasılıkla siz de bu grubun içindesiniz) söz konusu modelleri iyi anlaması gerekir.



[1]: http://vmdepot.msopentech.com/
[2]: http://msdn.microsoft.com/en-us/library/windowsazure/dn197896.aspx