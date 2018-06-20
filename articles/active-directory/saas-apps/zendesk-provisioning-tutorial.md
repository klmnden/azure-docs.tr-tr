---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Zendesk yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına Zendesk sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 6ba2fd9ee81b8551cc2a267cdc9767f47fe27456
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229062"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Öğretici: Zendesk otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya Zendesk gruplarına sağlanmasını Zendesk ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir. 

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

*   Azure AD kiracısı
*   Zendesk Kiracı ile [Kurumsal](https://www.zendesk.com/product/pricing/) planlayabilir ya da daha iyi etkin 
*   Zendesk yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE]
> Tümleştirme sağlama Azure AD dayanan [Zendesk Rest API](https://developer.zendesk.com/rest_api/docs/core/introduction), daha iyi veya Kurumsal plan üzerindeki Zendesk ekipleri için kullanılabilir.

## <a name="adding-zendesk-from-the-gallery"></a>Galeriden Zendesk ekleme
Otomatik kullanıcı Azure AD ile hazırlama için Zendesk yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize Zendesk eklemeniz gerekir.

**Azure AD uygulama Galeriden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. Zendesk eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Zendesk**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk6.png)

5. Sonuçlar panelinde seçin **Zendesk**ve ardından **Ekle** Zendesk SaaS uygulamaları listenize eklemek için düğmeyi.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk7.png)

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk20.png)

## <a name="assigning-users-to-zendesk"></a>Kullanıcılar için Zendesk atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir. 

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Zendesk erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Zendesk için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Kullanıcılar için Zendesk atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için Zendesk atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Zendesk atarken, (varsa) geçerli bir uygulamaya özgü rol atama iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-zendesk"></a>Zendesk için otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde, oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için hizmet sağlama Azure AD'yi yapılandırma adımlarında size rehberlik eder ve/veya Azure AD'de kullanıcı ve/veya grup atamaları Zendesk gruplarında tabanlı.

> [!TIP]
> Ayrıca aktarmızı SAML tabanlı çoklu oturum açma için Zendesk etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Zendesk tek Öğreticisi](zendesk-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Azure AD'de Zendesk için otomatik kullanıcı sağlamayı yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Zendesk SaaS uygulamaları listesinden seçin.
 
    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk3.png)

3. Seçin **sağlama** sekmesi.
    
    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı**, **gizli belirteci**, ve **etki alanı** Zendesk'ın hesap. Bu değerleri örnekleri şunlardır:

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak Zendesk Kiracı yönetici hesabı kullanıcı adı. Örnek: admin@contoso.com.

    *   İçinde **gizli belirteci** alanında, adım 6'te açıklandığı gibi gizli belirteci doldurun.

    *   İçinde **etki alanı** alan, Zendesk kiracınızın alt etki alanı doldurmak.
    Örnek: Kiracı URL'si ile bir hesap için https://my-tenant.zendesk.com, alt etki alanı olacaktır **Kiracı my**.

6. **Gizli belirteci** hesabının bulunduğu için Zendesk **yönetici > API > ayarlarını**. 

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk4.png) ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD için Zendesk bağlanabilir. Bağlantı başarısız olursa, Zendesk hesabınıza yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk19.png)
    
8. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Zendesk**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Azure AD içinde Zendesk eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zendesk kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory gruplarına ZenDesk**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Azure AD içinde Zendesk eşitlenir grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zendesk gruplara güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](./../active-directory-saas-scoping-filters.md).

15. Zendesk hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Kullanıcılara ve/veya istediğiniz grupları Zendesk sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk18.png)


Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Zendesk hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları
* Zendesk gruplarının kullanımı için kullanıcıları yalnızca Aracısı rolleriyle destekler. Daha fazla bilgi için lütfen [Zendesk'ın belgelerine](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
