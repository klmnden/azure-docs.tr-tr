---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı ThousandEyes yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına ThousandEyes sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: 055355be0de1c3f589bde468da3558963e8e1383
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Öğretici: ThousandEyes otomatik kullanıcı sağlamayı yapılandırın


Bu öğreticinin amacı ThousandEyes ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den ThousandEyes sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   ThousandEyes Kiracı ile [standart planı](https://www.thousandeyes.com/pricing) veya daha iyi etkin 
*   ThousandEyes yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE]
> Tümleştirme sağlama Azure AD dayanan [ThousandEyes SCIM'yi API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), standart plan üzerinde ThousandEyes ekipleri için kullanılabilir ya da daha iyi olduğu.

## <a name="assigning-users-to-thousandeyes"></a>Kullanıcılar için ThousandEyes atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları ThousandEyes uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar ThousandEyes uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Kullanıcılar için ThousandEyes atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için ThousandEyes atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için ThousandEyes atarken ya da seçmelisiniz **kullanıcı** rol ya da başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. **Varsayılan erişim** rol sağlamak için çalışmaz ve bu kullanıcılar atlandı.


## <a name="configuring-user-provisioning-to-thousandeyes"></a>Kullanıcı için ThousandEyes sağlama yapılandırma 

Bu bölümde Azure AD ThousandEyes'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre ThousandEyes atanan kullanıcı hesaplarında devre dışı bırak .

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için ThousandEyes, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için ThousandEyes Azure AD'de sağlamayı Yapılandır


1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için ThousandEyes zaten yapılandırdıysanız arama alanı kullanarak ThousandEyes Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **ThousandEyes** uygulama galerisinde. Arama sonuçlarından ThousandEyes seçin ve uygulamaları listenize ekleyin.

3. ThousandEyes örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![ThousandEyes sağlama](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **gizli belirteci** ThousandEyes'ın hesap tarafından oluşturulan (belirteç ThousandEyes hesabınızın altında bulabilirsiniz: **güvenlik & Kimlik doğrulama**). 

    ![ThousandEyes sağlama](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD ThousandEyes uygulamanıza bağlanabilir. Bağlantı başarısız olursa ThousandEyes hesabınız yönetici izinlerine sahip olduğundan emin olun ve 5. adım yeniden deneyin.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alanına ve "bir hata oluştuğunda e-posta bildirimi gönder." onay kutusunu işaretleyin

8. **Kaydet**’e tıklayın. 

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları ThousandEyes**.

10. İçinde **öznitelik eşlemelerini** bölümünde, ThousandEyes için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri ThousandEyes kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD hizmeti ThousandEyes için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. **Kaydet**’e tıklayın. 

Bu işlem, herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde ThousandEyes atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler anlatılmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)
