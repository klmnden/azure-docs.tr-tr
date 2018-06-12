---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Clarizen yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına Clarizen sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant-msft
ms.openlocfilehash: 5ee0d5248e3e53db8b7475dca1d51658ade25f81
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293271"
---
# <a name="tutorial-configure-clarizen-for-automatic-user-provisioning"></a>Öğretici: Clarizen otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya Clarizen gruplarına sağlanmasını Clarizen ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide verilen senaryoda aşağıdaki sahip olduğunuz varsayılmaktadır:

*   Azure AD kiracısı
*   Clarizen Kiracı ile [Enterprise Edition](https://www.clarizen.com/product/pricing/) planlayabilir ya da daha iyi etkin 
*   Clarizen yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Tümleştirme sağlama Azure AD dayanan [Clarizen API](https://api.clarizen.com/v2.0/services/), daha iyi veya Enterprise Edition plan üzerinde Clarizen ekipleri için kullanılabilir.

## <a name="adding-clarizen-from-the-gallery"></a>Galeriden Clarizen ekleme
Otomatik kullanıcı Azure AD ile hazırlama için Clarizen yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize Clarizen eklemeniz gerekir.

**Azure AD uygulama Galeriden Clarizen eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. Clarizen eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Clarizen**.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearch.png)

5. Sonuçlar panelinde seçin **Clarizen**ve ardından **Ekle** Clarizen SaaS uygulamaları listenize eklemek için düğmeyi.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearchResults.png)

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-clarizen"></a>Kullanıcılar için Clarizen atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir. 

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Clarizen erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Clarizen için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-clarizen"></a>Kullanıcılar için Clarizen atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için Clarizen atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Clarizen atarken, (varsa) geçerli bir uygulamaya özgü rol atama iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-clarizen"></a>Otomatik kullanıcı sağlamayı Clarizen için yapılandırma 

Bu bölümde, oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için hizmet sağlama Azure AD'yi yapılandırma adımlarında size rehberlik eder ve/veya Azure AD'de kullanıcı ve/veya grup atamaları Clarizen gruplarında tabanlı.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için Clarizen etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Clarizen tek Öğreticisi](active-directory-saas-clarizen-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-clarizen-in-azure-ad"></a>Azure AD'de Clarizen için otomatik kullanıcı sağlamayı yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Clarizen SaaS uygulamaları listesinden seçin.
 
    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/AppInstanceSearch.png)

3. Seçin **sağlama** sekmesi.
    
    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **etki alanı**, **yönetici kullanıcı adı**, ve **yönetici parolası** Clarizen'ın hesap. Bu değerleri örnekleri şunlardır:

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak Clarizen Kiracı yönetici hesabı kullanıcı adı. Örnek: admin@contoso.com.

    *   İçinde **yönetici parolası** alanında, yönetici kullanıcı adı için karşılık gelen yönetici hesabının parolasını doldurur.

    *   İçinde **etki alanı** alanında, adım 6'da temel bir alt etki alanı doldurur.
    
6. Alma **serverLocation** Clarizen hesabınızın altında belirtilen adımları temel **kimlik doğrulaması** , [Clarizen'ın Rest API Kılavuzu](https://success.clarizen.com/hc/en-us/articles/205711828-REST-API-Guide-Version-2). ServerLocation alma sırasında doldurmak için aşağıdaki alt vurgulanan bir URL'nin kullanma **etki alanı** alan.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/ClarizenDomain.png)

7. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD için Clarizen bağlanabilir. Bağlantı başarısız olursa Clarizen hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/TestConnection.png)
    
8. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/NotificationEmail.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Clarizen**.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMapping.png)

11. Azure AD içinde Clarizen eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Clarizen kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMappingAttributes.png)

12. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory gruplarına Clarizen**.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMapping.png)

13. Azure AD içinde Clarizen eşitlenir grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Clarizen gruplara güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMappingAttributes.png)

14. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](./active-directory-saas-scoping-filters.md).

15. Azure AD hizmeti Clarizen için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningStatus.png)

16. Kullanıcılara ve/veya istediğiniz grupları Clarizen sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/ScopeSelection.png)

17. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![Clarizen sağlama](./media/active-directory-saas-clarizen-provisioning-tutorial/SaveProvisioning.png)


Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Clarizen hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-clarizen-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
