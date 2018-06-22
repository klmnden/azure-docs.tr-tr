---
title: Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir? | Microsoft Docs
description: Tüm iş için gereken SaaS ve web uygulamaları için çoklu oturum açmayı etkinleştirmek için Azure Active Directory kullanın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.workload: identity
ms.topic: article
ms.date: 06/21/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: it-pro
ms.openlocfilehash: a6f116842ce61585feda8f20e204e0751a360036
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309907"
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?
Çoklu oturum açma tüm uygulamaları ve iş, yalnızca tek bir kullanıcı hesabı kullanarak bir kez oturum açarak yapmak için gereken kaynaklar erişebildiklerinden anlamına gelir. Oturum açıldıktan sonra tüm gereken kimlik doğrulaması için gerekli olmadan uygulamaları erişebilirsiniz (örneğin, bir parola yazın) ikinci kez.

Birçok kuruluş yazılım son kullanıcının üretkenliği için Office 365, kutusunu ve Salesforce gibi bir hizmet (SaaS) uygulamaları olarak kullanır. Tarihsel olarak, BT personeliniz tek tek oluşturun ve her SaaS uygulamasının kullanıcı hesaplarında güncelleştirmesi gerekir ve kullanıcıların her SaaS uygulaması için bir parola anımsamak zorunda.

Azure Active Directory, yalnızca etki alanına katılmış cihazlarını oturum açma ve şirket kaynaklarını birincil kuruluş hesaplarıyla kullanmalarını sağlama bulutunu şirket içi Active Directory genişletir, ancak aynı zamanda tüm web ve SaaS uygulamaları için gerekli işlerini.

Bu nedenle yalnızca kullanıcılar kullanıcı adlarını birden çok kümesini yönetmek zorunda değilsiniz ve parolalar, uygulamalarına erişim otomatik olarak sağlanan XML'deki sağlanan veya kuruluş Grup üyeleri ve durumlarını çalışan olarak dayanır. Azure Active Directory SaaS uygulamaları arasında kullanıcıların erişimini merkezi olarak yönetmenizi sağlayan güvenlik ve erişim yönetimi denetimlerini tanıtır.

Azure AD bugünün popüler SaaS uygulamalarının birçoğu için kolay tümleştirme sağlar; kimlik ve erişim yönetimi sağlar ve kullanıcıların doğrudan, çoklu oturum açmayı uygulamalara veya bulmak ve bunları Office 365 veya Azure AD erişim paneli gibi portaldan başlatma olanak tanır.

Tümleştirme mimarisi aşağıdaki dört ana yapı taşlarını oluşur:

* Çoklu oturum açma kullanıcıların Azure AD'de kuruluş kendi hesaplarına dayalı SaaS uygulamalarına erişmesini sağlar. Çoklu oturum açma hangi kullanıcıların kendi tek bir kurumsal hesap kullanarak bir uygulamaya kimlik doğrulaması olanak sağlayan ' dir.
* Kullanıcı hazırlama, kullanıcı sağlama ve Windows Server Active Directory ve/veya Azure AD içinde yapılan değişiklikleri SaaS dayanarak hedef içine sağlamayı kaldırma özelliklerini sağlar. Bir sağlanan ne çoklu oturum açma doğrulandıktan sonra bir uygulamayı kullanmak için yetki verilmesi bir kullanıcı etkinleştirir hesabıdır.
* Merkezi uygulamaya erişim yönetimi SaaS tek bir nokta uygulama erişim Azure portalı sağlar ve yönetimi, uygulama erişim kararlar ve kuruluştaki bir kişiye onayları temsilci seçme olanağı
* Raporlama ve Azure AD'de kullanıcı etkinliğini izleme birleşik

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Çoklu oturum açma özelliği, Azure Active Directory ile nasıl kullanılır?
Kullanıcıların oturum açma, bir uygulamaya kimlerin kendilerine olduklarından emin kanıtlamak için burada gereken bir kimlik doğrulama işleminde size gidin. Çoklu oturum açma, olmadan bu kimlik doğrulama işlemi, genellikle uygulamayı depolanan bir parola girerek yapılır ve kullanıcıların bu parolayı bilmeniz gerekir.

