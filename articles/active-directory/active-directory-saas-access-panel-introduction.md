---
title: Azure Active Directory'ye erişim panelinde nedir? | Microsoft Docs
description: (Web tarayıcısı, Android uygulaması, iPhone ve iPad uygulama) SaaS uygulamalara erişmek için erişim paneli varyasyonları kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/18
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 442bfa7081865b2549c07a9436296ba2385a0b66
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="what-is-the-access-panel"></a>Erişim paneli nedir?

Erişim paneli web tabanlı bir portalıdır. Bir iş veya Okul hesabı Azure Active Directory'de (Azure AD), görüntülemek ve bir Azure AD Yöneticisi için erişim izni bulut tabanlı uygulamalar başlatmak için erişim paneli kullanabilirsiniz. Ayrıca, Self Servis grup ve erişim paneli üzerinden uygulama yönetim özellikleri de kullanabilirsiniz.

Erişim paneli Azure Portalı'ndan ayrıdır. Bir Azure aboneliğinizin olmasını gerektirmez.

![Erişim paneli][1] erişim paneli kullanarak, bazı profil ayarları düzenleyin ve aşağıdakileri yapın:

- Bir iş veya Okul hesabıyla ilişkili parolayı değiştirin.

- Parola sıfırlama ayarlarını düzenleyin.

- Çok faktörlü kimlik doğrulamasını (yönetici tarafından kullanmak için gerekli hesapları için) ilgili kişi ve tercih ayarlarını düzenleyin.

- Kullanıcı kimliği gibi ayrıntıları e-posta, mobil ve office telefon numaraları ve aygıtları alternatif hesabı görüntüleyin.

- Görüntülemek ve Azure AD Yöneticisi için erişim izni bulut tabanlı uygulamalar başlatın. 

- Kendi kendine gruplarını yönetin. Yöneticiler oluşturmak ve güvenlik gruplarını yönetme ve güvenlik grubu üyeliklerini Azure AD'de isteyin. Daha fazla bilgi için bkz: [Azure AD'de kullanıcıları için Self Servis Grup Yönetimi](active-directory-accessmanagement-self-service-group-management.md) ve [gruplarınızı yönetmesine](active-directory-manage-groups.md).




## <a name="access-the-access-panel"></a>Erişim paneline erişmek

Erişim paneli sayfasına giderek erişebilirsiniz `http://myapps.microsoft.com`.

Oturum açma sayfanız için yapılandırılan özel marka varsa, URL'sini kuruluşunuzun etki alanı ekleyerek markalama yükleyebilirsiniz (örneğin, `http://myapps.microsoft.com/<your domain>.com`).

Aşağıda gösterildiği gibi Azure portalında yapılandırılmış herhangi bir etkin ya da doğrulanmış etki alanı adını kullanabilirsiniz: ![Wingtip Toys etki alanı adı][2]  

URL Azure AD ile tümleşik uygulamalar için oturum açın, tüm kullanıcılara dağıtın.

## <a name="authentication"></a>Kimlik Doğrulaması

Erişim paneli ulaşmak için bir iş veya Okul hesabı ile Azure AD içinde kimliğinin doğrulanması gerekir. Doğrudan Azure AD ile doğrulanabilir. Alternatif olarak, bir kuruluş, Active Directory Federasyon Hizmetleri (AD FS) veya diğer teknolojileri kullanarak Federasyon yapılandırdıysa, Windows Server Active Directory tarafından doğrulanabilir.

Azure veya Office 365 için bir abonelik varsa ve Azure portalını veya Office 365 uygulama kullanmakta olduğunuz tekrar oturum açmayı olmadan uygulamaların listesini görüntüleyebilirsiniz. Kimlik doğrulaması yapılmıyor, hesabınız için Azure AD'de kullanıcı adı ve parola kullanarak oturum açmak için istenir. Kuruluşunuzun Federasyon yapılandırdıysa, kullanıcı adı yazmaya yeterli olur.

