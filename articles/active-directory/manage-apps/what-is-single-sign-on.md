---
title: Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? | Microsoft Docs
description: Tüm işletmeler için gereksinim duyduğunuz SaaS ve web uygulamaları için çoklu oturum açmayı etkinleştirmek için Azure Active Directory kullanın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.workload: identity
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: it-pro
ms.openlocfilehash: 4ad1416f79b8cf9c03904da5f9efc1d1aae475d9
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364039"
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?
Çoklu oturum açma tüm uygulamalar ve yalnızca tek bir kullanıcı hesabı kullanarak bir kez oturum açarak iş yapmanız gereken kaynaklara erişmeye çalıştığında anlamına gelir. Oturum açtıktan sonra tüm gereken kimlik doğrulaması için gerekli olmadan uygulamaları erişebilirsiniz (örneğin, bir parola yazmak) ikinci kez.

Pek çok kuruluş yazılım üzerinde son kullanıcı üretkenliğini için Office 365, Box ve Salesforce gibi bir hizmet (SaaS) uygulamaları olarak kullanır. Tarihsel olarak, BT personeliniz ayrı ayrı oluşturup her SaaS uygulamasında kullanıcı hesaplarını güncelleştirmek gereken ve kullanıcıların her bir SaaS uygulaması için bir parola hatırlamaları gereken.

Azure Active Directory şirket içi Active Directory Kullanıcıları yalnızca kendi etki alanına katılmış cihazlarda oturum açın ve şirket kaynaklarına birincil kuruluş hesabını kullanmak etkinleştirme buluta genişletir, ancak aynı zamanda tüm SaaS uygulamalarına ve web için gerekli kendi iş.

Bu nedenle yalnızca kullanıcılar birden fazla kullanıcı adları yönetmek zorunda değilsiniz ve kuruluş Grup üyeleri ve durumlarını çalışan olarak parolalar, uygulamalarına erişim otomatik olarak sağlanan ya da sağlanan dayanır. Azure Active Directory SaaS uygulamaları arasında kullanıcıların erişimini merkezi olarak yönetmenizi sağlayan güvenlik ve erişim yönetimi denetimler sunar.

Azure AD, günümüzün popüler SaaS uygulamalarının çoğu için kolay tümleştirme sağlar; kimlik ve erişim yönetimi sağlar ve kullanıcıların doğrudan, çoklu oturum açma uygulamalarına veya keşfedin ve bunları Office 365 veya Azure AD erişim paneli gibi bir portaldan başlatma imkan tanır.

Tümleştirme mimarisi aşağıdaki dört temel yapı taşları oluşur:

* Çoklu oturum açma kullanıcıların Azure AD'de kuruluş hesabını temel SaaS uygulamalarına erişmelerini sağlar. Çoklu oturum açma hangi kullanıcıların kendi tek bir kurumsal hesap kullanarak bir uygulamaya kimlik doğrulaması sağlayan'i.
* Kullanıcı sağlama, kullanıcı hazırlama ve hedef Windows Server Active Directory ve/veya Azure AD içinde yapılan değişiklikler temel SaaS uygulamasına sağlamayı sağlar. Sağlanan hesap ne çoklu oturum açmayı doğrulandıktan sonra uygulama kullanmak üzere yetki verilmesini sağlar.
* Merkezi bir uygulamaya erişim yönetimi tek nokta SaaS uygulama erişim Azure portalı sağlar ve yönetimi, uygulama erişim karar alma ve onayları kuruluşunuzdaki herkes için temsilci seçme olanağı
* Birleşik raporlama ve Azure AD'de kullanıcı etkinliğini izleme

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Çoklu oturum açma özelliği, Azure Active Directory ile nasıl kullanılır?
Uygulamaya kullanıcılar oturum açtığında, kimin söyledikleri olduklarını kanıtlamak için gerekli olduğu bir kimlik doğrulama işleminden geçin. Çoklu oturum açma olmadan bu kimlik doğrulama işlemi, genellikle uygulamayı depolanmış bir parola girerek gerçekleştirilir ve kullanıcıların bu parolayı bilmeniz gerekir.

Azure AD uygulamaları için oturum açmak için üç farklı yolla destekler:

