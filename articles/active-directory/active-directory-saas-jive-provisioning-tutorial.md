---
title: "Öğretici: Azure Active Directory Tümleştirme ile Jive | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Jive arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 957b152fdd40d08a867e788b0cb9f7d57ed481e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a>Öğretici: Jive kullanıcı sağlamak için yapılandırma

Bu öğreticinin amacı Jive ve Azure AD Jive için Azure AD'den otomatik sağlama ve devre dışı bırakma sağlama kullanıcı hesapları gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir Jive çoklu oturum açma etkin abonelik.
*   Bir kullanıcı hesabında Jive takım yönetici izinlerine sahip.

## <a name="assigning-users-to-jive"></a>Kullanıcılar için Jive atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Jive uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Jive uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Kullanıcılar için Jive atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Jive atanabilir. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Jive atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD Jive'nın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre Jive atanan kullanıcı hesaplarında devre dışı bırakın.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için Jive etkin, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Jive kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.
Bu yordam bir parçası olarak, Jive.com istemek için gereken bir kullanıcı güvenlik belirteci sağlamak için gereklidir.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Jive zaten yapılandırdıysanız arama alanı kullanarak Jive Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Jive** uygulama galerisinde. Arama sonuçlarından Jive seçin ve uygulamaları listenize ekleyin.

3. Jive örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![Sağlama](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlar:
   
    a. İçinde **Jive yönetici kullanıcı adı** metin kutusuna, bir Jive hesap adı türü **Sistem Yöneticisi** atanan Jive.com profilinde.
   
    b. İçinde **Jive yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.
   
    c. İçinde **Jive Kiracı URL** metin kutusuna, Jive Kiracı URL'sini yazın.
      
      > [!NOTE]
      > Jive Kiracı için Jive oturum açmak için kuruluşunuz tarafından kullanılan URL'dir.  
      > Genellikle, URL'si aşağıdaki biçime sahiptir: **www.\< Kuruluş\>. jive.com**.          

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Jive uygulamanıza bağlanabilir.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Jive.**

10. İçinde **öznitelik eşlemelerini** bölümünde, Jive için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Jive kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD hizmeti Jive için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

12. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde Jive atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Jive uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Şimdi sınama hesabı oluşturabilirsiniz. Hesap için Jive eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-jive-tutorial.md)