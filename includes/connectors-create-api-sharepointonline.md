---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 0cabc58d856c09accd9b1924fe63d6518b1cb9ef
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130005"
---
Bağlanmak için **SharePoint Online**, SharePoint Online (kullanıcı adı ve parola, akıllı kart kimlik bilgileri, vb.) kimliğinizi sağlamanız gerekir. Doğrulandı sonra mantıksal uygulamanızda SharePoint Online Bağlayıcısı'nı kullanmaya devam edebilirsiniz. 

Sırada tasarımcısında mantıksal uygulamanızın, SharePoint'te oturum oluşturmak için aşağıdaki adımları izleyin. **bağlantı** mantıksal uygulamanızda kullanmak için:

1. SharePoint arama kutusuna girin ve tüm tetikleyiciler ve eylemler SharePoint Online için ilgili döndürmek arama bekleyin:   
   ![SharePoint'i yapılandırma][1]  
2. Seçin **SharePoint bir dosya oluşturulduğunda Online -** tetikleyicisi  
3. Seçin **SharePoint oturumu açma çevrimiçi**:   
   ![SharePoint'i yapılandırma][2]    
4. SharePoint ile kimlik doğrulaması oturum açmak için SharePoint kimlik bilgilerinizi sağlayın   
   ![SharePoint'i yapılandırma][3]     
5. Kimlik doğrulaması tamamlandıktan sonra mantıksal uygulamanıza yönlendirilirsiniz. İşte bu kadar bağlantı oluşturuldu. SharePoint için artık bağlısınız gösteren alttaki iletisini görürsünüz.  
   ![SharePoint'i yapılandırma][4]  
6. Ardından, diğer tetikleyiciler ve mantıksal uygulamanızı tamamlamak için gereken eylemler ekleyebilirsiniz.   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png
