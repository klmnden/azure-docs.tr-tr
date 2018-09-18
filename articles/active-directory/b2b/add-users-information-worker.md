---
title: Bir bilgi çalışanı - Azure Active Directory B2B işbirliği kullanıcıları ekleme | Microsoft Docs
description: Bilgi çalışanları ve uygulama sahipleri erişim için Azure AD'ye Konuk kullanıcıları eklemek B2B işbirliğine olanak tanır | Microsoft Docs
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: e590500dd622988226c592352b0b86f16d54a9d4
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983072"
---
# <a name="how-users-in-your-organization-can-invite-guest-users-to-an-app"></a>Nasıl kuruluşunuzdaki kullanıcılar uygulama Konuk kullanıcılar davet edebilir

Sonra Konuk kullanıcı eklendiğini dizine Azure AD'de uygulama sahibinin Konuk kullanıcı paylaşmak istediğiniz uygulamaya doğrudan bağlantı gönderebilirsiniz. Uygulama sahipleri kendi Konuk kullanıcılar yönetebilmeniz için azure AD yöneticileri Konuk kullanıcıları dizine henüz bile eklenmediyseniz Self Servis yönetimini de ayarlayabilirsiniz. Bir uygulama için Self Servis yapılandırıldığında, uygulama sahibi kendi erişim panellerine uygulamaya Konuk kullanıcı davet veya bir gruba uygulama erişimi olan Konuk kullanıcı Ekle için kullanır. Self Servis uygulama yönetimi, bir yönetici tarafından bazı ilk kurulum gerektirir Kurulum adımları bir özeti aşağıda verilmiştir (daha ayrıntılı yönergeler için bkz. [önkoşulları](#prerequisites) daha sonra bu sayfayı):

 - Kiracınız için Self Servis Grup Yönetimi etkinleştir
 - Uygulamaya atamak ve kullanıcı sahibi olmak için bir grup oluşturun
 - Self Servis uygulamasını yapılandırma ve uygulamaya grup atama

## <a name="invite-a-guest-user-to-an-app-from-the-access-panel"></a>Konuk kullanıcı bir uygulamaya erişim panelinden davet edin.

Bir uygulama için Self Servis yapılandırdıktan sonra uygulama sahibi kendi erişim paneli paylaşmak istediğiniz uygulama için bir Konuk kullanıcı davet etmek için kullanabilirsiniz. Konuk kullanıcı mutlaka önceden Azure AD'ye eklemiş gerekmez. 

1. Giderek, erişim paneli `https://myapps.microsoft.com`.
2. Uygulamaya gelin, elipsleri seçin (**...** ) ve ardından **Yönet uygulama**.
 
   ![Uygulama erişim panellerinde yönetme](media/add-users-iw/access-panel-manage-app.png)
 
3. Kullanıcılar listenin en üstünde seçin **+**.
   
   ![Erişim paneli, bir kullanıcı ekleyin](media/add-users-iw/access-panel-manage-app-add-user.png)
   
4. İçinde **üye ekleme** arama kutusuna, Konuk kullanıcı için e-posta adresini yazın. İsteğe bağlı olarak bir karşılama iletisi içerir.
   
   ![Erişim paneli davet](media/add-users-iw/access-panel-invitation.png)
   
5. Seçin **Ekle** Konuk kullanıcı davet gönderilecek. Davet gönderdikten sonra kullanıcı hesabı dizinine konuk olarak otomatik olarak eklenir.

## <a name="invite-someone-to-join-a-group-that-has-access-to-the-app"></a>Uygulama erişimi olan bir gruba katılması için davet
Bir uygulama için Self Servis yapılandırdıktan sonra uygulama sahipleri paylaşmak istediğiniz uygulamaları erişimi yönettikleri gruplara Konuk kullanıcılar davet edebilirsiniz. Konuk kullanıcıları dizinde zaten gerekmez. Uygulama sahibi uygulama erişebilmesi için grubuna Konuk kullanıcı davet etmek için aşağıdaki adımları izleyin.

1. Paylaşmak istediğiniz uygulama erişimi olan Self Servis Grup sahibi olduğunuzdan emin olun.
2. Giderek, erişim paneli `https://myapps.microsoft.com`.
3. Seçin **grupları** uygulama.
   
   ![Erişim paneli grupları uygulama](media/add-users-iw/access-panel-groups.png)
   
4. Altında **olduğum gruplar**, paylaşmak istediğiniz uygulama erişimi olan grubu seçin.
   
   ![Kendi erişim paneli-gruplar](media/add-users-iw/access-panel-groups-i-own.png)
   
