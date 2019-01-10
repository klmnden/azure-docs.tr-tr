---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması için Azure AD Federasyon seçeneğinden geçiş | Microsoft Docs'
description: Karma kimlik ortamınızı geçişli kimlik doğrulaması için Federasyon seçeneğinden taşıma konusunda bilgi.
services: active-directory
author: billmath
manager: mtillman
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: c7d236769d5e9adca0402affc2d0eccdf78a6837
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107761"
---
# <a name="migrate-from-federation-to-pass-through-authentication-for-azure-ad"></a>Geçişli kimlik doğrulaması için Azure AD için Federasyon seçeneğinden geçirme
Aşağıdaki belge, AD FS'den doğrudan kimlik doğrulama için taşıma rehberlik sağlar.

>[!NOTE]
>İndirilebilir bir kopyasını bu belgenin kullanılabilir [burada](https://aka.ms/ADFSTOPTADPDownload).

## <a name="prerequisites-for-pass-through-authentication"></a>Geçişli kimlik doğrulaması için Önkoşullar
Geçişi yapmadan önce aşağıdaki önkoşullar gereklidir.
### <a name="update-azure-ad-connect"></a>Güncelleştirme Azure AD'ye bağlanma

Başarılı bir şekilde doğrudan kimlik doğrulamaya geçiş adımlarını gerçekleştirmek için minimum olarak olmalıdır [Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Bu sürümü, oturum açma dönüştürme yapılır ve dakika potansiyel olarak saat bulut kimlik doğrulaması Federasyon seçeneğinden geçirmek için toplam süreyi azaltır şekilde önemli değişiklikler içerir.

> [!IMPORTANT]
> Eski belgeleri ve araçları blogları kullanıcı dönüştürme için yönetilen etki alanları federe dönüştürülürken gerekli bir adım olduğunu gösterir. **Kullanıcıların dönüştürme** olan Microsoft, belgeleri ve araçları, bunu yansıtmak için güncelleştirme üzerinde çalıştığından ve artık gerekli değildir.

Azure AD Connect takip en son sürüme güncelleştirmek için [güncelleştirme yönergeleri](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="plan-authentication-agent-number-and-placement"></a>Kimlik doğrulama aracısı sayısı ve yerleşimi planlama

Geçişli kimlik doğrulaması, Azure AD Connect sunucusu ve şirket içi Windows sunucularınızı hafif aracıları dağıtarak gerçekleştirilir. Olabildiğince gecikme süresini azaltmak için Active Directory etki alanı denetleyicilerinize mümkün olduğunca yakın aracılarını yükleyin.

Çoğu müşteri, iki veya üç kimlik doğrulaması için aracıları yüksek kullanılabilirlik ve kapasite için yeterli olan ve bir kiracı en fazla 12 aracıları kayıtlı olabilir. İlk aracı her zaman AAD Connect sunucusunda yüklüdür. Başvurmak [ağ trafiği tahminleri ve performans rehberi bilgi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-current-limitations) aracı sınırlamalar ve aracı dağıtım seçeneklerini anlamak için.

### <a name="plan-migration-method"></a>Planı geçiş yöntemi

Federe kimlik doğrulamasını PTA ve sorunsuz çoklu oturum açma için geçirmek için iki yöntem vardır. Kullandığınız yöntem, nasıl AD FS özgün yapılandırmasında üzerinde bağlıdır.

- **Azure AD Connect kullanma**. AD FS, ilk olarak Azure AD Connect'i ve geçişli kimlik doğrulaması için değişiklik kullanarak yapılandırıldıysa, *gerekir* Azure AD Connect Sihirbazı gerçekleştirilebilir.

Azure AD Connect kullanarak, Set-MsolDomainAuthentication cmdlet sizin için otomatik olarak kullanıcı oturum açma yöntemini değiştirme ve bu nedenle, tüm Azure AD kiracınızda doğrulanmış Federasyon etki alanlarının unfederating üzerinde denetiminiz yoktur çalıştırır.  
‎  
> [!NOTE]
> Şu anda beklemediğiniz federating tüm yoksayılamaz kullanıcı değiştirdiğinizde, kiracınızın etki alanlarında oturum açma geçişli kimlik doğrulaması için AAD Connect başlangıçta sizin için AD FS'yi yapılandırmak için kullanılan zaman.  
‎
- **PowerShell ile Azure AD kullanarak bağlanma**. Bu yöntem, yalnızca AD FS ilk olarak Azure AD Connect ile değil yapılandırıldığında kullanılabilir. Azure AD Connect Sihirbazı aracılığıyla kullanıcı oturum açma yöntemini değiştirmek yine, ancak otomatik olarak kümesi MsolDomainAuthentication cmdlet sizin için AD FS grubunuzun hiçbir bilincini vardır ve bu nedenle üzerinde tam denetim sahibi olarak çalışmayacaktır, temel fark vardır. Hangi etki alanlarının dönüştürülür ve hangi sırayla.

Hangi yöntemini kullanmanız gerektiğini anlamak için aşağıdaki bölümde bulunan adımları gerçekleştirin.

#### <a name="verify-current-user-sign-in-settings"></a>Geçerli kullanıcı oturum açma ayarlarını doğrulayın

Azure AD Portalı'nda oturum açtıktan sonra geçerli kullanıcı oturum açma ayarlarınızı doğrulayın [ https://aad.portal.azure.com ](https://aad.portal.azure.com/) bir genel yönetici hesabıyla. 

İçinde **kullanıcı oturum** bölümünde, doğrulayın **Federasyon** olduğu **etkin** ve **sorunsuz çoklu oturum açma** ve  **Geçişli kimlik doğrulaması** olan **devre dışı bırakılmış**. 

![Resim 5](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image1.png)

#### <a name="verify-how-federation-was-configured"></a>Federasyon nasıl yapılandırılan doğrulayın

   1. Azure AD Connect sunucusu ve Azure AD Connect başlatma gidin ve ardından **yapılandırma**.
   2. Üzerinde **ek görevler** ekranındayken **geçerli yapılandırmayı görüntüleme** seçip **sonraki**.</br>
   ![Resim 31](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image2.png)</br>
   3. İçinde **çözümünüzü İnceleme** ekran, aşağı kaydırarak **Active Directory Federasyon Hizmetleri (AD FS)**.</br>
   Bu bölümde AD FS yapılandırmadır sonra AD FS ile Azure AD Connect başlangıçta yapılandırıldı ve bu nedenle yönetilen, yönetilenden dönüştürülmesi Federasyon güvenli bir şekilde varsayabilirsiniz görürseniz Azure AD Connect temelli  **Kullanıcı oturum açma değiştirmek** seçeneği, bu işlem bölümde ayrıntılı olarak verilmiştir **1. seçenek: Azure AD Connect kullanarak geçişli kimlik doğrulaması yapılandırma**.  
‎
   4. Active Directory Federasyon Hizmetleri geçerli ayarları listelenen göremez sonra bölümünde ayrıntılı PowerShell aracılığıyla yönetilen Federasyon oluşturan etki alanlarından el ile dönüştürmeniz gerekir **seçeneği 2 - Federasyon anahtarına PTA Azure AD Connect ve PowerShell kullanarak**.

### <a name="document-current-federation-settings"></a>Belge geçerli federasyon ayarları

Get-msoldomainfederationsettings komutunu cmdlet'i çalıştırarak geçerli federasyon ayarındaki bulabilirsiniz.

Komut şöyledir:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Örneğin:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```


Federasyon tasarım ve dağıtım belgelerinize, özellikle özelleştirilmiş herhangi bir ayarı doğrulamak **PreferredAuthenticationProtocol**, **SupportsMfa**, ve  **PromptLoginBehavior**.

Ne bu ayarlar altında bulunabilir daha fazla bilgi.

[Active Directory Federasyon Hizmetleri sor = oturum açma parametresi desteği](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> SupportsMfa değeri şu anda "True" olarak ayarlanmışsa, ardından bu 2 Faktörlü zor kullanıcı kimlik doğrulaması akışı eklenmek üzere şirket içi MFA çözümünü kullanıyorsunuz anlamına gelir. Bu Azure AD kimlik doğrulama senaryoları için artık çalışmayacak ve bunun yerine aynı işlevi gerçekleştirmek için Azure mfa'yı (bulut tabanlı) hizmetini kullanarak gerekecek. Dikkatli bir şekilde geçmeden önce MFA gereksinimlerinizi değerlendirin ve nasıl Azure mfa'yı, lisanslama etkileri ve son kullanıcı kayıt işlemini etki alanlarınızı dönüştürmeden önce yararlanacağınızı anladığınızdan emin olun.

#### <a name="backup-federation-settings"></a>Yedekleme Federasyon ayarları

Herhangi bir değişiklik için AD FS grubunuzdaki diğer bağlı olan taraflara bu işlem sırasında yapılır olsa da, geçerli bir geçerli yedek geri yüklenebilir, AD FS grubunun olduğundan emin olmak için önerilir. Ücretsiz Microsoft kullanarak bunu yapabilirsiniz [AD FS hızlı geri yükleme aracı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool). Bu araç, yedekleme ve geri yükleme ya da mevcut bir gruba ya da yeni bir grup için AD FS için kullanılabilir.

Ardından en azından, AD FS hızlı geri yükleme aracını kullanmayı tercih ederseniz, "Microsoft Office 365 kimlik taraf güveni ve ilişkili özel talep kuralları eklemiş olabileceğiniz bağlı olan platformu" vermelisiniz. Aşağıdaki PowerShell örneği bunu yapabilirsiniz

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-ad-fs-usage"></a>Dağıtım hakkında önemli noktalar ve AD FS kullanımı

### <a name="validate-your-current-ad-fs-usage"></a>Geçerli AD FS kullanımınızı doğrula

Federe yönetilen dönüştürmeden önce nasıl AD FS bugün Azure AD/Office 365 ve diğer uygulamalar (bağlı olan taraf güvenleri) için kullanmakta olduğunuz yakın görünmelidir. Özellikle, aşağıdaki tabloda dikkate almanız gerekir:

| Eğer| Ardından |
|-|-|
| AD FS için diğer bu uygulamaları korumak için oluşturacaksınız.| Hem AD FS ve Azure AD kullanma ve son kullanıcı deneyimi sonucunda göz önünde bulundurmanız gerekir. Kullanıcılar, iki kez bazı senaryolarda, bir kez (bunlar nereden SSO ve sonraki sürümlerde diğer uygulamalara Office 365 gibi) Azure ad kimlik doğrulaması gerekebilir ve yeniden yine de AD FS bağlı olan taraf güveni olarak ilişkili tüm uygulamalar için. |
| AD FS yoğun olarak özelleştirilmiş ve Azure AD'de yinelenen onload.js dosyasındaki belirli özelleştirme ayarları sayfalarında (kullanıcılar yalnızca bir SamAccountName biçimi için kullanıcı adı olarak girin, örneğin, oturum açma deneyimini değiştirdiniz bir UPN veya yoğun markalı oturum açma deneyimi)| Devam etmeden önce Azure AD tarafından geçerli özelleştirme gereksinimlerinizi karşılayabilirsiniz doğrulamanız gerekir. Daha fazla bilgi ve yönergeler için AD FS markası ve AD FS özelleştirmesi bölümlerine bakın.|
| AD FS aracılığıyla eski kimlik doğrulama istemcilerini engelliyor.| AD FS ile birlikte üzerinde mevcut sürümden eski bir kimlik doğrulama istemcilerin engellenip engellenmeyeceğini denetimleri değiştirmeyi göz önüne alın [koşullu erişim için eski bir kimlik doğrulama denetimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) ve [Exchange Online istemci erişimi Kuralları](https://aka.ms/EXOCAR). |
| Kullanıcıların AD FS'ye kimlik doğrulaması yapılırken bir şirket içi MFA sunucusu çözümü karşı MFA gerçekleştirmesini gerektirir.| Bir kez etki alanı ileride dönüştürülür Bunu yapmak için Azure MFA hizmetinde kullanabilirsiniz ancak bir MFA testini şirket içi MFA çözüm aracılığıyla yönetilen bir etki alanı için kimlik doğrulaması akışı eklemesine mümkün olmayacaktır. Daha sonra kullanıcıları Azure mfa'yı bugün kullanmıyorsanız bu hazırlayın ve iletişim kurmak için son kullanıcılarınızın olacak onetime son kullanıcı kayıt adımı içerir. |
| Erişim denetim ilkeleri (AuthZ kuralları) AD FS, Office 365 erişimi denetlemek için kullandığınız.| Bu eşdeğer Azure AD ile değiştirmeyi göz önüne alın [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) ve [Exchange Online istemci erişim kuralları](https://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Ortak AD FS özelleştirmelerini dikkate alınacak noktalar

#### <a name="inside-corporate-network-claim"></a>İç şirket ağı talep

Kimliği doğrulanan kullanıcının kurumsal ağ içinde değilse InsideCorporateNetwork talep AD FS tarafından verilir. Bu talep Azure AD'ye geçirildi ve kullanıcının ağ konumuna göre çok faktörlü kimlik doğrulamasını atlamak için kullanılır. Bkz: [güvenilen IP'leri Federasyon kullanıcıları için](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) bu AD FS'de şu anda etkin olup olmadığını belirleme hakkında daha fazla bilgi için.

Geçişli kimlik doğrulaması için etki alanlarınızın dönüştürülen InsideCorporateNetwork talep artık kullanılabilir olmayacaktır. [Azure AD'de adlandırılmış Konumlar](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) bu işlevin yerini için kullanılabilir.

Adlandırılmış konumları yapılandırma yapıldıktan sonra tüm koşullu erişim ilkeleri, dahil etmek veya "Tüm Güvenilen Konumlar" ağ konumları hariç tutmak için yapılandırılmış veya "MFA güvenilen IP'ler" adlı yeni oluşturulan konumlarını gösterecek şekilde güncelleştirilmesi gerekir.

Bkz: [Active Directory koşullu erişimi konumlarındaki](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) koşullu erişimde konum koşulu hakkında daha fazla bilgi için.

#### <a name="hybrid-azure-ad-joined-devices"></a>Karma Azure AD alanına katılmış aygıtlar

Bir cihaz için Azure AD'ye katılımı, güvenlik ve uyumluluk için erişim standartlarınızı karşılayan cihazların zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar ve bir cihaza bir kurumsal iş veya Okul hesabı yerine kişisel kullanarak oturum açmalarını sağlar hesabı. Hibrit Azure AD'ye katılmış cihazların, AD etki alanına katılan cihazları Azure AD'ye sağlar. Bu özellik ile Federasyon ortamınızı yapılandırılmış olabilir.

Karma katılın yeni cihazlar için çalışma, geçişli kimlik doğrulaması için etki alanlarınızın üzere dönüştürdükten sonra etki alanına katılmış devam edebilmesini sağlamak üzere Azure AD Connect için Windows 10 istemcileri için Active Directory bilgisayar hesaplarını eşitlemek üzere yapılandırılmalıdır Azure AD. Windows 7 ve Windows 8 bilgisayar hesapları için karma katılın sorunsuz çoklu oturum açma bilgisayar Azure AD'ye kaydetme kullanır ve Windows 10 cihazları için yaptığınız gibi bunları eşitleyebilirsiniz gerekmez. Kendilerini sorunsuz çoklu oturum açma kullanarak kaydetmek için ancak bir güncelleştirilmiş workplacejoin.exe dosyasını (aracılığıyla bir .msi) dağıtmak bu alt düzey istemciler için gerekir. [.msi indirme](https://www.microsoft.com/download/details.aspx?id=53554).

Bu gereksinim hakkında daha fazla bilgi için bkz. [karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup)

#### <a name="branding"></a>Markalama

Kuruluşunuzun olabilir [, AD FS oturum sayfalarının özelleştirilmiş](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) kuruluşa daha ilgili bilgileri görüntülemek için. Bu durumda, benzer yapmayı göz önüne alın [Azure AD oturum açma sayfası özelleştirmeleri](https://docs.microsoft.com/azure/active-directory/customize-branding).

Benzer özelleştirmeleri kullanılabilir, ancak bazı görsel değişiklikler beklenmelidir. Beklediğiniz değişikliklerin son kullanıcılar, iletişim eklemek isteyebilirsiniz.

> [!NOTE]
> Şirket markası, yalnızca Azure AD Premium veya temel lisansı satın veya Office 365 lisansınız varsa kullanılabilir.

## <a name="planning-for-smart-lockout"></a>Akıllı kilitleme için planlama

Azure AD akıllı kilitleme, parola deneme yanılma saldırılarına karşı korur ve şirket içi Active Directory hesabı geçişli kimlik doğrulaması kullanılır ve hesap kilitleme Grup İlkesi Active Directory'de ayarlandığında kilitlenmelerini önler. 

Daha fazla bilgi için [akıllı kilitleme özelliğini ve yapılandırmasını düzenlemek nasıl](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout).

## <a name="planning-deployment-and-support"></a>Dağıtım planlama ve Destek

### <a name="plan-the-maintenance-window"></a>Bakım penceresi planlama

Etki alanına dönüştürme işlemini kendisi görece hızlı olsa da, Azure AD yine de bazı kimlik doğrulama isteklerini, AD FS sunucuları için bir etki alanına dönüştürme işlemini tamamladıktan sonra en fazla 4 saat boyunca gönderebilir. Bu dört saatlik pencerede sırasında ve çeşitli hizmet tarafı önbellekler bağlı olarak, bu kimlik doğrulamaları Azure AD tarafından kabul edilmeyebilir ve kullanıcılar, bir hata alırsınız, AD FS hala karşı kimlik doğrulaması başarılı mümkün olacaktır, ancak Azure AD'ye artık kabul edeceği bir Bu Federasyon güveni şimdi kaldırıldı olarak kullanıcının belirteç.

> [!NOTE]
> Bu hizmet tarafı önbellek temizlenene kadar bu post dönüştürme penceresi sırasında hizmetler bir tarayıcı üzerinden erişen kullanıcılar etkisi olur. Kullanıcı sessizce AD FS'ye geri gitmek zorunda kalmadan yeniden kimlik doğrulaması için kullanılan bir süre için Exchange Online kimlik bilgilerini önbelleğini korur gibi eski istemciler (Exchange ActiveSync, Outlook 2010/2013) etkilenmeyenler. Bu istemciler için cihazda depolanan kimlik bilgileri, kendilerini sessiz bir şekilde önbelleğe işaretli değildir ve bu nedenle kullanıcıların etki alanı dönüştürme işlemi sonucunda herhangi bir parola istemi etkinleştirilmediğinden yeniden kimlik doğrulaması için kullanılır. Buna karşılık, Modern kimlik doğrulaması istemcileri (Office 2013/2016, IOS ve Android uygulamaları) için bunlar sürekli kaynaklara erişim için AD FS geri gitmeyi yerine yeni erişim belirteçleri almak için bir geçerli yenileme belirteci kullanın ve bu nedenle herhangi bir parola istemi için ayrıcalıklı bir etki alanı dönüştürme sonucunu işlemek ve herhangi bir ek yapılandırma gerekli çalışmaya devam eder.
> [!IMPORTANT]
> Yoksa, AD FS ortamı kapatın veya Office 365 bağlı olan taraf güveni tüm kullanıcılar başarıyla bulut kimlik doğrulamasını kullanarak kimlik doğrulayan doğrulayana kadar kaldırın.

### <a name="plan-for-rollback"></a>Geri alma planlama

Önemli bir sorun bulunursa ve hızlı bir şekilde geri almak karar verebilirsiniz çözümlenemedi, çözüm için Federasyon yedekleyin. Dağıtımınızı planlandığı gibi Git değil durumunda yapmanız gerekenler planlamanız önemlidir. Dağıtım sırasında etki alanı ya da kullanıcılar dönüştürülmesi başarısız veya geri alma Federasyon için gerekirse, herhangi bir kesinti azaltmak ve kullanıcılara etkisini azaltmak nasıl anlamanız gerekir.

#### <a name="rolling-back"></a>Geri alma

Belirli dağıtım ayrıntılarınızı Federasyon tasarım ve dağıtım belgelerinize başvurun. İşlem içermelidir:

* Federasyon için Convert-MSOLDomainToFederated kullanarak yönetilen etki alanlarını dönüştürme 

* Gerekirse, ek yapılandırma kuralları talepleri.

### <a name="plan-change-communications"></a>Değişiklik iletişimi planlama

Dağıtım planlama ve Destek önemli bir parçası son kullanıcılarınızın değişiklikleri ve hangi karşılaşabilirsiniz veya yapmalısınız hakkında proaktif bir şekilde bilgiye dayalı olduğundan emin olmaktır. 

Geçişli kimlik doğrulaması ve sorunsuz çoklu oturum açma hem dağıtıldıktan sonra Office 365 ve diğer erişme ilişkili kaynakları Azure AD kimlik doğrulaması yapılırken son kullanıcı oturum açma deneyimi değişir. Ağa dış kullanıcılar, dış yönelik Web uygulaması Proxy sunucuları tarafından sunulan form tabanlı sayfasına yeniden yönlendirilen aksine Azure AD oturum açma sayfası yalnızca, şimdi görür.

İletişim stratejinizi planlama için birden fazla öğe vardır. Bunlar:

* Yaklaşan ve yayımlanan işlevselliği kullanıcılara bildirme
  * E-posta ve diğer şirket içi iletişim kanallarını
  * Görseller gibi posterler
  * Canlı ya da diğer yönetim iletişimleri
* Kimin özelleştireceğim kullanan iletişimleri gönderir belirlemekten ve ne zaman.

## <a name="implementing-your-solution"></a>Çözümünüzü uygulama

Çözümünüzü planladıktan sonra bu uygulamaya hazır olursunuz. Uygulama aşağıdaki bileşenleri içerir:

   1. Üzerinde sorunsuz çoklu oturum açma için hazırlama
   2. Oturum açma doğrudan kimlik doğrulama yöntemini değiştirme ve sorunsuz çoklu oturum açmayı etkinleştirme

## <a name="step-1--prepare-for-seamless-sso"></a>Sorunsuz çoklu oturum açma için adım 1 – hazırlama

Cihazlarınıza sorunsuz çoklu oturum açmayı kullanmak için bir Azure AD URL'si, Active Directory'de Grup İlkesi'ni kullanarak kullanıcıların Intranet bölgesi ayarlarına eklemeniz gerekir.

Varsayılan olarak, tarayıcı, doğru bölgeyi, Internet veya Intranet, belirli bir URL'den otomatik olarak hesaplar. Örneğin, "http://contoso/"eşler Intranet bölgesine ise"http://intranet.contoso.com/" (URL bir dönemi içerdiğinden) Internet bölgesine eşler. Tarayıcının Intranet bölgesine açıkça URL eklemediğiniz sürece tarayıcılar Kerberos biletleri Azure AD URL'yi gibi bir bulut uç noktasına gönderir.

İzleyin [kullanımına sunmak için adımları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) cihazlarınıza gerekli değişiklikleri.

> [!IMPORTANT]
> Bu değişiklik, kullanıcıların Azure AD'de oturum biçimini değiştirmez. Ancak, bu yapılandırma etmemiş olan cihazlarda oturum açan kullanıcılar yalnızca kullanıcı adı ve Azure AD'de oturum açmak için parola girmeniz gerekir ayrıca adım 3 not ile devam etmeden önce bu yapılandırma tüm cihazlarınıza uygulandıktan önemlidir.

## <a name="step-2--change-sign-in-method-to-pta-and-enable-seamless-sso"></a>2. adım – PTA için oturum açma yöntemini değiştirme ve sorunsuz çoklu oturum açmayı etkinleştir

### <a name="option-a-configuring-pta-by-using-azure-ad-connect"></a>Seçenek A: Azure AD Connect kullanarak PTA yapılandırma

Azure AD Connect kullanarak AD FS'yi başlangıçta yapılandırıldığı durumlarda bu yöntemi kullanın. Azure AD Connect kullanarak AD FS'yi ilk olarak yapılandırılmamış, bu yöntemi kullanamazsınız.

> [!IMPORTANT]
> Tüm etki alanları aşağıdaki adımları izleyerek federe yönetilen dönüştürülecek olduğunu unutmayın. Daha fazla bilgi için geçiş yöntemi planlama bölümüne bakın.[](#_Plan_Migration_Method)

İlk oturum açma yöntemini değiştirmeniz gerekir:

   1. Azure AD Connect sunucusunda sihirbazını açın.
   2. Seçin **değişiklik kullanıcı oturum açma** seçip **sonraki**. 
   3. İçinde **Azure ad Connect** ekran, kullanıcı adı ve bir genel yönetici parolasını sağlayın.
   4. **Kullanıcı oturum açma** ekran, radyo düğmesini Değiştir **AD FS ile Federasyon** için **geçişli kimlik doğrulaması**seçin **çoklu oturum açmayı etkinleştir**  seçip **sonraki**.
   5. Etkinleştirme çoklu oturum açma ekranında, etki alanı yöneticisi hesabının kimlik bilgilerini girin ve ardından İleri'yi seçin.  

   > [!NOTE]
   > Etki alanı yöneticisi kimlik bilgileri, işlemin bu yükseltilmiş izinleri gerektiren aşağıdaki eylemleri gerçekleştirirken sorunsuz çoklu oturum açmayı etkinleştirmek için gereklidir. Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılan ve işlemin başarıyla tamamlanmasından sonra atılır
   >
   > * (Azure AD temsil eden) AZUREADSSOACC adlı bir bilgisayar hesabı, şirket içi Active Directory (AD) oluşturulur.
   > * Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
   > * Ayrıca, Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.
   > * Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılan ve işlemin başarıyla tamamlanmasından sonra atılır

   6. İçinde **yapılandırmaya hazır** ekranında, emin **Yapılandırma tamamlandığında eşitleme başlangıç işlemi** onay kutusunun seçili. Ardından **yapılandırma**.</br>
   ![resmi](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image8.png)</br>
   7. Açık **Azure AD portalında**seçin **Azure Active Directory**ve ardından **Azure AD Connect**.
   8. Doğrulayın **Federasyon devre dışı** sırada **sorunsuz çoklu oturum açma** ve **geçişi kapsamlı bir kimlik doğrulama** olan **etkin**.</br>
   ![resmi](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image9.png)</br>

Sonraki ek kimlik doğrulama yöntemleri dağıtmanız gerekir. Açık **Azure portalında**, Gözat **Azure Active Directory, Azure AD Connect** tıklatıp **geçişli kimlik doğrulaması**.

Gelen **geçişli kimlik doğrulaması** sayfasında, İndir düğmesine tıklayın. Gelen **indirme** aracı ekran, tıklayarak **koşulları kabul et ve indir**.

Ek kimlik doğrulama aracılarının indirilmesi başlar. İkincil kimlik doğrulaması Aracısı, etki alanına katılmış bir sunucuya yükleyin. 

> [!NOTE]
> İlk aracı her zaman Azure AD Connect sunucusunun kendisi kullanıcı oturum bölümünde Azure AD Connect aracı, yapılandırma değişikliklerinin bir parçası olarak yüklenir. Ayrı bir sunucuya ek bir kimlik doğrulama aracılarının yüklenmesi gerekir. 2-3 ek kimlik doğrulama aracılarının arasında kullanılabilir olması önerilir. 

Kimlik Doğrulama Aracı yüklemesini çalıştırın. Yükleme sırasında kimlik bilgilerini sağlamanız gerekir bir **genel yönetici** hesabı.

![Resim 1243067202](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image11.png)

![Resim 1243067210](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image12.png)

Kimlik Doğrulama Aracısı yüklendikten sonra ek aracıların durumunu denetlemek için geçişli kimlik doğrulaması Aracısı sistem durumu sayfasına geri dönebilirsiniz.


Test ve sonraki adımlara geçin.

> [!IMPORTANT]
> Seçenek B: bölümü atlayın Federasyon seçeneğinden için PTA bölümü geçerli değildir, Azure AD Connect ve PowerShell adımları kullanarak geçiş.  

### <a name="option-b---switch-from-federation-to-pta-using-azure-ad-connect-and-powershell"></a>Seçenek B - Azure AD Connect ve PowerShell kullanarak PTA için Federasyon geçiş

Azure AD Connect kullanarak Federasyon başlangıçta yapılandırılmamış olduğunda bu seçeneği kullanın. Öncelikle, geçişli kimlik doğrulamasını etkinleştirmeniz gerekir:

   1. Azure AD Connect sunucusunda sihirbazını açın.
   2. Seçin **değişiklik kullanıcı oturum açma** seçip **sonraki**.
   3. İçinde **Azure ad Connect** ekran, kullanıcı adı ve bir genel yönetici parolasını sağlayın.
   4. Üzerinde **kullanıcı oturum açma** ekran, radyo düğmesini Değiştir **yapılandırmayın** için **geçişli kimlik doğrulaması**seçin **çoklu oturum açmayıEtkinleştir** seçip **sonraki**.
   5. İçinde **etkinleştirme çoklu oturum açma** ekran, etki alanı yöneticisi hesabının kimlik bilgilerini girin ve ardından seçin **sonraki**.

   > [!NOTE]
   > Etki alanı yöneticisi kimlik bilgileri, işlemin bu yükseltilmiş izinleri gerektiren aşağıdaki eylemleri gerçekleştirirken sorunsuz çoklu oturum açmayı etkinleştirmek için gereklidir. Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılır ve Kurulum başarıyla tamamlandıktan sonra atılır.
   >
   > * (Azure AD temsil eden) AZUREADSSOACC adlı bir bilgisayar hesabı, şirket içi Active Directory (AD) oluşturulur.
   > * Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
   > * Ayrıca, Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.

   6. İçinde **yapılandırmaya hazır** ekranında, emin **Yapılandırma tamamlandığında eşitleme başlangıç işlemi** onay kutusunun seçili. Ardından **yapılandırma**.</br>
   ‎![resmi](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image18.png)</br>
   Yapılandırma seçerken aşağıdaki adımlardan oluşur:
   * İlk geçişli kimlik doğrulaması aracısı yüklü
   * Geçiş özelliği etkinleştirilir
   * Sorunsuz çoklu oturum açma etkin.  
   7. Doğrulayın **Federasyon** hala **etkin** ve **sorunsuz çoklu oturum açma** ve **geçişi kapsamlı bir kimlik doğrulama** şimdi etkinleştirilir.
   ![Resim 2](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image19.png)
   8. Seçin **geçişli kimlik doğrulaması** ve durum olduğundan emin olun **etkin**.</br>
   
   Kimlik Doğrulama Aracısı etkin değilse izleyin [Bu sorun giderme adımlarını](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication) sonraki adımda etki alanı konuşma işlemine devam etmeden önce. Etki alanlarınızı doğrulama önce dönüştürürseniz, kimlik doğrulaması kesinti PTA aracılarınızı başarıyla yüklediniz ve bunların durumunu Azure portalında "Etkin" olarak gösterildiğinden emin neden riski oluşur.  
   9. Sonraki ek kimlik doğrulama aracılarının dağıtın. Açık **Azure portalında**, Gözat **Azure Active Directory**, **Azure AD Connect** tıklatıp **geçişli kimlik doğrulaması**.
   10. Gelen **geçişli kimlik doğrulaması** sayfasında, tıklayarak **indirin** düğmesi. Gelen **aracısını indir** ekranında, tıklayarak **koşulları kabul et ve indir**.
   
   Ek kimlik doğrulama aracılarının indirilmesi başlar. İkincil kimlik doğrulaması Aracısı, etki alanına katılmış bir sunucuya yükleyin.

  > [!NOTE]
  > İlk aracı her zaman Azure AD Connect sunucusunun kendisi kullanıcı oturum bölümünde Azure AD Connect aracı, yapılandırma değişikliklerinin bir parçası olarak yüklenir. Ayrı bir sunucuya ek bir kimlik doğrulama aracılarının yüklenmesi gerekir. 2-3 ek kimlik doğrulama aracılarının arasında kullanılabilir olması önerilir.
  
   11. Kimlik Doğrulama Aracı yüklemesini çalıştırın. Yükleme sırasında kimlik bilgilerini sağlamanız gerekir bir **genel yönetici** hesabı.</br>
   ![Resim 1243067215](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image23.png)</br>
   ![Resim 1243067216](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image24.png)</br>
   12. Kimlik Doğrulama Aracısı yüklendikten sonra ek aracıların durumunu denetlemek için geçişli kimlik doğrulaması Aracısı sistem durumu sayfasına geri dönebilirsiniz.

Bu noktada, Federasyon hala etkinse ve operasyonel etki. Dağıtıma devam etmek için her etki alanı geçişli kimlik doğrulaması için etki alanı kimlik doğrulama isteklerine hizmet başlar bu nedenle federe yönetilen dönüştürülmesi gerekiyor.

Tüm etki alanları gereksinim dönüştürülmesi aynı anda Başlarken tercih edebileceğiniz üretim kiracınızın veya etki alanı ile en az bir test etki alanı kullanıcı sayısı.

Dönüştürme, Azure AD PowerShell modülü kullanılarak gerçekleştirilir.

   1. Açık **PowerShell** ve Azure AD kullanarak oturum açma bir **genel yönetici** hesabı.
   2. İlk etki alanı dönüştürmek için aşağıdaki komutu çalıştırın:
 
 ``` PowerShell
 Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
 ```
 
   3. Açık **Azure AD portalında**seçin **Azure Active Directory**ve ardından **Azure AD Connect**.  
   4. Tüm Federasyon etki alanları dönüştürülen doğrulayın **Federasyon devre dışı** sırada **sorunsuz çoklu oturum açma** ve **geçişli kimlik doğrulaması** olan **Etkin**.</br>
   ![resmi](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image26.png)</br>

## <a name="testing-and-next-steps"></a>Test ve sonraki adımlar

### <a name="test-pass-through-authentication"></a>Geçişli kimlik doğrulamasını Sına 

Kiracınızın Federasyon kullanırken, kullanıcıları Azure AD oturum açma sayfasından AD FS ortamınıza yönlendiriliyorsa. Kiracı yerine Federasyon geçişli kimlik doğrulaması kullanmak üzere yapılandırılmış, kullanıcıları AD FS'ye yeniden yönlendirilir değil ve bunun yerine doğrudan Azure AD oturum açma sayfasının oturum açacaksınız.

Internet Explorer otomatik olarak oturum açma sorunsuz SSO önlemek ve Office 365 oturum açma sayfasına gitmek için InPrivate Modu Aç ([https://portal.office.com](https://portal.office.com/)). Tür **UPN** tıklayın ve kullanıcı **sonraki**. Şirket içi Active Directory'nizden eşitlenen ve kimin daha önce Federal karma kullanıcının UPN'sini yazdığınızdan emin olun. Kullanıcının kullanıcı adı ve parola yazmak için bir ekran görürsünüz.

![Resim 18](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image27.png)

![Resim 19](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image28.png)

Parolayı yazın sonra Office 365 portalına yönlendirilen.

![Resim 23](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image29.png)

### <a name="test-seamless-single-sign-on"></a>Sorunsuz çoklu oturum açma testi

   1. Şirket ağına bağlı makineyi oturum açma etki alanına katıldı. 
   2. Açık **Internet Explorer** veya **Chrome** ve aşağıdaki URL'lerden birine gidin:   
      ‎  
      ‎[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (Contoso etki alanınız ile değiştirin).

      Kullanıcı kısa bir süre için Azure AD oturum açma sayfasına yönlendirilir ve ", oturum açmaya çalışırken" iletisini görürsünüz ve bir kullanıcı adı veya parola için sizden.</br>
   ![Resim 24](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image30.png)</br>
   3. Ardından, kullanıcı yönlendirilen ve erişim paneline başarıyla imzalandı:

> [!NOTE]
> Sorunsuz çoklu oturum açma etki alanı İpucu (örneğin, myapps.microsoft.com/contoso.com) destekleyen Office 365 hizmetleri üzerinde çalışır. Office 365 portalı (portal.office.com) şu anda etki alanı ipucu desteklemiyor ve bu nedenle, kullanıcıların UPN yazmanız gerekecektir beklenmektedir. UPN girilen, sorunsuz çoklu oturum açıldığında, kullanıcı adına bir Kerberos anahtarı almak ve bunları bir parola yazmadan oturum.
> [!TIP]
> Dağıtmayı göz önünde bulundurun [Windows 10 Azure AD karma birleştirme](https://docs.microsoft.com/azure/active-directory/device-management-introduction) geliştirilmiş çoklu oturum açma deneyimi için.

### <a name="removal-of-the-relying-party-trust"></a>Bağlı olan taraf güveni kaldırma

Tüm kullanıcılar ve istemcilerin başarıyla aracılığıyla Azure AD kimlik doğrulama yapıyorsunuz olduğunu doğruladıktan sonra Office 365 bağlı olan taraf güveni kaldırmak güvenli düşünülebilir.

AD FS (diğer bağlı olan taraf güvenleri yapılandırılmış) başka bir amaçla kullanılmayan, AD FS şimdi yetkisini almak güvenlidir.

### <a name="rollback"></a>Geri alma

Önemli bir sorun bulunursa ve hızlı bir şekilde geri almak karar verebilirsiniz çözümlenemedi, çözüm için Federasyon yedekleyin.

Belirli dağıtım ayrıntılarınızı Federasyon tasarım ve dağıtım belgelerinize başvurun. İşlem içermelidir:

* Federasyon için Convert-MSOLDomainToFederated kullanarak yönetilen etki alanlarını dönüştürme
* Gerekirse, ek yapılandırma kuralları talepleri.

### <a name="enable-synchronization-of-userprincipalname-updates"></a>UserPrincipalName güncelleştirmeleri eşitlemeyi etkinleştirme

Tarihsel olarak, güncelleştirmeler için eşitleme hizmeti aracılığıyla şirket içi UserPrincipalName özniteliği engellendi, bu koşulların her ikisinin de doğru olduğu sürece:

* Kullanıcı (Federasyon olmayan) yönetilir.
* Kullanıcı bir lisans atanmadı.

Doğrulamak veya bu özelliği etkinleştirmek yönergeler için aşağıdaki makaleye başvurun:

[UserPrincipalName güncelleştirmelerini eşitlemek](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features).

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Sorunsuz SSO Kerberos şifre çözme anahtarını başa döndürmek

Şirket içinde oluşturulan Kerberos şifre çözme anahtarı (Azure AD temsil eden) AZUREADSSOACC bilgisayar hesabının üzerinden sık geri önemlidir AD ormanı. Active Directory etki alanı üyeleri parola değişiklikleri nasıl gönderme ile hizalamak için en azından her 30 gün Kerberos şifre çözme anahtarı alma önemle öneririz. İlişkili cihaz AZUREADSSOACC bilgisayar hesabı nesnesine bağlı olmadığından geçiş işlemini el ile uygulanması gerekir.

Kerberos şifre çözme anahtarının dökümünü üzerinden başlatmak için Azure AD Connect çalıştırdığınız şirket içi sunucusunda bu adımları izleyin.

[Nasıl miyim geri AZUREADSSOACC bilgisayar hesabının Kerberos şifre çözme anahtarı](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

Kimlik doğrulama aracılarının çalıştıran sunucular, çözüm kullanılabilirliği sürdürmek için izlenmelidir. Genel sunucu performans sayaçlarını ek olarak, kimlik doğrulama aracılarının kimlik doğrulaması istatistikleri ve hataları anlamak için kullanılabilir performans nesnesi ortaya çıkarır.

Kimlik doğrulama aracılarının işlemleri Windows olay günlükleri altında "Uygulama ve hizmet Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin" oturum açın.

Sorun giderme günlükleri gerekirse etkinleştirilebilir.

Daha fazla bilgi [izleme ve günlüğe kaydetme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-Pass-through-authentication).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Connect tasarım kavramları](plan-connect-design-concepts.md)
- [Doğru kimlik doğrulamasını seçme](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Desteklenen topolojiler](plan-connect-design-concepts.md)