* **Federasyon çoklu oturum açma** uygulamaların yerine için kendi parola istemi kullanıcı kimlik doğrulaması için Azure ad yeniden yönlendirme sağlar. Federasyon çoklu oturum açma desteği gibi SAML 2.0, WS-Federation ve Openıd Connect protokolleri ve fikirlerini modu, çoklu oturum açma uygulamaları için desteklenir.
* **Parola tabanlı çoklu oturum açma** güvenli uygulama parola depolama ve bir web tarayıcısı uzantısı veya mobil uygulama kullanarak etkinleştirir. Parola tabanlı çoklu oturum açma uygulama tarafından sağlanan mevcut işlem kullanıyor ancak yönetici parolaları yönetmek etkinleştirir ve kullanıcının parolasını bilmesini gerektirmez.
* **Varolan çoklu oturum açma** herhangi var olan tek bir uygulama için ayarlanmadı, ancak bu uygulamaların Office 365 veya Azure AD erişim paneli portallarında bağlanmasını sağlar oturum yararlanmak Azure AD tarafından doğrulanmasını sağlar ve ek sağlar Raporlama, Azure AD'de uygulamaları var. ne zaman başlatılır.

Bir kullanıcı bir uygulama ile doğrulandıktan sonra bunlar Ayrıca uygulama izinleri ve erişim düzeyini içindeki uygulama nerede bildiren uygulama düzeyinde sağlanan hesap kaydı olması gerekir. Bu hesap kaydını sağlama ya da otomatik olarak gerçekleşebileceği veya kullanıcı çoklu oturum açma erişimi sağlanan önce el ile bir yönetici tarafından ortaya çıkabilir.

 Bu tek oturum açma modları ve aşağıda sağlama hakkında daha fazla bilgi.

### <a name="federated-single-sign-on"></a>Federasyon çoklu oturum açma
Federasyon çoklu oturum açma, kuruluşunuzdaki kullanıcılar Azure ad kullanıcı hesabı bilgilerini kullanarak Azure AD tarafından üçüncü taraf SaaS uygulaması için otomatik olarak oturum açmanız sağlar.

Bu senaryoda, Azure AD ile önceden kaydedilmiş ve bir üçüncü taraf SaaS uygulaması tarafından denetlenen kaynaklarına erişmek istediğinizde Federasyon kullanıcının kimliğinin yeniden doğrulanması ortadan kaldırır.

Azure AD Federasyon çoklu oturum açma SAML 2.0, WS-Federation destekleyen uygulamalarla destekleyebilir veya Openıd connect protokolleri.

Ayrıca bkz: [Federasyon çoklu oturum açma için sertifikaları yönetme](manage-certificates-for-federated-single-sign-on.md)

### <a name="password-based-single-sign-on"></a>Parola tabanlı çoklu oturum açma
Parola tabanlı çoklu oturum açmayı yapılandırma, üçüncü taraf SaaS uygulamasında kullanıcı hesap bilgileri kullanılarak Azure AD tarafından üçüncü taraf SaaS uygulaması için otomatik olarak oturum açmanız, kuruluşunuzdaki kullanıcılardan sağlar. Bu özelliği etkinleştirmek, Azure AD toplar ve kullanıcı hesabı bilgilerini ve ilgili parolayı güvenli bir şekilde depolar.

Azure AD, bir HTML tabanlı oturum açma sayfasına sahip tüm bulut tabanlı uygulama için parola tabanlı çoklu oturum açma destekleyebilir. Özel tarayıcı eklentisi kullanarak, AAD güvenli bir şekilde kullanıcı adı ve parola gibi uygulama kimlik bilgileri dizinden almak aracılığıyla oturum açma işlemini otomatikleştiren ve uygulama oturum açma sayfası kullanıcı adına bu kimlik bilgilerini girer. İki kullanım örnekleri vardır:

