---
title: "Flow, Logic Apps, Functions ve WebJobs arasında seçim yapma | Microsoft Docs"
description: "Microsoft'un bulut tümleştirme hizmetlerini karşılaştırın ve hangi hizmetleri kullanacağınıza karar verin."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: "microsoft flow, flow, akış, mantıksal uygulamalar, azure işlevleri, işlevler, azure web işleri, web işleri, olay işleme, dinamik işlem, sunucusuz mimari"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7ffe44828735a5687008ebc5a7d8d9f017f49daa
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Flow, Logic Apps, Functions ve WebJobs arasında seçim yapma
Bu makalede, Microsoft bulutunda yer alan ve hepsi tümleştirme sorunlarını çözmeye, iş süreçlerini otomatikleştirmeye yönelik olan aşağıdaki hizmetler karşılaştırılır:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure App Service Web İşleri](../app-service/web-sites-create-web-jobs.md)

Bu hizmetlerin tümü, dağınık sistemleri birbirine "yapıştırmak" için yararlıdır. Tümü giriş, eylemler, koşullar ve çıkış tanımı yapabilir. Her birini belirli bir zamanlamayla veya tetikleyiciyle çalıştırabilirsiniz. Bununla birlikte, her hizmetin kendine özgü avantajları vardır ve "En iyi hizmet hangisi?" bağlamında bir karşılaştırma yapılmaz. Karşılaştırma, "Bu duruma en uygun hizmet hangisi?" bağlamında yapılır. Genellikle, ölçeklendirilebilir, tam özellikli bir tümleştirme çözümünü kısa sürede oluşturmak için en iyi yol bu hizmetlerin bir bileşimini kullanmaktır.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flow - Logic Apps
Microsoft Flow ile Azure Logic Apps'i birlikte tartışabiliriz çünkü her ikisi de *yapılandırmaya öncelik veren* tümleştirme hizmetleridir. Süreçleri ve iş akışlarını oluşturmayı kolaylaştırır, çeşitli SaaS ve kuruluş uygulamalarıyla tümleşik çalışırlar. 

* Flow, Logic Apps'in üzerine yapılandırılmıştır
* İş akışı tasarımcıları aynıdır
* Birinde çalışan [Bağlayıcılar](../connectors/apis-list.md) diğerinde de çalışabilir

Flow, her türlü ofis çalışanını geliştiricilere veya BT'ye başvurmadan basit tümleştirmeler yapma (örneğin, SharePoint Belge Kitaplığı üzerindeki bir onay işlemi) yönünde güçlendirir. Öte yandan Logic Apps, kurumsal düzeyde DevOps ve güvenlik uygulamaları gerektiğinde gelişmiş tümleştirmelere (örneğin, B2B işlemleri) olanak tanıyabilir. Kurumsal iş akışının zamanla karmaşık hale gelmesi tipik bir durumdur. Buna uygun olarak, önce Flow ile başlayabilir ve daha sonra gerektiğinde bunu Logic Apps'e dönüştürebilirsiniz.

Aşağıdaki tablo, belirli bir tümleştirme için Flow'un mu yoksa Logic Apps'in mi daha uygun olduğunu saptamanıza yardımcı olur.

