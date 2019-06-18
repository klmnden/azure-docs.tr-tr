---
title: Outlook.com - Azure Logic Apps'i bağlama | Microsoft Docs
description: E-posta, Takvim ve kişiler Outlook.com REST API'leri ve Azure Logic Apps ile yönetme
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.topic: article
ms.date: 08/18/2016
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: fd6836451a73551487b8f97903594154a2efc894
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105811"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-with-azure-logic-apps"></a>E-posta, takvimler ve Azure Logic Apps ile Outlook.com kişileri Yönet

Bu makalede nasıl oluşturabileceğinizi ve Outlook.com hesabınızda kutusu Bağlayıcısı ile bir mantıksal uygulama içinde yönetme gösterilmektedir. Böylece, örneğin görevleri ve Outlook.com hesabınız için iş akışlarını otomatik hale getiren mantıksal uygulamaları oluşturabilirsiniz:

* E-posta gönderin. 
* Toplantı zamanlama.
* Kişileri ekleyin. 

Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir [Outlook.com hesabınız](https://outlook.live.com/owa/)

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Outlook.com hesabınız için istediğiniz mantıksal uygulaması. Bir Outlook tetikleyicisi ile mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-outlookcom"></a>Outlook.com'da bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Connect to Outlook.com](../../includes/connectors-create-api-outlook.md)]

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Swagger dosyası tarafından açıklandığı gibi sınırları, tetikleyiciler ve Eylemler gibi teknik ayrıntılar için bkz [bağlayıcının başvuru sayfası](/connectors/outlook/). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)