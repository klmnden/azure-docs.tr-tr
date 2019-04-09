---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Samanage yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Samanage kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca43b62e66e3a736aa52fdd10fe36e635daba245
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59058025"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Samanage yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Samanage sağlamasını Samanage ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

* Azure AD kiracısı
* A [Samanage Kiracı](https://www.samanage.com/pricing/) profesyonel paket
* Samanage yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Samanage Rest API](https://www.samanage.com/api/), profesyonel paket sahip hesaplar için Samanage geliştiricilerin kullanımına olduğu.

## <a name="adding-samanage-from-the-gallery"></a>Galeriden Samanage ekleme

Otomatik kullanıcı hazırlama ile Azure AD için Samanage yapılandırmadan önce Samanage Azure AD uygulama Galerisi yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Samanage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Samanage**seçin **Samanage** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Samanage](common/search-new-app.png)

## <a name="assigning-users-to-samanage"></a>Samanage için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Samanage erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Samanage için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Kullanıcılar için Samanage atamak için önemli ipuçları

*    Samanage rolleri otomatik olarak ve dinamik olarak Azure portalı kullanıcı arabirimini bugün doldurulur. Kullanıcılara Samanage rol atama önce bir ilk eşitleme Samanage kiracınızda bulunan en son rollerini almak üzere Samanage karşı tamamlandığından emin olun.

*    Önerilir tek bir Azure AD kullanıcı ilk otomatik kullanıcı sağlama yapılandırmasını test etmek için Samanage atanır. Testler başarılı olduktan sonra ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Samanage atarken, (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Samanage için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Samanage gruplarında Azure AD'de kullanıcı ve/veya grup atamaları temel alarak.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Samanage etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Samanage tek öğretici](samanage-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Azure AD'de Samanage için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Samanage**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Samanage**.

    ![Uygulamalar listesinde Samanage bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **yönetici kullanıcı adı** ve **yönetici parolası** Samanage hesabınızın. Bu değerleri örnekleri şunlardır:

   * İçinde **yönetici kullanıcı adı** alan, Samanage kiracınıza yönetici hesabının kullanıcı adını doldurun. Örnek: admin@contoso.com.

   * İçinde **yönetici parolası** alan, yönetici kullanıcı adı için karşılık gelen yönetici hesabının parolasını doldurun.

6. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD için Samanage bağlanabilirsiniz. Bağlantı başarısız olursa Samanage hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/TestConnection.png)

7. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Samanage**.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/UserMappings.png)

10. İçinde Samanage için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Samanage kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Grup eşlemelerini altında etkinleştirmek için **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Samanage**.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Ayarlama **etkin** için **Evet** grupları eşitlenecek. İçinde Samanage için Azure AD'den eşitlenen grup öznitelikleri gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Samanage kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Azure AD sağlama hizmeti için Samanage etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. Kullanıcılara ve/veya istediğiniz grupları Samanage sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü. Seçerken **tüm kullanıcıları ve grupları eşitleme** seçeneğinde, açıklandığı sınırlamaları göz önünde bulundurun **bağlayıcı sınırlamaları** bölümüne bakın.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Samanage üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Varsa **tüm kullanıcıları ve grupları eşitleme** seçeneği belirlendiğinde ve Samanage için yapılandırılmış bir varsayılan değer **rolleri** özniteliği, istenen değerin altında olduğundan emin olun **null ise varsayılan değer (değer İsteğe bağlı)** alan şu biçimde ifade **{"displayName": "rolü"}** istediğiniz varsayılan değere olduğu rol.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
