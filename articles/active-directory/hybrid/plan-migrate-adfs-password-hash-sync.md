---
title: "Azure AD Connect: Federasyon seçeneğinden parola karması eşitleme için Azure AD'ye geçirme | Microsoft Docs"
description: Karma kimlik ortamınızı Federasyon seçeneğinden parola karması eşitleme için taşıma konusunda bilgi.
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
ms.openlocfilehash: cad368cb968b94d1327cc99ed4dfa6df0aedd2cd
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555107"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-ad"></a>Federasyon seçeneğinden parola karması eşitleme için Azure AD'ye geçirme
Aşağıdaki belge için parola karması eşitleme AD FS'den geçmeden rehberlik sağlar.

>[!NOTE]
>İndirilebilir bir kopyasını bu belgenin kullanılabilir [burada](https://aka.ms/ADFSTOPHSDPDownload).


## <a name="prerequisites-for-the-migration"></a>Geçiş için Önkoşullar 
Geçişi yapmadan önce aşağıdaki önkoşullar gereklidir.
### <a name="update-azure-ad-connect"></a>Güncelleştirme Azure AD'ye bağlanma

Başarılı bir şekilde doğrudan kimlik doğrulamaya geçiş adımlarını gerçekleştirmek için minimum olarak olmalıdır [Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Bu sürümü, oturum açma dönüştürme yapılır ve dakika potansiyel olarak saat bulut kimlik doğrulaması Federasyon seçeneğinden geçirmek için toplam süreyi azaltır şekilde önemli değişiklikler içerir.

> [!IMPORTANT]
> Eski belgeleri ve araçları blogları kullanıcı dönüştürme için yönetilen etki alanları federe dönüştürülürken gerekli bir adım olduğunu gösterir. Kullanıcıların dönüştürme artık gerekli değildir ve Microsoft, belgeleri ve araçları bu yansıtacak şekilde güncelleştirmeyi çalıştığını unutmayın.

Azure AD Connect takip en son sürüme güncelleştirmek için [güncelleştirme yönergeleri](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Parola Karması eşitleme gerekli izinler

Azure AD Connect'i hızlı ayarlar veya özel yükleme kullanılarak yapılandırılabilir. Özel yükleme seçeneğini kullandıysanız [gerekli izinler](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) parola karması eşitleme yerinde olabilir.

Azure AD Connect AD DS hesabı gereksinimlerine parola karmalarını eşitleyecek şekilde aşağıdaki izinlerin hizmet.

* Dizin Değişikliklerini Çoğaltma

* Çoğaltma Directory yapılan tüm değişiklikler

Artık bu doğrulamak için iyi bir zamandır ormandaki tüm etki alanları yürürlükte olan izinler.

### <a name="plan-migration-method"></a>Planı geçiş yöntemi

Parola Karması eşitleme ve sorunsuz çoklu oturum açma için Federasyon kimlik doğrulamasını geçirmek için iki yöntem vardır. Kullandığınız yöntem, nasıl AD FS özgün yapılandırmasında üzerinde bağlıdır. 



- **Seçenek A: Azure AD Connect kullanma**. Azure AD Connect kullanarak AD FS özgün yapılandırmasında, kullanıcı oturum açma yöntemi olarak parola karması eşitlemesi değişikliği Azure AD Connect Sihirbazı gerçekleştirilmelidir.   
Azure AD Connect kullanarak, Set-MsolDomainAuthentication cmdlet sizin için otomatik olarak kullanıcı oturum açma yöntemini değiştirme ve bu nedenle, tüm Azure AD kiracınızda doğrulanmış Federasyon etki alanlarının unfederating üzerinde denetiminiz yoktur çalıştırır.

> [!NOTE]
> Şu anda AAD Connect özgün AD FS sizin için yapılandırmak amacıyla kullanılan, kullanıcı oturum açma için parola karması eşitleme değiştirdiğinizde, kiracınızdaki tüm etki alanları unfederating kaçınamazsınız.  



- **Seçenek B: PowerShell ile Azure AD kullanarak bağlanma**. Bu yöntem, yalnızca AD FS ilk olarak Azure AD Connect ile değil yapılandırıldığında kullanılabilir. Azure AD Connect Sihirbazı aracılığıyla kullanıcı oturum açma yöntemini değiştirmek yine, ancak otomatik olarak kümesi MsolDomainAuthentication cmdlet sizin için AD FS grubunuzun hiçbir bilincini vardır ve bu nedenle üzerinde tam denetim sahibi olarak çalışmayacaktır, temel fark vardır. Hangi etki alanlarının dönüştürülür ve hangi sırayla.

Hangi yöntemini kullanmanız gerektiğini anlamak için aşağıdaki bölümde bulunan adımları gerçekleştirin.

#### <a name="verify-current-user-sign-in-settings"></a>Geçerli kullanıcı oturum açma ayarlarını doğrulayın

Azure AD Portalı'nda oturum açtıktan sonra geçerli kullanıcı oturum açma ayarlarınızı doğrulayın [ https://aad.portal.azure.com ](https://aad.portal.azure.com/) bir genel yönetici hesabıyla.

Kullanıcı oturum bölümünde, Federasyon etkin olduğundan ve sorunsuz çoklu oturum açma ve geçişli kimlik doğrulaması devre dışı olduğunu doğrulayın. Ayrıca, bu daha önce açıldı sürece bu devre dışı göstermelidir parola eşitleme durumunu doğrulayın.

![Resim 5](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-azure-ad-connect-configuration"></a>Azure AD Connect yapılandırmasını doğrula

   1. Azure AD Connect sunucunuza gidin ve Azure AD Connect başlatın ve ardından Yapılandır'ı seçin. 
   2. Ek görevler ekranında, geçerli yapılandırmayı görüntüleme seçin ve ardından İleri'yi seçin.</br>
   ![Resim 31](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)</br>
   
   3. Çözümünüzü İnceleme ekranında, parola eşitleme durumu not alın.</br> 

   Parola Karması eşitleme şu anda devre dışı olarak ayarlarsanız, etkinleştirmek için bu kılavuzdaki adımları izlemeniz gerekir. Parola Karması eşitleme şu anda etkin olarak ayarlanırsa, güvenli bir şekilde bölümü atlayabilirsiniz [1. adım: parola karması eşitlemeyi etkinleştir](#step-1--enable-password-hash-synchronization) bu kılavuzdaki.
   4. Çözümünüzü İnceleme ekranda Active Directory Federasyon Hizmetleri (AD FS) aşağı kaydırın.</br>
 
   Bu bölümde AD FS yapılandırmadır sonra özgün AD FS ile Azure AD Connect yapılandırmasında ve bu nedenle yönetilen, yönetilenden dönüştürülmesi Federasyon güvenli bir şekilde varsayabilirsiniz görürseniz Azure AD Connect "değişiklik kullanıcı oturumu tabanlı -de "seçeneği, bu işlem bölümde ayrıntılı olarak verilmiştir **seçeneğinden A - anahtar Federasyon Azure AD Connect kullanarak parola karması eşitleme**.
   5. Active Directory Federasyon Hizmetleri geçerli ayarları listelenen göremez sonra bölümünde ayrıntılı PowerShell aracılığıyla yönetilen Federasyon oluşturan etki alanlarından el ile dönüştürmeniz gerekir **seçeneği B - Federasyon hizmetine geçiş Azure AD Connect ve PowerShell kullanarak parola karması eşitleme**.

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
Federasyon tasarım ve dağıtım belgelerinize, özelleştirilmiş, tüm ayarlarını özellikle PreferredAuthenticationProtocol SupportsMfa ve PromptLoginBehavior doğrulayın.

Ne bu ayarlar altında bulunabilir daha fazla bilgi.

[Active Directory Federasyon Hizmetleri sor = oturum açma parametresi desteği](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> SupportsMfa değeri şu anda "True" olarak ayarlanmışsa, ardından bu 2 Faktörlü zor kullanıcı kimlik doğrulaması akışı eklenmek üzere şirket içi MFA çözümünü kullanıyorsunuz anlamına gelir. Bu Azure AD kimlik doğrulama senaryoları için artık çalışmayacak ve bunun yerine aynı işlevi gerçekleştirmek için Azure mfa'yı (bulut tabanlı) hizmetini kullanarak gerekecek. Dikkatli bir şekilde geçmeden önce MFA gereksinimlerinizi değerlendirin ve nasıl Azure mfa'yı, lisanslama etkileri ve son kullanıcı kayıt işlemini etki alanlarınızı dönüştürmeden önce yararlanacağınızı anladığınızdan emin olun. Daha fazla ayrıntıya gider Azure MFA için dağıtım kılavuzunu şu yolda bulunabilir: [ https://aka.ms/deploymentplans ](https://aka.ms/deploymentplans).

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
| AD FS aracılığıyla eski kimlik doğrulama istemcilerini engelliyor.| AD FS ile birlikte üzerinde mevcut sürümden eski bir kimlik doğrulama istemcilerin engellenip engellenmeyeceğini denetimleri değiştirmeyi göz önüne alın [koşullu erişim için eski bir kimlik doğrulama denetimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) ve [Exchange Online istemci erişimi Kuralları](http://aka.ms/EXOCAR).|
| Kullanıcıların AD FS'ye kimlik doğrulaması yapılırken bir şirket içi MFA sunucusu çözümü karşı MFA gerçekleştirmesini gerektirir.| Bir kez etki alanı ileride dönüştürülür Bunu yapmak için Azure MFA hizmetinde kullanabilirsiniz ancak bir MFA testini şirket içi MFA çözüm aracılığıyla yönetilen bir etki alanı için kimlik doğrulaması akışı eklemesine mümkün olmayacaktır. Daha sonra kullanıcıları Azure mfa'yı bugün kullanmıyorsanız bu hazırlayın ve iletişim kurmak için son kullanıcılarınızın olacak tek seferlik bir son kullanıcı kayıt adımı içerir.|
| Erişim denetim ilkeleri (AuthZ kuralları) AD FS, Office 365 erişimi denetlemek için kullandığınız.| Bu eşdeğer Azure AD ile değiştirmeyi göz önüne alın [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) ve [Exchange Online istemci erişim kuralları](http://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Ortak AD FS özelleştirmelerini dikkate alınacak noktalar

#### <a name="inside-corporate-network-claim"></a>İç şirket ağı talep

Kimliği doğrulanan kullanıcının kurumsal ağ içinde değilse InsideCorporateNetwork talep AD FS tarafından verilir. Bu talep Azure AD'ye geçirildi ve kullanıcının ağ konumuna göre çok faktörlü kimlik doğrulamasını atlamak için kullanılır. Bkz: [güvenilen IP'leri Federasyon kullanıcıları için](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) bu AD FS'de şu anda etkin olup olmadığını belirleme hakkında daha fazla bilgi için.

Parola Karması eşitleme için etki alanlarınızın dönüştürülen InsideCorporateNetwork talep artık kullanılabilir olmayacaktır. [Azure AD'de adlandırılmış Konumlar](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) bu işlevin yerini için kullanılabilir.

Adlandırılmış konumları yapılandırma yapıldıktan sonra tüm koşullu erişim ilkeleri, dahil etmek veya "Tüm Güvenilen Konumlar" ağ konumları hariç tutmak için yapılandırılmış veya "MFA güvenilen IP'ler" adlı yeni oluşturulan konumlarını gösterecek şekilde güncelleştirilmesi gerekir.

Bkz: [Active Directory koşullu erişimi konumlarındaki](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) koşullu erişimde konum koşulu hakkında daha fazla bilgi için.

#### <a name="hybrid-azure-ad-joined-devices"></a>Karma Azure AD alanına katılmış aygıtlar

Bir cihazı Azure AD'ye katılma, güvenlik ve uyumluluk için erişim standartlarınızı karşılayan cihazların zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar ve bir cihaza bir kurumsal iş veya Okul hesabı yerine kişisel kullanarak oturum açmasına izin verir hesabı. Hibrit Azure AD'ye katılmış cihazların, AD etki alanına katılan cihazları Azure AD'ye sağlar. Bu özellik ile Federasyon ortamınızı yapılandırılmış olabilir.

Karma katılın yeni cihazlar için çalışma, parola karma eşitlemesini alanlarınızı üzere dönüştürdükten sonra etki alanına katılmış devam edebilmesini sağlamak üzere Azure AD Connect için Windows 10 istemcileri için Active Directory bilgisayar hesaplarını eşitlemek üzere yapılandırılmalıdır Azure AD. Windows 7 ve Windows 8 bilgisayar hesapları için karma katılın sorunsuz çoklu oturum açma bilgisayar Azure AD'ye kaydetme kullanır ve Windows 10 cihazları için yaptığınız gibi bunları eşitleyebilirsiniz gerekmez. Kendilerini sorunsuz çoklu oturum açma kullanarak kaydetmek için ancak bir güncelleştirilmiş workplacejoin.exe dosyasını (aracılığıyla bir .msi) dağıtmak bu alt düzey istemciler için gerekir. [.msi indirme](https://www.microsoft.com/download/details.aspx?id=53554). 

Daha fazla bilgi için [karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Markalama

Kuruluşunuzun olabilir [, AD FS oturum sayfalarının özelleştirilmiş](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) kuruluşa daha ilgili bilgileri görüntülemek için. Bu durumda, benzer yapmayı göz önüne alın [Azure AD oturum açma sayfası özelleştirmeleri](https://docs.microsoft.com/azure/active-directory/customize-branding).

Benzer özelleştirmeleri kullanılabilir, ancak bazı görsel değişiklikler beklenmelidir. Beklediğiniz değişikliklerin son kullanıcılar, iletişim eklemek isteyebilirsiniz.

> [!NOTE]
> Şirket markası, yalnızca Azure AD Premium veya temel lisansı satın veya Office 365 lisansınız varsa kullanılabilir.

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

Parola Karması eşitleme hem sorunsuz çoklu oturum açma dağıtıldıktan sonra Office 365 ve diğer erişme ilişkili kaynakları Azure AD kimlik doğrulaması yapılırken son kullanıcı oturum açma deneyimi değişir. Ağa dış kullanıcılar, dış yönelik Web uygulaması Proxy sunucuları tarafından sunulan form tabanlı sayfasına yeniden yönlendirilen aksine Azure AD oturum açma sayfası yalnızca, şimdi görür.

İletişim stratejinizi planlama için birden fazla öğe vardır. Bunlar:

* Yaklaşan ve yayımlanan işlevselliği kullanıcılara bildirme
  * E-posta ve diğer şirket içi iletişim kanallarını
  * Görseller gibi posterler
  * Canlı ya da diğer yönetim iletişimleri
* Kimin özelleştireceğim kullanan iletişimleri gönderir belirlemekten ve ne zaman.

## <a name="implementing-your-solution"></a>Çözümünüzü uygulama

Çözümünüzü planladıktan sonra bu uygulamaya hazır olursunuz. Uygulama aşağıdaki bileşenleri içerir:

1. Parola Karmasını Eşitlemeyi Etkinleştirme

2. Üzerinde sorunsuz çoklu oturum açma için hazırlama

3. Parola karma eşitlemesi ve sorunsuz çoklu oturum açmayı etkinleştirmek için oturum açma yöntemini değiştirme

### <a name="step-1--enable-password-hash-synchronization"></a>1. adım: parola karma eşitlemesini etkinleştirme

Bu çözümü uygulamak için ilk adım, parola karması eşitleme Azure AD Connect Sihirbazı'nın etkileştirir. Parola Karması eşitleme, Federasyon kimlik doğrulaması akışı etkilenmeden kullanarak ortamlar etkinleştirilebilir isteğe bağlı bir özelliktir. Bu durumda, Azure AD Connect parola karmalarının kullanıcıları etkilemeden eşitleme başlar imzalama Federasyon kullanarak açma.

Bu nedenle, etki alanı oturum açma yönteminizi de değiştirmeden önce bir hazırlık görevi olarak bu adımı gerçekleştirmeden öneririz. Bu size verecektir parola karması eşitleme doğrulamak için bol zaman düzgün çalışmasını.

Parola Karması eşitlemeyi etkinleştirmek için:

   1. Sunucusunda Azure AD Connect Sihirbazı'nı açın ve Yapılandır'ı seçin.
   2. Eşitleme seçeneklerini özelleştirme seçin ve ardından İleri'yi seçin.
   3. Azure AD ekran Bağlan kullanıcı adı ve bir genel yönetici parolasını sağlayın.
   4. Dizinleri ekranınızın Bağlan'İleri ' ye tıklayın.
   5. Etki alanı ve OU filtreleme ekranda İleri'ye tıklayın.
   6. İsteğe bağlı özellikler ekranında, Parola Eşitleme'ı seçin ve İleri'yi seçin.
   ![Resim 21](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)</br>
   7. Sonraki tüm kalan ekranlar ve yapılandırma son ekranda seçin.
   8. Azure AD Connect, sonraki eşitlemede parola karmalarının eşitleme başlar.

Parola Karması eşitleme etkinleştirildikten sonra Azure AD Connect eşitleme kapsamı yeniden karma haline getirilen ve Azure AD'ye yazılan tüm kullanıcılar için parola karma hale getirir. Kullanıcıların sayısına bağlı olarak, bu işlem, dakika veya birkaç saat sürebilir.

Planlama amacıyla, 20.000 kullanıcı yaklaşık 1 saat içinde işlenebilecek tahmin etmelidir.

Parola Karması eşitleme doğrulamak için düzgün çalıştığından, Azure AD Connect Sihirbazı'nda sorun giderme görevini kullanın.

   1. Yeni bir Windows PowerShell oturumunda, Azure AD Connect sunucunuzda olan çalıştırma yönetici seçeneğini açın.
   2. Set-ExecutionPolicy RemoteSigned veya Set-ExecutionPolicy sınırsız çalıştırın.
   3. Azure AD Connect Sihirbazı'nı başlatın.
   4. Ek Görevler sayfasına gidin, sorun giderme seçin ve İleri'ye tıklayın.
   5. Sorun giderme sayfasında, sorun giderme menü PowerShell'de başlamak için Başlat'ı tıklatın.
   6. Ana menüde, parola karması eşitleme sorunlarını giderme seçin.
   7. Alt menüde, parola karması eşitleme hiç çalışmıyor seçin.

Bir sorunla karşılaşmanız halinde, bilgileri kullanmak [bu makalede](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization) giderilir.

### <a name="step-2--prepare-for-seamless-sso"></a>Sorunsuz çoklu oturum açma için adım 2 – hazırlama

Cihazlarınıza sorunsuz çoklu oturum açmayı kullanmak için bir Azure AD URL'si, Active Directory'de Grup İlkesi'ni kullanarak kullanıcıların Intranet bölgesi ayarlarına eklemeniz gerekir.

Varsayılan olarak, tarayıcı, doğru bölgeyi, Internet veya Intranet, belirli bir URL'den otomatik olarak hesaplar. Örneğin, "http://contoso/"eşler Intranet bölgesine ise"http://intranet.contoso.com/" (URL bir dönemi içerdiğinden) Internet bölgesine eşler. Tarayıcının Intranet bölgesine açıkça URL eklemediğiniz sürece tarayıcılar Kerberos biletleri Azure AD URL'yi gibi bir bulut uç noktasına gönderir.

İzleyin [kullanımına sunmak için adımları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) cihazlarınıza gerekli değişiklikleri.

> [!IMPORTANT]
> Bu değişiklik, kullanıcıların Azure AD'de oturum biçimini değiştirmez. Ancak, bu yapılandırma, adım 3 ile devam etmeden önce tüm cihazlarınıza uygulandıktan önemlidir. Ayrıca bu yapılandırma etmemiş olan cihazlarda oturum açan kullanıcılar yalnızca kullanıcı adı ve Azure AD'de oturum açmak için parola girmeniz gerektiğini unutmayın.

### <a name="step-3--change-sign-in-method-to-phs-and-enable-seamless-sso"></a>3. adım – PHS için oturum açma yöntemini değiştirme ve sorunsuz çoklu oturum açmayı etkinleştir

#### <a name="option-a---switch-from-federation-to-phs-by-using-azure-ad-connect"></a>Seçenek A - Azure AD Connect kullanarak PHS için Federasyon geçiş

Azure AD Connect kullanarak AD FS'yi başlangıçta yapılandırıldığı durumlarda bu yöntemi kullanın. Azure AD Connect kullanarak AD FS'yi ilk olarak yapılandırılmamış, bu yöntemi kullanamazsınız. İlk **kullanıcı oturum açma yöntemini değiştirme**

   1. Azure AD Connect sunucusunda sihirbazını açın.
   2. Değişiklik kullanıcı oturum açma seçin ve ardından İleri'yi seçin.
   ![Resim 27](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)</br>
   3. İçinde **Azure ad Connect** ekran, kullanıcı adını ve parolasını sağlayın bir **genel yönetici**.
   4. İçinde **kullanıcı oturum açma** ekranında, radyo düğmesinin AD FS ile Federasyon için karma eşitlemeyi geçirmek değiştirin ve onay kutusunu emin olun kullanım dışı bir adımdır ve'gelecek bir sürümünde kaldırılacak bu kullanıcı hesaplarını dönüştürme AAD'ın bağlanın. Ayrıca etkinleştir çoklu oturum açma ve ardından seçin **sonraki**.
   ![29 resmi](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)</br>
   
   > [!NOTE]
   > Sorunsuz çoklu oturum açma onay kutusunu, Azure AD Connect sürümü 1.1.880.0 ile başlayarak, varsayılan olarak etkindir.
   
   > [!IMPORTANT]
   > Bu kullanıcı dönüştürme belirten uyarılar güvenle yok sayabilirsiniz ve tam parola karması eşitleme Federasyon seçeneğinden bulut kimlik doğrulaması için dönüştürmek için gerekli adımlar verilmiştir. Bu adımları artık gerekli değildir, Azure AD Connect gelecek sürümlerinde kullanıcılarını bir seçeneğe sahip olmaz lütfen unutmayın. Bu uyarılar görmeye devam ediyorsanız, onay Azure AD Connect ve bu, en son sürümünü çalıştıran bu kılavuzun en son sürümünü kullanıyor. Daha fazla bilgi için [güncelleştirme Azure AD Connect Bölümü](#_Update_Azure_AD).
   
   5. Etkinleştirme çoklu oturum açma ekranında, etki alanı yöneticisi hesabının kimlik bilgilerini girin ve ardından İleri'yi seçin.
   ![35 resmi](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)</br>
   
   > [!NOTE]
   > Etki alanı yöneticisi kimlik bilgileri, işlemin bu yükseltilmiş izinleri gerektiren aşağıdaki eylemleri gerçekleştirirken sorunsuz çoklu oturum açmayı etkinleştirmek için gereklidir. Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılan ve işlemin başarıyla tamamlanmasından sonra atılır
   >  * (Azure AD temsil eden) AZUREADSSOACC adlı bir bilgisayar hesabı, şirket içi Active Directory (AD) oluşturulur.
   >  * Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
   >  * Ayrıca, Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.
   >  * Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılan ve işlemin başarıyla tamamlanmasından sonra atılır
   
   6. Yapılandırma ekranına hazır, emin olun "yapılandırma tamamlandığında eşitleme başlangıç işlemi" onay kutusunun seçili. Configure'ı seçin.
   ![Resim 36](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)</br>
   
   > [!IMPORTANT]
   > Bu noktada tüm Federasyon etki alanları için kimlik doğrulama yöntemi olarak parola karması eşitleme artık özelliğinden yararlanır yönetilen kimlik doğrulaması için değiştirilecek.
       
   7. Azure AD portalı açın, Azure Active Directory'yi seçin ve ardından Azure AD Connect'i seçin.
   8. Sorunsuz çoklu oturum açma sırasında Federasyon devre dışı bırakıldı ve parola eşitleme etkin olduğunu doğrulayın.  
  ![Resim 37](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)</br>
   9. Git [test ve sonraki adımları](#testing-and-next-steps).
   
   > [!IMPORTANT]
   > Atla seçeneği B - bölüm Federasyon anahtardan bu bölümdeki adımları olarak Azure AD Connect ve PowerShell kullanarak parola karması eşitleme için geçerli değildir.  

#### <a name="option-b---switch-from-federation-to-phs-using-azure-ad-connect-and-powershell"></a>Seçenek B - Azure AD Connect ve PowerShell kullanarak PHS için Federasyon geçiş

Azure AD Connect kullanarak Federasyon başlangıçta yapılandırılmamış olduğunda bu seçeneği kullanın.

Bu işlemin bir parçası, sorunsuz çoklu oturum açmayı etkinleştirin ve alanlarınızı federe gelen yönetilen geçin.

   1. Azure AD Connect sunucusunda sihirbazını açın.
   2. Değişiklik kullanıcı oturum açma seçin ve ardından İleri'yi seçin. 
   3. Azure AD ekran Bağlan kullanıcı adı ve bir genel yönetici parolasını sağlayın.
   4. Kullanıcı oturum açma ekranında, parola karması eşitleme için radyo düğmesini yapılandırmazsanız değişiklik etkinleştir çoklu oturum açma seçip İleri'yi seçin.
   
   Değişiklikten önce: ![Resim 20](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)</br>

   Değişiklikten sonra:  
   ![Resim 22](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)</br>
   
   > [!NOTE]
   > Sorunsuz çoklu oturum açma onay kutusunu, Azure AD Connect sürümü 1.1.880.0 ile başlayarak, varsayılan olarak etkindir.
   
   5. Etkinleştirme çoklu oturum açma ekranında, etki alanı yöneticisi hesabının kimlik bilgilerini girin ve ardından İleri'yi seçin.
   
   > [!NOTE]
   > Etki alanı yöneticisi kimlik bilgileri, işlemin bu yükseltilmiş izinleri gerektiren aşağıdaki eylemleri gerçekleştirirken sorunsuz çoklu oturum açmayı etkinleştirmek için gereklidir. Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılır ve Kurulum başarıyla tamamlandıktan sonra atılır.
   > * (Azure AD temsil eden) AZUREADSSOACC adlı bir bilgisayar hesabı, şirket içi Active Directory (AD) oluşturulur.
   > * Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
   > * Ayrıca, Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.
   
   6. Yapılandırma ekranına hazır, emin olun "yapılandırma tamamlandığında eşitleme başlangıç işlemi" onay kutusunun seçili. Configure'ı seçin.
   ![Resim 41](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)</br>
   Seçme yapılandırdığınızda, sorunsuz çoklu oturum açma önizlemeler adım göre yapılandırılır. Daha önce etkinleştirilmiş olduğu gibi parola karma eşitlemesini yapılandırma değişiklik olmaz.
   
   > [!IMPORTANT]
   > Kullanıcılar bu noktada oturum şekilde hiçbir değişiklik yapılmaz.  
   
   7. Azure AD portalı üzerinde Federasyon etkinleştirilmesi devam eder ve sorunsuz çoklu oturum açma etkinleştirildi doğrulayın.
   ![Resim 42](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Yönetilen Federasyon oluşturan etki alanlarından Dönüştür

Bu noktada, Federasyon hala etkinse ve operasyonel etki. Dağıtıma devam etmek için her etki alanı kullanıcı kimlik doğrulaması aracılığıyla parola karma eşitlemesini zorlamak için yönetilen için federe dönüştürülmesi gerekiyor.

> [!IMPORTANT]
> Tüm etki alanları gereksinim dönüştürülmesi aynı anda Başlarken tercih edebileceğiniz üretim kiracınızın veya etki alanı ile en az bir test etki alanı kullanıcı sayısı.

Dönüştürme, Azure AD PowerShell modülü kullanılarak gerçekleştirilir.

   1. PowerShell ve Azure AD genel yönetici hesabını kullanarak oturum açın.  
   2. İlk etki alanı dönüştürmek için aşağıdaki komutu çalıştırın:  
   
   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
   ```
   
   3. Azure AD portalı açın, Azure Active Directory'yi seçin ve ardından Azure AD Connect'i seçin.
   4. Aşağıdaki komutu çalıştırarak etki alanı için yönetilen dönüştürülmüş doğrulayın:
   
   ``` PowerShell
   Get-MsolDomain -DomainName <domainname>
   ```

## <a name="testing-and-next-steps"></a>Test ve sonraki adımlar

### <a name="test-authentication-with-phs"></a>PHS ile kimlik doğrulamasını Sına

Kiracınızın Federasyon kullanırken, kullanıcıları Azure AD oturum açma sayfasından AD FS ortamınıza yönlendiriliyorsa. Kiracı Federasyon yerine parola karma eşitlemesini kullanmak üzere yapılandırılmış, kullanıcıları AD FS'ye yeniden yönlendirilir değil ve bunun yerine doğrudan Azure AD oturum açma sayfasının oturum açacaksınız.

Internet Explorer otomatik olarak oturum açma sorunsuz SSO önlemek ve Office 365 oturum açma sayfasına gitmek için InPrivate Modu Aç ([http://portal.office.com](http://portal.office.com/)). Kullanıcının UPN'sini girin ve İleri'ye tıklayın. Şirket içi Active Directory'nizden eşitlenen ve kimin daha önce Federal karma kullanıcının UPN'sini yazdığınızdan emin olun. Kullanıcının kullanıcı adı ve parola yazmak için bir ekran görürsünüz.

![Resim 9](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

![Resim 12](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

Parolayı yazın sonra Office 365 portalına yönlendirilen.

![Resim 17](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)

### <a name="test-seamless-single-sign-on"></a>Sorunsuz çoklu oturum açma testi

Şirket ağına bağlanan etki alanına katılmış bir makinedeki oturum açın. Internet Explorer'ı açın ve aşağıdaki URL'lerden birine gidin:  
  
[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (Contoso etki alanınız ile değiştirin).

Kullanıcı kısa bir süre için Azure AD oturum açma sayfasına yönlendirilir ve ", oturum açmaya çalışırken" iletisini görürsünüz ve bir kullanıcı adı veya parola için sizden.

![Resim 24](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)

Ardından, kullanıcı yönlendirilen ve erişim paneline başarıyla imzalandı:

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

### <a name="troubleshooting"></a>Sorun giderme

Sırasında veya değişiklikten sonra yönetilen için Federasyon seçeneğinden kaynaklanan kimlik doğrulama sorunları gidermek nasıl destek ekibinize anlamanız gerekir. Kendilerini tanıyın genel sorun giderme adımları ve yalıtmak ve sorunu çözmek için yardımcı olabilecek uygun eylemleri ile destek ekibinize yardımcı olması için aşağıdaki sorun giderme belgelerini kullanın.

[Azure Active Directory parola karması eşitleme sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Azure Active Directory sorunsuz çoklu oturum açma sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)  

## <a name="roll-over-the-seamless-sso-kerberos-decryption"></a>Sorunsuz SSO Kerberos şifre çözme Aktar

Şirket içinde oluşturulan Kerberos şifre çözme anahtarı (Azure AD temsil eden) AZUREADSSOACC bilgisayar hesabının üzerinden sık geri önemlidir AD ormanı. Active Directory etki alanı üyeleri parola değişiklikleri nasıl gönderme ile hizalamak için en azından her 30 gün Kerberos şifre çözme anahtarı alma önemle öneririz. Hayır ilişkili olarak olduğundan AZUREADSSOACC bilgisayar hesabı nesnesi Top üzerinden bağlı cihaz el ile uygulanması gerekir.

Kerberos şifre çözme anahtarı geçiş işlemini başlatmak için Azure AD Connect çalıştırdığınız şirket içi sunucusunda bu adımları izleyin.

[Nasıl miyim geri AZUREADSSOACC bilgisayar hesabının Kerberos şifre çözme anahtarı](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Connect tasarım kavramları](plan-connect-design-concepts.md)
- [Doğru kimlik doğrulamasını seçme](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Desteklenen topolojiler](plan-connect-design-concepts.md)
