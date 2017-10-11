---
title: "Azure mantıksal uygulamaları bir şirket içi SAP sisteme bağlanma | Microsoft Docs"
description: "Bir şirket içi SAP sisteme mantığı uygulama akışınızı şirket içi veri ağ geçidi üzerinden bağlanması"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a>Logic apps SAP Bağlayıcısı ile bir şirket içi SAP sistemine bağlanması 

Şirket içi veri ağ geçidi verilerini yönetmek ve şirket kaynaklarına güvenle erişmesini sağlar. Bu konu, bir şirket içi SAP sisteme logic apps nasıl bağlanabilirsiniz gösterir. Bu örnekte, mantıksal uygulamanızı IDOC HTTP istekleri ve yanıtı geri gönderir.    

> [!NOTE]
> Geçerli sınırlamalar: 
> - Yanıt için gereken tüm adımları içinde son yok, mantıksal uygulamanızı zaman aşımına [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md). Bu senaryoda, istekleri engellenen. 
> - Dosya seçiciyi kullanılabilir tüm alanlar görüntülenmez. Bu senaryoda, yollarını el ile ekleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Yükleme ve yapılandırma en son [şirket içi veri ağ geçidi](https://www.microsoft.com/download/details.aspx?id=53127) sürüm 1.15.6150.1 ya da daha yeni. [Bir mantıksal uygulama şirket içi veri ağ geçidi bağlanma](http://aka.ms/logicapps-gateway) adımlar listelenmektedir. Devam etmeden önce ağ geçidini bir şirket içi makineye yüklenmesi gerekir.

- Karşıdan yükle ve veri ağ geçidinin yüklü olduğu makinede en son SAP istemci Kitaplığı yükleyin. SAP sürümlerinden birini kullanın: 
    - SAP sunucusuna
        - SAP sunucuların (NCo) 3.0 .NET bağlayıcı desteği
 
    - SAP istemci
        - SAP .NET bağlayıcı (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a>Tetikleyiciler ve Eylemler SAP sisteminize bağlamak için ekleme

SAP bağlayıcısı Eylemler ancak değil Tetikleyicileri vardır. Bu nedenle, biz iş akışının başlangıcında başka bir tetikleyici kullanmanız gerekir. 

1. İstek/yanıt tetikleyicisi ekleyin ve ardından **yeni adım**.

2. Seçin **Eylem Ekle**seçip SAP Bağlayıcısı'nı yazarak `SAP` arama alanında:    

     ![SAP uygulama sunucusu ya da SAP ileti sunucusu seçin](media/logic-apps-using-sap-connector/sap-action.png)

3. Seçin [ **SAP uygulama sunucusu** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) veya [ **SAP ileti sunucusu**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), SAP kurulumunuzu göre. Varolan bir bağlantıyı sahip değilseniz, birini oluşturmanız istenir.

   1. Seçin **Connect şirket içi veri ağ geçidi üzerinden**ve SAP sisteminizi ayrıntıları girin:   

       ![SAP için bağlantı dizesi Ekle](media/logic-apps-using-sap-connector/picture2.png)  

   2. Altında **ağ geçidi**, mevcut bir ağ geçidi seçin veya yeni bir ağ geçidi yüklemek için seçin **ağ geçidi yükleme**.

        ![Yeni bir ağ geçidi yükleyin](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Tüm Ayrıntılar girdikten sonra Seç **oluşturma**. 
   Logic Apps yapılandırır ve bağlantı düzgün çalıştığından emin olmayı bağlantısını test eder.

4. SAP bağlantınız için bir ad girin.

5. Farklı SAP seçenekleri şimdi kullanılabilir. IDOC kategori bulmak için listeden seçin. Veya el ile yolu yazın ve HTTP yanıtı seçin **gövde** alan:

     ![SAP eylemi](media/logic-apps-using-sap-connector/picture3.png)

6. Oluşturma eylem eklemek bir **HTTP yanıtı**. Yanıt iletisi SAP çıkışı olmalıdır.

7. Mantıksal uygulamanızı kaydedin. IDOC HTTP tetikleme URL'si aracılığıyla göndererek test. IDOC gönderildikten sonra mantıksal uygulama yanıttan bekleyin:   

     > [!TIP]
     > Nasıl yapılır kullanıma [mantıksal uygulamalarınızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md).

SAP bağlayıcısı mantıksal uygulamanızı eklenir, diğer işlevleri keşfetmeye başlayın:

- BAPI
- RFC

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını öğrenmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Azure Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

- Dönüştürme nasıl doğrulamak ve diğer BizTalk benzeri işlevlerde öğrenin [Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Şirket içi veri bağlanmak](../logic-apps/logic-apps-gateway-connection.md) mantığı uygulamalardan
