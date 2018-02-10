---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı ServiceNow yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den ServiceNow sağlanmasını öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 50d5ecd0542d236d4d68656af7808c329728aa39
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı ServiceNow yapılandırın.

Bu öğreticinin amacı ServiceNow ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den ServiceNow sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   İş veya eğitim için ServiceNow ServiceNow için geçerli bir kiracı olması gerekir. Ücretsiz bir deneme hesabı ya da hizmet için kullanabilir.
*   ServiceNow kullanıcı hesabında takım yönetici izinlerine sahip.

## <a name="assigning-users-to-servicenow"></a>ServiceNow kullanıcılar atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları ServiceNow uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra bu kullanıcılar ServiceNow uygulamanıza Buradaki yönergeleri izleyerek atayabilirsiniz: [bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için ServiceNow atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.
>*   Bir kullanıcı ServiceNow atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD ServiceNow'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre ServiceNow atanan kullanıcı hesaplarında devre dışı bırakın.

> [!TIP]
>Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için ServiceNow, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı Yapılandır

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için servicenow'ı zaten yapılandırdıysanız arama alanı kullanarak ServiceNow Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **ServiceNow** uygulama galerisinde. Arama sonuçlarından servicenow'ı seçin ve uygulamaları listenize ekleyin.

3. ServiceNow örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama** moduna **otomatik**. 

    ![sağlama](./media/active-directory-saas-servicenow-provisioning-tutorial/provisioning.png)

5. Yönetici kimlik bilgileri bölümü altında aşağıdaki adımları gerçekleştirin:
   
    a. İçinde **ServiceNow örneği adı** metin kutusuna, ServiceNow örneğinin adını yazın.

    b. İçinde **ServiceNow yönetici kullanıcı adı** metin kutusuna, bir yönetici kullanıcı adını yazın.

    c. İçinde **ServiceNow yönetici parolası** metin kutusuna, yöneticinin parola.

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD ServiceNow uygulamanıza bağlanabilir. Bağlantı başarısız olursa ServiceNow hesabınıza takım yönetici izinlerine sahip olduğundan emin olun ve deneyin **"Yönetici kimlik bilgileri"** adım yeniden uygulayın.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

8. Tıklatın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları ServiceNow.**

10. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den ServiceNow eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri ServiceNow kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. ServiceNow hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

12. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde ServiceNow atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve ServiceNow uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-servicenow-tutorial.md)


