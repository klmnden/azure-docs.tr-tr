---
title: "Azure App Service, sanal makineler, Service Fabric ve Cloud Services karşılaştırması | Microsoft Docs"
description: "Azure App Service, sanal makineleri, Service Fabric ve web uygulamalarını barındırmak için bulut hizmetleri arasında seçim yapma hakkında bilgi edinin."
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: edd5099d2804fdb5867b4be5b11a361004db1665
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure App Service, sanal makineler, Service Fabric ve Cloud Services karşılaştırması
## <a name="overview"></a>Genel Bakış
Azure web siteleri barındırmak için çeşitli yollar sunar: [Azure App Service][Azure App Service], [sanal makineleri][Virtual Machines], [Service Fabric][Service Fabric], ve [bulut Hizmetleri][Cloud Services]. Bu makalede seçenekleri anlamanıza ve web uygulamanız için doğru seçim yapmanıza yardımcı olur.

Azure uygulama hizmeti, çoğu web uygulamaları için en iyi seçimdir. Dağıtım ve yönetim platformuyla doğrudan tümleşik, siteleri hızlı bir şekilde yüksek trafik yükleri işlemek için ölçeklendirilebilir ve yerleşik Yük Dengeleme ve trafik Yöneticisi yüksek kullanılabilirlik sağlar. Azure App Service ile kolayca varolan siteler taşıyabilirsiniz bir [çevrimiçi geçiş aracı](https://www.migratetoazure.net/), Web uygulaması Galeriden bir açık kaynak uygulama kullanın ya da framework ve tercih ettiğiniz Araçları'nı kullanarak yeni bir site oluşturun. [WebJobs] [ WebJobs] özelliği, App Service web uygulamanızın arka plan iş işleme eklemek kolaylaştırır.

Yeni bir uygulama oluşturma ya da mevcut bir uygulamayı mikro hizmet mimarisi kullanmak üzere yeniden yazmadan Service Fabric iyi bir seçimdir. Paylaşılan bir havuzunda makineleri çalıştırmak, uygulamaları küçük başlayın ve büyük ölçekli yüzlerce veya binlerce gerektiğinde makinelerin büyüyebileceği. Durum bilgisi olan hizmetler tutarlı ve güvenilir bir şekilde uygulama durumunu depolamak kolaylaştırır ve Service Fabric otomatik olarak hizmet bölümlendirme, ölçeklendirme ve kullanılabilirlik tarafından yönetilir.  Service Fabric Webapı açık Web arabirimi ile .NET (OWIN) ve ASP.NET Core için de destekler.  Uygulama hizmeti ile karşılaştırıldığında, Service Fabric ayrıca daha fazla denetime ya da doğrudan erişim, altyapının sağlar. Sunucu başlangıç görevleri yapılandırmak veya sunucularınızı uzaktan kullanabilirsiniz. Bulut Hizmetleri, kullanım kolaylığı ve denetim ölçüde Service Fabric benzer, ancak bu artık eski bir hizmet olduğundan ve Service Fabric yeni geliştirme projeleri için önerilir.

Uygulama hizmeti veya Service Fabric çalıştırmak için önemli değişiklikler gerektiren var olan bir uygulamanız varsa, buluta geçiş basitleştirmek için sanal makineleri seçebilir. Ancak, doğru şekilde güvenli hale getirme, yapılandırma ve bakımını yapmak VM'ler gerektirir çok daha fazla zaman ve Azure App Service ve Service Fabric karşılaştırıldığında BT uzmanlık. Azure sanal makineleri düşünüyorsanız, düzeltme eki, güncelleştirme ve VM ortamınızı yönetmek için gerekli devam eden bakım çaba dikkate emin olun. Azure sanal makinelerin App Service ve Service Fabric Platform olarak-hizmet (Paas) durumdayken-olarak-hizmet altyapı (Iaas) şeklindedir. 

## <a name="features"></a>Özellik karşılaştırması
Aşağıdaki tabloda, en iyi seçim yapmanıza yardımcı olmak için özelliklerini uygulama hizmeti, bulut Hizmetleri, sanal makineler ve Service Fabric karşılaştırır. Her seçenek için SLA hakkında güncel bilgiler için bkz: [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

| Özellik | Uygulama Hizmeti (web uygulamaları) | Bulut Hizmetleri (web rolleri) | Virtual Machines | Service Fabric | Notlar |
| --- | --- | --- | --- | --- | --- |
| Neredeyse anında dağıtım |X | | |X |Bir uygulama veya bir uygulama güncelleştirmesi için bir bulut hizmeti dağıtma veya bir VM oluşturma en az birkaç dakika sürer; bir uygulama bir web uygulamasına dağıtma birkaç saniye sürer. |
| Daha büyük makinelere dağıtın olmadan ölçeği |X | | |X | |
| Web sunucusu örneğine içerik ve yeniden dağıtın veya kendi ölçeklendirmenize göre yeniden yapılandırmak zorunda kalmazsınız yapılandırma paylaşır. |X | | |X | |
| Birden çok dağıtım ortamı (üretim ve hazırlama) |X |X | |X |Service Fabric, uygulamalarınız için birden çok ortamlarınız veya, uygulama yan yana farklı sürümlerini dağıtmak için sağlar. |
| Otomatik işletim sistemi güncelleştirme yönetimi |X |X | | |Kısmen aracılığıyla düzeltme eki Orchestration uygulama (POA) ve tam olarak gelecekte. |
| Sorunsuz platform değiştirme (32 bit ve 64 bit arasında kolayca geçiş) |X |X | | | |
| GIT, FTP ile kod dağıtma |X | |X | | |
| Web dağıtımı ile kod dağıtma |X | |X | |Bulut Hizmetleri tek rol örneklerine güncelleştirmeleri dağıtmak için Web dağıtımı kullanımını destekler. Ancak, bir rolün ilk dağıtımınızı kullanamaz ve bir güncelleştirme için Web dağıtımı kullanıyorsanız, her bir rol örneği için ayrı olarak dağıtmanız gerekir. Birden çok örneği üretim ortamları için bulut hizmeti SLA'sı için nitelemek için gereklidir. |
| WebMatrix desteği |X | |X | | |
| Hizmet veri yolu, depolama, SQL veritabanı gibi hizmetlere erişimi |X |X |X |X | |
| Ana bilgisayar web veya web hizmetleri katmanı çok katmanlı mimarisi |X |X |X |X | |
| Çok katmanlı mimarisinin konak orta katman |X |X |X |X |App Service web apps kolayca bir REST API'si Orta katmanı barındırmak ve [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) özelliği, arka plan işleme işleri barındırabilir. Web işleri katmanı için bağımsız ölçeklenebilirlik elde etmek için ayrılmış bir Web sitesinde çalıştırabilirsiniz. |
| Tümleşik Hizmet olarak MySQL desteği |X |X |X | |Bulut Hizmetleri hizmet olarak MySQL Cleardb'nin teklifleri aracılığıyla ancak Azure Portal iş akışının parçası olarak değil tümleştirebilirsiniz. |
| ASP.NET, klasik ASP, Node.js, PHP, Python desteği |X |X |X |X |Service Fabric kullanarak bir web ön uç oluşturmayı destekleyen [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) veya herhangi bir türde uygulamayı (Node.js, Java, vb.) olarak dağıtabileceğiniz bir [Konuk yürütülebilir](../service-fabric/service-fabric-deploy-existing-app.md). |
| Birden çok örneği dağıtın olmadan için genişletme |X |X |X |X |Sanal makineler için birden çok örneği ölçeğini, ancak bunlar üzerinde çalışan hizmetleri bu genişleme işlemek için yazılmış olmalıdır. Makine genelinde istekleri yönlendirmek ve Bakım veya donanım hataları nedeniyle tüm örnekleri aynı anda yeniden engellemek için bir benzeşim grubu oluşturmak için bir yük dengeleyici yapılandırmanız gerekir. |
| SSL desteği |X |X |X |X |App Service web uygulamaları için özel etki alanı adları için SSL yalnızca temel ve Standart modu için desteklenir. Web uygulamaları ile SSL kullanma hakkında daha fazla bilgi için bkz: [bir Azure Web sitesi için bir SSL sertifikası yapılandırma](app-service-web-tutorial-custom-ssl.md). |
| Visual Studio tümleştirmesi |X |X |X |X | |
| Uzaktan hata ayıklama |X |X |X | | |
| Kod TFS ile Dağıt |X |X |X |X | |
| Ağ yalıtımı ile [Azure sanal ağ](/azure/virtual-network/) |X |X |X |X |Ayrıca bkz. [Azure Web siteleri sanal ağ tümleştirme](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Desteği [Azure trafik Yöneticisi](/azure/traffic-manager/) |X |X |X |X | |
| Tümleşik uç nokta izleme |X |X |X | | |
| Uzak Masaüstü erişimi sunucuları | |X |X |X | |
| Tüm özel MSI yükleme | |X |X |X |Service Fabric sağlar, herhangi bir yürütülebilir dosya olarak barındırmak bir [Konuk yürütülebilir](../service-fabric/service-fabric-deploy-existing-app.md) veya Vm'lerinde herhangi bir uygulama yükleyebilirsiniz. |
| Başlangıç görevleri tanımlamak/yürütün yeteneği | |X |X |X | |
| ETW olayları dinleme | |X |X |X | |

## <a name="scenarios"></a>Senaryolar ve öneriler
Burada, bazı ortak uygulama senaryoları için hangi Azure web barındırma seçeneği her biri için en uygun olabilecek öneriler bulunmaktadır.

* [Bir web ön uç ile şirket içi varlıklarını tümleşik iş uygulamaları çalıştırmak için arka plan işleme ve veritabanı arka ucu ile ihtiyacım.](#onprem)
* [Teklifler genel ulaşmak ve buna iyi ölçeklenir my Kurumsal Web sitesi barındırmak için güvenilir bir yol gerekir.](#corp)
* [Windows Server 2003'te çalışan bir IIS6 uygulama var.](#iis6)
* [Küçük işletme sahibi ben ve Sitem barındırmak için uygun maliyetli bir yol gerekiyor ancak Gelecekteki büyümeyi unutmayın.](#smallbusiness)
* [Bir web veya grafik Tasarımcı ben ve tasarlayın ve my müşteriler için web siteleri oluşturmak istiyorum.](#designer)
* [Bir web ön uç ile çok katmanlı Uygulamam buluta geçirme.](#multitier)
* [Buluta taşımak Linux ortamları ve istediğiniz veya üst düzeyde özelleştirilmiş Windows Uygulamam bağlıdır.](#custom)
* [Açık kaynak yazılımının Sitem kullanır ve Azure üzerinde barındırmak istediğiniz.](#oss)
* [Şirket ağına bağlanmak için gereken bir iş kolu satır uygulama var.](#lob)
* [Bir REST API veya mobil istemciler için web hizmetini barındırmak istediğiniz.](#mobile)

### <a id="onprem"></a>Bir web ön uç ile şirket içi varlıklarını tümleşik iş uygulamaları çalıştırmak için arka plan işleme ve veritabanı arka ucu ile ihtiyacım.
Azure uygulama hizmeti, karmaşık iş uygulamaları için mükemmel bir çözümdür. Bu, olanak tanır, şirket kaynaklarına bağlanmak ölçeği otomatik olarak bir yük dengeli platformda ve Active Directory ile güvenliği sağlanan uygulamalar geliştirme. Bu uygulamaları bir dünya çapındaki portal ve API kolay hale getirir ve nasıl müşteriler bunları app Insight araçlarıyla kullandığını içine kavramak olanak sağlar. [Webjobs] [ Webjobs] özelliği arka plan işlemlerini çalıştırmak olanak tanır ve görevleri karma bağlantı ve sanal ağ özellikleri bağlanmayı kolaylaştırır karşın, web katmanının bir parçası olarak geri şirket içi kaynaklara. Azure uygulama hizmeti web uygulamaları için üç 9 ait SLA sağlar ve sağlar:

* Uygulamalarınızın güvenilir bir şekilde kendini onarma, otomatik düzeltme eki uygulama bulut platformu üzerinde çalıştırın.
* Veri merkezlerinin küresel bir ağda otomatik olarak ölçeklendirin.
* Yedekleme ve olağanüstü durum kurtarma için geri yükleme.
* ISO, SOC2 ve PCI uyumlu olmalıdır.
* Active Directory ile tümleştirin

### <a id="corp"></a>Teklifler genel ulaşmak ve buna iyi ölçeklenir my Kurumsal Web sitesi barındırmak için güvenilir bir yol gerekir.
Azure uygulama hizmeti, şirket Web siteleri barındırmak için harika bir çözümdür. Web uygulamalarını kolayca ölçek için ve veri merkezlerinin küresel bir ağda talebi karşılamak için kolayca sağlar. Yerel reach, hata toleransı ve akıllı trafik yönetimi sunar. Dünya çapındaki yönetim araçları sağlayan tüm bir platformda, site durumu ve site trafiği hızla ve kolayca kavramanıza olanak sağlar. Azure uygulama hizmeti web uygulamaları için üç 9 ait SLA sağlar ve sağlar:

* Sitelerinizi güvenilir bir şekilde kendini onarma, otomatik düzeltme eki uygulama bulut platformu üzerinde çalıştırın.
* Veri merkezlerinin küresel bir ağda otomatik olarak ölçeklendirin.
* Yedekleme ve olağanüstü durum kurtarma için geri yükleme.
* Günlüklerini yönetmek ve tümleşik araçlarıyla trafiği.
* ISO, SOC2 ve PCI uyumlu olmalıdır.
* Active Directory ile tümleştirin

### <a id="iis6"></a>Windows Server 2003'te çalışan bir IIS6 uygulama var.
Azure uygulama hizmeti geçirme eski IIS6 uygulamalarla ilişkili altyapı maliyetleri önlemek kolaylaştırır. Microsoft oluşturdu [kullanımı kolay geçiş araçları ve ayrıntılı geçiş yönergeleri](https://www.movemetowebsites.net/) uyumluluğunu denetlemek ve yapılması gereken tüm değişiklikleri belirlemek etkinleştirin. Visual Studio, TFS ve ortak CMS araçları ile tümleştirme, IIS6 uygulama doğrudan buluta dağıtmak kolaylaştırır. Uygulama dağıtıldıktan sonra Azure Portal maliyetlerini yönetmek ve karşılamak kadar gerektiğinde talep ölçeklendirmenizi sağlayan güçlü bir yönetim araçlarını sağlar. Geçiş Aracı ile şunları yapabilirsiniz:

* Hızlı ve kolay bir şekilde buluta eski Windows Server 2003 web uygulamanızı geçirin.
* Ekli SQL veritabanı bir karma uygulaması oluşturmak için şirket içi bırakmayı tercih et.
* Otomatik olarak SQL veritabanınız eski uygulamanızı birlikte taşınır.

### <a id="smallbusiness"></a>Küçük işletme sahibi ben ve Sitem barındırmak için uygun maliyetli bir yol gerekiyor ancak Gelecekteki büyümeyi unutmayın.
Ücretsiz kullanmaya başlamak ve bunları gerektiğinde daha fazla özellikleri eklemek için azure uygulama hizmeti bu senaryo için harika bir çözümdür. Her ücretsiz bir web uygulamasını Azure tarafından sağlanan bir etki alanı ile birlikte gelir (*your_company*. azurewebsites.net), ve platform kolaylaştıran başlamak bir uygulama Galerisi yanı sıra tümleşik dağıtım ve yönetim araçları içerir. Diğer birçok Hizmetleri ve artan kullanıcı taleple gelişmesi izin ölçeklendirme seçenekleri vardır. Azure App Service ile şunları yapabilirsiniz:

* Ücretsiz katmanı ile başlayın ve gerektikçe sonra ölçeği.
* Uygulama Galerisi WordPress gibi popüler web uygulamalarını hızlı bir şekilde ayarlamak için kullanın.
* Ek Azure Hizmetleri ve özellikleri uygulamanıza gerektiği gibi ekleyin.
* Web uygulamanızı HTTPS ile güvenli hale getirin.

### <a id="designer"></a>Bir web veya grafik Tasarımcı ben ve tasarlayın ve my müşteriler için Web siteleri oluşturmak istiyorum
Azure uygulama hizmeti çeşitli çerçeveler ve araçları ile kolayca tümleşir web geliştiricileri ve tasarımcıları, Git ve FTP için dağıtım desteği içerir ve araçları ve Visual Studio ve SQL veritabanı gibi hizmetleri ile sıkı tümleştirme sağlar. App Service ile yapabilecekleriniz:

* İçin komut satırı araçlarını kullanmayı [otomatik görevleri][scripting].
* Popüler dillerde gibi çalışmak [.Net][dotnet], [PHP][PHP], [Node.js][nodejs], ve [Python][Python].
* Çok yüksek kapasite ölçekleme için üç farklı ölçeklendirme düzeylerini seçin.
* Gibi diğer Azure hizmetleriyle tümleştirme [SQL veritabanı][sqldatabase], [Service Bus] [ servicebus] ve [depolama][Storage], veya ortak tekliflerinin dışında [Azure depolama][azurestore], MySQL ve MongoDB gibi.
* Visual Studio, Git, WebMatrix, WebDeploy, TFS ve FTP gibi araçları ile tümleştirin.

### <a id="multitier"></a>I my çok katmanlı uygulama bir web ön uç ile buluta geçiş
Bir veritabanına bağlanan bir web sunucusu gibi çok katmanlı bir uygulama çalıştırıyorsanız, Azure App Service, Azure SQL veritabanı ile sıkı tümleştirme sunan iyi bir seçenektir. Ve arka uç işlemlerini çalıştırmak için WebJobs özelliğini kullanabilirsiniz.

Service Fabric birini veya daha fazla ihtiyacınız varsa, bu katmanların yeteneği uzak sunucunuz içine gibi sunucu ortamı üzerinden denetlemesine veya sunucu başlangıç görevleri yapılandırın.

Kendi makine görüntüsünü kullanabilir veya sunucu yazılımı veya Service Fabric yapılandıramazsınız hizmetleri çalıştırmak istiyorsanız, bir veya daha fazla, katmanları için sanal makineleri seçin.

### <a id="custom"></a>Buluta taşımak Linux ortamları ve istediğiniz veya üst düzeyde özelleştirilmiş Windows Uygulamam bağlıdır.
Uygulamanızı karmaşık yükleme veya yazılım ve işletim sistemi yapılandırmasını gerektiriyorsa, sanal makineleri büyük olasılıkla en iyi çözüm. Sanal makineler ile şunları yapabilirsiniz:

* Bir işletim sistemi, Windows veya Linux gibi başlatmak için sanal makineye Galerisi kullanın ve sonra uygulamanızın gereksinimleri için özelleştirin.
* Oluşturun ve azure'da bir sanal makinede çalıştırmak için var olan bir şirket içi sunucusunun özel bir görüntü yükleyin.

### <a id="oss"></a>Açık kaynak yazılımının Sitem kullanır ve Azure üzerinde barındırmak istediğiniz
Uygulama hizmeti, açık kaynak framework destekleniyorsa, dilleri ve çerçeveleri uygulamanızın gereksinim duyduğu sizin için otomatik olarak yapılandırılır. Uygulama hizmeti sağlar:

* Birçok popüler açık kaynak dilleri, gibi kullandığınız [.NET][dotnet], [PHP][PHP], [Node.js][nodejs], ve [Python][Python].
* WordPress, Drupal, Umbraco, DNN ve diğer birçok üçüncü taraf web uygulamalarını ayarlayın.
* Var olan bir uygulama geçirmek veya uygulama galeriden yeni bir tane oluşturun.

Uygulama hizmeti, açık kaynak framework desteklenmiyorsa, seçenekleri diğer Azure web barındırma birinde çalıştırabilirsiniz. Sanal makineler ile yükleme ve Windows olabilen makine görüntüsüne yazılım yapılandırma veya Linux tabanlı.

### <a id="lob"></a>Şirket ağına bağlanmak için gereken bir iş kolu satır uygulama yüklü
Bir iş kolu satır uygulama oluşturmak istiyorsanız, Web sitenizi Hizmetleri ya da kurumsal ağ üzerindeki veri doğrudan erişim gerektirebilir. Bu uygulama hizmeti, Service Fabric ve kullanarak sanal makineleri mümkündür [Azure Virtual Network service](/azure/virtual-network/). Uygulama hizmeti kullandığınız [VNET tümleştirme özelliği](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), Azure uygulamalarınızı şirket ağınızda değilmiş gibi çalışmasına izin verir.

### <a id="mobile"></a>Bir REST API veya mobil istemciler için web hizmetini barındırmak istiyorum
HTTP tabanlı web hizmetleri çok çeşitli mobil istemciler dahil olmak üzere istemcileri desteklemek etkinleştirin. ASP.NET Web API gibi çerçeveleri oluşturun ve REST hizmetlerini kullanma kolaylaştırmak için Visual Studio ile tümleştirin.  Bu senaryoyu desteklemek için Azure ile ilgili teknik barındırma web kullanmak mümkün olması için bu hizmetleri bir web uç noktasından sunulur. Ancak, REST API'leri barındırmak için harika bir seçim uygulama hizmetidir. App Service ile yapabilecekleriniz:

* Hızlı Oluştur bir [mobil uygulama](../app-service-mobile/app-service-mobile-value-prop.md) veya Azure'nın birinde HTTP web hizmeti genel barındırmak için API uygulaması Dağıtılmış veri merkezlerinde.
* Varolan hizmetlerini geçirme veya yenilerini oluşturun.
* Tek bir örnekle kullanılabilirlik SLA elde etmek veya birden çok ayrılmış makinelerine ölçeğini.
* Yayımlanan site REST API'leri mobil istemciler dahil olmak üzere tüm HTTP istemcilere sağlamak için kullanın.

> [!NOTE]
> Bir hesabı için kaydolmadan önce Azure App Service'i kullanmaya başlamak istiyorsanız, Git <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, burada hemen bir kısa süreli başlangıç uygulaması Azure App Service'te ücretsiz oluşturabilirsiniz. Kredi kartı gerekli, hiçbir taahhüt.
> 
> 

## <a id="nextsteps"></a>Sonraki adımlar
Üç hakkında daha fazla bilgi için web barındırma seçenekleri, bkz: [Tanıtımı Azure](../fundamentals-introduction-to-azure.md).

Uygulamanız için seçilen seçenekleri ile çalışmaya başlamak için aşağıdaki kaynaklara bakın:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Azure sanal makineler](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
