---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Pingboard yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Pingboard kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: asmalser
ms.reviewer: asmalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2ab7f58c3061044583baf9db73e193966d7d4eb
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59058389"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Pingboard yapılandırın

Bu öğreticide otomatik hazırlama ve kullanıcı hesaplarını Azure Active Directory'den (Azure AD) için Pingboard sağlamayı etkinleştirmek için izlemeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* Pingboard Kiracı [Pro hesabı](https://pingboard.com/pricing)
* Pingboard yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanır [Pingboard API](https://pingboard.docs.apiary.io/#), hesabınızda bulunan.

## <a name="assign-users-to-pingboard"></a>Pingboard için kullanıcı atama

Azure AD "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'de bir uygulamaya atanan kullanıcılar eşitlenir. 

Yapılandırıp sağlama hizmetini etkinleştirmeden önce hangi kullanıcıların Azure AD'de Pingboard uygulamanıza erişmeniz karar vermeniz gerekir. Ardından Buradaki yönergeleri izleyerek bu kullanıcılar Pingboard uygulamanıza atayabilirsiniz:

[Bir kuruluş uygulamaya kullanıcı atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Kullanıcılar için Pingboard atamak için önemli ipuçları

Tek bir atamanızı öneririz Pingboard sağlama yapılandırmayı test etmek için Azure AD kullanıcısı. Ek kullanıcılar daha sonra atanabilir.

## <a name="configure-user-provisioning-to-pingboard"></a>Pingboard için kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama API'si Pingboard kullanıcı hesabına bağlama size yol gösterir. Sağlama hizmeti oluşturmak, güncelleştirmek ve Azure AD'de kullanıcı atamaları temel alan Pingboard atanan kullanıcı hesaplarını devre dışı bırakmak için de yapılandırmanız.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Pingboard etkinleştirmek için bölümlerinde sağlanan yönergeleri izleyin. [Azure portalında](https://portal.azure.com). Bu iki özellik birbirini tamamlar ancak otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Otomatik kullanıcı hesabı için Pingboard Azure AD'de sağlama yapılandırmak için

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tümuygulamalar** bölümü.

1. Çoklu oturum açma için Pingboard zaten yapılandırılmışsa, Pingboard Örneğiniz için arama alanını kullanarak arayın. Aksi takdirde seçin **Ekle** araması **Pingboard** uygulama galerisinde. Seçin **Pingboard** Arama sonuçlarından ve uygulama listenize ekleyin.

1. Pingboard örneğinizi seçin ve ardından **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**.

    ![Pingboard sağlama](./media/pingboard-provisioning-tutorial/pingboardazureprovisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki adımları kullanın:

    a. İçinde **Kiracı URL'si**, girin `https://your_domain.pingboard.com/scim/v2`ve "şöyledir: your_domain" gerçek etki alanınız ile değiştirin.

    b. Oturum [Pingboard](https://pingboard.com/) , yönetici hesabı kullanarak.

    c. Seçin **eklentileri** > **tümleştirmeler** > **Azure Active Directory**.

    d. Git **yapılandırma** sekmesine tıklayın ve **Azure'dan kullanıcı sağlamayı etkinleştirin**.

    e. Belirteci Kopyala **OAuth taşıyıcı belirteci**girin **gizli belirteç**.

1. Azure portalında **Bağlantıyı Sına** Pingboard uygulamanızı Azure AD'ye test etmek için bağlanabilirsiniz. Bağlantı başarısız olursa Pingboard hesabınız yönetici izinleri bulunan test ve deneyin **Bağlantıyı Sına** adım yeniden uygulayın.

1. Bir kişi veya sağlama hata bildirimleri almak istediğiniz gruba e-posta adresini girin **bildirim e-posta**. Altındaki onay kutusunu seçin.

1. **Kaydet**’i seçin.

1. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Pingboard**.

1. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Pingboard için eşitlenmesi gereken kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Pingboard kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için. Daha fazla bilgi için [sağlama öznitelik eşlemelerini özelleştirme kullanıcı](../manage-apps/customize-application-attributes.md).

1. Azure AD içinde sağlama hizmeti için Pingboard, etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

1. Seçin **Kaydet** Pingboard için atanan kullanıcılar, ilk eşitleme başlatılamadı.

İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan aşağıdaki eşitlemeler çalışması daha uzun sürer. Kullanım **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve etkinlik günlüklerini sağlama için bağlantıları izleyin. Günlükleri Pingboard uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporu](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı, kurumsal uygulamalar için hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](pingboard-tutorial.md)
