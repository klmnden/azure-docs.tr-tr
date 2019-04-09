---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için MerchLogix yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını MerchLogix kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: 9df4c7c5-9a58-478e-93b7-2f77aae12807
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8fecc5232b26c98c4027174454cf29b81b0ee41
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263613"
---
# <a name="tutorial-configure-merchlogix-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için MerchLogix yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara MerchLogix sağlamasını MerchLogix ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* A MerchLogix tenant
* SCIM uç nokta URL'nizi ve kullanıcı sağlama için gereken gizli belirteç sağlayabilen MerchLogix teknik bir ilgili kişi

## <a name="adding-merchlogix-from-the-gallery"></a>Galeriden MerchLogix ekleme

Otomatik kullanıcı hazırlama ile Azure AD için MerchLogix yapılandırmadan önce MerchLogix Azure AD uygulama Galerisi yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden MerchLogix eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, üzerinde sol gezinti bölmesinde, tıklayarak **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölümü][2]

3. MerchLogix eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **MerchLogix**.

5. Sonuçlar panelinde seçin **MerchLogix**ve ardından **Ekle** düğmesini MerchLogix SaaS uygulamaları listenize ekleyin.

    ![MerchLogix Provisioning][4]

## <a name="assigning-users-to-merchlogix"></a>MerchLogix için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir. 

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları MerchLogix erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek MerchLogix için bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-merchlogix"></a>Kullanıcılar için MerchLogix atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı ilk otomatik kullanıcı sağlama yapılandırmasını test etmek için MerchLogix atanır. Testler başarılı olduktan sonra ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için MerchLogix atarken, (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-merchlogix"></a>MerchLogix için otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya MerchLogix gruplarında Azure AD'de kullanıcı ve/veya grup atamaları temel alarak.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için MerchLogix etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma MerchLogix tek öğretici](merchlogix-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-merchlogix-in-azure-ad"></a>Azure AD'de MerchLogix için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. MerchLogix SaaS uygulamaları listesinden seçin.

3. Seçin **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![MerchLogix Provisioning](./media/merchlogix-provisioning-tutorial/Merchlogix1.png)

5. Altında **yönetici kimlik bilgileri** bölümü:

    * İçinde **Kiracı URL'si** , MerchLogix teknik konular ilgili kişisi tarafından sağlanan SCIM uç nokta URL'sini girin.

    * İçinde **gizli belirteç** , MerchLogix teknik konular ilgili kişisi tarafından sağlanan gizli belirteç girin.

6. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD için MerchLogix bağlanabilirsiniz. Bağlantı başarısız olursa MerchLogix hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

7. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları MerchLogix**.

10. İçinde MerchLogix için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri MerchLogix kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

11. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına MerchLogix**.

12. İçinde MerchLogix için Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri MerchLogix gruplarında güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

13. Azure AD sağlama hizmeti için MerchLogix etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

14. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti MerchLogix üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: common/select-azuread.png
[2]: common/enterprise-applications.png
[3]: common/add-new-app.png
[4]: common/search-new-app.png
