---
ms.openlocfilehash: 53c683dacbb3b94e34bd8662743673c3a0490d94
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119968"
---
#### <a name="prerequisites"></a>Önkoşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* A [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) hesabı 

Bir mantıksal uygulama çalıştırmasında Dynamics hesabınızı kullanmadan önce mantıksal uygulama, CRM Online hesabınıza bağlanmak için yetkilendirin. Kolayca Azure portalında mantıksal uygulama içinde bunu yapabilirsiniz. 

Mantıksal uygulamanızı, aşağıdaki adımları kullanarak, CRM Online hesabınıza bağlanmak için yetkilendirin.

1. Mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda seçin **yönetilen API'leri Göster Microsoft** açılan liste ve ardından arama kutusuna "dynamics" girin. Tetikleyiciler veya Eylemler birini seçin:  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Daha önce Dynamics herhangi bir bağlantı oluşturmadıysanız, Dynamics kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Seçin **oturum**, kullanıcı adı ve parolayı girin. Seçin **oturum**. 
   
    Bu kimlik bilgileri, mantıksal uygulamanızı bağlanın ve Dynamics hesabınızdaki verilere erişim yetkisi vermek için kullanılır. 
4. Bağlantıyı oluşturan dikkat edin. Şimdi mantıksal uygulamanızı diğer adımlarla devam edin:  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

