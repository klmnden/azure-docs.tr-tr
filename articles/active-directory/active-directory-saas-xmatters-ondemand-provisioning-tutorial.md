---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı için OnDemand xMatters yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına xMatters OnDemand sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
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
ms.date: 04/19/2018
ms.author: v-ant-msft
ms.openlocfilehash: e0b945a99766ee52cb357f54d7135fbbdf1fada2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-xmatters-ondemand-for-automatic-user-provisioning"></a>Öğretici: otomatik kullanıcı sağlamayı için OnDemand xMatters yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya xMatters OnDemand gruplarına sağlanmasını için Azure AD yapılandırmak için xMatters OnDemand ve Azure Active Directory (Azure AD) gerçekleştirilmesi için adımları göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide verilen senaryoda aşağıdaki sahip olduğunuz varsayılmaktadır:

*   Azure AD kiracısı
*   Bir xMatters OnDemand kiracıyla [Starter](https://www.xmatters.com/pricing) planlayabilir ya da daha iyi etkin 
*   XMatters OnDemand yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Tümleştirme sağlama Azure AD kullanır [xMatters OnDemand API](https://help.xmatters.com/xmAPI), daha iyi veya xMatters OnDemand takımlara Starter plan üzerinde kullanılabilir.

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Galeriden xMatters OnDemand ekleme

XMatters OnDemand otomatik kullanıcı Azure AD ile hazırlama için yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize xMatters OnDemand eklemeniz gerekir.

**Azure AD uygulama Galeriden xMatters OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. XMatters OnDemand eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **xMatters OnDemand**.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearch.png)

5. Sonuçlar panelinde seçin **xMatters OnDemand**ve ardından **Ekle** xMatters OnDemand SaaS uygulamaları listenize eklemek için düğmeyi.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-xmatters-ondemand"></a>Kullanıcıları xMatters OnDemand atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları xMatters OnDemand erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek xMatters OnDemand bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-xmatters-ondemand"></a>Kullanıcıların xMatters OnDemand atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için xMatters OnDemand atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı xMatters OnDemand atarken atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rol seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-xmatters-ondemand"></a>XMatters OnDemand otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya kullanıcı OnDemand tabanlı xMatters gruplarında ve/veya Azure AD Grup atamaları devre dışı bırakmak için hizmet sağlama Azure AD yapılandırma adımlarını size kılavuzluk eder.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için xMatters OnDemand etkinleştirmek, yönergeleri izleyerek sağlanan [xMatters OnDemand çoklu oturum açma Öğreticisi](active-directory-saas-xmatters-ondemand-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-xmatters-ondemand-in-azure-ad"></a>Azure AD'de OnDemand xMatters için otomatik kullanıcı sağlamayı yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. XMatters OnDemand SaaS uygulamaları listesinden seçin.
 
    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ApplicationInstanceSearch.png)

3. Seçin **sağlama** sekmesi.
    
    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı**, **yönetici parolası**, ve **etki alanı** , xMatters OnDemand's, hesabı.

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak xMatters OnDemand Kiracı yönetici hesabı kullanıcı adı. Örnek: admin@contoso.com.

    *   İçinde **yönetici parolası** alanında, yönetici kullanıcı adı için karşılık gelen parola doldurur.

    *   İçinde **etki alanı** alan, xMatters OnDemand kiracınızın alt etki alanı doldurmak.
    Örnek: Kiracı URL'si ile bir hesap için https://my-tenant.xmatters.com, alt etki alanı olacaktır **Kiracı my**.

6. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD xMatters OnDemand bağlanabilir. Bağlantı başarısız olursa olun, xMatters OnDemand hesabı yönetici izinleri olan ve yeniden deneyin.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/TestConnection.png)
    
7. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/EmailNotification.png)

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları xMatters OnDemand**.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncUsers.png)

10. XMatters OnDemand Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin, **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri xMatters OnDemand güncelleştirme işlemleri için kullanıcı hesapları eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/UserAttributes.png)

11. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory gruplarına xMatters OnDemand**.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncGroups.png)

12. XMatters OnDemand Azure AD'den eşitlenen grup öznitelikleri gözden geçirin, **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri xMatters OnDemand güncelleştirme işlemleri için gruplarında eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/GroupAttributes.png)

13. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](./active-directory-saas-scoping-filters.md).

14. Azure AD hizmeti xMatters OnDemand için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningStatus.png)

15. XMatters OnDemand sağlamak için kullanıcıların ve/veya istediğiniz grupları tanımlayın istenen değerleri seçerek **kapsam** içinde **ayarları** bölümü.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ScopeSelection.png)

16. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![xMatters OnDemand sağlama](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve xMatters OnDemand hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_03.png
