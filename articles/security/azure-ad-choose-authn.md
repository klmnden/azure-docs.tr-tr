---
title: Azure AD karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin. | Microsoft Docs
description: Bu kılavuz, şirket, Cıo'lar, CISOs, baş kimlik mimarları, Kurumsal mimarlar ve güvenlik yardımcı olur ve BT karar alıcılar Orta ve büyük kuruluşların kendi Azure AD karma kimlik çözümü için bir kimlik doğrulama yöntemi seçme sorumlu.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 01b76ea902ec92f8ab32bc00cff27b1b890ce9ff
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113111"
---
# <a name="choose-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Azure Active Directory karma kimlik çözümü için uygun kimlik doğrulama yöntemini seçin 

Bu makalede, bir dizi tam Azure Active Directory (Azure AD) karma kimlik çözümü uygulamak kuruluşların Yardım makaleleri başlar. Bu çözüm olarak ana hatlarıyla [karma kimlik sayısal dönüşüm Framework](https://aka.ms/aadframework). İşletme sonuçlarını kapsayan ve hedefleri kuruluşlar güçlü ve Güvenli Karma kimlik çözümü uygulamak için üzerinde odaklanabilirsiniz. 

Framework'ün ilk iş sonucu harfe dönüştüren kullanıcılar bulut uygulamalarını eriştiklerinde kimlik doğrulama işlemi güvenliğini sağlamak kuruluşlar için gereksinimleri çıkışı. İlk iş kimlik doğrulaması güvenli iş sonucu, kullanıcıların uygulamaları kendi şirket içi kullanıcı adlarını ve parolaları kullanarak bulut oturum açmak yeteneğini hedeftir. Bu oturum açma işlemi için ve nasıl kullanıcıların kimlik doğrulaması her şeyi bulutta mümkün kılar.

Doğru kimlik doğrulama yöntemi seçme uygulamalarını buluta taşımak isteyen kuruluşlar için ilk konusudur. Bu kararı, aşağıdaki nedenlerle dikkatli yok:

1. Buluta taşımak isteyen bir kuruluş için ilk karardır. 

2. Kimlik doğrulama yöntemi, bir kuruluşun varlık bulutta kritik bir bileşenidir. Tüm bulut verilerini ve kaynaklarına erişimi denetler.

3. Bu tüm diğer gelişmiş güvenlik ve kullanıcı deneyimi özellikleri Azure AD'de oluşturmaktadır.

4. Kimlik doğrulama yöntemini uygulandıktan sonra değiştirmek zor olabilir.

BT güvenlik yeni denetim düzlemi kimliğidir. Bu nedenle kimlik doğrulaması kuruluşun erişim koruma yeni bir bulut dünya için kullanılır. Kuruluşların kendi güvenlik güçlendirir ve bulut uygulamalarını saldırganların güvenli tutar bir kimlik denetim düzlemi gerekir.

### <a name="out-of-scope"></a>Kapsamının dışında
Varolan bir şirket içi dizin ayak izini sahip olmayan kuruluşlarda, bu makaleyi odağını değil. Genellikle, bu işletmeler, bir karma kimlik çözümü gerektirmeyen yalnızca bulutta kimlikleri oluşturun. Yalnızca bulut kimlikleri yalnızca bulutta mevcut ve karşılık gelen şirket içi kimlikleri ile ilişkili değil.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri
Azure AD karma kimlik çözümü yeni denetim düzlemi olduğunda, kimlik doğrulama bulut erişimi temelidir. Doğru kimlik doğrulama yöntemi seçerek Azure AD karma kimlik çözümünü ayarlama önemli bir ilk karar değil. Ayrıca kullanıcılar bulutta sağlar Azure AD Connect kullanarak yapılandırılan kimlik doğrulama yöntemini uygulayın.

Bir kimlik doğrulama yöntemi seçmek için zaman, mevcut altyapınızı, karmaşıklık ve tercih ettiğiniz uygulama maliyetini göz önünde bulundurmanız gerekir. Bu faktörlerin her kuruluş için farklı ve zaman içinde değişebilir. 

>[!VIDEO https://www.youtube.com/embed/YtW2cmVqSEw]

Azure AD karma kimlik çözümleri için aşağıdaki kimlik doğrulama yöntemini destekler.

### <a name="cloud-authentication"></a>Bulut kimlik doğrulaması
Bu kimlik doğrulama yöntemini seçtiğinizde, Azure AD kullanıcıların oturum açma işlemini gerçekleştirir. Sorunsuz çoklu oturum açma ile (SSO) eşleşmiş, kullanıcıların kendi kimlik bilgilerini girmek zorunda kalmadan bulut uygulamalarında oturum açabilir. Bulut kimlik doğrulamasıyla iki seçenekler arasından seçim yapabilirsiniz: 

**Azure AD parola karma eşitlemesi**. Şirket içi dizin nesneleri için Azure AD kimlik doğrulaması yapmanın en kolay yolu. Kullanıcılar, aynı kullanıcı adı ve parola şirket içinde kullandıkları kullanabilir herhangi bir ek altyapı dağıtmak zorunda kalmadan. Seçtiğiniz olsun hangi kimlik doğrulama yöntemi için bazı premium özellikleri kimlik koruması gibi Azure ad parola karması eşitlemesi gerektirir.

> [!NOTE] 
> Parolaları hiçbir zaman şifresiz metin olarak depolanır veya Azure AD'de bir ters çevrilebilir algoritması ile şifrelenmiş. Parola karma eşitlemesi gerçek işlemi hakkında daha fazla bilgi için bkz: [parola karma eşitlemesi ile Azure AD Connect eşitleme uygulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Azure AD doğrudan kimlik doğrulama**. Azure AD kimlik doğrulama hizmetleri için bir basit parola doğrulaması bir veya daha fazla şirket içi sunucular üzerinde çalışan bir yazılım aracı kullanarak sağlar. Sunucularını doğrudan şirket içi Active directory'nizle parola doğrulama bulutta gerçekleşmez sağlayan, kullanıcıların doğrulayın. 

Şirket içi kullanıcı hemen uygulamak için bir güvenlik gereksinimidir şirketlerle durumları, parola ilkeleri, hesap ve oturum açma saatleri bu kimlik doğrulama yöntemini kullanabilirsiniz. Gerçek doğrudan kimlik doğrulama işlemi hakkında daha fazla bilgi için bkz: [kullanıcı oturum açma Azure AD doğrudan kimlik doğrulama ile](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Federe kimlik doğrulaması
Bu kimlik doğrulama yöntemini seçtiğinizde, Azure AD ellerini şirket içi Active Directory Federasyon Hizmetleri (AD FS) gibi bir güvenilen ayrı kimlik doğrulama sistemi için kimlik doğrulama işlemi kullanıcının parolasını doğrulamak için kapalı.

Kimlik doğrulama sistemi ek Gelişmiş kimlik doğrulama gereksinimleri sağlayabilir. Akıllı kart tabanlı kimlik doğrulaması veya üçüncü taraf çok faktörlü kimlik doğrulama örnektir. Daha fazla bilgi için bkz: [Active Directory Federasyon Hizmetleri dağıtma](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

Aşağıdaki bölümde karar ağacı kullanarak hangi kimlik doğrulama yönteminin size uygun olduğuna karar vermenize yardımcı olur. Bulut veya şirket dışı kimlik doğrulaması için Azure AD karma kimlik çözümü dağıtılıp dağıtılmayacağını belirlemenize yardımcı olur.

## <a name="decision-tree"></a>Karar ağacı

![Azure AD kimlik doğrulama karar ağacı](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations"></a>Ayrıntılı değerlendirmeler

### <a name="cloud-authentication-password-hash-synchronization"></a>Bulut kimlik doğrulaması: parola karması eşitlemesi

* **Efor**. Parola karma eşitlemesi dağıtımı, Bakım ve altyapı ile ilgili en az çabayı gerektirir.  Bu düzey çaba genellikle yalnızca Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklara oturum açmak için kullanıcılar gereken kuruluşlar için de geçerlidir. Etkinleştirildiğinde, parola karması eşitlemesi Azure AD Connect eşitleme işleminin bir parçasıdır ve her iki dakikada bir çalışır.

* **Kullanıcı deneyimi**. Kullanıcıların oturum açma deneyimini geliştirmek için parola karma eşitlemesi ile sorunsuz SSO dağıtın. Kullanıcılar oturum açtığında sorunsuz SSO gereksiz istemleri ortadan kaldırır.

* **Gelişmiş senaryoları**. Kuruluşlar seçerseniz, Azure AD Identity Protection raporlarla kimlikleri ilişkin bilgiler kullanmak da mümkündür. Sızan kimlik bilgileri rapor olarak buna bir örnektir. İş için Windows Hello olduğu sahip başka bir seçenek [parola karması eşitlemesi kullandığınızda belirli gereksinimleri](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). 

    Parola karma eşitlemesi ile çok faktörlü kimlik doğrulaması gerektiren kuruluşların, Azure AD çok faktörlü kimlik doğrulaması kullanmanız gerekir. Kuruluşlar, üçüncü taraf veya şirket içi çok faktörlü kimlik doğrulama yöntemleri kullanamazsınız.

* **İş sürekliliği**. Parola karma eşitlemesi ile bulut kimlik doğrulaması kullanarak tüm Microsoft veri merkezleri için ölçeklenebilir bir bulut hizmeti yüksek oranda kullanılabilir. Parola karma eşitlemesi uzun süreler ermez emin olmak için hazırlama modunda bekleme yapılandırmasında ikinci bir Azure AD Connect sunucusu dağıtın.

* **Dikkat edilecek noktalar**. Şu anda parola karması eşitlemesi hemen şirket içi hesap durumları değişiklikleri zorunlu değildir. Bu durumda, bir kullanıcının kullanıcı hesabı durumunu Azure AD ile eşitlenene kadar bulut uygulamalarında erişimi vardır. Kuruluşlar, yöneticiler şirket içi kullanıcı hesabı durumları güncelleştirmeleri toplu sonra yeni bir eşitleme döngüsü çalıştırarak bu sınırlamanın üstesinden gelmek isteyebilirsiniz. Bir örnek hesapları devre dışı bırakılmalıdır.

> [!NOTE]
> Parolanın süresi doldu ve hesap kilitli durumları şu anda Azure AD Connect ile Azure AD'ye eşitlenen değil. 

Başvurmak [parola karması eşitlemesi uygulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) dağıtım adımları için.

### <a name="cloud-authentication-pass-through-authentication"></a>Bulut kimlik doğrulaması: doğrudan kimlik doğrulama  

* **Efor**. Geçişli kimlik doğrulaması için bir veya daha fazla (öneririz üç) ihtiyacınız var olan sunucularda yüklü basit aracıları. Bu aracıları, şirket içi Active Directory etki alanı, şirket içi dahil olmak üzere hizmetlerine erişiminiz olması gerekir AD etki alanı denetleyicileri. Bunlar, giden Internet erişimi ve etki alanı denetleyicilerine erişimi gerekir. Bu nedenle, onu bir çevre ağında aracılarını dağıtmak için desteklenmiyor. 

    Doğrudan kimlik doğrulaması Kısıtlanmamış ağ etki alanı denetleyicilerine erişim gerektirir. Tüm ağ trafiğini şifrelenmiş ve kimlik doğrulama isteklerini sınırlıdır. Bu işlem hakkında daha fazla bilgi için bkz: [güvenlik derinlemesine](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) doğrudan kimlik doğrulaması.

* **Kullanıcı deneyimi**. Kullanıcıların oturum açma deneyimini geliştirmek için doğrudan kimlik doğrulama ile sorunsuz SSO dağıtın. Kullanıcı oturum açtıktan sonra sorunsuz SSO gereksiz istemleri ortadan kaldırır.

* **Gelişmiş senaryoları**. Doğrudan kimlik doğrulaması oturum açma zaman şirket içi hesap ilkesi zorlar. Durum devre dışıysa, bir şirket içi kullanıcının hesap kilitli, örneğin, erişim engellenir veya [parolanın süresi](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication) veya ne zaman kullanıcı izin verilir oturum saatleri dışında döner. 

    Doğrudan kimlik doğrulama ile çok faktörlü kimlik doğrulaması gerektiren kuruluşların Azure çok faktörlü kimlik doğrulama (MFA) kullanmanız gerekir. Kuruluşlar, bir üçüncü taraf veya şirket içi çok faktörlü kimlik doğrulama yöntemini kullanamazsınız. Gelişmiş Özellikler, geçişli kimlik doğrulaması seçtiğiniz olup olmadığına bakılmaksızın parola karması eşitlemesi dağıtılır gerektirir. Bir örnek kimliği koruma sızan kimlik bilgileri rapor eder.

* **İş sürekliliği**. İki ek doğrudan kimlik doğrulama aracı dağıtmanızı öneririz. Azure AD Connect sunucusu ilk aracısında yanı sıra bu ek özellikler var. Bu ek dağıtım kimlik doğrulama isteklerinin yüksek kullanılabilirlik sağlar. Dağıtılan üç aracıları içerdiğinde, bakım için başka bir aracı aşağı olduğunda, bir aracı yine başarısız olabilir. 

    Parola karma eşitlemesi doğrudan kimlik doğrulama ek olarak dağıtmak için başka bir faydası yoktur. Birincil kimlik doğrulama yöntemini artık kullanılabilir olduğunda yedek kimlik doğrulama yöntemi olarak davranır.

* **Dikkat edilecek noktalar**. Doğrudan kimlik doğrulama için bir yedek kimlik doğrulama yöntemi olarak parola karması eşitlemesi kullanabilir ve aracıları kullanıcının kimlik bilgileri doğrulanamıyor. Ardından bir parola karması eşitlemesi için yük devretme otomatik olarak gerçekleşmez. Oturum açma yöntemi, Azure AD Connect kullanarak el ile geçiş. 

    Doğrudan kimlik doğrulaması yalnızca modern kimlik doğrulaması ve belirli Exchange Online protokoller kullanan bulut uygulamaları destekler. Bazı, ActiveSync, POP3 ve IMAP4 protokollerdir. Örneğin, Microsoft Office 2013 ve üzeri desteği modern kimlik doğrulaması, ancak önceki sürümlerinde yoktur. Office uygulama desteği hakkında daha fazla bilgi için bkz: [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/). Doğrudan kimlik doğrulaması alternatif kimlik dahil olmak üzere, diğer konuları desteklemek için bkz: [sık sorulan sorular](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq).

Başvurmak [doğrudan kimlik doğrulama uygulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) dağıtım adımları için.

### <a name="federated-authentication"></a>Federe kimlik doğrulaması

* **Efor**. Bir Federasyon kimlik doğrulama sistemi kullanıcıların kimliklerini doğrulamak için bir dış güvenilen sistemine bağlıdır. Bazı şirketler, var olan Federasyon sistem yatırımlarını kendi Azure AD karma kimlik çözümü ile yeniden kullanmak istiyorum. Bakım ve Federasyon sisteminin yönetimi, Azure AD denetimi dışında döner. Kuruluş güvenli bir şekilde dağıtıldıktan ve kimlik doğrulama yükü işleyebilir emin olmak için Federasyon sistemini kullanarak olmasından. 

* **Kullanıcı deneyimi**. Federe kimlik doğrulaması kullanıcı deneyimini özellikleri, topoloji ve Federasyon grubu yapılandırmasının mantığınız bağlıdır. Bazı kuruluşlarda uyum ve güvenlik gereksinimlerine uyacak şekilde Federasyon grubu erişimi yapılandırmak için bu esneklik gerekir. Örneğin, kullanıcılar, otomatik olarak, kimlik bilgileri olmadan oturum dahili olarak bağlı kullanıcıları ve aygıtları Yapılandır mümkündür. Bunlar bir zaten cihazlarına oturum çünkü bu yapılandırma çalışır. Gerekirse, bazı gelişmiş güvenlik özellikleri kullanıcıların oturum açma işlemini daha zor hale.

* **Gelişmiş senaryoları**. Azure AD yerel olarak desteklemeyen bir kimlik doğrulama gereksinimini müşterilerin sahip bir federe kimlik doğrulama çözümü genellikle gereklidir. Yardımcı olmak için ayrıntılı bilgi [sağ oturum açma seçeneği](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/). Aşağıdaki ortak gereksinimlerini dikkate alın:

    * Akıllı kart veya sertifika gerektiren kimlik doğrulaması.
    * Şirket içi MFA sunucuları veya üçüncü taraf çok faktörlü sağlayıcıları.
    * Üçüncü taraf kimlik doğrulama çözümleri kullanarak kimlik doğrulaması. Bkz: [Azure AD Federasyonu uyumluluk listesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Oturum açma, gerektiren bir sAMAccountName, örneğin, etki alanı\kullanıcı adı, yerine bir kullanıcı asıl adı (UPN), örneğin, user@domain.com.

* **İş sürekliliği**. Federasyon sistemleri genellikle bir grup olarak bilinen sunucular, yük dengeli bir dizi gerektirir. Bu grubun bir iç ağ ve kimlik doğrulama istekleri için yüksek kullanılabilirlik sağlamak için çevre ağ topolojisi içinde yapılandırılır.

    Birincil kimlik doğrulama yöntemini artık kullanılabilir olduğunda parola karması eşitlemesi federe kimlik doğrulaması ile birlikte bir yedek kimlik doğrulama yöntemi olarak dağıtın. Şirket içi sunucular kullanılamaz olduğunda bir örnektir. Bazı büyük ölçekli kuruluşların coğrafi DNS düşük gecikme süreli kimlik doğrulama istekleri için yapılandırılmış birden fazla Internet giriş noktaları desteklemek için bir Federasyon çözümü gerektirir.

* **Dikkat edilecek noktalar**. Federasyon sistemleri genellikle şirket içi altyapı daha önemli yatırım gerektirir. Çoğu kuruluş bir şirket içi Federasyon yatırım zaten varsa bu seçeneği belirleyin. Ve tek kimlik sağlayıcısı kullanmak için güçlü iş gereksinimi varsa. Federasyon, bulut kimlik doğrulaması çözümleri için çalışır ve sorun gidermek için daha karmaşık karşılaştırıldığında ' dir.

Azure AD'de doğrulanamayan bir nonroutable etki alanı için kullanıcı kimliği oturum açma uygulamak için ek yapılandırma gerekir. Bu gereksinim alternatif oturum açma kimliği desteği denir. Bkz: [alternatif oturum açma Kimliğini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) kısıtlamalar ve gereksinimler için. Bir üçüncü taraf çok faktörlü kimlik doğrulama sağlayıcısı ile Federasyon kullanmayı seçerseniz, aygıtların Azure AD katılım olanak tanımak için WS-Trust sağlayıcıya desteklediğinden emin olun.

Başvurmak [federasyon sunucuları dağıtma](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) dağıtım adımları için.

> [!NOTE] 
> Azure AD karma kimlik çözümü dağıttığınızda, Azure AD Connect desteklenen topolojiler birini uygulamanız gerekir. Yaklaşık desteklenen ve desteklenmeyen yapılandırmalar, daha fazla bilgi edinin [Azure AD Connect için topolojiler](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Mimarisi diyagramları

Aşağıdaki diyagramlarda, Azure AD karma kimlik çözümü ile kullandığınız her kimlik doğrulama yöntemi için gereken üst düzey mimari bileşenleri ana hatlarını vermektedir. Bunlar çözümleri arasındaki farklılıklar karşılaştırmanıza yardımcı olmak için genel bir bakış sağlar.

* Bir parola karması eşitlemesi çözüm Basitlik:

    ![Parola karma eşitlemesi ile Azure AD karma kimlik](media/azure-ad/azure-ad-authn-image2.png)

* Doğrudan kimlik doğrulama Aracısı gereksinimleri:

    ![Doğrudan kimlik doğrulaması ile Azure AD karma kimlik](media/azure-ad/azure-ad-authn-image3.png)

* Çevre ve iç ağ, kuruluşunuzun Federasyon için gereken bileşenler:

    ![Federe kimlik doğrulaması ile Azure AD karma kimlik](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-methods"></a>Yöntem karşılaştırma

|Değerlendirme|Parola karma eşitlemesi + sorunsuz SSO|Doğrudan kimlik doğrulama + sorunsuz SSO|AD FS ile Federasyon|
|:-----|:-----|:-----|:-----|
|Kimlik doğrulama burada oluşuyor?|Bulutta|Güvenli parola doğrulaması exchange şirket içi kimlik doğrulama Aracısı ile sonra bulutta|Şirket içi|
|Sağlama sistem ötesinde şirket içi sunucu gereksinimleri nelerdir: Azure AD Connect?|None|Her ek kimlik doğrulama aracısı için bir sunucu|İki veya daha fazla AD FS sunucuları<br><br>Çevre/çevre ağında iki veya daha fazla WAP sunucuları|
|Sağlama sistem ötesinde ağ ve şirket içi Internet için gereksinimler nelerdir?|None|[Giden Internet erişimi](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start) sunucularından kimlik doğrulama aracılar çalışan|[Internet erişimi gelen](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-requirements) çevredeki WAP sunucuları<br><br>AD FS sunucuları çevredeki WAP sunucularından gelen ağ erişimi<br><br>Ağ Yükü Dengeleme|
|Bir SSL sertifikası gereksinimi var mı?|Hayır|Hayır|Evet|
|Bir sistem durumu izleme çözümü var mı?|Gerekli değil|Aracı durumu tarafından sağlanan [Azure Active Directory Yönetim Merkezi](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication)|[Azure AD Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs)|
|Kullanıcılar, şirket ağı içinde etki alanına katılmış aygıtlardan bulut kaynaklarına çoklu oturum açma alma?|Evet ile [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Evet ile [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Evet|
|Hangi oturum açma türleri desteklenir?|UserPrincipalName + parola<br><br>Kullanarak Windows tümleşik kimlik [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)|UserPrincipalName + parola<br><br>Kullanarak Windows tümleşik kimlik [sorunsuz SSO](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq)|UserPrincipalName + parola<br><br>sAMAccountName + parola<br><br>Tümleşik Windows Kimlik Doğrulaması<br><br>[Sertifika ve akıllı kart kimlik doğrulaması](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Windows Hello desteklenen iş için mi?|[Anahtar güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Intune ile sertifika güven modeli](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Anahtar güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Intune ile sertifika güven modeli](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Anahtar güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Sertifika güven modeli](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Çok faktörlü kimlik doğrulama seçenekleri nelerdir?|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)<br><br>[Azure MFA sunucusu](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[Üçüncü taraf MFA](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)|
|Hangi kullanıcı hesabı durumları destekleniyor mu?|Devre dışı bırakılan hesapları<br>(en fazla 30 dakikalık gecikme)|Devre dışı bırakılan hesapları<br><br>Hesap Kilitlendi<br><br>Parolanın süresi dolsun<br><br>Oturum açma saatleri|Devre dışı bırakılan hesapları<br><br>Hesap Kilitlendi<br><br>Parolanın süresi dolsun<br><br>Oturum açma saatleri|
|Koşullu erişim seçenekleri nelerdir?|[Azure AD koşullu erişim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD koşullu erişim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD koşullu erişim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[AD FS talep kuralları](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Desteklenen eski protokolleri engelliyor?|[Evet](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Evet](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Evet](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Logosu, resmi ve oturum açma sayfalarındaki açıklama özelleştirebilir miyim?|[Evet, Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Evet, Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Evet](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Hangi Gelişmiş senaryolar desteklenir?|[Akıllı parola kilitleme](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-secure-passwords)<br><br>[Kimlik bilgileri raporları sızmasını](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-risk-events)|[Akıllı parola kilitleme](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Çok siteli düşük gecikme süreli kimlik doğrulama sistemi<br><br>[AD FS extranet kilitleme](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection)<br><br>[Üçüncü taraf kimlik sistemleriyle tümleştirme](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

## <a name="recommendations"></a>Öneriler
Kimlik sistemi bulut uygulamaları ve geçirmek ve bulutta kullanılabilir hale satır iş kolu uygulamaları, kullanıcılarınızın erişim sağlar. Yetkili kullanıcıların üretken ve kötü aktörleri, kuruluşunuzun hassas verileri dışında tutmak için kimlik doğrulama uygulamalara erişimi denetler.

Kullanın veya aşağıdaki nedenlerle seçin hangi kimlik doğrulama yöntemi için parola karma eşitlemesini etkinleştir:

1. **Yüksek kullanılabilirlik ve olağanüstü durum kurtarma**. Doğrudan kimlik doğrulama ve Federasyon içi altyapınızı kullanır. Doğrudan kimlik doğrulama için doğrudan kimlik doğrulama ağ aracıları gerektirir ve şirket içi ayak sunucu donanımını içerir. Federasyon için şirket içi ayak izini daha büyüktür. Proxy kimlik doğrulama isteklerini, çevre ağındaki sunucuları ve iç federasyon sunucuları gerektirir. 

    Tek nokta hataları önlemek için yedek sunucuları dağıtın. Herhangi bir bileşenin başarısız olursa kimlik doğrulama istekleri her zaman verilmeyecek. Doğrudan kimlik doğrulama ve Federasyon etki alanı denetleyicilerinde de başarısız olabilir kimlik doğrulama isteklerine yanıt vermek için de kullanır. Bu bileşenlerin çoğu sağlıklı kalmak için bakım gerekir. Bakım değil ve planlanan doğru uygulanan kesintilerine karşı daha yüksektir. Microsoft Azure AD bulut kimlik doğrulama hizmeti genel olarak ölçeklendirir ve her zaman kullanılabilir olduğu için parola karma eşitlemesi kullanarak kesintileri kaçının.

2. **Şirket içi kesinti hayatta**.  Şirket içi kesinti siber saldırı veya olağanüstü durum nedeniyle sonuçlarını reputational marka zarar bir saldırı ile mücadele etmek mümkün paralyzed kuruluş arasında değişen önemli olabilir. Son olarak, çoğu kuruluş gidin, şirket içi sunucularına neden hedeflenen yazılımı dahil olmak üzere kötü amaçlı yazılım saldırılarını kurbanı yoktu. Microsoft, bu tür saldırılar Dağıt müşterilerin yardımcı olur, kuruluşların iki kategoriye görür:

   * Parola karma eşitlemesini önceden açık kuruluşların parola karma eşitlemesi kullanmak için kendi kimlik doğrulama yöntemini değiştirildi. Bunlar birkaç saat içinde tekrar çevrimiçi. Office 365 e-posta erişimi kullanarak sorunları çözün ve diğer bulut tabanlı iş yüklerini erişmek için çalıştıkları.

   * Daha önce parola karma eşitlemesini etkinleştirmek kaydetmedi kuruluşlar güvenilmeyen dış tüketici e-posta sistemleri iletişimleri ve çözümleme sorunları için çözümlemelere gerekiyordu. Bu durumda, bunları hafta veya yeniden çalışır olması için daha fazla sürdü.

3. **Kimlik koruması**. Kullanıcıların bulut korumak için en iyi şekilde Azure AD Identity Protection biridir. Microsoft Internet kullanıcı için sürekli tarar ve parola hatalı aktörlerin satmak ve koyu Web'de kullanılabilir hale listeler. Azure AD, herhangi bir kullanıcı adları ve parolalar, kuruluşunuzda tehlikeye olmadığını doğrulamak için bu bilgileri kullanabilirsiniz. Bu nedenle hangi kimlik doğrulama yöntemi, Federasyon olup olmadığını, kullanın veya doğrudan kimlik doğrulama olsun parola karma eşitlemesini etkinleştirmek için önemlidir. Sızan kimlik bilgileri, bir rapor olarak sunulur. Engelleme veya kullanıcıları oturum sızan parolaları imzalamak çalıştıklarında kullanıcıların parolalarını değiştirmeye zorlamak için bu bilgileri kullanın.

Son olarak, aşağıdakilere göre [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), Microsoft, kimlik ve erişim yönetimi işlevleri en tam özellikli kümesi sahiptir. Microsoft tanıtıcıları [450 milyar kimlik doğrulama isteklerini](https://www.microsoft.com/en-us/security/intelligence-report) binlerce SaaS uygulamasına Office 365 gibi neredeyse her aygıttan erişim sağlamak için her ay. 

## <a name="conclusion"></a>Sonuç

Bu makalede, kuruluşlar, yapılandırma ve bulut uygulamalarında erişimini desteklemek üzere dağıtma çeşitli kimlik doğrulama seçenekleri özetlenmektedir. Çeşitli iş, güvenlik ve teknik gereksinimleri karşılamak için kuruluşların parola karması eşitlemesi, geçişli kimlik doğrulaması ve Federasyon arasında seçebilirsiniz. 

Her kimlik doğrulama yöntemini göz önünde bulundurun. Çözüm ve kullanıcı deneyimi oturum açma işleminin dağıtmak için iş gereksinimlerinizi adres çaba mu? Gelişmiş senaryolar ve iş devamlılığı özellikleri her kimlik doğrulama yönteminin kuruluşunuzun ihtiyacı olup olmadığını değerlendirin. Son olarak, her kimlik doğrulama yönteminin dikkat edilecek noktaları değerlendirin. Bunlardan herhangi birinin tercih ettiğiniz uygulama önlemek?

## <a name="next-steps"></a>Sonraki adımlar

Günümüzün dünyada tehditleri günde 24 saat mevcut olan ve her yerde gelir. Doğru kimlik doğrulama yöntemi uygulama ve güvenlik riskleri azaltmak ve kimliklerinizi koruyun.

[Başlama](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) Azure AD ile ve kuruluşunuz için doğru kimlik doğrulama çözümü dağıtın.

Geçiş hakkında düşünüyorsanız federe kimlik doğrulaması bulut, daha fazla bilgi edinmek için [oturum açma yöntemini değiştirme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Geçiş planlamanızı ve yardımcı olması için kullanın [bunlar dağıtım planları proje](http://aka.ms/deploymentplans).
