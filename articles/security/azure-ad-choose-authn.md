---
title: Azure AD karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin. | Microsoft Docs
description: Bu kılavuz, şirket, Cıo'lar, CISOs, baş kimlik mimarları, Kurumsal mimarlar ve güvenlik yardımcı olmak için tasarlanmıştır ve BT karar alıcılar sorumlu Orta ve büyük kendi Azure AD karma kimlik çözümü için bir kimlik doğrulama yöntemi seçme kuruluşlar.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 373efcd821946ab5814ab58284ef7778b779260a
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="choosing-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Azure Active Directory karma kimlik çözümü için uygun kimlik doğrulama yöntemini seçme 

Bu makalede bir tam uygulamak hızlandırmasına yardımcı olmak için ilk makaleleri bir dizi olan Azure AD karma kimlik çözümü. Tam bir Azure AD karma kimlik çözümü özetlenen kapsar işletme sonuçlarını ve karma kimlik sayısal dönüşüm Framework ve hedefleri kuruluşlar odaklanabilirsiniz üzerinde bunlar güçlü ve Güvenli Karma kimlik uygulanan emin olmak için çözümü. Framework'ün ilk iş sonucu harfe dönüştüren kullanıcılar bulut uygulamalarını eriştiklerinde kimlik doğrulama işlemi güvenliğini sağlamak kuruluşlar için gereksinimleri çıkışı. İlk kimlik doğrulaması güvenli iş sonucu iş hedefte kullanıcıların uygulamaları kendi şirket içi kullanıcı adlarını ve parolaları kullanarak bulut oturum açmak özelliğidir. Bu oturum açma işlemi ve nasıl kullanıcıların kimlik doğrulaması her şeyi bulutta olanaklı kılar.

Doğru kimlik doğrulama yöntemi seçme uygulamalarını buluta taşımak isteyen kuruluşlar için ilk konusudur. Bu karara hafifçe aşağıdaki nedenlerden dolayı alınması gerekir:

1. Buluta taşımak isteyen bir kuruluş için ilk karardır. 

2. Kimlik doğrulama yöntemi, bir kuruluşun bulunması tüm bulut verilerini ve kaynaklara erişimi denetleme bulut kritik bir bileşenidir.

3. Bu tüm diğer gelişmiş güvenlik ve kullanıcı deneyimi özellikleri Azure AD'de oluşturmaktadır.

4. Bir kez uygulanan kimlik doğrulama yöntemini değiştirme zorluk.

BT güvenlik yeni denetim düzlemi olarak kimliği ile kimlik doğrulaması kuruluşun erişim koruma yeni bir bulut dünya için kullanılır. Kuruluşlar, daha güçlü güvenlik kimlik denetim düzlemi yapar emin olun ve bulut uygulamalarını saldırganların güvenli tutun.

### <a name="out-of-scope"></a>Kapsamının dışında

Varolan bir şirket içi dizin ayak izini sahip olmayan kuruluşlarda, bu makaleyi odağını değil. Genellikle, bu işletmeler, bir karma kimlik çözümü gerektirmeyen yalnızca bulutta kimlikleri oluşturun. Yalnızca bulut kimlikleri yalnızca bulutta mevcut ve karşılık gelen şirket içi kimlikleri ile ilişkili değil.  

## <a name="choosing-the-right-authentication-method"></a>Doğru kimlik doğrulama yöntemi seçme

Azure AD karma kimlik çözümü ile yeni denetim düzlem olarak, kimlik doğrulama bulut erişimi oluşturmaktadır. Doğru kimlik doğrulama yöntemi seçerek Azure AD karma kimlik çözümünü ayarlama önemli bir ilk karar değil. Kimlik doğrulama yöntemi uygulama de buluttaki kullanıcılara sağlama Azure AD Connect kullanılarak yapılandırılır. 

Bir kimlik doğrulama yöntemi seçmek için zaman, mevcut altyapınızı, karmaşıklık ve tercih ettiğiniz uygulama maliyetini göz önünde bulundurmanız gerekir. Bu faktörlerin her kuruluş için farklı ve zaman içinde değişebilir. 

Azure AD karma kimlik çözümleri için aşağıdaki kimlik doğrulama yöntemlerini destekler:

