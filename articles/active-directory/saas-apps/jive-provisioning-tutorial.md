---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Jive yapılandırma | Microsoft Docs'
description: Azure Active Directory ve Jive arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 607d538a2a2636e17265e95195000a777f162dc4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60263364"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Öğretici: Jive otomatik kullanıcı hazırlama için yapılandırma

Bu öğreticinin amacı Jive ve Azure AD içinde Jive için Azure AD'den otomatik olarak sağlama ve devre dışı bırakma sağlama kullanıcı hesaplarına gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Bir Jive çoklu oturum açma etkin aboneliği.
*   Jive takım Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-jive"></a>Jive için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Jive uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar Jive uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Jive için kullanıcı atama önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Jive atanabilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Jive atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si Jive kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirme ve Azure AD'de kullanıcı ve Grup atamasına dayalı Jive atanan kullanıcı hesaplarını devre dışı bırakın.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma Jive için etkin olarak, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Jive Active Directory kullanıcı hesaplarının kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.
Bu yordam bir parçası olarak, Jive.com istemeniz gerekir kullanıcı güvenlik belirteci gereklidir.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

1. Çoklu oturum açma için Jive zaten yapılandırdıysanız arama alanını kullanarak Jive Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Jive** uygulama galerisinde. Arama sonuçlarından Jive seçin ve uygulama listenize ekleyin.

1. Jive örneğinizi seçin ve ardından **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**. 

    ![Sağlama](./media/jive-provisioning-tutorial/provisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlayın:
   
    a. İçinde **Jive yönetici kullanıcı adı** metin kutusuna bir Jive hesap adı **Sistem Yöneticisi** atanan Jive.com profilinde.
   
    b. İçinde **Jive yönetici parolası** metin kutusuna bu hesabın parolasını yazın.
   
    c. İçinde **Jive Kiracı URL'si** metin Jive Kiracı URL'sini yazın.
      
      > [!NOTE]
      > Jive Kiracı URL'si için Jive oturum açmak için kuruluşunuz tarafından kullanılan URL'dir.  
      > Genellikle, URL'si şu biçimdedir: **www.\< Kuruluş\>. jive.com**.          

1. Azure portalında **Test Bağlantısı** Azure emin olmak için AD Jive uygulamanıza bağlanabilirsiniz.

1. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

1. Tıklayın **kaydedin.**

1. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Jive.**

1. İçinde **öznitelik eşlemelerini** bölümünde, gözden Jive için Azure AD'den eşitlenen kullanıcı öznitelikleri. Seçilen öznitelikler **eşleşen** özellikleri Jive kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için Jive etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

1. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya Jive kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Jive uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](jive-tutorial.md)
