---
title: Azure Active Directory erişim panelinde nedir? | Microsoft Docs
description: (Web tarayıcısı, Android uygulaması, iPhone ve iPad uygulaması) SaaS uygulamalarına erişmek için erişim paneli çeşitleri kullanmayı öğrenin.
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
ms.date: 05/11/18
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eff93b37be2ff770b90518f886bd4b54fa0ca2a1
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868999"
---
# <a name="what-is-the-access-panel"></a>Erişim paneli nedir?

Erişim paneli web tabanlı bir portaldır. Bir iş veya Okul hesabı Azure Active Directory'de (Azure AD), görüntülemek ve bir Azure AD Yöneticisi erişim izni vermiştir, bulut tabanlı uygulamalarda başlatmak için erişim paneli kullanabilirsiniz. Self Servis grup ve uygulama erişim panelinde üzerinden yönetim özellikleri de kullanabilirsiniz.

Erişim paneli, Azure Portalı'ndan ayrıdır. Azure aboneliğinin olmasını gerektirmez.

![Erişim paneli][1] erişim paneli kullanarak bazı profili ayarlarınızı düzenleyin ve aşağıdakileri yapın:

- Bir iş veya Okul hesabı ile ilişkili parolayı değiştirin.

- Parola sıfırlama ayarlarını düzenleyin.

- Çok faktörlü kimlik doğrulamasını (yönetici tarafından kullanmak için gerekli olan hesapları için) ilgili kişi ve tercih ayarlarını düzenleyin.

- E-posta, mobil ve office telefon numaraları ve cihazlar, kullanıcı kimliği gibi ayrıntıları alternatif hesabı görüntüleyin.

- Görüntüleyebilir ve bulut tabanlı uygulamalar, Azure AD Yöneticisi için erişim izni başlar. 

- Kendi kendine gruplarını yönetin. Yöneticiler oluşturabilir ve güvenlik gruplarını yönetme ve Azure AD'de güvenlik grup üyelikleri isteme. Daha fazla bilgi için [Self Servis Grup Yönetimi kullanıcıların Azure AD'de](users-groups-roles/groups-self-service-management.md) ve [gruplarınızı yönetmek](fundamentals/active-directory-manage-groups.md).




## <a name="access-the-access-panel"></a>Erişim paneline erişmek

Erişim paneli giderek erişebileceğiniz `http://myapps.microsoft.com`.

Oturum açma sayfanız için yapılandırılmış özel marka öğelerini varsa, kuruluşunuzun etki alanı URL'si ekleyerek markalama yükleyebilirsiniz (örneğin, `http://myapps.microsoft.com/<your domain>.com`).

Burada gösterildiği gibi Azure portalında yapılandırılmış herhangi bir etkin veya doğrulanmış etki alanı adı kullanabilirsiniz: ![Wingtip Toys etki alanı adı][2]  

URL ile Azure AD tümleşik uygulamaları için oturum açın, tüm kullanıcılara dağıtın.

## <a name="authentication"></a>Kimlik Doğrulaması

Erişim Paneli'ne ulaşmak için Azure AD'de bir iş veya Okul hesabı kimliğinin doğrulanması gerekir. Doğrudan Azure AD'ye kimlik doğrulaması. Alternatif olarak, bir kuruluş, Active Directory Federasyon Hizmetleri (AD FS) veya diğer teknolojileri kullanarak Federasyon yapılandırdıysa, Windows Server Active Directory tarafından doğrulanabilir.

Azure veya Office 365 aboneliğinizin olması ve Azure portalı veya Office 365 uygulama kullanmakta olduğunuz, yeniden imzalama olmadan uygulamaların listesini görüntüleyebilirsiniz. Doğrulanmaz hesabınız için Azure AD'de kullanıcı adı ve parola kullanarak oturum açmanız istenir. Kuruluşunuzun Federasyon yapılandırdıysa, kullanıcı adını yazarak yeterli olur.

Kimlik doğrulaması yaptıktan sonra yöneticinize directory ile tümleşik olan uygulamalar ile etkileşim kurabilirsiniz. Uygulamalar Azure AD ile tümleştirme konusunda bilgi almak için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="web-browser-requirements"></a>Web tarayıcısı gereksinimleri

