---
title: Azure sunucusuz genel bakış | Microsoft Docs
description: Altyapı hakkında endişelenmeden bulutta güçlü çözümler oluşturma hakkında bilgi edinin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 9cd1946d8fa670764bcc95b82298d07cc70417bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60684117"
---
# <a name="overview-azure-serverless-with-azure-logic-apps-and-azure-functions"></a>Genel Bakış: Azure Logic Apps ve Azure işlevleri ile sunucusuz Azure

[Sunucusuz](https://azure.microsoft.com/solutions/serverless/) uygulamalar geliştirme hızını artış, gerekli kod ve Basitlik ölçek azaltma avantajlarını sağlar.  Bu makalede, sunucusuz çözümler ve Azure sunucusuz teklifleri ' farklı özniteliklerini gider.

## <a name="what-is-serverless"></a>Sunucusuz nedir?

Sunucusuz, sunucu yok - yalnızca Geliştirici sunucuları hakkında endişelenmenize gerek yok anlamına gelmez.  Geleneksel uygulama geliştirme büyük bir bölümünü sorular ölçeklendirme, barındırma ve izleme çözümleri uygulamanın gereksinimlerini karşılayacak şekilde yanıt.  Sunucusuz ile bu soruları çözümün bir parçası dikkate.  Ayrıca, sunucusuz uygulamalar, tüketim temelli bir plan üzerinde faturalandırılır.  Uygulamanın hiçbir zaman kullanılmaz, bir ücret hiçbir zaman oluşur.  Bu özellikler, geliştiricilerin yalnızca çözümün iş mantığına odaklanabilir olanak sağlar.

Azure'da sunucusuz geçici Çekirdek Hizmetleri [Azure işlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Hem bu çözümlerin yukarıdaki ilkelerine uyduğunuzdan ve olabildiğince az kodla sağlam bulut uygulamaları oluşturmak üzere geliştiricilere sağlar.

## <a name="what-are-azure-functions"></a>Azure işlevleri nedir?

Azure İşlevleri, küçük kod parçalarını veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Tüm uygulama veya bunu çalıştıracak altyapı hakkında endişelenmeden elinizdeki sorun için ihtiyacınız olan kodu yazabilirsiniz. İşlevleri yapabilirsiniz geliştirme sürecinizi daha da verimli ve gibi tercih ettiğiniz geliştirme dilini kullanabilir C#, F#, Node.js, Python veya PHP. Yalnızca kodunuzun çalıştığı süre için ödeme yaparsınız ve gerektiğinde Azure ölçeklendirir.

Azure İşlevlerini kullanmaya hemen başlamak isterseniz [İlk Azure İşlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md) ile başlayın. İşlevler hakkında daha teknik bilgi arıyorsanız bkz. [geliştirici başvurusu](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Azure Logic Apps nedir?

Azure Logic Apps basitleştirmek ve bulutta ölçeklenebilir tümleştirmeleri ve iş akışları uygulamak için bir yol sağlar. Bu model ve bir dizi olarak adlandırılan bir iş akışı adımları olarak işleminizi otomatikleştirmek için bir görsel tasarımcı sağlar.  Vardır [çok sayıda bağlayıcı](../connectors/apis-list.md) sunucusuz bir uygulama için diğer API'ler hızlı bir şekilde bağlanmak için bulut ve şirket içi hizmetler arasında.  Mantıksal uygulama bir tetikleyici ile başlar ('Dynamics CRM’e bir hesabın eklenmesi' gibi) ve başlatma sonrasında çok sayıda birleştirme eylemi, dönüştürme ve koşul mantığı başlayabilir.  Özellikle bir dış sistem veya API ile etkileşim kurma işlemi gerektirmesi durumunda farklı Azure işlevleri'nde bir işlem - işlemlerini zamandır, Logic Apps harika bir seçim kullanır.

Logic Apps'i kullanmaya başlamak için başlayın [ilk mantıksal uygulamanızı oluşturma](quickstart-create-first-logic-app-workflow.md).  Logic Apps hakkında daha fazla teknik bilgi arıyorsanız bkz [Geliştirici Başvurusu](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Nasıl oluşturmak ve azure'da sunucusuz uygulamalar dağıtma?

Azure, geliştirme, dağıtım ve yönetimini sunucusuz uygulamalar arasında zengin bir araç sağlar.  Doğrudan Azure portalında veya ile uygulamaları derlenebilir [araçları Visual Studio'dan](logic-apps-serverless-get-started-vs.md).  Bir uygulamayı geliştirildikten sonra olabilir [anında dağıtılan](logic-apps-create-deploy-template.md).  Azure sunucusuz uygulamalar için izleme de sağlar.  Bu izleme Azure portalı, API veya SDK'ları aracılığıyla veya tümleşik Araçlar ile Azure İzleyici günlüklerine ve Application Insights için erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da sunucusuz bir uygulama oluşturmaya başlayın](logic-apps-serverless-get-started-vs.md)
* [İle sunucusuz bir müşteri öngörüleri panosu oluşturma](logic-apps-scenario-social-serverless.md)
* [Mantıksal uygulama için bir dağıtım şablonu oluşturma](logic-apps-create-deploy-template.md)
