---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Netsuite yapılandırma | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Netsuite arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: c7d335df3509365954cc4b64c284d0460576e187
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35970243"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Öğretici: Netsuite otomatik kullanıcı sağlamayı için yapılandırma

Bu öğreticinin amacı Netsuite ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den Netsuite sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir Netsuite çoklu oturum açma etkin abonelik.
*   Bir kullanıcı hesabında Netsuite takım yönetici izinlerine sahip.

## <a name="assigning-users-to-netsuite"></a>Kullanıcılar için Netsuite atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Netsuite uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Netsuite uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite"></a>Kullanıcılar için Netsuite atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Netsuite atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Netsuite atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD Netsuite'nın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre Netsuite atanan kullanıcı hesaplarında devre dışı bırakın.

> [!TIP] 
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Netsuite, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Netsuite kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Netsuite zaten yapılandırdıysanız arama alanı kullanarak Netsuite Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Netsuite** uygulama galerisinde. Arama sonuçlarından Netsuite seçin ve uygulamaları listenize ekleyin.

3. Netsuite örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![sağlama](./media/netsuite-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlar:
   
    a. İçinde **yönetici kullanıcı adı** metin kutusuna, bir Netsuite hesap adı türü **Sistem Yöneticisi** atanan Netsuite.com profilinde.
   
    b. İçinde **yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.
      
6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Netsuite uygulamanıza bağlanabilir.

7. İçinde **bildirim e-posta** alan, bir kişi veya grubun sağlama hata bildirimleri almak ve gerekir onay e-posta adresini girin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Netsuite.**

10. İçinde **öznitelik eşlemelerini** bölümünde, Netsuite için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri Netsuite kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD hizmeti Netsuite için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

12. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde Netsuite atanan ilk eşitleme başlatır. İlk eşitlemeyi gerçekleştirmek için yaklaşık her 40 dakika çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer unutmayın. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Netsuite uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](netsuite-tutorial.md)