En azından, CSS etkin olduğundan ve erişim paneli JavaScript destekleyen bir tarayıcı gerektirir. Parola tabanlı çoklu oturum açma (SSO) ile uygulamalar için oturum açmanız erişim paneli uzantısını tarayıcınızda yüklü olmalıdır. Uzantı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinizde otomatik olarak indirilir.

Yükleyici mimarisi özeldir. İndirme bağlantısına tıklarsanız, yalnızca yükleyici, şu anda çalışan işletim sistemi mimarisine sahip olursunuz. Uygulama Dağıtım Yöneticisi, her iki yükleyici almak için bir 64 bit ve 32 bit CİHAZDAN indirme bağlantısı ziyaret emin olun.


Erişim paneli uzantısını şu anda kullanılabilir:
- **Edge**: Windows 10 Anniversary Edition veya sonrası. 
- **Chrome**: Windows 7 ve daha sonra ve MacOS x veya sonrası.
- **26,0 veya üzeri Firefox**: Windows XP SP2 veya sonraki sürümlerde ve Mac OS X 10.6 veya daha sonra.
- **Internet Explorer 11**: Windows 7 veya üzeri (sınırlı destek).

## <a name="my-apps-secure-sign-in-extension"></a>My Apps Güvenli Oturum Açma Uzantısı
Parola tabanlı çoklu oturum açma için oturum açmak için uzantı kullanmanız gerekir. Uzantıyı yükledikten sonra ona ek özellikleri seçerek etkinleştirmek üzere oturum açabilmesi **kullanmaya başlamak oturum**. 

