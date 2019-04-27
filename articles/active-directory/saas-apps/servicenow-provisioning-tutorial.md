---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için servicenow'ı yapılandırma | Microsoft Docs"
description: Otomatik olarak sağlama ve sağlamasını servicenow'ı Azure AD'ye kullanıcı hesaplarını hakkında bilgi edinin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19b3e4cc5ba4bc0173721947bd1e1a680ca7b3a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60869851"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için servicenow'ı yapılandırma

Bu öğreticinin amacı, ServiceNow ve Azure AD'deki otomatik olarak sağlama ve sağlamasını ServiceNow için Azure AD'den kullanıcı hesapları için gerçekleştirmeniz gereken adımlar gösterir sağlamaktır.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi ServiceNow ile yapılandırma için aşağıdakiler gerekir:

- Azure AD aboneliği
- Servicenow'ı, bir örnek veya Kiracı, ServiceNow, Calgary sürümü veya üzeri
- Servicenow'ı hızlı, ServiceNow Express, Helsinki sürümü örneğini veya üzeri

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="assigning-users-to-servicenow"></a>ServiceNow kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları ServiceNow uygulamanızı erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar ServiceNow uygulamanızı atayabilirsiniz: [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için ServiceNow atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.
>*   Bir kullanıcı için ServiceNow atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si ServiceNow'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı servicenow'ı devre dışı.

> [!TIP]
>Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için ServiceNow, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı yapılandırma

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

1. Çoklu oturum açma için servicenow'ı zaten yapılandırdıysanız arama alanını kullanarak ServiceNow Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **ServiceNow** uygulama galerisinde. Arama sonuçlarından ServiceNow'ı seçin ve uygulama listenize ekleyin.

1. ServiceNow örneğinizin seçip **sağlama** sekmesi.

1. Ayarlama **sağlama** moduna **otomatik**. 

    ![sağlama](./media/servicenow-provisioning-tutorial/provisioning.png)

1. Yönetici kimlik bilgileri bölümü altında aşağıdaki adımları gerçekleştirin:
   
    a. İçinde **Servıcenow örneğinin adı** metin Servıcenow örneğinin adı yazın.

    b. İçinde **ServiceNow yönetici kullanıcı adı** metin kutusu, bir yönetici kullanıcı adını yazın.

    c. İçinde **ServiceNow yönetici parolası** yöneticinin parolasını metin.

1. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, ServiceNow uygulamanızı bağlanabilirsiniz. Bağlantı başarısız olursa, ServiceNow hesabınızda takım Yöneticisi izinlerine sahip olduğundan emin olun ve deneyin **"Yönetici kimlik bilgileri"** adım yeniden uygulayın.

1. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

1. Tıklayın **kaydedin.**

1. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için ServiceNow.**

1. İçinde **öznitelik eşlemelerini** bölümünde, ServiceNow için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için ServiceNow kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için ServiceNow'ı etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

1. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya ServiceNow kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve ServiceNow uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](servicenow-tutorial.md)


