---
title: Web uygulamaları ve mobil uygulamalar için Azure App Service | Microsoft Docs
description: Azure App Service’in web uygulamalarını ve mobil uygulamaları geliştirmenize, dağıtmanıza ve yönetmenize nasıl yardımcı olduğunu öğrenin.
keywords: app service, azure app service, app service maliyeti, ölçek, ölçeklenebilir, uygulama dağıtımı, azure uygulama dağıtımı, paas, hizmet olarak platform
services: app-service
documentationcenter: ''
author: omarkmsft
manager: dwrede
editor: jimbe

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2016
ms.author: omark

---
# Azure App Service nedir?
*App Service*, Microsoft Azure’un bir [hizmet olarak platform](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) teklifidir. Herhangi bir platform veya cihaz için web ve mobil uygulamalar oluşturun. Uygulamalarınızı SaaS çözümleriyle tümleştirin, şirket içi uygulamalara bağlanın ve iş süreçlerinizi otomatikleştirin. Azure, uygulamalarınızı tam yönetilen sanal makineler (VM) üzerinde, seçtiğiniz paylaşılan VM kaynakları veya özel VM’ler ile çalıştırır. 

App Service, Azure Web Siteleri ve Azure Mobile Services halinde daha önce ayrı olarak sunulan web ve mobil özelliklerini içerir.  Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir tümleşik hizmet olarak App Service; web siteleri, mobil uygulama arka uçları, RESTful API’leri ve iş süreçleri gibi çeşitli bileşenleri tek bir çözümde oluşturmanıza olanak sağlar.

Aşağıdaki 4 dakikalık video, App Service’in daha önceki Azure teklifleriyle nasıl ilişkili olduğu ve yenilikleri hakkında kısa bir açıklama sağlar.

+[AZURE.VIDEO app-service-history-lesson] 

## App Service nedir?
App Service’in temel özelliklerinden bazıları şunlardır: 

