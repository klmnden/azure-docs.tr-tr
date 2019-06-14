---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için muhasebesi OneWorld yapılandırma | Microsoft Docs'
description: Azure Active Directory ve muhasebesi OneWorld arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 928070ae7e5c9077c6f77e8cb7beb36815f47d6a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515834"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için muhasebesi yapılandırma

Bu öğreticinin amacı muhasebesi OneWorld ve Azure AD sağlama ve sağlamasını muhasebesi Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Muhasebesi OneWorld aboneliği. Otomatik kullanıcı hazırlama şu anda olduğuna dikkat edin yalnızca muhasebesi OneWorld ile desteklenir.
*   Muhasebesi yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-netsuite-oneworld"></a>Muhasebesi OneWorld için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları muhasebesi uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar muhasebesi uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite-oneworld"></a>Kullanıcılar için muhasebesi OneWorld atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için muhasebesi atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için muhasebesi atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si muhasebesi'nın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı muhasebesi, devre dışı.

> [!TIP] 
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için muhasebesi, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, muhasebesi Active Directory kullanıcı hesaplarının kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

1. Çoklu oturum açma için muhasebesi zaten yapılandırdıysanız arama alanını kullanarak muhasebesi Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **muhasebesi** uygulama galerisinde. Arama sonuçlarından muhasebesi seçin ve uygulama listenize ekleyin.

1. Örneğiniz muhasebesi seçip **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**. 

    ![Sağlama](./media/netsuite-provisioning-tutorial/provisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlayın:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna bir muhasebesi hesap adı **Sistem Yöneticisi** atanan Netsuite.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna bu hesabın parolasını yazın.
      
1. Azure portalında **Test Bağlantısı** Azure emin olmak için AD muhasebesi uygulamanıza bağlanabilirsiniz.

1. İçinde **bildirim e-posta** alanında, bir kişi veya grubun sağlama hata bildirimleri almak ve onay e-posta adresi girin.

1. Tıklayın **kaydedin.**

1. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için muhasebesi.**

1. İçinde **öznitelik eşlemelerini** bölümünde, muhasebesi için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri muhasebesi kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için muhasebesi etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

1. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya muhasebesi kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmetini çalıştıran sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve muhasebesi uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](netsuite-tutorial.md)
