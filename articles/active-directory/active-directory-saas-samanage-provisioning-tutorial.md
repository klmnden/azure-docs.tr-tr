---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Samanage yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına Samanage sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
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
ms.author: v-ant
ms.openlocfilehash: 8f11beff2c78386f4c3e8130c8e5d72465b8fe87
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Öğretici: Samanage otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya Samanage gruplarına sağlanmasını Samanage ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide verilen senaryoda aşağıdaki sahip olduğunuz varsayılmaktadır:

*   Azure Active Directory kiracısı
*   Samanage Kiracı ile [Professional](https://www.samanage.com/pricing/) planlayabilir ya da daha iyi etkin 
*   Samanage yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE]
> Azure AD tümleştirme sağlama dayanan [Samanage REST API](https://www.samanage.com/api/), Samanage takımlara profesyonel plan üzerinde kullanılabilir veya daha iyi.

## <a name="adding-samanage-from-the-gallery"></a>Galeriden Samanage ekleme
Otomatik kullanıcı Azure AD ile hazırlama için Samanage yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize Samanage eklemeniz gerekir.

**Azure AD uygulama Galeriden Samanage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. Samanage eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Samanage**.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage4.png)

5. Sonuçlar panelinde seçin **Samanage**ve ardından **Ekle** Samanage SaaS uygulamaları listenize eklemek için düğmeyi.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage6.png)

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage5.png)

## <a name="assigning-users-to-samanage"></a>Kullanıcılar için Samanage atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir. 

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Samanage erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Samanage için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Kullanıcılar için Samanage atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için Samanage atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Samanage atarken, (varsa) geçerli bir uygulamaya özgü rol atama iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Otomatik kullanıcı sağlamayı Samanage için yapılandırma

Bu bölümde, oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için hizmet sağlama Azure AD'yi yapılandırma adımlarında size rehberlik eder ve/veya Azure AD'de kullanıcı ve/veya grup atamaları Samanage gruplarında tabanlı.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için Samanage etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Samanage tek Öğreticisi](active-directory-saas-samanage-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir. Daha fazla bilgi için bkz: [oturum açma Samanage tek Öğreticisi](active-directory-saas-samanage-tutorial.md).

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Azure AD'de Samanage için otomatik kullanıcı sağlamayı yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Samanage SaaS uygulamaları listesinden seçin.
 
    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage7.png)

3. Seçin **sağlama** sekmesi.
    
    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage8.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage9.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı**, ve **yönetici parolası** Samanages'ın hesap. Bu değerleri örnekleri şunlardır:

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak Samanage Kiracı yönetici hesabı kullanıcı adı. Örnek: admin@contoso.com.

    *   İçinde **yönetici parolası** alanında, önceden belirtilen yönetici kullanıcı adı için karşılık gelen parola doldurur.

6. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD için Samanage bağlanabilir. Bağlantı başarısız olursa Samanage hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage10.png)

7. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage11.png)

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Samanage**.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage12.png)

10. Azure AD içinde Samanage eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Samanage kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage13.png)

12. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory gruplarına Samanage**.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage14.png)

13. Azure AD içinde Samanage eşitlenir grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Samanage gruplara güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage15.png)

14. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](./active-directory-saas-scoping-filters.md).

15. Azure AD hizmeti Samanage için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage16.png)

16. Kullanıcılara ve/veya istediğiniz grupları Samanage sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage17.png)

17. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![Samanage sağlama](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage18.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Samanage hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
