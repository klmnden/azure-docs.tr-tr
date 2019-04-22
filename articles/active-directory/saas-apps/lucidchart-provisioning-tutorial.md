---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için LucidChart yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını LucidChart kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: daveba
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: asmalser-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc181625ead251480bb107fc59e3aae46afab1ee
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59257697"
---
# <a name="tutorial-configure-lucidchart-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için LucidChart yapılandırın

Bu öğreticinin amacı LucidChart ve Azure AD sağlama ve sağlamasını LucidChart Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Bir Azure Active directory kiracısı
* LucidChart kiracıyla [Kurumsal plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) ya da daha iyi etkin
* LucidChart yönetici izinlerine sahip bir kullanıcı hesabı

## <a name="assigning-users-to-lucidchart"></a>LucidChart için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları LucidChart uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar LucidChart uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a>Kullanıcılar için LucidChart atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için LucidChart atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için LucidChart atarken ya da seçmelisiniz **kullanıcı** rol veya başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. **Varsayılan erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configuring-user-provisioning-to-lucidchart"></a>LucidChart için kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD sağlama API'si LucidChart'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirme ve Azure AD'de kullanıcı ve Grup atamasına dayalı LucidChart atanan kullanıcı hesaplarını devre dışı bırakın.

> [!TIP]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için LucidChart, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a>Otomatik kullanıcı hesabı için LucidChart Azure AD'de sağlamayı Yapılandır

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için LucidChart zaten yapılandırdıysanız arama alanını kullanarak LucidChart Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **LucidChart** uygulama galerisinde. Arama sonuçlarından LucidChart seçin ve uygulama listenize ekleyin.

3. Örneğiniz LucidChart seçip **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![LucidChart sağlama](./media/lucidchart-provisioning-tutorial/LucidChart1.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **gizli belirteç** , LucidChart'ın hesap tarafından oluşturulan (belirteç hesabınızın altında bulabilirsiniz: **Takım** > **uygulama tümleştirmesi** > **SCIM**).

    ![LucidChart sağlama](./media/lucidchart-provisioning-tutorial/LucidChart2.png)

6. Azure portalında **Test Bağlantısı** Azure emin olmak için AD LucidChart uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa LucidChart hesabınız yönetici izinlerine sahip olduğundan emin olun ve 5. adımı yeniden deneyin.

7. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve "bir hata oluştuğunda e-posta bildirimi gönderin." onay kutusunu işaretleyin

8. **Kaydet**’e tıklayın.

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları LucidChart**.

10. İçinde **öznitelik eşlemelerini** bölümünde, LucidChart için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri LucidChart kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD sağlama hizmeti için LucidChart etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. **Kaydet**’e tıklayın.

Bu işlem, herhangi bir kullanıcı ve/veya LucidChart kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
