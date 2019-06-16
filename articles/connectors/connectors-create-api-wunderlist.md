---
title: Azure Logic Apps'ten Wunderlist'e bağlanın | Microsoft Docs
description: Görevler ve izleme ve Azure Logic Apps kullanarak listeleri, görevler, anımsatıcı ve diğer Wunderlist hesabınızdaki yönetme iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: e3570ab1227ca388ac62bffdc74bb68b1ddc41d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105675"
---
# <a name="monitor-and-manage-wunderlist-by-using-azure-logic-apps"></a>İzleme ve Azure Logic Apps kullanarak Wunderlist yönetme

Azure Logic Apps ve Wunderlist Bağlayıcısı ile otomatik görevler ve iş akışlarını izlemek ve yapılacaklar listeleri, görevleri, anımsatıcılar ve diğer eylemlerin yanı sıra Wunderlist hesabınızda daha fazla örneğin yönetmek oluşturabilirsiniz:

* Görevlerimin veya anımsatıcılar ortaya yeni görevler oluşturulmasına zamana yönelik İzleyici.
* Oluşturma ve listeleri, notlar, görevler, görevleri ve diğer yönetin.
* Anımsatıcıları ayarlayın.
* Listeler, görevleri, görevleri, anımsatıcıları, dosyaları, notları, açıklamaları ve daha fazla bilgi alın.

[Wunderlist](https://www.wunderlist.com/) planlama, yönetme ve, projeler, yapılacaklar listeleri ve görevler - yerde her cihazda, son yardımcı olan bir hizmettir. Çıkış diğer eylemler için kullanılabilir hale getirmek ve yanıtları Wunderlist hesabınızdan alma Tetikleyicileri kullanabilirsiniz. Görevleri Wunderlist hesabınızla Eylemler kullanabilirsiniz. Ayrıca, Wunderlist eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, yeni görevlerin sona erme tarihleri geldiğinde, iletileri Slack Bağlayıcısı ile gönderebilir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Wunderlist hesabı ve kullanıcı bilgilerinizi

   Mantıksal uygulamanızı bir bağlantı oluşturup Wunderlist hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Yammer hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Wunderlist tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Wunderlist eylem kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlayın, **yinelenme** tetikleyici.

## <a name="connect-to-wunderlist"></a>Wunderlist'e bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Boş mantıksal uygulama için arama kutusuna filtreniz olarak "wunderlist" girin. 
   Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

     veya

   * Var olan mantıksal uygulamalar için: 
   
     * Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**. 

       veya

     * Bir eylem eklemek istediğiniz adımları arasında işaretçinizi adımlar arasındaki okun üzerine getirin. 
     Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.
     
       Arama kutusuna filtreniz olarak "wunderlist" girin. 
       Eylemler listesinde, istediğiniz eylemi seçin.

1. Wunderlist'e oturum açmanız istenirse, erişime izin verebilir böylece şimdi oturum açın.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/wunderlist/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)