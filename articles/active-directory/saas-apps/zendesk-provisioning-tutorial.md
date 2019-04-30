---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Zendesk yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını kaldırma Zendesk kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: f559d2c2398998ba590419758de559f21d9b65f5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114674"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Öğretici: Zendesk otomatik kullanıcı hazırlama için yapılandırma

Bu öğreticide, Zendesk ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için otomatik olarak sağlamak ve kullanıcıların ve grupların Zendesk sağlamasını kaldırmak için gerçekleştirme adımları gösterilmektedir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular hakkında daha fazla bilgi için bkz: [kullanıcı sağlamayı ve Azure Active Directory ile hizmet olarak yazılım-a-(SaaS) uygulamalarına sağlama kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, olduğunu varsayar:

* Azure AD kiracısı.
* Bir Zendesk kiracıyla [Kurumsal](https://www.zendesk.com/product/pricing/) planlayabilir ya da daha iyi etkin.
* Zendesk yönetici izinlerine sahip bir kullanıcı hesabı.

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Zendesk Rest API](https://developer.zendesk.com/rest_api/docs/core/introduction). Bu API, Zendesk takımlar Kurumsal plan veya üzeri için kullanılabilir.

## <a name="add-zendesk-from-the-azure-marketplace"></a>Azure Market'ten Zendesk Ekle

Otomatik kullanıcı hazırlama ile Azure AD için Zendesk yapılandırmadan önce Zendesk Azure Market'te yönetilen SaaS uygulamalarının listenize ekleyin.

Marketten Zendesk eklemek için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com), sol taraftaki gezinti bölmesinde seçin **Azure Active Directory**.

    ![Azure Active Directory simgesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zendesk** seçip **Zendesk** sonucu panelinden. Uygulama eklemek için seçin **Ekle**.

    ![Sonuç listesinde Zendesk](common/search-new-app.png)

## <a name="assign-users-to-zendesk"></a>Zendesk için kullanıcı atama

Azure Active Directory kullanan adlı bir kavram *atamaları* hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcılar veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı sağlamayı etkinleştirmek için önce hangi kullanıcıların veya grupların Azure AD'de Zendesk erişmesi karar verin. Bu kullanıcılar veya gruplar için Zendesk atamak için yönergeleri izleyin. [kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md).

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Zendesk için kullanıcı atama önemli ipuçları

* Bugün, Zendesk rolleri Azure portalı kullanıcı arabirimini otomatik olarak ve dinamik olarak doldurulur. Zendesk roller kullanıcılara atamadan önce bir ilk eşitleme, Zendesk kiracınızda bulunan en son rollerini almak üzere Zendesk karşı tamamlandığını emin olun.

* Tek bir atamanızı öneririz zendesk'e ilk otomatik kullanıcı sağlama yapılandırmasını test etmek için Azure AD kullanıcısı. Testler başarılı olduktan sonra ek kullanıcılar veya gruplar daha sonra atayabilirsiniz.

* Zendesk'te bir kullanıcıya atadığınızda, geçerli herhangi bir uygulamaya özgü rol ataması iletişim kutusunda kullanılabilir olması durumunda seçin. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configure-automatic-user-provisioning-to-zendesk"></a>Zendesk için otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde Azure AD sağlama hizmeti yapılandırma adımlarında size kılavuzluk eder. Oluşturma, güncelleştirme ve kullanıcılar veya gruplar Azure AD'de kullanıcı veya grup atamaları temel alınarak Zendesk, devre dışı bırakmak için kullanın.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Zendesk etkinleştirebilirsiniz. Bölümündeki yönergeleri [oturum açma Zendesk tek öğretici](zendesk-tutorial.md). Bu iki özellik birbirini tamamlar ancak otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Azure AD'de Zendesk için otomatik kullanıcı sağlamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **kurumsal uygulamalar** > **tüm uygulamaları** > **Zendesk**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zendesk**.

    ![Uygulamalar listesini Zendesk bağlantıdaki](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zendesk sağlama](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zendesk sağlama modu](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, yönetici kullanıcı adı, gizli belirteç ve etki alanı Zendesk hesabınızın giriş. Bu değerleri örnekleri şunlardır:

   * İçinde **yönetici kullanıcı adı** kutusunda, kiracınıza Zendesk yönetici hesabı kullanıcı adı girin. admin@contoso.com bunun bir örneğidir.

   * İçinde **gizli belirteç** kutusunda, adım 6'da açıklandığı gibi gizli belirteç doldurun.

   * İçinde **etki alanı** kutusunda, Zendesk kiracınızın alt etki alanı doldurun. Örneğin, bir kiracı URL'si olan bir hesap için `https://my-tenant.zendesk.com`, kendi alt etki alanı olan **Kiracı my**.

6. Zendesk hesabınızın gizli belirteç bulunan **yönetici** > **API** > **ayarları**. Emin olun **belirteç erişimi** ayarlanır **etkin**.

    ![Zendesk yönetici ayarları](./media/zendesk-provisioning-tutorial/ZenDesk4.png)

    ![Zendesk gizli belirteç](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Adım 5'te gösterilen kutuları doldurduktan sonra seçin **Test Bağlantısı** Azure'un emin olmak için AD için Zendesk bağlanabilirsiniz. Bağlantı başarısız olursa, Zendesk hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zendesk Bağlantıyı Sına](./media/zendesk-provisioning-tutorial/ZenDesk19.png)

8. İçinde **bildirim e-posta** kutusunda kişinin e-posta adresi girin veya sağlama hata bildirimleri almak için Grup. Seçin **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusu.

    ![Zendesk bildirim e-postası](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. **Kaydet**’i seçin.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları zendesk'e**.

    ![Zendesk kullanıcı eşitleme](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Zendesk'te için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zendesk kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için seçmeniz **Kaydet**.

    ![Zendesk eşleşen kullanıcı öznitelikleri](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Zendesk**.

    ![Zendesk Grup eşitleme](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Zendesk'te için Azure AD'den eşitlenen grup öznitelikleri gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için Zendesk gruplarında eşleştirmek için kullanılır. Değişiklikleri kaydetmek için seçmeniz **Kaydet**.

    ![Zendesk eşleşen grup öznitelikleri](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Kapsam belirleme filtrelerini yapılandırmak için yönergeleri izleyin. [kapsam belirleme filtresi öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Azure AD içinde sağlama hizmeti için Zendesk, etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

    ![Zendesk sağlama durumu](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Zendesk sağlamak için kullanıcıları veya istediğiniz grupları tanımlayın. İçinde **ayarları** bölümünde, istediğiniz değerleri seçin **kapsam**.

    ![Zendesk kapsamı](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Sağlama için hazır olduğunuzda **Kaydet**.

    ![Zendesk Kaydet](./media/zendesk-provisioning-tutorial/ZenDesk18.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme daha sonraki eşitlemeler gerçekleştirmek için daha uzun sürer. Azure AD sağlama hizmeti çalıştırdığı sürece, yaklaşık 40 dakikada oluşur. 

Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama etkinliği raporunu için bağlantıları izleyin. Rapor hizmette Zendesk sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz: [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Zendesk ile kullanıcılar için gruplarının kullanımı destekleyen **aracı** yalnızca rolleri. Daha fazla bilgi için [Zendesk belgeleri](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

* Özel bir rol, bir kullanıcı veya gruba atandığında de sağlama hizmeti Azure AD otomatik kullanıcı varsayılan rolü atar **aracı**. Yalnızca aracılar, özel bir rol atanabilir. Daha fazla bilgi için [Zendesk API belgeleri](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı, kurumsal uygulamalar için hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
