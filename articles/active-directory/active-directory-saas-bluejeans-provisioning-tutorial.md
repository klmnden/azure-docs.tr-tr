---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı BlueJeans yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına BlueJeans sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: v-ant
ms.openlocfilehash: 53d063573165a13fe35c4f149784bbfe1d498e01
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35292013"
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Öğretici: BlueJeans otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya BlueJeans gruplarına sağlanmasını BlueJeans ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide verilen senaryoda aşağıdaki sahip olduğunuz varsayılmaktadır:

*   Azure AD kiracısı
*   BlueJeans Kiracı ile [Şirketim](https://www.BlueJeans.com/pricing) planlayabilir ya da daha iyi etkin
*   BlueJeans yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Tümleştirme sağlama Azure AD dayanan [BlueJeans API](https://BlueJeans.github.io/developer), standart plan üzerinde BlueJeans ekipleri için kullanılabilir ya da daha iyi olduğu.

## <a name="adding-bluejeans-from-the-gallery"></a>Galeriden BlueJeans ekleme
Azure AD ile otomatik kullanıcı sağlamayı BlueJeans yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize BlueJeans eklemeniz gerekir.

**Azure AD uygulama Galeriden BlueJeans eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. BlueJeans eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **BlueJeans**.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansAppSearch.png)

5. Sonuçlar panelinde seçin **BlueJeans**ve ardından **Ekle** BlueJeans SaaS uygulamaları listenize eklemek için düğmeyi.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansAppSearchResults.png)

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansAppCreate.png)
    
## <a name="assigning-users-to-bluejeans"></a>Kullanıcılar için BlueJeans atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları BlueJeans erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek BlueJeans için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>Kullanıcılar için BlueJeans atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için BlueJeans atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için BlueJeans atarken, (varsa) geçerli bir uygulamaya özgü rol atama iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>Otomatik kullanıcı sağlamayı BlueJeans için yapılandırma

Bu bölümde, oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için hizmet sağlama Azure AD'yi yapılandırma adımlarında size rehberlik eder ve/veya Azure AD'de kullanıcı ve/veya grup atamaları BlueJeans gruplarında tabanlı.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için BlueJeans etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma BlueJeans tek Öğreticisi](active-directory-saas-bluejeans-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Azure AD'de BlueJeans için otomatik kullanıcı sağlamayı yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. BlueJeans SaaS uygulamaları listesinden seçin.
 
    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/Bluejeans2.png)

3. Seçin **sağlama** sekmesi.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/Bluejeans1.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı**, ve **yönetici parolası** BlueJeans hesabınızın. Bu değerleri örnekleri şunlardır:

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak BlueJeans Kiracı yönetici hesabı kullanıcı adı. Örnek: admin@contoso.com.

    *   İçinde **yönetici parolası** alanında, yönetici kullanıcı adı için karşılık gelen parola doldurur.

6. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD için BlueJeans bağlanabilir. Bağlantı başarısız olursa BlueJeans hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları BlueJeans**.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. Azure AD içinde BlueJeans eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri BlueJeans kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](./active-directory-saas-scoping-filters.md).

12. Azure AD hizmeti BlueJeans için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. Kullanıcılara ve/veya istediğiniz grupları BlueJeans sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![BlueJeans sağlama](./media/active-directory-saas-bluejeans-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve BlueJeans hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](./active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Bluejeans 30 karakterden uzun kullanıcı adları izin verilmez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
