---
title: Örnekleri ve senaryoları - Azure Logic Apps | Microsoft Docs
description: Örnekler, senaryolar, öğreticiler ve Kılavuzlar Azure Logic Apps için
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.date: 01/31/2018
ms.openlocfilehash: 89e0294db3178cedd3b14aada0b505787b17c75e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303698"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Yaygın senaryolar, örnekler, öğreticiler ve Kılavuzlar Azure Logic Apps için

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) düzenleyin ve sağlayarak farklı Hizmetleri Tümleştirme yardımcı olan [100'den fazla kullanıma hazır Bağlayıcılar](../connectors/apis-list.md)Microsoft Bilişsel hizmetler için şirket içi SQL Server veya SAP kadar. Logic Apps hizmetinin "sunucusuz" olduğundan ölçek veya örnekleri hakkında endişelenmeniz gerekmez. Tek yapmanız gereken olan iş akışı tetikleyici ve iş akışı gerçekleştirdiği eylemleri tanımlayın. Temel platform, Ölçek, kullanılabilirlik ve performans işler. Logic Apps, kullanım örnekleri ve senaryoları, birden çok sistem üzerinde birden fazla eylemlerini koordine gerek duyduğunuz senaryolara için özellikle yararlıdır.

Birçok desenleri ve özellikler hakkında daha fazla bilgi edinmenize yardımcı olmak için [Azure Logic Apps](../logic-apps/logic-apps-overview.md) destekleyen ortak örnekler ve senaryolar aşağıda verilmiştir.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Başlangıç noktaları mantıksal uygulama iş akışları için popüler

Her mantıksal uygulama ile başlayan bir [ *tetikleyici*](../logic-apps/logic-apps-overview.md#logic-app-concepts)ve yalnızca bir tetikleyici, mantıksal uygulama iş akışınızı başlatan ve Bu tetikleyici bir parçası olarak tüm verileri geçirir. Bazı bağlayıcılar, hangi bu türlerinde Tetikleyiciler sağlar:

* *Yoklama Tetikleyicileri*: Bir hizmet uç noktası yeni veriler için düzenli olarak denetler. Yeni veriler mevcut olduğunda, bir tetikleyici oluşturur ve yeni bir iş akışı örneği girdi olarak verilerle çalışır.

* *Anında iletme Tetikleyicileri*: Hizmet uç noktası verileri dinler ve belirli bir olay meydana kadar bekler. Olay meydana geldiğinde, Tetikleyici oluşturma ve çalıştırma giriş olarak kullanılabilir tüm verileri kullanan yeni bir iş akışı örneği hemen etkinleştirilir.

Yalnızca birkaç popüler bir tetikleyici örnekleri aşağıda verilmiştir:

* Yoklama: 

  * [**Zamanlama - yinelenme** tetikleyici](../connectors/connectors-native-recurrence.md) , başlangıç tarihi ve saati artı mantıksal uygulamanız tetikleme için yineleme ayarlamanıza olanak tanır. 
  Örneğin, haftanın günleri ve saatleri mantıksal uygulamanız tetikleme günün seçebilirsiniz.

  * Mantıksal uygulamanız yeni e-posta, örneğin, Logic Apps tarafından desteklenen herhangi bir posta sağlayıcıdan gelen denetle "E-posta alındığında" tetikleyici sağlar [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/)ve benzeri.

  * [ **HTTP** tetikleyici](../connectors/connectors-native-http.md) belirtilen hizmet uç noktası, HTTP üzerinden iletişim kurarak denetleyin mantıksal uygulamanızı sağlar.
  
* Anında iletme:

  * [ **İstek / yanıt - istek** tetikleyici](../connectors/connectors-native-reqres.md) mantıksal uygulamanız HTTP isteklerini alır ve gerçek zamanlı bir şekilde olayları yanıtlamanıza olanak tanır.

  * [ **HTTP Web kancası** tetikleyici](../connectors/connectors-native-webhook.md) hizmet uç noktası'na kaydederek abone olan bir *geri çağırma URL'si* bu hizmetle. 
  Belirtilen olay meydana geldiğinde, tetikleyici hizmetin yoklama gerekmez, bu şekilde, hizmet yalnızca tetikleyici bildirebilir.

Tetikleyiciyi harekete yeni veriler veya bir olay hakkında bir bildirim alma sonra yeni bir mantıksal uygulama iş akışı örneği oluşturur ve iş akışında eylemleri çalıştıran. İş akışı boyunca tetikleyiciden herhangi bir veri erişebilirsiniz. Örneğin, "üzerinde yeni bir tweet" tetikleyicisi mantıksal uygulama çalıştırması tweet içeriği geçirir. 

## <a name="respond-to-triggers-and-extend-actions"></a>Tetikleyicilere yanıt vermeye ve eylemleri genişletir

Logic apps, sistemleri ve bağlayıcılar yayımlamadınız hizmetler için de genişletebilirsiniz.

* [Özel Tetikleyiciler veya Eylemler oluşturma](../logic-apps/logic-apps-create-api-app.md)
* [İş akışı çalıştırmaları için uzun süre çalışan eylemlerin ayarlama](../logic-apps/logic-apps-create-api-app.md)
* [Dış olaylar ve Eylemler ile Web kancalarını yanıt](../logic-apps/logic-apps-create-api-app.md)
* [Çağrı, tetikleyici veya iç içe HTTP isteklerine yanıt veren zaman uyumlu iş akışları](../logic-apps/logic-apps-http-endpoint.md)
* [Öğretici: Logic Apps ve Power BI ile dakikalar içinde bir yapay ZEKA destekli bir Sosyal Panosu derleme](https://aka.ms/logicappsdemo)
* [Video: Yanıt için Twilio SMS Web kancaları ve metin yanıt gönderin](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Denetim akışı, hata işleme ve günlüğe kaydetme özellikleri

Mantıksal uygulamalar, koşullar, anahtarları, döngüler ve kapsamları gibi gelişmiş denetim akışı için zengin özellikler içerir. Esnek çözümler emin olmak için ayrıca hata ve özel durum işleme akışlarınızı uygulayabilirsiniz. Bildirim ve iş akışı çalıştırma durumu için tanılama günlükleri, Azure Logic Apps izleme ve uyarılar da sağlar.

* Göre farklı eylemler gerçekleştirmesini [koşullu deyimler](../logic-apps/logic-apps-control-flow-conditional-statement.md) ve [geçiş deyimleri](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Adımları veya işlem öğelerin diziler ve Koleksiyonlar ile döngüleri yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Grup eylemleri kapsamları ile birlikte](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* [Hata yazarın ve özel durum işleme bir iş akışında](../logic-apps/logic-apps-exception-handling.md)
* [Kullanım örneği: Bir sağlık şirketinin mantıksal uygulama özel durum işleme HL7 FHIR iş akışları kullanma](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [İzleme, günlüğe kaydetme ve var olan mantıksal uygulamalar için uyarıları Aç](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Mantıksal uygulama oluşturma izleme ve tanılama günlüğünü açma](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Mantıksal uygulamaları dağıtma ve yönetme

Tam olarak, geliştirin ve Visual Studio, Azure DevOps veya diğer kaynak denetimi ve otomatik derleme araçları ile mantıksal uygulamaları dağıtın. Dağıtım iş akışları ve kaynak şablonu bağımlı bağlantıları desteklemek için logic apps, Azure kaynak dağıtım şablonlarını kullanın. Visual Studio Araçları, sürüm oluşturma için kaynak denetimine iade bu şablonları otomatik olarak oluşturur.

* [Visual Studio ile mantıksal uygulamaları oluşturun ve dağıtın](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [İzleme, günlüğe kaydetme ve var olan mantıksal uygulamalar için uyarıları Aç](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Bir otomatik dağıtım şablonu oluşturma](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>İçerik türleri, dönüştürme ve çalışması içinde dönüşümleri

Erişim, dönüştürün ve Azure Logic Apps'te birçok işlevleri kullanarak birden çok içerik türüne dönüştürme [iş akışı tanımlama dili](https://aka.ms/logicappsdocs). Örneğin, bir dize, JSON ve XML arasında dönüştürme yapabilirsiniz `@json()` ve `@xml()` ifadeleri iş akışı. Logic Apps altyapısı, içerik aktarımı kayıpsız bir şekilde hizmetler arasında desteklemek için içerik türleri korur.

* [İş akışı ifadeleri logic apps'te nasıl çalışır?](../logic-apps/logic-apps-author-definitions.md)
* [JSON olmayan içerik türlerini işleme](../logic-apps/logic-apps-content-type.md)gibi `application/xml`, `application/octet-stream`, ve `multipart/formdata`
* [Azure Logic Apps iş akışı tanımı dil şeması](https://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Diğer tümleştirmeler ve özellikleri

Mantıksal uygulamalar, Azure işlevleri, Azure API Management, Azure uygulama hizmetleri ve özel HTTP uç noktalarını, örneğin, REST ve SOAP gibi çok sayıda hizmet ile tümleştirme de sunar.

* [Azure sunucusuz ile gerçek zamanlı bir sosyal Pano oluşturma](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Mantıksal uygulamaları Azure işlevleri çağırma](../logic-apps/logic-apps-azure-functions.md)
* [Öğretici: Azure işlevleri ile tetiklenen logic apps](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Öğretici: Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Öğretici: Twitter gönderi düşüncelerini çözümleme için Azure Logic Apps ve Microsoft Bilişsel hizmetler ile tümleşen bir işlev oluşturma](../azure-functions/functions-twitter-email.md)
* [Öğretici: IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanan Azure Logic Apps ile bildirimleri](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: Logic apps'ten SOAP uç noktası çağrısı](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Uçtan uca senaryolar

* [Teknik İnceleme: Logic Apps gibi Azure hizmetleriyle uçtan uca servis talebi yönetimi tümleştirmesi](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Müşteri hikayeleri

Azure Logic Apps, diğer Azure Hizmetleri ve Microsoft ürünlerinin yanı sıra nasıl yardımcı olduğunu öğrenin [bu şirketler](https://aka.ms/logic-apps-customer-stories) kendi süreçlerinin çevikliğini artırın ve odak işletmelerin basitleştirme, düzenleme, otomatikleştirme ve düzenleme karmaşık işlemleri.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama tanımları JSON ile geliştirin](../logic-apps/logic-apps-author-definitions.md)
* [Hataları ve logic apps'te özel durumları işleme](../logic-apps/logic-apps-exception-handling.md)
* [Açıklamalar, sorular, geri bildirim ya da Azure Logic Apps artırmak için önerileri gönderin](https://feedback.azure.com/forums/287593-logic-apps)
