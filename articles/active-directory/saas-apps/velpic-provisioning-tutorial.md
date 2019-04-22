---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Velpic yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Velpic kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16c302fbe151d6cd8c2198240bc31a2bd69dbd7b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59270924"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Velpic yapılandırma

Bu öğreticinin amacı Velpic ve Azure AD sağlama ve sağlamasını Velpic Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* Velpic kiracıyla [Kurumsal plan](https://www.velpic.com/pricing.html) ya da daha iyi etkin
* Velpic yönetici izinlerine sahip bir kullanıcı hesabı

## <a name="assigning-users-to-velpic"></a>Velpic için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenecektir. 

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Velpic uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar Velpic uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Kullanıcılar için Velpic atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Velpic atanabilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için Velpic atarken ya da seçmelisiniz **kullanıcı** rol veya başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. Unutmayın **varsayılan erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanacak.

## <a name="configuring-user-provisioning-to-velpic"></a>Velpic için kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD sağlama API'si Velpic'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve kullanıcı hesapları, kullanıcı ve Grup ataması Azure AD'de göre Velpic atanan sağlama hizmeti oluşturma, güncelleştirme ve devre dışı bırakmak için yapılandırma.

> [!TIP]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için Velpic, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Otomatik kullanıcı hesabı için Velpic Azure AD'de sağlama yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Velpic zaten yapılandırdıysanız arama alanını kullanarak Velpic Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Velpic** uygulama galerisinde. Arama sonuçlarından Velpic seçin ve uygulama listenize ekleyin.

3. Örneğiniz Velpic seçip **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Velpic sağlama](./media/velpic-provisioning-tutorial/Velpic1.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si & Gizli belirteç** Velpic,. () Bu değerleri Velpic hesabınızın altında bulabilirsiniz: **Yönetme** > **tümleştirme** > **eklentisi** > **SCIM**)

    ![Yetkilendirme değerleri](./media/velpic-provisioning-tutorial/Velpic2.png)

6. Azure portalında **Test Bağlantısı** Azure emin olmak için AD Velpic uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa Velpic hesabınız yönetici izinlerine sahip olduğundan emin olun ve 5. adımı yeniden deneyin.

7. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

8. **Kaydet**’e tıklayın.

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Velpic**.

10. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Velpic için eşitlenecek kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri Velpic kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD sağlama hizmeti için Velpic etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. **Kaydet**’e tıklayın.

Bu, herhangi bir kullanıcı ve/veya Velpic kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitlemeyi başlatır. İlk eşitleme hizmetini çalıştıran sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik raporlarını sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)