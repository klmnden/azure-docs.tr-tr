---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için DocuSign'ı yapılandırma | Microsoft Docs"
description: Azure Active Directory ve DocuSign arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 121d147a3f8c91f17e955120b2c14f7dbd3da592
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60280109"
---
# <a name="tutorial-configure-docusign-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için DocuSign'ı yapılandırma

Bu öğreticinin amacı, DocuSign ve Azure AD sağlama ve sağlamasını DocuSign Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Bir DocuSign çoklu oturum açma abonelik etkin.
*   DocuSign takım Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-docusign"></a>DocuSign için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları DocuSign uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar DocuSign uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>DocuSign için kullanıcı atama önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için DocuSign atanır. Ek kullanıcılar daha sonra atanabilir.

*   Bir kullanıcı için DocuSign atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

> [!NOTE]
> Azure AD grubu Docusign uygulamayla sağlama desteklemez, yalnızca kullanıcıların sağlanabilir.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si DocuSign'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı DocuSign, devre dışı.

> [!Tip]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için DocuSign, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, DocuSign Active Directory kullanıcı hesaplarının kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

1. Çoklu oturum açma için zaten DocuSign yapılandırdıysanız, DocuSign arama alanını kullanarak Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **DocuSign** uygulama galerisinde. Arama sonuçlarından DocuSign'ı seçin ve uygulama listenize ekleyin.

1. Örneğiniz DocuSign'ı seçin ve ardından **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**. 

    ![Sağlama](./media/docusign-provisioning-tutorial/provisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlayın:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna bir DocuSign hesap adı **Sistem Yöneticisi** atanan DocuSign.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna bu hesabın parolasını yazın.

1. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, DocuSign uygulamanıza bağlanabilirsiniz.

1. İçinde **bildirim e-posta** alanında, bir kişi veya grubun sağlama hata bildirimleri almak ve onay e-posta adresi girin.

1. Tıklayın **kaydedin.**

1. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için DocuSign.**

1. İçinde **öznitelik eşlemelerini** bölümünde, DocuSign için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri, DocuSign kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için DocuSign'ı etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

1. Tıklayın **kaydedin.**

DocuSign kullanıcılar ve Gruplar bölümünde atanan tüm kullanıcıların ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve DocuSign uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](docusign-tutorial.md)
