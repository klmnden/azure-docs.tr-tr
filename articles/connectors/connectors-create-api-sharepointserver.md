---
title: Logic Apps içinde SharePoint sunucusunu bağlayıcıyı kullanmak | Microsoft Docs
description: SharePoint Server bağlayıcısını Logic apps kullanmaya başlama
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 3ac3105231017cdbf8bfcf143b26451a68da5283
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868710"
---
# <a name="get-started-with-the-sharepoint-connector"></a>SharePoint bağlayıcı ile çalışmaya başlama
SharePoint bağlayıcı, SharePoint'te listelerle çalışmak için bir yol sağlar.

Mantıksal uygulama oluşturarak başlayın; bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>Bir SharePoint bağlantısı oluşturma
SharePoint bağlayıcıyı kullanmak için önce oluşturmanız bir **bağlantı** ardından bu özellikler için ayrıntıları belirtin: 

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Belirteç |Evet |SharePoint Kimlik Bilgilerini belirtin |

Bağlanmak için **SharePoint**, kimliğinizi (kullanıcı adı ve parola, akıllı kart kimlik bilgilerini ve benzeri) girin. Kimlik doğrulaması yaptınız sonra mantıksal uygulamanızda SharePoint Bağlayıcısı'nı kullanmaya devam edebilirsiniz. 

Ancak Tasarımcı mantıksal uygulamanızın üzerinde oturum açın ve oluşturmak için aşağıdaki adımları kullanın **bağlantı** mantıksal uygulamanızda kullanmak için:

1. Arama kutusuna SharePoint ve SharePoint ile tüm girişleri adı döndürülecek arama bekleyin:   
   ![SharePoint'i yapılandırma][1]  
2. Seçin **SharePoint - bir dosya oluşturulduğunda**   
3. Seçin **SharePoint oturumu açma**:   
   ![SharePoint'i yapılandırma][2]    
4. SharePoint ile kimlik doğrulaması oturum açmak için SharePoint kimlik bilgilerinizi sağlayın   
   ![SharePoint'i yapılandırma][3]     
5. Kimlik doğrulaması tamamlandıktan sonra mantıksal uygulamanızı SharePoint'in yapılandırarak tamamlanması yönlendirilirsiniz **bir dosya oluşturulduğunda** iletişim.          
   ![SharePoint'i yapılandırma][4]  
6. Ardından, diğer tetikleyiciler ve mantıksal uygulamanızı tamamlamak için gereken eylemler ekleyebilirsiniz.   
7. Seçerek çalışmanızı kaydedin **Kaydet** (üst doğru) menüsünde.

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/sharepoint/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
