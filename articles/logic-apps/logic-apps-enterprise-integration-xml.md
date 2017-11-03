---
title: "XML iletileri iş akışlarında - Azure Logic Apps ile çalışma | Microsoft Docs"
description: "İşlem, doğrulama, dönüştürme ve mantıksal uygulamalar ve iş XML iletileri zenginleştirmek-Kurumsal tümleştirme paketi kullanan senaryolar için"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 3fec4935f5317be4bf8c9e05f1c24a7c05381b1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a>Doğrulama ve XML dönüştürme, kodlama ve düz dosyalar kod çözme ve logic apps iletilerin özelliklerinde zenginleştirmek

Logic apps'ı kullanarak, gönderme ve alma işleminin XML iletileri yeteneğine sahip. Bu özellik kuruluş tümleştirme paketi dahil edilir. BizTalk Server arka plana sahip olan kullanıcılar için Kurumsal tümleştirme paketi, dönüştürme ve iletileri doğrulamak için düz dosyalarla ve hatta zenginleştirmek veya belirli özellikleri iletiden ayıklamak için XPath kullanın benzer yetenekler sağlar. 

Bu alan için yeni kullanıcılar bu özellikler, iş akışı içinde iletileri işleminin nasıl genişletin. İşletmeler için senaryoda ve kurumsal tümleştirme paketi geliştirmek için kullanabileceğiniz sonra belirli XML şemaları ile çalışma, örneğin, nasıl şirketiniz bu iletileri işler. 

Enterprise Integration Pack içerir: 

* [XML doğrulaması](logic-apps-enterprise-integration-xml-validation.md "XML ileti doğrulama hakkında daha fazla bilgi") -gelen veya giden XML iletisine belirli bir şemaya karşı doğrulama.
* [XML dönüştürme](../logic-apps/logic-apps-enterprise-integration-transform.md "XML ileti dönüşümleri ve eşlemeleri hakkında bilgi edinin") - Dönüştür veya gereksinimlerinizi veya bir iş ortağı gereksinimlerine göre bir XML iletisini özelleştirme.
* [Düz dosya kodlama ve kod çözme düz dosya](logic-apps-enterprise-integration-flatfile.md "düz dosya kodlama/kod çözme hakkında bilgi edinin") - kodlamak veya düz dosyadır kodunu çözer. Örneğin, SAP kabul eder ve düz dosya biçiminde IDOC dosyaları gönderir. Birçok tümleştirme platformda Logic Apps dahil olmak üzere, XML iletileri oluşturun. Bu nedenle, "düz dosyalara XML dosyaları dönüştürmek için" düz dosya Kodlayıcısı kullanan bir mantıksal uygulama oluşturabilirsiniz. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx) - bir ileti zenginleştirmek ve belirli özellikler iletiden ayıklayın. Sonra bir hedef veya aracı bir uç nokta ileti yönlendirmek için ayıklanan özelliklerini kullanabilirsiniz.

## <a name="try-it-out"></a>Deneyin
[Tam olarak işlevsel mantıksal uygulama dağıtma ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub örneği) XML özelliklerini Azure Logic Apps içinde kullanarak.

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")
