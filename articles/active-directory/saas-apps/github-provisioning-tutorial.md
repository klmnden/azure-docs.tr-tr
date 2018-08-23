---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için GitHub yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını GitHub kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.openlocfilehash: ff22ad4f8abae019591150dd9a8ea2cd4ca4e5d9
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42061731"
---
# <a name="tutorial-configure-github-for-automatic-user-provisioning"></a>Öğretici: GitHub için otomatik kullanıcı hazırlama yapılandırın


Bu öğreticinin amacı, GitHub ve Azure AD sağlama ve sağlamasını GitHub Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   Bir Github kiracıyla [iş planı](https://help.github.com/articles/organization-billing-plans/#business-plan) ya da daha iyi etkin 
*   GitHub yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE]
> Azure AD tümleştirmesi sağlama kullanır [GitHub SCIM API](https://developer.github.com/v3/scim/), daha iyi veya Github takımlar iş plan için kullanılabilir.

## <a name="assigning-users-to-github"></a>GitHub için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir. 

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları GitHub uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar GitHub uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a>Önemli ipuçları için GitHub kullanıcı atama

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için GitHub atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için GitHub atarken ya da seçmelisiniz **kullanıcı** rol veya başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. **Varsayılan erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.


## <a name="configuring-user-provisioning-to-github"></a>GitHub için kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama API'si GitHub'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı github'da devre dışı bırak.

> [!TIP]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için GitHub, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a>Otomatik kullanıcı hesabı için GitHub Azure AD'de sağlamayı Yapılandır


1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için GitHub zaten yapılandırdıysanız arama alanını kullanarak GitHub Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **GitHub** uygulama galerisinde. GitHub Arama sonuçlarından seçin ve uygulama listenize ekleyin.

3. GitHub örneğinizi seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![GitHub sağlama](./media/github-provisioning-tutorial/GitHub1.png)

5. Altında **yönetici kimlik bilgileri** bölümünde **Authorize**. Bu işlem, yeni bir tarayıcı penceresinde GitHub yetkilendirme iletişim kutusu açılır. 

6. Yeni pencerede, Github'da oturum yönetici hesabınızı kullanarak oturum açın. Sonuç yetkilendirme iletişim için sağlamayı etkinleştirmek için istediğiniz GitHub takımı seçin ve ardından **Authorize**. Tamamlandığında, sağlama yapılandırmasını tamamlamak için Azure portalına dönün.

    ![Yetkilendirme iletişim](./media/github-provisioning-tutorial/GitHub2.png)

7. Azure portalında giriş **Kiracı URL'si** tıklatıp **Test Bağlantısı** Azure emin olmak için AD, GitHub uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa, GitHub hesabınızı yönetici izinlerine sahip olduğundan emin olun ve **Kiracı URL'si** doğru girilen ve "Yetkilendir" adımı yeniden deneyin (, oluşturabilecek **Kiracı URL'si** kuralı tarafından: `https://api.github.com/scim/v2/<Organizations_name>` Kuruluşunuzun GitHub hesabınızın altında bulabilirsiniz: **ayarları** > **kuruluşlar**).

    ![Yetkilendirme iletişim](./media/github-provisioning-tutorial/GitHub3.png)

8. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve "bir hata oluştuğunda e-posta bildirimi gönderin." onay kutusunu işaretleyin

9. **Kaydet**’e tıklayın. 

10. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için GitHub**.

11. İçinde **öznitelik eşlemelerini** bölümünde, Github'da Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri, GitHub kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

12. Azure AD sağlama hizmeti için GitHub'ı etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

13. **Kaydet**’e tıklayın. 

Bu işlem, herhangi bir kullanıcı ve/veya GitHub kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)
