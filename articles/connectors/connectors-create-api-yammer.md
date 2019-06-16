---
title: Azure Logic Apps'ten Yammer'a bağlanın | Microsoft Docs
description: Görevler ve izleme, gönderin ve Azure Logic Apps'i kullanarak ileti, akışları ve diğer Yammer'da yönetme iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: ca2d28f3438fd166fa282488206662c95777bf3b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104740"
---
# <a name="monitor-and-manage-your-yammer-account-by-using-azure-logic-apps"></a>İzleme ve Azure Logic Apps kullanarak Yammer hesabınızı yönetme

Azure Logic Apps ve Yammer Bağlayıcısı ile otomatik görevler ve izleme ve iletileri, akışları ve diğer eylemlerin yanı sıra, Yammer hesabınızdaki daha fazla örneğin yönetme iş akışları oluşturabilirsiniz:

* Yeni iletileri izlenen akışları ve grupları görünen zamana yönelik İzleyici.
* İletileri, grupları, ağlar, kullanıcıların Ayrıntılar ve daha fazla bilgi alın.
* Gönderin ve ileti ister.

Çıkış diğer eylemler için kullanılabilir hale getirmek ve yanıtları Yammer hesabınızdan alma Tetikleyicileri kullanabilirsiniz. Yammer hesabınızla görevleri gerçekleştiren eylemlerini kullanabilirsiniz. Ayrıca, Yammer eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, yeni iletiler akışları ya da grupları göründüğünde, bu iletileri Slack Bağlayıcısı ile paylaşabilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Yammer hesabı ve kullanıcı bilgilerinizi

   Mantıksal uygulamanızı bir bağlantı oluşturun ve Yammer hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Yammer hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Yammer tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Yammer eylem kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlatın, **yinelenme** tetikleyici.

## <a name="connect-to-yammer"></a>Yammer'a bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Boş mantıksal uygulamaları, arama kutusuna filtreniz olarak "yammer" girin. 
   Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

     veya

   * Var olan mantıksal uygulamalar için: 
   
     * Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**. 

       veya

     * Bir eylem eklemek istediğiniz adımları arasında işaretçinizi adımlar arasındaki okun üzerine getirin. 
     Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.
     
       Arama kutusuna filtreniz olarak "yammer" girin. 
       Eylemler listesinde, istediğiniz eylemi seçin.

1. Erişime izin verebilir böylece şimdi oturum açma Yammer'a oturum açmanız istenirse, şimdi oturum açın.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/yammer/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)