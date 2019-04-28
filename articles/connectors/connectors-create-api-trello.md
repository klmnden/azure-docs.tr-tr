---
title: Azure Logic Apps'ten Trello'ya bağlanın | Microsoft Docs
description: Görevler ve izleme ve Azure Logic Apps kullanarak listeleri, panoları ve projelerinizi Trello kartları yönetme iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: fe7a4377-5c24-4f72-ab1a-6d9d23e8d895
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 31357fa313cfa31c0f8a90c0f7722f627e4394d1
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62106176"
---
# <a name="monitor-and-manage-trello-with-azure-logic-apps"></a>İzleme ve Trello Azure Logic Apps ile yönetme

Azure Logic Apps ve Trello Bağlayıcısı ile otomatik görevler ve iş akışlarını izlemek ve Trello listesi, kartlar, panoları, takım üyeleri ve benzeri, örneğin yönetmek oluşturabilirsiniz:

* Panoları ve listeleri için yeni kart eklendiğinde İzleyici. 
* Oluşturma, alma ve panoları, kartlar ve listeleri yönetin.
* Kartlar, açıklamalar ve üyeleri ekleyin.
* Panoları, Pano etiketleri, panoları, kart açıklamaları, kart üyeleri, takım üyeleri ve üyesi olduğunuz takımlar kartları listeleyin. 
* Ekipler yararlanabilir.

Trello hesabınızdan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir Tetikleyicileri kullanabilirsiniz. Trello hesabınızla görevleri gerçekleştiren eylemlerini kullanabilirsiniz. Trello eylemleri çıktısını kullanan diğer eylemler de olabilir. Örneğin, Pano veya listesine yeni kart eklendiğinde Slack bağlayıcısıyla iletileri gönderebilir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Trello hesabı ve kullanıcı bilgilerinizi

  Mantıksal uygulamanızı bir bağlantı oluşturun ve Trello hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Trello hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Trello tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir Trello eylemi kullanmak için mantıksal uygulamanızın bir tetikleyici ile örneğin başlatın, **yinelenme** tetikleyici.

## <a name="connect-to-trello"></a>Trello'ya bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "trello" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   -veya-

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**. 
   Arama kutusuna filtreniz olarak "trello" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

1. Trello için oturum açmanız istenirse, mantıksal uygulamanız için erişimi yetkilendirmeniz ve oturum açın.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/trello/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)