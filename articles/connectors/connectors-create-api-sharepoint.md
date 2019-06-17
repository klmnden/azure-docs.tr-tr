---
title: Azure Logic Apps'ten SharePoint'e bağlanma | Microsoft Docs
description: Görevleri ve Azure Logic Apps kullanarak SharePoint Online veya SharePoint Server şirket içi kaynakları yönetmek ve izlemek, iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: e636b2bb08477e6c56c6ae41f08983fc5bfa2a9b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60450752"
---
# <a name="monitor-and-manage-sharepoint-resources-with-azure-logic-apps"></a>İzleme ve Azure Logic Apps ile SharePoint kaynaklarını yönetme

Azure Logic Apps ve SharePoint Bağlayıcısı ile otomatik görevler ve örneğin dosyaları, klasörleri, listeler, öğeleri, kişiler, vb., SharePoint Online veya şirket içinde SharePoint Server gibi kaynakları yönetme ve izleme iş akışları oluşturabilirsiniz:

* Dosyaları veya öğeleri oluşturulan, değiştirilen veya silinen zamana yönelik İzleyici.
* Oluşturma, alma, güncelleştirme veya öğeleri silin.
* Ekle, Al veya ekleri silin. İçerik ekleri alın.
* Oluşturma, kopyalama, güncelleştirme veya dosyaları silin. 
* Dosya özelliklerini güncelleştirin. İçerik, meta verileri veya özellikleri için bir dosya alın.
* Liste veya klasörleri ayıklar.
* Liste veya liste görünümleri alın.
* İçerik onay durumunu ayarlayın.
* Kişiler çözümleyin.
* SharePoint için HTTP istekleri göndermek.
* Varlık değerlerini Al.

Çıkış diğer eylemler için kullanılabilir hale getirmek ve yanıtları Al Tetikleyicileri kullanabilirsiniz. Logic apps eylemleri, SharePoint'te görevleri gerçekleştirmek için kullanabilirsiniz. Ayrıca, SharePoint eylemleri çıktısını kullanan diğer eylemler olabilir. Düzenli olarak dosyaları SharePoint'ten getirir, örneğin, iletileri takımınız için Slack Bağlayıcısı'nı kullanarak gönderebilirsiniz.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* SharePoint sitesi adresi ve kullanıcı kimlik bilgileri

  Mantıksal uygulamanızı bir bağlantı oluşturun ve SharePoint hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin. 

* SharePoint Server gibi şirket içi sistemlere mantıksal uygulamalar bağlanmadan önce yapmanız [yükleyin ve bir şirket içi veri ağ geçidi ayarlama](../logic-apps/logic-apps-gateway-install.md). Bu şekilde mantıksal uygulamanız için SharePoint Server bağlantı oluşturduğunuzda ağ geçidi yüklemenizi kullanılacağını belirtebilirsiniz.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* SharePoint hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir SharePoint tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir Salesforce hesabınız varsa, SharePoint eylemi kullanmak için mantıksal uygulamanızı gibi bir Salesforce tetikleyici bir tetikleyici ile başlayın.

  Örneğin, mantıksal uygulamanız ile başlayabilirsiniz **bir kayıt oluşturulduğunda** Salesforce tetikleyici. 
  Bu tetikleyici, bir müşteri adayı gibi yeni bir kayıt Salesforce'ta oluşturulan her zaman etkinleştirilir. 
  Ardından bu tetikleyiciyi SharePoint ile izleyebilirsiniz **dosya oluştur** eylem. Yeni kayıt oluşturulduğunda bu şekilde, mantıksal uygulamanız bu yeni kayıtla ilgili bilgilerle SharePoint'te bir dosya oluşturur.

## <a name="connect-to-sharepoint"></a>SharePoint’e bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "sharepoint" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   veya

   Var olan mantıksal uygulamalar, son adım, bir SharePoint eylem eklemek istediğiniz altında seçin için **yeni adım**. 
   Arama kutusuna filtreniz olarak "sharepoint" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Oturum açmanız istendiğinde, gereken bağlantı bilgilerini sağlayın. SharePoint Server kullanıyorsanız, seçtiğinizden emin olun **şirket içi veri ağ geçidi üzerinden Bağlan**. İşiniz bittiğinde **Oluştur**’u seçin.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/sharepoint/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
