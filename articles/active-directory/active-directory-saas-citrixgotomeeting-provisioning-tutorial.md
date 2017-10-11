---
title: "Öğretici: Azure Active Directory Tümleştirme ile Citrix GoToMeeting | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Citrix GoToMeeting arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Öğretici: Citrix GoToMeeting otomatik kullanıcı sağlamayı için yapılandırma

Bu öğreticinin amacı Citrix GoToMeeting ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den Citrix GoToMeeting sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir Citrix GoToMeeting çoklu oturum açma etkin abonelik.
*   Citrix GoToMeeting takım yönetici izinlerine sahip bir kullanıcı hesabının.

## <a name="assigning-users-to-citrix-gotomeeting"></a>Kullanıcılar için Citrix GoToMeeting atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Citrix GoToMeeting uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Citrix GoToMeeting uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a>Kullanıcılar için Citrix GoToMeeting atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Citrix GoToMeeting atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Citrix GoToMeeting atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD Citrix GoToMeeting'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve kullanıcı ve Grup Citrix GoToMeeting hesaplarında göre atanan kullanıcı devre dışı bırak Azure AD'de atama.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Citrix GoToMeeting, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik olarak bir kullanıcı hesabı sağlama yapılandırmak için:

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Citrix GoToMeeting zaten yapılandırdıysanız Citrix arama alanı kullanarak GoToMeeting Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Citrix GoToMeeting** uygulama galerisinde. Arama sonuçlarından Citrix GoToMeeting seçin ve uygulamaları listenize ekleyin.

3. Citrix GoToMeeting örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama** moduna **otomatik**. 

    ![Sağlama](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. Yönetici kimlik bilgileri bölümü altında aşağıdaki adımları gerçekleştirin:
   
    a. İçinde **Citrix GoToMeeting yönetici kullanıcı adı** metin kutusuna, bir yönetici kullanıcı adını yazın.

    b. İçinde **Citrix GoToMeeting yönetici parolası** metin kutusuna, yöneticinin parola.

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Citrix GoToMeeting uygulamanıza bağlanabilir. Bağlantı başarısız olursa, Citrix GoToMeeting hesabınızın Team yönetici izinleri olduğundan emin olun ve deneyin **"Yönetici kimlik bilgileri"** adım yeniden uygulayın.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için Citrix GoToMeeting.**

10. İçinde **öznitelik eşlemelerini** bölümünde, Citrix GoToMeeting Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Citrix GoToMeeting kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Citrix GoToMeeting için hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

12. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde Citrix GoToMeeting atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Citrix GoToMeeting uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-citrix-gotomeeting-tutorial.md)


