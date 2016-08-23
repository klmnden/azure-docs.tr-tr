<properties
    pageTitle="Web Apps’e Genel Bakış | Microsoft Azure"
    description="Azure App Service’in web uygulamaları geliştirmenize ve barındırmanıza nasıl yardımcı olabileceğini öğrenin."
    services="app-service\web"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="rachelap"/>

# Web Apps’e Genel Bakış

*App Service Web Apps* web sitelerini ve web uygulamalarını barındırmak için en uygun hale getirilmiş tam olarak yönetilen bir işlem platformudur. Microsoft Azure tarafından sunulan bu [hizmet olarak platform](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) teklifi, Azure sizin yerinize uygulamalarınızı çalıştıracak ve ölçeklendirecek altyapınızla ilgilenirken, sizin işlerinize odaklanmanızı sağlar.

Aşağıdaki 5 dakikalık videoda Azure App Service Web Apps hakkında genel bilgi verilmektedir.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

## App Service’teki bir web uygulaması nedir?

App Service’teki bir *web uygulaması* bir web sitesini veya web uygulamasını barındırmak için Azure tarafından sağlanan işlem kaynaklarıdır.  

İşlem kaynakları, seçtiğiniz fiyatlandırma katmanına bağlı olarak paylaşılan veya ayrılmış sanal makineler (VM'ler) üzerinde bulunabilir. Uygulama kodlarınız, diğer müşterilerden yalıtılmış yönetilen bir sanal makinede çalıştırılır.

Kodlarınız, ASP.NET, Node.js, Java, PHP veya Python [Azure App Service](../app-service/app-service-value-prop-what-is.md) tarafından desteklenen herhangi bir dilde veya çerçevede olabilir. Ayrıca, bir web uygulamasında [PowerShell ve diğer betik dosyası oluşturma dillerini](web-sites-create-web-jobs.md#acceptablefiles) kullanan betikler çalıştırabilirsiniz.

Web Apps’i kullanabileceğiniz tipik uygulama senaryo örnekleri için bkz. [Azure App Service, Virtual Machines, Service Fabric ve Cloud Services karşılaştırmasının](choose-web-site-cloud-service-vm.md#scenarios) [Web uygulaması senaryoları](https://azure.microsoft.com/documentation/scenarios/web-app/) ve **Senaryolar ve öneriler** bölümüne bakın.

## Web Apps neden kullanılır?

App Service’in Web Apps için geçerli olan temel özelliklerinden bazıları aşağıda sunulmuştur:

- **Birden çok dil ve çerçeve** - App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek sunar. Ayrıca, App Service sanal makineleri üzerinde [PowerShell ve diğer betikleri veya yürütülebilirleri](../app-service-web/web-sites-create-web-jobs.md) çalıştırabilirsiniz.

- **DevOps iyileştirmesi** - Visual Studio Team Services, GitHub veya BitBucket ile [sürekli tümleştirme ve dağıtım](../app-service-web/app-service-continuous-deployment.md) ayarlayın. [Test ve hazırlık ortamları](../app-service-web/web-sites-staged-publishing.md) aracılığıyla güncelleştirmeleri yükseltin. [A/B testi](../app-service-web/app-service-web-test-in-production-get-start.md) gerçekleştirin. [Azure PowerShell](../powershell-install-configure.md) veya [platformlar arası komut satırı arabirimi (CLI)](../xplat-cli-install.md) kullanarak uygulamalarınızı App Service’de yönetin.
 
- **Yüksek kullanılabilirlik ile küresel ölçeklendirme** - El ile veya otomatik olarak ölçek [artırabilir](../app-service-web/web-sites-scale.md) veya [genişletebilirsiniz](../azure-portal/insights-how-to-scale.md). Uygulamalarınızı Microsoft'un küresel veri merkezi altyapılarının herhangi bir yerinde barındırın. App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/)’sı yüksek kullanılabilirlik taahhüt eder.

- **SaaS platformları ve şirket için veri bağlantıları** - Kurumsal sistemler (SAP, Siebel ve Oracle gibi), SaaS hizmetleri (Salesforce ve Office 365 gibi) ve İnternet hizmetleri (Facebook ve Twitter gibi) için 50’den fazla [bağlayıcı](../connectors/apis-list.md) arasından seçim yapın. [Karma Bağlantılar](../biztalk-services/integration-hybrid-connection-overview.md)’ı ve [Azure Sanal Ağlar](../app-service-web/web-sites-integrate-with-vnet.md)’ı kullanarak şirket içi verilere erişin.

- **Güvenlik ve uyumluluk** - App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/TrustCenter/).

- **Uygulama şablonları** - WordPress, Joomla ve Drupal gibi popüler açık kaynaklı yazılımları bir sihirbaz ile yüklemenize olanak tanıyan [Azure Marketi](https://azure.microsoft.com/marketplace/)’ndeki kapsamlı uygulama şablonu listesinden seçiminizi yapın.

- **Visual Studio tümleştirmesi** -Visual Studio’daki ayrılmış araçlar oluşturma, dağıtma ve hata ayıklama işlemlerini kolaylaştırır.

Ayrıca, web uygulamasında [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (CORS desteği gibi) ve [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (anında iletme bildirimleri) tarafından sunulan özelliklerden yararlanılabilir. App Service’teki uygulama türleri hakkında daha fazla bilgi için bkz. [Azure App Service’e genel bakış](../app-service/app-service-value-prop-what-is.md).

Azure, App Service’te Web Apps’in yanı sıra web siteleri ve web uygulamaları barındırmak için kullanılabilen başka hizmetler de sunar. Çoğu senaryo için Web Apps en iyi seçenektir.  Mikro hizmet mimarisi için [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)’i göz önünde bulundurun ve kodlarınızın çalıştığı sanal makineler üzerinde daha fazla denetim elde etmeniz gerekirse [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)’i düşünün. Bu Azure hizmetleri arasında seçim yapma hakkında daha fazla bilgi için bkz. [Azure App Service, Virtual Machines, Service Fabric ve Cloud Services karşılaştırması](choose-web-site-cloud-service-vm.md).

## Başlarken

App Service’te basit kodları yeni web uygulamasına dağıtmaya başlamak için [Azure’da ilk web uygulamanızı 5 dakika içinde dağıtma](app-service-web-get-started.md) öğreticisini izleyin. Ücretsiz Azure hesabınızın olması gerekir.

Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.



<!--HONumber=Aug16_HO1-->


