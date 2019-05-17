---
title: Erişim paneli Web sitesinde oturum açarken sorun | Microsoft Docs
description: Erişim paneli kullanmak için oturum açma çalışırken karşılaşabileceğiniz sorunları gidermek için yönergeler
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.reviwer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7c6a9c3f26c8939176197a2ecf2fcd6026e9928
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784318"
---
# <a name="problem-signing-in-to-the-access-panel-website"></a>Erişim paneli Web sitesinde oturum açarken sorun

Erişim paneli, Azure AD Yöneticisi erişim izni görüntüleyin ve başlatma bulut tabanlı uygulamalar, Azure Active Directory (Azure AD) bir iş veya Okul hesabı olan bir kullanıcının sağlayan web tabanlı bir portal sağlamaktır. Azure AD sürümleri olan bir kullanıcı, Self Servis grup ve uygulama erişim panelinde üzerinden yönetim özellikleri de kullanabilirsiniz. Erişim paneli, Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliğine sahip olmasını gerektirmez.

Azure AD'de bir iş veya Okul hesabı varsa kullanıcılar için erişim paneli oturum açabilir.

-   Kullanıcılar, doğrudan Azure AD tarafından doğrulanabilir.

-   Kullanıcıların, Active Directory Federasyon Hizmetleri (AD FS) kullanılarak doğrulanabilir.

-   Kullanıcılar, Windows Server Active Directory tarafından doğrulanabilir.

Bir kullanıcı, Azure veya Office 365 için bir aboneliği olduğundan ve Azure portalını veya Office 365 uygulamasını kullanarak erişim panelinde sorunsuz bir şekilde gerek kalmadan yeniden oturum açmak için kullandığınız mümkün olacaktır. Kimliği doğrulanmamış kullanıcılar, kendi hesabı için Azure AD'de kullanıcı adı ve parola kullanarak oturum açmanız istenir. Kuruluşun Federasyon yapılandırdıysa, kullanıcı adını yazarak yeterli olur.

## <a name="general-issues-to-check-first"></a>İlk denetlenecek genel sorunlar 

-   Emin kullanıcının oturum açmak için **URL'yi düzeltin**: <https://myapps.microsoft.com>

-   Kullanıcının tarayıcı URL ekledi emin olun, **Güvenilen siteler**

-   Kullanıcı hesabı olduğundan emin olun **etkin** için oturum açma işlemleri.

-   Kullanıcı hesabı olduğundan emin olun **kilitli değil.**

-   Kullanıcının emin **parola süresi değil veya unutulursa.**

-   Emin **multi-Factor Authentication** kullanıcı erişimi engellemediğini.

-   Emin bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi, kullanıcı erişimini engellemediğinden.

-   Emin olun kullanıcının **kimlik doğrulaması iletişim bilgileri** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkeleri izin vermek için güncel durumda.

-   Tarayıcınızın tanımlama bilgilerini temizleyip tekrar oturum açmaya çalışırken de deneyin için emin olun.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Toplantı için erişim paneli tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma (SSO) erişim Paneli'nde kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bu uzantı, bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde otomatik olarak indirilir.

Parola tabanlı SSO için son kullanıcının tarayıcılar olabilir:

-   Internet Explorer 8, 9, 10, 11 – Windows 7 veya üzeri

-   Windows 10 Anniversary Edition veya sonrası, Microsoft Edge 

-   Chrome--Üzerinde Windows 7 ve daha sonra ve MacOS x veya sonrası

-   Firefox 26,0 veya daha sonra--Windows XP SP2 veya üstü ve Mac OS X 10,6 veya sonraki bir sürümü üzerinde


## <a name="problems-with-the-users-account"></a>Kullanıcı hesabı ile ilgili sorunlar

Kullanıcı hesabı ile ilgili bir sorun nedeniyle, erişim Paneli'ne erişim engellenebilir. Sorun giderme ve kullanıcıları ve hesap ayarlarına çözmekte bazı yollar şunlardır:

