---
title: App Service, VM, Service Fabric karşılaştırın ve bulut Hizmetleri - Azure | Microsoft Docs
description: Web uygulamalarını barındırmak için Azure App Service, Sanal Makineler, Service Fabric ve Cloud Services’dan hangisini seçeceğinizi öğrenin.
services: app-service\web, virtual-machines, cloud-services
documentationcenter: ''
author: cephalin
manager: jeconnoc
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: aac2a0b102d50c8d3f0506c2cc1469a838706703
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835558"
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure App Service, Sanal Makineler, Service Fabric ve Cloud Services karşılaştırması

Azure web sitelerini barındırmak için çeşitli yollar sunar: [Azure App Service'e][Azure App Service], [sanal makineler][Virtual Machines], [Service Fabric][Service Fabric], ve [Bulut Hizmetleri][Cloud Services]. Bu makale, Web uygulamanız için seçenekleri anlamanıza ve doğru seçim yapmanıza yardımcı olur.

Azure App Service, çoğu Web uygulaması için en iyi seçenektir. Dağıtım ve yönetim süreçleri platform ile tümleştirilmiştir, siteler hızla yüksek trafik yüklerinin altından kalkacak şekilde ölçeklendirilebilir ve yerleşik yük dengeleme ve trafik yöneticisi yüksek kullanılabilirlik sağlar. Mevcut siteleri [çevrimiçi geçiş aracı][migrate-tool] ile kolayca Azure App Service’a taşıyabilir, Web Uygulaması Galerisi'nden açık kaynaklı bir uygulamayı kullanabilir veya istediğiniz çerçeve ve araçları kullanarak yeni bir site oluşturabilirsiniz. [WebJobs][WebJobs] özelliği, App Service Web uygulamanıza arka plan iş işlemleri eklemeyi kolaylaştırır.

Mikro hizmet mimarisi kullanan yeni bir uygulama oluşturuyor ya da mevcut bir uygulamayı yeniden yazıyorsanız Service Fabric iyi bir seçenektir. Paylaşılan makine havuzu üzerinde çalışan uygulamalar küçükten başlayıp ihtiyaç olduğunda yüzlerce hatta binlerce makineyle devasa bir ölçeğe ulaşabilirler. Durum bilgisi olan hizmetler uygulama durumunu tutarlı ve güvenilir bir şekilde depolamayı kolaylaştırır ve Service Fabric hizmet bölümleme, ölçeklendirme ve kullanılabilirlik işlemlerini sizin yerinize otomatik olarak yönetir.  Service Fabric aynı zamanda .NET İçin Açık Web Arabirimi (OWIN) ve ASP.NET Core ile WebAPI’yi destekler.  App Service ile karşılaştırıldığında Service Fabric ayrıca temel altyapı üzerinde daha fazla kontrol veya ona doğrudan erişim sağlar. Sunucularınıza uzaktan bağlanabilir veya sunucu başlatma görevleri yapılandırabilirsiniz. Cloud Services, kullanım kolaylığı ve kontrol derecesi bakımından Service Fabric’e benzerdir, ancak artık eski bir hizmettir ve yeni dağıtımlar için Service Fabric önerilir.

App Service’ta veya Service Fabric’te çalışması için önemli değişiklikler yapılmasını gerektiren mevcut bir uygulamanız varsa buluta geçmeyi kolaylaştırmak için Sanal Makineler’i seçebilirsiniz. Ne var ki sanal makineleri doğru yapılandırmak, güvenliğini sağlamak ve bakımını yapmak Azure App Service ve Service Fabric ile karşılaştırıldığında çok daha fazla zaman ve IT uzmanlığı gerektirir. Azure Sanal Makineleri düşünüyorsanız VM ortamınızı yamamak, güncelleştirmek ve yönetmek üzere sürekli bir bakım çabası gerekeceğini dikkate aldığınızdan emin olun. Azure Sanal Makineler Hizmet Olarak Altyapı (IaaS) iken App Service ve Service Fabric ise Hizmet Olarak Platform’dur (Paas).

## <a name="features"></a>Özellik Karşılaştırması
Aşağıdaki tabloda, en iyi seçimi yapmanıza yardımcı olmak için App Service, Cloud Services, Sanal Makineler ve Service Fabric yetenekleri karşılaştırılmıştır. Her seçeneğin SLA’sı hakkında güncel bilgiler için bkz. [Azure Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

| Özellik | App Service (Web uygulamaları) | Cloud Services (Web rolleri) | Virtual Machines | Service Fabric | Notlar |
| --- | --- | --- | --- | --- | --- |
| Neredeyse anında dağıtım |X | | |X |Bir uygulamayı veya uygulama güncelleştirmesini Cloud Service’a dağıtmak veya bir VM oluşturmak en az birkaç dakika sürer; bir uygulamayı Web uygulamasına dağıtmak ise birkaç saniye alır. |
| Yeniden dağıtmadan daha büyük makinelere ölçeklendirme |X | | |X | |
| Web sunucusu örnekleri içeriği ve yapılandırmayı paylaşır, yani ölçeklendirirken tekrar dağıtmanız ya da yeniden yapılandırmanız gerekmez. |X | | |X | |
| Birden çok dağıtım ortamı (üretim ve hazırlama) |X |X | |X |Service Fabric, uygulamalarınız için birden çok ortamınız olmasına veya uygulamanızın farklı sürümlerini yan yana dağıtmanıza olanak sağlar. |
| Otomatik işletim sistemi güncelleştirme yönetimi |X |X | | |Kısmen Yama Düzenleme Uygulaması (POA) aracılığıyla ve gelecekte tam olarak. |
| Sorunsuz platform değiştirme (32 bit ile 64 bit arasında kolay geçiş) |X |X | | | |
| GIT, FTP ile kod dağıtma |X | |X | | |
| Web Dağıtımı ile kod dağıtma |X | |X | |Cloud Services güncelleştirmeleri rol örneklerine tek tek dağıtmak için Web Dağıtımı kullanımını destekler. Ancak, bir rolün ilk dağıtımı için kullanamazsınız ve Web Dağıtımını bir güncelleştirme için kullanıyorsanız rolün her örneğine ayrı olarak dağıtmanız gerekir. Üretim ortamlarında Bulut Hizmeti SLA'sı ile kullanabilmek için birden çok örnek gereklidir. |
| WebMatrix desteği |X | |X | | |
| Service Bus, Depolama, SQL Veritabanı gibi hizmetlere erişim |X |X |X |X | |
| Web sitesini veya çok katmanlı mimarinin Web hizmetleri katmanını barındırma |X |X |X |X | |
| Çok katmanlı mimarinin orta katmanını barındırma |X |X |X |X |App Service Web uygulamaları kolayca REST API orta katmanını, [WebJobs](https://go.microsoft.com/fwlink/?linkid=390226) özelliği de arka planda işlenen işleri barındırabilir. Katmana yönelik olarak bağımsız ölçeklenebilirlik elde etmek için WebJobs’ı ayrılmış bir Web sitesinde çalıştırabilirsiniz. |
| Tümleşik Hizmet Olarak MySQL desteği |X |X | | | |
| ASP.NET, klasik ASP, Node.js, PHP, Python desteği |X |X |X |X |Service Fabric, [ASP.NET 5](../service-fabric/service-fabric-reliable-services-communication-aspnetcore.md) kullanarak bir Web ön ucu oluşturmayı destekler veya her türden uygulamayı (Node.js, Java vb.) [konuk yürütülebilir dosyası](../service-fabric/service-fabric-guest-executables-introduction.md) olarak dağıtabilirsiniz. |
| Yeniden dağıtmadan ölçeği birden fazla örneğe genişletme |X |X |X |X |Sanal Makineler, ölçeği birden fazla örneğe genişletebilir ancak bunlar üzerinde çalışan hizmetler, bu ölçek genişletmeyi kullanabilecek şekilde yazılmış olmalıdır. İstekleri makineler arasında yönlendirmek için bir yük dengeleyici yapılandırmanız ve [kullanılabilirlik kümesinde](../virtual-machines/windows/manage-availability.md) birden fazla VM örneğine sahip olduğunuzdan emin olmanız gerekir. |
| SSL desteği |X |X |X |X |App Service Web uygulamalarında özel etki alanı adlarına yönelik SSL yalnızca Temel ve Standart modlarında desteklenir. Web uygulamalarında SSL kullanma hakkında bilgi için bkz. [Azure Web sitesi için SSL sertifikası yapılandırma](app-service-web-tutorial-custom-ssl.md). |
| Visual Studio ile tümleştirme |X |X |X |X | |
| Uzaktan Hata Ayıklama |X |X |X |X | |
| TFS ile kod dağıtma |X |X |X |X | |
| [Azure Sanal Ağı](/azure/virtual-network/) ile ağ yalıtımı |X |X |X |X |Ayrıca bkz. [Azure Web Siteleri Sanal Ağ Tümleştirmesi](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| [Azure Traffic Manager](/azure/traffic-manager/) desteği |X |X |X |X | |
| Tümleşik Uç Nokta İzleme |X |X |X | | |
| Sunuculara uzak masaüstü erişimi | |X |X |X | |
| Herhangi bir özel MSI’yi yükleme | |X |X |X |Service Fabric, tüm yürütülebilir dosyaları [konuk yürütülebilir dosyası](../service-fabric/service-fabric-guest-executables-introduction.md) olarak barındırmanıza olanak tanır veya istediğiniz uygulamayı Vm'lere yükleyebilirsiniz. |
| Başlangıç görevleri tanımlama/yürütme yeteneği | |X |X |X | |
| ETW olayları dinlenebilir | |X |X |X | |

## <a name="scenarios"></a>Senaryolar ve öneriler
Aşağıda, bazı yaygın uygulama senaryoları, hangi Azure Web barındırma seçeneğinin hangi senaryoya uygun olabileceği hakkında önerilerle birlikte verilmiştir.

* [Şirket içi varlıklarla tümleşik iş uygulamaları çalıştırmak için arka plan işlemleri ve veritabanı arka ucu olan bir Web ön ucuna ihtiyacım var.](#onprem)
* [İyi ölçeklenen ve küresel erişim sunan Kurumsal Web sitemi barındırmak için güvenilir bir yola ihtiyacım var.](#corp)
* [Windows Server 2003'te çalışan bir IIS6 uygulamam var.](#iis6)
* [Küçük işletme sahibiyim ve sitemi barındırmak için uygun maliyetli ancak gelecekte büyüyebilecek bir yönteme ihtiyacım var.](#smallbusiness)
* [Web veya grafik tasarımcısıyım ve müşterilerim için Web siteleri tasarlamak ve oluşturmak istiyorum.](#designer)
* [Web ön ucu olan çok katmanlı uygulamamı Buluta geçiriyorum.](#multitier)
* [Uygulamam üst düzeyde özelleştirilmiş Windows veya Linux ortamlarına bağlı olduğundan onu buluta taşımak istiyorum.](#custom)
* [Sitem açık kaynak yazılımı kullanıyor ve ben sitemi Azure üzerinde barındırmak istiyorum.](#oss)
* [Şirket ağına bağlanması gereken bir iş kolu uygulamam var.](#lob)
* [Mobil istemciler için REST API veya Web hizmeti barındırmak istiyorum.](#mobile)

### <a id="onprem"></a> Şirket içi varlıklarla tümleşik iş uygulamaları çalıştırmak için arka plan işlemleri ve veritabanı arka ucu olan bir Web ön ucuna ihtiyacım var.
Azure App Service, karmaşık iş uygulamaları için mükemmel bir çözümdür. Yükü dengelenmiş bir platformda otomatik olarak ölçeklenen, Active Directory ile güvenliği sağlanmış ve şirket içi kaynaklarınıza bağlanan uygulamalar geliştirmenizi sağlar. Birinci sınıf portal ve API’ler aracılığıyla bu uygulamaların yönetimini kolaylaştırır ve uygulama analiz araçlarıyla müşterilerin onları nasıl kullandıklarını anlamanızı sağlar. [Webjobs] [ Webjobs] hibrit bağlantı ve VNet özellikleri bağlanmak kolay yaptığınız sırada web katmanınızın parçası olarak görevleri arka şirket içi kaynaklara ve arka plan işlemleri çalıştırmanın özelliği sağlar. Azure App Service, Web uygulamaları için %99,9 SLA sağlar ve şunları yapmanıza olanak tanır:

* Uygulamalarınızı kendi kendini onaran, otomatik düzeltme eki uygulanan bir bulut platformunda güvenilir bir şekilde çalıştırma.
* Genel veri merkezleri ağında otomatik olarak ölçeklendirme.
* Olağanüstü durum kurtarması için yedekleme ve geri yükleme.
* ISO, SOC2 ve PCI uyumlu olma.
* Active Directory ile tümleştirme

### <a id="corp"></a> İyi ölçeklenen ve küresel erişim sunan Kurumsal Web sitemi barındırmak için güvenilir bir yola ihtiyacım var.
Azure App Service, şirket Web sitelerini barındırmak için harika bir çözümdür. Küresel veri merkezleri ağına yönelik talebi karşılamak için Web uygulamalarını hızla ve kolayca ölçeklendirmenizi sağlar. Yerel erişim, hataya dayanıklılık ve akıllı trafik yönetimi sunar. Birinci sınıf yönetim araçları sağlayan bir platform üzerinde, site durumunu ve site trafiğini hızla ve kolayca kavramanıza olanak sağlar. Azure App Service, Web uygulamaları için %99,9 SLA sağlar ve şunları yapmanıza olanak tanır:

* Web sitelerinizi kendi kendini onaran, otomatik düzeltme eki uygulanan bir bulut platformunda güvenilir bir şekilde çalıştırma.
* Genel veri merkezleri ağında otomatik olarak ölçeklendirme.
* Olağanüstü durum kurtarması için yedekleme ve geri yükleme.
* Tümleşik araçlarla günlükleri ve trafiği yönetme.
* ISO, SOC2 ve PCI uyumlu olma.
* Active Directory ile tümleştirme

### <a id="iis6"></a> Windows Server 2003'te çalışan bir IIS6 uygulamam var.
Azure App Service, eski IIS6 uygulamalarını geçirme ile ilişkili altyapı maliyetlerini önlemeyi kolaylaştırır. Microsoft, uyumluluğu denetlemenizi ve yapılması gereken tüm değişiklikleri belirlemenizi sağlayan [kullanımı kolay geçiş araçları ve ayrıntılı geçiş yönergeleri][migrate-tool] hazırlamıştır. Visual Studio, TFS ve ortak CMS araçları ile tümleştirme, IIS6 uygulamalarını doğrudan buluta dağıtmayı kolaylaştırır. Uygulama dağıtıldıktan sonra, Azure Portalı, maliyetleri yönetmek amacıyla ölçeği azaltmanızı ve gerektiğinde talebi karşılamak için de genişletmenizi sağlayan güçlü yönetim araçları sunar. Geçiş aracı ile şunları yapabilirsiniz:

* Eski Windows Server 2003 Web uygulamanızı hızlı ve kolay bir şekilde buluta geçirme.
* Hibrit uygulama oluşturmak amacıyla iliştirilmiş SQL veritabanını şirket içinde bırakmayı tercih etme.
* SQL veritabanınızı eski uygulamanızla birlikte otomatik olarak taşıma.

### <a id="smallbusiness"></a> Küçük işletme sahibiyim ve sitemi barındırmak için uygun maliyetli ancak gelecekte büyüyebilecek bir yönteme ihtiyacım var.
Ücretsiz kullanmaya başlayabileceğinizden ve gerektiğinde daha fazla özellikler ekleyebileceğinizden dolayı Azure App Service bu senaryo için harika bir çözümdür. Ücretsiz Web uygulamalarının her biri Azure tarafından sağlanan bir etki alanı ile birlikte gelir (*sirketiniz*.azurewebsites.net) ve bu platform, tümleşik dağıtım ve yönetim araçlarının yanı sıra kullanmaya başlamayı kolaylaştıran bir uygulama galerisi de içerir. Artan kullanıcı talebiyle birlikte sitenin gelişmesine izin veren başka birçok hizmetleri ve ölçeklendirme seçenekleri vardır. Azure App Service ile yapabilecekleriniz:

* Ücretsiz katman ile başlayıp ardından gerektikçe ölçeği artırma.
* WordPress gibi popüler Web uygulamalarını hızlı bir şekilde kurmak için Uygulama Galerisini kullanma.
* Gerektikçe uygulamanıza başka Azure hizmetleri ve özellikleri ekleme.
* Web uygulamanızı HTTPS ile güvenli hale getirme.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

### <a id="designer"></a> Web veya grafik tasarımcısıyım ve müşterilerim için Web siteleri tasarlamak ve oluşturmak istiyorum
Web geliştiricileri ve tasarımcıları için Azure App Service çeşitli çerçeve ve araçlarla kolayca tümleşir, Git ve FTP için dağıtım desteği içerir ve ayrıca Visual Studio ve SQL Veritabanı gibi araç ve hizmetlerle sıkı tümleştirme sağlar. App Service ile yapabilecekleriniz:

* [Otomatik görevler][scripting] için komut satırı araçlarını kullanma.
* [.Net][dotnet], [PHP][PHP], [Node.js][nodejs] ve [Python][Python] gibi popüler dillerle çalışma.
* Ölçeği çok yüksek kapasitelere artırmak için üç farklı ölçeklendirme düzeyini seçme.
* [SQL Veritabanı][sqldatabase], [Service Bus] [ servicebus] ve [Depolama][Storage] gibi diğer Azure hizmetleriyle veya MySQL ve MongoDB gibi [Azure Mağazası][azurestore]’ndan iş ortağı teklifleriyle tümleştirme.
* Visual Studio, Git, WebMatrix, WebDeploy, TFS ve FTP gibi araçlarla tümleştirme.

### <a id="multitier"></a> Web ön ucu olan çok katmanlı uygulamamı Buluta geçiriyorum
Veritabanına bağlanan Web sunucusu gibi çok katmanlı bir uygulama çalıştırıyorsanız Azure App Service, Azure SQL Veritabanı ile sıkı tümleştirme sunan iyi bir seçenektir. Ayrıca, arka uç işlemlerini çalıştırmak için WebJobs özelliğini kullanabilirsiniz.

Sunucunuzda uzaktan oturum açma veya sunucu başlangıç görevlerini yapılandırma gibi sunucu ortamı üzerinde daha fazla denetime ihtiyacınız varsa katmanlarınızdan biri veya birkaçı için Service Fabric’i seçin.

Kendi makine görüntünüzü kullanmak veya Service Fabric üzerinde yapılandıramayacağınız sunucu yazılımları ya da hizmetleri çalıştırmak istiyorsanız bir veya daha fazla katmanınız için Sanal Makineleri seçin.

### <a id="custom"></a> Uygulamam üst düzeyde özelleştirilmiş Windows veya Linux ortamlarına bağlı olduğundan onu buluta taşımak istiyorum.
Uygulamanız yazılım ve işletim sistemi için karmaşık yükleme veya yapılandırma gerektiriyorsa Sanal Makineleri büyük olasılıkla en iyi çözümdür. Sanal Makineler ile şunları yapabilirsiniz:

* Sanal Makine galerisini kullanarak Windows veya Linux gibi bir işletim sistemi ile başlama ve sonra uygulama gereksinimlerinize göre özelleştirme.
* Var olan bir şirket içi sunucunun özel görüntüsünü oluşturma ve Azure'da bir sanal makinede çalışacak şekilde yükleme.

### <a id="oss"></a> Sitem açık kaynak yazılımı kullanıyor ve ben sitemi Azure üzerinde barındırmak istiyorum
Açık kaynaklı çerçeveniz App Service’ta destekleniyorsa uygulamanızın gereksinim duyduğu diller ve çerçeveler sizin için otomatik olarak yapılandırılır. App Service şunları yapmanızı sağlar:

* [.NET][dotnet], [PHP][PHP], [Node.js][nodejs] ve [Python][Python] gibi birçok popüler açık kaynaklı dili kullanma.
* WordPress, Drupal, Umbraco, DNN ve diğer birçok üçüncü taraf Web uygulamasını kurma.
* Var olan bir uygulamayı geçirme veya Uygulama Galerisinden yeni bir tane oluşturma.

Açık kaynaklı çerçeveniz App Service’ta desteklenmiyorsa onu diğer Azure Web barındırma seçeneklerinden birinde çalıştırabilirsiniz. Sanal Makineler ile yazılımı Windows veya Linux tabanlı olabilen makine görüntüsüne yükler ve yapılandırırsınız.

### <a id="lob"></a> Şirket ağına bağlanması gereken bir iş kolu uygulamam var
Bir iş kolu uygulaması oluşturmak istiyorsanız Web siteniz şirket ağındaki hizmetlere ya da verilere doğrudan erişmeyi gerektirebilir. Bu App Service’ta, Service Fabric’te ve Sanal Makineler’de [Azure Sanal Ağ hizmetini](/azure/virtual-network/) kullanarak yapılabilir. App Service üzerinde kullanabileceğiniz [VNet tümleştirme özelliğini](/azure/app-service/web-sites-integrate-with-vnet), Azure uygulamalarınızın şirket ağınızda değilmiş gibi çalışmasına izin verir.

### <a id="mobile"></a> Mobil istemciler için REST API veya Web hizmeti barındırmak istiyorum
HTTP tabanlı Web hizmetleri mobil istemciler dahil olmak üzere çok çeşitli istemcileri desteklemenizi sağlar. ASP.NET Web API gibi çerçeveler, REST hizmetlerini oluşturup kullanmayı kolaylaştırmak için Visual Studio ile tümleşirler.  Bu hizmetler bir Web uç noktasından sunulduğundan bu senaryoyu desteklemek için Azure’da herhangi bir Web barındırma yöntemini kullanmak mümkündür. Ancak, App Service REST API'leri barındırmak için harika bir seçimdir. App Service ile yapabilecekleriniz:

* HTTP Web hizmetini Azure'un küresel çapta dağıtılmış veri merkezlerinden birinde barındırmak için hızla bir [mobil uygulama](../app-service-mobile/app-service-mobile-value-prop.md) veya API uygulaması oluşturma.
* Var olan hizmetleri geçirme veya yenilerini oluşturma.
* Tek bir örnekle kullanılabilen SLA elde etme veya birden çok ayrılmış makineye ölçeği genişletme.
* REST API'leri mobil istemciler dahil olmak üzere herhangi bir HTTP istemcisine sağlamak için yayımlanmış siteyi kullanma.

> [!NOTE]
> Bir hesaba kaydolmadan önce Azure App Service’ı kullanmaya başlamak istiyorsanız Azure App Service’ta hemen kısa süreli bir başlangıç uygulamasını ücretsiz olarak oluşturabileceğiniz <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a> sayfasına gidin. Kredi kartı ve taahhüt gerekli değildir.

## <a id="nextsteps"></a> Sonraki Adımlar
Üç Web barındırma seçeneği hakkında daha fazla bilgi için bkz. [Azure’a Giriş](../fundamentals-introduction-to-azure.md).

Uygulamanıza yönelik belirlenmiş seçeneklerle başlamak için aşağıdaki kaynaklara bakın:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Azure Sanal Makineler](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[WebJobs]: https://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
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
[migrate-tool]: https://www.movemetothecloud.net/
