<properties  linkid="manage-scenarios-choose-web-app-service" urlDisplayName="Web Options for Azure" pageTitle="Azure Web Sites, Cloud Services and Virtual Machines comparison" metaKeywords="Cloud Services, Virtual Machines, Web Sites" description="Learn when to use Azure Web Sites, Cloud Services, and Virtual Machines for hosting web applications. Review a feature comparison." metaCanonical="" services="web-sites,virtual-machines,cloud-services" documentationCenter="" title=" Cloud Services" authors="jroth" solutions="" manager="paulettm" editor="mollybos" />

# Azure Web Siteleri, Bulut Hizmetleri ve Sanal Makineleri'nin karşılaştırması

Azure, web uygulamalarınızı barındırmanız için [Azure Web Siteleri][1], [Bulut Hizmetleri][2] ve [Sanal Makineler][3] gibi farklı yöntemler sunar. Bu farklı seçeneklere göz attıktan sonra hangisinin ihtiyaçlarınızı karşılayacağından emin olamayabilirsiniz veya IaaS ve PaaS kavramları yeterli bilginiz olmayabilir. Bu makale, sunulan seçenekleri anlamanıza ve web senaryonuz için doğru seçimi yapmanıza yardımcı olur. Üç seçenek de Azure'da yüksek düzeyde ölçeklendirilebilir web uygulamaları çalıştırmanızı sağlamasına rağmen karar vermenize yardımcı olacak farklara sahiptirler.

Birçok durumda Azure Web Siteleri en iyi seçenektir. Dağıtım ve yönetim için basit ve esnek seçenekler sunar, yüksek hacimli web siteleri barındırabilir. Web Uygulama Galerisi'nden seçebileceğiniz WordPress gibi popüler bir yazılımla hızlı bir şekilde web sitesi oluşturabilir veya varolan bir web sitesini Azure Web Siteleri'ne taşıyabilirsiniz. Şu anda önizlemede olan [Azure WebJobs SDK][4]'sini kullanarak arka plan iş işlemleri ekleyebilirsiniz.

Aynı zamanda Azure Bulut Hizmetleri veya Azure Sanal Makineleri'nde web uygulamaları barındırabilirsiniz. Web katmanınız, ek denetim seviyesi ve özelleştirme gerektiriyorsa iyi birer seçenektir ancak yüksek düzeyde denetim nedeniyle uygulama oluşturma, yönetim ve dağıtım işlemleri de karmaşıklaşır. Aşağıdaki diyagramda bu üç seçenek arasındaki farklar gösterilmektedir.

![ChoicesDiagram](./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_2.png)

Web Siteleri'nin kurulumu, yönetimi ve izlenmesi daha kolaydır ancak sahip olduğunuz yapılandırma seçenekleri daha azdır. Web ön ucunu Bulut Hizmetleri'nde veya Sanal Makineler'de oluşturmanız gerekmiyorsa Azure Web Siteleri'ni kullanın. Bu belgenin geri kalanında doğru bir karar vermenize yardımcı olacak bilgilere yer verilmektedir. Bunlar:

