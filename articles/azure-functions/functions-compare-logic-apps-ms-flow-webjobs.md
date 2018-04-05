---
title: Flow, Logic Apps, Functions ve WebJobs arasında seçim yapma | Microsoft Docs
description: Microsoft'un bulut tümleştirme hizmetlerini karşılaştırın ve hangi hizmetleri kullanacağınıza karar verin.
services: functions,app-service\logic
documentationcenter: na
author: tdykstra
manager: wpickett
tags: ''
keywords: microsoft flow, flow, akış, mantıksal uygulamalar, azure işlevleri, işlevler, azure web işleri, web işleri, olay işleme, dinamik işlem, sunucusuz mimari
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/20/2018
ms.author: tdykstra
ms.custom: mvc
ms.openlocfilehash: 577031c58e95781dc97721acc71fb22114b1c606
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Flow, Logic Apps, Functions ve WebJobs arasında seçim yapma

Bu makalede, Microsoft buluttaki aşağıdaki hizmetler karşılaştırılmaktadır:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure App Service Web İşleri](../app-service/web-sites-create-web-jobs.md)

Tüm bu hizmetler, tümleştirme sorunlarını çözebilir ve iş süreçlerini otomatikleştirebilir. Tümü giriş, eylemler, koşullar ve çıkış tanımı yapabilir. Her birini belirli bir zamanlamayla veya tetikleyiciyle çalıştırabilirsiniz. Ancak, her hizmetin benzersiz avantajları vardır ve bu makalede farklar açıklanmaktadır.

## <a name="flow-vs-logic-apps"></a>Flow - Logic Apps

Microsoft Flow da Azure Logic Apps de *yapılandırmaya öncelik veren* tümleştirme hizmetleridir. Her ikisi de çeşitli SaaS ve kuruluş uygulamalarıyla tümleştirilen iş akışları oluşturur. 

Flow, Logic Apps’in üzerine yapılandırılmıştır. Aynı iş akışı tasarımcısını ve aynı [Bağlayıcıları](../connectors/apis-list.md) paylaşırlar. 

Flow, her türlü ofis çalışanını geliştiricilere veya BT'ye başvurmadan basit tümleştirmeler yapma (örneğin, SharePoint Belge Kitaplığı üzerindeki bir onay işlemi) yönünde güçlendirir. Öte yandan Logic Apps, kurumsal düzeyde DevOps ve güvenlik uygulamaları gerektiğinde gelişmiş tümleştirmelere (örneğin, B2B işlemleri) olanak tanıyabilir. Kurumsal iş akışının zamanla karmaşık hale gelmesi tipik bir durumdur. Buna uygun olarak, önce Flow ile başlayabilir ve daha sonra gerektiğinde bunu Logic Apps'e dönüştürebilirsiniz.

Aşağıdaki tablo, belirli bir tümleştirme için Flow'un mu yoksa Logic Apps'in mi daha uygun olduğunu saptamanıza yardımcı olur.

