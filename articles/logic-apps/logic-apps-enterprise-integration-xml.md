---
title: B2B Kurumsal tümleştirme - Azure Logic Apps için XML iletileri | Microsoft Docs
description: İşlem, doğrulama, dönüştürme ve Azure Logic Apps Enterprise Integration Pack ile B2B çözümleri için XML iletileri zenginleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.date: 02/27/2017
ms.openlocfilehash: 5f804bf656b423d6dbe8f5c2ed73ba54d8361000
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124544"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML iletileri ve Azure Logic Apps Enterprise Integration Pack ile düz dosya

Logic apps'ı kullanarak, alma ve gönderme işleminin XML iletileri yeteneğine sahip. Bu özellik ile Enterprise Integration Pack dahil edilir. BizTalk Server arka plana sahip bu kullanıcılar için Enterprise Integration Pack, dönüştürme ve iletileri doğrulamak, düz dosyalar ile çalışma ve hatta zenginleştirin veya belirli özellikleri iletiden ayıklamak için XPath kullanın benzer yetenekler sağlar. 

Bu alan için yeni olan kullanıcılar, bu özellikler, iş akışı içinde iletileri işleminin nasıl genişletin. İşletmeler arası senaryoda ve Enterprise Integration Pack geliştirmek için kullanabileceğiniz sonra belirli XML şemalarıyla çalışmak gibi nasıl şirketiniz bu iletileri işler. 

Enterprise Integration Pack içerir: 

* [XML doğrulaması](logic-apps-enterprise-integration-xml-validation.md "XML ileti doğrulama hakkında bilgi edinin") -bir gelen veya giden XML iletisi belirli bir şemaya karşı doğrulama.
* [XML dönüştürme](../logic-apps/logic-apps-enterprise-integration-transform.md "XML ileti dönüşümleri ve eşlemeleri hakkında bilgi edinin") - Dönüştür veya bir XML iletisi gereksinimlerinizi veya iş ortağı gereksinimlerine göre özelleştirin.
* [Düz dosya kodlama ve düz dosya kodu çözme](logic-apps-enterprise-integration-flatfile.md "düz dosya kodlama/kod çözme hakkında bilgi edinin") - kodlayın veya düz dosya kodunu çözme. Örneğin, SAP kabul eder ve düz dosya biçiminde IDOC dosyaları gönderir. XML iletileri, Logic Apps dahil olmak üzere birçok tümleştirme platformu oluşturun. Bu nedenle, düz dosya Kodlayıcısı "düz dosyaları için XML dosyalarını dönüştürmek için" kullanan bir mantıksal uygulama oluşturabilirsiniz. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx) - bir ileti zenginleştirin ve belirli özellikler iletiden ayıklayın. Sonra bir hedef veya aracı bir uç nokta ileti yönlendirmek için ayıklanan özelliklerini kullanabilirsiniz.

## <a name="try-it-out"></a>Deneyin
[Tam olarak işlevsel bir mantıksal uygulama dağıtma ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub örneği) XML özelliklerini kullanarak Azure Logic Apps'te.

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")