1. **Yönetici kimlik bilgilerini yöneten** – Yöneticiler oluşturma ve uygulama kimlik bilgilerini yönetme ve grup veya kullanıcıları uygulamaya erişmeniz için bu kimlik bilgileri atayın. Bu gibi durumlarda, son kullanıcı kimlik bilgilerini bilmeniz gerekmez, ancak yine de yalnızca kendi erişim panellerine veya sağlanan bağlantı yoluyla tıklayarak uygulama için çoklu oturum açma erişimi kazanır. Bu işlem hem de yapabildiği, uygulamaya özgü parolaları yönetme veya unutmayın gerekmez son kullanıcılar için kimlik bilgilerinin kolaylık yanı sıra yönetici tarafından yaşam döngüsü yönetimi sağlar. Kimlik bilgileri, son kullanıcıdan otomatik oturum açma işlemi sırasında gizlenmiş olan; ancak web hata ayıklama araçları'nı kullanarak kullanıcı tarafından teknik olarak bulunabilir ve alacağı kimlik bilgilerini doğrudan kullanıcı tarafından sunulan aynı güvenlik ilkelerini kullanıcılara ve yöneticilere izlemelidir. Yönetici tarafından sağlanan kimlik bilgileri, sosyal medya veya belge paylaşımı uygulamaları gibi çok sayıda kullanıcı arasında paylaşılan bir hesap erişim sağlarken yararlıdır.
2. **Kullanıcı kimlik bilgilerini yöneten** – yöneticiler uygulamaları son kullanıcılara veya gruplara atamak ve son kullanıcıların erişim panelinde ilk kez uygulamaya erişmeyi doğrudan bağlı kendi kimlik bilgilerini girmek izin. Bu kolaylık yapabildiği bunlar sürekli olarak kullanıcıların uygulamaya erişebilmesi için her zaman uygulamaya özgü parolaları girmeniz gerekmez, son kullanıcılar için oluşturur. Kullanıcıların parolalarını güncelleştirmek ya da gerektiği şekilde silerek yönetmeye devam edebilirsiniz. Bu kullanım örneği, bir atlama taşı yapabildiği yönetici uygulama için yeni kimlik bilgileri gelecekteki bir tarihte son kullanıcının uygulama erişim deneyimi değiştirmeden ayarlayabilir, kimlik bilgilerinin yönetimsel yönetim olarak da kullanılabilir.

Her iki durumda da kimlik bilgilerini şifrelenmiş bir duruma dizininde depolanır ve yalnızca HTTPS üzerinden otomatik oturum açma işlemi sırasında geçirilir. Azure AD parola tabanlı çoklu oturum açma kullanarak bir Federasyon protokollerine destekleme kapasitesine sahip olmayan uygulamalar için uygun kimlik erişim yönetimi çözümü sunar.

Parola tabanlı SSO güvenli bir şekilde Azure AD'den uygulama ve kullanıcıya özgü bilgileri almak ve hizmete uygulamak için bir tarayıcı uzantısı kullanır. Azure AD tarafından desteklenen çoğu üçüncü taraf SaaS uygulamaları, bu özelliği desteklemez.

Parola tabanlı SSO için son kullanıcının tarayıcılar olabilir:
* Internet Explorer 11 – Windows 7 veya üzeri
* Edge Windows 10 Anniversary Edition veya sonrası 
* Chrome--Üzerinde Windows 7 ve daha sonra ve MacOS x veya sonrası
* Firefox 26,0 veya daha sonra--Windows XP SP2 veya üstü ve Mac OS X 10,6 veya sonraki bir sürümü üzerinde

### <a name="existing-single-sign-on"></a>Varolan çoklu oturum açma
Bir uygulama için çoklu oturum açmayı yapılandırırken, Azure portalı, "mevcut çoklu oturum açma" üçüncü bir seçenek sağlar. Bu seçenek, yalnızca bir uygulamaya bir bağlantı oluşturun ve seçili kullanıcıların erişim panelinde yerleştirme silebilirler.

Active Directory Federasyon Hizmetleri 2.0 kullanan kullanıcıların kimliğini doğrulamak üzere yapılandırılmış bir uygulama ise, örneğin, bir yönetici "varolan çoklu oturum açma" seçeneği erişim panelinde bir bağlantı oluşturmak için kullanabilirsiniz. Kullanıcılar bağlantıyı eriştiğinde, Active Directory Federasyon Hizmetleri 2.0 veya uygulama tarafından sağlanan tüm var olan tek oturum açma çözümü kullanarak doğrulanır.

