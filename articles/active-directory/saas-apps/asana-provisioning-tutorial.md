---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Asana yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Asana kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: a763b2516f88e8c92efc321db50dc15881f54c9b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59265653"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Asana yapılandırın

Bu öğreticinin amacı Asana ve Azure Active Directory (Azure AD) otomatik olarak sağlama ve sağlamasını Asana Azure AD'den kullanıcı hesaplarına gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* Bir Asana kiracısı ile bir [Kurumsal](https://www.asana.com/pricing) planlayabilir ya da daha iyi etkin
* Asana'da yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanır [Asana API](https://asana.com/developers/api-reference/users), olduğu için Asana kullanılabilir.

## <a name="assign-users-to-asana"></a>Kullanıcı için Asana atama

Azure AD kullanan adlı bir kavram *atamaları* hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'de bir uygulamaya atanan kullanıcılar eşitlenir.

Yapılandırıp sağlama hizmetini etkinleştirmeden önce hangi kullanıcıların Azure AD'de Asana uygulamanıza erişmeniz karar vermeniz gerekir. Ardından Buradaki yönergeleri izleyerek bu kullanıcılar için Asana uygulamanızı atayabilirsiniz:

[Bir kuruluş uygulamaya kullanıcı atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Kullanıcılar için Asana atamak için önemli ipuçları

Tek bir atamanızı öneririz Asana sağlama yapılandırmayı test etmek için Azure AD kullanıcısı. Ek kullanıcılar daha sonra atanabilir.

## <a name="configure-user-provisioning-to-asana"></a>İçin Asana kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD sağlama API'si Asana kullanıcı hesabına bağlama size yol gösterir. Sağlama hizmeti oluşturmak, güncelleştirmek ve Azure AD'de kullanıcı atamaları temel alınarak asana atanan kullanıcı hesaplarını devre dışı bırakmak için de yapılandırmanız.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Asana etkinleştirmek için bölümlerinde sağlanan yönergeleri izleyin. [Azure portalında](https://portal.azure.com). Bu iki özellik birbirini tamamlar ancak otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Otomatik kullanıcı hesabı için Asana Azure AD'de sağlama yapılandırmak için

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tümuygulamalar** bölümü.

1. Çoklu oturum açma için Asana zaten yapılandırılmışsa, Asana Örneğiniz için arama alanını kullanarak arayın. Aksi takdirde seçin **Ekle** araması **Asana** uygulama galerisinde. Seçin **Asana** Arama sonuçlarından ve uygulama listenize ekleyin.

1. Asana örneğinizi seçin ve ardından **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**.

    ![Asana Provisioning](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünde, girin ve belirteci oluşturmak için bu yönergeleri izleyin **gizli belirteç**:

    a. Oturum [Asana](https://app.asana.com) , yönetici hesabı kullanarak.

    b. Profil fotoğrafı üst çubuğundan seçin ve geçerli kuruluş adı ayarlarınızı seçin.

    c. Git **hizmet hesapları** sekmesi.

    d. Seçin **hizmet hesabı ekleme**.

    e. Güncelleştirme **adı** ve **hakkında** ve gerektiğinde profil fotoğrafı. Belirteci Kopyala **belirteci**ve onu seçip **Değişiklikleri Kaydet**.

1. Azure portalında **Test Bağlantısı** için Asana uygulamanızı Azure AD'ye bağlanabildiğinden emin olun. Bağlantı başarısız olursa Asana hesabınızın yönetici izinlerine sahip olduğundan emin olun ve deneyin **Test Bağlantısı** adım yeniden uygulayın.

1. Bir kişi veya sağlama hata bildirimleri almak istediğiniz gruba e-posta adresini girin **bildirim e-posta**. Altındaki onay kutusunu seçin.

1. **Kaydet**’i seçin.

1. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Asana**.

1. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den için Asana eşitlenmek üzere kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için Asana kullanıcı hesaplarını eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için. Daha fazla bilgi için [kullanıcı sağlama öznitelik eşlemelerini özelleştirme](../manage-apps/customize-application-attributes.md).

1. Azure AD hizmeti için Asana, sağlamayı etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

1. **Kaydet**’i seçin.

Asana'da atanmış tüm kullanıcılar için ilk eşitleme başlar. Şimdi **kullanıcılar** bölümü. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanım **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve etkinlik günlüklerini sağlama için bağlantıları izleyin. Denetim günlüklerini Asana uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporu](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı, kurumsal uygulamalar için hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](asana-tutorial.md)