- Uygulamanın doğrudan kullanarak bir uygulamaya oturum açabilir **oturum açma URL'si**. Uygulamanın URL'si kullandığınızda, uzantı işlemleri algılar ve uzantı oturum açma seçeneği sunar.
- Erişim paneli uygulamalarınızdan birini kullanarak başlatabilirsiniz *hızlı arama* uzantı özelliğidir. 
- Uzantı, başlatılan son üç uygulamalar gösterilmektedir **kısa süre önce kullanılan** bölümü.
- Şirket içi URL'ler uzaktan çalışırken kullanabileceğiniz [uygulama proxy'si](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

> [!NOTE]
> Ek özellikler, yalnızca Edge, Chrome ve Firefox için kullanılabilir.
>
Uzantı aşağıdaki sitelerden doğrudan indirebilirsiniz:
- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

My Apps URL dışında kullanıyorsanız `https://myapps.microsoft.com`, aşağıdakileri yaparak, varsayılan URL yapılandırın:
1. Siz *değil* uzantı açtığınız uzantı simgesine sağ tıklayın.
2. Menüsünde **My Apps URL**.
3. Varsayılan URL'yi seçin.
4. Genişletme simgesini seçin.
5. Seçin **kullanmaya başlamak oturum**.

Şirket içi URL'leri uzantıyı kullanarak uzaktan erişim sırasında kullanmak için aşağıdakileri yapın:
1. [Uygulama Ara sunucusu yapılandırma](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable) kiracınıza.
2. [Uygulama yayımlama](https://docs.microsoft.com/en-us/azure/active-directory/application-proxy-publish-azure-portal) ve uygulama proxy'si aracılığıyla uygulama URL'si.
3. Uzantıyı yüklemek ve ona oturum açma'yı seçerek kullanmaya başlamak için oturum açın.
4. Artık uzaktan çalışırken bile şirket içi URL'sine göz atabilirsiniz.

> [!NOTE]
> Ayrıca şirket URL'leri otomatik yeniden yönlendirme ana menüsündeki ayarlar dişli simgesini seçerek ve devre dışı **kapalı** şirket İç URL yeniden yönlendirme seçeneği için.


## <a name="mobile-app-support"></a>Mobil uygulama desteği

Azure Active Directory ekibi uygulamalarım mobil uygulamanın yayınlar. Uygulamayı yüklediğinizde, parola tabanlı SSO uygulamaları iOS ve Android cihazlarda oturum açabilir.

> [!NOTE]
> (Salesforce, Google Apps, Dropbox, kutusu, Concur, Workday, Office 365 ve 70'ten fazla diğerleri dahil) Azure AD ile Federasyon destekleyen uygulamalar için herhangi bir cihazda herhangi bir web tarayıcısı üzerinde bir eklenti veya mobil uygulama gerek olmadan oturum açabilir. Mobil cihazda, diğer kullanılacak [erişim paneli deneyimleri](https://myapps.microsoft.com/) uygulamalarım mobil uygulamaya da gerektirmez.
>
>

### <a name="my-apps-for-android"></a>Android için uygulamalarım

Android için uygulamalarım Android sürüm 4.1 veya üstünü çalıştıran tüm Android cihazlarda desteklenmektedir.  

Adresten edinilebilir [Google Play Store'da](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Android için uygulamalarım][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>İPhone ve iPad için uygulamalarım

Herhangi bir iPhone veya iPad iOS sürüm 7 veya sonraki sürümlerini çalıştıran uygulamalarım iOS için desteklenir.  

Adresten edinilebilir [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![İOS için uygulamalarım][4]    


## <a name="managed-browser-for-my-apps"></a>Uygulamalarım için yönetilen tarayıcı

Uygulamalarım ayrıca Intune Managed Browser ile tümleşiktir. İOS ve Android cihazları için Intune Managed Browser, mobil cihazlarda verilerin güvenli kalmasını sağlamaya yardımcı olacak önemli bir rol oynar. Tarayıcı, güvenli bir şekilde şirket bilgileri içerebilen Web sayfalarını gitmek ve görüntülemek sağlar ve onu güvenli bir web tarama deneyimi sağlamaya yardımcı olur.  

Daha az tıklamayla erişmek istediğiniz herhangi bir uygulama erişmek için gerekli olacak şekilde, Managed Browser giriş sayfasında ve, yer işaretleri uygulamalarım hızlı erişim sağlayın.

Intune Managed Browser kullanılabilir [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) ve [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Uygulamalarım için yönetilen tarayıcı][5]    


## <a name="tips-for-testing-the-user-experience"></a>Kullanıcı deneyimini test etmeye yönelik ipuçları

Azure Yöneticisi olduğunuz ve Azure portalında dizindeki bir hesapla oturum, otomatik olarak erişim paneline yönlendiren geçerli hesap oturum açtınız. Bu görünüm size atanmış olan tüm uygulamaları görüntüler.

Test bir *farklı* kullanıcı hesabı, aşağıdakileri yapın:

1. Sağ üst köşesinde Azure portalı veya erişim panelinde, seçin **oturum kapatma**. 
2. Git [erişim paneli](http://myapps.microsoft.com).
3. Oturum açma sayfasında, dizininizde test etmek istediğiniz kullanıcı adını ve parolasını yazın.


## <a name="starting-applications"></a>Uygulamaları başlatma

Bu bölümde, erişim panelinde çeşitli görünebilir uygulamaları açıklanmaktadır.

### <a name="office-365-applications"></a>Office 365 uygulamaları

Kuruluşunuz Office 365 uygulamaları kullanıyor ve bunlar için lisanslı mı, Office 365 uygulamaları, erişim panelinde görüntülenir.

Bir Office 365 uygulama için bir uygulama kutucuğu seçtiğinizde, uygulamaya yeniden yönlendirilen ve otomatik olarak imzalanmış.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Federasyon tabanlı SSO ile yapılandırılan, Microsoft ve üçüncü taraf uygulamalar

Yöneticiniz, SSO modu ayarlamak, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **Azure AD çoklu oturum açma**. Yalnızca onlara yönelik erişimi açıkça yöneticinize vermiş, bu uygulamaları görebilirsiniz.

Bir uygulama için bir kutucuğu seçtiğinizde, yeniden yönlendirilen ve kendisine oturumu otomatik olarak.

### <a name="password-based-sso-without-identity-provisioning"></a>Kimlik sağlama parola tabanlı SSO

Yöneticiniz, SSO modu ayarlamak, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **parola tabanlı çoklu oturum açma**. Dizindeki tüm kullanıcılar bu modda yapılandırmış olduğunuz tüm uygulamaları görebilirsiniz.

İlk kez bir uygulama kutucuğu seçin için Internet Explorer veya Chrome eklentisi parola SSO yüklemeniz istenir. Yükleme, web tarayıcınızı yeniden başlatmanızı gerektirebilir. Erişim paneline dönün ve uygulama kutucuğu yeniden seçin, bir kullanıcı adı ve uygulama için parola istenir. Kullanıcı kimliğiniz ve parolanızı girdikten sonra kimlik bilgilerini güvenli bir şekilde depolanır ve Azure AD'de hesabınıza bağlı.

Uygulama kutucuğunu seçin sonraki açışınızda, otomatik olarak uygulamaya açtınız.  

Kimlik bilgilerinizi yeniden girin ve parola SSO eklentisini yüklemek sahip değilsiniz.

Hedef üçüncü taraf uygulamada kimlik bilgilerinizi değiştirilirse, ayrıca Azure AD'de depolanan kimlik bilgilerinizi güncelleştirmeniz gerekir. 

Kimlik bilgilerinizi güncelleştirmek için aşağıdakileri yapın:

1. Uygulama kutucuğuna simgesini seçin.
2. Seçin **kimlik bilgilerini güncelleştirme** kullanıcı adı ve uygulama parolası girmek.


### <a name="password-based-sso-with-identity-provisioning"></a>Parola tabanlı SSO ile kimlik sağlama

Yöneticiniz, SSO modu ayarlamak, Azure portal'ın Active Directory bölümünde uygulamaları ekleyebilirsiniz **parola tabanlı çoklu oturum açma**, birlikte kimlik sağlama.

İlk kez bir uygulama kutucuğu seçin için Internet Explorer veya Chrome eklentisi parola SSO yüklemeniz istenir. Yükleme, web tarayıcınızı yeniden başlatmanızı gerektirebilir.  

Ne zaman erişim paneline dönün ve uygulamaya otomatik olarak oturum uygulama kutucuğu yeniden seçin.

Bazı uygulamalar, ilk oturum açma işlemi sırasında parolanızı değiştirmenizi gerektirebilir. Hedef üçüncü taraf uygulamada kimlik bilgilerinizi değiştirilirse, ayrıca Azure AD'de depolanan kimlik bilgilerini güncelleştirmeniz gerekir. 

Kimlik bilgilerinizi güncelleştirmek için aşağıdakileri yapın:

1. Uygulama kutucuğuna simgesini seçin.
2. Seçin **kimlik bilgilerini güncelleştirme** kullanıcı adı ve uygulama parolası girmek.


### <a name="application-with-existing-sso-solutions"></a>Mevcut SSO çözümleri ile uygulama

Bir uygulama için SSO yapılandırmak için Azure portalında mevcut çoklu oturum açma adı verilen üçüncü bir seçenek sağlar. Bu seçenek, bir uygulamaya bir bağlantı oluşturun ve seçili kullanıcıların erişim paneli yerleştirmek için yöneticinize sağlar.

Uygulamanın AD FS 2.0 kullanarak kullanıcıların kimlik doğrulaması için yapılandırılmışsa, örneğin, yöneticiniz varolan çoklu oturum açma seçeneği erişim panelinde bir bağlantı oluşturmak için kullanabilirsiniz. Bağlantıyı eriştiğinde uygulamanın sağladığı AD FS 2.0 veya hangi mevcut SSO çözüm kimlik doğrulaması yapılır.


## <a name="next-steps"></a>Sonraki adımlar

- Uygulama yönetimiyle ilgili tüm konularda listesini görüntülemek için bkz: [Azure Active Directory'de uygulama yönetimi için makale dizini](active-directory-apps-index.md).
 
- Bir SaaS uygulaması, Azure AD ile tümleştirme konusunda bilgi almak için bkz: [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](saas-apps/tutorial-list.md).
 
- Azure AD ile uygulamaları yönetme hakkında daha fazla bilgi için bkz: [Azure Active Directory ile çoklu oturum açma ve yönetme uygulaması erişimi için giriş](manage-apps/what-is-single-sign-on.md).
 
- Kullanıcı hazırlama hakkında daha fazla bilgi için bkz: [kullanıcı sağlamayı ve SaaS uygulamaları için sağlama kaldırmayı otomatikleştirme](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
