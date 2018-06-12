---
title: Örnekler & yaygın senaryolar - Azure Logic Apps | Microsoft Docs
description: Logic apps ile örnekler, senaryoları, öğreticiler ve izlenecek yollar hakkında daha fazla bilgi edinin
services: logic-apps
author: jeffhollan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 01/31/18
ms.author: LADocs; jehollan
ms.openlocfilehash: 8b340db5f89f93869486bf7541acbc3d580868a3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300047"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Ortak senaryolar, örnekler, öğreticiler ve Azure Logic Apps için izlenecek yollar

[Azure mantıksal uygulamaları](../logic-apps/logic-apps-overview.md) , düzenlemek ve farklı Hizmetleri sağlayarak tümleştirmenize yardımcı [100 + kullanıma hazır Bağlayıcılar](../connectors/apis-list.md), Microsoft Bilişsel hizmetler için şirket içi SQL Server veya SAP aralığı. Logic Apps hizmeti "sunucusuz" olduğundan ölçek veya örnekleri hakkında endişelenmeniz gerekmez. Yapmanız gereken tek şey iş akışı tetikleyici ve iş akışı gerçekleştirdiği eylemleri tanımlayın. Temel alınan platform ölçek, kullanılabilirlik ve performans işler. Logic Apps kullanım örnekleri ve burada birden çok sistem üzerinde birden çok eylem koordine olmanız senaryoları için kullanışlıdır.

Birçok desenleri ve yetenekler hakkında daha fazla bilgi edinmenize yardımcı olmak için [Azure Logic Apps](../logic-apps/logic-apps-overview.md) destekler, yayın örnekleri ve senaryoları şunlardır.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Başlangıç noktaları mantığı uygulama iş akışları için popüler