5. Grup üyeleri listenin en üstünde seçin **+**.
   
   ![Üye erişim paneli-gruplar ekleyin](media/add-users-iw/access-panel-groups-add-member.png)
   
6. İçinde **üye ekleme** arama kutusuna, Konuk kullanıcı için e-posta adresini yazın. İsteğe bağlı olarak bir karşılama iletisi içerir.
   
   ![Erişim paneli grubu davet](media/add-users-iw/access-panel-invitation.png)
   
7. Seçin **Ekle** otomatik olarak Konuk kullanıcı davet göndermek için. Davet gönderdikten sonra kullanıcı hesabı dizinine konuk olarak otomatik olarak eklenir.


## <a name="prerequisites"></a>Önkoşullar

Self Servis uygulama yönetimi, genel yönetici ve bir Azure AD Yöneticisi tarafından bazı ilk kurulum gerektirir. Bu kurulumunun bir parçası olarak, Self Servis uygulamasını yapılandırma ve uygulama sahibi yönetebileceği uygulamaya bir gruba atayın. Ayrıca grubun üyelik istemek bir grup sahibinin onayı iste herkesin izin verecek şekilde yapılandırabilirsiniz. (Daha fazla bilgi edinin [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management).) 

> [!NOTE]
> Konuk kullanıcıların dinamik bir grup veya şirket içi Active Directory ile eşitlenen bir gruba eklenemiyor.

### <a name="enable-self-service-group-management-for-your-tenant"></a>Kiracınız için Self Servis Grup Yönetimi etkinleştir
1. Oturum [Azure portalında](https://portal.azure.com) genel Yöneticisi olarak.
2. Gezinti panelinde seçin **Azure Active Directory**.
3. Seçin **grupları**.
4. Altında **ayarları**seçin **genel**.
5. Altında **Self Servis Grup Yönetimi**yanındaki **sahipler, erişim Paneli'nde grup üyeliği isteklerini yönetebilir**seçin **Evet**.
6. **Kaydet**’i seçin.

### <a name="create-a-group-to-assign-to-the-app-and-make-the-user-an-owner"></a>Uygulamaya atamak ve kullanıcı sahibi olmak için bir grup oluşturun
1. Oturum [Azure portalında](https://portal.azure.com) bir Azure AD Yöneticisi veya genel yönetici olarak.
2. Gezinti panelinde seçin **Azure Active Directory**.
3. Seçin **grupları**.
4. Seçin **yeni grup**.
5. Altında **grup türü**seçin **güvenlik**.
6. Tür a **grup adı** ve **Grup açıklaması**.
7. Altında **üyelik türü**seçin **atanan**.
8. Seçin **Oluştur**ve Kapat **grubu** sayfası.
9. Üzerinde **gruplar - tüm gruplar** sayfasında, grubun açın. 
10. Altında **Yönet**seçin **sahipleri** > **sahipler eklemeyi**. Uygulama erişimi yönetmesi gereken kullanıcıyı arayın. Kullanıcıyı seçin ve ardından **seçin**.

### <a name="configure-the-app-for-self-service-and-assign-the-group-to-the-app"></a>Self Servis uygulamasını yapılandırma ve uygulamaya grup atama
1. Oturum [Azure portalında](https://portal.azure.com) bir Azure AD Yöneticisi veya genel yönetici olarak.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **kurumsal uygulamalar** > **tüm uygulamaları**.
4. Uygulama listesinde, bulmak ve uygulamayı açın.
5. Altında **Yönet**seçin **çoklu oturum açma**ve uygulama için çoklu oturum açmayı yapılandırın. (Ayrıntılar için bkz [kurumsal uygulamalar için çoklu oturum açmayı yönetme](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).)
6. Altında **Yönet**seçin **Self Servis**ve Self Servis uygulama erişimini ayarlama. (Ayrıntılar için bkz [Self Servis uygulama erişiminin nasıl kullanıldığını](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to).) 
    > [!NOTE]
    > Ayar için **atanan kullanıcılar hangi gruba eklenir?** önceki bölümde oluşturduğunuz grubunu seçin.
7. Altında **Yönet**seçin **kullanıcılar ve gruplar**, oluşturduğunuz Self Servis Grup listesinde göründüğünü doğrulayın.
8. Uygulama grubu sahibinin erişim paneline yönlendiren eklemek için seçin **Kullanıcı Ekle** > **kullanıcılar ve gruplar**. Grup sahibi için arama yapın ve kullanıcı seçin, **seçin**ve ardından **atama** kullanıcı uygulamaya ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-administrator.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [Azure AD B2B işbirliği lisanslama](licensing-guidance.md)
