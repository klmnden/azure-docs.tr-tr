---
title: "Web uygulamaları, mobil uygulamalar ve API uygulamaları için Azure Uygulama Hizmeti | Microsoft Belgeleri"
description: "Azure App Service’in web uygulamalarını ve mobil uygulamaları geliştirmenize, dağıtmanıza ve yönetmenize nasıl yardımcı olduğunu öğrenin."
keywords: "app service, azure app service, app service maliyeti, ölçek, ölçeklenebilir, uygulama dağıtımı, azure uygulama dağıtımı, paas, hizmet olarak platform, websitesi, web sitesi, azure mobil"
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
translationtype: Human Translation
ms.sourcegitcommit: 40dd75832302d7d88e852e2ea93821750675607e
ms.openlocfilehash: 4deb60c25bf13d1f31b58f002a7edea0672eca25


---
# <a name="what-is-azure-app-service"></a>Azure App Service nedir?
*App Service*, Microsoft Azure’un bir [hizmet olarak platform](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) teklifidir. Herhangi bir platform veya cihaz için web ve mobil uygulamalar oluşturun. Uygulamalarınızı SaaS çözümleriyle tümleştirin, şirket içi uygulamalara bağlanın ve iş süreçlerinizi otomatikleştirin. Azure, uygulamalarınızı tam yönetilen sanal makineler (VM) üzerinde, seçtiğiniz paylaşılan VM kaynakları veya özel VM’ler ile çalıştırır.

App Service, Azure Web Siteleri ve Azure Mobile Services halinde daha önce ayrı olarak sunulan web ve mobil özelliklerini içerir. Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir tümleşik hizmet olarak App Service; web siteleri, mobil uygulama arka uçları, RESTful API’leri ve iş süreçleri gibi çeşitli bileşenleri tek bir çözümde oluşturmanıza olanak sağlar.

Aşağıdaki 4 dakikalık video, App Service’in daha önceki Azure teklifleriyle nasıl ilişkili olduğu ve yenilikleri hakkında kısa bir açıklama sağlar.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>App Service nedir?
App Service’in temel özelliklerinden bazıları şunlardır:

* **Birden çok dil ve çerçeve** - App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek sunar. Ayrıca, App Service sanal makineleri üzerinde [Windows PowerShell ve diğer betikleri veya yürütülebilir dosyaları](../app-service-web/web-sites-create-web-jobs.md) çalıştırabilirsiniz.
* **DevOps iyileştirmesi** - Visual Studio Team Services, GitHub veya BitBucket ile [sürekli tümleştirme ve dağıtım](../app-service-web/app-service-continuous-deployment.md) ayarlayın. [Test ve hazırlık ortamları](../app-service-web/web-sites-staged-publishing.md) aracılığıyla güncelleştirmeleri yükseltin. [A/B testi](../app-service-web/app-service-web-test-in-production-get-start.md) gerçekleştirin. [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya [platformlar arası komut satırı arabirimi (CLI)](../xplat-cli-install.md) kullanarak uygulamalarınızı App Service’de yönetin.
* **Yüksek kullanılabilirlik ile küresel ölçeklendirme** - El ile veya otomatik olarak ölçek [artırabilir](../app-service-web/web-sites-scale.md) veya [genişletebilirsiniz](../monitoring-and-diagnostics/insights-how-to-scale.md). Uygulamalarınızı Microsoft'un küresel veri merkezi altyapılarının herhangi bir yerinde barındırın. App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/)’sı yüksek kullanılabilirlik taahhüt eder.
* **SaaS platformları ve şirket için veri bağlantıları** - Kurumsal sistemler (SAP, Siebel ve Oracle gibi), SaaS hizmetleri (Salesforce ve Office 365 gibi) ve İnternet hizmetleri (Facebook ve Twitter gibi) için 50’den fazla [bağlayıcı](../connectors/apis-list.md) arasından seçim yapın. [Karma Bağlantılar](../biztalk-services/integration-hybrid-connection-overview.md)’ı ve [Azure Sanal Ağlar](../app-service-web/web-sites-integrate-with-vnet.md)’ı kullanarak şirket içi verilere erişin.
* **Güvenlik ve uyumluluk** - App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/TrustCenter/).
* **Uygulama şablonları** - WordPress, Joomla ve Drupal gibi popüler açık kaynaklı yazılımları bir sihirbaz ile yüklemenize olanak tanıyan [Azure Marketi](https://azure.microsoft.com/marketplace/)’ndeki kapsamlı şablon listesinden seçiminizi yapın.
* **Visual Studio tümleştirmesi** -Visual Studio’daki ayrılmış araçlar oluşturma, dağıtma ve hata ayıklama işlemlerini kolaylaştırır.

## <a name="app-types-in-app-service"></a>App Service’deki uygulama türleri
App Service, her birinin belirli bir iş yükü barındırması amaçlanan birkaç *uygulama türü* sunar:

* [**Web Apps**](../app-service-web/app-service-web-overview.md) - Web sitelerini ve web uygulamalarını barındırmaya yöneliktir.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) Mobil uygulama arka uçlarını barındırmaya yöneliktir.
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - RESTful API'lerini barındırmaya yöneliktir.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) - İş süreçlerini otomatikleştirmeye ve sistemler ile verileri kod yazmadan bulut üzerinde tümleştirmeye yöneliktir.

