---
title: "Logic Apps içinde SharePoint sunucusunu bağlayıcıyı kullanmak | Microsoft Docs"
description: "Kullanmaya başlama Logic apps içinde SharePoint Server Bağlayıcısı"
services: logic-apps
documentationcenter: 
author: ecfan
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
ms.author: estfan; ladocs
ms.openlocfilehash: d342b3c4f84c5dab212b9327d6a72759934d0ae5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-sharepoint-connector"></a>SharePoint Bağlayıcısı ile çalışmaya başlama
SharePoint bağlayıcı listeleri SharePoint üzerinde çalışmak için bir yol sağlar.

Bir mantıksal uygulama oluşturarak başlama; bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>SharePoint bağlantı oluşturun.
SharePoint Bağlayıcısı'nı kullanmak için önce oluşturduğunuz bir **bağlantı** ardından ayrıntılar için bu özellikleri sağlar: 

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Belirteç |Evet |SharePoint kimlik bilgilerini sağlayın |

Bağlanmak için **SharePoint**, kimliğinizi (kullanıcı adı ve parola, akıllı kart kimlik bilgileri ve benzeri) girin. Kimlik doğruladınız sonra mantıksal uygulamanızı SharePoint Bağlayıcısı'nı kullanmaya devam edebilirsiniz. 

Tasarımcısı mantıksal uygulamanızı karşın üzerinde oturum açmak ve oluşturmak için aşağıdaki adımları kullanın **bağlantı** mantıksal uygulamanızı kullanmak için:

1. SharePoint arama kutusuna girin ve SharePoint ile tüm girişleri adını döndürmek arama bekleyin:   
   ![SharePoint'i yapılandırma][1]  
2. Seçin **bir dosya oluşturulduğunda SharePoint -**   
3. Seçin **SharePoint'e oturum**:   
   ![SharePoint'i yapılandırma][2]    
4. SharePoint ile kimlik doğrulaması oturum açmak için SharePoint kimlik bilgilerinizi sağlayın   
   ![SharePoint'i yapılandırma][3]     
5. Kimlik doğrulama tamamlandıktan sonra SharePoint yapılandırarak tamamlamak için mantıksal uygulamanızı yönlendirilirsiniz **bir dosya oluşturulduğunda** iletişim.          
   ![SharePoint'i yapılandırma][4]  
6. Daha sonra diğer tetikleyiciler ve mantıksal uygulamanızı tamamlamanız gereken eylemler ekleyebilirsiniz.   
7. Seçerek çalışmanızı kaydedin **kaydetmek** (üst doğru) menüsünde.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/sharepoint/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
