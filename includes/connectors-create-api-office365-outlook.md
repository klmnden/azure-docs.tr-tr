---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: b216de0a5094066977467b2899567122d585fb7e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66149724"
---
#### <a name="prerequisites"></a>Önkoşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [Office 365](https://office365.com) hesabı  

Office 365 hesabınıza bir mantıksal uygulama çalıştırmasında kullanmadan önce mantıksal uygulama Office 365 hesabınıza bağlanmak için yetkilendirin. Kolayca Azure portalında mantıksal uygulama içinde bunu yapabilirsiniz.  

Mantıksal uygulamanızı, aşağıdaki adımları kullanarak Office 365 hesabınıza bağlanmaya izin verirsiniz:

1. Mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda seçin **yönetilen API'leri Göster Microsoft** açılan liste ve ardından arama kutusuna "office 365" girin. Tetikleyiciler veya Eylemler birini seçin:  
    ![Office 365 bağlantı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Daha önce Office 365 için herhangi bir bağlantı oluşturmadıysanız, Office 365 kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
    ![Office 365 bağlantı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Seçin **oturum**, kullanıcı adı ve parolayı girin. Seçin **oturum**:  
    ![Office 365 bağlantı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Bu kimlik bilgileri, mantıksal uygulamanızı bağlayın ve Office 365 hesabınıza erişmek üzere yetkilendirmek için kullanılır. 
4. Bağlantıyı oluşturan dikkat edin. Şimdi mantıksal uygulamanızı diğer adımlarla devam edin:   
    ![Office 365 bağlantı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

