---
title: Get Azure üzerinde geliştiriciler için Başlangıç Kılavuzu | Microsoft Docs
description: Bu konu, Microsoft Azure platform için geliştirme gereksinimlerine kullanmaya başlamak için isteyen geliştiriciler için önemli bilgiler sağlar.
services: ''
cloud: ''
documentationcenter: ''
author: ggailey777
manager: erikre
ms.assetid: ''
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: glenga
ms.openlocfilehash: 0cc08ee595bda478adbb0e39dbebca8019ebbad4
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-guide-for-azure-developers"></a>Azure geliştiricileri için kullanmaya başlama kılavuzu

## <a name="what-is-azure"></a>Azure nedir?

Azure, mevcut uygulamalarınızı barındırmak, yeni uygulamaların geliştirilmesini kolaylaştırır ve hatta şirket içi uygulamalar geliştirmek tam bulut platformudur. Azure tümleştirir geliştirmek, test, dağıtmak ve uygulamalarınızı yönetmek için gereken bulut Hizmetleri — bulut verimliliği yararlanarak çalışırken bilgisayar.

Azure uygulamalarınızda barındırarak küçük başlayın ve müşteri talebi arttıkça uygulamanızı kolayca ölçeklendirin. Azure bile farklı bölgelere arasında yük devretme de dahil olmak üzere, yüksek kullanılabilirlik uygulamaları için gerekli olan güvenilirlik de sunar. [Azure portal](https://portal.azure.com) tüm Azure hizmetlerinde kolayca yönetmenize olanak sağlar. Hizmete özgü API'leri ve şablonlar kullanarak hizmetlerinizi program aracılığıyla da yönetebilirsiniz.

**Kimler bu**: Azure platformunda uygulama geliştiricileri için giriş niteliğindeki bu kılavuzdur. Kılavuz ve Azure içinde yeni uygulamalar oluşturmak veya var olan Azure uygulamalarını geçirme başlatmak için gereken yönü sağlar.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Azure'un sunduğu tüm hizmetleri ile bu çözüm mimarisini desteklemek için gereken hangi hizmetlerini bulmak için zorlu bir görev olabilir. Bu bölüm, geliştiricilerin sık kullandığınız Azure Hizmetleri vurgular. Tüm Azure hizmetlerini bir listesi için bkz: [Azure belgelerine](../../index.md).

İlk olarak, uygulamanızı azure'da barındırmak nasıl karar vermeniz gerekir. Bir sanal makine (VM) bütün altyapınızı yönetme gerekir. Azure sağlar platform yönetim olanakları kullanabilir miyim? Belki de konak kod yalnızca yürütülmesine sunucusuz bir çerçeve gerekiyor?

Uygulamanızı Azure için çeşitli seçenekler sağlayan bulut depolama gerekir. Azure'nın Kurumsal kimlik doğrulamasının avantajından yararlanabilirsiniz. Aynı zamanda bulut tabanlı geliştirme ve izleme ve çoğu barındırma hizmetleri DevOps tümleştirme sunmak için de araçlar vardır.

Şimdi, bazı uygulamalarınız için araştırma öneririz belirli hizmetleri bakalım.

### <a name="application-hosting"></a>Uygulama barındırma

Azure, çeşitli bulut tabanlı altyapı ayrıntılarını endişelenmenize gerek yoktur, uygulamanız çalışmaya teklifleri işlem sağlar. Kolayca artırın veya kaynaklarınızı, uygulama kullanımınızı büyüdükçe ölçeğinizi.

Azure uygulama geliştirme ve gereksinimlerini barındırma destek hizmetleri sunar. Azure (Iaas) uygulama barındırma üzerinde tam denetim vermek için hizmet olarak altyapı sağlar. Azure'nın Platform (PaaS) hizmet teklifleri olarak uygulamalarınızı güç için gereken tam olarak yönetilen hizmetler sağlar. Olduğu bile true sunucusuz Azure'da barındıran tüm yapmanız gereken kodunuzu yazın.

![Azure uygulama seçenekleri barındırma](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Web tabanlı projeleri yayımlamak için en hızlı yolu istediğinizde, Azure App Service göz önünde bulundurun. Uygulama hizmeti, mobil istemcilerinizi desteklemek ve kolayca tüketilen REST API'leri yayımlamak için web uygulamalarınızı genişletmek kolaylaştırır. Bu platform, üretim ve sürekli ve kapsayıcı tabanlı dağıtımlar test sosyal sağlayıcılardan, trafiği tabanlı otomatik ölçeklendirmenin kullanarak kimlik doğrulaması sağlar.

Web uygulamaları, mobil uygulama arka uçları ve API uygulamaları oluşturabilirsiniz.

Tüm üç uygulama türleri uygulama hizmeti çalışma zamanı paylaştığından, Web sitesi barındırma, mobil istemcileri desteklemek ve Azure, Apı'lerinizi tümü aynı proje veya çözüm kullanıma. Uygulama hizmeti hakkında daha fazla bilgi için bkz: [Azure Web Apps nedir](../../app-service/app-service-web-overview.md).

Uygulama hizmeti ile DevOps göz önünde tasarlanmıştır. Bu, GitHub Web kancası, Jenkins, Visual Studio Team Services, TeamCity ve diğerleri de dahil olmak üzere yayımlama ve sürekli tümleştirme dağıtımları için çeşitli araçlar destekler.

Kullanarak mevcut uygulamalarınızı App Service'e geçirebilirsiniz [çevrimiçi geçiş aracı](https://www.migratetoazure.net/).

>**Ne zaman kullanılacağı**: uygulama varolan geçirmekte zaman hizmeti kullanan web uygulamaları ve Azure web uygulamaları için tam olarak yönetilen bir ana bilgisayar platformu gerektiğinde. Mobil istemcilerin desteklemek veya uygulamanızla REST API'lerini kullanıma gerektiğinde uygulama hizmetini de kullanabilirsiniz.

>**Başlama**: uygulama hizmeti oluşturma ve dağıtma ilk kolaylaştırır [web uygulaması](../../app-service/app-service-web-get-started-dotnet.md), [mobil uygulama](../../app-service-mobile/app-service-mobile-ios-get-started.md), veya [API uygulaması](../../app-service/app-service-web-tutorial-rest-api.md).

>**Şimdi deneyin**: App Service platformu Azure hesabı için kaydolun gerek kalmadan denemek için bir kısa süreli uygulaması sağla olanak sağlar. Platform deneyin ve [Azure uygulama hizmeti uygulamanızı oluşturun](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Olarak altyapı (Iaas) sağlayıcısı olarak, Azure dağıtmak veya uygulamanızı Windows veya Linux sanal makineleri geçirmek olanak sağlar. Azure Virtual Network ile birlikte, Azure sanal makineleri Windows veya Linux sanal makineleri azure'a dağıtımını destekler. VM'ler ile makinenin yapılandırmasını toplam denetime sahip olursunuz. Sanal makineleri kullanırken, tüm sunucu yazılımı yükleme, yapılandırma, Bakım ve işletim sistemi için düzeltme ekleri sorumlu.

Vm'lerde bulunan denetim düzeyi nedeniyle, çok çeşitli sunucu iş yükleri uymayan Azure üzerinde bir PaaS modelini çalıştırabilirsiniz. Bu iş yükleri, veritabanı sunucuları, Windows Server Active Directory ve Microsoft SharePoint içerir. Daha fazla bilgi için ya da sanal makineleri belgelerine bakın [Linux](/azure/virtual-machines/linux/) veya [Windows](/azure/virtual-machines/windows/).

>**Ne zaman kullanılacağı**: tam istediğiniz ederken sanal makinelerin'ı kullanmak kontrol uygulama altyapınızı üzerinden veya değişiklik yapmak zorunda kalmadan şirket içi uygulama iş yüklerini Azure'a geçirmek için.

>**Başlama**: oluşturmak bir [Linux VM](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) veya [Windows VM](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) Azure portalından.

#### <a name="azure-functions-serverless"></a>Azure işlevleri (sunucusuz)

Yerine çıkışı oluşturma ve kodunuzu çalıştırmak için tüm uygulama veya altyapısını yönetme hakkında kaygı. Ne, yalnızca kodunuzu yazın ve yanıtta olayları veya bir zamanlamaya göre çalıştırmak sahip?  [Azure işlevleri](../../azure-functions/functions-overview.md) olan "sunucusuz" bir-, ihtiyacınız olan kodu yazmanıza olanak veren stili teklifi. İşlevleriyle, HTTP isteklerini, Web kancalarını, bulut hizmeti olayları tarafından veya bir zamanlamaya göre kod yürütmeyi tetiklenir. C gibi tercih ettiğiniz geliştirme dilini içindeki kod\#, F\#, Node.js, Python veya PHP. Tüketim tabanlı faturalama ile yalnızca kodunuzu yürütür ve gerektiğinde Azure ölçeklendirir süre için ödeme yaparsınız.

>**Ne zaman kullanılacağı**: Azure web tabanlı olaylar tarafından veya bir zamanlamaya göre diğer Azure Hizmetleri tarafından tetiklenen kod olduğunda işlevleri kullanın. Tam bir barındırılan projesini ya da yalnızca kodunuzun çalıştığı zaman için ödeme yapmak istediğiniz ek yükü gerekmediğinde işlevleri de kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure işlevlerine genel bakış](../../azure-functions/functions-overview.md).

>**Başlama**: işlevleri hızlı başlangıç öğreticisi için izleyin [ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-azure-function.md) portalından.

>**Şimdi deneyin**: Azure işlevleri, bir Azure hesabı için kaydolun gerek kalmadan kodunuzu çalıştırmak olanak sağlar. Şimdi, deneyin ve [ilk Azure işlevinizi oluşturma](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric oluşturmak, paketi, dağıtmak ve ölçeklenebilir ve güvenilir mikro daha kolay hale getirir dağıtılmış sistemler platformudur. Kapsamlı uygulama yönetim özellikleri de sağlar sağlama, dağıtma, izleme, yükseltme ve düzeltme için ve dağıtılan uygulamalar siliniyor. Paylaşılan bir makineler havuzu üzerinde çalışır, uygulamalar, küçük başlayın ve yüzlerce veya binlerce makinelerin gerektiği şekilde ölçekleyebilirsiniz.

Service Fabric .NET (OWIN) ve ASP.NET Core için açık Web arabirimi ile Webapı destekler. SDK'ları .NET Core ve Java Linux'ta hizmetler oluşturmaya yönelik sağlar. Service Fabric hakkında daha fazla bilgi için bkz: [Service Fabric öğrenme yolunu](https://azure.microsoft.com/documentation/learning-paths/service-fabric/).

>**Ne zaman kullanılmalı:** Service Fabric olduğunda iyi bir seçimdir uygulama oluşturma ya da mikro hizmet mimarisi kullanmak için mevcut bir uygulamayı yeniden yazma işlemi. Daha fazla denetime ya da doğrudan erişim, altyapının gerektiğinde Service Fabric kullanın.

>**Kullanmaya başlama:** [ilk Azure Service Fabric uygulamanızı oluşturma](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Uygulamalarınızı Azure hizmetleriyle geliştirmek

Uygulama barındırma yanı sıra Azure işlevleri, geliştirme ve Bakım uygulamalarınızın, hem bulutta ve şirket içi geliştirebilirsiniz hizmet teklifleri sağlar.

#### <a name="hosted-storage-and-data-access"></a>Barındırılan depolama ve veri erişimi

Çoğu uygulama verileri, bu nedenle depolamanız gerekir nasıl uygulamanızı azure'da barındırmak karar bağımsız olarak, bir veya daha fazla aşağıdaki depolama ve Veri Hizmetleri düşünün.

-   **Azure Cosmos DB**: özellikler esnek herhangi bir sayıda kapsamlı bir SLA ile coğrafi bölgeler arasında işleme ve depolama ölçek olanak tanıyan bir genel dağıtılmış, birden çok model veritabanı hizmeti. 
    >**Ne zaman kullanılmalı:** uygulamanızı belge, tablo veya grafik veritabanları gerektiğinde, birden çok iyi tanımlanmış tutarlılık modellerle MongoDB veritabanları dahil olmak üzere. 

    >**Başlama**: [Azure Cosmos DB web uygulaması oluşturma](../../cosmos-db/create-sql-api-dotnet.md). MongoDB Geliştirici değilseniz, bkz [Azure Cosmos DB ile MongoDB web uygulaması oluşturma](../../cosmos-db/create-mongodb-dotnet.md).

-   **Azure depolama**: BLOB, kuyruklar, dosyalar ve ilişkisel olmayan verileri diğer tür dayanıklı ve yüksek oranda kullanılabilir depolama sunar. Depolama VM'ler için depolamaya temel sağlar.

    >**Ne zaman kullanılacağı**: zaman uygulamanızı anahtar-değer çiftleri (tablolar) gibi ilişkisel olmayan verileri depolar, BLOB'ların, paylaşımları dosyaları veya iletileri (sıralar için).

    >**Başlama**: Bu türleri depolama birini seçin: [BLOB'lar](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [tabloları](../../cosmos-db/table-storage-how-to-use-dotnet.md), [sıraları](../../storage/queues/storage-dotnet-how-to-use-queues.md), veya [dosyaları](../../storage/files/storage-dotnet-how-to-use-files.md).

-   **Azure SQL veritabanı**: bulutta tablo ilişkisel veri depolamak için Microsoft SQL Server altyapısını bir Azure tabanlı sürümü. SQL veritabanı tahmin edilebilir performans, ölçeklenebilirlik hiçbir kapalı kalma süresi, iş sürekliliği ve veri koruması sağlar.

    >**Ne zaman kullanılacağı**: zaman uygulamanızın gerektirdiği başvuru bütünlüğü, işlem desteği ve desteği ile veri depolama için TSQL sorgular.

    >**Başlama**: [Azure portalını kullanarak dakika içinde bir SQL veritabanı oluşturma](../../sql-database/sql-database-get-started.md).


Kullanabileceğiniz [Azure Data Factory](../../data-factory/introduction.md) mevcut şirket içi veri azure'a taşımak için. Bulut için veri taşımak hazır değilseniz [karma bağlantılar](../../biztalk-services/integration-hybrid-connection-overview.md) bağlandığınız BizTalk Services olanak tanır, uygulama hizmeti şirket içi kaynaklara uygulama barındırılan. Ayrıca, şirket içi uygulamalarınızdan Hizmetleri Azure veri ve depolama birimine bağlanabilir.

#### <a name="docker-support"></a>Docker desteği

Docker kapsayıcıları, işletim sistemi sanallaştırma, bir form, uygulamaları daha etkili ve tahmin edilebilir bir şekilde dağıtmasını sağlar. Kapsayıcılı uygulama üretimde yolu sistemlerde, geliştirme ve test gibi aynı şekilde çalışır. Kapsayıcıları standart Docker araçlarını kullanarak yönetebilirsiniz. Azure üzerinde kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için var olan becerileri ve popüler açık kaynaklı Araçları'nı kullanabilirsiniz.

Azure kapsayıcı uygulamalarınızda kullanmak için çeşitli yöntemler sağlar.

-   **Azure Docker VM uzantısı**: VM Docker araçlarıyla Docker ana bilgisayar olarak görev yapması için yapılandırmanızı sağlar.

    >**Ne zaman kullanılacağı**: bir VM'de, uygulamalarınız için tutarlı kapsayıcı dağıtımlarını oluşturmak istediğinizde ya da kullanmak istediğinizde [Docker Compose](https://docs.docker.com/compose/overview/).

    >**Başlama**: [Docker VM uzantısı kullanılarak bir Docker ortamı oluşturma](../../virtual-machines/virtual-machines-linux-dockerextension.md).

-   **Azure kapsayıcı hizmeti**: oluşturma, yapılandırma ve sanal makinelerin kapsayıcılı uygulamaları çalıştırmak için yapılandırılmış bir küme yönetmenize olanak tanır. Kapsayıcı hizmeti hakkında daha fazla bilgi için bkz: [Azure kapsayıcı hizmeti giriş](../../container-service/container-service-intro.md).

    >**Ne zaman kullanılacağı**: ek planlama ve yönetim araçları sağlamak veya ne zaman dağıtmakta Docker Swarm kümesi üretime hazır, ölçeklenebilir ortamlar oluşturmak gerektiğinde.

    >**Başlama**: [bir kapsayıcı hizmeti kümesini dağıtma](../../container-service/dcos-swarm/container-service-deployment.md).

-   **Docker makine**: yükleme ve Docker altyapısına sanal konaklarda docker makine komutlarını kullanarak yönetmenize olanak tanır.

    >**Ne zaman kullanılacağı**: gerektiğinde hızlı bir şekilde prototip için bir uygulama tek bir Docker ana oluşturarak.

-   **Uygulama hizmeti için özel Docker görüntü**: Linux üzerinde bir web uygulaması dağıttığınızda bir kapsayıcı kayıt defteri Docker kapsayıcılardan veya bir müşteri kapsayıcı kullanmanıza olanak sağlar.

    >**Ne zaman kullanılacağı**: Linux üzerinde bir web uygulaması için Docker görüntü dağıtırken.

    >**Başlama**: [Linux uygulama hizmeti için özel bir Docker görüntü kullanmak](../../app-service/containers/quickstart-docker-go.md).

### <a name="authentication"></a>Kimlik Doğrulaması

Yalnızca uygulamalarınızı kullanan öğrenmek için aynı zamanda kaynaklarınıza yetkisiz erişimi önlemek için önemlidir. Azure app istemcilerin kimliğini doğrulamak için çeşitli yöntemler sağlar.

-   **Azure Active Directory (Azure AD)**: Microsoft çok kiracılı, bulut tabanlı kimlik ve erişim yönetimi hizmeti. Azure AD ile tümleştirme, uygulamalarınız için çoklu oturum açma (SSO) ekleyebilirsiniz. Dizin özelliklerini doğrudan Azure AD grafik API'si veya Microsoft Graph API kullanarak erişebilirsiniz. Azure AD desteği OAuth2.0 yetkilendirme framework ve Open ID Connect ile yerel HTTP/REST uç noktaları ve multiplatform Azure AD kimlik doğrulama kitaplıklarını kullanarak tümleştirebilirsiniz.

    >**Ne zaman kullanılacağı**: SSO bir deneyim sağlamak istediğinizde, grafik tabanlı verilerle çalışma veya kullanıcıların etki alanı tabanlı kimlik doğrulaması.

    >**Başlama**: daha fazla bilgi için bkz: [Azure Active Directory Geliştirici Kılavuzu](../../active-directory/develop/active-directory-developers-guide.md).

-   **App Service kimlik doğrulaması**: uygulamanızı barındırmak için uygulama hizmeti seçtiğinizde, sosyal kimlik sağlayıcıları yanı sıra Azure AD için yerleşik kimlik doğrulama desteği de alın — Facebook, Google, Microsoft ve Twitter gibi.

    >**Ne zaman kullanılacağı**: Azure AD kullanarak bir uygulama hizmeti uygulamasında kimlik doğrulamasını etkinleştirmek istediğinizde sosyal kimlik sağlayıcıları ya da her ikisini de.

    >**Başlama**: uygulama hizmetinde kimlik doğrulaması hakkında daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme Azure App Service'te](../../app-service/app-service-authentication-overview.md).

Azure en iyi güvenlik uygulamaları hakkında daha fazla bilgi için bkz: [Azure güvenlik en iyi yöntemler ve yaklaşımlar](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>İzleme

Uygulamanızı Azure'da çalışan ile, performansını izlemek için sorunlarını izlemek ve müşteriler uygulamanızı nasıl kullandığını görmek. Azure birkaç izleme seçeneklerini sağlar.

-   **Visual Studio Application Insights**: Canlı web uygulamalarınızı izlemek için Visual Studio ile tümleşen bir Azure barındırılan genişletilebilir bir analiz hizmetidir. Azure üzerinde barındırılan da veya olup olmadığını sürekli uygulamalarınızın, kullanılabilirlik ve performansı geliştirmek için gereken verileri sağlar.

    >**Başlama**: izleyin [Application Insights Öğreticisi](../../application-insights/app-insights-overview.md).

-   **Azure İzleyici**: Görselleştirme, sorgu, yol, arşiv ve ölçümleri ve Azure altyapı ve kaynaklar tarafından oluşturulan günlükleri hareket olanak sağlayan hizmet. İzleyici Azure portalında görmek ve Azure kaynakları izlemek için tek bir kaynak veri görünümleri sağlar.
 
    >**Başlama**: [Azure İzleyicisi ile çalışmaya başlama](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>DevOps tümleştirmesi

VM'ler sağlama veya web uygulamalarınızı sürekli tümleştirme ile yayımlama olup Azure en popüler DevOps araçları ile tümleştirir. Jenkins, GitHub, Puppet, Chef, TeamCity, Ansible, VSTS ve diğerleri gibi araçlar desteği sayesinde, zaten varsa ve varolan deneyiminizi en üst düzeye araçları ile çalışabilirsiniz.

>**Şimdi deneyin:** [birkaç DevOps tümleştirmeler deneyin](https://azure.microsoft.com/try/devops/).

>**Başlama**: bir uygulama hizmeti uygulaması için DevOps seçeneklerini görmek için bkz: [Azure App Service için sürekli dağıtım](../../app-service/app-service-continuous-deployment.md).


## <a name="azure-regions"></a>Azure bölgeleri

Azure dünyadaki birçok bölgelerdeki genel olarak kullanılabilir olan bir genel bulut platformudur. Bir hizmet, uygulama veya azure'da VM sağladığınızda, uygulamanızın çalıştığı veya verilerinizin depolandığı belirli bir veri merkezini temsil eden bir bölge seçmek için istenir. Bu bölgeler üzerinde yayımlanan belirli konumlara karşılık [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfası.

### <a name="choose-the-best-region-for-your-application-and-data"></a>Uygulama ve veri için iyi bir bölge seçin

Azure kullanmanın avantajları dünyanın çeşitli veri merkezleri, uygulamaları dağıtabileceğiniz biridir. Seçtiğiniz bölge, uygulamanızın performansını etkileyebilir. Örneğin, ağ istek gecikme süresini azaltmak için müşterilerinize en yakın olan bir bölge seçmek iyidir. Bazı ülkelerde uygulamanızı dağıtmak için yasal gereksinimleri karşılamak üzere bölgenizi seçmek isteyebilirsiniz. Aynı veri merkezinde veya bir veri merkezinde uygulamanızı barındıran veri merkezinde mümkün olduğunca uygulama verilerini depolamak için her zaman en iyi bir uygulamadır.

### <a name="multi-region-apps"></a>Bölgeli uygulamalar

Olası rağmen Internet hatası veya doğal afet gibi bir olay nedeniyle çevrimdışına veri merkezinin tamamı için mümkün değil. En yüksek kullanılabilirlik sağlamak için önemli iş uygulamalarını barındırmasını birden fazla veri merkezinde en iyi bir uygulamadır. Birden çok bölgeye kullanarak da genel kullanıcılar için gecikme süresini azaltmak ve esneklik için başka fırsatları uygulamaları güncelleştirirken sağlayın.

Sanal makine ve uygulama hizmetleri gibi bazı hizmetler kullanmak [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) yüksek kullanılabilirlik Kurumsal uygulamaları desteklemek için bölgeler arasında yük devretme ile bölgeli desteğini etkinleştirmek için. Bir örnek için bkz: [Azure referans mimarisi: Web uygulaması ile yüksek kullanılabilirlik](../../guidance/guidance-web-apps-multi-region.md).

>**Ne zaman kullanılacağı**: yük devretme ve çoğaltma fayda enterprise ve yüksek kullanılabilirlik uygulamaları olduğunda.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Benim uygulamaları ve projeleri nasıl yönetebilirim?

Azure deneyimler, Azure kaynaklarını, uygulamaları ve projeleri oluşturmak ve yönetmek zengin bir dizi sağlar — hem program aracılığıyla ve [Azure portal](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>PowerShell ve komut satırı arabirimi

Azure Bash, Terminal, komut istemini veya tercih, komut satırı aracını kullanarak komut satırından uygulamaları ve hizmetleri yönetmek için iki yöntem sunar. Genellikle, Azure portalında olduğu gibi komut satırından aynı görevleri gerçekleştirebilmeniz için — oluşturma ve sanal makineler, sanal ağlar, web uygulamaları ve diğer hizmetleri yapılandırma gibi.

-   [Azure komut satırı arabirimi (CLI)](../../xplat-cli-install.md): bir Azure aboneliğine bağlanma ve komut satırından Azure kaynaklarına karşı çeşitli görevleri program olanak tanır.

-   [Azure PowerShell](../../powershell-install-configure.md): Windows PowerShell kullanarak Azure kaynaklarını yönetmenizi sağlayan cmdlet'leri ile bir modül kümesini sağlar.

### <a name="azure-portal"></a>Azure portalına

Azure portalı, Azure kaynaklarını ve Hizmetleri Kaldır oluşturmak ve yönetmek için kullanabileceğiniz bir web tabanlı bir uygulamadır. Azure portalı konumundadır <https://portal.azure.com>. Özelleştirilebilir bir pano, Azure kaynaklarınızı yönetmek için Araçlar içerir ve erişim için Abonelik ayarları ve fatura bilgileri. Daha fazla bilgi için bkz: [Azure portalına genel bakış](../../azure-portal-overview.md).

### <a name="rest-apis"></a>REST API'leri

Azure Azure portalı kullanıcı arabirimini destekler API'lerini bir dizi yerleşik olarak bulunur. Bu REST API'leri çoğu program aracılığıyla sağlamak ve Internet özellikli herhangi bir aygıttan Azure kaynakları ve uygulamaları yönetmenize izin vermek için de desteklenir. REST API belgeleri tam kümesi için bkz: [Azure REST SDK'sı başvurusu](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>API'ler

REST API'leri ek olarak, çoğu Azure hizmeti de program aracılığıyla kaynakları uygulamalarınızdan aşağıdaki geliştirme platformlar için SDK'lar dahil olmak üzere, platforma özgü Azure SDK kullanarak yönetmenizi sağlar:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](https://docs.microsoft.com/javascript/azure)
-   [Java](https://docs.microsoft.com/java/azure)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](https://docs.microsoft.com/python/azure)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)
-   [Go](https://docs.microsoft.com/go/azure)

Gibi hizmetleri [Mobile Apps](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) ve [Azure Media Services](../../media-services/media-services-dotnet-how-to-use.md) istemci-tarafı SDK'ları olanak sağlayan web ve mobil istemci uygulamaları hizmetlere erişim.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Büyük olasılıkla Azure uygulamanızı çalıştıran her biri aynı yaşam döngüsü izleyin ve, mantıksal bir birim olarak düşünülebilir birden çok Azure Hizmetleri ile çalışmayı içerir. Örneğin, bir web uygulaması, Web uygulamaları, SQL veritabanı, depolama, Azure Redis önbelleği ve Azure içerik teslim ağı Hizmetleri kullanabilirsiniz. [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) bir grup olarak, uygulamanızdaki kaynaklarla çalışma sağlar. Dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle tüm kaynakları silin.

Mantıksal gruplandırma ve ilgili kaynakları yönetme ek olarak, Azure Resource Manager dağıtımı ve yapılandırılmasıyla ilgili kaynakların özelleştirmenize olanak sağlayan dağıtım özellikleri içerir. Örneğin, Kaynak Yöneticisi'ni kullanarak dağıtın ve birden çok sanal makine, yük dengeleyici ve bir Azure SQL veritabanı tek bir birim olarak oluşan bir uygulamayı yapılandırın.

JSON biçimli bir belge bir Azure Resource Manager şablonu kullanarak bu dağıtımlar geliştirin. Şablonları bir dağıtım tanımlamanıza ve komut dosyalarının yerine bildirim temelli şablonlar kullanarak uygulamalarınızı yönetmenize olanak tanır. Şablonlarınızı test, hazırlık ve üretim gibi farklı ortamları için çalışabilir. Örneğin, şablonları kullanarak tek bir tıklatmayla Azure Hizmetleri kümesi için kod depodaki dağıtan bir GitHub deposuna için bir düğme ekleyebilirsiniz.

>**Ne zaman kullanılacağı**: program aracılığıyla REST API'leri, Azure CLI ve Azure PowerShell kullanarak yönetebileceğiniz uygulamanız için şablon tabanlı bir dağıtım istediğinizde kullanın Resource Manager şablonları.

>**Başlama**: şablonları kullanmaya başlamak için bkz: [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Anlama hesapları, abonelikleri ve faturalama

Geliştiriciler, biz sağ koda daha yakından inceleyin ve çalışan bizim uygulamaların yapma ile mümkün olduğunca hızlı başlayacağınızı deneyin ister. Kesinlikle Azure'da gibi kolayca çalışmaya başlamak için öneririz istiyoruz. Kolay, Azure teklifleri olmasına yardımcı olmak için bir [ücretsiz deneme sürümü](https://azure.microsoft.com/free/). Gibi hatta sahip bir "ücretsiz deneyin" işlevsellik bazı hizmetler [Azure App Service](https://tryappservice.azure.com/), hangi gerektirmez, hatta hesap oluşturmak. Kodlama ve Azure, uygulamanıza dağıtma ıntune'un olarak eğlenceli da Azure kullanıcı hesapları, abonelikleri ve faturalandırma açısından nasıl çalıştığını anlamak için biraz zaman önemlidir.

### <a name="what-is-an-azure-account"></a>Bir Azure hesabı nedir?

Oluşturma veya bir Azure aboneliği ile çalışmak için bir Azure hesabınızın olması gerekir. Bir Azure hesap yalnızca bir kimlik Azure AD içinde veya Azure AD tarafından güvenilen bir dizinde bir iş veya Okul kuruluş gibi. Bu tür bir kuruluşa ait değil, Microsoft, Azure AD tarafından güvenilen Account kullanarak bir abonelik her zaman oluşturabilirsiniz. Azure AD ile şirket içi Windows Server Active Directory tümleştirme hakkında daha fazla bilgi için bkz: [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../../active-directory/active-directory-aadconnect.md).

Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](../../active-directory/active-directory-how-subscriptions-associated-directory.md).

Tek tek Azure tanımlama yanı sıra olarak da bilinir kimlikleri, hesap *kullanıcılar*, ayrıca tanımlayabilirsiniz *grupları* Azure AD'de. Kullanıcı grubu oluşturmak, rol tabanlı erişim denetimi (RBAC) kullanarak bir abonelikte kaynaklarına erişimi yönetmek için iyi bir yoludur. Grupları oluşturma konusunda bilgi almak için bkz: [Azure Active Directory Önizleme'de bir grup oluşturmak](../../active-directory/active-directory-groups-create-azure-portal.md). Ayrıca oluşturabilir ve gruplarına göre yönetme [PowerShell kullanarak](../../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Aboneliklerinizi yönetme

Bir aboneliği bir Azure hesabı için bağlı olan bir mantıksal Azure Hizmetleri birimidir. Her ilişkili hesap bir Abonelikteki bir rolü var. Faturalama Azure Hizmetleri için bir abonelik başına temelinde gerçekleştirilir. Kullanılabilir abonelik tekliflerini türüne göre bir listesi için bkz: [Microsoft Azure Teklif Ayrıntıları](https://azure.microsoft.com/support/legal/offer-details/).

#### <a name="administrator-roles"></a>Yönetici rolleri

Bir Azure aboneliği olan herhangi bir zamanda atayabilirsiniz birden fazla hesap Yönetici rolü vardır.

-   **Hesap Yöneticisi**: Bu rolün aboneliği üzerinde tam denetime sahip ve için fatura sorumludur hesabıdır.

-   **Hizmet Yöneticisi**: Bu rolün tüm hizmetleri üzerinde denetim abonelikte sahiptir. Varsayılan olarak, bu aynı hesabı yönetici olarak hesabıdır.

-   **Ortak yönetici**: Azure dizinine abonelik ilişkisini değiştiremezsiniz dışında bu rolü aynı Hizmet Yöneticisi olarak erişebilir.

Yönetici rolleri hakkında daha fazla bilgi için bkz: [eklemek veya Azure yönetici rollerini değiştirmek nasıl](../../billing/billing-add-change-azure-subscription-administrator.md#add-an-admin-for-a-subscription).

#### <a name="resource-groups"></a>Kaynak grupları

Yeni Azure hizmetleri sağlamak, belirli bir aboneliğe bunu. Kaynaklar olarak da bilinen ayrı ayrı Azure hizmetlerini bir kaynak grubu bağlamında oluşturulur. Kaynak grupları, dağıtmak ve uygulamanızın kaynakları yönetmek kolaylaştırır. Bir kaynak grubu için bir birim olarak çalışmak istediğiniz uygulamanız tüm kaynakları içermelidir. Kaynak grupları arasında ve hatta farklı Aboneliklerde kaynakları taşıyabilirsiniz. Kaynakları taşıma hakkında bilgi edinmek için [yeni kaynak grubu veya abonelik kaynaklarını taşıma](../../resource-group-move-resources.md).

Azure kaynak Gezgini, aboneliğinizde önceden oluşturduğunuz kaynak görselleştirmek için harika bir araçtır. Daha fazla bilgi için bkz: [kaynakları görüntülemek ve değiştirmek için kullanım Azure kaynak Gezgini](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-to-resources"></a>Kaynaklara erişim izni ver

Azure kaynakları için erişim izni verdiğinizde, her zaman belirli bir görevi gerçekleştirmek için gerekli olan en az ayrıcalık ile kullanıcılara sağlamak için bir en iyi uygulamadır.

-   **Rol tabanlı erişim denetimi (RBAC)**: içinde Azure, belirtilen bir kapsamda (sorumluları) kullanıcı hesaplarına erişim vermek: Abonelik, kaynak grubu veya bireysel kaynaklar. RBAC, bir kaynak kümesi bir kaynak grubuna dağıtmak ve belirli bir kullanıcı veya grup için izinler sağlar. Ayrıca, hedef kaynak grubuna ait kaynaklara erişimi sınırlamak sağlar. Ayrıca, bir sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni verebilir. Erişim vermek için kullanıcı, Grup veya hizmet sorumlusu bir rol atayın. Çok sayıda önceden tanımlanmış roller yoktur ve özel rollerinizi de tanımlayabilirsiniz.

    >**Ne zaman kullanılacağı**: gerektiğinde ayrıntılı erişim yönetimini kullanıcılar ve gruplar için.

    >**Başlama**: daha fazla bilgi için bkz: [Azure portalında erişim yönetimini kullanmaya başlama](../../role-based-access-control/overview.md).

-   **Hizmet sorumlusu nesneleri**: kullanıcı ilkeleri ve gruplara erişim sağlamasının yanı sıra, bir hizmet sorumlusu aynı erişim verebilirsiniz.

    > **Ne zaman kullanılacağı**: ne zaman, program aracılığıyla Azure kaynaklarını yönetmek veya uygulamalar için erişim verme. Daha fazla bilgi için bkz: [oluşturma Active Directory uygulaması ve hizmet sorumlusu](../../resource-group-create-service-principal-portal.md).

#### <a name="tags"></a>Etiketler

Azure Resource Manager kaynakların için özel etiketler atamanıza olanak tanır. Faturalama veya izleme için kaynakları düzenleyin gerektiğinde anahtar-değer çiftleri olan etiketleri yararlı olabilir. Etiketleri birden çok kaynak grubu içinde kaynakları izlemek için bir yol sağlar. Portal, Azure Resource Manager şablonu veya program aracılığıyla, REST API, Azure CLI veya PowerShell kullanarak etiketlerinde atayabilirsiniz. Her kaynak için birden çok etiket atayabilirsiniz. Daha fazla bilgi için bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](../../resource-group-using-tags.md).

### <a name="billing"></a>Faturalandırma

Şirket içi bulutta barındırılan hizmetleri için bilgi işlem taşımak içinde izleme ve hizmet kullanımı ve ilgili maliyetleri tahmin etme önemli sorunları var. Yeni kaynaklar aylık olarak çalıştırmak için maliyetini tahmin etmek önemlidir. Faturalama geçerli harcama göre belirli bir ay boyunca nasıl göründüğünü proje olması gerekir.

#### <a name="get-resource-usage-data"></a>Kaynak kullanım verilerini alma

Azure faturalama REST kaynak tüketimini ve meta veri bilgileri Azure abonelikleri için erişim verin API kümesi sağlar. Bu fatura API'leri daha iyi tahmin etmek ve Azure maliyetlerini yönetme olanağı verir. İzleme ve saatlik artışlarda harcadığı miktarı analiz, harcama uyarılar oluşturabilir ve geçerli kullanım eğilimler temelinde gelecek fatura tahmin etmek.

>**Başlama**: faturalama API'ları kullanma hakkında daha fazla bilgi edinmek için [Azure faturalama kullanımını ve RateCard API'leri genel bakış](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Gelecekteki maliyetleri tahmin etme

Önceden maliyetlerini tahmin etmek için zor olsa da, Azure sahip bir [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) dağıtılan kaynakları maliyetini tahmin ederken kullanabilirsiniz. Geçerli tüketime dayanarak gelecekteki maliyetlerini tahmin etmek için faturalama dikey penceresinde portal ve faturalandırma REST API de kullanabilirsiniz.

>**Başlama**: bkz [Azure faturalama kullanımını ve RateCard API'leri genel bakış](../../billing-usage-rate-card-overview.md).

#### <a name="set-up-billing-alerts"></a>Fatura uyarılarını ayarlama

Uygulamanızı veya Azure ile ilgili çözüm dağıttıktan sonra uyarıda tanımlanan harcama sınırı yaklaşımını olduğunda e-posta Gönder uyarılar oluşturabilirsiniz.

>**Başlama**: daha fazla bilgi için bkz: [uyarılar, Microsoft Azure abonelikleri için ödeme yukarı ayarlamak](../../billing-set-up-alerts.md).
