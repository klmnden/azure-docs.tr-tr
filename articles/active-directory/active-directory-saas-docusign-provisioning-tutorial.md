---
title: "Öğretici: Azure Active Directory Tümleştirme ile DocuSign | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile DocuSign arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 3b509ffa934949200277ae431761d2accd4a02d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a>Öğretici: DocuSign kullanıcı sağlamak için yapılandırma

Bu öğreticinin amacı DocuSign ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den DocuSign sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir DocuSign çoklu oturum açma abonelik etkin.
*   Bir kullanıcı hesabında DocuSign takım yönetici izinlerine sahip.

## <a name="assigning-users-to-docusign"></a>Kullanıcılar için DocuSign atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları DocuSign uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar DocuSign uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>Kullanıcılar için DocuSign atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için DocuSign atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için DocuSign atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD DocuSign'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre DocuSign atanan kullanıcı hesaplarında devre dışı bırakın.

> [!Tip]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için DocuSign, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının DocuSign kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için DocuSign zaten yapılandırdıysanız arama alanı kullanarak DocuSign Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **DocuSign** uygulama galerisinde. Arama sonuçlarından DocuSign seçin ve uygulamaları listenize ekleyin.

3. DocuSign örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![Sağlama](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlar:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna, bir DocuSign hesap adı türü **Sistem Yöneticisi** atanan DocuSign.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD DocuSign uygulamanıza bağlanabilir.

7. İçinde **bildirim e-posta** alan, bir kişi veya grubun sağlama hata bildirimleri almak ve gerekir onay e-posta adresini girin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları DocuSign.**

10. İçinde **öznitelik eşlemelerini** bölümünde, DocuSign için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri DocuSign kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD hizmeti DocuSign için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

12. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde DocuSign atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve DocuSign uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Şimdi sınama hesabı oluşturabilirsiniz. Hesap için DocuSign eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-docusign-tutorial.md)