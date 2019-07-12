---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için dizin Federasyon yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarını Directory'ye federe sağlamasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: 19f5690a6852161abce2565a8c4a52ce86ff5187
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840595"
---
# <a name="tutorial-configure-federated-directory-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Federasyon dizinini yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya dizin Federasyon gruplarına sağlamasını için Federasyon Directory ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
>  Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir dizin Federasyon](https://www.federated.directory/pricing).
* Dizin Federasyon yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="assign-users-to-federated-directory"></a>Federasyon dizinine kullanıcı atama
Azure Active Directory atamaları adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Federasyon Directory erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Federasyon dizinine bu kullanıcılara ve/veya grupları atayabilirsiniz:

 * [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md) 
 
 ## <a name="important-tips-for-assigning-users-to-federated-directory"></a>Federasyon dizinine kullanıcı atama önemli ipuçları
 * Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Federasyon dizinine atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Federasyon dizinine kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Varsayılan erişim rolüne sahip kullanıcılar, sağlamasından bırakılır.
    
 ## <a name="set-up-federated-directory-for-provisioning"></a>Dizini Federasyon sağlama için ayarlama

Azure AD ile otomatik kullanıcı hazırlama için Federasyon Directory yapılandırmadan önce Federal dizinlerde SCIM sağlamayı etkinleştirmek gerekir.

1. Oturum açın, [dizin Yönetici konsolunda federasyon](https://federated.directory/of)

    ![Federasyon Directory öğreticisini](media/federated-directory-provisioning-tutorial/companyname.png)

2. Gidin **dizinleri > Kullanıcı dizini** kiracınızı seçin. 

    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/ad-user-directories.png)

3.  Kalıcı taşıyıcı belirteci oluşturmak için gidin **dizin anahtarlar > Yeni anahtarı oluşturun.** 

    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/federated01.png)

4. Dizin anahtarı oluşturun. 

    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/federated02.png)
    

5. Kopyalama **erişim belirteci** değeri. Bu değer girilir **gizli belirteç** dizin Federasyon uygulamanızın Azure portalında sağlama sekmesindeki alan. 

    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/federated03.png)
    
## <a name="add-federated-directory-from-the-gallery"></a>Galeriden Federasyon Dizin Ekle

Otomatik kullanıcı hazırlama Azure AD ile Federasyon Directory yapılandırmak için dizin Federasyon Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Federasyon dizin eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** bölmenin üstünde düğme.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Federasyon Directory**seçin **Federasyon Directory** Sonuçlar panelinde.

    ![Sonuç listesinde Federasyon dizini](common/search-new-app.png)

5. Gidin **URL** vurgulanmış altındaki ayrı bir tarayıcı. 

    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/loginpage1.png)

6. Tıklayın **oturum**.

    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/federated04.png)

7.  Microsoft iş hesabınızı kullanarak dizin Federasyon oturum açma Federasyon Directory Openıdconnect uygulama olduğundan,'ı seçin.
    
    ![Federasyon dizini](media/federated-directory-provisioning-tutorial/loginpage3.png)
 
8. Başarılı bir kimlik doğrulamasından sonra onay sayfası için onay istemi kabul edin. Uygulama kiracınıza sonra bir otomatik olarak eklenir ve Federal Directory hesabınızla yönlendirilir.

    ![Federasyon directory SCIM Ekle](media/federated-directory-provisioning-tutorial/premission.png)



## <a name="configuring-automatic-user-provisioning-to-federated-directory"></a>Federasyon dizinine otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya Azure AD'de kullanıcı ve/veya grup atamalarını temel dizin Federasyon gruplarında devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-federated-directory-in-azure-ad"></a>Azure AD'de Federasyon dizin için otomatik kullanıcı hazırlama yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Federasyon Directory**.

    ![Uygulamalar listesinde Federasyon dizin bağlantısı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Sağlama sekmesinde](common/provisioning.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Sağlama sekmesinde](common/provisioning-automatic.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde `https://api.federated.directory/v2/` Kiracı URL. Giriş alınır ve Federal dizininden daha önce kaydettiğiniz değeri **gizli belirteç**. Tıklayın **Test Bağlantısı** olun Azure AD Federasyon dizinine bağlanabilirsiniz. Bağlantı başarısız olursa, Federasyon Directory hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL'si + simgesi](common/provisioning-testconnection-tenanturltoken.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Federasyon dizinine**.

    ![Federasyon Directory öğreticisini](media/federated-directory-provisioning-tutorial/user-mappings.png)
    
    
11. Federasyon dizinine Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Federasyon dizinindeki kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Federasyon Directory öğreticisini](media/federated-directory-provisioning-tutorial/user-attributes.png)
    

12. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Azure AD Directory Federasyon Hizmeti sağlama etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Açıkken sağlama durumu](common/provisioning-toggle-on.png)

14. Kullanıcılara ve/veya istediğiniz grupları dizine Federasyon sağlama istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Kapsam sağlama](common/provisioning-scope.png)

15. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Sağlama yapılandırmasını kaydetme](common/provisioning-configuration-save.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Azure AD Directory federe bir hizmette sağlama tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [raporlama hesabı otomatik kullanıcı hazırlama](../manage-apps/check-status-user-account-provisioning.md)
## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)