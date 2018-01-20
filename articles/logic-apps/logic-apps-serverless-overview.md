---
title: "Azure sunucusuz genel bakış | Microsoft Docs"
description: "Güçlü çözümler, altyapı hakkında düşünmek zorunda kalmadan bulutta oluşturun."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5cc6837ed0b0f4467e48c736f5d596a51a799fae
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-azure-serverless-with-functions-and-logic-apps"></a>Azure işlevleri ve Logic Apps ile sunucusuz genel bakış

Sunucusuz uygulamaları geliştirme hızını artış, gerekli kod ve Basitlik ölçekli azalma avantajları sunar.  Bu makalede sunucusuz çözümleri ve Azure sunucusuz teklifleri ' farklı özniteliklerini gider.

## <a name="what-is-serverless"></a>Sunucusuz nedir?

Sunucusuz sunucu yok - yalnızca Geliştirici sunucuları hakkında endişelenmeniz gerekmez anlamına gelmez.  Geleneksel uygulama geliştirme büyük bir bölümünü ölçeklendirme, barındırma ve uygulama taleplerini karşılamak üzere çözümlerini izleme çevresinde sorulara yanıt verilmesi.  Sunucusuz ile bu soruları çözümün bir parçası dikkate.  Ayrıca, sunucusuz uygulamaları tüketim tabanlı bir plana göre faturalandırılır.  Uygulama hiçbir zaman kullanılırsa, bir ücret hiçbir zaman oluşur.  Bu özellikler yalnızca çözümün iş mantığına odaklanmaya geliştiriciler sağlar.

Çekirdek hizmetler sunucusuz geçici azure'da [Azure işlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Hem bu çözümlerin yukarıdaki ilkeleri izleyin ve geliştiricilerin çok az kod sağlam bulut uygulamalarıyla oluşturmasına olanak sağlar.

## <a name="what-are-azure-functions"></a>Azure işlevleri nelerdir?

Azure İşlevleri, küçük kod parçalarını veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Tüm uygulama veya bunu çalıştıracak altyapı hakkında endişelenmeden elinizdeki sorun için ihtiyacınız olan kodu yazabilirsiniz. Geliştirme işlevlerini daha da verimli hale getirebilir ve C#, F #, Node.js, Python veya PHP gibi tercih ettiğiniz geliştirme dilini kullanabilirsiniz. Yalnızca kodunuzun çalıştığı zaman için ödeme ve gerektiğinde Azure ölçeklendirir.

Azure İşlevlerini kullanmaya hemen başlamak isterseniz [İlk Azure İşlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md) ile başlayın. İşlevler hakkında daha teknik bilgi arıyorsanız bkz. [geliştirici başvurusu](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Azure Logic Apps nedir?

Azure mantıksal uygulamaları için ölçeklenebilir tümleştirmeler ve iş akışları basitleştirmek ve bulutta bir yol sağlar. Model ve işleminizi iş akışı adlı adımları bir dizi olarak otomatikleştirmek için bir görsel tasarımcı sağlar.  Vardır [birçok Bağlayıcılar](../connectors/apis-list.md) sunucusuz bir uygulama için diğer API'leri hızlı bir şekilde bağlanmak için bulut ve şirket içi hizmetler arasında.  Mantıksal uygulama bir tetikleyici ile başlar ('Dynamics CRM’e bir hesabın eklenmesi' gibi) ve başlatma sonrasında çok sayıda birleştirme eylemi, dönüştürme ve koşul mantığı başlayabilir.  Özellikle bir dış sistem veya API ile etkileşim işlemi gerektirdiğinde, bir işlemde - farklı Azure işlevleri yönetme olduğunda, Logic Apps harika bir seçim kullanır.

Logic Apps ile çalışmaya başlamak için başlayın [ilk mantıksal uygulamanızı oluşturma](quickstart-create-first-logic-app-workflow.md).  Logic Apps hakkında daha fazla teknik bilgi arıyorsanız bkz [Geliştirici Başvurusu](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Nasıl derleme ve Azure sunucusuz uygulamalar dağıtmak?

Azure, geliştirme, dağıtımı ve sunucusuz uygulamaların Yönetimi zengin birtakım araçlar sağlar.  Uygulamaları Azure Portalı'ndaki doğrudan ya da ile oluşturulabilen [Visual Studio'dan tooling](logic-apps-serverless-get-started-vs.md).  Bir uygulama geliştirilen bir kez olabilir [anında dağıtılan](logic-apps-create-deploy-template.md).  Azure sunucusuz uygulamaları için izleme de sağlar.  Bu izleme Azure portalından, API veya SDK aracılığıyla veya tümleşik araçları ile OMS ve Application Insights için erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da sunucusuz bir uygulama oluşturmaya başlayın](logic-apps-serverless-get-started-vs.md)
* [Bir müşteri öngörüleri Panosu sunucusuz ile oluşturma](logic-apps-scenario-social-serverless.md)
* [Bir dağıtım şablonu için bir mantıksal uygulama oluşturma](logic-apps-create-deploy-template.md)