Her mantıksal uygulama ile başlayan bir [ *tetikleyici*](../logic-apps/logic-apps-overview.md#logic-app-concepts)ve logic app akışınızı başlatır ve herhangi bir veri tetikleyicisi bir parçası olarak geçirir yalnızca bir tetikleyici. Bazı bağlayıcılar bu türlerinde gelen Tetikleyiciler sağlar:

* *Yoklama Tetikleyicileri*: hizmet uç noktası yeni veriler için düzenli olarak denetler. Yeni verileri mevcut olduğunda, tetikleyici oluşturur ve yeni bir iş akışı örneği girdi olarak verilerle çalışır.

* *Tetikleyiciler anında*: hizmet uç noktası verileri dinler ve belirli bir olay gerçekleştiğinde kadar bekler. Olay gerçekleştiğinde, Tetikleyici oluşturma ve herhangi bir kullanılabilir veri giriş olarak kullanan yeni bir iş akışı örneği çalıştırma hemen başlatılır.

Yalnızca birkaç popüler tetikleyici örnekler şunlardır:

* Yoklama: 

  * [**Zamanlama - yinelenme** tetikleyici](../connectors/connectors-native-recurrence.md) başlangıç tarihi ve saati artı mantıksal uygulamanızı tetikleme için yineleme ayarlamanıza olanak tanır. 
  Örneğin, haftanın günleri ve saatleri mantıksal uygulamanızı tetiklemek günün seçebilirsiniz.

  * Mantıksal uygulamanızı mantıksal uygulamalar tarafından Örneğin, desteklenen herhangi bir posta sağlayıcıdan gelen yeni e-posta denetle "Bir e-posta alındığında" tetikleyicisi sağlar [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/)ve benzeri.

  * [ **HTTP** tetikleyici](../connectors/connectors-native-http.md) belirtilen hizmet uç noktası, HTTP üzerinden iletişim kurarak denetleyin mantıksal uygulamanızı sağlar.
  
* Gönderin:

  * [ **İstek / yanıt - istek** tetikleyici](../connectors/connectors-native-reqres.md) HTTP isteklerini almak ve gerçek zamanlı olarak bazı şekilde olaylara yanıt mantıksal uygulamanızı sağlar.

  * [ **HTTP Web kancası** tetikleyici](../connectors/connectors-native-webhook.md) kaydederek hizmet uç noktası için abone olan bir *geri çağırma URL'si* bu hizmetle. 
  Belirtilen olay gerçekleştiğinde, tetikleyici hizmetin gerekmez, bu şekilde, hizmet yalnızca tetikleyici bildirebilir.

Tetikleyici harekete yeni veri ya da bir olay hakkında bildirim almak sonra yeni bir mantıksal uygulama iş akışı örneği oluşturur ve eylemleri iş akışı içinde çalıştırır. İş akışı boyunca tetikleyicinin herhangi bir veri erişebilirsiniz. Örneğin, "üzerinde yeni bir tweet" tetikleyicisi tweet içeriği çalıştırmak mantığı uygulamaya geçirir. 

## <a name="respond-to-triggers-and-extend-actions"></a>Tetikleyiciler için yanıt ve Eylemler genişletme

Sistemler ve bağlayıcılar yayımlanmamış hizmetler için mantıksal uygulamalar da genişletebilirsiniz.

* [Özel Tetikleyicileri veya eylemler oluşturun](../logic-apps/logic-apps-create-api-app.md)
* [İş akışı çalışmaya uzun süre çalışan eylemlerini ayarlama](../logic-apps/logic-apps-create-api-app.md)
* [Dış olayları ve eylemleri ile Web kancalarını yanıt](../logic-apps/logic-apps-create-api-app.md)
* [Çağrı, tetikleyici veya HTTP isteklerini zaman uyumlu yanıtlarını iş akışlarıyla iç içe](../logic-apps/logic-apps-http-endpoint.md)
* [Öğretici: dakika Logic Apps ve Power BI ile derleme olarak AI destekli bir Sosyal Panosu](http://aka.ms/logicappsdemo)
* [Video: Twilio SMS kancalarını yanıt ve metin yanıt gönderme](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Denetim akışı, hata işleme ve günlüğe kaydetme özellikleri

Logic apps koşullar, anahtarları, döngüler ve kapsamları gibi gelişmiş denetim akışı için zengin özellikleri içerir. Esnek çözümleri sağlamak için de hata ve özel durum, iş akışlarında işleme uygulayabilirsiniz. Bildirim ve iş akışı çalışma durumu için tanılama günlükleri için Azure mantıksal uygulamaları izleme ve uyarılar de sağlar.

* Temel alan farklı eylemler gerçekleştirmek [koşullu deyimler](../logic-apps/logic-apps-control-flow-conditional-statement.md) ve [switch ifadeleri](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Adımları veya işlem öğeler dizileri ve döngüler koleksiyonlarla yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Kapsamları birlikte grup eylemleri](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* [Yazar hata ve özel durum bir iş akışında işleme](../logic-apps/logic-apps-exception-handling.md)
* [Kullanım örneği: sağlık şirket mantığı uygulama özel durum HL7 FHIR iş akışları için işleme nasıl kullanır](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [İzleme, günlüğe kaydetme ve mevcut mantıksal uygulamaları için uyarıları Aç](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [İzleme ve tanılama günlük kaydı mantığı uygulamaları oluştururken Aç](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Mantıksal uygulamaları dağıtma ve yönetme

Tam olarak geliştirmek ve logic apps Visual Studio, Visual Studio Team Services veya diğer kaynak denetimi ve otomatik derleme araçları ile dağıtın. İş akışları ve kaynak şablonundaki bağımlı bağlantılar için dağıtım desteklemek için logic apps Azure kaynak dağıtım şablonları kullanın. Visual Studio Araçları, sürüm oluşturma için kaynak denetimine iade bu şablonları otomatik olarak oluşturur.

* [Visual Studio ile mantığı uygulamalar oluşturma ve dağıtma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [İzleme, günlüğe kaydetme ve mevcut mantıksal uygulamaları için uyarıları Aç](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Bir otomatik dağıtım şablonu oluşturma](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>İçerik türleri, dönüştürme ve çalışması içinde dönüşümleri

Erişim, dönüştürme ve Azure Logic Apps içinde birçok işlevini kullanarak birden çok içerik türleri dönüştürme [iş akışı tanımlama dili](http://aka.ms/logicappsdocs). Örneğin, bir dize, JSON ve XML ile arasındaki dönüştürebilirsiniz `@json()` ve `@xml()` iş akışı ifadeler. Logic Apps altyapısı kayıpsız bir şekilde hizmetleri arasında içerik aktarımını desteklemek için içerik türleri korur.

* [İş akışı ifadeleri logic apps içinde nasıl çalışır](../logic-apps/logic-apps-author-definitions.md)
* [JSON olmayan içerik türleri işlemek](../logic-apps/logic-apps-content-type.md)gibi `application/xml`, `application/octet-stream`, ve `multipart/formdata`
* [Azure mantıksal uygulamaları için iş akışı tanımlama dili şeması](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Diğer tümleştirmeler ve özellikleri

Logic apps, Azure işlevleri, Azure API Management, Azure App Services ve özel HTTP uç noktaları, örneğin, REST ve SOAP gibi birçok Hizmetleri ile tümleştirme de sunar.

* [Gerçek zamanlı sosyal Pano Azure sunucusuz ile oluşturma](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Mantığı uygulamalardan Azure işlevlerini çağırma](../logic-apps/logic-apps-azure-functions.md)
* [Öğretici: Tetikleyici logic apps ile Azure işlevleri](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Öğretici: Azure olay kılavuz ve Logic Apps ile sanal makine değişikliklerini izleme](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Öğretici: Azure Logic Apps ile tümleşen bir işlev oluşturun ve Twitter analiz etmek için Microsoft Bilişsel hizmetler düşünceleri gönderin](../azure-functions/functions-twitter-email.md)
* [Öğretici: IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanma Azure Logic Apps ile bildirimleri](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: Çağrı mantığı uygulamalardan SOAP uç noktası](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Uçtan uca senaryolar

* [Teknik İnceleme: Uçtan uca durum yönetimi Logic Apps gibi Azure hizmetleriyle tümleştirme](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Müşteri hikayeleri

Azure mantıksal uygulamaları, diğer Azure Hizmetleri ve Microsoft ürünleri ile birlikte nasıl yardımcı olduğunu öğrenin [bu şirketlerin](https://aka.ms/logic-apps-customer-stories) kendi çeviklik geliştirmek ve odak Çekirdek işlerini basitleştirme, düzenleme, otomatikleştirme ve yönetme hakkında karmaşık işlemlere.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama tanımları JSON ile derleme](../logic-apps/logic-apps-author-definitions.md)
* [Hataları ve logic apps içinde özel durumları işleme](../logic-apps/logic-apps-exception-handling.md)
* [Açıklamalar, sorular, geri bildirim veya Azure mantıksal uygulamaları geliştirmek için öneriler gönderme](https://feedback.azure.com/forums/287593-logic-apps)