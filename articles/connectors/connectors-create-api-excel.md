---
title: Excel Online - Azure Logic Apps bağlayın | Microsoft Docs
description: Excel Online REST API'leri ve Azure Logic Apps ile verileri yönetme
ms.service: logic-apps
services: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 94960b95e6de30159ec34b3f97bb5119cac42c35
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818108"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Azure Logic Apps ile Excel Online veri yönetme

Azure Logic Apps ve Excel Online Bağlayıcısı ile otomatik görevler ve iş veya OneDrive için Excel Online'da verilerinizi temel iş akışları oluşturabilirsiniz. İş akışlarınızı bu eylemler ve diğerleri ile verilerinizi örneğin gerçekleştirebilirsiniz:

* Yeni çalışma sayfaları ve tabloları oluşturun.
* Alma ve çalışma, tabloları ve satırları yönetin.
* Tek satır ve anahtar sütunlarını ekleyin.

Excel Online eylemleri çıktısını kullanan logic apps eylemleri dahil edebilirsiniz. Bu bağlayıcı, bu nedenle, mantıksal uygulamanızı başlatmak için yalnızca eylemleri kullanan ayrı bir tetikleyici gibi sağlar bir **yinelenme** tetikleyici. Örneğin, her hafta çalışma oluşturursanız, Office 365 Outlook Bağlayıcısı'nı kullanarak bu yeni çalışma sayfaları hakkında e-posta gönderebilirsiniz.

Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> [Excel Online iş](/connectors/excelonlinebusiness/) ve [Excel Online OneDrive için](/connectors/excelonline/) bağlayıcıları Azure Logic Apps ile çalışma ve farklı [powerapps Excel bağlayıcı](/connectors/excel/).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Bir [Office 365 hesabı](https://www.office.com/) iş hesabı veya kişisel Microsoft hesabı 

  Excel verilerinizi storage klasöründeki virgülle ayrılmış değer (CSV) dosyası olarak Örneğin, Onedrive'da bulunabilir. 
  Aynı CSV dosyası ile de kullanabileceğiniz [düz dosya bağlayıcı](../logic-apps/logic-apps-enterprise-integration-flatfile.md).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Excel Online verilerinize erişmek için istediğiniz mantıksal uygulaması. Bu bağlayıcı, bu nedenle, mantıksal uygulamanızı başlatmak için yalnızca eylem seçebilir ayrı bir tetikleyici örneğin sağlar, **yinelenme** tetikleyici.

## <a name="add-excel-action"></a>Excel Eylem Ekle

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Tetikleyici altında seçin **yeni adım**.

1. Arama kutusuna filtreniz olarak "excel" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. Office 365 hesabınızda oturum açmanız istenirse seçin **oturum**. 

   Mantıksal uygulamanızı Excel Online bağlantı oluşturun ve verilerinize erişmek için kimlik bilgilerinizi yetkilendirin.

1. Seçili eylem için gerekli bilgileri sağlayarak ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Eylemler ve bağlayıcılar Swagger dosyaları tarafından açıklanan sınırları gibi teknik ayrıntılar için bu bağlayıcı başvuru sayfalarına bakın:

* [İş için Excel Online'da](/connectors/excelonlinebusiness/) 
* [Excel Online'da OneDrive için](/connectors/excelonline/) 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