### <a name="cloud-authentication"></a>Bulut kimlik doğrulaması
Bu kimlik doğrulama yöntemini seçtiğinizde, Azure AD kullanıcılar için oturum açma işlemini gerçekleştirir. Sorunsuz çoklu oturum açma ile (SSO) eşleşmiş, kullanıcıların kendi kimlik bilgilerini girmek zorunda kalmadan bulut uygulamalarında oturum açabilir. Bulut kimlik doğrulamasıyla iki seçenekler arasından seçim yapabilirsiniz: 

**Parola karma eşitlemesi (PHS)** – şirket içi dizin nesneleri için Azure AD kimlik doğrulaması yapmanın en kolay yolu. Parola karma eşitlemesi kullanıcıların aynı kullanıcı adı ve parola şirket içinde kullandıkları kullanmasını sağlayan herhangi bir ek altyapı dağıtmak zorunda kalmadan. Bazı kimlik koruması gibi Azure AD premium özelliklerini bağımsız olarak kimlik doğrulama yöntemini seçili Parola Karması eşitlemesi gerektirir.

> [!NOTE] 
> Parolaları hiçbir zaman şifresiz metin olarak depolanır veya Azure AD'de bir ters çevrilebilir algoritması ile şifrelenmiş. Parola karma eşitlemesi gerçek işlemi hakkında daha fazla bilgi için bkz: [parola karma eşitlemesi ile Azure AD Connect eşitleme uygulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Doğrudan kimlik doğrulama (PTA)** – kullanıcıları, şirket içi Active ile doğrudan doğrulamak için bir veya daha fazla şirket içi sunucuları üzerinde çalışan bir yazılım aracı kullanarak Azure AD kimlik doğrulama hizmetleri için bir basit parola doğrulaması sağlar Parola doğrulama sağlama dizin bulutta gerçekleşmez. Şirket içi kullanıcı hesabı hemen uygulamak için bir güvenlik gereksinimidir şirketlerle durumları, parola ilkeleri ve oturum açma saatleri bu kimlik doğrulama yöntemini kullanırsınız. Gerçek doğrudan kimlik doğrulama işlemi hakkında daha fazla bilgi için bkz: [kullanıcı oturum açma Azure Active Directory doğrudan kimlik doğrulama ile](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Federe kimlik doğrulaması
Ne zaman sisteme kullanıcının parolasını doğrulamak için güvenilen ayrı kimlik doğrulama, örneğin, bir şirket içi Active Directory Federasyon Hizmetleri (AD FS) kimlik doğrulama işlemi Azure AD ellerini bu kimlik doğrulama yöntemini seçin. Kimlik doğrulama sistemi akıllı kart tabanlı kimlik doğrulaması veya bir üçüncü taraf çok faktörlü kimlik doğrulama gibi ek kimlik doğrulama gereksinimleri sağlayabilir. Daha fazla bilgi için bkz: [Active Directory Federasyon Hizmetleri dağıtma](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

Aşağıdaki bölümde, hangi kimlik doğrulama yönteminin size, karar ağacı kullanarak uygun olduğuna karar vermenize yardımcı olur. Bulut veya şirket dışı kimlik doğrulaması için Azure AD karma kimlik çözümü dağıtılıp dağıtılmayacağını belirlemenize yardımcı olur.

## <a name="azure-ad-authentication-decision-tree"></a>Azure AD kimlik doğrulama karar ağacı

![image1](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations-on-authentication-methods"></a>Kimlik doğrulama yöntemleri üzerinde ayrıntılı değerlendirmeler

### <a name="cloud-authentication-password-hash-sync"></a>Bulut kimlik doğrulaması: parola karması eşitlemesi 

* **Efor:** parola karması eşitlemesi yalnızca Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklara oturum açmak, kullanıcılar etkinleştirmeniz gerekiyor kuruluşlar için en az çaba dağıtımı, Bakım ve altyapı gerektirir. Etkinleştirildiğinde, parola karma eşitlemesi ve Azure AD Connect eşitleme işleminin bir parçasıdır ve her iki dakikada bir çalışır. 

* **Kullanıcı deneyimi:** kuruluşlar sorunsuz çoklu oturum açma (SSO) oturum açtıktan sonra gereksiz istemleri kaçınarak kullanıcının oturum açma deneyimini geliştirmek için parola karma eşitlemesi ile dağıtmanız önerilir.

* **Gelişmiş senaryolar:** kuruluşlar seçerseniz, bu kimlikleri ilişkin bilgiler sızan kimlik bilgileri rapor gibi Azure AD Identity Protection raporları ile birlikte kullanmak da mümkündür. İş için Windows Hello olduğu sahip başka bir seçenek [parola karması eşitlemesi kullandığınızda belirli gereksinimleri](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). Parola karma eşitlemesi ile çok faktörlü kimlik doğrulaması gerektiren kuruluşların Azure AD çok faktörlü kimlik doğrulaması kullanmanız gerekir ve bir üçüncü taraf veya şirket içi çok faktörlü kimlik doğrulama yöntemleri kullanamazsınız.

* **İş sürekliliği:** parola karma eşitlemesi için tüm Microsoft veri merkezlerine ölçeklendirir bir bulut hizmeti kendiliğinden yüksek oranda kullanılabilir. Hazırlama modu olağanüstü durum kurtarma amacıyla bekleme yapılandırmasında içinde ikinci bir Azure AD Connect sunucusu dağıttığınız önerilir.

* **Dikkate alınacak noktalar:** parola karması eşitlemesi hemen zorlamaz şirket içi hesap durumları değişiklikleri şu anda. Bu durumda, bir kullanıcının kullanıcı hesabı durumunu Azure AD ile eşitlenene kadar bulut uygulamalarında erişebilir. Kuruluşlar bu sınırlamanın üstesinden gelmek istiyorsanız, yöneticiler hesapları devre dışı bırakma gibi şirket içi kullanıcı hesabı durumları, güncelleştirmelerinin toplu sonra yeni bir eşitleme döngüsü etkinleştirilir önerilir. Kilitli, bir sonraki döngüde eşitlenen başka bir kullanıcı hesabı durumu hesabıdır. 

> [!NOTE] 
> Parolanın süresi durum anda eşitlenmedi Azure AD Connect ile Azure ad. 

Başvurmak [parola karması eşitlemesi uygulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) dağıtım adımları için.

### <a name="cloud-authentication-pass-through-authentication"></a>Bulut kimlik doğrulaması: doğrudan kimlik doğrulama  

* **Efor:** doğrudan kimlik doğrulaması için bir gereksinim duyduğunuz ya da (daha fazla üç önerilen) şirket içi Active Directory etki alanı dahil olmak üzere şirket içi hizmetlerinizi, erişimi olan mevcut sunucularda yüklü basit aracılar AD etki alanı denetleyicileri. Bu aracıları giden Internet erişimi gerekir ve etki alanı denetleyicileriniz erişebilir. Etki alanı denetleyicilerine Kısıtlanmamış ağ erişim gerektirdiğinden bu nedenle, onu bir çevre ağında aracılarını dağıtmak için desteklenmiyor. Tüm ağ trafiğini şifrelenmiş ve kimlik doğrulama isteklerini sınırlıdır. Bu işlem hakkında daha fazla bilgi için bkz: [güvenlik derinlemesine](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) doğrudan kimlik doğrulaması.

* **Kullanıcı deneyimi:** kuruluşlar sorunsuz çoklu oturum açma oturum açtıktan sonra gereksiz istemleri kaçınarak kullanıcının oturum açma deneyimini geliştirmek için doğrudan kimlik doğrulama ile dağıtmanız önerilir.

* **Gelişmiş senaryolar:** doğrudan kimlik doğrulama, şirket içi hesap ilkesi hemen zorlar. Örneğin, erişim, bir şirket içi kullanıcının hesap durumu, kilitli devre dışıdır, parolanın süresi doldu veya oturum açma saatleri dışında kullanıcının döner izin reddedildi.  Doğrudan kimlik doğrulama ile çok faktörlü kimlik doğrulaması gerektiren kuruluşların Azure AD çok faktörlü kimlik doğrulaması kullanmanız gerekir ve bir üçüncü taraf veya şirket içi çok faktörlü kimlik doğrulama yöntemini kullanamazsınız. Gelişmiş Özellikler, gibi kimlik koruması sızan kimlik bilgileri raporunu doğrudan kimlik doğrulama seçerseniz parola karması eşitlemesi bakılmaksızın dağıtılan gerektirir.

* **İş sürekliliği:** kimlik doğrulama isteklerini yüksek kullanılabilirliğini sağlamak için Azure AD Connect sunucudaki ilk aracı yanı sıra iki ek doğrudan kimlik doğrulama aracıları dağıtmanız önerilir. Dağıtılan üç aracıları içerdiğinde, bakım için başka bir aracı aşağı olduğunda, bir aracı yine başarısız olabilir. Parola karma eşitlemesi doğrudan kimlik doğrulama ek olarak dağıtma başka bir avantajdır birincil kimlik doğrulama yöntemini artık şirket içi sunucular kullanılabilir olmadığında örnek için kullanılabilir duruma geldiğinde yedek kimlik doğrulama yöntemi olarak davranamaz.

* **Dikkate alınacak noktalar:** doğrudan kimlik doğrulama için bir yedek kimlik doğrulama yöntemi olarak parola karması eşitlemesi kullanın ve aracıları kullanıcının kimlik bilgileri doğrulanamıyor durumunda parola karma eşitlemesi için yük devretme otomatik olarak gerçekleştirilmez. Azure AD Connect kullanarak el ile oturum açma yöntemi geçmeniz gerekir. Doğrudan kimlik doğrulaması yalnızca modern kimlik doğrulaması ve ActiveSync, POP3 ve IMAP4 gibi belirli Exchange Online protokoller kullanan bulut uygulamaları destekler. Örneğin, [Microsoft Office 2013 ve üzeri destekleyen modern kimlik doğrulaması, ancak değil önceki sürümlerinde](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/) Office uygulama desteği hakkında daha fazla bilgi üzerinde. Bkz: [sık sorulan sorular](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) ve diğer önemli noktalara alternatif kimlik dahil olmak üzere doğrudan kimlik doğrulama desteği.

Başvurmak [doğrudan kimlik doğrulama uygulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) dağıtım adımları için.

### <a name="federated-authentication"></a>Federe kimlik doğrulaması

* **Efor:** bir federe kimlik doğrulama sistemi kullanan kullanıcıların kimliklerini doğrulamak için bir dış sistemde kullanır. Bazı şirketler, var olan Federasyon sistem yatırımlarını kendi Azure AD karma kimlik çözümü ile yeniden kullanmak istiyorum. Bakım ve Federasyon sisteminin yönetimi, Azure AD denetimi dışında döner. Bu, güvenli bir şekilde dağıtıldıktan ve kimlik doğrulama yükü işleyebilir emin olmak için Federasyon sistemini kullanarak kuruluş kadar olur. 

* **Kullanıcı deneyimi:** federe kimlik doğrulaması kullanıcı deneyimini özellikleri, topoloji ve Federasyon grubu yapılandırmasının mantığınız bağlıdır. Bazı kuruluşlarda uyum ve güvenlik gereksinimlerine uyacak şekilde Federasyon grubu erişimi yapılandırmak için bu esneklik gerektirir. Örneğin, bunlar zaten cihazlarına açan çünkü bunları kimlik bilgileri için sormadan otomatik olarak, kullanıcılar oturum dahili olarak bağlı kullanıcıları ve aygıtları Yapılandır mümkündür. Diğer taraftan, gerekirse, bazı gelişmiş güvenlik özellikleri kullanıcının oturum açma işlemini daha zor hale getirebilir.

* **Gelişmiş senaryolar:** federe kimlik doğrulama çözümüdür genellikle gerekli müşteriler yerel olarak Azure AD tarafından desteklenen bir kimlik doğrulama gereksinimi varsa, ayrıntılı bilgiler [burada listelenen](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/), ancak ortak gereksinimleri şunları içerir:

    * Akıllı kart veya sertifika gerektiren kimlik doğrulaması
    * Kullanarak MFA sunucusu ya da üçüncü taraf çok faktörlü sağlayıcısı şirket içi.
    * Üçüncü taraf kimlik doğrulama çözümü kullanarak kimlik doğrulaması. Bkz: [Azure AD Federasyonu uyumluluk listesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Kullanıcılar kendi sAMAccountName, örneğin, etki alanı\kullanıcı adı, bir kullanıcı asıl adı (UPN), kullanmak yerine kullanarak örneğin oturum gerekir, user@domain.com

* **İş sürekliliği:** federe sistemleri genellikle gerektiren kimlik doğrulama istekleri için yüksek kullanılabilirlik sağlamak için bir iç ağ ve çevre ağ topolojisinde yapılandırılmış bir yük dengeli sunucuları dizisi, bir grup olarak da bilinir. Birincil kimlik doğrulama yöntemini artık şirket içi sunucular kullanılabilir olmadığında örneğin kullanılabilir olduğunda parola karması eşitlemesi federe kimlik doğrulaması ile birlikte yedek kimlik doğrulama yöntemi olarak dağıtılabilir. Bazı büyük ölçekli kuruluşların coğrafi DNS düşük gecikme süresi kimlik doğrulama istekleri için yapılandırılmış birden fazla Internet giriş noktaları desteklemek için bir Federasyon çözümü gerektirir.

* **Dikkate alınacak noktalar:** federe sistemleri genellikle şirket içi altyapı daha önemli yatırım gerektirir. Çoğu kuruluş, bunlar zaten bir şirket içi Federasyon yatırımınız var ve bu tek kimlik sağlayıcısı kullanmak için bir güçlü iş gereksinimi varsa bu seçeneği belirleyin. Federasyon, bulut kimlik doğrulaması çözümleri için çalışır ve sorun gidermek için daha karmaşık karşılaştırıldığında ' dir. Kullanıcı kimliklerini uygulamak için de need ek yapılandırma imzalamak için Azure AD'de doğrulanamıyor yönlendirilemeyen bir etki alanı ile kullanma. Bu gereksinim alternatif oturum açma kimliği desteği denir. Bkz: [alternatif oturum açma Kimliğini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) kısıtlamalar ve gereksinimler için. Bir üçüncü taraf çok faktörlü kimlik doğrulama sağlayıcısı ile Federasyon kullanmayı tercih ederseniz, lütfen sağlayıcı aygıtların Azure AD katılım olanak tanımak için WS-Trust desteklediğinden emin olun.

Başvurmak [Federasyon Hizmetleri uygulama](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) dağıtım adımları için.

> [!NOTE] 
> Azure AD karma kimlik çözümü dağıttığınızda, Azure AD Connect desteklenen topolojiler birini uygulamak emin olmalısınız. Yaklaşık desteklenen ve desteklenmeyen yapılandırmalar, daha fazla bilgi edinin [Azure AD Connect için topolojiler](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Mimarisi diyagramları

Aşağıdaki diyagram anahatları üst düzey mimari bileşenleri, Azure AD karma kimlik çözümü ile kullandığınız her kimlik doğrulama yöntemi için gereklidir. Çözümleri arasındaki farklılıklar karşılaştırmak için genel bir bakış sağlar.

Aşağıdaki diyagramda bir parola karması eşitlemesi çözüm basitliği özetlenmektedir:

![PHS](media/azure-ad/azure-ad-authn-image2.png)

Aşağıdaki diyagramda, geçiş kimlik doğrulama Aracısı gereksinimleri özetlenmektedir:

![PTA](media/azure-ad/azure-ad-authn-image3.png)

Aşağıdaki diyagramda, çevre ve iç ağ, kuruluşunuzun Federasyon için gereken bileşenleri özetlenmektedir:

![ADFS](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-the-authentication-methods"></a>Kimlik doğrulama yöntemlerini karşılaştırma

|Değerlendirme|Parola karma eşitlemesi + sorunsuz SSO|Doğrudan kimlik doğrulama + sorunsuz SSO|AD FS ile Federasyon|
|:-----|:-----|:-----|:-----|
|Kimlik doğrulama burada oluşuyor?|Bulutta|Bulutta, güvenli parola doğrulaması exchange şirket içi kimlik doğrulama Aracısı ile sonra|Şirket içi|
|Hazırlama sisteminin ötesine şirket içi sunucu gereksinimleri nelerdir: Azure AD Connect?|None|her ek kimlik doğrulama aracısı için 1 sunucusu|2 veya daha fazla AD FS sunucuları<br>çevre/çevre ağındaki 2 veya daha fazla WAP sunucuları|
|Şirket içi Internet ve ağ sağlama sistem ötesinde gereksinimleri nelerdir?|None|[Giden Internet erişimi](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start) sunucularından kimlik doğrulama aracılar çalışan|[Internet erişimi gelen](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-requirements) çevre WAP sunuculara<br>AD FS sunucuları çevre WAP sunuculardan gelen ağ erişimi<br>Ağ Yükü Dengeleme|
|Bir SSL sertifikası gereksinimi var mı?|Hayır|Hayır|Evet|
|Bir sistem durumu izleme çözümü var mı?|Gerekli değil|Aracı durumu tarafından sağlanan [Azure Active Directory Yönetim Merkezi](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication)|[Azure AD Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs)|
|Kullanıcılar, şirket ağı içinde etki alanına katılmış aygıtlardan bulut kaynaklarına çoklu oturum açma alma?|Evet ile [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Evet ile [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Evet|
|Hangi oturum açma türleri desteklenir?|UserPrincipalName + parola<br>Windows tümleşik kimlik doğrulaması kullanarak [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)|UserPrincipalName + parola<br>Windows tümleşik kimlik doğrulaması kullanarak [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq)|UserPrincipalName + parola<br>sAMAccountName + parola<br>Windows tümleşik kimlik doğrulaması<br>[Sertifika ve akıllı kart kimlik doğrulaması](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Windows Hello desteklenen iş için mi?|[Anahtar güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>[Intune ile sertifika güven modeli](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Anahtar güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>[Intune ile sertifika güven modeli](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Anahtar güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>[Sertifika güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Çok faktörlü kimlik doğrulama seçenekleri nelerdir?|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)<br>[Azure MFA sunucusu](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)<br>[Üçüncü taraf MFA](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)|
|Hangi kullanıcı hesabı durumları destekleniyor mu?|Devre dışı bırakılan hesapları<br>Hesap Kilitlendi<br>(en fazla 30 dakikalık gecikme)|Devre dışı bırakılan hesapları<br>Hesap Kilitlendi<br>Parolanın süresi dolsun<br>Oturum açma saatleri|Devre dışı bırakılan hesapları<br>Hesap Kilitlendi<br>Parolanın süresi dolsun<br>Oturum açma saatleri|
|Koşullu erişim seçenekleri nelerdir?|[Azure AD koşullu erişim ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD koşullu erişim ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD koşullu erişim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)<br>[AD FS talep kuralları](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Desteklenen eski protokolleri engelliyor?|Hayır|Hayır|[Evet](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|, Özelleştirilmiş logosu, resmi ve oturum açma sayfalarındaki açıklama?|[Azure AD Premium ile Evet](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Azure AD Premium ile Evet](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Evet](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Hangi Gelişmiş senaryolar desteklenir?|[Akıllı parola kilitleme](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-secure-passwords)<br>[Kimlik bilgileri raporları sızmasını](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-risk-events)|[Akıllı parola kilitleme](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Çok siteli düşük gecikme süresi kimlik doğrulama sistemi<br>[AD FS extranet kilitleme](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection)<br>[Üçüncü taraf kimlik sistemleriyle tümleştirme](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

## <a name="recommendations-and-considerations-from-azure-ad"></a>Öneriler ve Azure AD'den konuları

Kimlik sistemi, kullanıcıların uygulamaları ve geçirmek ve bulutta kullanılabilir hale satır iş kolu uygulamaları bulut erişimi sağlar. Kimlik doğrulama, yetkili kullanıcıların üretken ve kötü aktörleri, kuruluşunuzun hassas verileri dışında tutmak için uygulamalara erişimi denetler. Bu nedenle, kuruluşunuz için doğru kimlik doğrulama yöntemi seçerken aşağıdaki önerileri göz önünde bulundurun. 

Kullanın veya parola karma eşitlemesi hangi kimlik doğrulama yönteminin olsun, aşağıdaki nedenlerle seçtiğiniz etkinleştir:

1. **Yüksek kullanılabilirlik ve olağanüstü durum kurtarma:** doğrudan kimlik doğrulama ve Federasyon içi altyapınızı kullanır. Geçişli kimlik doğrulaması için şirket içi ayak doğrudan kimlik doğrulama Aracısı gerektiren sunucu donanım ve ağ var. Proxy kimlik doğrulama isteklerini, çevre ağındaki sunucuları ve iç federasyon sunucuları gerektirdiğinden Federasyon için şirket içi ayak bile daha büyük. Tek nokta hataları önlemek için kuruluşunuz kimlik doğrulama isteklerini herhangi bir bileşenin başarısız olursa her zaman sunulur emin olmak için yedekli sunucuları dağıtmanız gerekir. Ayrıca doğrudan kimlik doğrulama ve Federasyon de başarısız olabilir kimlik doğrulama isteklerine yanıt vermek için etki alanı denetleyicilerinde bağımlı değildir. Bu bileşenlerin çoğu sağlıklı kalmak için bakım gerekiyor ve Bakım değil planlanması ve doğru bir şekilde kesintilerine karşı daha yüksektir. Microsoft'un Azure AD bulut kimlik doğrulama hizmeti genel olarak ölçeklendirir ve her zaman kullanılabilir olduğu için parola karma eşitlemesi kullanarak kesintileri önlenebilir.

2. **Şirket içi kesinti hayatta:** şirket içi kesinti siber saldırı veya olağanüstü durum nedeniyle sonuçlarını reputational marka zarar bir saldırı ile mücadele etmek mümkün paralyzed kuruluş arasında değişen önemli olabilir. Geçen yıl içinde birçok kuruluşun kendi şirket içi sunucuların aşağı olmasını neden hedeflenen yazılımı dahil olmak üzere kötü amaçlı yazılım saldırılarını kurbanı yoktu. Müşteriler bu tür saldırılar ile ilgili Yardım Microsoft Kuruluşlar iki kategorisi fark:

   a. Daha önce parola karması eşitlemesi etkinleştirilmiş kuruluşlar birkaç saat içinde tekrar çevrimiçi parola karması eşitlemesi kullanmak için kendi kimlik doğrulama yöntemini değiştirerek. Office 365 e-posta erişimi kullanarak bunların sorunlarını çözün ve diğer bulut tabanlı iş yüklerini erişimi için işe yarayabilir.

   b. Daha önce parola karma eşitlemesini etkinleştirmek kaydetmedi kuruluşlar güvenilmeyen dış tüketici e-posta sistemleri iletişimleri ve çözümleme sorunları için çözümlemelere gerekiyordu. Bu durumda, bunları hafta veya yeniden çalışır olması için daha fazla sürdü.

3. **Kimlik koruması:** kullanıcıları bulutta korumak için en iyi yollarından biri Azure AD kimlik koruması. Microsoft Internet kullanıcı ve parola listeleri için sürekli tarar, kötü aktörleri satmak ve koyu Web'de kullanılabilir hale getirin. Azure AD, herhangi bir kullanıcı adları ve parolalar, kuruluşunuzda tehlikeye olmadığını doğrulamak için bu bilgileri kullanabilirsiniz. Bu nedenle, Federasyon veya doğrudan kimlik doğrulaması olup olmadığını kullanın, hangi kimlik doğrulama yöntemini bakılmaksızın parola karma eşitlemesini etkinleştirmek için önemlidir. Sızan kimlik bilgileri bir rapor olarak sunulan ve engelleme veya sızan parolasıyla oturum açmanız çalıştıklarında parolalarını değiştirmek için bir kullanıcı zorlamak için kullanılabilir.

Son olarak, aşağıdakilere göre [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), Microsoft, kimlik ve erişim yönetimi işlevleri en tam özellikli kümesi sahiptir. Yedi trilyon siber olayları sağlarken oturum açma binlerce SaaS uygulamasına Office 365 gibi yetkili kullanıcıların herhangi bir CİHAZDAN günde kapalı Microsoft fends. 

## <a name="conclusion"></a>Sonuç

Çeşitli kimlik doğrulama seçenekleri, kuruluşlar, yapılandırma ve bulut uygulamalarında erişimini desteklemek üzere dağıtma bu makalede anahatları. Çeşitli iş, güvenlik ve teknik gereksinimleri karşılamak için kuruluşların parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon arasında seçebilirsiniz. Her kimlik doğrulama yöntemiyle kuruluşunuzun iş gereksinimlerinizi çözümü ve kullanıcı deneyimi oturum açma işleminin dağıtmak için çaba tarafından ele alınan seçebilirsiniz. Gelişmiş senaryolar ve iş devamlılığı özellikleri her kimlik doğrulama yönteminin kuruluşunuzun ihtiyacı olup olmadığını değerlendirmek gerekir. Son olarak, herhangi bir tercih ettiğiniz uygulama önleme olmadığını görmek için her kimlik doğrulama yönteminin konuları değerlendirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Günümüzün dünyada tehditleri günde 24 saat mevcut olan ve her yerde gelir. Doğru kimlik doğrulama yöntemi uygulama, güvenlik riskleri azaltmak ve kimliklerinizi korunmasına yardımcı olur. 

[Başlama](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) Azure AD ile ve kuruluşunuz için doğru kimlik doğrulama çözümü dağıtın.

Bulut kimlik doğrulaması için Federasyon geçiş düşünüyorsanız, daha fazla bilgi edinin [oturum açma yöntemini değiştirme konusunda](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Geçiş planlamanızı ve yardımcı olmak için kullanabileceğiniz [bu proje yardımcı olması için planlarında](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/tree/master/Authentication).
