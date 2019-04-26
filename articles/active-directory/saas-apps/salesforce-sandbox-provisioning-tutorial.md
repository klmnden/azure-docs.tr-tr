---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Salesforce korumalı alan'ı yapılandırma | Microsoft Docs"
description: Azure Active Directory ve Salesforce korumalı alan arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e0a4eed020728bea5de196eebe438947ae509e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515647"
---
# <a name="tutorial-configure-salesforce-sandbox-for-automatic-user-provisioning"></a>Öğretici: Salesforce korumalı alan için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı, Salesforce korumalı alan ve Azure AD'deki otomatik olarak sağlama ve sağlamasını Salesforce korumalı alan için Azure AD'den kullanıcı hesapları için gerçekleştirmeniz gereken adımlarda sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Salesforce korumalı alan için iş veya eğitim için Salesforce korumalı alan için geçerli bir kiracı. Ücretsiz bir deneme hesabı ya da hizmeti için kullanabilirsiniz.
*   Salesforce korumalı alan takım Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-salesforce-sandbox"></a>Salesforce korumalı alan için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcıların veya grupların Azure AD'de Salesforce korumalı alan uygulamanıza erişmeniz karar vermeniz gerekir. Bu kararı yaptıktan sonra bu kullanıcıların Salesforce korumalı alan uygulamanıza yönergelerini takip ederek atayabilirsiniz [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Salesforce korumalı alan için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Salesforce korumalı alan atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Salesforce korumalı alan için kullanıcı atama, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

> [!NOTE]
> Bu uygulama, Salesforce korumalı müşteri kullanıcılar atarken seçmek isteyebilirsiniz sağlama işleminin bir parçası olarak özel roller içeri aktarır.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si Salesforce korumalı alanın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve kullanıcı ve grup hesapları Salesforce korumalı alan içinde dayanan atanan kullanıcı devre dışı bırak Azure AD'de atama.

>[!Tip]
>Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için Salesforce korumalı alan, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı yapılandırma

Bu bölümün amacı, Salesforce korumalı alan Active Directory kullanıcı hesaplarının kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

1. Çoklu oturum açma için Salesforce korumalı alan zaten yapılandırdıysanız, Salesforce arama alanını kullanarak korumalı alan Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Salesforce korumalı alan** uygulama galerisinde. Salesforce korumalı alan arama sonuçlarından seçin ve uygulama listenize ekleyin.

1. Salesforce korumalı alan örneğinizi seçin ve ardından **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**.

    ![sağlama](./media/salesforce-sandbox-provisioning-tutorial/provisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlayın:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna bir Salesforce korumalı alan olan adı hesap **Sistem Yöneticisi** atanan Salesforce.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna bu hesabın parolasını yazın.

1. Salesforce korumalı alan güvenlik belirtecini almak için aynı Salesforce korumalı alan yönetici hesabı yeni bir sekme ve oturum açın. Sayfanın sağ üst köşesindeki adınıza tıklayın ve ardından **ayarları**.

     ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "otomatik kullanıcı sağlamayı etkinleştirin")

1. Sol gezinti bölmesinde **kişisel Bilgilerim** ilgili bölümü genişletin ve ardından **sıfırlama My güvenlik belirteci**.
  
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "otomatik kullanıcı sağlamayı etkinleştirin")

1. Üzerinde **güvenlik belirteci sıfırlama** sayfasında **güvenlik belirteci sıfırlama** düğmesi.

    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "otomatik kullanıcı sağlamayı etkinleştirin")

1. Bu yönetici hesabı ile ilişkili e-posta gelen kutusunu kontrol edin. Salesforce Sandbox.com yeni bir güvenlik belirteci içeren bir e-posta için bakın.

1. Belirteci kopyalayın, Azure AD pencerenize gidin ve yapıştırın **gizli belirteç** alan.

1. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, Salesforce korumalı alan uygulamanıza bağlanabilirsiniz.

1. İçinde **bildirim e-posta** alanında, bir kişi veya grubun sağlama hata bildirimleri almak ve onay e-posta adresi girin.

1. Tıklayın **kaydedin.**  
    
1.  Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için Salesforce korumalı alan.**

1. İçinde **öznitelik eşlemelerini** bölümünde, Salesforce korumalı alan için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Salesforce korumalı alan kullanıcı hesapları için güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için Salesforce korumalı alan sağlamak için değiştirme **sağlama durumu** için **üzerinde** Ayarlar bölümünde

1. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya Salesforce korumalı alan kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Salesforce korumalı alan uygulamasında sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-sandbox-tutorial)
