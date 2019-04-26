---
title: Box - Azure Logic Apps'ı bağlama | Microsoft Docs
description: Oluşturun ve dosyalarını kutusu REST API'leri ve Azure Logic Apps ile yönetme
author: ecfan
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 971d38fa0fbd47f0deb815577033bbe684aac32f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60312585"
---
# <a name="create-and-manage-files-in-box-with-azure-logic-apps"></a>Azure Logic Apps ile kutusundaki dosyaları oluşturmak ve yönetmek

Bu makalede nasıl oluşturabileceğinizi ve kutusu Bağlayıcısı ile bir mantıksal uygulama içinde kutusunda dosyalarınızı yönetin gösterilmektedir. Böylece, görevler ve dosyalarınızı ve diğer Eylemler, örneğin yönetmek için iş akışlarını otomatik hale getiren mantıksal uygulamaları oluşturabilirsiniz:

* Box'tan alma verileri temel alan, iş akışınızı oluşturun.

* Bir dosya oluşturulduğunda veya otomatik görevler ve iş akışı tetikler.

* Bir dosyayı kopyalar ve bir dosya siler eylemleri çalıştırın.

  Bu eylemlerden yanıt aldığınızda bunlar çıkış diğer eylemler için kullanılabilir olun. 
  Örneğin, çubuğundaki bir dosya değiştirildiğinde, e-postayla Office 365'i kullanarak bu dosyayı gönderebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* A [kutusunda hesabı](https://www.box.com/home)

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Box hesabınıza erişmek için istediğiniz mantıksal uygulaması. Mantıksal uygulamanızı bir kutusu tetikleyici ile başlayın, gerek bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/box/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)