Doğrulandığında, yöneticiniz directory ile tümleşik olan uygulamalar ile etkileşim kurabilir. Uygulamaları Azure AD ile tümleştirmek öğrenmek için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Web tarayıcısı gereksinimleri

En azından, CSS etkin olduğundan ve erişim paneli JavaScript destekleyen bir tarayıcı gerektirir. Parola tabanlı çoklu oturum açma (SSO) aracılığıyla uygulamaları için oturum açmanız erişim paneli uzantı tarayıcınızda yüklü olmalıdır. Uzantı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinizde otomatik olarak yüklenir.

Yükleyici mimarisi özeldir. Bağlantıya tıklayın, yalnızca yükleyici, şu anda çalışan işletim sistemi mimarisi için alırsınız. Bir uygulama dağıtım yöneticisi, her iki yükleyicileri almak için bir 64 bit ve 32 bit aygıttan indirme bağlantısı ziyaret emin olun.


Erişim paneli uzantısı için şu anda kullanılabilir:
- **Kenar**: Windows 10 Anniversary Edition veya sonraki sürümlerde. 
- **Chrome**: Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde.
- **Firefox 26,0 veya üzeri**: Windows XP SP2 veya daha sonra ve Mac OS X 10.6 veya sonraki sürümlerde.
- **Internet Explorer 11**: Windows 7 veya üzeri (sınırlı destek).

## <a name="my-apps-secure-sign-in-extension"></a>My Apps Güvenli Oturum Açma Uzantısı
Parola tabanlı çoklu oturum açma için oturum açmak için uzantı kullanmanız gerekir. Uzantısı yüklendikten sonra kendisine ek özellikleri seçerek etkinleştirmek oturum açarak **başlamak oturum**. 