Azure AD uygulamalara oturum açmak için üç farklı yolla destekler:

* **Federe çoklu oturum açma** kendi parolasını istemek yerine, kullanıcı kimlik doğrulaması için Azure AD yeniden yönlendirmek uygulamaları etkinleştirir. Federasyon çoklu oturum açma desteği gibi SAML 2.0, WS-Federasyon veya Openıd Connect, protokoller ve çoklu oturum açma richest modunun uygulamalar için desteklenir.
* **Parola tabanlı çoklu oturum açma** güvenli uygulama parola depolama ve bir web tarayıcı uzantısı veya mobil uygulama kullanarak yeniden yürütme sağlar. Parola tabanlı çoklu oturum açma uygulama tarafından sağlanan var olan oturum açma işlemi kullanır ancak parolaları yönetmek bir yönetici sağlar ve kullanıcının parolayı bilmeniz gerektirmez.
* **Varolan çoklu oturum açma** tüm mevcut çoklu oturum açma, uygulama için ayarlanmadı, ancak bu uygulamalar için Office 365 veya Azure AD erişim paneli portalları bağlanmasını sağlar özelliğini kullanabilmeniz Azure AD sağlar ve ayrıca ek sağlar ne zaman uygulamaları var. başlatılan Azure AD'de raporlama.

Bir uygulama ile bir kullanıcı kimliğini doğrulamasından sonra Ayrıca uygulama bildiren uygulamayı sağlanan bir hesap kaydı ihtiyaç duydukları burada var. izinler ve erişim düzeyi olan uygulama içinden. Bu hesap kaydını sağlama ya da otomatik olarak gerçekleşebileceği veya kullanıcı oturum açma tek erişimi sağlanan önce el ile bir yönetici tarafından ortaya çıkabilir.

 Bu tek oturum açma modları ve aşağıda sağlama hakkında daha ayrıntılı bilgi.

### <a name="federated-single-sign-on"></a>Federasyon çoklu oturum açma
Federasyon çoklu oturum açma Azure AD kullanıcı hesabı bilgilerini kullanarak Azure AD tarafından bir üçüncü taraf SaaS uygulamasına otomatik olarak oturum açmanız ve kuruluşunuzdaki kullanıcıların sağlar.

Bu senaryoda, zaten Azure AD ile oturumunuz açıldı ve bir üçüncü taraf SaaS uygulaması tarafından denetlenen kaynaklara erişmek istediğinizde Federasyon bir kullanıcının yeniden kimlik doğrulaması yapılması ortadan kaldırır.

Azure AD Federasyon çoklu oturum açma, WS-Federation, SAML 2.0 destekleyen uygulamalarla destekleyebilir veya Openıd connect protokoller.

Ayrıca bkz: [için sertifikaları yönetme federe çoklu oturum açma](manage-certificates-for-federated-single-sign-on.md)

### <a name="password-based-single-sign-on"></a>Parola tabanlı çoklu oturum açma
Parola tabanlı çoklu oturum açma yapılandırma üçüncü taraf SaaS uygulamasının kullanıcı hesabı bilgilerini kullanarak Azure AD tarafından bir üçüncü taraf SaaS uygulamasına otomatik olarak oturum açmanız ve kuruluşunuzdaki kullanıcıların sağlar. Bu özelliği etkinleştirdiğinizde, Azure AD toplar ve kullanıcı hesabı bilgilerini ve ilişkili parolayı güvenli bir şekilde depolar.

Azure AD parola tabanlı çoklu oturum açma, bir HTML tabanlı oturum açma sayfası olduğu tüm bulut tabanlı uygulamaları için destekler. Özel tarayıcı eklentisi kullanarak, AAD oturum açma işlemini güvenli bir şekilde uygulama kimlik bilgileri kullanıcı adı ve parola gibi dizinden alma yoluyla kullanıcının otomatikleştirir ve adına uygulama oturum açma sayfasında bu kimlik bilgilerini girer Kullanıcı. İki kullanım örnekleri şunlardır:

