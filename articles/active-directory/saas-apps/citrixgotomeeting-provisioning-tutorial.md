---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı GoToMeeting yapılandırma | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile GoToMeeting arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 86da9d692bf814e62579d75dedd0089b5e6f6527
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982145"
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Öğretici: GoToMeeting otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı GoToMeeting ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den GoToMeeting sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir GoToMeeting çoklu oturum açma abonelik etkin.
*   Bir kullanıcı hesabında GoToMeeting takım yönetici izinlerine sahip.

## <a name="assigning-users-to-gotomeeting"></a>Kullanıcılar için GoToMeeting atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları GoToMeeting uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar GoToMeeting uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Kullanıcılar için GoToMeeting atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için GoToMeeting atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için GoToMeeting atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD GoToMeeting'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre GoToMeeting atanan kullanıcı hesaplarında devre dışı bırakın.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için GoToMeeting, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik olarak bir kullanıcı hesabı sağlama yapılandırmak için:

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için GoToMeeting zaten yapılandırdıysanız arama alanı kullanarak GoToMeeting Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **GoToMeeting** uygulama galerisinde. Arama sonuçlarından GoToMeeting seçin ve uygulamaları listenize ekleyin.

3. GoToMeeting örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama** moduna **otomatik**. 

    ![sağlama](./media/citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. Yönetici kimlik bilgileri bölümü altında aşağıdaki adımları gerçekleştirin:
   
    a. İçinde **GoToMeeting yönetici kullanıcı adı** metin kutusuna, bir yönetici kullanıcı adını yazın.

    b. İçinde **GoToMeeting yönetici parolası** metin kutusuna, yöneticinin parola.

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD GoToMeeting uygulamanıza bağlanabilir. Bağlantı başarısız olursa GoToMeeting hesabınızın Team yönetici izinlerine sahip olduğundan emin olun ve deneyin **"Yönetici kimlik bilgileri"** adım yeniden uygulayın.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları GoToMeeting.**

10. İçinde **öznitelik eşlemelerini** bölümünde, GoToMeeting için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri GoToMeeting kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD hizmeti GoToMeeting için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

12. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde GoToMeeting atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve GoToMeeting uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrix-gotomeeting-tutorial)