- Uygulamanın kullanarak doğrudan bir uygulamaya oturum açabilmeniz **oturum açma URL'si**. Uygulamanın URL kullandığınızda, uzantı eylem algılar ve uzantı oturum açma seçeneği sunar.
- Erişim paneli, uygulamalardan birini kullanarak başlatma *hızlı arama* uzantısı özelliğidir. 
- Uzantı içinde başlatılan son üç uygulamalar gösterilmektedir **kısa süre önce kullanılan** bölümü.
- Şirket içi URL'ler uzaktan sırasında kullanabileceğiniz [uygulama proxy'si](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

> [!NOTE]
> Ek özellikler, yalnızca kenar, Chrome ve Firefox için kullanılabilir.
>

My uygulamaları URL dışında kullanıyorsanız `https://myapps.microsoft.com`, aşağıdakileri yaparak, varsayılan URL yapılandırın:
1. Siz *değil* uzantısı açtığınız uzantısı simgesine sağ tıklayın.
2. Menüsünde seçin **My uygulamaları URL**.
3. Varsayılan URL seçin.
4. Uzantı simgesini seçin.
5. Seçin **başlamak oturum**.

## <a name="mobile-app-support"></a>Mobil uygulama desteği

Azure Active Directory ekibi My uygulamaları mobil uygulama yayımlar. Uygulamasını yüklediğinizde, parola tabanlı SSO uygulamaları iOS ve Android cihazlarda oturum açabilir.

> [!NOTE]
> (Salesforce, Google Apps, Dropbox, kutusunda, Concur, iş günü, Office 365 ve diğerleri 70'den fazla dahil) Azure AD ile Federasyon destekleyen uygulamalar için herhangi bir cihazda herhangi bir web tarayıcısında bir eklenti veya mobil uygulama gerek kalmadan oturum açabilir. Bir mobil cihazda, diğer kullanılacak [erişim paneli deneyimleri](https://myapps.microsoft.com/) da My uygulamaları mobil uygulama gerektirmez.
>
>

### <a name="my-apps-for-android"></a>Uygulamalarım Android için

Uygulamalarım Android için Android sürümü 4.1 veya üstü çalıştıran Android cihaz üzerinde desteklenir.  

Adresten edinilebilir [Google Play mağazasına](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Uygulamalarım Android için][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Uygulamalarım iPhone ve iPad için

Uygulamalarım iOS için herhangi bir iPhone veya iOS sürüm 7 veya üzeri çalıştıran iPad için desteklenir.  

Adresten edinilebilir [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Uygulamalarım iOS için][4]    


## <a name="managed-browser-for-my-apps"></a>My uygulamalar için yönetilen tarayıcı

Uygulamalarım ayrıca Intune yönetilen tarayıcı ile tümleşiktir. İOS ve Android cihazlar için Intune Managed Browser mobil cihazlarda veri güvenli kalmasını sağlamaya yardımcı olacak önemli bir rol oynar. Tarayıcı güvenli bir şekilde görüntülemenize ve şirket bilgileri içerebilen Web sayfalarını gidin sağlar ve güvenli bir web tarama deneyimi sağlamaya yardımcı olur.  

Daha az tıklama erişmek istediğiniz herhangi bir uygulama erişmek için gerekli olacak şekilde Managed Browser giriş sayfanız ve işaretlerinizi, My uygulamaları hızlı erişim sağlayın.

Intune yönetilen tarayıcı, şu adreste [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) ve [Google Play mağazası](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![My uygulamalar için yönetilen tarayıcı][5]    


## <a name="tips-for-testing-the-user-experience"></a>Kullanıcı deneyimini test etmek için ipuçları

Azure yönetici olduğunuz ve Azure portalına dizinde bir hesap kullanarak oturum, otomatik olarak erişim paneline geçerli hesabınıza açtınız. Bu görünüm, size atanan tüm uygulamaları görüntüler.

Test için bir *farklı* kullanıcı hesabı, aşağıdakileri yapın:

1. Üst sağına Azure portalından veya erişim paneli seçin **oturum kapatma**. 
2. Git [erişim paneli](http://myapps.microsoft.com).
3. Oturum açma sayfasında, dizininizde test etmek istediğiniz kullanıcı adını ve parolasını yazın.


## <a name="starting-applications"></a>Uygulamaları başlatma

Bu bölümde, çeşitli görünebilir uygulamalar üzerinde erişim paneli anlatılmaktadır.

### <a name="office-365-applications"></a>Office 365 uygulamaları

Kuruluşunuz Office 365 uygulamaları kullanıyorsanız ve bunlar için lisanslanır Office 365 uygulamaları, erişim panelinde görüntülenir.

Bir Office 365 uygulama için bir uygulama döşeme seçtiğinizde, uygulamaya yönlendirilir ve otomatik olarak imzalanmış.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Federasyon tabanlı SSO ile yapılandırılan Microsoft ve üçüncü taraf uygulamalar

Yöneticiniz, kümesine SSO moduyla, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **Azure AD çoklu oturum açma**. Yalnızca yöneticiniz açıkça, bunlara erişim izni varsa, bu uygulamaları görebilirsiniz.

Bir uygulama için bir kutucuğa seçtiğinizde, yeniden yönlendirildi ve kendisine oturumu otomatik olarak.

### <a name="password-based-sso-without-identity-provisioning"></a>Kimlik sağlama olmadan SSO parola tabanlı

Yöneticiniz, kümesine SSO moduyla, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **parola tabanlı çoklu oturum açma**. Dizindeki tüm kullanıcılar bu modda yapılandırılmış tüm uygulamaları görebilirsiniz.

Bir uygulama döşeme seçtiğiniz ilk kez Internet Explorer veya Chrome için eklenti parola SSO yüklemeniz istenir. Yükleme web tarayıcınızı yeniden başlatmanızı gerektirebilir. Erişim paneline dönün ve uygulama kutucuğu'ı yeniden seçin, bir kullanıcı adı ve parola uygulama için bir uyarı alırsınız. Kullanıcı adınızı ve parolanızı girdikten sonra kimlik bilgilerini güvenli bir şekilde depolanır ve Azure AD'de hesabınıza bağlanır.

Uygulama döşeme bir sonraki seçtiğinizde, otomatik olarak uygulamaya açtınız.  

Kimlik bilgilerinizi yeniden girin ve veya parola SSO eklentisini yüklemek sahip değilsiniz.

Kimlik bilgilerinizi hedef üçüncü taraf uygulama değiştirdiyseniz, Azure AD'de depolanan kimlik bilgilerinizi güncelleştirmeniz gerekir. 

Kimlik bilgilerinizi güncelleştirmek için aşağıdakileri yapın:

1. Uygulama kutucuğuna simgesini seçin.
2. Seçin **güncelleştirme kimlik bilgileri** kullanıcı adı ve uygulama parolasını tekrar girmelisiniz.


### <a name="password-based-sso-with-identity-provisioning"></a>Kimlik sağlama ile parola tabanlı SSO

Yöneticiniz, kümesine SSO moduyla, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **parola tabanlı çoklu oturum açma**, birlikte kimlik sağlama.

Bir uygulama döşeme seçtiğiniz ilk kez Internet Explorer veya Chrome için eklenti parola SSO yüklemeniz istenir. Yükleme web tarayıcınızı yeniden başlatmanızı gerektirebilir.  

Ne zaman erişim paneline dönün ve uygulamaya otomatik olarak imzalanmış uygulama döşeme yeniden seçin.

Bazı uygulamalar, ilk oturum açmada parolanızı değiştirmenizi gerektirebilir. Kimlik bilgilerinizi hedef üçüncü taraf uygulama değiştirdiyseniz, Azure AD'de depolanan kimlik bilgilerini güncelleştirmeniz gerekir. 

Kimlik bilgilerinizi güncelleştirmek için aşağıdakileri yapın:

1. Uygulama kutucuğuna simgesini seçin.
2. Seçin **güncelleştirme kimlik bilgileri** kullanıcı adı ve uygulama parolasını tekrar girmelisiniz.


### <a name="application-with-existing-sso-solutions"></a>Varolan SSO çözümleriyle uygulama

Bir uygulama için SSO yapılandırmak için Azure portal varolan çoklu oturum açma adı verilen üçüncü bir seçenek sağlar. Bu seçenek, bir uygulamaya bir bağlantı oluşturun ve onu Seçili kullanıcılar için erişim paneli yerleştirmek için yöneticinize sağlar.

Bir uygulama, AD FS 2.0 kullanarak kullanıcıların kimlik doğrulaması için yapılandırılmışsa, örneğin, yöneticiniz varolan çoklu oturum açma seçeneği erişim panelinde, bir bağlantı oluşturmak için kullanabilirsiniz. Bağlantıyı eriştiğinde uygulamanın sağladığı AD FS 2.0 veya ne olursa olsun varolan SSO çözüm kimlik doğrulaması yapılır.


## <a name="next-steps"></a>Sonraki adımlar

- Uygulama yönetimiyle ilgili tüm konuları listesini görüntülemek için bkz: [Azure Active Directory'de uygulama yönetimi için makale dizini](active-directory-apps-index.md).
 
- Bir SaaS uygulaması Azure AD ile tümleştirmek öğrenmek için bkz: [ilgili SaaS uygulamalarını tümleştirme ile öğreticiler listesi](active-directory-saas-tutorial-list.md).
 
- Azure AD ile uygulamaları yönetme hakkında daha fazla bilgi için bkz: [tek oturum açma ve yönetme uygulama erişimi Azure Active Directory ile giriş](active-directory-appssoaccess-whatis.md).
 
- Kullanıcı hazırlama hakkında daha fazla bilgi için bkz: [kullanıcı sağlama ve SaaS uygulamaları etkinleştirmektir otomatikleştirmek](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