1. **Yönetici kimlik bilgilerini yöneten** – Yöneticiler oluşturmak ve uygulama kimlik bilgilerini yönetebilir ve kullanıcılara veya uygulamaya erişmek isteyen grupları bu kimlik bilgilerini atayın. Bu durumda, son kullanıcı kimlik bilgilerini bilmeniz gerekmez, ancak hala yalnızca kullanıcıların erişim panelinde veya sağlanan bağlantı üzerinden tıklayarak tek oturum açma uygulamaya erişim kazanır. Bu işlem hem de son yapabildiği unutmayın veya uygulamaya özgü parolaları yönetmek için ihtiyaç duydukları olmayan kullanıcılar için kullanışlı yanı sıra yönetici tarafından kimlik yaşam döngüsü yönetimi sağlar. Kimlik bilgileri son kullanıcıdan otomatik oturum açma işlemi sırasında gizlenmiş olan; ancak web hata ayıklama araçları'nı kullanarak kullanıcı tarafından teknik olarak bulunabilir ve kimlik bilgilerini doğrudan kullanıcı tarafından sunulan gibi kullanıcıların ve yöneticilerin aynı güvenlik ilkeleri izlemelisiniz. Yönetici tarafından sağlanan kimlik bilgileri, sosyal medya veya belge paylaşımı uygulamalar gibi çok sayıda kullanıcı arasında paylaşılan hesap erişim sağlarken faydalıdır.
2. **Kullanıcı kimlik bilgilerini yöneten** – yöneticiler uygulamaları son kullanıcılara veya gruplara atamak ve kullanıcıların erişim panelinde ilk kez uygulamaya erişmeyi doğrudan bağlı kendi kimlik bilgilerini girmek son kullanıcıların izin. Bu son yapabildiği bunlar sürekli uygulamaya özgü parolaları uygulamaya erişim her zaman girmenize gerek olmayan kullanıcılar için bir kolaylık oluşturur. Kullanıcıların parolalarını güncelleştirme ya da gerektiği şekilde silerek yönetmek devam edebilirsiniz. Bu kullanım örneği, atlama taşı yapabildiği yönetici uygulama için yeni kimlik bilgileri gelecekteki bir tarihte son kullanıcının uygulama erişim deneyimi değiştirmeden ayarlayabilir kimlik bilgilerinin yönetimsel yönetim olarak da kullanılabilir.

Kimlik bilgileri, her iki durumda da şifrelenmiş bir duruma dizininde depolanır ve HTTPS üzerinden otomatik oturum açma işlemi sırasında yalnızca geçirilir. Parola tabanlı çoklu oturum açma kullanarak, Azure AD Federasyon protokolleri destekleme kapasitesine sahip olmayan uygulamalar için uygun kimlik erişim yönetimi çözümü sunar.

Parola tabanlı SSO güvenli bir şekilde uygulama ve kullanıcıya özgü bilgileri Azure AD'den almak ve hizmete uygulamak için bir tarayıcı uzantısı kullanır. Azure AD tarafından desteklenen çoğu üçüncü taraf SaaS uygulamaları bu özelliği desteklemez.

Parola tabanlı, SSO için son kullanıcının tarayıcılar olabilir:
* Internet Explorer 11--Windows 7 veya üzeri
* Edge Windows 10 Anniversary Edition veya daha yenisi 
* Chrome--Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde
* Firefox 26,0 veya daha sonra--Windows XP SP2 veya sonraki ve Mac OS X 10,6 veya üzeri

### <a name="existing-single-sign-on"></a>Varolan çoklu oturum açma
Bir uygulama için çoklu oturum açmayı yapılandırırken, Azure portalı, "mevcut çoklu oturum açma" üçüncü bir seçenek sağlar. Bu seçenek yalnızca yöneticinin bir uygulamaya bir bağlantı oluşturun ve seçilen kullanıcı için erişim paneli Yerleştir sağlar.

Active Directory Federasyon Hizmetleri 2.0 kullanan kullanıcıların kimliklerini doğrulamak için yapılandırılmış bir uygulama varsa, örneğin, bir yönetici "var olan çoklu oturum açma" seçeneği erişim panelinde, bir bağlantı oluşturmak için kullanabilirsiniz. Kullanıcılar bağlantıyı eriştiğinde, Active Directory Federasyon Hizmetleri 2.0 veya ne olursa olsun varolan tek oturum açma çözümü uygulama tarafından sağlanan kullanılarak doğrulanır.

