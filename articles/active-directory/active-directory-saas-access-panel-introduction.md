---
title: "Azure Active Directory'ye erişim panelinde nedir? | Microsoft Docs"
description: "(Web tarayıcısı, Android uygulaması, iPhone ve iPad uygulama) SaaS uygulamalara erişmek için erişim paneli varyasyonları kullanmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4178b07f59885a67b12f0863129995542ee0752a
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="what-is-the-access-panel"></a>Erişim paneli nedir?

Erişim paneli web tabanlı bir portalıdır. Bir kullanıcı bir iş etkinleştirir ya da Okul hesabı görüntülemek ve bulut tabanlı uygulamalar başlatmak için Azure Active Directory'de Azure AD yönetici bunları erişim izni. Ayrıca, Self Servis grup ve erişim paneli üzerinden uygulama yönetim özellikleri de kullanabilirsiniz.

Erişim paneli Azure Portalı'ndan ayrıdır ve bir Azure aboneliğinizin olmasını sizin yapar.

![Erişim Paneli][1]

Erişim paneli yeteneği dahil olmak üzere profil ayarlarınızı bazıları düzenlemenize olanak tanır:

- Bir iş veya Okul hesabıyla ilişkili parolayı değiştirme

- Parola sıfırlama ayarlarını Düzenle

- Çok faktörlü kimlik doğrulamasını (yönetici tarafından kullanmak için gerekli hesapları için) ilgili kişi ve tercih ayarlarını Düzenle

- Kullanıcı kimliği gibi ayrıntıları e-posta ve mobil ve office telefon numaraları ve aygıtların alternatif hesabı görüntüleyin

- Görüntülemek ve Azure AD Yöneticisi bunları erişim izni bulut tabanlı uygulamalar başlatın. Kullanıcı açısından erişim paneli hakkında daha fazla bilgi için bkz: erişim paneli kullanma. 

- Kendi kendine gruplarını yönetin. Daha belirgin olarak yönetici oluşturmak ve güvenlik gruplarını yönetebilir ve Azure AD'de güvenlik grubu üyeliği isteyin. Daha fazla bilgi için bkz: [Azure AD'de kullanıcıları için Self Servis Grup Yönetimi](active-directory-accessmanagement-self-service-group-management.md) ve [gruplarınızı yönetmesine](active-directory-manage-groups.md).




## <a name="accessing-the-access-panel"></a>Erişim paneli erişme

Bir web tarayıcısında aşağıdaki URL'yi ziyaret ederek erişim paneli erişebilirsiniz:`http://myapps.microsoft.com`

Oturum açma sayfanız için yapılandırılan özel marka varsa, bu, kuruluşunuzun etki alanı URL'si sonuna ekleyerek markalama yükleyebilirsiniz:`http://myapps.microsoft.com/<your domain>.com`

Bu durumda, Azure portalında yapılandırılmış herhangi bir etkin ya da doğrulanmış etki alanı adını kullanabilirsiniz.

![Wingtip Toys etki alanı adı][2]  

Azure AD ile tümleşik uygulamalar için oturum açacak tüm kullanıcılar için URL dağıtmanız gerekir.

## <a name="authentication"></a>Kimlik Doğrulaması

Erişim paneli ulaşmak için bir iş veya Okul hesabı ile Azure AD içinde kimliğinin doğrulanması gerekir. Doğrudan Azure AD ile doğrulanabilir. Alternatif olarak, bir kuruluş, Active Directory Federasyon Hizmetleri (AD FS) veya diğer teknolojileri kullanarak Federasyon yapılandırdıysa, Windows Server Active Directory tarafından doğrulanabilir.

Azure veya Office 365 için bir abonelik varsa ve Azure portalını veya Office 365 uygulama kullanmakta olduğunuz imzalama yeniden bileşenini olmadan uygulamaların listesini görebilirsiniz. Doğrulanmamış, hesabınız için Azure AD'de kullanıcı adı ve parola kullanarak oturum açmak isteyip istemediğiniz sorulur. Kuruluşunuzun Federasyon yapılandırdıysa, kullanıcı adı yazmaya yeterli olur.

