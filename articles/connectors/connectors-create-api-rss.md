---
title: Azure Logic Apps'ten RSS akışlarına bağlanmak | Microsoft Docs
description: Görevler ve izleme ve Azure Logic Apps kullanarak RSS akışları yönetme iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.assetid: a10a6277-ed29-4e68-a881-ccdad6fd0ad8
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 01573871700bbeeb653ce3efdbf6c6aca88fd454
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104894"
---
# <a name="manage-rss-feeds-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak RSS akışlarını yönetme

Azure Logic Apps ve RSS Bağlayıcısı ile otomatik görevler oluşturabilir ve iş akışları için herhangi bir RSS akışı, örneğin:

* RSS Akış öğelerini yayımlandığında izleyin.
* Tüm RSS akışı öğelerini listele.

Gerçekten Basit Dağıtım olarak da adlandırılan RSS (zengin Site Özeti), web dağıtımı için popüler bir biçimidir ve blog gönderileri ve haber başlıklarını gibi sık güncelleştirilen içerikler yayımlamak için kullanılır. Birçok içerik yayımcılarının, kullanıcıların bu içeriğe abone, böylece bir RSS akışı sağlar. 

Bir RSS akışından yanıtları alır ve çıktıyı diğer eylemler için kullanılabilir hale getirir, bir RSS tetikleyicisi kullanabilirsiniz. Logic apps RSS eylem, bir RSS akışı bir görevi gerçekleştirmek için kullanabilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Bir RSS akışı URL'si

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Bir RSS erişmek istediğiniz mantıksal uygulama akışı. Bir RSS tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). RSS eylem kullanmak için mantıksal uygulamanızı başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

## <a name="connect-to-an-rss-feed"></a>Bir RSS akışına bağlanma

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Bir yolu seçin: 

   * Boş mantıksal uygulama için arama kutusuna filtreniz olarak "rss" yazın. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

     veya

   * Var olan mantıksal uygulamalar, bir eylem eklemek istediğiniz adımı altında seçin için **yeni adım**. Arama kutusuna "rss" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/rss/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)