### <a name="user-provisioning"></a>Kullanıcı hazırlama
Select uygulamalar için Windows Server Active Directory veya Azure AD kimlik bilgilerinizi kullanarak otomatik kullanıcı sağlamayı ve Azure portalındaki üçüncü taraf SaaS uygulamalarında hesaplarının sağlamayı kaldırma özelliklerini Azure AD sağlar. Bir kullanıcı, bu uygulamalardan birini için Azure AD'de izin verildiğinde, bir hesap otomatik olarak (hedef SaaS uygulamasına sağlanan) oluşturulabilir.

Bir kullanıcı silindi veya Azure AD içinde kendi bilgilerini değiştirir, bu değişiklikler de SaaS uygulamada yansıtılır. Yani, otomatik kimlik yaşam döngüsü yönetimi yapılandırma denetlemek ve Otomatik hazırlama ve sağlamayı kaldırma özelliklerini SaaS uygulamalardan sağlamak yöneticilerin sağlar. Azure AD'de bu Otomasyon kimlik yaşam döngüsü yönetimi kullanıcı sağlamayı tarafından etkinleştirilir.

Daha fazla bilgi için bkz: [otomatik kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini SaaS uygulamaları için](../active-directory-saas-app-provisioning.md)

## <a name="get-started-with-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi'ni kullanmaya başlama
Başlamaya hazır mısınız? Çoklu oturum açma Azure AD arasında dağıtmak için ve kuruluşunuzun kullandığı, SaaS uygulamaları bu yönergeleri izleyin.

### <a name="using-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde kullanma
[Azure Active Directory Uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/) Azure Active Directory ile çoklu oturum açma biçimi desteklediği bilinen uygulamaların bir listesini sağlar.

![Azure çevrimiçi uygulama Galerisi](media/what-is-single-sign-on/onlineappgallery.png)

Burada, destekledikleri hangi özellikleri tarafından uygulamaları bulmak için bazı ipuçları verilmektedir:

