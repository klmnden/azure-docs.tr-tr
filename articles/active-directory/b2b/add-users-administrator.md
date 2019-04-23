---
title: Azure portal - Azure Active Directory B2B işbirliği kullanıcıları ekleme | Microsoft Docs
description: Nasıl yönetici Konuk kullanıcılar kendi dizin için Azure Active Directory (Azure AD) B2B işbirliğini kullanarak iş ortağı kuruluştan ekleyebileceğinizi gösterir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/10/2019
ms.author: mimart
author: msmimart
manager: celested
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef0a4e4a05427b1ed83b017b7a49862596aaf50a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799414"
---
# <a name="add-azure-active-directory-b2b-collaboration-users-in-the-azure-portal"></a>Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme

Dizin sınırlı Yönetici rolüne atanan kullanıcı, B2B işbirliği kullanıcıları davet etmek için Azure portalını kullanabilirsiniz. Dizin, bir grup veya uygulamanın Konuk kullanıcılar davet edebilirsiniz. Bu yöntemlerin herhangi biriyle bir kullanıcıyı davet sonra davet edilen kullanıcının hesabı Azure Active Directory (Azure AD), bir kullanıcı türü ile eklenir *Konuk*. Konuk kullanıcı daha sonra kaynaklara erişmek için davetini gerekir.

Dizinine Konuk kullanıcı ekledikten sonra ya da Konuk kullanıcı doğrudan bağlantısını için paylaşılan bir uygulama gönderebilirsiniz veya Konuk kullanıcı davet e-posta kullanım URL'de tıklayabilirsiniz. Alma işlemi hakkında daha fazla bilgi için bkz. [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md).

> [!IMPORTANT]
> Adımları izlemelidir [nasıl yapılır: Azure Active Directory'de, kuruluşunuzun gizlilik bilgisi eklemek](https://aka.ms/adprivacystatement) kuruluşunuzun gizlilik bildirimi URL'si eklemek için. İlk zaman davet alma işleminin bir parçası olarak, devam etmek için gizlilik koşullarını bir davet edilen kullanıcının onaylaması gerekir. 

## <a name="add-guest-users-to-the-directory"></a>Konuk kullanıcıları dizine ekleme

B2B işbirliği kullanıcıları dizine eklemek için aşağıdaki adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com) herhangi birini sınırlı yönetici olan bir kullanıcı dizin rolleri atanmış olarak.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. **Yeni konuk kullanıcı**’yı seçin.

   ![Yeni Konuk kullanıcı Arabiriminde burada gösterilir](./media/add-users-administrator/NewGuestUser-Directory.png) 
 
   > [!NOTE]
   > **Yeni Konuk kullanıcı** seçeneği de üzerinde kullanılabilir **kuruluş ilişkileri** sayfası. İçinde **Azure Active Directory**altında **Yönet**seçin **kuruluş ilişkileri**.

5. **Kullanıcı adı** bölümüne, harici kullanıcının e-posta adresini girin. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin. Örneğin:

   ![Yeni Konuk kullanıcı Arabiriminde burada gösterilir](./media/add-users-administrator/InviteGuest.png) 

    > [!NOTE]
    > Grup e-posta adreslerini desteklenmez; tek bir e-posta adresi girin. Ayrıca, bazı e-posta sağlayıcıları artı eklemek kullanıcıların (+) simgesini ve gelen kutusu filtreleme gibi şeyler yardımcı olmak için e-posta adreslerini ek metni. Ancak, Azure AD, e-posta adreslerini simgeler ayrıca şu anda desteklemiyor. Teslim sorunları önlemek için artı simgesini ve herhangi bir karakter kadar aşağıdaki atlayın @ sembolü.

6. Konuk kullanıcıya otomatik olarak daveti göndermek için **Davet Et**’i seçin. 
 
Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.


![B2B kullanıcı Konuk kullanıcı türü ile gösterir](./media/add-users-administrator/GuestUserType.png)  

