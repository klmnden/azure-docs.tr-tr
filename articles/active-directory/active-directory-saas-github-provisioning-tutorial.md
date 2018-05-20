---
title: 'Öğretici: Azure Active Directory ile otomatik olarak bir kullanıcı sağlamak için GitHub yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve GitHub kullanıcı hesaplarına sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.openlocfilehash: 25ab5e2628b312ae508f17cc80b945700f034273
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-github-for-automatic-user-provisioning"></a>Öğretici: GitHub otomatik kullanıcı sağlamayı yapılandırın


Bu öğreticinin amacı, GitHub ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den GitHub sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   Github Kiracı ile [iş planı](https://help.github.com/articles/organization-billing-plans/#business-plan) veya daha iyi etkin 
*   GitHub yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE]
> Azure AD tümleştirme sağlama dayanan [GitHub SCIM'yi API](https://developer.github.com/v3/scim/), Github takımlara iş plan üzerinde kullanılabilir veya daha iyi.

## <a name="assigning-users-to-github"></a>Kullanıcılar için GitHub atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları GitHub uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar GitHub uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a>Kullanıcılar için GitHub atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için GitHub atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için GitHub atarken ya da seçmelisiniz **kullanıcı** rol ya da başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. **Varsayılan erişim** rol sağlamak için çalışmaz ve bu kullanıcılar atlandı.


## <a name="configuring-user-provisioning-to-github"></a>Kullanıcı için GitHub sağlama yapılandırma 

Bu bölümde Azure AD GitHub'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre github'da atanan kullanıcı hesapları devre dışı bırak.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için GitHub, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için GitHub Azure AD'de sağlamayı Yapılandır


1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için GitHub zaten yapılandırdıysanız arama alanı kullanarak GitHub Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **GitHub** uygulama galerisinde. Arama sonuçlarından GitHub seçin ve uygulamaları listenize ekleyin.

3. GitHub örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![GitHub sağlama](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. Altında **yönetici kimlik bilgileri** 'yi tıklatın **Authorize**. Bu işlem, yeni bir tarayıcı penceresinde bir GitHub yetkilendirme iletişim kutusunu açar. 

6. Yeni pencerede GitHub yönetici hesabınızı kullanarak oturum açın. Elde edilen yetkilendirme iletişim kutusunda sağlamayı etkinleştirmek istediğiniz GitHub ekibi seçin ve ardından **Authorize**. Tamamlandığında, sağlama yapılandırmasını tamamlamak için Azure portalına geri dönün.

    ![Yetkilendirme iletişim](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. Azure portalında giriş **Kiracı URL** tıklatıp **Bağlantıyı Sına** Azure emin olmak için AD GitHub uygulamanıza bağlanabilir. Bağlantı başarısız olursa, GitHub hesabınızda yönetici izinleri olduğundan emin olun ve **Kiracı URl** doğru girilen ve "Yetkilendir" adımı yeniden deneyin (size oluşturabilecek **Kiracı URL** kuralı tarafından: "https://api.github.com/scim/v2/organizations/ + < Organizations_name > ", kuruluşlar, GitHub hesabınızda altında bulabilirsiniz: **ayarları** > **kuruluşların**).

    ![Yetkilendirme iletişim](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alanına ve "bir hata oluştuğunda e-posta bildirimi gönder." onay kutusunu işaretleyin

9. **Kaydet**’e tıklayın. 

10. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları GitHub için**.

11. İçinde **öznitelik eşlemelerini** bölümünde, GitHub için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için github kullanıcı hesapları eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

12. GitHub için hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

13. **Kaydet**’e tıklayın. 

Bu işlem, herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde GitHub atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler anlatılmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)
