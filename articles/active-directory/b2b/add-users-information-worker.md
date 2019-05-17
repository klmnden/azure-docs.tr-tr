---
title: Bir bilgi çalışanı - Azure Active Directory B2B işbirliği kullanıcıları ekleme | Microsoft Docs
description: Bilgi çalışanları ve uygulama sahipleri erişim için Azure AD'ye Konuk kullanıcıları eklemek B2B işbirliğine olanak tanır | Microsoft Docs
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8606a0d4e203e1a910a5cd15ca83a622f5286bd
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65812543"
---
# <a name="how-users-in-your-organization-can-invite-guest-users-to-an-app"></a>Nasıl kuruluşunuzdaki kullanıcılar uygulama Konuk kullanıcılar davet edebilir

Sonra Konuk kullanıcı eklendiğini dizine Azure AD'de uygulama sahibinin Konuk kullanıcı paylaşmak istediğiniz uygulamaya doğrudan bağlantı gönderebilirsiniz. Azure AD yöneticileri, Self Servis Yönetimi galeri veya kendi Azure AD kiracısında SAML tabanlı uygulamalar için de ayarlayabilirsiniz. Konuk kullanıcıları dizine henüz bile eklemediyseniz bu şekilde, uygulama sahibi kendi Konuk kullanıcılar yönetebilirsiniz. Bir uygulama için Self Servis yapılandırıldığında, uygulama sahibi kendi erişim panellerine uygulamaya Konuk kullanıcı davet veya bir gruba uygulama erişimi olan Konuk kullanıcı Ekle için kullanır. Galeri ve SAML tabanlı uygulamaları için Self Servis uygulama yönetimi, bir yönetici tarafından bazı ilk kurulum gerektirir Kurulum adımları bir özeti aşağıda verilmiştir (daha ayrıntılı yönergeler için bkz. [önkoşulları](#prerequisites) daha sonra bu sayfayı):

 - Kiracınız için Self Servis Grup Yönetimi etkinleştir
 - Uygulamaya atamak ve kullanıcı sahibi olmak için bir grup oluşturun
 - Self Servis uygulamasını yapılandırma ve uygulamaya grup atama

> [!NOTE]
> Bu makalede, Galeri ve Azure AD kiracınıza eklediğiniz SAML tabanlı uygulamaları için Self Servis Yönetimi ayarlamak açıklar. Ayrıca [Self Servis Office 365 grupları ayarlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management) kullanıcılarınız kendi Office 365 grupları erişimi yönetebilirsiniz. Konuk kullanıcılarla daha yollarını kullanıcılar Office dosyalarını ve uygulamaları paylaşmak için bkz: [Konuk erişimi Office 365 gruplarında](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6) ve [SharePoint paylaşım dosyaları veya klasörleri](https://support.office.com/article/share-sharepoint-files-or-folders-1fe37332-0f9a-4719-970e-d2578da4941c).

## <a name="invite-a-guest-user-to-an-app-from-the-access-panel"></a>Konuk kullanıcı bir uygulamaya erişim panelinden davet edin.

Bir uygulama için Self Servis yapılandırdıktan sonra uygulama sahibi kendi erişim paneli paylaşmak istediğiniz uygulama için bir Konuk kullanıcı davet etmek için kullanabilirsiniz. Konuk kullanıcı mutlaka önceden Azure AD'ye eklemiş gerekmez. 

1. Giderek, erişim paneli `https://myapps.microsoft.com`.
2. Uygulamaya gelin, elipsleri seçin (**...** ) ve ardından **Yönet uygulama**.
 
   ![Doğrudan Salesforce uygulamasının Yönet uygulama alt menüsünü gösteren ekran görüntüsü](media/add-users-iw/access-panel-manage-app.png)
 
3. Kullanıcılar listenin en üstünde seçin **+**.
   
   ![Uygulamaya üyeleri eklemek için artı simgesini gösteren ekran görüntüsü](media/add-users-iw/access-panel-manage-app-add-user.png)
   
4. İçinde **üye ekleme** arama kutusuna, Konuk kullanıcı için e-posta adresini yazın. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin.
   
   ![Ekleme gösteren ekran görüntüsü ekleme Konuk üyeleri penceresi](media/add-users-iw/access-panel-invitation.png)
   
5. Seçin **Ekle** Konuk kullanıcı davet gönderilecek. Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.

## <a name="invite-someone-to-join-a-group-that-has-access-to-the-app"></a>Uygulama erişimi olan bir gruba katılması için davet
Bir uygulama için Self Servis yapılandırdıktan sonra uygulama sahipleri paylaşmak istediğiniz uygulamaları erişimi yönettikleri gruplara Konuk kullanıcılar davet edebilirsiniz. Konuk kullanıcıları dizinde zaten gerekmez. Uygulama sahibi uygulama erişebilmesi için grubuna Konuk kullanıcı davet etmek için aşağıdaki adımları izleyin.

1. Paylaşmak istediğiniz uygulama erişimi olan Self Servis Grup sahibi olduğunuzdan emin olun.
2. Giderek, erişim paneli `https://myapps.microsoft.com`.
3. Seçin **grupları** uygulama.
   
   ![Gruplar uygulama erişim panelinde gösteren ekran görüntüsü](media/add-users-iw/access-panel-groups.png)
   
4. Altında **olduğum gruplar**, paylaşmak istediğiniz uygulama erişimi olan grubu seçin.
   
   ![Sahip olduğum gruplar altındaki bir grubu seçmek nereye gösteren ekran görüntüsü](media/add-users-iw/access-panel-groups-i-own.png)
   
5. Grup üyeleri listenin en üstünde seçin **+**.
   
   ![Gruba üye eklemek için artı simgesini gösteren ekran görüntüsü](media/add-users-iw/access-panel-groups-add-member.png)
   
6. İçinde **üye ekleme** arama kutusuna, Konuk kullanıcı için e-posta adresini yazın. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin.
   
   ![Ekleme gösteren ekran görüntüsü ekleme Konuk üyeleri penceresi](media/add-users-iw/access-panel-invitation.png)
   
7. Seçin **Ekle** otomatik olarak Konuk kullanıcı davet göndermek için. Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.


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
6. Bir **Grup adı** ve **Grup açıklaması** girin.
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
- [Azure AD B2B işbirliği lisanslaması](licensing-guidance.md)
