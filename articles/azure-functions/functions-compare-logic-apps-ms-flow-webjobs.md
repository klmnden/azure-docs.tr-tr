---
title: "Akış, Logic Apps, İşlevler ve Web işleri arasında seçim | Microsoft Docs"
description: "Karşılaştırır ve buna karşılık bulut tümleştirme hizmetleri Microsoft'tan ve kullanmanız gereken hangi hizmetlere karar verin."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: "Microsoft akışı, akış, logic apps, azure işlevleri, İşlevler, azure webjobs, Web işleri, olay işleme, dinamik işlem sunucusuz mimarisi"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: ab0aa377f9803d74d8a7a94bdb4c7b780e3ae41d
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Flow, Logic Apps, Functions ve WebJobs arasında seçim yapma
Bu makalede karşılaştırır ve tüm tümleştirme sorunlarını çözmek ve iş süreçlerini otomatikleştirmek aşağıdaki hizmetleri Microsoft bulutta farklılık gösterir:

* [Microsoft Akış](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure uygulama hizmeti Web işleri](../app-service/web-sites-create-web-jobs.md)

Tüm bu hizmetleri "yapıştırma farklı sistemleri birbirine" yararlı olur. Bunlar tüm giriş, Eylemler, koşullar ve çıkış tanımlayabilirsiniz. Bunların her biri bir zamanlama veya tetikleyici çalıştırabilirsiniz. Ancak, her hizmetin benzersiz avantajları vardır ve bunları karşılaştırma "hangi hizmetin en iyi olduğu?" sorusunu değil Ancak bir "hangi hizmetin en iyisidir bu durum için uygun?" Genellikle, bu hizmetleri birlikte hızlı bir şekilde ölçeklenebilir, tam özellikli bir çözümü oluşturmak için en iyi yoludur.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Akış vs. Logic Apps
Her ikisi de olduklarından Azure mantıksal uygulamaları ve Microsoft Flow birlikte aşağıdakiler ele *yapılandırma ilk* Integration services. Bunlar işlemleri ve iş akışları oluşturma ve çeşitli SaaS ve kuruluş uygulamalarıyla tümleştirmek kolaylaştırır. 

* Akış Logic Apps en üstünde oluşturulmuştur
* Aynı iş akışı Tasarımcısı'nı sahip
* [Bağlayıcılar](../connectors/apis-list.md) bir iş ayrıca diğer çalışabilmesi için

Akış güçlendirir basit tümleştirmeler gerçekleştirmek için herhangi bir office çalışan (örneğin, SMS için önemli e-postaları alması) aracılığıyla geliştiricilerin giderek olmadan veya BT. Diğer taraftan, Logic Apps kuruluş düzeyinde DevOps ve güvenlik uygulamalarını gerekli olduğu Gelişmiş ya da kritik tümleştirmeler (örneğin, B2B işlemler) etkinleştirebilirsiniz. Zaman içinde karmaşıklığı ulaşması bir iş akışı için genel bir durumdur. Buna uygun olarak, başta bir akış başlayın sonra gerektiği gibi bir mantıksal uygulama dönüştürmek.

Aşağıdaki tabloda, akış veya Logic Apps belirli bir tümleştirme için en iyi olup olmadığını belirlemenize yardımcı olur.

