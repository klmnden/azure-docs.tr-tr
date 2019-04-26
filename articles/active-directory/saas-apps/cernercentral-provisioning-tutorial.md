---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Cerner Orta yapılandırma | Microsoft Docs'
description: Azure Active Directory kullanıcı Cerner Orta bir listesi için otomatik sağlama için yapılandırmayı öğrenin.
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
ms.openlocfilehash: 00a967d61a5f81fc871488ea48df9cb4cf18c269
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60281205"
---
# <a name="tutorial-configure-cerner-central-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Cerner Orta yapılandırın

Bu öğreticinin amacı Cerner merkezi ve Azure AD'deki otomatik olarak sağlama ve sağlamasını Cerner Central içinde bir kullanıcı listesi için Azure AD'den kullanıcı hesapları için gerçekleştirmeniz gereken adımlar gösterir sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* Cerner Orta Kiracı

> [!NOTE]
> Azure Active Directory kullanarak Cerner Central ile tümleştirilir [SCIM](http://www.simplecloud.info/) protokolü.

## <a name="assigning-users-to-cerner-central"></a>Merkezi siteden Cerner kullanıcıları atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir. 

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Cerner Orta erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar için Cerner Orta atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Kullanıcılar için Orta Cerner atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Cerner Orta atanabilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Tek bir kullanıcı için ilk testi tamamlandıktan sonra listenin tamamını Cerner'ın kullanıcı listesi için sağlanacak Cerner çözümlerle (yalnızca Cerner Merkezi) erişmeye yönelik kullanıcı atama Cerner Orta önerir.  Bu kullanıcı listesi içerisindeki kullanıcıların listesi diğer Cerner çözümleri yararlanın.

* Bir kullanıcı için Cerner Orta atarken seçmelisiniz **kullanıcı** rol ataması iletişim. "Varsayılan erişimi" rolüne sahip kullanıcılar, sağlamasından bırakılır.

## <a name="configuring-user-provisioning-to-cerner-central"></a>Cerner Orta için kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD sağlama API'si Cerner'ın SCIM kullanıcı hesabını kullanarak Cerner Central'ın kullanıcı listesi için bağlama size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesaplarında Cerner Orta temel devre dışı Azure AD'de kullanıcı ve grup atama.

> [!TIP]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için Cerner Orta, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirini tamamlar ancak otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir. Daha fazla bilgi için [oturum açma Cerner Orta tek öğretici](cernercentral-tutorial.md).

### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Otomatik kullanıcı hesabı Azure AD'de Cerner merkezi siteden sağlamayı yapılandırmak için:

Cerner Orta için kullanıcı hesapları sağlamak için Cerner Cerner Merkezi sistem hesabı isteyin ve Azure AD Cerner'ın SCIM uç noktaya bağlanmak için kullanabileceğiniz bir OAuth taşıyıcı belirteci oluşturmak gerekir. Tümleştirme Cerner korumalı alan ortamında üretim ortamına dağıtmadan önce gerçekleştirilmesi önerilir.

1. İlk adım Cerner yönetme kişiler sağlamak ve Azure AD tümleştirme yönergeleri işlemini tamamlamak için gereken belgelere erişmek için gerekli bir CernerCare hesabı. Gerekirse, her bir geçerli ortamda CernerCare hesaplarını oluşturmak için aşağıdaki URL'lerden kullanın.

   * Korumalı alan:  https://sandboxcernercare.com/accounts/create

   * Üretim:  https://cernercare.com/accounts/create  

2. Ardından, Azure AD için bir sistem hesabı oluşturulmalıdır. Korumalı alan ve üretim ortamlarınız için bir sistem hesabı istemek için aşağıdaki yönergeleri kullanın.

   * Yönergeler:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Korumalı alan: https://sandboxcernercentral.com/system-accounts/

   * Üretim:  https://cernercentral.com/system-accounts/

3. Ardından, bir OAuth taşıyıcı belirteci her biri, sistem hesapları için oluşturur. Bunu yapmak için aşağıdaki yönergeleri izleyin.

   * Yönergeler:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Korumalı alan: https://sandboxcernercentral.com/system-accounts/

   * Üretim:  https://cernercentral.com/system-accounts/

4. Son olarak, yapılandırmayı tamamlamak için Cerner hem korumalı hem de üretim ortamlarında için kullanıcı listesi bölge kimliklerini almak gerekir. Bu alma hakkında daha fazla bilgi için bkz: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Artık Azure AD Cerner için kullanıcı hesapları sağlamak üzere yapılandırabilirsiniz. Oturum [Azure portalında](https://portal.azure.com)ve **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

6. Çoklu oturum açma için zaten Cerner Orta yapılandırdıysanız, Cerner arama alanını kullanarak merkezi Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Cerner orta** uygulama galerisinde. Arama sonuçlarından Cerner Orta seçin ve uygulama listenize ekleyin.

7. Örneğiniz Cerner Orta seçip **sağlama** sekmesi.

8. Ayarlama **hazırlama modu** için **otomatik**.

   ![Cerner sağlama Orta](./media/cernercentral-provisioning-tutorial/Cerner.PNG)

9. Altında aşağıdaki alanları doldurun **yönetici kimlik bilgileri**:

   * İçinde **Kiracı URL'si** #4. adımda alınan bölge kimliği ile "User-listesi-bölge-ID" değiştirerek bir URL aşağıdaki biçimde girin.

    > Korumalı alan: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 
    > 
    > Üretim: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * İçinde **gizli belirteç** alan, #3. adımda oluşturulan OAuth taşıyıcı belirtecini girin ve tıklayın **Bağlantıyı Sına**.

   * Portalınızı upperright tarafında bir başarı bildirimini görmeniz gerekir.

1. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

1. **Kaydet**’e tıklayın.

1. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Cerner merkezi siteden eşitlenmesi için kullanıcı ve grup öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için Cerner merkez grupları ve kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Azure AD sağlama hizmeti için Cerner Orta etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

1. **Kaydet**’e tıklayın.

Bu, herhangi bir kullanıcı ve/veya Cerner Orta kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitlemeyi başlatır. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Cerner Orta uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Cerner Merkezi: Azure AD kullanarak kimlik verileri yayımlama](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Öğretici: Azure Active Directory ile çoklu oturum açma için yapılandırma Cerner Orta](cernercentral-tutorial.md)
* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).
