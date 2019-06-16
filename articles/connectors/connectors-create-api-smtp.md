---
title: Azure Logic Apps'ten SMTP'ye bağlanın | Microsoft Docs
description: Görevleri ve e-posta hesabınız SMTP (Basit Posta Aktarım Protokolü) üzerinden Azure Logic Apps göndermek için iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 78b1eb6272fa97ef392e97723454d29cf56bb4bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106159"
---
# <a name="send-email-from-your-smtp-account-with-azure-logic-apps"></a>Azure Logic Apps ile SMTP hesabınızdan e-posta Gönder

Azure Logic Apps ve Basit Posta Aktarım Protokolü (SMTP) Bağlayıcısı ile otomatik görevler ve SMTP hesabınızdan e-posta gönderen iş akışları oluşturabilirsiniz. Ayrıca SMTP eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, SMTP e-posta gönderdiğinde size Slack, ekibinizin Slack Bağlayıcısı ile bildirebilir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* SMTP hesabı ve kullanıcı bilgilerinizi

  Mantıksal uygulamanızı bir bağlantı oluşturun ve SMTP hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* SMTP hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Salesforce hesabınız varsa, bir SMTP eylemi kullanmak için mantıksal uygulamanızı gibi bir Salesforce tetikleyici bir tetikleyici ile başlayın.

  Örneğin, mantıksal uygulamanız ile başlayabilirsiniz **bir kayıt oluşturulduğunda** Salesforce tetikleyici. 
  Bu tetikleyici, bir müşteri adayı gibi yeni bir kayıt Salesforce'ta oluşturulan her zaman etkinleştirilir. 
  Ardından bu tetikleyiciyle SMTP izleyebilirsiniz **e-posta Gönder** eylem. Yeni kayıt oluşturulduğunda bu şekilde, mantıksal uygulamanız yeni kayıtla ilgili SMTP hesabınızdan bir e-posta gönderir.

## <a name="connect-to-smtp"></a>SMTP'ye bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. SMTP eylemi eklemek istediğiniz son adımı altında seçin **yeni adım**. 

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "smtp" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. İstendiğinde, bu bağlantı bilgisini sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Bağlantı Adı** | Evet | SMTP sunucunuz bağlantı için bir ad | 
   | **SMTP sunucu adresi** | Evet | SMTP sunucunuzun adresi | 
   | **Kullanıcı adı** | Evet | SMTP hesabınız için kullanıcı adı | 
   | **Parola** | Evet | SMTP hesabı için parola | 
   | **SMTP sunucusu bağlantı noktası** | Hayır | Kullanmak istediğiniz SMTP sunucunuzun belirli bir bağlantı noktası | 
   | **SSL etkinleştirilsin mi?** | Hayır | Etkinleştirmek veya devre dışı SSL şifrelemesi kapatabilirsiniz. | 
   |||| 

1. Seçili eyleminiz için gerekli bilgileri sağlayın. 

1. Mantıksal uygulamanızı kaydedin veya mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/smtpconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)