Doğrulandığında, yöneticiniz directory ile tümleşik olan uygulamalar ile etkileşim kurabilir. Uygulamaları Azure AD ile tümleştirmek öğrenmek için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Web tarayıcısı gereksinimleri

En azından, CSS etkin olduğundan ve erişim paneli JavaScript destekleyen bir tarayıcı gerektirir. Kullanıcı parola tabanlı çoklu oturum açma (SSO) aracılığıyla uygulamaları için oturum açmanız erişim paneli uzantısı tarayıcınızda yüklü olması gerekir. Uzantı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinizde otomatik olarak yüklenir.

Erişim paneli uzantısı için şu anda kullanılabilir:
-   Edge Windows 10 Anniversary Edition veya daha yenisi 

-   Chrome--Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde

-   Firefox 26,0 veya daha sonra--Windows XP SP2 veya sonraki ve Mac OS X 10,6 veya üzeri

-   Internet Explorer 8, 9, 10, 11--Windows 7 veya üzeri (sınırlı destek)

## <a name="my-apps-secure-sign-in-extension"></a>My Apps Güvenli Oturum Açma Uzantısı
Uzantı, kullanıcıların parola tabanlı çoklu oturum açma imzalamak gereklidir. Yüklü kullanıcılar ayrıca ek özellikler tıklayarak uzantısına imzalayarak etkinleştirebilir sonra **başlamak oturum**. 

- Kullanıcılar oturum açabilir uygulamalarda doğrudan uygulamanın ziyaret ederek **oturum açma URL'si**. Kullanıcı oturum açma uygulamanızın URL'sine gittiğinde uzantısı bu algılar ve kullanıcının içine uzantı imzalamak seçeneği sağlar.
- Kullanıcılar de başlatma herhangi bir erişim paneli kullanarak uygulamalarını **hızlı arama** uzantısı özelliğidir. 
- Uzantı de başlatıldıktan altında son üç uygulamaları kullanıcılara gösterilir **kısa süre önce kullanılan** bölümü.
> [!NOTE]
> Ek özellikler, yalnızca kenar, Chrome, Firefox için kullanılabilir.


Https://myapps.microsoft.com daha farklı bir My uygulamaları URL kullanıyorsanız, sonra varsayılan URL ancak aşağıdaki adımları yapılandırmanız gerekir:
1. Uzantısına, imzalanmamış sırada **sağ tıklatın** uzantısı simgesi.
2. Tıklayın **My uygulamaları URL Seç** menüsünde.
3. **Seçin** , varsayılan URL.
4. Uzantı simgesine tıklayın.
5. Oturum açma seçerek uzantısına **başlamak oturum**.

## <a name="mobile-app-support"></a>Mobil uygulama desteği

Azure Active Directory ekibi yayımlar uygulamaları mobil uygulamamı. Uygulamasını yüklediğinizde, parola tabanlı SSO uygulamaları iOS ve Android cihazlarda oturum açabilir.