|  | Akış | Logic Apps |
| --- | --- | --- |
| Kullanıcılar |Ofis çalışanları, iş kullanıcıları veya SharePoint yöneticileri |Uzman tümleştiriciler ve geliştiriciler, BT uzmanları |
| Senaryolar |Self servis |Gelişmiş tümleştirmeler |
| Tasarım Aracı |Tarayıcı içi ve mobil uygulama, yalnızca kullanıcı arabirimi |Tarayıcı içi ve [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Cod görünümü](../logic-apps/logic-apps-author-definitions.md) sağlanır |
| Uygulama Yaşam Döngüsü Yönetimi (ALM) |Üretim dışı ortamlarda tasarlayıp test edin, hazır olduğunuzda üretime geçirin. |DevOps: [Azure Kaynak Yönetimi](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md)'nde kaynak denetimi, test, destek, otomasyon ve yönetilebilirlik |
| Yönetici Deneyimi |Flow Ortamlarını ve Veri Kaybı Önleme (DLP) ilkelerini yönetme, lisansları izleme [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Kaynak Gruplarını, Bağlantıları, Erişim Yönetimini ve Günlüğe Kaydetmeyi Yönetme[https://portal.azure.com](https://portal.azure.com) |
| Güvenlik |Office 365 Güvenlik ve Uyumluluk denetim günlükleri, Veri Kaybı Önleme (DLP), gizli veriler için [bekleyen verileri şifreleme](https://wikipedia.org/wiki/Data_at_rest#Encryption) vs. |Azure'un güvenlik güvencesi: [Azure Güvenliği](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), [denetim günlükleri](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/), ve dahası. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>İşlevler - WebJobs

Azure İşlevleri gibi, WebJobs SDK ile Azure App Service WebJobs da geliştiriciler için tasarlanmış, *koda öncelik veren* bir tümleştirme hizmetidir. Her ikisi de [Azure App Service](../app-service/app-service-web-overview.md) üzerinde derlenmiş olup [source control integration](../app-service/app-service-continuous-deployment.md), [authentication](../app-service/app-service-authentication-overview.md) ve [monitoring with Application Insights integration](functions-monitoring.md) gibi özellikleri destekler.

### <a name="webjobs-vs-the-webjobs-sdk"></a>WebJobs ve WebJobs SDK karşılaştırması

App Service’ın *WebJobs* özelliği, bir App Service web uygulaması bağlamında betik veya kod çalıştırmanıza olanak sağlar. *WebJobs SDK*, Azure hizmetlerine yanıt olarak yazdığınız kodu kolaylaştıran WebJobs için tasarlanmış bir çerçevedir. Örneğin, bir küçük resim oluşturarak Azure Depolama’da görüntü blob’u oluşturulmasına yanıt verebilirsiniz. WebJobs SDK, WebJob’a dağıtabileceğiniz bir .NET konsol uygulaması olarak çalıştırılır. 

WebJobs ve WebJobs SDK birlikte en iyi şekilde çalışır; ancak WebJobs’ı WebJobs SDK olmadan kullanabilirsiniz; bunun tersi de olabilir. WebJob, App Service korumalı alanında çalıştırılabilen herhangi bir programı veya betiği çalıştırabilir. WebJobs SDK konsol uygulaması, şirket içi sunucular gibi, konsol uygulamalarının çalıştırılabildiği her yerde çalıştırılabilir.

### <a name="comparison-table"></a>Karşılaştırma tablosu

Azure İşlevleri, WebJobs SDK’da derlendiğinden diğer Azure hizmetlerine yönelik aynı bağlantıların ve olay tetikleyicilerinin birçoğunu paylaşır. WebJobs SDK ile WebJobs ve Azure İşlevleri arasında seçim yapılırken dikkate alınacak bazı faktörler şunlardır:

|  | İşlevler | WebJobs SDK ile WebJobs |
| --- | --- | --- |
|[Otomatik ölçeklendirme](functions-scale.md#how-the-consumption-plan-works) ile [sunucusuz uygulama modeli](https://azure.microsoft.com/overview/serverless-computing/)|✔||
|[Tarayıcıda geliştirme ve test etme](functions-create-first-azure-function.md) |✔||
|[Kullanım başına ödeme fiyatlandırması](functions-scale.md#consumption-plan)|✔||
|[Logic Apps ile tümleştirme](functions-twitter-email.md)|✔||
| Tetikleyici olayları |[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Dosya sistemi](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Desteklenen diller  |C#<br>F#<br>JavaScript<br>Java (önizleme) |C#<sup>1</sup>|
|Paket yöneticileri|NPM ve NuGet|NuGet<sup>2</sup>|

<sup>1</sup> WebJobs (WebJobs SDK olmadan); C#, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python ve daha fazlasını destekler. Bu listede hepsine yer verilmemiştir. WebJob, App Service korumalı alanında çalıştırılabilen herhangi bir programı veya betiği çalıştırabilir.

<sup>2</sup> WebJobs (WebJobs SDK olmadan), NPM ve NuGet’i destekler.

### <a name="summary"></a>Özet

Azure İşlevleri, daha yüksek geliştirici üretkenliği, daha fazla programlama dili seçeneği, daha fazla geliştirme ortamı seçeneği, daha fazla Azure hizmet tümleştirme seçenekleri ve daha fazla fiyatlandırma seçeneği sunar. Çoğu senaryo için bu en iyi seçenektir.

WebJobs’ın en iyi seçenek olduğu iki senaryo aşağıda verilmiştir:

* Olayları dinleyen kod (`JobHost` nesnesi) üzerinde daha fazla denetime ihtiyacınız vardır. İşlevler, [host.json](functions-host-json.md) dosyasında `JobHost` davranışını özelleştirmek için sınırlı sayıda yöntem sunar. Bazen bir JSON dosyasındaki dize tarafından belirtilemeyen şeyler yapmanız gerekir. Örneğin, yalnızca WebJobs SDK, Azure Depolama için özel bir yeniden deneme ilkesi yapılandırmanıza olanak sağlar.
* Kod parçacıklarını çalıştırmak istediğiniz bir App Service uygulamanız vardır ve bunları aynı DevOps ortamında birlikte yönetmek istiyorsunuz.

Azure veya üçüncü taraf hizmetleri tümleştirmek için kod parçacıklarını çalıştırmak istediğiniz diğer durumlarda, WebJobs SDK ile WebJobs üzerinden Azure İşlevleri’ni seçin.

<a name="together"></a>

## <a name="flow-logic-apps-functions-and-webjobs-together"></a>Flow, Logic Apps, Azure İşlevleri ve WebJobs

Bu hizmetlerden birini seçmeniz gerekmez; birbirleriyle ve dış hizmetlerle tümleştirilir.

Akış bir mantıksal uygulamayı çağırabilir. Mantıksal uygulama bir işlevi çağırabilir ve işlev de bir mantıksal uygulamayı çağırabilir. Örneğin, bkz. [Azure Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md).

Flow, Logic Apps ve İşlevler arasındaki tümleştirme zaman içinde geliştirilmeye devam ediyor. Bir hizmette bir şey oluşturabilir ve bunu diğer hizmetlerde kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İlk akışınızı, mantıksal uygulamanızı veya işlev uygulamanızı oluşturarak başlayın. Aşağıdaki bağlantılardan herhangi birine tıklayın:

* [Microsoft Flow’u kullanmaya başlama](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)

İsterseniz, aşağıdaki bağlantılarda bu tümleştirme hizmetleriyle ilgili daha fazla bilgi bulabilirsiniz:

* [Tümleştirme senaryoları için Azure İşlevleri ve Azure App Service'ten yararlanma - Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Tümleştirmeler Basitleşti - Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Canlı Web Yayını](http://aka.ms/logicappslive)
* [Microsoft Flow Sık sorulan sorular](https://flow.microsoft.com/documentation/frequently-asked-questions/)