-   [Bir kullanıcı hesabı Azure Active Directory'de mevcut olup olmadığını denetleyin](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Kullanıcı hesabınızın durumunu denetleyin](#check-a-users-account-status)

-   [Bir kullanıcının parolasını sıfırlama](#reset-a-users-password)

-   [Self servis parola sıfırlamayı etkinleştirme](#enable-self-service-password-reset)

-   [Bir kullanıcının çok faktörlü kimlik doğrulaması durumunu denetleyin](#check-a-users-multi-factor-authentication-status)

-   [Bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol edin](#check-a-users-authentication-contact-info)

-   [Bir kullanıcının grup üyeliklerini denetle](#check-a-users-group-memberships)

-   [Bir kullanıcının atanan lisansları denetleme](#check-a-users-assigned-licenses)

-   [Bir kullanıcı bir lisans atayın](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Bir kullanıcı hesabı Azure Active Directory'de mevcut olup olmadığını denetleyin

Bir kullanıcı hesabının mevcut olup olmadığını denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  Veri içermeyen eksik ve beklediğiniz gibi göründüğünü emin olmak için kullanıcı nesnesinin özelliklerini denetleyin.

### <a name="check-a-users-account-status"></a>Kullanıcı hesabınızın durumunu denetleyin

Bir kullanıcı hesabı durumunu denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **profili**.

8.  Altında **ayarları** emin **oturum açmayı engelle** ayarlanır **Hayır**.

### <a name="reset-a-users-password"></a>Bir kullanıcının parolasını sıfırlama

Bir kullanıcının parolasını sıfırlamak için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **parolayı Sıfırla** kullanıcı bölmenin üstünde düğme.

8.  tıklayın **parolayı Sıfırla** düğmesini **parolayı Sıfırla** bölmesi görünür.

9.  Kopyalama **geçici parola** veya **yeni bir parola girin** kullanıcı.

10. Bu yeni parolayı kullanıcıya iletişim, Azure Active Directory, sonraki oturum açma sırasında bu parolayı değiştirmeniz gerekiyor.

### <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

Self Servis parola sıfırlamayı etkinleştirmek için dağıtım adımları izleyin:

-   [Azure Active Directory parolalarını sıfırlamalarına olanak tanıma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Kullanıcılara sıfırlama veya Active Directory şirket içi parolalarını değiştirme olanağı](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

### <a name="check-a-users-multi-factor-authentication-status"></a>Bir kullanıcının çok faktörlü kimlik doğrulaması durumunu denetleyin

Bir kullanıcının çok faktörlü kimlik doğrulaması durumunu denetlemek için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. tıklayın **multi-Factor Authentication** bölmenin üstünde düğme.

7. Bir kez **multi-Factor Authentication Yönetim Portalı** yüklendiğinde olun, bulunduğunuz **kullanıcılar** sekmesi.

8. Kullanıcı, arama, filtreleme veya sıralama kullanıcılar listesinde bulun.

9. Kullanıcı listesinden kullanıcıyı seçin ve **etkinleştirme**, **devre dışı**, veya **zorla** istediğiniz gibi çok faktörlü kimlik doğrulaması.

   >[!NOTE]
   >Bir kullanıcı olarak bulunuyorsa bir **zorlanan** durumunda ayarlayın bunları **devre dışı bırakılmış** geçici olarak geri hesaba izin vermek için. Daha sonra geri içinde bulundukları sonra durumlarına değiştirebilirsiniz **etkin** kişi bilgileri, sonraki oturum açma sırasında yeniden kaydolmak için bunları yeniden istemek için. Alternatif olarak, adımları izleyebilirsiniz [bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol](#check-a-users-authentication-contact-info) doğrulamak veya bunlar için bu veri kümesi için.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol edin

Çok faktörlü kimlik doğrulaması, koşullu erişim, kimlik koruması ve parola sıfırlama için kullanılan kullanıcı kimlik doğrulaması iletişim bilgileri denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **profili**.

8.  Ekranı aşağı kaydırarak **kimlik doğrulaması iletişim bilgileri**.

9.  **Gözden geçirme** veri, güncelleştirme ve kullanıcı için gerektiği şekilde kayıtlı.

### <a name="check-a-users-group-memberships"></a>Bir kullanıcının grup üyeliklerini denetle

Bir kullanıcının grup üyeliklerini denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **grupları** olduğu kullanıcı grupları görmek için bir üyesidir.

### <a name="check-a-users-assigned-licenses"></a>Bir kullanıcının atanan lisansları denetleme

Bir kullanıcının lisans atanmış denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

### <a name="assign-a-user-a-license"></a>Bir kullanıcı bir lisans atayın 

Bir kullanıcıya bir lisans atamak için bu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

8.  Tıklayın **atama** düğmesi.

9.  Seçin **bir veya daha çok ürünlerin** kullanılabilir ürünler listesinden.

10. **İsteğe bağlı** tıklayın **atama seçenekleri** hedefle ürünleri atamak için öğesi. Tıklayın **Tamam** bu ne zaman tamamlanır.

11. Tıklayın **atama** bu lisans bu kullanıcıya atamak için düğme.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Bu sorun giderme adımlarını sorunu çözmezse

Aşağıdaki bilgilerle varsa bir destek bileti açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesi sırasında hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