### <a name="user-provisioning"></a>Kullanıcı sağlama
Uygulama için Windows Server Active Directory veya Azure AD kimlik bilgilerinizi kullanarak otomatik kullanıcı hazırlama ve hesapların Azure Portal'ı, üçüncü taraf SaaS uygulamaları'nda sağlamayı Azure AD sağlar. Bir kullanıcı, bu uygulamalardan birini için Azure ad'deki izinleri verildiğinde, bir hesap otomatik olarak (sağlanan hedef SaaS uygulaması) oluşturulabilir.

Bir kullanıcı silindi veya Azure AD'de kendi bilgilerini değiştirir, bu değişiklikler ayrıca SaaS uygulamasında yansıtılır. Diğer bir deyişle, otomatik kimlik yaşam döngüsü yönetimini yapılandırma, yöneticilerin denetlemek ve otomatik sağlama ve sağlamayı SaaS uygulamaları sağlamak sağlar. Azure AD'de kullanıcı sağlamayı tarafından bu Otomasyon kimlik yaşam döngüsü yönetimi etkindir.

Daha fazla bilgi için bkz: [otomatik kullanıcı hazırlama ve SaaS uygulamalarına sağlama kaldırmayı](../active-directory-saas-app-provisioning.md)

## <a name="get-started-with-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi'ni kullanmaya başlama
Başlamaya hazır mısınız? Azure AD arasında çoklu oturum açmayı dağıtmayı ve kuruluşunuzun kullandığı, SaaS uygulamaları, şu yönergeleri izleyin.

### <a name="using-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi kullanma
[Azure Active Directory Uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/) Azure Active Directory ile çoklu oturum açma biçimi desteklediği bilinen uygulamaların bir listesini sağlar.

![Azure çevrimiçi uygulama Galerisi](media/what-is-single-sign-on/onlineappgallery.png)

Uygulamaları destekledikleri hangi özellikleri tarafından bulma için bazı ipuçları şunlardır:

* Azure AD otomatik hazırlama ve tüm "Öne çıkan uygulamalar" uygulamalar için sağlamayı destekler [Azure Active Directory Uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/).
* Özellikle destekleyen Federasyon uygulamalarına listesini Federasyon tek SAML, WS-Federasyon gibi bir protokol kullanarak oturum açmayı veya Openıd Connect bulunabilir [burada](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Uygulamanızı bulduktan sonra çoklu oturum açmayı etkinleştirmek için adım adım yönergeleri app Galerisi'nde hem de Azure portalında izleyerek başlayabilirsiniz.

### <a name="application-not-in-the-gallery"></a>Uygulama galerisinde?
Uygulamanızı Azure AD uygulama galerisinde bulunamazsa, bu seçenekler vardır:

* **Kullanmakta olduğunuz listelenmemiş uygulama ekleme** -kuruluşunuz kullanarak listelenmemiş bir uygulamayı bağlamak için Azure portalındaki uygulama galerisinde özel kategorisini kullanın. Bir federasyon uygulaması olarak, SAML 2.0 destekleyen herhangi bir uygulama ya da bir HTML tabanlı oturum açma sayfasında parola SSO uygulama olarak olan herhangi bir uygulama ekleyebilirsiniz. Daha fazla ayrıntı için bu makaleye bakın [kendi uygulamanızı ekleme](../application-config-sso-how-to-configure-federated-sso-non-gallery.md).
* **Geliştirdiğiniz kendi uygulamanızı ekleyin** - kendiniz uygulama geliştirdiyseniz, Federasyon çoklu oturum açmayı uygulamak için Azure AD Geliştirici belgelerindeki yönergeleri izleyin veya graph API kullanarak Azure AD sağlama. Daha fazla bilgi için şu kaynaklara bakın:
  
  * [Azure AD için Kimlik Doğrulama Senaryoları](../active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Bir uygulama tümleştirmesi istek** -ihtiyacınız kullanarak uygulama için destek isteyin [Azure AD geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Uygulamanın çoklu oturum açmayı yapılandırmak için Azure portalında Active Directory uzantısını kullanın. İlk adım, portalda Active Directory bölümünden bir dizin seçmeniz gerekir:

![](./media/what-is-single-sign-on/azuremgmtportal.png)

Üçüncü taraf SaaS uygulamalarınızı yönetmek için seçili dizin uygulamaları sekmesine geçebilirsiniz. Bu görünüm, yöneticilerin sağlar:

* Azure AD galeri, yanı, geliştirdiğiniz uygulamaları yeni uygulamalar ekleme
* Tümleşik uygulamaları silin
* Önceden tümleştirilmiş uygulamaları yönetme

Bir üçüncü taraf SaaS uygulaması için tipik yönetim görevleri şunlardır:

* Azure AD parola SSO kullanarak veya mevcut SaaS hedefi varsa, Federasyon SSO ile çoklu oturum açmayı etkinleştirme
* İsteğe bağlı olarak, kullanıcı için hazırlama ve sağlamayı (kimlik yaşam döngüsü yönetimi) kullanıcı sağlamayı etkinleştirme
* Uygulamalar için 
* Kullanıcı sağlamayı etkin olduğunda, bu uygulamaya erişim sahip kullanıcıları seçme

Yapılandırma, Federasyon çoklu oturum açmayı destekleyen galeri uygulamalar için üçüncü taraf uygulama ve Azure AD arasında federasyon güveni oluşturmak için sertifikalar ve meta verileri gibi ek yapılandırma ayarları sağlamak, genellikle gerektirir. Yapılandırma Sihirbazı'nı yönergeleri ve SaaS uygulamaya özgü verileri kolay erişim sağlar ve size ayrıntılarını gösterir.

Otomatik kullanıcı hazırlama destekleyen galeri uygulamalar için Azure AD SaaS uygulamasında hesaplarınızı yönetme izni vermenizi gerektirir. En azından, kimlik bilgilerinin Azure AD üzerinden hedef uygulama için kimlik doğrulaması yapılırken kullanması gereken sağlamanız gerekir. Ek yapılandırma ayarları sağlanması gerekip gerekmediğini, uygulama gereksinimlerine bağlıdır.

## <a name="deploying-azure-ad-integrated-applications-to-users"></a>Azure AD tümleşik uygulamalarını kullanıcılara dağıtma
Azure AD, kuruluşunuzda son kullanıcıların uygulamaları dağıtmak için çeşitli özelleştirilebilir yollar sağlar:

* Azure AD erişim paneli
* Office 365 uygulama Başlatıcısı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Kuruluşunuzda dağıtmayı tercih hangi yöntemleri kümeleri olur.

### <a name="azure-ad-access-panel"></a>Azure AD erişim paneli
Adresinden erişim Paneli'nde https://myapps.microsoft.com Azure görüntülemek için Active Directory'de son kullanıcı bir kurumsal hesap ile sağlayan web tabanlı bir portal ve Azure AD yöneticinizin erişim verilen başlatma bulut tabanlı uygulamalar, olan. Son kullanıcı ile başladıysanız [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), ayrıca erişim paneli ile Self Servis Grup Yönetimi özellikleri kullanabilir.

![Azure AD erişim paneli](media/what-is-single-sign-on/azure-ad-access-panel.png)

Erişim paneli, Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliği veya Office 365 aboneliğine sahip olmasını gerektirmez.

Azure AD erişim paneli hakkında daha fazla bilgi için bkz. [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Office 365 uygulama Başlatıcısı
Office 365 dağıtmış olan kuruluşlar için Azure AD üzerinden kullanıcılara atanan uygulamalar ayrıca Office 365 portalında görünür https://portal.office.com/myapps. Bu ikinci bir portal kullanmak zorunda kalmadan uygulamalarını başlatmak için bir kuruluştaki kullanıcılar için kolay getirir ve Office 365 kullanan kuruluşlar için önerilen uygulama başlatılırken çözüm.

![](./media/what-is-single-sign-on/officeapphub.png)

Office 365 uygulama başlatıcısında hakkında daha fazla bilgi için bkz: [uygulamanızın Office 365 uygulama başlatıcısında sahip](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-to-federated-apps"></a>Birleştirilmiş uygulamalarda doğrudan oturum açma
SAML 2.0, WS-Federation ve Openıd destekleyen en Federasyon uygulamaları, kullanıcıların uygulamayı başlatın ve ardından Azure AD kullanarak otomatik yeniden yönlendirme veya bir bağlantıya tıklayarak oturum açmak için oturum de destek bağlanın. Bu hizmet sağlayıcısı olarak bilinir-oturum açma başlatılan ve Azure AD uygulama galerisinde en Federasyon uygulamalarına destek (Ayrıntılar için Azure Portalı'nda uygulamanın çoklu oturum açma Yapılandırması Sihirbazı'ndan belgelerine bağlı bu bakın).

![](./media/what-is-single-sign-on/workdaymobile.png)

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Doğrudan bağlantılar Federasyon, parola tabanlı veya var olan uygulamalar için oturum açma
Azure AD çoklu oturum açma uygulamalar için doğrudan bağlantılar parola tabanlı çoklu oturum açma, var olan çoklu oturum açma ve herhangi bir biçimde Federasyon çoklu oturum açmayı destekleyen tek tek de destekler.

Bu bağlantılar bir kullanıcı belirli bir uygulama için Azure AD oturum açma işlemi aracılığıyla bunları Azure ad erişim paneli veya Office 365 kullanıcı başlatma gerek kalmadan gönderme özel olarak hazırlanmış URL'leri verilmiştir. Bu tek oturum açma URL'leri önceden tümleştirilmiş tüm uygulamalar, Pano sekmesi altında Azure portal'ın Active Directory bölümünde aşağıdaki ekran görüntüsünde gösterildiği gibi bulunabilir.

![](./media/what-is-single-sign-on/deeplink.png)

Bu bağlantıları kopyalanır ve seçilen uygulamanın oturum açma bağlantı sağlamak için istediğiniz yere yapıştırılan. Bu e-posta ya da kullanıcı uygulama erişimi için ayarladığınız tüm özel web tabanlı portal olabilir. Bir Azure AD doğrudan çoklu oturum açma URL'si için Twitter'ın bir örnek aşağıda verilmiştir:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Kuruluşa özgü URL'lere erişim paneli için benzer, daha fazla bu URL'yi dizininiz için etkin veya doğrulanmış etki alanlarından biri myapps.microsoft.com etki alanından sonra ekleyerek özelleştirebilirsiniz. Bu, tüm kurumsal markayı hemen oturum açma sayfasında kullanıcının ilk kullanıcı kimliği girildikten gerek yüklenir sağlar:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Yetkili bir kullanıcı bu uygulamaya özgü bağlantılardan birini tıkladığında, bunlar ilk olarak (Bunlar zaten oturum varsayılarak), Kurumsal oturum açma sayfasına bakın ve oturum açma işleminden sonra uygulamasının adresinden erişim Paneli'nde durdurmadan yönlendirilirsiniz. Kullanıcı, parola tabanlı çoklu oturum açma tarayıcı uzantısı gibi bir uygulamaya erişmek için ön koşullar eksik ardından bağlantıyı eksik uzantıyı yüklemek için girmesini ister. Bağlantı URL'si de uygulama için tek oturum açma yapılandırması değiştiğinde olursa sabit kalır.

Bu bağlantıları erişim panelinde ve Office 365 olarak aynı erişim denetimi mekanizmaları kullanın ve yalnızca bu kullanıcılar veya gruplar, Azure portalında uygulamanıza atanan kimliğini başarıyla doğrulamak mümkün olacaktır. Ancak, yetkilendirilmemiş herhangi bir kullanıcı, erişim verilmemiş ve erişim sahip oldukları mevcut uygulamaları görüntülemek için erişim paneli yüklemek için bir bağlantı verilen açıklayan bir ileti görürsünüz.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
* [Cloud discovery'yi ayarlama](/cloud-app-security/set-up-cloud-discovery)
* [Uygulamalara erişimi yönetme giriş](what-is-access-management.md)
* [Dış kimlikler Azure AD'de yönetmek için özellikleri karşılaştırma](../active-directory-b2b-compare-b2c.md)