* [Senaryolar](#scenarios)
* [Hizmet Özetleri](#services)
* [Özelliklerin Karşılaştırması](#features)

## <a name="scenarios"></a>Senaryolar

### Küçük işletme sahibiyim, ancak gelecekte işletmemi büyütmeyi planlıyorum. Web sitemi barındırmak için düşük maliyetli ve esnek bir yönteme ihtiyacım var.

Azure Web Siteleri (WAWS) bu senaryo için harika bir çözümdür; ücretsiz olarak kullanmaya başlayabilir ve ardından ihtiyaç duyduğunuzda daha fazla özellik ekleyebilirsiniz. Örneğin, ücretsiz web sitelerinin tümü Azure tarafından sağlanan bir etki alanıyla gelir (*your_company*.azurewebsites.net). Kendi etki alanınızı kullanmak isterseniz bu özelliği aylık 9,80 ABD doları gibi düşük bir ücret karşılığında ekleyebilirsiniz (Ocak 2014 tarihinden itibaren geçerlidir). Kullanıcı talebi arttıkça sitenizin gelişmesini sağlayacak çok sayıda başka hizmet ve ölçeklendirme seçeneği bulunur. **Azure Web Siteleri** ile şunları yapabilirsiniz:

* Ücretsiz olarak kullanmaya başladıktan sonra ölçeği ihtiyaç duyduğunuz
  kadar büyütebilirsiniz.
* WordPress gibi popüler web uygulamalarını kolay bir şekilde kurmak
  için Uygulama Galerisini kullanabilirsiniz.
* Uygulamanıza gerektiğinde ek Azure hizmetleri ve özellikleri
  ekleyebilirsiniz.
* *your_company*.azurewebsites.net etki alanı için sağlanmış sertifikayı
  kullanarak HTTPS ile web sitenizin güvenliğini sağlayabilirsiniz.

### Web tasarımcısı veya grafik tasarımcısıyım, müşterilerim için web sitesi tasarlamak ve oluşturmak istiyorum

Azure Web Siteleri, web geliştiricilerinin karmaşık web uygulamaları oluşturmak için ihtiyaç duydukları araçları ve özellikleri sağlar. Web Siteleri, Visual Studio ve SQL veritabanı gibi araçlarla yüksek düzeyde bütünleştirme sağlar. **Web Siteleri** ile geliştiriciler şunları yapabilir:

* [Otomatik görevler][5] için komut satırı araçlarını kullanabilirler.
* [.Net][6], [PHP][7], [Node.js][8] ve [Python][9].gibi popüler
  programlama dilleriyle çalışabilirler.
* Yüksek kapasitelere çıkmak için üç farklı ölçeklendirme seçeneğinden
  yararlanabilirler.
* [SQL Veritabanı][10], [Hizmet Veri Yolu][11] ve [Depolama][12] gibi
  Azure hizmetleriyle veya iş ortaklarının [Azure Mağazası][13]
  aracılığıyla sunduğu MySQL, MongoDB vb. veritabanlarıyla tümleştirme
  sağlayabilirler.
* Visual Studio, Git, WebMatrix, WebDeploy, TFS ve FTP gibi araçlarla
  tümleştirme sağlayabilirler.

### Web ön ucuna sahip çok katmanlı uygulamamı Buluta taşıyorum

Web sitesinin verilerini depolamak ve almak için veritabanı sunucusuyla iletişim kuran web sunucusu gibi çok katmanlı bir uygulama çalıştırıyorsanız Azure'da birkaç seçenek bulabilirsiniz. Bu mimari seçeneklere Web Siteleri, Bulut Hizmetleri ve Sanal Makineler dahildir. **Web Siteleri**, çözümünüzün web katmanına yönelik iyi bir seçimdir, iki katmanlı bir mimari oluşturmak için Azure SQL Veritabanı ile kullanılabilir. Web Siteleri ayrıca Azure WebJobs SDK önizlemesini kullanarak arka planda veya uzun süreli çalışan süreçleri çalıştırmanıza izin verir. Daha karmaşık mimariye veya daha esnek ölçeklendirme seçeneklerine ihtiyaç duyuyorsanız Bulut Hizmetleri veya Sanal Makineler bunlar için daha uygundur.

**Bulut Hizmetleri** ile şunları yapabilirsiniz:

* Web, orta katman ve arka uç hizmetlerini ölçeklendirilebilen web ve
  çalışan rollerinde barındırabilirsiniz.
* Çalışan rollerinde yalnızca orta katman ve arka uç hizmetlerini
  barındırabilir, ön ucu Azure Web Siteleri'nde bırakabilirsiniz.
* Ön uç ve arka uç hizmetlerini birbirlerinden bağımsız olarak
  ölçeklendirebilirsiniz.

**Sanal Makineler** ile şunları yapabilirsiniz:

* Yüksek düzeyde özelleştirilmiş ortamları sanal makine görüntüsü olarak
  kolay bir şekilde taşıyabilirsiniz.
* Web Siteleri veya Bulut Hizmetleri üzerinde yapılandırılamayan
  yazılımları veya hizmetleri çalıştırabilirsiniz.

### Uygulamam yüksek düzeyde özelleştirilmiş Windows veya Linux ortamına bağlı

Uygulamanız karmaşık yazılım ve işletim sistemi yükleme veya yapılandırma işlemleri gerektiriyorsa Sanal Makineler büyük olasılıkla en iyi çözümdür. **Sanal Makineler** ile şunları yapabilirsiniz:

* Sanal Makine galerisini kullanarak Windows veya Linux gibi bir işletim
  sistemi başlatabilir ve ardından ortamı uygulamanızın gereksinimlerine
  uygun şekilde özelleştirebilirsiniz.
* Azure'daki sanal makinede çalıştırılmak üzere varolan bir şirket içi
  sunucunun özel görüntüsünü oluşturabilir ve karşıya yükleyebilirsiniz.

### Sitem açık kaynaklı yazılım kullanıyor ve siteyi Azure'da barındırmak istiyorum

Üç seçenekte de açık kaynaklı dilleri ve çerçeveleri barındırabilirsiniz. **Bulut Hizmetleri**'nde, Windows'da çalışan gerekli açık kaynaklı yazılımları yüklemek ve yapılandırmak için başlangıç görevlerini kullanmanız gerekir. **Virtual Makineler**'de yazılımı Windows veya Linux tabanlı makine görüntüsüne yüklersiniz ve burada yapılandırırsınız. Açık kaynaklı çerçeveniz Web Siteleri tarafından destekleniyorsa, Web Siteleri uygulamanız için gerekli diller ve çerçevelerle otomatik olarak yapılandırıldığından bu tür uygulamalar daha kolay bir şekilde barındırılır. **Web Siteleri** ile şunları yapabilirsiniz:

* [.Net][6], [PHP][7], [Node.js][8] ve [Python][9].gibi popüler açık
  kaynaklı dilleri kullanabilirsiniz.
* WordPress, Drupal, Umbraco, DNN ve daha birçok üçüncü taraf web
  uygulamasını kurabilirsiniz.
* Varolan bir uygulamayı taşıyabilir veya Uygulama Galerisi ile yeni bir
  uygulama oluşturabilirsiniz.

### Şirket ağına bağlanması gereken bir iş kolu uygulamasına sahibim

İş kolu uygulaması oluşturmak istiyorsanız, web sitenizin şirket ağında
bulunan hizmetlere veya verilere doğrudan erişmesi gerekebilir. Bu,
**Web Siteleri**, **Bulut Hizmetleri** ve **Sanal Makinler** ile
yapılabilir. Seçtiğiniz yaklaşıma göre şu farklar görülebilir:

* Web Siteleri şirket içi kaynaklara Hizmet Veri Yolu Geçişi'ni
  kullanarak güvenli bir şekilde bağlanabilir. Böylece her şeyi Bulut'a
  taşımadan veya sanal ağ kurmadan şirket ağındaki hizmetler web hizmeti
  yerine görevleri gerçekleştirilebilir.
* Bulut Hizmetleri ve Sanal Makineler, Sanal Ağ'dan yararlanabilir.
  Sanal Ağ, Azure'da çalışan makinelerin şirket içi ağa bağlanmasını
  sağlar. Böylece Azure, şirket veri merkezinizin bir uzantısı haline
  gelir.

### Mobil istemcilere yönelik REST API veya web hizmeti barındırmak istiyorum

HTTP tabanlı web hizmetleri, mobil müşteriler dahil olmak üzere birçok farklı müşteriye yönelik destek sunmanızı sağlar. REST hizmetlerini daha kolay oluşturmak ve kullanmak için ASP.NET Web API gibi çerçeveler Visual Studio ile tümleştirilir. Bu hizmetler bir web ön ucunda açık bir şekilde yer aldığından bu senaryoyu desteklemek için Azure üzerinde herhangi bir web barındırma yöntemi kullanılabilir. Bununla birlikte
**Web Siteleri**, REST API'lerini barındırmak için harika bir seçimdir. Web Siteleri ile şunları yapabilirsiniz:

* HTTP web hizmetini Azure'un küresel olarak dağıtılmış veri
  merkezlerinde barındırılacak bir Web Sitesini kolayca
  oluşturabilirsiniz.
* Visual Studio içindeki ASP.NET Web API'dan yararlanarak varolan
  hizmetleri taşıyabilir veya yeni hizmetler oluşturabilirsiniz.
* Tek bir örnekte kullanılabilirlik SLA'sı elde edebilir veya birden
  fazla adanmış makineye ölçeklendirebilirsiniz.
* Mobil istemciler dahil olmak üzere tüm HTTP istemcileri için REST
  API'lar sağlamak üzere yayımlanmış siteyi kullanabilirsiniz.

## <a name="services"></a>Hizmet Özetleri

[Azure Web Siteleri][1], Azure'da kolay bir şekilde yüksek düzeyde ölçeklendirilebilir web siteleri oluşturmanızı sağlar. .NET, PHP, Node.js ve Python gibi popüler programlama dilleri ile bir web sitesi kurmak için Azure Portalı'nı veya komut satırı araçlarını kullanabilirsiniz. Desteklenen çerçeveler zaten dağıtılmıştır ve başka bir yükleme adımı gerektirmez. Azure Web Siteleri galerisi, Drupal ve WordPress gibi üçüncü taraf uygulamaların yanı sıra Django ve CakePHP gibi geliştirme framework’lerini de içerir. Site oluşturduktan sonra varolan bir web sitesini taşıyabilir veya tamamen yeni bir web sitesi oluşturabilirisiniz. Web Siteleri, fiziksel donanım yönetimi gerekliliğini ortadan kaldırır ve birçok ölçeklendirme seçeneği sunar. Birden çok kullanıcı tarafından paylaşılan modelden, gelen trafiği adanmış makinelerin yönettiği standart moda geçebilirsiniz. Web Siteleri aynı zamanda SQL Veritabanı, Hizmet Veri Yolu ve Depolama gibi diğer Azure hizmetleriyle tümleştirme imkanı tanır. [Azure WebJobs SDK][4] önizlemesini kullanarak arka plan işlemi ekleyebilirsiniz. Kısaca Azure Web Siteleri, çeşitli dilleri, açık kaynaklı uygulamaları ve dağıtım metodolojilerini (FTP, Git, Web Deploy veya TFS) destekleyerek uygulama geliştirmeye odaklanmayı kolaylaştırır. Bulut Hizmetleri veya Sanal Makineleri gerektiren özel nedenler yoksa Azure Web Siteleri büyük olasılıkla sizin için en iyi seçimdir.

[Bulut Hizmetleri][2] zengin Hizmet olarak Platform (PaaS) ortamında yüksek düzeyde kullanılabilirliğe sahip, ölçeklendirilebilen web uygulamaları oluşturmanızı sağlar. Web Siteleri'nin aksine bulut hizmeti Azure'da dağıtılmadan önce Visual Studio gibi bir dağıtım ortamında oluşturulur. PHP vb. çerçeveler, çerçeveyi başlangıç rolüne yükleyen özel dağıtım adımları veya görevleri gerektirirler. Bulut Hizmetleri'nin en önemli avantajı daha karmaşık çok katmanlı mimarileri destekleme özelliğine sahip olmasıdır. Bulut hizmeti bir ön web rolü ve bir veya daha fazla çalışan rollerinden oluşabilir. Her katman birbirinden bağımsız olarak ölçeklendirilebilir. Web uygulamanızın altyapısına yönelik yüksek düzeyde denetim sağlar. Örneğin, rol örneklerini çalıştıran makinelerde uzaktan masaüstünü kullanabilirsiniz. Ayrıca yönetici denetimi gerektiren görevler dahil olmak üzere rol başlangıcında çalıştırılan daha gelişmiş IIS ve makine yapılandırma değişikleri komut dosyası oluşturabilirsiniz.

[Sanal Makineler][3], Azure'daki sanal makinelerde web uygulamaları çalıştırmanızı sağlar. Bu özellik, Hizmet olarak Altyapı (IaaS) olarak da bilinir. Portal üzerinde Windows Server veya Linux makineleri oluşturabilir veya varolan bir sanal makine görüntüsünü karşıya yükleyebilirsiniz. Sanal Makineler; işletim sistemi, yapılandırma, yüklü yazılımlar ve hizmetler üzerinde en yüksek seviyede denetim imkanı tanır. Makineler bütün olarak taşınabildiğinden karmaşık şirket içi web uygulamalarını kolay bir şekilde buluta taşımak için en uygun seçenektir. Ayrıca Sanal Ağlar ile bu sanal makineleri şirket içi ağlara bağlayabilirsiniz. Bulut Hizmetlerinde olduğu gibi bu makinelere uzaktan erişebilir ve yönetici düzeyinde yapılandırma değişiklikleri gerçekleştirebilirsiniz. Bununla birlikte Web Siteleri ve Bulut Hizmetleri'nin aksine sanal makinelerinizin görüntülerini ve uygulama mimarisini tamamen altyapı düzeyinde yönetmeniz gerekir. Örneğin, işletim sisteminizin düzeltme eklerini kendiniz uygulamanız gerekir.

## <a name="features"></a>Özelliklerin Karşılaştırması

Aşağıdaki tabloda, en doğru kararı vermenize yardımcı olmak için Web Siteleri, Bulut Hizmetleri ve Sanal Makineler karşılaştırılmıştır. Notlarda yıldız işaretine sahip kutularla ilgili daha fazla açıklama yer almaktadır.

<table  cellspacing="0" border="1">
<tr>
   <th  align="left" valign="middle">Özellik</th>

   <th  align="left" valign="middle">Web Siteleri</th>

   <th  align="left" valign="middle">Bulut Hizmetleri (web rolleri)</th>

   <th  align="left" valign="middle">Sanal Makineler</th>

</tr>

<tr>
   <td  valign="middle"><p>Hizmet Veri Yolu, Depolama, SQL Veritabanı gibi hizmetlere erişim</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Çok katmanlı mimaride web veya web hizmeti katmanını barındırma</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Çok katmanlı mimaride orta katmanı barındırma</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Tümleştirilmiş hizmet olarak MySQL desteği</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X <sup>1</sup>
</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>ASP.NET, klasik ASP, Node.js, PHP, Python desteği</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Yeniden dağıtım olmaksızın birden çok örneğe ölçeklendirme</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X <sup>2</sup>
</td>

</tr>

<tr>
   <td  valign="middle"><p>SSL desteği</p>
</td>

   <td  valign="middle">X <sup>3</sup>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Visual Studio tümleştirmesi</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Uzaktan Hata Ayıklama</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>TFS ile dağıtım kodu</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>GIT, FTP ile dağıtım kodu</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Web Deploy ile dağıtım kodu</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle"><sup>4</sup>
</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>WebMatrix desteği</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Anında dağıtım</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Örneklerde içerik ve yapılandırma paylaşımı</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Yeniden dağıtım olmaksızın daha büyük makinelere ölçeklendirme</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Birden fazla dağıtım ortamı (üretim ve hazırlık)</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Azure Sanal Ağı ile ağ yalıtımı</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Azure Trafik Yöneticisi desteği</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Sunuculara uzaktan masaüstü ile erişim</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Başlangıç görevleri tanıtma/yürüme özelliği</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Otomatik işletim sistemi güncelleştirme yönetimi</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Tümleştirilmiş Uç Nokta İzleme</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Sorunsuz platform değiştirme (32 bit / 64 bit)</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

</table>

<sup>1</sup> Web veya çalışan rolleri hizmet olarak MySQL ile ClearDB sunumları aracılığıyla tümleştirilir ancak Yönetim Portalı iş akışı buna dahil değildir.

<sup>2</sup> Sanal Makineler'in birden çok örneğe ölçeklendirilebilmesine rağmen bu makinelerde çalıştırılan hizmetler bu ölçeklendirmeyi işlemek için yazılmalıdır. İstekleri makinelere yönlendirmek için bir ek yük dengeleyicisi yapılandırılmalıdır. Son olarak, bakım ve donanım hataları nedeniyle eş zamanlı yeniden başlatmaları engellemek için aynı roldeki tüm makineler için bir Affinity Group (Benzeşim Grubu) oluşturulmalıdır.

<sup>3</sup> Web Siteleri'ne yönelik özel etki alanı adları için SSL yalnızca standart modda desteklenir. SSL'i Web Siteleri ile kullanma hakkında daha fazla bilgi için bkz. [Azure Web Sitesi için SSL sertifikası yapılandırma][14].

<sup>4</sup> Web Deploy, tek örnekli rollere dağıtımda bulut hizmetleri için desteklenir. Bununla birlikte üretim rolleri, Azure SLA'sını karşılamak için birden fazla örnek gerektirir. Bu nedenle Web Deploy, üretimdeki bulut hizmetleri için uygun bir dağıtım mekanizması değildir.



[1]: http://go.microsoft.com/fwlink/?LinkId=306051
[2]: http://go.microsoft.com/fwlink/?LinkId=306052
[3]: http://go.microsoft.com/fwlink/?LinkID=306053
[4]: http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs
[5]: http://www.windowsazure.com/tr-tr/documentation/scripts/?services=web-sites
[6]: http://www.windowsazure.com/en-us/develop/net/
[7]: http://www.windowsazure.com/en-us/develop/php/
[8]: http://www.windowsazure.com/en-us/develop/nodejs/
[9]: http://www.windowsazure.com/en-us/develop/python/
[10]: http://www.windowsazure.com/tr-tr/documentation/services/sql-database/
[11]: http://www.windowsazure.com/tr-tr/documentation/services/service-bus/
[12]: http://www.windowsazure.com/tr-tr/documentation/services/storage/
[13]: http://www.windowsazure.com/en-us/gallery/store/
[14]: http://www.windowsazure.com/en-us/develop/net/common-tasks/enable-ssl-web-site/