## <a name="add-guest-users-to-a-group"></a>Konuk kullanıcıları gruba ekleyin.
B2B işbirliği kullanıcıları el ile bir grup Azure AD Yöneticisi olarak eklemek gerekiyorsa, aşağıdaki adımları izleyin:

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **grupları**.
4. Bir grubu seçin (veya **yeni grup** yeni bir tane oluşturmak için). Grubu B2B Konuk kullanıcıları içeren grubu tanımı eklemek iyi bir fikirdir.
5. Seçin **üyeleri**. 
6. Aşağıdakilerden birini yapın:
   - Konuk kullanıcı dizinde zaten varsa, B2B kullanıcısını aramak. Kullanıcıyı seçin ve ardından **seçin** kullanıcı grubuna eklemek için.
   - Dizinde Konuk kullanıcı zaten mevcut değilse bunları gruba e-posta adresi arama kutusuna isteğe bağlı bir kişisel ileti yazarak ve ardından davet **seçin**. Davet otomatik olarak davet edilen kullanıcının gider.
     
     ![Konuk üyeler eklemek için davet et düğmesi ekleme](./media/add-users-administrator/GroupInvite.png)
   
Bu gibi durumlarda, dinamik gruplar da Azure AD B2B işbirliği ile kullanabilirsiniz. Daha fazla bilgi için [dinamik gruplar ve Azure Active Directory B2B işbirliği](use-dynamic-groups.md).

## <a name="add-guest-users-to-an-application"></a>Konuk kullanıcılar için uygulama ekleme

B2B işbirliği kullanıcıları Azure AD yönetici olarak bir uygulama eklemek için aşağıdaki adımları izleyin:

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **kurumsal uygulamalar** > **tüm uygulamaları**.
4. Konuk kullanıcıları eklemek istediğiniz uygulamayı seçin.
5. Uygulamanın Panoda seçin **toplam kullanıcı** açmak için **kullanıcılar ve gruplar** bölmesi.

    ![Toplam açık kullanıcılar ve gruplar eklemek için kullanıcıların düğmesi](./media/add-users-administrator/AppUsersAndGroups.png)

6. Seçin **Kullanıcı Ekle**.
7. Altında **atama Ekle**seçin **kullanıcı ve grupları**.
8. Aşağıdakilerden birini yapın:
   - Konuk kullanıcı dizinde zaten varsa, B2B kullanıcısını aramak. Kullanıcı seçin, **seçin**ve ardından **atama** kullanıcı uygulamaya ekleyin.
   - Konuk kullanıcı zaten bir dizinde altında yoksa **üye seçin veya bir dış kullanıcıyı davet**, kullanıcının e-posta adresini yazın. İleti kutusunda isteğe bağlı kişisel bir ileti yazın. İleti kutusu altında tıklatın **davet**.
           
       ![Konuk üyeler eklemek için davet et düğmesi ekleme](./media/add-users-administrator/AppInviteUsers.png)
   
      Tıklayın **seçin**ve ardından **atama** kullanıcı uygulamaya ekleyin. Davetiye otomatik olarak davet edilen kullanıcının gider.

9. Konuk kullanıcı uygulamanın görünür **kullanıcılar ve gruplar** atanan rolü listesiyle **varsayılan erişim**. Rol değiştirmek istiyorsanız, aşağıdakileri yapın:
   - Konuk kullanıcı seçin ve ardından **Düzenle**. 
   - Altında **ataması Düzenle**, tıklayın **rolü Seç**ve seçili kullanıcıya atamak istediğiniz rolü seçin.
   - **Seç**'e tıklayın.
   - **Ata**'ya tıklayın.
 
## <a name="resend-invitations-to-guest-users"></a>Konuk kullanıcılar Davet Gönder

Konuk kullanıcı henüz davetini kuponları değil, davet e-postayı yeniden gönder.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
5. Kullanıcı hesabını seçin.
6. Altında **Yönet**seçin **profili**.
7. Kullanıcı henüz davetini kabul etmediyse bir **daveti yeniden gönder** seçeneği kullanılabilir. Yeniden göndermek için bu düğmeyi seçin.

   ![Kullanıcı profili davet seçeneği yeniden gönder](./media/add-users-administrator/Resend-Invitation.png)

> [!NOTE]
> Başlangıçta kullanıcının belirli bir uygulamaya yönlendiren bir daveti yeniden gönder, yeni davet bağlantısını kullanıcı üst düzey erişim paneline yönlendiren yerine aldığını anlayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure olmayan AD yöneticileri B2B Konuk kullanıcıları nasıl ekleyebileceğinizi öğrenmek için bkz [nasıl bilgi çalışanları ekleme B2B işbirliği kullanıcıları?](add-users-information-worker.md)
- Davet e-posta hakkında daha fazla bilgi için bkz. [B2B işbirliği davet e-posta öğelerinden](invitation-email-elements.md).