> [!NOTE]
> (Salesforce, Google Apps, Dropbox, kutusunda, Concur, iş günü, Office 365 ve diğerleri 70'den fazla dahil) Azure AD ile Federasyon destekleyen uygulamalar için herhangi bir cihazda herhangi bir web tarayıcısında bir eklenti veya mobil uygulama gerek kalmadan oturum açabilir. Diğer tüm [erişim paneli deneyimleri](https://myapps.microsoft.com/) de gerektirmez uygulamaları mobil uygulamama bir mobil cihazda kullanılacak.
>
>

### <a name="my-apps-for-android"></a>Uygulamalarım Android için

Uygulamalarım Android için desteklenen herhangi bir cihazda Android sürümü 4.1 çalıştıran Android ve daha sonra.  
İçinde kullanılabilir [Google Play mağazasına](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Uygulamalarım Android için][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Uygulamalarım iPhone ve iPad için

Uygulamalarım iOS için herhangi bir iPhone veya iOS sürüm 7 ve üzeri çalıştıran iPad için desteklenir.  
İçinde kullanılabilir [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Uygulamalarım iOS için][4]    



## <a name="managed-browser-for-my-apps"></a>Uygulamalarım için yönetilen tarayıcı

Uygulamalarım Intune yönetilen tarayıcı'da da tümleşiktir. İOS ve Android cihazlar için Intune Managed Browser mobil cihazlarda veri güvenli kalmasını sağlamaya önemli bir rol oynar. Güvenli bir şekilde görüntülemenize ve şirket bilgileri içerebilen ve güvenli bir web tarama deneyimi sağlayan bir web sayfalarında gezinmek izin verir.  
Erişmek istediğiniz herhangi bir uygulama ulaşması daha az tıklama vermiş uygulamalarım hızlı erişim Managed Browser giriş sayfanız ve işaretlerinizi, bulur.

İçinde kullanılabilir [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) ve [Google Play mağazası](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Uygulamalarım Mananged tarayıcısı][5]    





## <a name="tips-for-testing-the-user-experience"></a>Kullanıcı deneyimini test etmek için ipuçları

Azure yönetici olduğunuz ve Azure portalına dizinde bir hesap kullanarak oturum, otomatik olarak erişim paneline geçerli hesabınıza açtınız. Bu durumda, size atanmış olan tüm uygulamaları görebilirsiniz.

**Olarak test etmek için bir *farklı* kullanıcı hesabı:**

1. Azure portal ya da erişim panelinde sağ üst köşesindeki kullanıcı menüsünü tıklatın ve ardından **oturum kapatma**. 
2. Git [erişim paneli](http://myapps.microsoft.com).
3. Oturum açma sayfasında, dizininizde test etmek istediğiniz kullanıcı adını ve parolasını yazın.


## <a name="starting-applications"></a>Uygulamaları başlatma

Çeşitli uygulamalar üzerinde erişim paneli görünebilir.

### <a name="office-365-applications"></a>Office 365 uygulamaları

Kuruluşunuz Office 365 uygulamaları kullanıyorsanız ve bunlar için lisanslanır Office 365 uygulamaları, erişim panelinde görüntülenir.

Bir Office 365 uygulama için bir uygulama kutucuğuna tıkladığınızda, uygulamaya yönlendirilir ve otomatik olarak imzalanmış.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Federasyon tabanlı SSO ile yapılandırılan Microsoft ve üçüncü taraf uygulamalar

Yöneticiniz, kümesine SSO moduyla, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **Azure AD çoklu oturum açma**. Yöneticiniz açıkça uygulamalara erişim izni varsa, yalnızca bu uygulamaları görebilirsiniz.

Bu uygulamalardan birini için bir kutucuğa tıkladığınızda, yeniden yönlendirilir ve uygulamaya otomatik olarak açık.

### <a name="password-based-sso-without-identity-provisioning"></a>Kimlik sağlama olmadan SSO parola tabanlı

Yöneticiniz, kümesine SSO moduyla, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **parola tabanlı çoklu oturum açma**. Dizindeki tüm kullanıcılar bu modda yapılandırılmış tüm uygulamaları görebilirsiniz.

İlk kez, bir bölme Bu uygulamalardan birini için sizden istenir eklenti parola SSO Internet Explorer veya Chrome yüklemek için tıklatın. Yükleme web tarayıcınızı yeniden başlatmanızı gerektirebilir. Erişim paneline dönün ve yeniden uygulama kutucuğa tıklayın, bir kullanıcı adı ve parola uygulama için bir uyarı alırsınız. Kullanıcı adınızı ve parolanızı girdikten sonra bu kimlik bilgileri güvenli bir şekilde depolanır ve Azure AD içinde hesabınıza bağlanır.

Uygulama kutucuğuna tıklayın açtığınızda, otomatik olarak uygulamaya açtınız.  
Kimlik bilgilerinizi yeniden girin ve veya parola SSO eklentisini yüklemek sahip değilsiniz.

Kimlik bilgilerinizi hedef üçüncü taraf uygulama değiştirdiyseniz, Azure AD'de depolanan kimlik bilgilerinizi güncelleştirmeniz gerekir. 

**Kimlik bilgilerini güncelleştirmek için:**

1. Uygulama kutucuğuna simgesini seçin.
2. Seçin **güncelleştirme kimlik bilgileri** kullanıcı adı ve uygulama parolasını tekrar girmelisiniz.


### <a name="password-based-sso-with-identity-provisioning"></a>Kimlik sağlama ile parola tabanlı SSO

Yöneticiniz uygulamalarda ekleyebilirsiniz **Active Directory** kümesine SSO modu Azure portalıyla bölümünü **parola tabanlı çoklu oturum açma**, kimlik sağlama yanı sıra.

İlk kez Bu uygulamalardan birini için bir uygulama kutucuğuna tıklayın, yüklemeniz istenir **parola SSO Internet Explorer veya Chrome için eklenti**. Yükleme web tarayıcınızı yeniden başlatmanızı gerektirebilir.  
Ne zaman erişim Masası'na dönün ve uygulamaya otomatik olarak imzalanmış uygulama döşeme tekrar tıklatın.

Bazı uygulamalar, ilk oturum açma parolanızı değiştirmenizi gerektirebilir. Kimlik bilgilerinizi hedef üçüncü taraf uygulama değiştirdiyseniz, Azure AD'de depolanan kimlik bilgilerini güncelleştirmeniz gerekir. 

**Kimlik bilgilerini güncelleştirmek için:**

1. Uygulama kutucuğuna simgesini seçin.
2. Seçin **güncelleştirme kimlik bilgileri** kullanıcı adı ve uygulama parolasını tekrar girmelisiniz.


### <a name="application-with-existing-sso-solutions"></a>Varolan SSO çözümleriyle uygulama

Bir uygulama için SSO yapılandırmak için Azure portalı adı verilen üçüncü bir seçenek sağlar **varolan çoklu oturum açma**. Bu seçenek, bir uygulamaya bir bağlantı oluşturun ve onu Seçili kullanıcılar için erişim paneli yerleştirmek için yöneticinize sağlar.

Örneğin, bir uygulama, AD FS 2.0 kullanarak kullanıcıların kimlik doğrulaması için yapılandırılmışsa, yöneticiniz kullanabilirsiniz **varolan çoklu oturum açma** erişim paneli üzerinde bir bağlantı oluşturmak için seçeneği. Bağlantıyı eriştiğinde uygulamanın sağladığı AD FS 2.0 veya ne olursa olsun varolan SSO çözüm kimlik doğrulaması yapılır.


## <a name="next-steps"></a>Sonraki adımlar

- Uygulama yönetimiyle ilgili tüm konuları listesini görmek için bkz: [Azure Active Directory'de uygulama yönetimi için makale dizini](active-directory-apps-index.md).
 
- Bir SaaS uygulaması Azure AD ile tümleştirmek öğrenmek için bkz: [ilgili SaaS uygulamalarını tümleştirme ile öğreticiler listesi](active-directory-saas-tutorial-list.md).
 
- Azure AD ile uygulamaları yönetme hakkında daha fazla bilgi için bkz: [tek oturum açma ve yönetme uygulama erişimi Azure Active Directory ile giriş](active-directory-appssoaccess-whatis.md).
 
- Kullanıcı hazırlama hakkında daha fazla bilgi için bkz: [kullanıcı sağlama ve SaaS uygulamaları etkinleştirmektir otomatikleştirmek](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
