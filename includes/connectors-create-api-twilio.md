---
ms.openlocfilehash: 845b27b8efd66de87ec08e0b5b81bcc332dffdfb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129975"
---
### <a name="prerequisites"></a>Önkoşullar
* Twilio hesap
* SMS alabilecek doğrulanmış bir Twilio telefon numarası
* SMS gönderebilen doğrulanmış bir Twilio telefon numarası

> [!NOTE]
> Bir Twilio deneme hesabı kullanıyorsanız, SMS için yalnızca gönderebilirsiniz **doğrulandı** telefon numaraları.  
> 
> 

Twilio hesabınızın bir mantıksal uygulama kullanabilmeniz için önce Twilio hesabınıza bağlanmak için mantıksal uygulama yetkilendirmeniz gerekir. Neyse ki, Azure Portalı'nda mantıksal uygulama içinde bunu kolayca yapabilirsiniz. 

Mantıksal uygulamanızı Twilio hesabınıza bağlanmak için yetki vermek için adımlar şunlardır:

1. Mantıksal Uygulama Tasarımcısı'nda, Twilio bağlantı oluşturmak için seçin **yönetilen API'leri Göster Microsoft** açılan liste enter *Twilio* arama kutusuna. Tetikleyici veya eylemi kullanmak ister seçin:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Twilio'ya önce herhangi bir bağlantı oluşturmadıysanız, Twilio kimlik bilgilerinizi sağlamanız girmem isteniyor. Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak için yetki vermek için kullanılan ve Twilio hesabınızın veri erişimi:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. İhtiyacınız olacak **Twilio hesap kimliği** ve **Twilio erişim belirteci** Twilio, panodan, bu nedenle Twilio hesabınızın şimdi bu iki bilgi parçasını almak oturum açın:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio ve Logic apps, bu iki bilgi parçasını belirlemek için farklı adlar kullanın. İşte nasıl bunları için Logic apps iletişim eşlemeniz gerekir: ![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Seçin **Mumbai'ye** düğmesi:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Bağlantı oluşturuldu ve artık mantıksal uygulamanızdaki diğer adımlarla devam etmek ücretsiz, dikkat edin:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

