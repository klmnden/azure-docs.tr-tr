---
title: Azure Logic Apps'ten Twitter'a bağlanın | Microsoft Docs
description: Görevler ve izleme ve yönetme tweetleri yanı sıra Azure Logic Apps hakkında takipçi, izlenen kullanıcılarınıza, diğer kullanıcılar, zaman çizelgeleri ve Twitter hesabınızdan daha fazla veri almak, iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 0fbd89202796cb4543dbecbeee605c9b87cc9d05
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104995"
---
# <a name="monitor-and-manage-twitter-by-using-azure-logic-apps"></a>İzleme ve Azure Logic Apps kullanarak Twitter'ı yönetme

Azure Logic Apps ve Twitter Bağlayıcısı ile otomatik görevler oluşturabilir ve tweet, takipçi, kullanıcılar ve kullanıcıların, zaman çizelgeleri ve diğer eylemlerin yanı sıra diğer örneğin ardından izleme ve yönetme veri Twitter gibi verdiğiniz iş akışları:

* İzleme, gönderin ve tweetleri arayın.
* Takipçi, takip edilen kullanıcıları, zaman çizelgeleri ve diğer verileri elde edersiniz.

Çıkış diğer eylemler için kullanılabilir hale getirmek ve yanıtları Twitter hesabınızdan alma Tetikleyicileri kullanabilirsiniz. Twitter hesabınızla görevleri gerçekleştiren eylemlerini kullanabilirsiniz. Ayrıca, diğer eylemlerin çıktısı bir Twitter eylemler olabilir. Örneğin, belirli bir diyez etiketi ile yeni bir tweet gönderildiğinde, Slack bağlayıcısıyla iletileri gönderebilir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Twitter hesabı ve kullanıcı kimlik

   Mantıksal uygulamanızı bir bağlantı oluşturun ve Twitter hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Twitter hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Twitter tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir Twitter eylemi kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlatın, **yinelenme** tetikleyici.

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Boş mantıksal uygulamaları, arama kutusuna filtreniz olarak "twitter" girin. 
   Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

     -veya-

   * Var olan mantıksal uygulamalar için: 
   
     * Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**. 

       -veya-

     * Bir eylem eklemek istediğiniz adımları arasında işaretçinizi adımlar arasındaki okun üzerine getirin. 
     Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.
     
       Arama kutusuna filtreniz olarak "twitter" girin. 
       Eylemler listesinde, istediğiniz eylemi seçin.

1. Twitter'da oturum açmanız istenirse, mantıksal uygulamanız için erişim yetki verebilir böylece şimdi oturum açın.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

### <a name="twitter-trigger-when-a-new-tweet-is-posted"></a>Twitter tetikleyici: Yeni bir tweet gönderildiğinde

Tetikleyici yeni bir tweet diyez etiketi, #Seattle gibi algıladığında, bu tetikleyiciyi bir mantıksal uygulama iş akışı başlatır. Örneğin, bu tweetleri bulunmadığında, Dropbox Bağlayıcısı'nı kullanarak bir Dropbox hesabı gibi depolama ortamlarına tweetleri içeriğe sahip bir dosya ekleyebilirsiniz. 

İsteğe bağlı olarak, uygun tweetleri belirtilen en az bir takipçilerin sayısı kullanıcılarla gelmelidir bir koşul ekleyebilirsiniz.

**Kuruluş örnek**: Bu tetikleyici, şirketiniz tweetleri izleyin ve bir SQL veritabanı'na tweetleri içeriği yüklemek için kullanabilirsiniz.

### <a name="twitter-action-post-a-tweet"></a>Twitter eylem: Tweet at

Bu eylem bir tweet gönderir, ancak daha önce açıklandığı gibi bir tetikleyici tarafından bulunan bir tweet içeriği tweeti içeren eylem ayarlayabilirsiniz. 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/twitterconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
