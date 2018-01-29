---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Salesforce korumalı alan yapılandırma | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Salesforce korumalı alan arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 9ff50ddc2460a94c17b2401f0c8e4ad12c6d23a7
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-salesforce-sandbox-for-automatic-user-provisioning"></a>Öğretici: Salesforce korumalı alan otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı Salesforce korumalı alan ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den Salesforce korumalı alan sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Salesforce korumalı alan için iş veya eğitim için Salesforce korumalı alan için geçerli bir kiracı. Ücretsiz bir deneme hesabı ya da hizmet için kullanabilir.
*   Salesforce korumalı alan takım yönetici izinlerine sahip bir kullanıcı hesabının.

## <a name="assigning-users-to-salesforce-sandbox"></a>Salesforce korumalı alan kullanıcılar atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcıların veya grupların Azure AD'de Salesforce korumalı alan uygulamanızı erişmeniz karar vermeniz gerekir. Bu karara yaptıktan sonra bu kullanıcılar Salesforce korumalı alan uygulamanıza'ndaki yönergeleri izleyerek atayabilirsiniz [bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Salesforce korumalı alan kullanıcılara atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Salesforce korumalı alan atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için Salesforce korumalı alan atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

> [!NOTE]
> Bu uygulama özel roller Salesforce korumalı alan müşteri kullanıcılar atarken seçmek isteyebilirsiniz sağlama işleminin bir parçası olarak alır.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD Salesforce korumalı alanın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirmek ve Azure AD'de kullanıcı ve grup atama Salesforce korumalı alan hesaplarında dayalı atanan kullanıcı devre dışı bırakın.

>[!Tip]
>Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Salesforce korumalı alan, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı Yapılandır

Bu bölümün amacı Salesforce korumalı alan Active Directory kullanıcı hesaplarının kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Salesforce korumalı alan zaten yapılandırdıysanız arama alanı kullanarak Salesforce korumalı alan Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Salesforce korumalı alan** uygulama galerisinde. Arama sonuçlarından Salesforce korumalı alan seçin ve uygulamaları listenize ekleyin.

3. Salesforce korumalı alan örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![sağlama](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlar:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna, Salesforce korumalı alan adı hesap türü **Sistem Yöneticisi** atanan Salesforce.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.

6. Salesforce korumalı alan güvenlik belirtecini almak için aynı Salesforce korumalı alan yönetici dikkate yeni sekmede ve oturum açın. Sayfanın sağ üst köşesinde adınıza tıklayın ve ardından **ayarları**.

     ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "otomatik kullanıcı sağlamayı etkinleştirin")

7. Sol gezinti bölmesinde tıklatın **kişisel bilgilerimi** ilgili bölümü genişletin ve ardından **sıfırlama My güvenlik belirteci**.
  
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "otomatik kullanıcı sağlamayı etkinleştirin")

8. Üzerinde **güvenlik belirteci sıfırlama** sayfasında, **güvenlik belirteci sıfırlama** düğmesi.

    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "otomatik kullanıcı sağlamayı etkinleştirin")

9. Bu Yönetici hesabınızla ilişkili e-posta gelen kutusunu kontrol edin. Salesforce Sandbox.com yeni güvenlik belirteci içeren bir e-posta için bakın.

10. Belirteç kopyalama, Azure AD penceresine gidin ve yapıştırın **gizli belirteci** alan.

11. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Salesforce korumalı alan uygulamanıza bağlanabilir.

12. İçinde **bildirim e-posta** alan, bir kişi veya grubun sağlama hata bildirimleri almak ve gerekir onay e-posta adresini girin.

13. Tıklatın **kaydedin.**  
    
14.  Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Salesforce korumalı alan için.**

15. İçinde **öznitelik eşlemelerini** bölümünde, Salesforce korumalı alan için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Salesforce korumalı alan kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

16. Salesforce korumalı alan için hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

17. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya Salesforce korumalı alan kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Salesforce korumalı alan uygulama sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Şimdi sınama hesabı oluşturabilirsiniz. Hesap salesforce eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-sandbox-tutorial)
