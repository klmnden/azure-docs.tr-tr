---
title: "Web Apps’e Genel Bakış | Microsoft Belgeleri"
description: "Azure App Service’in web uygulamaları geliştirmenize ve barındırmanıza nasıl yardımcı olabileceğini öğrenin."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 18959934c53e2e1c719cc627ffa286acbdcaa967


---
# <a name="web-apps-overview"></a>Web Apps’e Genel Bakış
*App Service Web Apps* web sitelerini ve web uygulamalarını barındırmak için en uygun hale getirilmiş tam olarak yönetilen bir işlem platformudur. Microsoft Azure tarafından sunulan bu [hizmet olarak platform](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) teklifi, Azure sizin yerinize uygulamalarınızı çalıştıracak ve ölçeklendirecek altyapınızla ilgilenirken, sizin işlerinize odaklanmanızı sağlar.

Aşağıdaki 5 dakikalık videoda Azure App Service Web Apps hakkında genel bilgi verilmektedir.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

## <a name="what-is-a-web-app-in-app-service"></a>App Service’teki bir web uygulaması nedir?
App Service’teki bir *web uygulaması* bir web sitesini veya web uygulamasını barındırmak için Azure tarafından sağlanan işlem kaynaklarıdır.  

İşlem kaynakları, seçtiğiniz fiyatlandırma katmanına bağlı olarak paylaşılan veya ayrılmış sanal makineler (VM'ler) üzerinde bulunabilir. Uygulama kodlarınız, diğer müşterilerden yalıtılmış yönetilen bir sanal makinede çalıştırılır.

Kodlarınız, ASP.NET, Node.js, Java, PHP veya Python [Azure App Service](../app-service/app-service-value-prop-what-is.md) tarafından desteklenen herhangi bir dilde veya çerçevede olabilir. Ayrıca, bir web uygulamasında [PowerShell ve diğer betik dosyası oluşturma dillerini](web-sites-create-web-jobs.md#acceptablefiles) kullanan betikler çalıştırabilirsiniz.

Web Apps’i kullanabileceğiniz tipik uygulama senaryo örnekleri için bkz. [Azure App Service, Virtual Machines, Service Fabric ve Cloud Services karşılaştırmasının](choose-web-site-cloud-service-vm.md#scenarios) [Web uygulaması senaryoları](https://azure.microsoft.com/documentation/scenarios/web-app/) ve **Senaryolar ve öneriler** bölümüne bakın.

## <a name="why-use-web-apps"></a>Web Apps neden kullanılır?
App Service’in Web Apps için geçerli olan temel özelliklerinden bazıları aşağıda sunulmuştur:

* **Birden çok dil ve çerçeve** - App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek sunar. Ayrıca, App Service sanal makineleri üzerinde [PowerShell ve diğer betikleri veya yürütülebilirleri](web-sites-create-web-jobs.md) çalıştırabilirsiniz.
* **DevOps iyileştirmesi** - Visual Studio Team Services, GitHub veya BitBucket ile [sürekli tümleştirme ve dağıtım](app-service-continuous-deployment.md) ayarlayın. [Test ve hazırlık ortamları](web-sites-staged-publishing.md) aracılığıyla güncelleştirmeleri yükseltin. [A/B testi](app-service-web-test-in-production-get-start.md) gerçekleştirin. [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya [platformlar arası komut satırı arabirimi (CLI)](../xplat-cli-install.md) kullanarak uygulamalarınızı App Service’de yönetin.
* **Yüksek kullanılabilirlik ile küresel ölçeklendirme** - El ile veya otomatik olarak ölçek [artırabilir](web-sites-scale.md) veya [genişletebilirsiniz](../monitoring-and-diagnostics/insights-how-to-scale.md). Uygulamalarınızı Microsoft'un küresel veri merkezi altyapılarının herhangi bir yerinde barındırın. App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/)’sı yüksek kullanılabilirlik taahhüt eder.
* **SaaS platformları ve şirket için veri bağlantıları** - Kurumsal sistemler (SAP, Siebel ve Oracle gibi), SaaS hizmetleri (Salesforce ve Office 365 gibi) ve İnternet hizmetleri (Facebook ve Twitter gibi) için 50’den fazla [bağlayıcı](../connectors/apis-list.md) arasından seçim yapın. [Karma Bağlantılar](../biztalk-services/integration-hybrid-connection-overview.md)’ı ve [Azure Sanal Ağlar](web-sites-integrate-with-vnet.md)’ı kullanarak şirket içi verilere erişin.
* **Güvenlik ve uyumluluk** - App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/TrustCenter/).
* **Uygulama şablonları** - WordPress, Joomla ve Drupal gibi popüler açık kaynaklı yazılımları bir sihirbaz ile yüklemenize olanak tanıyan [Azure Marketi](https://azure.microsoft.com/marketplace/)’ndeki kapsamlı uygulama şablonu listesinden seçiminizi yapın.
* **Visual Studio tümleştirmesi** -Visual Studio’daki ayrılmış araçlar oluşturma, dağıtma ve hata ayıklama işlemlerini kolaylaştırır.

Ayrıca, web uygulamasında [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (CORS desteği gibi) ve [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (anında iletme bildirimleri) tarafından sunulan özelliklerden yararlanılabilir. App Service’teki uygulama türleri hakkında daha fazla bilgi için bkz. [Azure App Service’e genel bakış](../app-service/app-service-value-prop-what-is.md).

Azure, App Service’te Web Apps’in yanı sıra web siteleri ve web uygulamaları barındırmak için kullanılabilen başka hizmetler de sunar. Çoğu senaryo için Web Apps en iyi seçenektir.  Mikro hizmet mimarisi için [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)’i göz önünde bulundurun ve kodlarınızın çalıştığı sanal makineler üzerinde daha fazla denetim elde etmeniz gerekirse [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)’i düşünün. Bu Azure hizmetleri arasında seçim yapma hakkında daha fazla bilgi için bkz. [Azure App Service, Virtual Machines, Service Fabric ve Cloud Services karşılaştırması](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Başlarken
App Service’te basit kodları yeni bir web uygulamasına dağıtmaya başlamak için aşağıdaki açılan kutuda yer alan öğreticilerden birini izleyin. Ücretsiz Azure hesabınızın olması gerekir.

> [!div class="op_single_selector"]
> * [5 dakikada Azure’a ilk HTML sitenizi dağıtın](app-service-web-get-started-html-cli-nodejs.md)
> * [5 dakikada Azure’a ilk ASP.NET web uygulamanızı dağıtın](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [5 dakikada Azure’a ilk PHP web uygulamanızı dağıtın](app-service-web-get-started-php-cli-nodejs.md)
> * [5 dakikada Azure’a ilk Node.js web uygulamanızı dağıtın](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [5 dakikada Azure’a ilk Python web uygulamanızı dağıtın](app-service-web-get-started-python-cli-nodejs.md)
> * [5 dakikada Azure’a ilk Java web uygulamanızı dağıtın](app-service-web-get-started-java.md)
> 
> 

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](https://azure.microsoft.com/try/app-service/). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 



<!--HONumber=Feb17_HO2-->


