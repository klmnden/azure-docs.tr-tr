---
title: "Logic Apps içinde SharePoint sunucusunu bağlayıcıyı kullanmak | Microsoft Docs"
description: "Kullanmaya başlama Logic apps içinde SharePoint Server Bağlayıcısı"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: da863e0249cb46e4e569812a851f3199d57b2107
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-sharepoint-connector"></a>SharePoint Bağlayıcısı ile çalışmaya başlama
SharePoint Bağlayıcısı, SharePoint listesindeki çalışmak için bir yol sağlar.

Bir mantıksal uygulama oluşturarak başlama; bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>SharePoint bağlantı oluşturun.
SharePoint Bağlayıcısı'nı kullanmak için önce oluşturduğunuz bir **bağlantı** ardından ayrıntılar için bu özellikleri sağlar: 

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Belirteç |Evet |SharePoint Kimlik Bilgilerini belirtin |

Bağlanmak için **SharePoint**, SharePoint için (kullanıcı adı ve parola, akıllı kart kimlik bilgileri, vb.) kimlik bilgilerinizi girin. Kimlik doğruladınız sonra mantıksal uygulamanızı SharePoint Bağlayıcısı'nı kullanmaya devam edebilirsiniz. 

Tasarımcısı mantıksal uygulamanızı karşın, bağlantı oluşturmak için SharePoint ile imzalamak için bu adımları takip **bağlantı** mantıksal uygulamanızı kullanmak için:

1. SharePoint arama kutusuna girin ve SharePoint ile tüm girişleri adını döndürmek arama bekleyin:   
   ![SharePoint'i yapılandırma][1]  
2. Seçin **bir dosya oluşturulduğunda SharePoint -**   
3. Seçin **SharePoint'e oturum**:   
   ![SharePoint'i yapılandırma][2]    
4. SharePoint ile kimlik doğrulaması oturum açmak için SharePoint kimlik bilgilerinizi sağlayın   
   ![SharePoint'i yapılandırma][3]     
5. Kimlik doğrulama tamamlandıktan sonra mantıksal uygulamanızı SharePoint yapılandırarak tamamlamak için yeniden yönlendirilmesini **bir dosya oluşturulduğunda** iletişim.          
   ![SharePoint'i yapılandırma][4]  
6. Daha sonra diğer tetikleyiciler ve mantıksal uygulamanızı tamamlamanız gereken eylemler ekleyebilirsiniz.   
7. Seçerek çalışmanızı kaydedin **kaydetmek** yukarıdaki menü çubuğunda.  

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/sharepoint/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