|  | Akış | Logic Apps |
| --- | --- | --- |
| Hedef kitle |Office çalışanlarının, iş kullanıcılarının |BT uzmanları, geliştiricilerin |
| Senaryolar |Self Servis |Kritik |
| Tasarım aracı |Tarayıcı içi ve mobil uygulama, yalnızca kullanıcı Arabirimi |Tarayıcı içi ve [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [kod görünümü](../logic-apps/logic-apps-author-definitions.md) kullanılabilir |
| DevOps |Geçici, üretimde geliştirin |Kaynak denetimi, test, destek otomasyon ve yönetilebilirlik, [Azure kaynak yönetimi](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Yönetici deneyimi |[https://Flow.microsoft.com](https://flow.microsoft.com) |[https://Portal.Azure.com](https://portal.azure.com) |
| Güvenlik |Standart uygulamalar: [veri egemenliği](https://wikipedia.org/wiki/Technological_Sovereignty), [bekleyen şifreleme](https://wikipedia.org/wiki/Data_at_rest#Encryption) hassas verileri için vb.. |Azure güvenlik güvencesi: [Azure güvenlik](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), [denetim günlüklerini](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)ve daha fazlası. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>İşlevler vs. WebJobs
Her ikisi de olduklarından Azure işlevleri ve Azure App Service Web işleri birlikte aşağıdakiler ele *kod ilk* tümleştirme hizmetleri ve geliştiricileri için tasarlanmıştır. Bir komut dosyası veya paylaştırılabilen bir kod çeşitli olaylarına yanıt olarak gibi çalıştırmak etkinleştirme [yeni depolama BLOB'lar](functions-bindings-storage.md) veya [bir Web kancası isteği](functions-bindings-http-webhook.md). Kendi benzerlikler şunlardır: 

* Her ikisi de yerleşik [Azure App Service](../app-service/app-service-web-overview.md) ve özellikleri gibi keyfini [kaynak denetimi](../app-service/app-service-continuous-deployment.md), [kimlik doğrulaması](../app-service/app-service-authentication-overview.md), ve [izleme](../app-service/web-sites-monitor.md).
* Her ikisi de Geliştirici odaklı hizmetleridir.
* Hem standart komut ve programlama dilleri destekler.
* Hem NuGet ve NPM'yi destek sahiptir.

İşlevler en iyi şeyler Web işleri hakkında alır ve bunlar üzerine artırır WebJobs doğal evrimi olduğu. Geliştirmeleri içerir: 

* [Sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) uygulama modeli.
* Kolaylaştırılmış geliştirme, test ve doğrudan tarayıcıda kodunun çalıştırın.
* Daha fazla Azure ve 3. taraf hizmetleri ile dahili tümleştirmeyi ister [GitHub Web kancası](https://developer.github.com/webhooks/creating/).
* Ödeme-kullanım başına ödeme gerek bir [uygulama hizmeti planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Otomatik, [dinamik ölçeklendirme](functions-scale.md).
* (Kullanılan kaynaklar yararlanmak) hala olası uygulama hizmeti plan üzerinde çalışan uygulama hizmeti, var olan müşteriler için.
* Logic Apps ile tümleştirme.

Aşağıdaki tabloda, İşlevler ve Web işleri arasındaki farklar özetlenmektedir:

|  | İşlevler | WebJobs |
| --- | --- | --- |
| Ölçeklendirme |Configurationless ölçeklendirme |Uygulama hizmeti planı ile ölçeklendirin |
| Fiyatlandırma |Kullanım başına ödeme ya da uygulama hizmeti planının bir parçası |Uygulama hizmeti planının bir parçası |
| Çalışma türü |tetiklenen, zamanlanmış (Zamanlayıcı tetikleyicisi tarafından) |tetiklenen, sürekli, zamanlanmış |
| Tetikleyici olayları |[Zamanlayıcı](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-documentdb.md), [Azure Event Hubs'a](functions-bindings-event-hubs.md), [HTTP/Web (GitHub, Slack'e) kancası](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure Notification Hubs](functions-bindings-notification-hubs.md), [Azure hizmet veri yolu](functions-bindings-service-bus.md), [Azure depolama](functions-bindings-storage-blob.md) |[Azure depolama](functions-bindings-storage-blob.md), [Azure hizmet veri yolu](functions-bindings-service-bus.md) |
| Tarayıcı içi geliştirme |Destekleniyor |Desteklenmiyor |
| C# |Destekleniyor |Destekleniyor |
| F# |Destekleniyor |Desteklenmiyor |
| JavaScript |Destekleniyor |Destekleniyor |
| Java |Destekleniyor | Desteklenmiyor |
| Bash |Deneysel |Destekleniyor |
| (.Cmd, .bat) Windows komut dosyası |Deneysel |Destekleniyor |
| PowerShell |Deneysel |Destekleniyor |
| PHP |Deneysel |Destekleniyor |
| Python |Deneysel |Destekleniyor |
| TypeScript |Deneysel |Desteklenmiyor |

İşlevler veya Web işleri kullanmak için sonuçta ne zaten App Service ile yaptığınız üzerinde bağlıdır. Kod parçacıkları çalıştırmak istediğiniz bir uygulama hizmeti uygulama varsa ve bunları birlikte aynı DevOps ortamda yönetmek istediğiniz Web işleri kullanın. Aşağıdaki senaryolarda işlevlerini kullanın.

* Diğer Azure Hizmetleri veya 3. taraf uygulamaları için kod parçacıklarını çalıştırmak isteyebilirsiniz.
* Uygulama hizmeti uygulamalarınız tümleştirme kodunuzu ayrı ayrı yönetmek istiyorsunuz.
* Kod parçacıkları mantığı uygulamasından çağırma istiyor. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Akış, Logic Apps ve işlevleri birlikte
Daha önce belirtildiği gibi hangi hizmet için uygundur, durumunuza bağlıdır. 

* Basit iş iyileştirme için akışı kullanın.
* Tümleştirme senaryonuz için akışı çok gelişmiş veya DevOps özellikleri ve güvenlik compliances gerekirse, Logic Apps kullanın.
* Tümleştirme senaryonuz bir adımda, yüksek oranda özel dönüştürme veya özel bir kod gerektiriyorsa, bir işlev yazdığınızda ve bir eylem mantıksal uygulamanızı olarak Tetik işlevi.

Bir mantıksal uygulama bir akışı çağırabilirsiniz. Ayrıca, bir mantıksal uygulama ve bir mantıksal uygulama işlevinde bir işlevde çağırabilirsiniz. Akış, Logic Apps ve işlevleri arasında tümleştirme, zaman içinde iyileştirmek devam eder. Şeyin bir hizmet oluşturmak ve diğer hizmetleri kullanın. Bu nedenle, bu üç teknolojileri yaptığınız tüm Yatırımlar faydalı olur.

## <a name="next-steps"></a>Sonraki adımlar
Her Hizmetleri ile ilk akış, mantıksal uygulama, işlev uygulaması veya Web işi oluşturarak başlayın. Aşağıdaki bağlantılardan herhangi birine tıklayın:

* [Microsoft Flow kullanmaya başlama](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Mantıksal uygulama oluşturun.](../logic-apps/logic-apps-create-a-logic-app.md)
* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)
* [Visual Studio kullanarak Web İşleri’ni dağıtma](../app-service/websites-dotnet-deploy-webjobs.md)

Veya bu Integration services aşağıdaki bağlantıları ile ilgili daha fazla bilgi alın:

* [Yararlanmayı Azure işlevleri & Christopher Yılmaz tarafından tümleştirme senaryoları için Azure uygulama hizmeti](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Charles Lamanna tarafından basit yapılan tümleştirmeler](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps canlı Web yayını](http://aka.ms/logicappslive)
* [Microsoft Akış sık sorulan sorular](https://flow.microsoft.com/documentation/frequently-asked-questions/)

