---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Zendesk yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Zendesk kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: 01d5e4d5-d856-42c4-a504-96fa554baf66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf747fb75ea663d2c64038d73f48adb19d9fb804
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59278590"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Öğretici: Zendesk otomatik kullanıcı hazırlama için yapılandırma

Bu öğreticinin amacı, Zendesk ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zendesk sağlamasını için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* Bir Zendesk kiracıyla [Kurumsal](https://www.zendesk.com/product/pricing/) planlayabilir ya da daha iyi etkin
* Zendesk yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Zendesk Rest API](https://developer.zendesk.com/rest_api/docs/core/introduction), daha iyi veya Kurumsal plan Zendesk takımlar için kullanılabilir.

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk galeri ekleme

Otomatik kullanıcı hazırlama ile Azure AD için Zendesk yapılandırmadan önce Zendesk Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zendesk**seçin **Zendesk** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Zendesk](common/search-new-app.png)

## <a name="assigning-users-to-zendesk"></a>Zendesk için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Zendesk erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Zendesk için bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Zendesk için kullanıcı atama önemli ipuçları

* Zendesk rolleri otomatik olarak ve dinamik olarak Azure portalı kullanıcı arabirimini bugün doldurulur. Zendesk roller kullanıcılara atamadan önce bir ilk eşitleme Zendesk kiracınızda bulunan en son rollerini almak üzere Zendesk karşı tamamlandığından emin olun.

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını ilk otomatik kullanıcı test etmek için Zendesk atanır. Testler başarılı olduktan sonra ek kullanıcılar ve/veya grupları daha sonra atanabilir.
  
* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Zendesk atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Zendesk için kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-zendesk"></a>Zendesk için otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Azure AD'de kullanıcı ve/veya grup atamaları bu zendesk'teki gruplandırır.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Zendesk etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Zendesk tek öğretici](zendesk-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Azure AD'de Zendesk için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zendesk**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zendesk**.

    ![Uygulamalar listesini Zendesk bağlantıdaki](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **yönetici kullanıcı adı**, **gizli belirteç**, ve **etki alanı** , Zendesk'in hesabının. Bu değerleri örnekleri şunlardır:

   * İçinde **yönetici kullanıcı adı** alan, Zendesk kiracınıza yönetici hesabının kullanıcı adını doldurun. Örnek: admin@contoso.com.

   * İçinde **gizli belirteç** alanında, adım 6'da açıklandığı gibi gizli belirteç doldurun.

   * İçinde **etki alanı** alan, Zendesk kiracınızın alt etki alanı doldurun.
     Örnek: Bir kiracı URL'si sahip bir hesap `https://my-tenant.zendesk.com`, kendi alt etki alanı olacaktır **Kiracı my**.

6. **Gizli belirteç** hesabının bulunduğu için Zendesk **yönetici > API > ayarları**.
   Emin **belirteç erişimi** ayarlanır **etkin**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk4.png)

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD için Zendesk bağlanabilirsiniz. Bağlantı başarısız olursa, Zendesk hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk19.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları zendesk'e**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Zendesk'te için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zendesk kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına ZenDesk**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Zendesk'te için Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için Zendesk gruplarında eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Azure AD sağlama hizmeti için Zendesk etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Kullanıcılara ve/veya istediğiniz grupları Zendesk sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk18.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Azure AD sağlama hizmeti Zendesk şirket tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Zendesk gruplarının kullanımı, yalnızca aracı rolleri ile kullanıcılar için destekler. Daha fazla bilgi için bkz [Zendesk'ın belgeleri](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

* Bir kullanıcı ve/veya grup için özel bir rol atandığında, sağlama hizmetini Azure AD'ye otomatik kullanıcı aynı zamanda varsayılan rol atama **aracı**. Yalnızca **aracıları** özel bir rol atanabilir. Daha fazla bilgi için bu başvuru [Zendesk API belgeleri](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests).  

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
