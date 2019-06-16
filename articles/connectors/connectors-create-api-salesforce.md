---
title: Azure Logic Apps'ten Salesforce'a Bağlan | Microsoft Docs
description: Görevler ve izleme, oluşturma ve Azure Logic Apps kullanarak Salesforce kayıtları ve işlerini yönetmek, iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 292d517f2c99974f4674a4c94472a0a320320ce4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106024"
---
# <a name="monitor-create-and-manage-salesforce-resources-by-using-azure-logic-apps"></a>İzleme, oluşturma ve Azure Logic Apps'ı kullanarak Salesforce kaynaklarını yönetme

Azure Logic Apps ve Salesforce Bağlayıcısı ile otomatik görevler ve iş akışları, Salesforce gibi kaynaklar için kayıt, işler ve nesneler, örneğin oluşturabilirsiniz:

* Kayıt oluşturulduğunda veya değiştirildiğinde zamana yönelik İzleyici. 
* Oluşturma, get ve işler ve kayıt dahil olmak üzere INSERT, update yönetme ve silme eylemlerini.

Salesforce'tan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir hale getirmek Salesforce Tetikleyicileri kullanabilirsiniz. Logic apps eylemleri, Salesforce kaynaklarla görevleri gerçekleştirmek için kullanabilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* A [Salesforce hesabı](https://salesforce.com/)

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Salesforce hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Salesforce tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Salesforce eylem kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlayın, **yinelenme** tetikleyici.

## <a name="connect-to-salesforce"></a>Salesforce'a Bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Boş mantıksal uygulama için arama kutusuna filtreniz olarak "salesforce" girin. 
   Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

     veya

   * Var olan mantıksal uygulamalar, bir eylem eklemek istediğiniz adımı altında seçin için **yeni adım**. Arama kutusuna filtreniz olarak "salesforce" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. Salesforce oturum açmanız istenirse, şimdi oturum açın ve erişim izni.

   Mantıksal uygulamanızı Salesforce bağlantı oluşturun ve verilerinize erişmek için kimlik bilgilerinizi yetkilendirin.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/salesforce/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)