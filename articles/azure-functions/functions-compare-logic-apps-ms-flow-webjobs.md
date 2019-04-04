---
title: Microsoft Flow, Logic Apps, İşlevler ve Web işleri nelerdir? - Azure
description: 'Tümleştirme görevleri için iyileştirilen Microsoft bulut hizmetlerini karşılaştırın: Microsoft Flow, Logic Apps, İşlevler ve Web işleri.'
services: functions, logic-apps
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: microsoft flow, flow, akış, mantıksal uygulamalar, azure işlevleri, işlevler, azure web işleri, web işleri, olay işleme, dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 04/09/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: ea99c7fe9bc7fd8d6e4e26baa0afe45505949098
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895656"
---
# <a name="what-are-microsoft-flow-logic-apps-functions-and-webjobs"></a>Microsoft Flow, Logic Apps, İşlevler ve Web işleri nelerdir?

Bu makalede aşağıdaki Microsoft bulut hizmetleri karşılaştırılır:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure App Service Web işleri](../app-service/webjobs-create.md)

Tüm bu hizmetler, tümleştirme sorunlarını çözebilir ve iş süreçlerini otomatikleştirebilir. Tümü giriş, eylemler, koşullar ve çıkış tanımı yapabilir. Her birini belirli bir zamanlamayla veya tetikleyiciyle çalıştırabilirsiniz. Her hizmetin benzersiz avantajları vardır ve bu makalede farklar açıklanmaktadır.

## <a name="compare-microsoft-flow-and-azure-logic-apps"></a>Microsoft Flow ve Azure Logic Apps karşılaştırması

Microsoft Flow ve Logic Apps hem de olan *tasarımcıya öncelik veren* tümleştirme hizmetleri, iş akışları oluşturabilirsiniz. Her iki hizmet de çeşitli SaaS uygulamaları ve kurumsal uygulamalarla tümleştirilir. 

Microsoft Flow, Logic Apps'in üzerine yapılandırılmıştır. Aynı iş akışı tasarımcısını ve aynı paylaştıkları [Bağlayıcılar](../connectors/apis-list.md). 

Microsoft Flow güçlendirir türlü ofis çalışanını geliştiricilere basit tümleştirmeler (örneğin, bir SharePoint belge kitaplığına bir onay işlemine) yapma veya BT. Mantıksal uygulamalar, nerede Kurumsal düzeyde Azure DevOps ve güvenlik uygulamaları gerektiğinde Gelişmiş tümleştirmelere (örneğin, B2B işlemleri) de etkinleştirebilirsiniz. Kurumsal iş akışının zamanla karmaşık hale gelmesi tipik bir durumdur. Buna uygun olarak, başta bir akış ile başlayın ve ardından bir mantıksal uygulama için gerektiği şekilde dönüştürün.

Aşağıdaki tabloda, Microsoft Flow veya Logic Apps belirli bir tümleştirme için en iyi olup olmadığını belirlemenize yardımcı olur:

