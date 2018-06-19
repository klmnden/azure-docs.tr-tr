---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Replicon yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına Replicon sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
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
ms.date: 02/20/2018
ms.author: v-ant
ms.openlocfilehash: eb28e80cb01ca3134236efa8b40698fabffbdbb7
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982183"
---
# <a name="tutorial-configure-replicon-for-automatic-user-provisioning"></a>Öğretici: Replicon otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya Replicon gruplarına sağlanmasını Replicon ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure AD kiracısı
*   Replicon Kiracı ile [artı](https://www.replicon.com/time-bill-pricing/) planlayabilir ya da daha iyi etkin
*   Replicon yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Tümleştirme sağlama Azure AD dayanan [Replicon API](https://www.replicon.com/help/developers), Replicon takıma kullanılabilen Plus üzerinde planlayabilir ya da daha iyi.

## <a name="adding-replicon-from-the-gallery"></a>Galeriden Replicon ekleme
Azure AD ile otomatik kullanıcı sağlamayı Replicon yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize Replicon eklemeniz gerekir.

**Azure AD uygulama Galeriden Replicon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. Replicon eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Replicon**.

    ![Replicon uygulama arama](./media/replicon-provisioning-tutorial/RepliconAppSearch.png)

5. Sonuçlar panelinde seçin **Replicon**ve ardından **Ekle** Replicon SaaS uygulamaları listenize eklemek için düğmeyi.

    ![Replicon arama sonuçları](./media/replicon-provisioning-tutorial/RepliconAppSearchResults.png)

    ![Replicon uygulama oluşturma](./media/replicon-provisioning-tutorial/RepliconAppCreate.png)


## <a name="assigning-users-to-replicon"></a>Kullanıcılar için Replicon atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Replicon erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Replicon için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-replicon"></a>Kullanıcılar için Replicon atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için Replicon atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Replicon atarken, (varsa) geçerli bir uygulamaya özgü rol atama iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-replicon"></a>Otomatik kullanıcı sağlamayı Replicon için yapılandırma 

Bu bölümde, oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için hizmet sağlama Azure AD'yi yapılandırma adımlarında size rehberlik eder ve/veya Azure AD'de kullanıcı ve/veya grup atamaları Replicon gruplarında tabanlı.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için Replicon etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Replicon tek Öğreticisi](replicon-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-replicon-in-azure-ad"></a>Azure AD'de Replicon için otomatik kullanıcı sağlamayı yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Replicon SaaS uygulamaları listesinden seçin.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/Replicon2.png)

3. Seçin **sağlama** sekmesi.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconProvisioningTab.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/Replicon1.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı**, **yönetici parolası**, **Companyıd**, ve **etki alanı**  Replicon'ın hesap. Bu değerleri örnekleri şunlardır:

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak Replicon Kiracı yönetici hesabı kullanıcı adı. Örnek: contosoadmin.

    *   İçinde **yönetici parolası** alanında, yönetici kullanıcı adı için karşılık gelen parola doldurur.

    *   İçinde **Companyıd** alan, Replicon kiracınızın Companyıd doldurur. Örnek: Contoso oturum açma göre Companyıd verilebilir.

    ![Replicon oturum açma](./media/replicon-provisioning-tutorial/RepliconLogin.png)

    *   İçinde **etki alanı** alanında, adım 6'te açıklandığı gibi etki alanını doldurun.
    
6. Elde **serviceEndpointRootURL** , Replicon için Kiracı belirtilen adımları bağlı hesabını [Replicon yardımcı olan hizmet](https://www.replicon.com/help/determining-the-url-for-your-service-calls). URL alma sırasında **etki alanı** alt etki alanı adını olacaktır **serviceEndpointRootURL** vurgulanan. 

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconEndpoint.png)

7. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD için Replicon bağlanabilir. Bağlantı başarısız olursa Replicon hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconTestConnection.png)

8. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconNotificationEmail.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Replicon**.
    
    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconUserMapping.png)

11. Azure AD içinde Replicon eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Replicon kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconUserMappingAtrributes.png)

12. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](../active-directory-saas-scoping-filters.md).

13. Azure AD hizmeti Replicon için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/RepliconProvisioningStatus.png)

14. Kullanıcılara ve/veya istediğiniz grupları Replicon sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/UserGroupSelection.png)

15. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![Replicon sağlama](./media/replicon-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Replicon hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/replicon-tutorial/tutorial_general_01.png
[2]: ./media/replicon-tutorial/tutorial_general_02.png
[3]: ./media/replicon-tutorial/tutorial_general_03.png
