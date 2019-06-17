---
title: Azure Logic Apps'ten Sendgrid'e Bağlan | Microsoft Docs
description: Görevleri ve e-posta gönderin ve Azure Logic Apps kullanarak SendGrid, posta listelerini yönetmek, iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: bc4f1fc2-824c-4ed7-8de8-e82baff3b746
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 7eecd3908883b195b52755d03e70872afe9180bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105743"
---
# <a name="send-emails-and-manage-mailing-lists-in-sendgrid-by-using-azure-logic-apps"></a>E-posta Gönder ve Azure Logic Apps kullanarak SendGrid, posta listelerini yönetin

Azure Logic Apps ve SendGrid Bağlayıcısı ile otomatik görevler ve e-posta Gönder ve örneğin, alıcı listelerini yönetmek, iş akışları oluşturabilirsiniz:

* E-posta gönderin.
* Alıcı listesine ekleyin.
* Genel gizlemeyi yönetmek ve ekleme veya alın.

Logic apps SendGrid eylemleri, bu görevleri gerçekleştirmek için kullanabilirsiniz. Ayrıca SendGrid eylemleri çıktısını kullanan diğer eylemler olabilir. 

Bu bağlayıcı, bu nedenle, mantıksal uygulamanızı başlatmak için yalnızca eylemleri kullanan ayrı bir tetikleyici gibi sağlar bir **yinelenme** tetikleyici. Örneğin, listelerinizi için düzenli olarak alıcılar eklerseniz, alıcıları ve Office 365 Outlook Bağlayıcısı veya Outlook.com Bağlayıcısı'nı kullanarak listeleri hakkında e-posta gönderebilirsiniz.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* A [SendGrid hesabı](https://www.sendgrid.com/) ve [SendGrid API anahtarı](https://sendgrid.com/docs/ui/account-and-settings/api-keys/)

   Mantıksal uygulamanızı bir bağlantı oluşturup SendGrid hesabınıza erişmek için API anahtarınızı yetkilendirir.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* SendGrid hesabınıza erişmek için istediğiniz mantıksal uygulaması. SendGrid eylem kullanmak için mantıksal uygulamanızı başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

## <a name="connect-to-sendgrid"></a>Sendgrid'e Bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**. 

     veya

   * Bir eylem eklemek istediğiniz adımları arasında işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "sendgrid" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. Bağlantınıza bir ad verin. 

1. SendGrid API anahtarınızı girin ve ardından **Oluştur**.

1. Seçili eyleminiz için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/sendgrid/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)