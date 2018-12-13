---
title: App Service'e genel bakış - Azure | Microsoft Docs
description: Azure App Service’in web uygulamaları geliştirmenize ve barındırmanıza nasıl yardımcı olabileceğini öğrenin.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 17f90d54b31fbc46fc831fca0650417c50937fe6
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253380"
---
# <a name="app-service-overview"></a>App Service’a genel bakış

*Azure App Service* mobil arka uçlar ve web uygulamaları, REST API'leri barındırmak için bir hizmettir. .NET, .NET Core, Java, Ruby, Node.js, PHP veya Python dahil en sevdiğiniz dilde geliştirebilirsiniz. Windows tabanlı ortamlarda uygulamalar kolaylıkla çalıştırılıp ölçeklendirilebilir. Linux tabanlı ortamlar için bkz. [Linux’ta App Service](containers/app-service-linux-intro.md). 

Web Apps, uygulamanıza güvenlik, yük dengeleme, otomatik ölçeklendirme ve otomatik yönetim gibi özelliklerle Microsoft Azure’un gücünü katmakla kalmaz. Aynı zamanda Azure DevOps, GitHub, Docker Hub ve diğer kaynaklardan sürekli dağıtım, paket yönetimi, hazırlık ortamları, özel etki alanı ve SSL sertifikaları gibi DevOps özelliklerinden de yararlanabilirsiniz. 

App Service ile kullandığınız Azure işlem kaynakları için ödeme yaparsınız. Kullandığınız işlem kaynakları, Web Apps’inizi üzerinde çalıştırdığınız _App Service planına_ göre belirlenir. Daha fazla bilgi edinmek için bkz. [Azure Web Apps’teki App Service planları](azure-web-sites-web-hosting-plans-in-depth-overview.md).

## <a name="why-use-web-apps"></a>Web Apps neden kullanılır?

App Service Web Apps’in temel özelliklerinden bazıları aşağıda sunulmuştur:

* **Birden çok dil ve çerçeve** - Web Apps’te ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP veya Python için birinci sınıf destek sağlanır. Ayrıca, [PowerShell’i ve diğer betikleri ya da yürütülebilir hizmetleri](web-sites-create-web-jobs.md) arka plan hizmetleri olarak çalıştırabilirsiniz.
* **DevOps iyileştirmesi** - Azure DevOps, GitHub, BitBucket, Docker Hub veya Azure Container Registry ile [sürekli tümleştirme ve dağıtım](app-service-continuous-deployment.md) ayarlayın. [Test ve hazırlık ortamları](web-sites-staged-publishing.md) aracılığıyla güncelleştirmeleri yükseltin. [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya [platformlar arası komut satırı arabirimi (CLI)](/cli/azure/install-azure-cli) kullanarak uygulamalarınızı Web Apps’te yönetin.
* **Yüksek kullanılabilirlik ile küresel ölçeklendirme** - El ile veya otomatik olarak ölçek [artırabilir](web-sites-scale.md) veya [genişletebilirsiniz](../monitoring-and-diagnostics/insights-how-to-scale.md). Uygulamalarınızı Microsoft'un küresel veri merkezi altyapılarının herhangi bir yerinde barındırın. App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/)’sı yüksek kullanılabilirlik taahhüt eder.
* **SaaS platformları ve şirket için veri bağlantıları** - Kurumsal sistemler (SAP gibi), SaaS hizmetleri (Salesforce gibi) ve İnternet hizmetleri (Facebook gibi) için 50’den fazla [bağlayıcı](../connectors/apis-list.md) arasından seçim yapın. [Karma Bağlantılar](app-service-hybrid-connections.md)’ı ve [Azure Sanal Ağlar](web-sites-integrate-with-vnet.md)’ı kullanarak şirket içi verilere erişin.
* **Güvenlik ve uyumluluk** - App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/en-us/trustcenter). [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) veya sosyal oturum açma ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) ve [Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)) aracılığıyla kullanıcılarınızın kimliğini doğrulayın. [IP adresi kısıtlamaları](app-service-ip-restrictions.md) oluşturun ve [hizmet kimliklerini yönetin](app-service-managed-service-identity.md).
* **Uygulama şablonları** - [Azure Market](https://azure.microsoft.com/marketplace/)’teki WordPress, Joomla ve Drupal’i de içeren kapsamlı uygulama şablonu listesinden seçiminizi yapın.
* **Visual Studio tümleştirmesi** -Visual Studio’daki ayrılmış araçlar oluşturma, dağıtma ve hata ayıklama işlemlerini kolaylaştırır.
* **API ve mobil özellikler** - Web Apps, RESTful API senaryoları için kullanıma hazır CORS desteği sağlar ve kimlik doğrulama, çevrimdışı veri eşitleme, anında iletme bildirimleri ve daha fazlasını mümkün kılarak mobil uygulama senaryolarını basitleştirir.
* **Sunucusuz kod** - Açıkça altyapı sağlamanıza veya yönetmenize gerek kalmadan isteğe bağlı olarak bir kod parçacığı veya betik çalıştırın ve yalnızca kodunuzun gerçekte kullandığı işlem süresi (bkz. [Azure İşlevleri](/azure/azure-functions/)) için ücret ödeyin.

Azure, App Service’te Web Apps’in yanı sıra web siteleri ve web uygulamaları barındırmak için kullanılabilen başka hizmetler de sunar. Çoğu senaryo için Web Apps en iyi seçenektir.  Mikro hizmet mimarisi için [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)’i göz önünde bulundurun. Kodlarınızın çalıştığı sanal makineler üzerinde daha fazla denetime sahip olmanız gerekiyorsa [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)’i düşünün. Bu Azure hizmetleri arasında seçim yapma hakkında daha fazla bilgi için bkz. [Azure App Service, Virtual Machines, Service Fabric ve Cloud Services karşılaştırması](choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Sonraki adımlar

İlk web uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Ruby (Linux üzerinde)](containers/quickstart-ruby.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python (Linux üzerinde)](containers/quickstart-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)
