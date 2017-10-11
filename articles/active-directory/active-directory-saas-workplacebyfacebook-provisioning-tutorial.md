---
title: "Öğretici: Azure Active Directory Tümleştirme ile çalışma alanına Facebook tarafından | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Facebook ile çalışma alanına arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 9b22679c304248ed7ba7a6bd9eaf82b64f7143cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a>Öğretici: Kullanıcı sağlamak için çalışma alanına Facebook ile yapılandırma

Bu öğreticinin amacı, Facebook ve Azure AD otomatik olarak sağlamak ve Azure AD kullanıcı hesaplarından çalışma Facebook tarafından sağlanmasını tarafından çalışma gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Ön koşullar

Facebook ile çalışma alanına ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Facebook çoklu oturum açma etkin abonelik tarafından bir çalışma alanı

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-workplace-by-facebook"></a>Kullanıcıların Facebook ile çalışma alanına atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Facebook uygulama tarafından işyeriniz erişmek isteyen kullanıcıların temsil eden karar vermeniz gerekir. Karar sonra bu kullanıcılar çalışma alanınıza Facebook uygulama tarafından Buradaki yönergeleri izleyerek atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a>Facebook ile çalışma alanına kullanıcılara atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının atanması çalışma alanına Facebook tarafından sağlama yapılandırmayı test etme. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için çalışma alanına Facebook tarafından atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde API sağlama Facebook'ın kullanıcı hesabı tarafından Azure AD çalışma alanına konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve çalışma Azure AD'de kullanıcı ve grup atama göre Facebook tarafından atanan kullanıcı hesapları devre dışı bırak.

>[!Tip]
>Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma Facebook tarafından çalışma alanı için yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Kullanıcı hesap için çalışma alanına Facebook tarafından Azure AD'de sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesapları için çalışma alanına Facebook tarafından sağlanmasını etkinleştirme anahat sağlamaktır.

Azure AD çalışma alanına Facebook tarafından atanan kullanıcı hesabı ayrıntıları otomatik olarak eşitleme özelliği destekler. Bu otomatik eşitleme ilk kez oturum açmaya önlerinde erişim için Kullanıcıları yetkilendirmek için gereken verileri almak için Facebook ile çalışma alanına sağlar. Erişim Azure AD'de iptal edilmiş durumda olduğunda, ayrıca çalışma alanına Facebook tarafından kullanıcılardan XML'deki sağlamasını yapar.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Facebook ile çalışma alanına zaten yapılandırdıysanız arama alanı kullanarak Facebook ile çalışma alanına örneğiniz arayın. Aksi takdirde seçin **Ekle** arayın ve **Facebook ile çalışma alanına** uygulama galerisinde. Arama sonuçlarından Facebook tarafından çalışma alanı seçin ve uygulamaları listenize ekleyin.

3. Facebook ile çalışma alanına örneğiniz seçin, sonra seçin **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![Sağlama](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, gizli belirteci ve işyeriniz Facebook yönetici tarafından Kiracı URL'sini girin.

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD çalışma alanına Facebook uygulaması tarafından bağlanabilirler. Bağlantı başarısız olursa işyeriniz Facebook hesabına göre takım yönetici izinleri olduğundan emin olun.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Facebook ile çalışma.**

10. İçinde **öznitelik eşlemelerini** bölümünde, Facebook ile eşitlenmiş Azure AD çalışma alanına kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikler kullanılan çalışma alanına kullanıcı hesaplarında eşleştirmek için Facebook tarafından güncelleştirme işlemleri için. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD çalışma alanına Facebook tarafından hizmet sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. Tıklatın **kaydedin.**

Otomatik sağlama yapılandırma hakkında daha fazla bilgi için bkz: [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

Şimdi sınama hesabı oluşturabilirsiniz. Hesap çalışma alanına Facebook ile eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-workplacebyfacebook-tutorial.md)

