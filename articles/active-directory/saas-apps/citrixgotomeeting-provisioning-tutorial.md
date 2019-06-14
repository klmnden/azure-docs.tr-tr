---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için GoToMeeting yapılandırma | Microsoft Docs'
description: Azure Active Directory ve GoToMeeting arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e3145d0faaa3aecb90b582b3b6ef0063572ff43
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60430809"
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için GoToMeeting yapılandırın

Bu öğreticinin amacı GoToMeeting ve Azure AD sağlama ve sağlamasını GoToMeeting Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Bir GoToMeeting çoklu oturum açma abonelik etkin.
*   GoToMeeting takım Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-gotomeeting"></a>Kullanıcı için bir GoToMeeting atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları GoToMeeting uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar GoToMeeting uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Kullanıcılar için GoToMeeting atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için GoToMeeting atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için bir GoToMeeting atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si GoToMeeting'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirme ve Azure AD'de kullanıcı ve Grup atamasına dayalı GoToMeeting atanan kullanıcı hesaplarını devre dışı bırakın.

> [!TIP]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için GoToMeeting, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

1. Çoklu oturum açma için GoToMeeting zaten yapılandırdıysanız arama alanını kullanarak GoToMeeting Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **GoToMeeting** uygulama galerisinde. Arama sonuçlarından GoToMeeting seçin ve uygulama listenize ekleyin.

1. Örneğiniz GoToMeeting seçip **sağlama** sekmesi.

1. Ayarlama **sağlama** moduna **otomatik**. 

    ![Sağlama](./media/citrixgotomeeting-provisioning-tutorial/provisioning.png)

1. Yönetici kimlik bilgileri bölümü altında aşağıdaki adımları gerçekleştirin:
   
    a. İçinde **GoToMeeting yönetici kullanıcı adı** metin kutusu, bir yönetici kullanıcı adını yazın.

    b. İçinde **GoToMeeting yönetici parolası** yöneticinin parolasını metin.

1. Azure portalında **Test Bağlantısı** Azure emin olmak için AD GoToMeeting uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa GoToMeeting hesabınız takım Yöneticisi izinlerine sahip olduğundan emin olun ve deneyin **"Yönetici kimlik bilgileri"** adım yeniden uygulayın.

1. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

1. Tıklayın **kaydedin.**

1. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için GoToMeeting.**

1. İçinde **öznitelik eşlemelerini** bölümünde, GoToMeeting için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri GoToMeeting kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için GoToMeeting etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

1. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya GoToMeeting kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve GoToMeeting uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrix-gotomeeting-tutorial)


