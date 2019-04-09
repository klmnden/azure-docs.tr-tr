---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için temel taşıdır OnDemand yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını dönüm OnDemand kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d17a3c81784d56c6fcad7c7608559abf732882a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274102"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için temel taşıdır OnDemand yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya temel taşıdır OnDemand gruplarına sağlamasını için dönüm OnDemand ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* Temel taşıdır OnDemand Kiracı
* Temel taşıdır OnDemand yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [dönüm OnDemand Webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), temel taşıdır OnDemand takımlar için kullanılabildiği.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Galeriden dönüm OnDemand ekleme

Azure AD ile otomatik kullanıcı hazırlama için temel taşıdır OnDemand yapılandırmadan önce temel taşıdır OnDemand Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden dönüm OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **dönüm OnDemand**seçin **dönüm OnDemand** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde dönüm OnDemand](common/search-new-app.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Temel taşıdır OnDemand için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları dönüm OnDemand erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek dönüm OnDemand için bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Kullanıcılar için dönüm OnDemand atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için dönüm OnDemand atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için dönüm OnDemand atarken, (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Temel taşıdır OnDemand için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya Azure AD'de kullanıcı ve/veya grup atamalarını tabanlı dönüm OnDemand gruplarında devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Azure AD'de dönüm OnDemand için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamalar**, ardından **dönüm OnDemand**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **dönüm OnDemand**.

    ![Uygulamalar listesinde dönüm OnDemand bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **yönetici kullanıcı adı**, **yönetici parolası**, ve **etki alanı** dönüm OnDemand's, hesabı.

    * İçinde **yönetici kullanıcı adı** alan, yönetici hesabının dönüm OnDemand kiracınıza etki alanı\kullanıcı adı doldurun. Örnek: contoso\admin.

    * İçinde **yönetici parolası** alan, yönetici kullanıcı adı için karşılık gelen parola doldurun.

    * İçinde **etki alanı** alanında, temel taşıdır OnDemand Kiracı Web hizmeti URL'sini doldurma. Örnek: Hizmet şu konumdadır `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, Contoso etki alanı için `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Web hizmeti URL'si alma hakkında daha fazla bilgi için bkz. [burada](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD için dönüm OnDemand bağlanabilirsiniz. Bağlantı başarısız olursa, temel taşıdır OnDemand hesabının yönetici izinlerine sahip olun ve yeniden deneyin.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için dönüm OnDemand**.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. İçinde dönüm OnDemand için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri dönüm OnDemand kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Azure AD sağlama hizmeti için temel taşıdır OnDemand etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Kullanıcılara ve/veya istediğiniz grupları dönüm OnDemand sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![OnDemand dönüm sağlama](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Azure AD temel taşıdır OnDemand hizmette sağlama tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Temel taşıdır OnDemand **konumu** özniteliği dönüm OnDemand portalında rollerine karşılık gelen bir değer bekliyor. Geçerli listesini **konumu** değerler elde edilebilir giderek **kullanıcı kaydını Düzenle > kuruluş yapısı > Konum** dönüm OnDemand portalında.

    ![Kullanıcıyı Düzenle dönüm OnDemand sağlama](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![konumu sağlama dönüm OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![dönüm OnDemand konumları listesi sağlama](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
