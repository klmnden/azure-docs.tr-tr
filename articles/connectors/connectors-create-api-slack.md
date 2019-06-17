---
title: Azure Logic Apps'ten için Slack bağlayın | Microsoft Docs
description: Görevler ve dosyaları izlemek ve Azure Logic Apps kullanarak kanalları, grupları ve iletileri Slack hesabınızı yönetmek, iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 675e37120b06af3add58b564495f22875647a0fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105658"
---
# <a name="monitor-and-manage-slack-with-azure-logic-apps"></a>İzleme ve Slack Azure Logic Apps ile yönetme

Azure Logic Apps ve Slack Bağlayıcısı ile otomatik görevler ve Slack dosyalarınızı izleme ve Slack kanalı, iletileri, grupları ve benzeri, yönetme, örneğin iş akışları oluşturabilirsiniz:

* Yeni dosyalar oluşturulur zamana yönelik İzleyici.
* Oluşturun ve kanalları katılın 
* İletiler yayınlayın.
* Grupları oluşturma ve ayarlama rahatsız etmeyin.

Slack hesabınızdan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir Tetikleyicileri kullanabilirsiniz. Slack hesabınızı kullanarak görevleri gerçekleştiren eylemlerini kullanabilirsiniz. Ayrıca, Slack eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, yeni bir dosya oluşturulduğunda Office 365 Outlook Bağlayıcısı ile e-posta gönderebilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* [Slack](https://slack.com/) hesabı ve kullanıcı kimlik bilgileri

  Mantıksal uygulamanızı bir bağlantı oluşturun ve Slack hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Slack hesabınıza erişmek için istediğiniz mantıksal uygulaması. Slack bir tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir Slack eylemi kullanmak için mantıksal uygulamanızı Slack bir tetikleyici veya başka bir tetikleyici gibi bir tetikleyici ile gibi başlatın **yinelenme** tetikleyici.

## <a name="connect-to-slack"></a>Slack için Bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "slack" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   veya

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**. 
   Arama kutusuna filtreniz olarak "slack" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Slack için oturum açmanız istenirse, Slack, çalışma alanına oturum açın. 

   ![Çalışma alanı Slack oturum açın](./media/connectors-create-api-slack/slack-sign-in-workspace.png)

1. Mantıksal uygulamanız için erişimi yetkilendirin.

   ![Slack erişim yetkisi verme](./media/connectors-create-api-slack/slack-authorize-access.png)

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın. Mantıksal uygulamanızın iş akışı oluşturmaya devam etmek için daha fazla eylem ekleme.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/slack/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