* **Birden çok dil ve çerçeve** - App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek sunar. Ayrıca, App Service sanal makineleri üzerinde [Windows PowerShell ve diğer betikleri veya yürütülebilir dosyaları](../app-service-web/web-sites-create-web-jobs.md) çalıştırabilirsiniz.
* **DevOps iyileştirmesi** - Visual Studio Team Services, GitHub veya BitBucket ile [sürekli tümleştirme ve dağıtım](../app-service-web/app-service-continuous-deployment.md) ayarlayın. [Test ve hazırlık ortamları](../app-service-web/web-sites-staged-publishing.md) aracılığıyla güncelleştirmeleri yükseltin. [A/B testi](../app-service-web/app-service-web-test-in-production-get-start.md) gerçekleştirin. [Azure PowerShell](../powershell-install-configure.md) veya [platformlar arası komut satırı arabirimi (CLI)](../xplat-cli-install.md) kullanarak uygulamalarınızı App Service’de yönetin.
* **Yüksek kullanılabilirlik ile küresel ölçeklendirme** - El ile veya otomatik olarak ölçek [artırabilir](../app-service-web/web-sites-scale.md) veya [genişletebilirsiniz](../azure-portal/insights-how-to-scale.md). Uygulamalarınızı Microsoft'un küresel veri merkezi altyapılarının herhangi bir yerinde barındırın. App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/)’sı yüksek kullanılabilirlik taahhüt eder.
* **SaaS platformları ve şirket için veri bağlantıları** - Kurumsal sistemler (SAP, Siebel ve Oracle gibi), SaaS hizmetleri (Salesforce ve Office 365 gibi) ve İnternet hizmetleri (Facebook ve Twitter gibi) için 50’den fazla [bağlayıcı](../connectors/apis-list.md) arasından seçim yapın. [Karma Bağlantılar](../biztalk-services/integration-hybrid-connection-overview.md)’ı ve [Azure Sanal Ağlar](../app-service-web/web-sites-integrate-with-vnet.md)’ı kullanarak şirket içi verilere erişin.
* **Güvenlik ve uyumluluk** - App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/TrustCenter/).
* **Uygulama şablonları** - WordPress, Joomla ve Drupal gibi popüler açık kaynaklı yazılımları bir sihirbaz ile yüklemenize olanak tanıyan [Azure Marketi](https://azure.microsoft.com/marketplace/)’ndeki kapsamlı uygulama şablonu listesinden seçiminizi yapın.
* **Visual Studio tümleştirmesi** -Visual Studio’daki ayrılmış araçlar oluşturma, dağıtma ve hata ayıklama işlemlerini kolaylaştırır.

## App Service’deki uygulama türleri
App Service her biri belirli bir iş yükü türünü barındırmaya yönelik birkaç *uygulama türü* sunar:

* [**Web Apps**](../app-service-web/app-service-web-overview.md) - Web sitelerini ve web uygulamalarını barındırmaya yöneliktir.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) Mobil uygulama arka uçlarını barındırmaya yöneliktir.
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - Bulut API’lerini barındırmaya yöneliktir. 
* [**Logic Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - Kod yazmadan verilerin bulutlardaki erişimini ve kullanımını otomatikleştirmeye yöneliktir.

Buradaki *uygulama* sözcüğü bir iş yükünü çalıştırmaya ayrılmış barındırma kaynaklarını ifade eder. “Web uygulaması” örnek olarak alındığında, web uygulamasını tarayıcıya işlev kazandıran işlem kaynaklar ve uygulama kodu bütünü olarak düşünmeye büyük olasılıkla alışkınsınız. Ancak, App Service’deki *web uygulamaları*, uygulama kodunuzun barındırılması için Azure tarafından sağlanan işlem kaynaklarıdır. Uygulamanız bir web ön ucu ve bir RESTful API’si arka ucundan oluşuyorsa hem bir web uygulamasına dağım yapabilir hem de ön uç kodunuzu bir web uygulamasına, arka uç kodunuzu bir API uygulamasına dağıtabilirsiniz. Uygulamanız farklı türlerde birden fazla App Service uygulamasından oluşabilir.

## App Service Planları
[App Service Planları](azure-web-sites-web-hosting-plans-in-depth-overview.md) uygulamanızın üzerinde çalıştığı işlem kaynaklarının türünü belirtir. Hafif trafik yükleri bekliyorsanız paylaşılan sanal makineler (VM) kullanabilirsiniz. Daha yüksek yükler için çeşitli boyutlarda ayrılmış sanal makinelerden seçim yapabilirsiniz. Birden fazla App Service uygulaması aynı planı paylaşabilir ve planla birlikte ölçeği artıp azalabilir.

Daha fazla ölçeklenebilirlik ve ağ yalıtımını gerekiyorsa uygulamalarınızı bir [App Service Ortamı](../app-service-web/app-service-app-service-environment-intro.md) içinde çalıştırabilirsiniz. 

## Fiyatlandırma
App Service’in maliyeti hakkında bilgi için bkz. [App Service Fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/). 

## App Service’i Kullanmaya Başlama
[Geçici bir web uygulamasını, mobil uygulamayı veya mantıksal uygulamayı](http://go.microsoft.com/fwlink/?LinkId=523751) kredi kartına ve herhangi bir taahhüde gerek olmadan zahmetsiz bir biçimde ücretsiz olarak oluşturun.

Ya da bir [ücretsiz Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) açıp, kullanmaya başlama öğreticilerimizden birini deneyin:

* [Öğretici: Web uygulaması oluşturma](../app-service-web/app-service-web-get-started.md)
* [Öğretici: Mobil uygulama oluşturma](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Öğretici: API uygulaması oluşturma](../app-service-api/app-service-api-dotnet-get-started.md)
* [Öğretici: Mantıksal uygulama oluşturma](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!--HONumber=Sep16_HO3-->