|  | Microsoft Flow | Logic Apps |
| --- | --- | --- |
| Kullanıcılar |Ofis çalışanları, iş kullanıcıları veya SharePoint yöneticileri |Uzman tümleştiriciler ve geliştiriciler, BT uzmanları |
| Senaryolar |Self servis |Gelişmiş tümleştirmeler |
| Tasarım aracı |Tarayıcı içi ve mobil uygulama, yalnızca kullanıcı arabirimi |Tarayıcı içi ve [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Cod görünümü](../logic-apps/logic-apps-author-definitions.md) sağlanır |
| Uygulama Yaşam Döngüsü Yönetimi (ALM) |Tasarım ve test üretim dışı ortamlarda, hazır olduğunuzda üretime Yükselt |Azure DevOps: kaynak denetimi, test, destek, otomasyon ve yönetilebilirlik, [Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Yönetici deneyimi |Microsoft Flow ortamları ve veri kaybı önleme (DLP) ilkelerini yönetme, lisansları izleme: [Microsoft Flow Yönetim Merkezi](https://admin.flow.microsoft.com) |Kaynak gruplarını, bağlantıları, erişim yönetimi ve günlüğe kaydetme yönetin: [Azure portal](https://portal.azure.com) |
| Güvenlik |Office 365 güvenlik ve uyumluluk denetim günlükleri, DLP, [bekleyen şifreleme](https://wikipedia.org/wiki/Data_at_rest#Encryption) hassas veriler için |Azure'un güvenlik güvencesi: [Azure güvenlik](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity), [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), [denetim günlükleri](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) |

## <a name="compare-azure-functions-and-azure-logic-apps"></a>Azure İşlevleri ve Azure Logic Apps karşılaştırması

İşlevler ve Logic Apps, sunucusuz iş yüklerine olanak tanıyan Azure hizmetleridir. Azure işlevleri, sunucusuz bir işlem hizmeti, Azure Logic Apps, sunucusuz iş akışları sağlar. Hem de karmaşık oluşturabilirsiniz *düzenlemeleri*. Düzenleme, Logic Apps’te karmaşık bir görevin gerçekleştirilmesi için yürütülen, *eylemler* olarak adlandırılan işlevlerin veya adımların bir koleksiyonudur. Örneğin, toplu sipariş işlemek için bir işlevin birçok örneğini paralel olarak yürütmek, tüm örneklerin tamamlanmasını bekleyebilir ve sonra toplama işleminde sonuç hesaplayan bir işlevi yürütmek.

Azure İşlevleri için düzenlemeleri kod yazarak ve [Dayanıklı İşlevler uzantısını](durable/durable-functions-concepts.md) kullanarak geliştirirsiniz. Logic Apps için düzenlemeleri, GUI kullanarak veya yapılandırma dosyalarını düzenleyerek oluşturursunuz.

Düzenleme oluşturduğunuzda, mantıksal uygulamalardan işlev çağırdığınızda ve işlevlerden mantıksal uygulama çağırdığınızda hizmetleri karıştırıp eşleştirebilirsiniz. Hizmet özelliklerine veya kişisel tercihinize göre her düzenlemenin nasıl oluşturulacağını seçin. Aşağıdaki tabloda bu hizmetler arasındaki temel farklılıklardan bazıları listelenmektedir:
 
|  | Dayanıklı İşlevler | Logic Apps |
| --- | --- | --- |
| Geliştirme | Koda öncelik veren (kesinlik temelli) | Tasarımcıya öncelik veren (bildirim temelli) |
| Bağlantı | [Yaklaşık bir düzine bağlama türü](functions-triggers-bindings.md#supported-bindings), özel bağlamalar için kod yazma | [Bağlayıcılardan oluşan büyük koleksiyon](../connectors/apis-list.md), [B2B senaryoları için Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md), [özel bağlayıcı oluşturma](../logic-apps/custom-connector-overview.md) |
| Eylemler | Her etkinlik bir Azure işlevidir; eylem işlevleri için kod yazma |[Hazır eylemlerden oluşan büyük koleksiyon](../logic-apps/logic-apps-workflow-actions-triggers.md)|
| İzleme | [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) | [Azure portalında](../logic-apps/quickstart-create-first-logic-app-workflow.md), [Azure İzleyicisi](../logic-apps/logic-apps-monitor-your-logic-apps.md)|
| Yönetim | [REST API](durable/durable-functions-http-api.md), [Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer) | [Azure portalı](../logic-apps/quickstart-create-first-logic-app-workflow.md), [REST API](https://docs.microsoft.com/rest/api/logic/), [PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp), [Visual Studio](https://docs.microsoft.com/azure/logic-apps/manage-logic-apps-with-visual-studio) |
| Yürütme bağlamı | Çalıştırabilirsiniz [yerel olarak](functions-runtime-overview.md) veya bulutta | Yalnızca bulutta çalışan|

<a name="function"></a>

## <a name="compare-functions-and-webjobs"></a>İşlevler Web İşleri karşılaştırması

Azure İşlevleri gibi, WebJobs SDK ile Azure App Service WebJobs da geliştiriciler için tasarlanmış, *koda öncelik veren* bir tümleştirme hizmetidir. Her ikisi de [Azure App Service](../app-service/overview.md) üzerinde derlenmiş olup [source control integration](../app-service/deploy-continuous-deployment.md), [authentication](../app-service/overview-authentication-authorization.md) ve [monitoring with Application Insights integration](functions-monitoring.md) gibi özellikleri destekler.

### <a name="webjobs-and-the-webjobs-sdk"></a>Web İşleri ve Web İşleri SDK’sı

Kullanabileceğiniz *WebJobs* betik veya kod bir App Service bağlamında web uygulamasını çalıştırmak için App Service özelliğidir. *WebJobs SDK*, Azure hizmetlerine yanıt olarak yazdığınız kodu kolaylaştıran WebJobs için tasarlanmış bir çerçevedir. Örneğin, bir küçük resim oluşturarak Azure Depolama'da görüntü blob'u oluşturulmasına yanıt. WebJobs SDK, WebJob’a dağıtabileceğiniz bir .NET konsol uygulaması olarak çalıştırılır. 

WebJobs ve WebJobs SDK birlikte en iyi şekilde çalışır; ancak WebJobs’ı WebJobs SDK olmadan kullanabilirsiniz; bunun tersi de olabilir. Bir Web İşi, App Service korumalı alanında çalışan herhangi bir programı veya betiği çalıştırabilir. Web İşleri SDK konsolu uygulaması, şirket içi sunucular gibi konsol uygulamalarının çalıştığı her yerde çalışabilir.

### <a name="comparison-table"></a>Karşılaştırma tablosu

Azure İşlevleri, WebJobs SDK’da derlendiğinden diğer Azure hizmetlerine yönelik aynı bağlantıların ve olay tetikleyicilerinin birçoğunu paylaşır. Azure işlevleri ve WebJobs SDK ile WebJobs arasında seçim yapılırken dikkate alınacak bazı faktörler şunlardır:

|  | İşlevler | WebJobs SDK ile WebJobs |
| --- | --- | --- |
|[Otomatik ölçeklendirme](functions-scale.md#how-the-consumption-and-premium-plans-work) ile [sunucusuz uygulama modeli](https://azure.microsoft.com/solutions/serverless/)|✔||
|[Tarayıcıda test ve geliştirme](functions-create-first-azure-function.md) |✔||
|[Kullanım başına ödeme fiyatlandırması](functions-scale.md#consumption-plan)|✔||
|[Logic Apps ile tümleştirme](functions-twitter-email.md)|✔||
| Tetikleyici olayları |[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/Web (GitHub, Slack) kancası](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Dosya sistemi](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Desteklenen diller  |C#<br>F#<br>JavaScript<br>Java (önizleme) |C#<sup>1</sup>|
|Paket yöneticileri|NPM ve NuGet|NuGet<sup>2</sup>|

<sup>1</sup> WebJobs (WebJobs SDK olmadan) destekleyen C#, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python ve daha fazlası. Bu liste kapsamlı değildir. WebJob, App Service korumalı alanında çalıştırılabilen herhangi bir programı veya betiği çalıştırabilir.

<sup>2</sup> WebJobs (WebJobs SDK olmadan), NPM ve Nuget'i destekler.

### <a name="summary"></a>Özet

Azure işlevleri, Azure App Service WebJobs sağladığından daha fazla Geliştirici üretkenliğini sunar. Ayrıca, programlama dilleri, geliştirme ortamları, Azure hizmet tümleştirmesi ve fiyatlandırma için daha fazla seçenek sunar. Çoğu senaryo için bu en iyi seçenektir.

WebJobs’ın en iyi seçenek olduğu iki senaryo aşağıda verilmiştir:

* Olayları dinleyen kod (`JobHost` nesnesi) üzerinde daha fazla denetime ihtiyacınız vardır. İşlevler, [host.json](functions-host-json.md) dosyasında `JobHost` davranışını özelleştirmek için sınırlı sayıda yöntem sunar. Bazen bir JSON dosyasındaki dize tarafından belirtilemeyen şeyler yapmanız gerekir. Örneğin, yalnızca WebJobs SDK, Azure Depolama için özel bir yeniden deneme ilkesi yapılandırmanıza olanak sağlar.
* Kod parçacıklarını çalıştırmak istediğiniz bir App Service uygulamanız vardır ve aynı Azure DevOps ortamında birlikte yönetmek istiyorsunuz.

Azure veya üçüncü taraf hizmetleri tümleştirmek için kod parçacıklarını çalıştırmak istediğiniz diğer durumlarda, WebJobs SDK ile WebJobs üzerinden Azure İşlevleri’ni seçin.

<a name="together"></a>

## <a name="microsoft-flow-logic-apps-functions-and-webjobs-together"></a>Microsoft Flow, Logic Apps, İşlevler ve Web işleri birlikte

Yalnızca bu hizmetlerden birini seçmeniz gerekmez. Bunlar birbirleriyle yanı sıra yaptıkları dış hizmetlerle tümleştirin.

Akış bir mantıksal uygulamayı çağırabilir. Mantıksal uygulama bir işlevi çağırabilir ve işlev de bir mantıksal uygulamayı çağırabilir. Örneğin, bkz. [Azure Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md).

Microsoft Flow, Logic Apps ve işlevler arasındaki tümleştirme zaman içinde geliştirilmeye devam ediyor. Bir hizmette bir şey oluşturabilir ve bunu diğer hizmetlerde kullanabilirsiniz.

Aşağıdaki bağlantıları kullanarak, tümleştirme hizmetleri hakkında daha fazla bilgi alabilirsiniz:

* [Azure işlevleri ve Azure App Service'ten yararlanma-Christopher Anderson tümleştirme senaryoları için yararlanarak](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Charles Lamanna ile kolay tümleştirme](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps canlı Web yayını](https://aka.ms/logicappslive)
* [Microsoft Flow sık sorulan sorular](https://flow.microsoft.com/documentation/frequently-asked-questions/)

## <a name="next-steps"></a>Sonraki adımlar

İlk akışınızı, mantıksal uygulamanızı veya işlev uygulamanızı oluşturarak başlayın. Aşağıdaki bağlantılardan birini seçin:

* [Microsoft Flow’u kullanmaya başlayın](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [İlk Azure işlevinizi oluşturma](functions-create-first-azure-function.md)
