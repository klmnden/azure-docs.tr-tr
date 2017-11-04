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
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f8510bb6b412e9af8aad30ba32bc74206c22042f
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Web Apps’e Genel Bakış

*Azure App Service Web Apps* (veya yalnızca Web uygulamaları) geri mobil sona erer ve web uygulamalarını, REST API'leri barındırmak için bir hizmettir. Sık kullanılan dilinizde geliştirmek, .NET, .NET Core, Java, Ruby, Node.js, PHP veya Python olabilir. Çalıştırın ve uygulamaları Windows veya Linux VM'ler kolaylığı ölçeklendirme (bkz [Linux uygulama hizmeti](containers/app-service-linux-intro.md)). 

Web uygulamaları yalnızca güvenlik, Yük Dengeleme, otomatik ölçeklendirmeyi gibi uygulamanızı Microsoft Azure gücünü ekler ve otomatik yönetim. VSTS, GitHub, Docker hub'a ve diğer kaynakları, paket Yönetimi sürekli dağıtım gibi kendi DevOps özelliklerinden hazırlama ortamları, özel etki alanı ve SSL sertifikalarını da alabilir. 

Uygulama hizmeti ile kullandığınız Azure işlem kaynakları için ücret ödersiniz. Kullandığınız işlem kaynakları belirlenir _uygulama hizmeti planı_ , Web uygulamalarının çalıştırılması. Daha fazla bilgi için bkz: [App Service planlarına Azure Web uygulamalarında](azure-web-sites-web-hosting-plans-in-depth-overview.md).

Aşağıdaki 5 dakikalık videoda Azure App Service Web Apps hakkında genel bilgi verilmektedir.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Web Apps neden kullanılır?
App Service Web Apps önemli özelliklerinden bazıları şunlardır:

* **Birden çok dil ve çerçeve** -Web Apps, ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP veya Python için birinci sınıf destek vardır. De çalıştırabilirsiniz [PowerShell ve diğer betikleri veya yürütülebilirleri](web-sites-create-web-jobs.md) arka plan gibi hizmetleri.
* **DevOps iyileştirmesi** -ayarlanan [sürekli tümleştirme ve dağıtım](app-service-continuous-deployment.md) Visual Studio Team Services, GitHub, BitBucket, Docker hub'a veya Azure kapsayıcı hizmeti. [Test ve hazırlık ortamları](web-sites-staged-publishing.md) aracılığıyla güncelleştirmeleri yükseltin. Web Apps uygulamalarınızda kullanarak yönetmek [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya [platformlar arası komut satırı arabirimi (CLI)](/cli/azure/install-azure-cli).
* **Yüksek kullanılabilirlik ile küresel ölçeklendirme** - El ile veya otomatik olarak ölçek [artırabilir](web-sites-scale.md) veya [genişletebilirsiniz](../monitoring-and-diagnostics/insights-how-to-scale.md). Uygulamalarınızı Microsoft'un küresel veri merkezi altyapılarının herhangi bir yerinde barındırın. App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/)’sı yüksek kullanılabilirlik taahhüt eder.
* **SaaS platformları ve şirket içi veri bağlantıları** -50'den fazla seçin [Bağlayıcılar](../connectors/apis-list.md) Kurumsal sistemler (SAP gibi) SaaS Hizmetleri (Salesforce gibi) ve internet Hizmetleri (Facebook gibi). [Karma Bağlantılar](../biztalk-services/integration-hybrid-connection-overview.md)’ı ve [Azure Sanal Ağlar](web-sites-integrate-with-vnet.md)’ı kullanarak şirket içi verilere erişin.
* **Güvenlik ve uyumluluk** - App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/TrustCenter/). İle kullanıcıların kimliklerini [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) veya sosyal oturum açma ile ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md), ve [ Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Oluşturma [IP adresi sınırlamaları](app-service-ip-restrictions.md) ve [hizmet kimlikleri yönetmek](app-service-managed-service-identity.md).
* **Uygulama Şablonları** -uygulama şablonlarında kapsamlı bir listesini arasından [Azure Marketi](https://azure.microsoft.com/marketplace/), WordPress, Joomla ve Drupal gibi.
* **Visual Studio tümleştirmesi** -Visual Studio’daki ayrılmış araçlar oluşturma, dağıtma ve hata ayıklama işlemlerini kolaylaştırır.
* **API ve mobil özelliklerini** -Web Apps RESTful API'si senaryoları için anahtar teslim CORS desteği sağlar ve kimlik doğrulama, çevrimdışı veri eşitlemeye, anında iletme bildirimleri ve daha fazla etkinleştirerek mobil uygulama senaryoları basitleştirir.
* **Sunucusuz kod** - bir kod parçacığında çalıştırmak veya açıkça sağlamak veya altyapısını yönetmek zorunda kalmadan isteğe bağlı komut dosyası ve yalnızca işlem süresi için kodunuzu gerçekte kullanır ödeme (bkz [Azure işlevleri](/azure/azure-functions/)).

Azure, App Service’te Web Apps’in yanı sıra web siteleri ve web uygulamaları barındırmak için kullanılabilen başka hizmetler de sunar. Çoğu senaryo için Web Apps en iyi seçenektir.  Mikro hizmet mimarisi için göz önünde bulundurun [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Kodunuzun çalıştığı VM'ler hakkında daha fazla denetime ihtiyacınız varsa, göz önünde bulundurun [Azure sanal makineleri](https://azure.microsoft.com/documentation/services/virtual-machines/). Bu Azure hizmetleri arasında seçim yapma hakkında daha fazla bilgi için bkz. [Azure App Service, Virtual Machines, Service Fabric ve Cloud Services karşılaştırması](choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Sonraki adımlar

İlk web uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

