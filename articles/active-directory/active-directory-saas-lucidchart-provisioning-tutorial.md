---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı LucidChart yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına LucidChart sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 1f9344a5e750360e21ed7dc8e3ed013c2c2e1a45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a>Öğretici: LucidChart otomatik kullanıcı sağlamayı için yapılandırma


Bu öğreticinin amacı LucidChart ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den LucidChart sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   LucidChart Kiracı ile [Kurumsal planı](https://www.lucidchart.com/user/117598685#/subscriptionLevel) veya daha iyi etkin 
*   LucidChart yönetici izinlerine sahip bir kullanıcı hesabı 

## <a name="assigning-users-to-lucidchart"></a>Kullanıcılar için LucidChart atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları LucidChart uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar LucidChart uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a>Kullanıcılar için LucidChart atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için LucidChart atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için LucidChart atarken ya da seçmelisiniz **kullanıcı** rol ya da başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. **Varsayılan erişim** rol sağlamak için çalışmaz ve bu kullanıcılar atlandı.


## <a name="configuring-user-provisioning-to-lucidchart"></a>Kullanıcı için LucidChart sağlama yapılandırma 

Bu bölümde Azure AD LucidChart'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre LucidChart atanan kullanıcı hesaplarında devre dışı bırakın.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için LucidChart, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için LucidChart Azure AD'de sağlamayı Yapılandır


1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için LucidChart zaten yapılandırdıysanız arama alanı kullanarak LucidChart Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **LucidChart** uygulama galerisinde. Arama sonuçlarından LucidChart seçin ve uygulamaları listenize ekleyin.

3. LucidChart örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![LucidChart sağlama](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **gizli belirteci** LucidChart'ın hesap tarafından oluşturulan (belirteç hesabınızın altında bulabilirsiniz: **takım**  >  **Uygulama tümleştirmesi** > **SCIM'yi**). 

    ![LucidChart sağlama](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD LucidChart uygulamanıza bağlanabilir. Bağlantı başarısız olursa LucidChart hesabınız yönetici izinlerine sahip olduğundan emin olun ve 5. adım yeniden deneyin.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alanına ve "bir hata oluştuğunda e-posta bildirimi gönder." onay kutusunu işaretleyin

8. **Kaydet** düğmesine tıklayın. 

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları LucidChart**.

10. İçinde **öznitelik eşlemelerini** bölümünde, LucidChart için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri LucidChart kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD hizmeti LucidChart için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. **Kaydet** düğmesine tıklayın. 

Bu işlem, herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde LucidChart atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler anlatılmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)
