---
title: Get Azure geliştiricileri için Başlarken Kılavuzu | Microsoft Docs
description: Bu konu, Microsoft Azure platformu için geliştirme ihtiyaçlarını kullanmaya başlamak isteyen geliştiriciler için gerekli bilgileri sağlar.
services: ''
cloud: ''
documentationcenter: ''
author: ggailey777
manager: erikre
ms.assetid: ''
ms.service: azure
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: glenga
ms.openlocfilehash: dc44cfbd24bd04caeede03dcbcfc60da06f61135
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60921781"
---
# <a name="get-started-guide-for-azure-developers"></a>Azure geliştiricileri için kullanmaya başlama kılavuzu

## <a name="what-is-azure"></a>Azure nedir?

Azure, mevcut uygulamalarınızı barındırın, yeni uygulamaların geliştirilmesini kolaylaştırır ve hatta şirket içi uygulamalar geliştirmek eksiksiz bir bulut platformudur. Azure, geliştirme, test etmek, dağıtmak ve uygulamalarınızı yönetmek için ihtiyacınız olan bulut hizmetlerini tümleşir — yararlanırken verimliliği bulut bilgi işlem.

Uygulamalarınızı Azure üzerinde barındırarak küçükten başlayabilir ve uygulamanızı, Müşteri talebi arttıkça kolayca ölçeklendirin. Azure Ayrıca, hatta farklı bölgeler arasında yük devretme dahil olmak üzere, yüksek oranda kullanılabilir uygulamalar için gereken güvenilirlik sunar. [Azure portalında](https://portal.azure.com) tüm Azure Hizmetleri kolayca yönetmenize olanak tanır. Hizmete özel API'ler ve şablonları kullanarak, hizmetlerinizi program aracılığıyla da yönetebilirsiniz.

**Kimler bu**: Bu kılavuz, uygulama geliştiricileri için Azure platformunda bir giriş niteliğindedir. Bu, rehberlik ve azure'da yeni uygulamalar oluşturmak veya mevcut uygulamaları azure'a geçirme başlatmanız yönü sağlar.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Azure'un sunduğu tüm hizmetleri ile bu şekil, çözüm mimarisini desteklemek için gereken hizmetleri öğrenmek için göz korkutucu bir görev olabilir. Bu bölümde, geliştiricilerin sık kullandığınız Azure hizmetlerini vurgular. Tüm Azure Hizmetleri listesi için bkz. [Azure belgeleri](../../index.md).

İlk olarak, uygulamanızı azure'da barındırmak nasıl karar vermeniz gerekir. Bir sanal makine (VM) altyapınızın tamamını yönetmenize gerek. Azure sağlayan platform yönetimi özellikleri kullanabilir miyim? Belki de yalnızca ana kod yürütme için sunucusuz bir çerçeve gerekiyor?

Uygulamanızın ihtiyaç duyduğu bulut depolama, Azure için çeşitli seçenekler sunar. Azure'nın enterprise Authentication yararlanabilirsiniz. Aynı zamanda bulut tabanlı geliştirme ve izleme ve çoğu barındırma Hizmetleri Tümleştirme DevOps teklifi için de araçlar vardır.

Şimdi, uygulamalarınız için araştırma öneririz belirli hizmetler bazılarına bakalım.

### <a name="application-hosting"></a>Uygulama barındırma

Azure, çeşitli bulut tabanlı altyapı ayrıntılar konusunda endişelenmeniz gerekmez. böylece uygulamanızı çalıştırmak için teklifleri işlem sağlar. Kolayca artırın veya uygulama kullanımınızı büyüdükçe, kaynaklara ölçeklendirin.

Azure, uygulama geliştirme ve barındırma gereksinimlerini destekleyen hizmetleri sunar. Azure (Iaas) uygulama barındırma üzerinde tam denetim vermek için hizmet olarak altyapı sağlar. Azure'un Platform olarak hizmet (PaaS) teklifleri, uygulamalarınızı desteklemek için gereken tam olarak yönetilen hizmetler sağlar. Yok bile true sunucusuz Azure'da barındırma tek yapmanız gereken olduğu kodunuzu yazın.

![Azure uygulama barındırma seçenekleri](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Azure App Service, web tabanlı projelerinizi yayımlamak için en hızlı yolu istediğinizde düşünün. App Service mobil istemcilerinizle desteği ve REST API'leri kolayca tüketilen yayımlamak için web uygulamalarınızı genişletmenizi kolaylaştırır. Bu platform, üretim ve sürekli ve kapsayıcı tabanlı dağıtımlar test sosyal sağlayıcılar, trafiği tabanlı otomatik ölçeklendirme, kullanılarak kimlik doğrulaması sağlar.

Web uygulamaları, mobil uygulama arka uçları ve API apps oluşturabilirsiniz.

Tüm üç uygulama türü, App Service çalışma zamanı paylaştığından, Web sitesi barındırma, mobil istemciler desteklemek ve API'leri, azure'da tümü aynı proje veya çözümü kullanıma sunma. App Service hakkında daha fazla bilgi için bkz: [Azure Web Apps nedir](../../app-service/overview.md).

App Service ile DevOps aklınızda tasarlanmıştır. Bu, GitHub Web kancası, Jenkins, Azure DevOps, TeamCity ve diğerleri dahil olmak üzere yayımlama ve sürekli tümleştirme dağıtımları için çeşitli araçlar destekler.

Kullanarak mevcut uygulamalarınızı App Service'e geçirebilirsiniz [çevrimiçi geçiş aracı](https://www.migratetoazure.net/).

> **Ne zaman kullanılacağı**: App Service, mevcut web uygulamaları azure'a geçirirken ve tam olarak yönetilen bir barındırma platformu, web uygulamalarınız için gerektiğinde kullanın. App Service, mobil istemciler veya REST API'leri ile uygulamanızı kullanıma desteklemeye gerektiğinde de kullanabilirsiniz.
> 
> **Başlama**: App Service ilk oluşturup dağıtmayı kolaylaştırır [web uygulaması](../../app-service/app-service-web-get-started-dotnet.md), [mobil uygulama](../../app-service-mobile/app-service-mobile-ios-get-started.md), veya [API uygulaması](../../app-service/app-service-web-tutorial-rest-api.md).
> 
> **Şimdi deneyin**: App Service, Azure hesabı için kaydolun gerek kalmadan, platformu denemek için bir kısa süreli uygulaması sağlama sağlar. Platform deneyin ve [Azure App Service uygulamanızı oluşturma](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Olarak altyapı (Iaas) sağlayıcısı olarak, Azure, dağıtmak veya uygulamanızı Windows veya Linux sanal makineleri geçirme olanak tanır. Azure sanal ağ ile birlikte Azure sanal makineler, Windows veya Linux sanal makinelerinizi Azure'da dağıtılmasını destekler. Vm'leri, makinenin yapılandırması üzerinde tam denetim sahibi sahip. Sanal makineleri kullanırken, tüm sunucu yazılım yükleme, yapılandırma, Bakım ve işletim sistemi düzeltme ekleri için sorumlu olursunuz.

Vm'lerde bulunan denetim düzeyi nedeniyle, bir PaaS modele uymayan Azure'da çok çeşitli sunucu iş yükleri çalıştırabilirsiniz. Bu iş yükleri, veritabanı sunucuları, Windows Server Active Directory ve Microsoft SharePoint içerir. Sanal makineler belgeleri ya da daha fazla bilgi için bkz. [Linux](/azure/virtual-machines/linux/) veya [Windows](/azure/virtual-machines/windows/).

> **Ne zaman kullanılacağı**: Sanal makineler, uygulama altyapınızı üzerinden ya da değişiklik yapmak zorunda kalmadan şirket içi uygulama iş yüklerini Azure'a geçirmek için tam denetim istediğinizde kullanın.
> 
> **Başlama**: Oluşturma bir [Linux VM](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) veya [Windows VM](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) Azure portalından.

#### <a name="azure-functions-serverless"></a>Azure işlevleri (sunucusuz)

Yerine oluşturmak ve bir uygulamanın veya altyapının tamamı kodunuzu çalıştırmak için yönetme hakkında endişelenmeden. Ne artık yalnızca kodunuzu yazın ve yanıtta olayları veya bir zamanlamaya göre çalışmasını?  [Azure işlevleri](../../azure-functions/functions-overview.md) olan "sunucusuz" bir-ihtiyacınız kod yazmanızı sağlayan bir stil teklifi. İşlevler ile HTTP isteklerini, Web kancaları, bulut hizmeti olayları veya bir zamanlamaya göre kod yürütme tetiklenir. C gibi tercih ettiğiniz geliştirme dilini içinde kod\#, F\#, Node.js, Python veya PHP. Kullanıma dayalı faturalandırma ile yalnızca kodunuzu yürütür ve gerektiğinde Azure ölçeklendirilebilen süre için ödeme yaparsınız.

> **Ne zaman kullanılacağı**: Diğer Azure Hizmetleri tarafından web tabanlı olaylar tarafından veya bir zamanlamaya göre tetiklenen kodunuz zaman Azure işlevlerini kullanın. Yalnızca kodunuzun çalıştığı süre için ödeme istediğinizde veya tam bir barındırılan projeye ek yükü ihtiyacınız kalmadığında işlevleri de kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure işlevlerine genel bakış](../../azure-functions/functions-overview.md).
> 
> **Başlama**: İşlevleri hızlı başlangıç Öğreticisi izleyin [ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-azure-function.md) portalından.
> 
> **Şimdi deneyin**: Azure işlevleri, bir Azure hesabı için kaydolun zorunda kalmadan kodunuzu çalıştırmanıza olanak tanır. Şimdi, deneyin ve [ilk Azure işlevinizi oluşturma](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric, derleme, paketleme, dağıtma ve ölçeklenebilir ve güvenilir mikro Hizmetleri kolay bir dağıtılmış sistemler platformudur. Kapsamlı uygulama yönetim özellikleri de sağlar sağlama, dağıtma, izleme, yükseltmek/düzeltme eki uygulama için ve dağıtılan uygulamalar siliniyor. Paylaşılan makine havuzu üzerinde çalışan, uygulamalar küçükten başlayabilir ve gerektiği gibi yüzlerce veya binlerce makineyi ölçeklendirin.

Service Fabric .NET (OWIN) ve ASP.NET Core için açık Web arabirimi ile Webapı'yi destekler. Bu, Linux'ta .NET Core hem de Java hizmetler oluşturmaya yönelik SDK'lar sağlar. Service Fabric hakkında daha fazla bilgi için bkz: [Service Fabric belgeleri](https://docs.microsoft.com/azure/service-fabric/).

> **Ne zaman kullanılır:** Uygulama oluşturma ya da bir mikro hizmet mimarisi kullanan mevcut bir uygulamayı yeniden yazma Service Fabric iyi bir seçimdir. Service Fabric, daha fazla denetime veya doğrudan erişim için temel altyapıyı ihtiyacınız olduğunda kullanın.
> 
> **Başlarken:** [İlk Azure Service Fabric uygulamanızı oluşturma](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Uygulamalarınızı Azure hizmetleriyle geliştirin

Uygulama barındırma ek olarak, Azure işlevleri, geliştirme ve Bakım uygulamalarınızın, hem bulutta ve şirket içi iyileştirebilecek hizmet teklifleri sağlar.

#### <a name="hosted-storage-and-data-access"></a>Barındırılan depolama ve veri erişimi

Çoğu uygulama verileri, bu nedenle depolaması gereken nasıl uygulamanızı azure'da barındırmak karar bağımsız olarak, bir veya daha fazla aşağıdaki depolama ve Veri Hizmetleri düşünün.

- **Azure Cosmos DB**: Dilediğiniz sayıda coğrafi bölgede kapsamlı bir SLA ile aktarım hızını ve depolamayı esnek bir şekilde ölçeklendirmenize olanak sağlayan bir Global olarak dağıtılmış çok modelli veritabanı hizmeti. 
  > **Ne zaman kullanılır:** Uygulamanızı belge, tablo veya grafik veritabanları birden çok sayıda iyi tanımlanmış tutarlılık modeli ile bir MongoDB veritabanları dahil olmak üzere, gerektiğinde. 
  > 
  > **Başlama**: [Bir Azure Cosmos DB web uygulaması derleme](../../cosmos-db/create-sql-api-dotnet.md). MongoDB geliştiricisiyseniz bkz [Azure Cosmos DB ile MongoDB uygulaması oluşturma](../../cosmos-db/create-mongodb-dotnet.md).

- **Azure depolama**: Bloblar, kuyruklar, dosyaları ve diğer ilişkisel olmayan veri türlerinin dayanıklı, yüksek oranda kullanılabilir depolama sağlar. Depolama, sanal makineler için depolama temeli sağlar.

  > **Ne zaman kullanılacağı**: Ne zaman uygulamanızı anahtar-değer çiftleri (tablolar), BLOB, dosya paylaşımlarını veya iletileri (kuyruklar) gibi ilişkisel olmayan verileri depolar.
  > 
  > **Başlama**: Bu tür depolama birini seçin: [blobları](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [tabloları](../../cosmos-db/table-storage-how-to-use-dotnet.md), [kuyrukları](../../storage/queues/storage-dotnet-how-to-use-queues.md), veya [dosyaları](../../storage/files/storage-dotnet-how-to-use-files.md).

- **Azure SQL veritabanı**: Azure tabanlı sürümü ilişkisel tablo verilerini bulutta depolamak için Microsoft SQL Server altyapısı. SQL veritabanı, tahmin edilebilir performans, ölçeklenebilirlik hiç kapalı kalma süresi, iş sürekliliği ve veri koruması sağlar.

  > **Ne zaman kullanılacağı**: Uygulamanızın veri depolama ile işlem başvurusal bütünlük gerektirdiğinde desteklemek ve TSQL sorgularını destekler.
  > 
  > **Başlama**: [Azure portalını kullanarak dakikalar içinde SQL veritabanı oluşturma](../../sql-database/sql-database-get-started.md).


Kullanabileceğiniz [Azure Data Factory](../../data-factory/introduction.md) mevcut şirket içi verileri azure'a taşımak için. Verileri buluta taşımaya hazır değilseniz [karma bağlantılar](../../biztalk-services/integration-hybrid-connection-overview.md) bağlandığınız BizTalk Hizmetleri olanak tanır, App Service uygulaması şirket içi kaynaklara barındırılan. Ayrıca, şirket içi uygulamalarınızı Hizmetleri Azure veri ve depolama birimine bağlanabilirsiniz.

#### <a name="docker-support"></a>Docker desteği

Docker kapsayıcıları, işletim sistemi sanallaştırma, bir form, uygulamaların daha verimli ve öngörülebilir bir şekilde dağıtmanızı sağlar. Buna kapsayıcılı bir uygulama üretimde aynı şekilde sistemlerde, geliştirme ve test gibi çalışır. Standart Docker araçları kullanarak kapsayıcıları yönetebilirsiniz. Azure üzerinde kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için mevcut becerilerini ve popüler açık kaynak Araçları'nı kullanabilirsiniz.

Azure kapsayıcılar uygulamalarınızda kullanmak için çeşitli yollar sunar.

- **Azure Docker VM uzantısı**: Sanal makinenize bir Docker konağı olarak görev yapacak Docker araçları ile yapılandırmanıza olanak sağlar.

  > **Ne zaman kullanılacağı**: Bir VM'de uygulamalarınız için tutarlı kapsayıcı dağıtımı oluşturmak istediğinizde ya da kullanmak istediğiniz [Docker Compose](https://docs.docker.com/compose/overview/).
  > 
  > **Başlama**: [Docker VM uzantısını kullanarak Azure'da bir Docker ortamında oluşturma](../../virtual-machines/virtual-machines-linux-dockerextension.md).

- **Azure Container Service'i**: Oluşturma, yapılandırma ve kapsayıcılı uygulamaları çalıştırmak için önceden yapılandırılmış sanal makine kümesi yönetmenize olanak tanır. Container Service hakkında daha fazla bilgi için bkz. [Azure Container Service'e Giriş](../../container-service/container-service-intro.md).

  > **Ne zaman kullanılacağı**: Ek planlama sağlayan yapı üretime hazır ve ölçeklenebilir ortamları ve yönetim araçları veya Docker Swarm kümesi dağıtırken gerektiğinde.
  > 
  > **Başlama**: [Bir kapsayıcı hizmeti kümesini dağıtma](../../container-service/dcos-swarm/container-service-deployment.md).

- **Docker makinesi**: Yükleme ve docker-machine komutlarını kullanarak bir Docker altyapısına sanal konaklar yönetmenize olanak sağlar.

  >**Ne zaman kullanılacağı**: Ne zaman tek bir Docker konağı oluşturarak hızlı bir şekilde prototip için uygulama gerekir.

- **App Service için özel Docker görüntüsü**: Linux üzerinde web uygulaması dağıttığınızda Docker kapsayıcılarını bir kapsayıcı kayıt defterinden ya da müşteri kapsayıcı kullanmanıza olanak sağlar.

  > **Ne zaman kullanılacağı**: Linux üzerinde web uygulaması için bir Docker görüntüsü dağıtırken.
  > 
  > **Başlama**: [Linux üzerinde App Service'te özel bir Docker görüntüsü kullanma](../../app-service/containers/quickstart-docker-go.md).

### <a name="authentication"></a>Kimlik Doğrulaması

Uygulamalarınızı kullanan yalnızca bilmek ancak kaynaklarınıza yetkisiz erişimi önlemek için önemlidir. Azure, uygulama istemcilerin kimliğini doğrulamak için çeşitli yollar sunar.

- **Azure Active Directory (Azure AD)**: Microsoft çok kiracılı, bulut tabanlı kimlik ve erişim yönetimi hizmeti. Azure AD ile tümleştirdiğinizde, çoklu oturum açma (SSO), uygulamalarınıza ekleyebilirsiniz. Dizin özellikleri, doğrudan Azure AD Graph API'si veya Microsoft Graph API'sini kullanarak erişebilirsiniz. OAuth2.0 yetkilendirme framework ve Open ID Connect desteği Azure AD ile yerel HTTP/REST uç noktaları ve çok platformlu Azure AD kimlik doğrulama kitaplıkları kullanarak tümleştirebilirsiniz.

  > **Ne zaman kullanılacağı**: SSO bir deneyim sağlamak istediğinizde, grafik tabanlı verilerle çalışmak veya kullanıcıların etki alanı tabanlı kimlik doğrulaması.
  > 
  > **Başlama**: Daha fazla bilgi için bkz. [Azure Active Directory Geliştirici Kılavuzu](../../active-directory/develop/v1-overview.md).

- **App Service kimlik doğrulaması**: Uygulamanızı barındırmak için App Service'ı seçtiğinizde, sosyal kimlik sağlayıcıları ile birlikte Azure AD için yerleşik kimlik doğrulama desteği ayrıca Al — Facebook, Google, Microsoft ve Twitter gibi.

  > **Ne zaman kullanılacağı**: Azure AD kullanarak bir App Service uygulamasında kimlik doğrulamasını etkinleştirmek istediğinizde sosyal kimlik sağlayıcıları ya da her ikisini de.
  > 
  > **Başlama**: App Service kimlik doğrulaması hakkında daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme Azure App Service'te](../../app-service/overview-authentication-authorization.md).

Azure en iyi güvenlik uygulamaları hakkında daha fazla bilgi için bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>İzleme

Uygulamanızı ayarlama ve Azure'da çalışan ile performansı izlemek için sorunlarını izleyin ve müşterilerin uygulamanızı nasıl kullandığını görün. Azure izleme çeşitli seçenekler sunar.

-   **Visual Studio Application Insights**: Canlı web uygulamalarınızı izleme için Visual Studio ile tümleşen Azure'da barındırılan genişletilebilir bir analiz hizmetidir. Azure üzerinde barındırılan da olup olmadığını performansı ve kullanılabilirliği, uygulamalarınızın sürekli olarak geliştirmek için gereken verileri sağlar.

    >**Başlama**: İzleyin [Application Insights öğretici](../../azure-monitor/app/app-insights-overview.md).

-   **Azure İzleyici**: Görselleştirin, sorgu, yol, arşiv ve Azure altyapınız ve kaynaklar tarafından oluşturulan günlükleri ve ölçümler üzerinde işlem yapmasına yardımcı olan bir hizmet. İzleyici, Azure portalında görmek ve Azure kaynakları izlemek için tek bir kaynak veri görünümleri sağlar.
 
    >**Başlama**: [Azure İzleyici ile çalışmaya başlama](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>DevOps tümleştirmesi

VM'ler sağlamayı veya sürekli tümleştirme ile web uygulamalarınızı yayımlamak ister, Azure ile birçok popüler DevOps araçlarıyla tümleşir. Jenkins, GitHub, Puppet, Chef, TeamCity, Ansible, Azure DevOps ve diğerleri gibi araçlar için destekle, zaten yüklü ve mevcut deneyiminizi en üst düzeye araçları ile çalışabilirsiniz.

> **Şimdi deneyin:** [Birkaç DevOps tümleştirmeleri'ni deneyin](https://azure.microsoft.com/try/devops/).
> 
> **Başlama**: Bir App Service uygulaması DevOps seçeneklerini görmek için bkz. [Azure uygulama Hizmeti'ne sürekli dağıtım](../../app-service/deploy-continuous-deployment.md).


## <a name="azure-regions"></a>Azure bölgeleri

Azure dünyanın dört bir yanındaki birçok bölgede genel kullanıma açık olan bir genel bulut platformudur. Bir hizmet, uygulama veya azure'da VM sağladığınızda, uygulamanızın çalıştığı veya verilerinizin depolandığı, belirli bir veri merkezini temsil eden bir bölge seçin istenir. Bu bölgeler yayımlanan belirli konumlara karşılık [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfası.

### <a name="choose-the-best-region-for-your-application-and-data"></a>Uygulama ve verileriniz için en iyi bir bölge seçin

Azure'ı kullanmanın avantajları, dünyanın çeşitli veri merkezleri, uygulamalarınızı dağıtmadan biridir. Seçtiğiniz bölge, uygulamanızın performansını etkileyebilir. Örneğin, daha iyi ağ istek gecikme süresini azaltmak için müşterilerinize en yakın bir bölge seçin. Belirli ülkelerde uygulamanızı dağıtmak için yasal gereksinimleri karşılamak için bölgenizi seçin isteyebilirsiniz. Aynı veri merkezinde veya bir veri merkezinde uygulamanızı barındıran bir veri merkezine mümkün olduğunca uygulama verilerini depolamak için her zaman en iyi bir uygulamadır.

### <a name="multi-region-apps"></a>Çok bölgeli uygulama

Olası olsa da, bu Internet hatası veya doğal afetler gibi bir olay nedeniyle çevrimdışına veri merkezinin tamamı için mümkün değildir. Birden fazla veri merkezinde konak önemli iş uygulamalarının en yüksek kullanılabilirlik sağlamak için en iyi bir yöntemdir. Kullanarak birden çok bölgede de genel kullanıcılar için gecikme süresini azaltın ve uygulamaları güncelleştirme ek esneklik olanaklarını sağlar.

Sanal makine ve uygulama hizmetleri gibi bazı hizmetler kullanan [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) yüksek kullanılabilirlik Kurumsal uygulamaları desteklemek için bölgeler arasında yük devretme ile birden çok bölge desteği etkinleştirmek için. Bir örnek için bkz [Azure başvuru mimarisi: Bir web uygulaması birden çok bölgede çalıştırın](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

>**Ne zaman kullanılacağı**: Kurumsal ve yük devretme ve çoğaltma avantajlarından yararlanarak yüksek kullanılabilirlik uygulamaları olduğunda.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Uygulamalarımı ve Projelerimi nasıl yönetebilirim?

Azure deneyimler, Azure kaynakları, uygulamaları ve projeleri oluşturmak ve yönetmek size zengin bir özellik kümesi sağlar; hem program aracılığıyla ve [Azure portalında](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>Komut satırı arabirimi ve PowerShell

Azure, uygulamalarınızı ve hizmetlerinizi Bash, Terminal, komut istemini veya tercih ettiğiniz, komut satırı aracını kullanarak komut satırından yönetmek için iki yol sunar. Genellikle, Azure portalında olduğu gibi komut satırından aynı görevleri gerçekleştirebilirsiniz: oluşturma ve sanal makineler, sanal ağlar, web uygulamaları ve diğer hizmetleri yapılandırma gibi.

-   [Azure komut satırı arabirimi (CLI)](../../xplat-cli-install.md): Bir Azure aboneliğine bağlanma ve Azure kaynaklarını komut satırından karşı çeşitli görevleri program olanak sağlar.

-   [Azure PowerShell](../../powershell-install-configure.md): Windows PowerShell kullanarak Azure kaynaklarını yönetmenizi sağlayan cmdlet'ler ile bir modül kümesini sağlar.

### <a name="azure-portal"></a>Azure portal

Azure portalında oluşturmak, yönetmek ve Azure kaynaklarını ve Hizmetleri kaldırmak için kullanabileceğiniz bir web tabanlı bir uygulamadır. Azure portalında şu konumdadır <https://portal.azure.com>. Özelleştirilebilir bir pano, Azure kaynaklarını yönetmek için Araçlar içerir ve erişim için Abonelik ayarları ve fatura bilgilerini. Daha fazla bilgi için [Azure portalına genel bakış](../../azure-portal-overview.md).

### <a name="rest-apis"></a>REST API'leri

Azure REST API'leri, Azure portalı kullanıcı arabirimini destekleyen bir dizi üzerinde geliştirilmiştir. Bu REST API'lerin çoğu, program aracılığıyla sağlama ve Azure kaynaklarını ve uygulamaların Internet özellikli herhangi bir CİHAZDAN yönetme izin vermek için de desteklenir. REST API belgelerini tam kümesi için bkz: [Azure REST SDK başvurusu](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>API'ler

REST API'lerine ek olarak, çoğu Azure hizmeti Ayrıca, program aracılığıyla kaynakları uygulamalarınızdan aşağıdaki geliştirme platformları için SDK'ları dahil olmak üzere, platforma özgü Azure SDK kullanarak yönetmenize olanak tanır:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](https://docs.microsoft.com/javascript/azure)
-   [Java](https://docs.microsoft.com/java/azure)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](https://docs.microsoft.com/python/azure)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)
-   [Go](https://docs.microsoft.com/go/azure)

Gibi hizmetleri [Mobile Apps](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) ve [Azure Media Services](../../media-services/previous/media-services-dotnet-how-to-use.md) istemci-tarafı SDK'lar olanak sağlayan web ve mobil istemci uygulamalardan hizmetlere erişim.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Azure üzerinde büyük olasılıkla, uygulamanızı çalıştıran her biri aynı yaşam döngüsünü izleyin ve, mantıksal bir birim olarak düşünülebilir birden çok Azure Hizmetleri ile çalışmayı içerir. Örneğin, bir web uygulaması Web uygulamaları, SQL veritabanı, depolama, Azure önbelleği için Redis, kullanabilir ve Azure Content Delivery Network hizmetlerinden. [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) bir grup olarak, uygulamanızdaki kaynaklarla çalışma sağlar. Dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle tüm kaynakları silin.

Mantıksal olarak gruplandırarak ve ilgili kaynakları yönetme yanı sıra Azure Resource Manager dağıtımını ve yapılandırmasını, ilgili kaynak özelleştirmenize olanak tanıyan dağıtım özellikleri içerir. Örneğin, Kaynak Yöneticisi'ni kullanarak dağıtma ve birden çok sanal makine, yük dengeleyici ve tek bir birim olarak Azure SQL veritabanı içeren bir uygulamayı yapılandırın.

Bu dağıtımlar, JSON biçimli bir belge olan bir Azure Resource Manager şablonu kullanarak geliştirin. Şablonları, bir dağıtım tanımlayın ve betikler yerine bildirim temelli şablonlar kullanarak uygulamalarınızı yönetmek olanak tanır. Şablonlarınızı test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Örneğin, şablonları kullanarak kod deposundaki bir dizi tek bir tıklamayla Azure Hizmetleri dağıtan bir GitHub deposuna bir düğme ekleyebilirsiniz.

> **Ne zaman kullanılacağı**: Resource Manager şablonları, uygulamanızın REST API'leri, Azure CLI ve Azure PowerShell kullanarak program aracılığıyla yönetebilirsiniz şablon tabanlı bir dağıtım istediğinizde kullanın.
> 
> **Başlama**: Şablonlar ile çalışmaya başlamak için bkz: [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Hesapları anlama, abonelik ve faturalandırma

Geliştiriciler olarak, kodun içine doğrudan içine dalmak ve çalıştırma uygulamalarımızın yapmadan ile mümkün olduğunca hızlı şekilde kullanmaya başlamak deneyin istiyoruz. Kesinlikle Azure'da mümkün olduğunca kolayca çalışmaya başlayın geçirmenizi istiyoruz. Kolay, Azure teklifleri olmasına yardımcı olmak için bir [ücretsiz deneme sürümü](https://azure.microsoft.com/free/). Bazı hizmetler bile bir "ücretsiz deneyin" işlevsellik gibi sahip [Azure App Service](https://tryappservice.azure.com/), hangi gerektirmez, hatta bir hesap oluşturabilirsiniz. Kodlama ve uygulamanızı azure'a dağıtmak derinlerine olarak eğlenceli ayrıca Azure'nın bir kullanıcı hesapları, abonelikleri ve faturalandırma açısından nasıl çalıştığını anlamak için biraz zaman alabilir önemlidir.

### <a name="what-is-an-azure-account"></a>Bir Azure hesabı nedir?

Oluşturun veya bir Azure aboneliği ile çalışmak için bir Azure hesabınız olmalıdır. Bir Azure hesabı, Azure AD'de yalnızca bir kimlik olduğunu veya Azure AD tarafından güvenilen bir iş veya Okul kuruluş gibi bir dizinde. Böyle bir kuruluşa ait olmayan, Microsoft, Azure AD tarafından güvenilen Account kullanarak, her zaman bir aboneliği oluşturabilirsiniz. Şirket içi Windows Server Active Directory, Azure AD ile tümleştirme hakkında daha fazla bilgi için bkz: [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../../active-directory/hybrid/whatis-hybrid-identity.md).

Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Daha fazla bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Kimlikleri olarak da bilinir, tek tek Azure tanımlanmasına ek olarak hesap *kullanıcılar*, ayrıca tanımlayabilirsiniz *grupları* Azure AD'de. Kullanıcı grubu oluşturmak, rol tabanlı erişim denetimi (RBAC) kullanarak bir Abonelikteki kaynakları erişimi yönetmek için iyi bir yoludur. Grupları oluşturmayı öğrenmek için bkz: [Azure Active Directory önizlemesinde bir grup oluşturma](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Ayrıca oluşturma ve yönetme grupları tarafından [PowerShell kullanarak](../../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Aboneliklerinizi yönetme

Bir Azure hesabına bağlı mantıksal bir gruplandırması olan Azure hizmetlerini bir aboneliktir. Tek bir Azure hesabı, birden fazla abonelik içerebilir. Azure Hizmetleri için faturalama, abonelik başına temelinde gerçekleştirilir. Kullanılabilir abonelik teklifleri türüne göre bir listesi için bkz. [Microsoft Azure Teklif Ayrıntıları](https://azure.microsoft.com/support/legal/offer-details/). Abonelik üzerinde tam denetime sahip bir Hesap Yöneticisi ve tüm hizmetleri denetime sahip abonelikte Hizmet Yöneticisi, Azure aboneliğiniz yok. Klasik abonelik yöneticileri hakkında daha fazla bilgi için bkz: [ekleme veya değiştirme Azure aboneliği yöneticileri](../../billing/billing-add-change-azure-subscription-administrator.md). Yöneticiler ek olarak, bireysel hesaplar verilebilir ayrıntılı denetim kullanarak Azure kaynaklarınızın [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md).

#### <a name="resource-groups"></a>Kaynak grupları

Yeni Azure hizmetlerini sağladığınızda, belirli bir abonelikte bunu yapın. Kaynaklar olarak da adlandırılan tek tek Azure hizmetlerinin bir kaynak grubu bağlamında oluşturulur. Kaynak gruplarını dağıtmak ve uygulamanızın kaynakları yönetmek kolaylaştırır. Bir kaynak grubu, bir birim olarak çalışmak istediğiniz uygulama için tüm kaynakları içermelidir. Kaynak grupları arasında ve hatta farklı Aboneliklerde, kaynakları taşıyabilirsiniz. Kaynakları taşıma hakkında bilgi edinmek için [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).

Azure kaynak Gezgini, aboneliğinizde zaten oluşturduğunuz kaynakları görselleştirmek için harika bir araçtır. Daha fazla bilgi için bkz. [kaynakları görüntülemek ve değiştirmek için kullanım Azure kaynak Gezgini](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-to-resources"></a>Kaynaklara erişim izni ver

Azure kaynaklarına erişime izin verdiğinizde, her zaman belirli bir görevi gerçekleştirmek için gereken en az ayrıcalık ile kullanıcılara sağlamak için en iyi uygulama olan.

- **Rol tabanlı erişim denetimi (RBAC)**: Azure'da, belirli bir kapsamda kullanıcı hesapları (asıl hesaplar) erişimi verebilir: Abonelik, kaynak grubu veya tek tek kaynaklar. RBAC, bir kaynak grubunda bir kaynak kümesini dağıtmak ve belirli kullanıcı veya grup için izinler sağlar. Ayrıca hedef kaynak grubuna ait kaynaklara erişimini sağlar. Ayrıca, bir sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni verebilirsiniz. Erişim vermek için kullanıcı, Grup veya hizmet sorumlusu için bir rol atayın. Birçok önceden tanımlı roller vardır ve kendi özel rollerinizi de tanımlayabilirsiniz. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md).

  > **Ne zaman kullanılacağı**: Ayrıntılı erişim yönetimi, bir kullanıcı bir abonelik sahibi olmak gerektiğinde veya kullanıcılar ve gruplar için gerektiğinde.
  > 
  > **Başlama**: Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).

- **Hizmet sorumlusu nesneleri**: Kullanıcı asıl adları ve gruplara erişim sağlamanın yanı sıra hizmet sorumlusu aynı erişim verebilirsiniz.

  > **Ne zaman kullanılacağı**: Ne zaman, program aracılığıyla Azure kaynaklarını yönetmek veya uygulamalar için erişim izni verme. Daha fazla bilgi için [oluşturma Active Directory uygulaması ve hizmet sorumlusu](../../active-directory/develop/howto-create-service-principal-portal.md).

#### <a name="tags"></a>Etiketler

Azure Resource Manager kaynakların için özel etiketler atama olanak sağlar. Faturalandırma veya izleme kaynakları düzenlemek gerektiğinde etiketleri, anahtar-değer çiftleridir yararlı olabilir. Etiketler, birden çok kaynak gruplarındaki kaynakların izlemek için bir yol sağlar. Portalında, Azure Resource Manager şablonunda veya programlama yoluyla, REST API, Azure CLI veya PowerShell kullanarak etiketler atayabilirsiniz. Her kaynak için birden çok etiket atayabilirsiniz. Daha fazla bilgi için bkz. [etiketleri kullanarak Azure kaynaklarınızı düzenleme](../../resource-group-using-tags.md).

### <a name="billing"></a>Faturalandırma

Şirket içinden bulutta barındırılan hizmetlere bilgi işlem hareket izleme ve hizmet kullanımı ve ilgili maliyetleri tahmin etme önemli edilir. Yeni kaynaklar aylık olarak çalıştırmak için maliyet tahmin edebilmek önemlidir. Geçerli harcama dayalı belirli bir ay için faturalandırma nasıl görüneceğini gösteren proje olması gerekir.

#### <a name="get-resource-usage-data"></a>Kaynak kullanım verilerini al

Azure faturalandırma REST kaynak tüketimi ve Azure abonelikleri için meta veri bilgilerini erişmesini API kümesi sağlar. Bu faturalandırma API'lerini daha iyi tahmin edin ve Azure maliyetleri yönetme olanağı sağlayacak. İzleyebilir ve saatlik artışlarla yaptığı Harcamalar çözümlenmiştir harcama uyarıları oluşturma ve geçerli kullanım eğilimlere gelecek faturalandırma tahmin edin.

>**Başlama**: Faturalandırma API'lerini kullanma hakkında daha fazla bilgi edinmek için [Azure faturalama kullanım ve RateCard API'leri genel bakış](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Gelecekteki maliyetleri tahmin edin

Önceden maliyetlerini tahmin etmek zor olsa da, Azure sahip bir [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) dağıtılan kaynakların maliyetini tahmin ederken kullanabilirsiniz. Portal ve faturalandırma REST API'lerini faturalama dikey penceresine, geçerli tüketimini temel alarak, gelecekteki maliyetlerini tahmin etmek için de kullanabilirsiniz.

>**Başlama**: Bkz: [Azure faturalama kullanım ve RateCard API'leri genel bakış](../../billing-usage-rate-card-overview.md).
