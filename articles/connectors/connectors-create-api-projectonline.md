---
title: Azure Logic Apps'ten Project Online'a bağlanma | Microsoft Docs
description: İzleme, oluşturma ve Azure Logic Apps kullanarak Project Online projeleri, görevler ve kaynakların yönetme iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.assetid: 40ce621e-4925-4653-93bb-71ab9abcbdf1
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 663363d05c1875d22a0ecc0478abcf7e0ec89c99
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105639"
---
# <a name="manage-project-online-projects-tasks-and-resources-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Project Online projeleri, görevleri ve kaynakları yönetme

Azure Logic Apps ile Project Online Bağlayıcısı, Office 365 aracılığıyla Project Online otomatik görevler ve projeleri, görevleri ve kaynaklar için iş akışları oluşturabilirsiniz. İş akışlarınızı bu eylemleri ve diğer, örneğin gerçekleştirebilirsiniz:

* Yeni projeler, görevler veya kaynakları oluşturulan zamana yönelik İzleyici. Veya yeni projeler yayımlanan zamana yönelik İzleyici.
* Yeni projeler, görevleri ve kaynakları oluşturun.
* Var olan projeleri veya görevleri listeleyin.
* Gözden geçirin, iade ve projelerini yayımlayamazsınız.

Project Online, planlamak, önceliklendirmek ve projeleri ve proje portföyü yatırımlarını gelen neredeyse her yerden hemen her CİHAZDAN güçlü proje yönetimi özellikleri sunarak değişiklikleri yönetmenize yardımcı olur. Project Online Tetikleyiciler Project Online'dan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir hale getirmek için kullanabilirsiniz. Logic apps eylemleri, Project Online'da çeşitli görevleri gerçekleştirmek için kullanabilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Project Online aracılığıyla bir [Office 365 hesabı](https://www.office.com/), 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Project Online verilerinize erişmek için istediğiniz mantıksal uygulaması. Project Online bir tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Project Online eylemlerini kullanmak için mantıksal uygulamanız başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

## <a name="connect-to-project-online"></a>Project Online'a bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Boş mantıksal uygulama için arama kutusuna filtreniz olarak "Project Online" yazın. 
   Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

     -veya-

   * Var olan mantıksal uygulamalar, bir eylem eklemek istediğiniz adımı altında seçin için **yeni adım**. Arama kutusuna filtreniz olarak "Project Online" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. Project Online için oturum açmanız istenirse, şimdi oturum açın.

   Mantıksal uygulamanızı Project Online bağlantı oluşturun ve verilerinize erişmek için kimlik bilgilerinizi yetkilendirin.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/projectonline/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)