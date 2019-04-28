---
title: Azure Logic Apps'ten Twilio'ya bağlanın | Microsoft Docs
description: Görevler ve genel SMS, MMS ve IP iletileri Twilio hesabınız üzerinden Azure Logic Apps kullanarak yönetme iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: fab52236c701f10c8e8e23ac398362ca4583ea06
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104910"
---
# <a name="manage-messages-in-twilio-with-azure-logic-apps"></a>Azure Logic Apps ile Twilio iletilerini yönetme

Azure Logic Apps ve Twilio Bağlayıcısı ile otomatik görevler ve almak, göndermek ve Twilio, genel SMS, MMS ve IP iletileri içeren iletileri Listele iş akışları oluşturabilirsiniz. Bu Eylemler, Twilio hesabınızla görevleri gerçekleştirmek için kullanabilirsiniz. Ayrıca, Twilio eylemleri çıktısını kullanan diğer eylemler olabilir. Yeni bir ileti geldiğinde, örneğin, Slack bağlayıcısıyla içerik gönderebilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Gelen [Twilio](https://www.twilio.com/): 

  * Twilio hesap Kimliğinizi ve [kimlik doğrulama belirteci](https://support.twilio.com/hc/en-us/articles/223136027-Auth-Tokens-and-How-to-Change-Them), Twilio panonuzu bulabilirsiniz

    Mantıksal uygulamanızı bağlantı kurun ve Twilio hesabınızın mantıksal uygulamanızdan erişmek için kimlik bilgilerinizi yetkilendirin. 
    Bir Twilio deneme hesabı kullanıyorsanız, yalnızca SMS gönder *doğrulandı* telefon numaraları.

  * SMS gönderebilen doğrulanmış bir Twilio telefon numarası

  * SMS alabilecek doğrulanmış bir Twilio telefon numarası

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Twilio hesabınız için istediğiniz mantıksal uygulaması. Twilio eylem kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlayın, **yinelenme** tetikleyici.

## <a name="connect-to-twilio"></a>Twilio'ya bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

     * Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**. 

       -veya-

     * Bir eylem eklemek istediğiniz adımları arasında işaretçinizi adımlar arasındaki okun üzerine getirin. 
     Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.
     
       Arama kutusuna filtreniz olarak "twilio" girin. 
       Eylemler listesinde, istediğiniz eylemi seçin.

1. Bağlantınız için gerekli bilgileri sağlayın ve ardından **Oluştur**:

   * Bağlantınız için kullanılacak adı
   * Twilio hesap kimliği 
   * Twilio (kimlik doğrulaması) erişim belirteci

1. Seçili eyleminiz için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/twilio/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)