|  | Akış | Logic Apps |
| --- | --- | --- |
| Hedef kitle |Ofis çalışanları, iş kullanıcıları veya SharePoint yöneticileri |Uzman tümleştiriciler ve geliştiriciler, BT uzmanları |
| Senaryolar |Self servis |Gelişmiş tümleştirmeler |
| Tasarım Aracı |Tarayıcı içi ve mobil uygulama, yalnızca kullanıcı arabirimi |Tarayıcı içi ve [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Cod görünümü](../logic-apps/logic-apps-author-definitions.md) sağlanır |
| Uygulama Yaşam Döngüsü Yönetimi (ALM) |Üretim dışı ortamlarda tasarlayıp test edin, hazır olduğunuzda üretime geçirin. |DevOps: kaynak denetimi, test, destek, [Azure Kaynak Yönetimi](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md)'nde otomasyon ve yönetilebilirlik |
| Yönetici Deneyimi |Flow Ortamlarını ve Veri Kaybı Önleme (DLP) ilkelerini yönetme, lisansları izleme [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Kaynak Gruplarını, Bağlantıları, Erişim Denetimini ve Günlüğe Kaydetmeyi Yönetme [https://portal.azure.com](https://portal.azure.com) |
| Güvenlik |Office 365 Güvenlik ve Uyumluluk denetim günlükleri, Veri Kaybı Önleme (DLP), gizli veriler için [bekleyen verileri şifreleme](https://wikipedia.org/wiki/Data_at_rest#Encryption) vs. |Azure'un güvenlik güvencesi: [Azure Güvenliği](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), [denetim günlükleri](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/), ve dahası. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>İşlevler - WebJobs
Azure İşlevleri ile Azure App Service Web İşleri'ni birlikte tartışabiliriz çünkü her ikisi de *koda öncelik veren* tümleştirme hizmetleridir ve geliştiricilere yönelik tasarlanmıştır. [Yeni Depolama Blobları](functions-bindings-storage.md) veya [Web Kancası isteği](functions-bindings-http-webhook.md) gibi çeşitli olaylara yanıt olarak bir betik veya kod parçası çalıştırmanıza olanak tanır. Benzerlikleri şunlardır: 

* Her ikisi de [Azure App Service](../app-service/app-service-web-overview.md) üzerine yapılandırılmıştır ve [kaynak denetimi](../app-service/app-service-continuous-deployment.md), [kimlik doğrulama](../app-service/app-service-authentication-overview.md) ve [izleme](../app-service/web-sites-monitor.md) gibi özelliklerden yararlanır.
* Her ikisi de geliştirici odaklı hizmetlerdir.
* Her ikisi de standart komut dosyası ve programlama dillerini destekler.
* Her ikisi de NuGet ve NPM desteğine sahiptir.

İşlevler, Web İşleri'nin doğal bir evrimini temsil eder; Web İşleri'nin en iyi özelliklerini almış ve bunlar üzerine geliştirilmiştir. Geliştirmeler şunlardır: 

* [Sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) uygulama modeli.
* Rahatça doğrudan tarayıcıda kod geliştirme, test etme ve çalıştırma.
* Daha çok Azure hizmetiyle ve [GitHub WebHooks](https://developer.github.com/webhooks/creating/) gibi üçüncü taraf hizmetleriyle yerleşik tümleşiklik.
* Kullandığın kadar ödeme; [App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) için ödeme yapmak gerekmez.
* Otomatik, [dinamik ölçeklendirme](functions-scale.md).
* Mevcut App Service müşterileri için, App Service planıyla devam etmek de mümkündür (değerinden az kullanılan kaynaklardan yararlanmak için).
* Logic Apps ile tümleştirme.

Aşağıdaki tabloda, İşlevler ile Web İşleri arasındaki farklar özetlenir:

|  | İşlevler | WebJobs |
| --- | --- | --- |
| Ölçeklendirme |Yapılandırmasız ölçeklendirme |App Service planıyla ölçeklendirme |
| Fiyatlandırma |Kullandığın kadar ödeme veya App Service planı kapsamında |App Service planı kapsamında |
| Çalıştırma türü |Tetiklenmiş, zamanlanmış (zamanlayıcı tetikleyicisi tarafından) |Tetiklenmiş, sürekli, zamanlanmış |
| Tetikleyici olayları |[Zamanlayıcı](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-cosmosdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/Web Kancası (GitHub, Slack)](functions-bindings-http-webhook.md), [Azure App Service Mobil Uygulamalar](functions-bindings-mobile-apps.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md), [Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md) |[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md), [Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md) |
| Tarayıcı içi geliştirme |Destekleniyor |Desteklenmiyor |
| C# |Destekleniyor |Destekleniyor |
| F# |Destekleniyor |Desteklenmiyor |
| JavaScript |Destekleniyor |Destekleniyor |
| Java |Önizleme | Desteklenmiyor |
| Bash |Deneysel |Destekleniyor |
| Windows komut dosyası (.cmd, .bat) |Deneysel |Destekleniyor |
| PowerShell |Deneysel |Destekleniyor |
| PHP |Deneysel |Destekleniyor |
| Python |Deneysel |Destekleniyor |
| TypeScript |Deneysel |Desteklenmiyor |

İşlevler'in mi yoksa Web İşleri'nin mi kullanılacağı, son tahlilde şu anda App Service ile ne yapıyor olduğunuza bağlıdır. Kod parçacıkları çalıştırmak istediğiniz bir App Service uygulamanız varsa ve bunları aynı DevOps ortamında birlikte yönetmek istiyorsanız, Web İşleri'ni kullanın. Aşağıdaki senaryolarda İşlevler'i kullanın.

* Diğer Azure hizmetleri veya üçüncü taraf uygulamalar için kop parçacıkları çalıştırmak istiyorsunuz.
* Tümleştirme kodunuzu App Service uygulamalarınızdan ayrı olarak yönetmek istiyorsunuz.
* Kod parçacıklarınızı Mantıksal uygulamanın içinden çağırmak istiyorsunuz. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Flow, Logic Apps ve İşlevler birlikte
Daha önce de belirtildiği gibi, size en uygun hizmetin hangisi olduğu sizin durumunuza bağlıdır. 

* Basit bir kurumsal iyileştirme için Flow kullanın.
* Tümleştirme senaryonuz Flow için fazla gelişmişse veya DevOps özelliklerine ihtiyacınız varsa Logic Apps kullanın.
* Tümleştirme senaryonuzdaki adımlardan biri için üst düzeyde özel bir dönüştürme veya özelleştirilmiş kod gerekiyorsa, bu durumda bir işlev yazın ve işlevi mantıksal uygulamanızda bir eylem olarak tetikleyin.

Bir akış içinde bir mantıksal uygulama çağırabilirsiniz. Ayrıca, mantıksal uygulama içinde bir işlev ve işlev içinde de mantıksal uygulama çağırabilirsiniz. Flow, Logic Apps ve İşlevler arasındaki tümleştirme zaman içinde geliştirilmeye devam ediyor. Bir hizmette bir şey oluşturabilir ve bunu diğer hizmetlerde kullanabilirsiniz. Dolayısıyla, bu üç teknolojiye yaptığınız hiçbir yatırım boşa gitmez.

## <a name="next-steps"></a>Sonraki adımlar
İlk akışınızı, mantıksal uygulamanızı, işlev uygulamanızı veya Web İşinizi oluşturup hizmetlerden her biriyle çalışmaya başlayın. Aşağıdaki bağlantılardan herhangi birine tıklayın:

* [Microsoft Flow’u kullanmaya başlama](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)
* [Visual Studio kullanarak Web İşleri’ni dağıtma](../app-service/websites-dotnet-deploy-webjobs.md)

İsterseniz, aşağıdaki bağlantılarda bu tümleştirme hizmetleriyle ilgili daha fazla bilgi bulabilirsiniz:

* [Tümleştirme senaryoları için Azure İşlevleri ve Azure App Service'ten yararlanma - Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Tümleştirmeler Basitleşti - Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Canlı Web Yayını](http://aka.ms/logicappslive)
* [Microsoft Flow Sık sorulan sorular](https://flow.microsoft.com/documentation/frequently-asked-questions/)

