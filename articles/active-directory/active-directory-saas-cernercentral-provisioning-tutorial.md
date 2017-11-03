---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Cerner Orta yapılandırma | Microsoft Docs"
description: "Azure Active Directory kullanıcılara otomatik olarak sağlamak için bir ad listesi Cerner Orta yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: 84613b7f8d7bd031d492a62da0bc53be96ac45a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a>Öğretici: Cerner Orta otomatik kullanıcı sağlamayı için yapılandırma

Bu öğreticinin amacı Cerner Orta ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den kullanıcı ad listesi Cerner Orta sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır. 


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active Directory kiracısı
*   Cerner Orta Kiracı 

> [!NOTE]
> Azure Active Directory Cerner kullanarak orta ile tümleşen [SCIM'yi](http://www.simplecloud.info/) protokolü.

## <a name="assigning-users-to-cerner-central"></a>Kullanıcıları Cerner Orta atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Cerner Orta erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Cerner Orta atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Kullanıcılar için Cerner Orta atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Cerner Orta atanabilir. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

* Tek bir kullanıcı için ilk test tamamlandıktan sonra Cerner Orta Cerner'ın kullanıcı kadrosu sağlanacak kullanıcıların herhangi bir Cerner çözümü (yalnızca Cerner Merkezi) erişmek için hedeflenen tüm listesini atama önerir.  Diğer Cerner çözümleri kullanıcı ad listesi kullanıcılar bu listesi yararlanın.

*   Bir kullanıcı için Cerner Orta atarken seçmelisiniz **kullanıcı** rol ataması iletişim. "Varsayılan erişim" rolüne sahip kullanıcılar sağlamasından hariç tutulur.


## <a name="configuring-user-provisioning-to-cerner-central"></a>Kullanıcı Cerner Orta sağlama yapılandırma

Bu bölümde Azure AD Cerner Orta'nın kullanıcı ad için listesi API sağlama Cerner'ın SCIM'yi kullanıcı hesabını kullanarak olduğu konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirmek ve atanan kullanıcıya Cerner Orta hesaplarında dayalı olarak devre dışı bırak Azure AD'de kullanıcı ve grup atama.

> [!TIP]
> Etkin SAML tabanlı çoklu oturum açma [Azure Portalı'nda (https://portal.azure.com). sağlanan yönergeleri izleyerek Cerner Orta tercih edebilirsiniz Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir. Daha fazla bilgi için bkz: [oturum açma Cerner Orta tek Öğreticisi](active-directory-saas-cernercentral-tutorial.md).


### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için Cerner Orta Azure AD'de sağlama yapılandırmak için:


Kullanıcı hesaplarına Cerner Orta sağlamak için Cerner Cerner Orta sistem hesabı istemek ve Azure AD Cerner'ın SCIM'yi uç noktasına bağlanmak için kullanabileceğiniz bir OAuth taşıyıcı belirteci üretmek gerekir. Ayrıca, üretime dağıtılmadan önce tümleştirme Cerner sanal ortamda gerçekleştirilmesi önerilir.

1.  Cerner yönetme kişiler emin olmak için ilk adımdır ve Azure AD tümleştirme yönergeleri işlemini tamamlamak için gereken belgelerine erişmek için gerekli bir CernerCare hesabı vardır. Gerekirse, her bir geçerli ortamda CernerCare hesapları oluşturmak için aşağıdaki URL'ler kullanın.

   * Korumalı alan: https://sandboxcernercare.com/accounts/create

   * Üretim: https://cernercare.com/accounts/create  

2.  Ardından, bir sistem hesabı için Azure AD oluşturulması gerekir. Korumalı alan ve üretim ortamları için bir sistem hesabı istemek için aşağıdaki yönergeleri kullanın.

   * Yönergeler: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Korumalı alan: https://sandboxcernercentral.com/system-accounts/

   * Üretim: https://cernercentral.com/system-accounts/

3.  Ardından, OAuth taşıyıcı belirteci her sistem hesapları için oluşturur. Bunu yapmak için aşağıdaki yönergeleri izleyin.

   * Yönergeler: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Korumalı alan: https://sandboxcernercentral.com/system-accounts/

   * Üretim: https://cernercentral.com/system-accounts/

4. Son olarak, yapılandırmayı tamamlamak için Cerner hem korumalı hem de üretim ortamlarında için kullanıcı ad listesi bölge kimlikleri alma gerekir. Bu alma hakkında daha fazla bilgi için bkz: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Artık Azure AD kullanıcı hesaplarına Cerner sağlamak için yapılandırabilirsiniz. Oturum [Azure portal](https://portal.azure.com)ve **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

6. Çoklu oturum açma için zaten Cerner Orta yapılandırdıysanız, Cerner arama alanı kullanarak orta Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Cerner orta** uygulama galerisinde. Arama sonuçlarından Cerner Orta seçin ve uygulamaları listenize ekleyin.

7.  Cerner Orta örneğiniz seçin ve ardından **sağlama** sekmesi.

8.  Ayarlama **sağlama modunda** için **otomatik**.

   ![Cerner sağlama Orta](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  Altında aşağıdaki alanları doldurun **yönetici kimlik bilgileri**:

   * İçinde **Kiracı URL** alan, "Kullanıcı-ad listesi-bölge-ID" #4. adımda aldığınız bölge kimliği değiştirerek bir URL aşağıdaki biçimde girin.

> Korumalı alan: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

> Üretim: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * İçinde **gizli belirteci** alanına #3. adımda oluşturulan OAuth taşıyıcı belirteci girin ve tıklatın **Bağlantıyı Sına**.

   * Portalınızı upperright tarafında bir başarı bildirimi görmelisiniz.

10. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

11. **Kaydet** düğmesine tıklayın. 

12. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Cerner Orta eşitlenmek üzere kullanıcı ve grup öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesapları ve grupları Cerner Orta güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

13. Azure AD hizmeti Cerner Orta için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

14. **Kaydet** düğmesine tıklayın. 

Bu, herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde Cerner Orta atanan ilk eşitleme başlatır. İlk eşitlemeyi gerçekleştirmek yaklaşık 20 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Cerner Orta uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

## <a name="additional-resources"></a>Ek kaynaklar

* [Cerner Orta: Azure AD kullanarak kimlik verilerini yayımlama](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Öğretici: Azure Active Directory ile çoklu oturum açma Cerner Orta yapılandırma](active-directory-saas-cernercentral-tutorial.md)
* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).
