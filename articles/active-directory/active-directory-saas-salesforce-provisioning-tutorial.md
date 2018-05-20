---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Salesforce yapılandırma | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Salesforce arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 3997d913525b44f154ca1e989ee1880308b82096
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Öğretici: Salesforce otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak için Salesforce ve Azure AD içinde gerçekleştirmek için gereken adımlar ve devre dışı bırakma sağlama kullanıcı Azure AD'den Salesforce hesapları göstermektir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   Bir Salesforce.com Kiracı

>[!IMPORTANT] 
>Ardından bir Salesforce.com deneme hesabı kullanıyorsanız, otomatik kullanıcı sağlamayı Yapılandır mümkün olmayacaktır. Deneme hesapları satın alınan kadar etkin gerekli API erişimi yoktur. Bu sınırlamaya geçici bir ücretsiz kullanarak alabileceğiniz [Geliştirici hesabını](https://developer.salesforce.com/signup) Bu öğreticiyi tamamlamak için.

Salesforce korumalı ortam kullanıyorsanız, lütfen bkz [Salesforce korumalı alan tümleştirme Öğreticisi](https://go.microsoft.com/fwLink/?LinkID=521879).

## <a name="assigning-users-to-salesforce"></a>Salesforce kullanıcılar atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcıların veya grupların Azure AD'de Salesforce uygulamanızı erişmeniz karar vermeniz gerekir. Bu karara yaptıktan sonra bu kullanıcılar Salesforce uygulamanıza'ndaki yönergeleri izleyerek atayabilirsiniz [bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Salesforce kullanıcılara atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Salesforce atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*  Bir kullanıcı için Salesforce atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmıyor

    > [!NOTE]
    > Bu uygulamayı Salesforce müşteri kullanıcılar atarken seçmek isteyebilirsiniz sağlama işleminin bir parçası olarak özel roller alır

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD Salesforce'nın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre Salesforce atanan kullanıcı hesaplarında devre dışı bırakın.

>[!Tip]
>Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Salesforce, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı Yapılandır

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Salesforce kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Salesforce zaten yapılandırdıysanız arama alanı kullanarak Salesforce Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Salesforce** uygulama galerisinde. Arama sonuçlarından Salesforce seçin ve uygulamaları listenize ekleyin.

3. Salesforce örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![sağlama](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlar:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna, bir Salesforce hesap adı türü **Sistem Yöneticisi** atanan Salesforce.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.

6. Salesforce güvenlik belirtecini almak için aynı Salesforce yönetici dikkate yeni sekmede ve oturum açın. Sayfanın sağ üst köşesinde adınıza tıklayın ve ardından **ayarları**.

     ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "otomatik kullanıcı sağlamayı etkinleştirin")

7. Sol gezinti bölmesinde tıklatın **kişisel bilgilerimi** ilgili bölümü genişletin ve ardından **sıfırlama My güvenlik belirteci**.
  
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "otomatik kullanıcı sağlamayı etkinleştirin")

8. Üzerinde **güvenlik belirteci sıfırlama** sayfasında, **güvenlik belirteci sıfırlama** düğmesi.

    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "otomatik kullanıcı sağlamayı etkinleştirin")

9. Bu Yönetici hesabınızla ilişkili e-posta gelen kutusunu kontrol edin. Yeni güvenlik belirteci içeriyor Salesforce.com bir e-posta arayın.

10. Belirteç kopyalama, Azure AD penceresine gidin ve yapıştırın **gizli belirteci** alan.

11. **Kiracı URL** Salesforce örneği üzerinde Salesforce Bulutu ise girilmesi gerekir. Aksi takdirde isteğe bağlıdır. Kiracı URL biçimi kullanarak girin https://your-instance.my.salesforce.com, örnek bilgisayarınızı Salesforce örneğinizi adıyla değiştirerek.

12. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Salesforce uygulamanıza bağlanabilir.

13. İçinde **bildirim e-posta** alan, bir kişi veya grubun sağlama hata bildirimleri almak ve gerekir aşağıdaki onay e-posta adresini girin.

14. Tıklatın **kaydedin.**  
    
15.  Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Salesforce için.**

16. İçinde **öznitelik eşlemelerini** bölümünde, Salesforce Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri Salesforce kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

17. Salesforce hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

18. Tıklatın **kaydedin.**

Bu, herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde Salesforce atanan ilk eşitleme başlatır. İlk eşitlemeyi gerçekleştirmek için yaklaşık her 40 dakika çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer unutmayın. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Salesforce uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
