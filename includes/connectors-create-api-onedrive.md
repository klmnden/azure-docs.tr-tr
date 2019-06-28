---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 7cfce34cb2d6002dba5ec570bf859ec47e894c65
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188627"
---
#### <a name="prerequisites"></a>Önkoşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* A [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) hesabı 

OneDrive hesabınızda bir mantıksal uygulama kullanabilmeniz için önce OneDrive hesabınıza bağlanmak için mantıksal uygulama yetkilendirin.  Kolayca Azure portalında mantıksal uygulama içinde bunu yapabilirsiniz. 

Mantıksal uygulamanızı, aşağıdaki adımları kullanarak OneDrive hesabınıza bağlanmak için yetkilendirin.

1. Mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda seçin **yönetilen API'leri Göster Microsoft** açılan liste ve ardından arama kutusuna "onedrive" girin. Tetikleyiciler veya Eylemler birini seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. OneDrive için herhangi bir bağlantı daha önce oluşturmadıysanız, OneDrive kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Seçin **oturum**, kullanıcı adı ve parolayı girin. Seçin **oturum**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak ve OneDrive hesabınızdaki verilere erişmek için son noktanın yetkilendirilmesi için kullanılır. 
4. Seçin **Evet** OneDrive hesabınızda kullanmak için mantıksal uygulama yetkilendirmek için:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Bağlantıyı oluşturan dikkat edin. Şimdi mantıksal uygulamanızı diğer adımlarla devam edin:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