Buradaki *uygulama* sözcüğü bir iş yükünü çalıştırmaya ayrılmış barındırma kaynaklarını ifade eder. “Web uygulaması” örnek olarak alındığında, web uygulamasını tarayıcıya işlev kazandıran işlem kaynaklar ve uygulama kodu bütünü olarak düşünmeye büyük olasılıkla alışkınsınız. Ancak, App Service’deki *web uygulamaları*, uygulama kodunuzun barındırılması için Azure tarafından sağlanan işlem kaynaklarıdır. 

Uygulamanız farklı türlerde birden fazla App Service uygulamasından oluşabilir. Örneğin, uygulamanız bir web ön ucu ve bir RESTful API arka ucundan oluşuyorsa şunları yapabilirsiniz:

- İkisini de (ön uç ve api) tek bir web uygulamasına dağıtma  
- Ön uç kodunuzu bir web uygulamasına, arka uç kodunuzu bir API uygulamasına dağıtma. 



## <a name="app-service-plans"></a>App Service planları
[App Service planları](azure-web-sites-web-hosting-plans-in-depth-overview.md), uygulamalarınızı barındırmak için kullanılan fiziksel kaynak bileşimini temsil eder.

App Service planları şunları tanımlar:

- **Bölge** (Batı ABD, Doğu ABD vb.)
- **Ölçek sayısı** (bir, iki, üç örnek, vb.)
- **Örnek boyutu** (Küçük, Orta, Büyük)
- **SKU** (Ücretsiz, Paylaşılan, Temel, Standart, Premium)

Bir **App Service planına** atanan tüm uygulamalar plan tarafından tanımlanan kaynakları paylaşarak birden çok uygulamayı barındırırken maliyetten tasarruf etmenize imkan sağlar.

Bu sırada **App Service planınızı** **Ücretsiz** ve **Paylaşılan** SKU’larından **Temel**, **Standart** ve **Premium** SKU’larına ölçeklendirerek daha fazla kaynak ve özelliğe erişebilirsiniz. App Service Planınız **Temel** veya üzeri olarak ayarlandığında, VM’lerin **boyutunu** ve ölçek sayısını da denetleyebilirsiniz.

App Service planının **SKU** ve **Ölçek** değeri, planda barındırılan uygulama sayısını değil planın maliyetini belirler. 

Daha fazla ölçeklenebilirlik ve ağ yalıtımını gerekiyorsa uygulamalarınızı bir [App Service Ortamı](../app-service-web/app-service-app-service-environment-intro.md) içinde çalıştırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma
App Service’in maliyeti hakkında bilgi için bkz. [App Service Fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>App Service'i Deneyin
Herhangi bir kredi kartına gerek olmadan, taahhüt vermeden veya sorun yaşamadan [örnek bir web uygulaması, mobil uygulama veya mantıksal uygulama oluşturun](https://azure.microsoft.com/try/app-service/) ve uygulamanızı&1; saat boyunca deneyin.

Ya da bir [ücretsiz Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) açıp, kullanmaya başlama öğreticilerimizden birini deneyin:

* [Öğretici: Web uygulaması oluşturma](../app-service-web/app-service-web-get-started.md)
* [Öğretici: Mobil uygulama oluşturma](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Öğretici: API uygulaması oluşturma](../app-service-api/app-service-api-dotnet-get-started.md)
* [Öğretici: Mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Feb17_HO1-->


