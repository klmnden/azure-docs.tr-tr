---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için ThousandEyes yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını ThousandEyes kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
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
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: f008e981abb11a4927ec045c33342bbac9a05bd8
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436813"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için ThousandEyes yapılandırın


Bu öğreticinin amacı ThousandEyes ve Azure AD sağlama ve sağlamasını ThousandEyes Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   Etkin bir [ThousandEyes hesabı](https://www.thousandeyes.com/pricing)
*   Aşağıdaki 3 izinleri içeren bir Role atanmış olan ThousandEyes kullanıcı hesabı:
    * tüm kullanıcıları görüntüle
    * Kullanıcıyı Düzenle
    * API erişim izinleri

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK_ThousandEyes-support-for-SCIM). 

## <a name="assigning-users-to-thousandeyes"></a>ThousandEyes için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir. 

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları ThousandEyes uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar ThousandEyes uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Kullanıcılar için ThousandEyes atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için ThousandEyes atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için ThousandEyes atarken ya da seçmelisiniz **kullanıcı** rol veya başka bir geçerli uygulamaya özgü rolünde (varsa) atama iletişim kutusu. **Varsayılan erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configure-auto-provisioned-user-roles-in-thousandeyes"></a>ThousandEyes içinde otomatik olarak sağlanan kullanıcı rollerini yapılandırma

Her hesap grubu için otomatik sağlama, kullanıcıları, yeni kullanıcı hesabı oluşturulduğunda uygulanacak roller kümesini yapılandırabilirsiniz. Varsayılan olarak, otomatik sağlama kullanıcıların atandığı _normal kullanıcı_ tüm hesap için rolü aksi şekilde yapılandırılmadıkça gruplandırır.

1. Yeni bir otomatik olarak sağlanan kullanıcılar için roller kümesini belirtmek için günlük içine ThousandEyes ve SCIM ayarları bölümüne gidin **>, sağ üst köşede kullanıcı simgesini > Hesap Ayarları > Kuruluş > Güvenlik ve kimlik doğrulaması.** 

   ![SCIM API ayarlarına gidin](https://monosnap.com/file/kqY8Il7eysGFAiCLCQWFizzM27PiBG)

2. Her hesap grubu için bir giriş ekleyin, ardından bir rol kümesi atama *Kaydet* yaptığınız değişiklikleri.

   ![SCIM API aracılığıyla oluşturulan kullanıcılar için varsayılan rollerin ve grupların hesabı ayarlayın](https://monosnap.com/file/16siam6U8xDQH1RTnaxnmIxvsZuNZG)


## <a name="configuring-user-provisioning-to-thousandeyes"></a>ThousandEyes için kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama API'si ThousandEyes'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirme ve Azure AD'de kullanıcı ve Grup atamasına dayalı ThousandEyes atanan kullanıcı hesaplarını devre dışı bırak .

> [!TIP]
> Etkin olarak SAML tabanlı çoklu oturum açma (SSO) aşağıdaki ThousandEyes için tercih edebilirsiniz [Azure Bilgi Bankası'ndaki sağlanan yönergeleri](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-tutorial) SSO tamamlanması. Bu iki özellik birbirini tamamlar ancak SSO otomatik sağlama bağımsız olarak yapılandırılabilir.


### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Otomatik kullanıcı hesabı için ThousandEyes Azure AD'de sağlamayı Yapılandır


1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için ThousandEyes zaten yapılandırdıysanız arama alanını kullanarak ThousandEyes Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **ThousandEyes** uygulama galerisinde. Arama sonuçlarından ThousandEyes seçin ve uygulama listenize ekleyin.

3. Örneğiniz ThousandEyes seçip **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![ThousandEyes sağlama](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **OAuth taşıyıcı belirteci** ThousandEyes hesap tarafından oluşturulan (bulabilir ve veya ThousandEyes hesabınız kapsamında bir belirteç oluşturun  **Profil** bölümü).

    ![ThousandEyes sağlama](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. Azure portalında **Test Bağlantısı** Azure emin olmak için AD ThousandEyes uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa ThousandEyes hesabınız yönetici izinlerine sahip olduğundan emin olun ve 5. adımı yeniden deneyin.

7. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve "bir hata oluştuğunda e-posta bildirimi gönderin." onay kutusunu işaretleyin

8. **Kaydet**’e tıklayın. 

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları ThousandEyes**.

10. İçinde **öznitelik eşlemelerini** bölümünde, ThousandEyes için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri ThousandEyes kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD sağlama hizmeti için ThousandEyes etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. **Kaydet**’e tıklayın. 

Bu işlem, herhangi bir kullanıcı ve/veya ThousandEyes kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