* Azure AD otomatik sağlama ve tüm "Öne çıkan" uygulamalar için sağlamayı kaldırma özelliklerini destekler [Azure Active Directory Uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/).
* Özellikle destekleyen Federasyon uygulamaların bir listesini federe tekli SAML, WS-Federasyon gibi protokolünü kullanarak oturum veya Openıd Connect bulunabilir [burada](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Uygulamanızı bulduktan sonra çoklu oturum açmayı etkinleştirmek için adım adım yönergeler uygulama galerisinde ve Azure portalında izleyerek başlayabiliriz.

### <a name="application-not-in-the-gallery"></a>Uygulama galerisinde?
Uygulamanızı Azure AD uygulama galerisinde bulunmazsa, bu seçenekler vardır:

* **Kullanmakta olduğunuz listede bulunmayan bir uygulama ekleyin** -kuruluşunuz kullanarak listelenmemiş uygulamaya bağlanmak için Azure portalındaki uygulama galerisinde özel kategorisini kullanın. SAML 2.0 federe bir uygulama olarak destekleyen herhangi bir uygulama veya bir HTML tabanlı oturum açma sayfası bir parola SSO uygulama olarak olduğu herhangi bir uygulama ekleyebilirsiniz. Bu makalede daha fazla ayrıntı için bakın [kendi uygulamanızı ekleme](../application-config-sso-how-to-configure-federated-sso-non-gallery.md).
* **Geliştirme kendi uygulama Ekle** - kendiniz uygulaması geliştirdiyseniz, Federasyon çoklu oturum açmayı uygulamak için Azure AD Geliştirici belgelerindeki yönergeleri izleyin veya Azure AD kullanarak sağlama graph API. Daha fazla bilgi için şu kaynaklara bakın:
  
  * [Azure AD için Kimlik Doğrulama Senaryoları](../active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Bir uygulama tümleştirmesi isteği** -istek gereksinim kullanarak uygulama desteği [Azure AD geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Uygulama çoklu oturum açma yapılandırmak için Azure portalında Active Directory uzantısını kullanabilirsiniz. İlk adım olarak, Active Directory bölümünden Portalı'nda bir dizin seçmeniz gerekir:

![](./media/what-is-single-sign-on/azuremgmtportal.png)

Üçüncü taraf SaaS uygulamaları yönetmek için seçilen dizin'ın uygulamaları sekmesinden geçiş yapabilirsiniz. Bu görünüm, yöneticilerin sağlar:

* Azure AD galeri yanı sıra geliştirdiğiniz uygulamalar yeni uygulamalar ekleme
* Tümleşik uygulamalar Sil
* Önceden tümleştirilmiş uygulamalarını yönetme

Bir üçüncü taraf SaaS uygulaması için tipik yönetim görevleri şunlardır:

* Çoklu oturum açma parolasını SSO kullanarak veya varsa hedef SaaS için SSO Federasyon Azure AD ile etkinleştirme
* İsteğe bağlı olarak, kullanıcı sağlama ve sağlamayı kaldırma özelliklerini (kimlik yaşam döngüsü yönetimi) kullanıcı için hazırlama etkinleştirme
* Kullanıcı sağlamayı burada etkin uygulamalar için hangi kullanıcıların uygulama erişimi seçme

Yapılandırma, Federasyon çoklu oturum açmayı destekleyen galeri uygulamaları için üçüncü taraf uygulama ve Azure AD arasında bir federasyon güveni oluşturmak için sertifikalar ve meta verileri gibi ek yapılandırma ayarlarını sağlamak genellikle gerektirir. Yapılandırma Sihirbazı'nı ayrıntılarını anlatılmaktadır ve kolay erişim SaaS uygulamaya özgü verileri ve yönergeler sağlar.

Otomatik kullanıcı hazırlama destekleyen galeri uygulamaları için Azure AD SaaS uygulamasına hesaplarınızdaki yönetme izni vermek gerektirir. En azından, kimlik bilgileri Azure AD üzerinden hedef uygulamaya kimlik doğrulamasını yaparken kullanması gereken sağlamanız gerekir. Ek yapılandırma ayarlarını sağlanması gerekip gerekmediğini uygulama gereksinimlerine bağlıdır.

## <a name="deploying-azure-ad-integrated-applications-to-users"></a>Azure AD tümleşik uygulamalarını kullanıcılara dağıtma
Azure AD kuruluşunuzdaki son kullanıcılar uygulamaları dağıtmak için özelleştirilebilir çeşitli yollar sağlar:

* Azure AD erişim paneli
* Office 365 uygulama Başlatıcı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Kuruluşunuzda dağıtmayı tercih hangi yöntemleri kümeleri ' dir.

### <a name="azure-ad-access-panel"></a>Azure AD erişim paneli
Adresinden erişim Paneli'nde https://myapps.microsoft.com Azure görüntülemek için Active Directory'de son kullanıcı bir kurumsal hesap ile imkan tanıyan web tabanlı bir portalı ve Azure AD yöneticinizin erişim verilen başlatma bulut tabanlı uygulamalar için bunlar olmuştur. Son kullanıcı varsa [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), erişim paneli üzerinden Self Servis Grup Yönetimi özellikleri de kullanabilir.

![Azure AD erişim paneli](media/what-is-single-sign-on/azure-ad-access-panel.png)

Erişim paneli Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliği veya Office 365 aboneliğine sahip olmasını gerektirmez.

Azure AD erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Office 365 uygulama Başlatıcı
Office 365 dağıtmış olan kuruluşlar için Azure AD aracılığıyla kullanıcılara atanan uygulamalar aynı zamanda Office 365 portalında görünür https://portal.office.com/myapps. Bu ikinci bir portal kullanmak zorunda kalmadan kendi uygulamalarını başlatmak için bir kuruluşta kolay ve kullanıcılar için uygun yapar ve Office 365 kullanan kurumlar için önerilen uygulama başlatılırken çözümüdür.

![](./media/what-is-single-sign-on/officeapphub.png)

Office 365 uygulama Başlatıcı hakkında daha fazla bilgi için bkz: [Office 365 uygulama Başlatıcısı'nda görünen uygulamanızı sahip](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-to-federated-apps"></a>Birleştirilmiş uygulamalarda doğrudan oturum açma
SAML 2.0, WS-Federasyon veya Openıd destekleyen en Federasyon uygulamalarına destek uygulamayı başlatın ve ardından Azure AD üzerinden otomatik yeniden yönlendirme veya bir bağlantıya tıklayarak oturum açmak için oturum kullanıcılara da bağlanır. Bu hizmet sağlayıcısı olarak bilinir-oturum açma başlatılan ve Azure AD uygulama galerisinde en Federasyon uygulamalarına destek (belgelere bağlantılı ayrıntıları için Azure Portalı'nda uygulamanın tek oturum açma Yapılandırma Sihirbazı'ndan bu bakın).

![](./media/what-is-single-sign-on/workdaymobile.png)

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Federasyon, parola tabanlı veya var olan uygulamalar için doğrudan oturum açma bağlantılar
Azure AD doğrudan tek oturum açma, parola tabanlı çoklu oturum açma, var olan çoklu oturum açma ve herhangi bir biçimde Federasyon çoklu oturum açma destekleyen tek tek uygulama bağlantıları da destekler.

Bu bağlantılar kullanıcı başlatın, bunları, Azure AD erişim paneli ya da Office 365 gerek kalmadan bir kullanıcı Azure AD oturum aracılığıyla belirli bir uygulama için işlem Gönder özel olarak hazırlanmış URL'leri vardır. Bu tek oturum açma URL'leri herhangi bir önceden tümleştirilmiş uygulama Pano sekmesi altında Azure portal'ın Active Directory bölümünde aşağıdaki ekran görüntüsünde gösterildiği gibi bulunabilir.

![](./media/what-is-single-sign-on/deeplink.png)

Bu bağlantılar kopyalanır ve seçilen uygulama oturum açma bağlantısı sağlamak istediğiniz her yerden yapıştırılan. Bu, bir e-posta veya kullanıcı uygulama erişimi için ayarlamış olduğunuz tüm özel web tabanlı portal olabilir. Bir Azure AD doğrudan tek oturum açma URL'sini Twitter bir örneği burada verilmiştir:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Kuruluşa özgü URL'lere erişim paneli için benzer, daha fazla bu URL dizininiz için etkin veya doğrulanmış etki alanlarından biri sonra myapps.microsoft.com etki alanı ekleyerek özelleştirebilirsiniz. Bu, tüm kurumsal markayı hemen oturum açma sayfasında kullanıcının kullanıcı Kimliğini ilk girmeye gerek yüklendi sağlar:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Yetkili bir kullanıcı bu uygulamaya özgü bağlantılardan birini tıkladığında, bunlar ilk (Bunlar zaten oturum açmış durumda değilsiniz varsayılarak), Kurumsal oturum açma sayfasına bakın ve oturum açma işleminden sonra uygulama adresinden erişim Paneli'nde durdurmadan yönlendirilir. Kullanıcı parola tabanlı, çoklu oturum açma tarayıcı uzantısı gibi bir uygulamaya erişmek için ön koşullar eksikse bağlantıyı eksik uzantıyı yüklemek için kullanıcıya sorar. Bağlantı URL'si de tek oturum açma yapılandırması uygulama için değişirse sabit kalır.

Office 365 ve erişim paneli aynı erişim denetimi mekanizmaları bu bağlantıları kullanın ve yalnızca bu kullanıcıları veya Azure portalında uygulamanıza atanan grupları başarıyla kimlik doğrulaması için olacaktır. Ancak, yetkilendirilmemiş herhangi bir kullanıcı oldukları erişim verilmemiş ve erişim sahip oldukları kullanılabilir uygulamaları görüntülemek için erişim paneli yüklemek için bir bağlantı belirtilen açıklayan bir ileti görürsünüz.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](../saas-apps/tutorial-list.md)
* [Cloud App Discovery ile bulut uygulamaları tasdik bulma](cloud-app-discovery.md)
* [Uygulamalara erişimi yönetme giriş](what-is-access-management.md)
* [Dış kimlikler Azure AD'de yönetmek için özellikleri karşılaştırma](../active-directory-b2b-compare-b2c.md)


