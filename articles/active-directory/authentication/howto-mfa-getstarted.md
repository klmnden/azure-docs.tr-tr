---
title: Planlama ve Azure multi-Factor Authentication dağıtım - Azure Active Directory yürütme
description: Microsoft Azure multi-Factor Authentication dağıtımı planlama
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ca69fc23d580b61e74fe56b3d0c3524fdfad747
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235544"
---
# <a name="planning-a-cloud-based-azure-multi-factor-authentication-deployment"></a>Bulut tabanlı bir Azure multi-Factor Authentication dağıtımı planlama

Kişiler, kuruluş kaynaklarına giderek daha karmaşık senaryolarda bağlanırsınız. Akıllı telefonlar, tabletler, bilgisayarlar ve dizüstü bilgisayarlar, genellikle birden çok platformda kullanarak şirket ağına ve kuruluşa ait, kişisel ve genel cihazları kişiler bağlanın. Bu her zaman bağlı, birden çok cihazlı ve çok platformlu dünyada güvenlik kullanıcı hesaplarının her zamankinden çok daha önemlidir. Parolalar, cihazlar, ağlar ve platformlar arasında kullanılan karmaşıklık, ne olursa olsun, artık kullanıcıların özellikle hesaplarda parolaları yeniden kullanma eğilimindedir olduğunda kullanıcı hesabının güvenliğini sağlamak yeterli değildir. Gelişmiş. kimlik avı ve diğer sosyal mühendislik saldırılarına kullanıcı adları ve parolalar nakledilen ve koyu web üzerinden satılan neden olabilir.

[Azure multi-Factor Authentication (MFA)](concept-mfa-howitworks.md) korunmasına yardımcı verilere ve uygulamalara erişim. Bu, ek bir ikinci bir form kimlik doğrulaması kullanarak bir güvenlik katmanı sağlar. Kuruluşlar kullanabilir [koşullu erişim](../conditional-access/overview.md) çözümü için kendi belirli gereksinimlerine en uygun.

## <a name="prerequisites"></a>Önkoşullar

Azure multi-Factor Authentication dağıtımı başlatmadan önce ele alınması gereken önkoşul öğe yok.

| Senaryo | Önkoşul |
| --- | --- |
| **Yalnızca bulutta yer alan** kimlik ortam modern kimlik doğrulaması | **Hiçbir ek önkoşul görevler** |
| **Karma** kimlik senaryoları | [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) dağıtılır ve eşitlenen ya da şirket içi Active Directory etki alanı Hizmetleri ile Azure Active Directory ile Federasyon kullanıcı kimlikleri. |
| Şirket içinde yayımlanmış eski uygulamalar için bulut erişimi | Azure AD [uygulama proxy'si](../manage-apps/application-proxy.md) dağıtılır. |
| RADIUS kimlik doğrulaması ile Azure MFA kullanma | A [ağ ilkesi sunucusu (NPS)](howto-mfa-nps-extension.md) dağıtılır. |
| İOS 11 veya daha önceki kullanıcılar Microsoft Office 2010 veya önceki bir sürümü ya da Apple Mail sahip | Yükseltme [Microsoft Office 2013 veya üzeri](https://support.microsoft.com/help/4041439/modern-authentication-configuration-requirements-for-transition-from-o) ve 12 veya üzerini iOS için Apple mail. Koşullu erişim, eski bir kimlik doğrulama protokolleri tarafından desteklenmiyor. |

## <a name="plan-user-rollout"></a>Kullanıcı dağıtımı planlama

MFA piyasaya çıkma planınız destek kapasitenizi olan dağıtım Dalgalar arkasından bir pilot dağıtım içermelidir. Küçük bir pilot kullanıcılar grup için koşullu erişim ilkelerini uygulayarak dağıtımınız başlar. Pilot kullanıcılar, kullanılan işlem ve kayıt davranışları üzerindeki etkisini değerlendirdikten sonra daha fazla Grup İlkesine eklemek veya mevcut grupları için daha fazla kullanıcı ekleyin.

### <a name="user-communications"></a>Kullanıcı iletişim

Planlanan iletişimleri, kullanıcılara yaklaşan değişiklikleri, Azure MFA kaydı gereksinimlerini ve tüm gerekli kullanıcı eylemler hakkında bilgilendirmek için önemlidir. Temsilcileri iletişimleri, değişiklik yönetimi ve İnsan Kaynakları Departmanlar gibi kuruluşunuzdaki uyumlu bir şekilde iletişim geliştirilen öneririz.

Microsoft'un sağladığı [iletişim şablonları](https://aka.ms/mfatemplates) ve [son kullanıcı belgelerini](../user-help/security-info-setup-signin.md) iletişimlerin taslak yardımcı olmak için. Kullanıcılara gönderdiğiniz [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com) doğrudan seçerek kaydedilecek **güvenlik bilgisi** bu sayfadaki bağlantıları.

## <a name="deployment-considerations"></a>Dağıtma konuları

Azure multi-Factor Authentication ile koşullu erişim ilkelerini zorlayarak dağıtılır. A [koşullu erişim ilkesi](../conditional-access/overview.md) gibi belirli ölçütler karşılandığında, çok faktörlü kimlik doğrulaması gerçekleştirmek üzere kullanıcıların isteyebilirsiniz:

* Tüm kullanıcılar, belirli bir kullanıcı, Grup veya atanmış bir rol üyesi
* Erişilen belirli bulut uygulamaları
* Cihaz platformu
* Cihaz durumu
* Ağ konumu veya coğrafi konumda bulunan IP adresi
* İstemci uygulamaları
* Oturum açma riski (kimlik koruması gerektirir)
* Uyumlu cihaz
* Hibrit Azure AD'ye katılmış
* Onaylı istemci uygulaması

Özelleştirilebilir posterler kullanın ve e-posta şablonlarını [çok faktörlü kimlik doğrulaması ürün reçetesi](https://www.microsoft.com/download/details.aspx?id=57600&WT.mc_id=rss_alldownloads_all) kuruluşunuz için çok faktörlü kimlik doğrulaması almak için.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Koşullu erişim ile çok faktörlü kimlik doğrulamasını etkinleştirme

Koşullu erişim ilkeleri gerektiren ilk oturum açma sırasında bir önemli güvenlik önlemleri kaydı tamamlamak kaydolmamış kullanıcılar kayıt uygular.

[Azure AD kimlik koruması](../identity-protection/howto-configure-risk-policies.md) bir kayıt ilkesi için hem otomatik risk algılama ve düzeltme ilkeleri Azure multi-Factor Authentication hikayeye katkıda bulunur. İlkeleri güvenliği aşılmış kimlik kesilmeyen parola değişikliklerini zorlamak için oluşturduğunuz veya bir oturum açma aşağıdaki riskli bulunduğu zaman MFA gerektirecek [olayları](../reports-monitoring/concept-risk-events.md):

* Sızdırılan kimlik bilgileri
* Anonim IP adreslerinden oturum açma işlemleri
* Alışılmadık konumlara imkansız seyahat
* Alışılmadık konumlardan oturum açma işlemleri
* Bulaşma olan cihazlardan oturum açma işlemleri
* Şüpheli etkinliklerle IP adreslerinden oturum açma

Bazı Azure Active Directory kimlik koruması tarafından algılanan risk olayları gerçek zamanlı olarak ortaya ve bazı çevrimdışı işleme gerektirir. Yöneticiler riskli davranışları sergileyen ve el ile düzeltin, bir parola değişikliği iste veya koşullu erişim ilkelerini bir parçası olarak çok faktörlü kimlik doğrulaması gerektiren kullanıcıları engellemeyi seçebilirsiniz.

## <a name="define-network-locations"></a>Ağ Konumları tanımlayın

Kuruluşlar, ağ kullanarak tanımlamak için koşullu erişim kullanmak önerilen [adlandırılmış Konumlar](../conditional-access/location-condition.md#named-locations). Kuruluşunuz, kimlik koruması kullanarak risk tabanlı ilkeler yerine adlandırılmış konumlar kullanmayı düşünün.

### <a name="configuring-a-named-location"></a>Adlandırılmış bir konuma yapılandırma

1. Açık **Azure Active Directory** Azure portalında
2. Tıklayın **koşullu erişim**
3. Tıklayın **adlandırılmış konumlar**
4. Tıklayın **yeni konum**
5. İçinde **adı** alan, anlamlı bir ad girin
6. Tanımladığınız IP aralıkları veya ülkeler/bölgeler kullanarak konumunu seçin
   1. IP aralıkları kullanılıyorsa
      1. Konumun güvenilir olarak işaretlemek oluşturulmayacağına karar verin. Güvenilir bir konumdan oturum açılması, kullanıcının oturum açma riskini azaltır. Yalnızca kuruluşunuz tarafından tanınıyor girilen IP aralıkları biliyorsanız, bu konumu güvenilir'olarak işaretleyin.
      2. IP aralıklarını belirtin.
   2. Ülkeler/bölgeler kullanıyorsanız
      1. Aşağı açılan menüsünü genişletin ve ülke veya bölgelerde bu adlandırılmış konumu tanımlamak istediğiniz seçin.
      2. Bilinmeyen alanlar dahil edilip edilmeyeceğini karar verin. Bilinmeyen alanlar, bir ülke/bölge eşlenemez IP adresleridir.
7. **Oluştur**'a tıklayın.

## <a name="plan-authentication-methods"></a>Kimlik doğrulama yöntemleri planlama

Yöneticiler [kimlik doğrulama yöntemleri](../authentication/concept-authentication-methods.md) kullanıcılar için kullanılabilir hale getirmek istedikleri. Böylece kendi birincil yöntemini kullanılamaz olması durumunda yedekleme yöntemi kullanılabilir kullanıcınız en fazla bir tek bir kimlik doğrulama yöntemi izin vermek önemlidir. Yöneticiler için etkinleştirmek aşağıdaki yöntemleri kullanılabilir:

### <a name="notification-through-mobile-app"></a>Mobil uygulama üzerinden bildirim

Mobil Cihazınızda Microsoft Authenticator uygulamasına anında iletme bildirimi gönderilir. Kullanıcı bildirimi görünümleri ve seçer **Onayla** doğrulamayı tamamlamak için. Bir mobil uygulama üzerinden anında iletme bildirimleri kullanıcılar için az müdahale eden seçeneğini sağlar. Telefon yerine bir veri bağlantısı kullandığından, ayrıca en güvenilir ve güvenli seçenektir.

> [!NOTE]
> Kuruluşunuzda çalışan veya Çin'e seyahat personeli varsa **mobil uygulama üzerinden bildirim** metodunda **Android cihazları** bu ülkede çalışmaz. Alternatif yöntemler söz konusu kullanıcılar için kullanılabilir yapılması gerekir.

### <a name="verification-code-from-mobile-app"></a>Mobil uygulamadan alınan doğrulama kodu

Microsoft Authenticator uygulaması gibi bir mobil uygulama, her 30 saniyede yeni bir OATH doğrulama kodu oluşturur. Kullanıcı oturum açma arabirimine doğrulama kodunu girer. Veri veya hücresel sinyal telefon sahip olup olmadığını, mobil uygulama seçeneği kullanılabilir.

### <a name="call-to-phone"></a>Telefonu arama

Otomatik bir sesli çağrıyla kullanıcıya yerleştirilir. Kullanıcı basışlarını ve çağrı yanıtını **#** kimlik doğrulamasını onaylamak için telefon klavyede. Telefon mobil uygulamasından bildirim ya da doğrulama kodu için harika bir yedekleme yöntemidir.

### <a name="text-message-to-phone"></a>Telefona kısa mesaj

Kullanıcıya doğrulama kodunu içeren bir SMS mesajı gönderilir, kullanıcıyı doğrulama kodunu oturum açma arabirimine girmesi istenir.

### <a name="choose-verification-options"></a>Doğrulama seçenekleri seçin

1. Gözat **Azure Active Directory**, **kullanıcılar**, **çok faktörlü kimlik doğrulaması**.

   ![Azure portalında Azure AD kullanıcıları dikey penceresinden multi-Factor Authentication portalına erişim](media/howto-mfa-getstarted/users-mfa.png)

1. Yeni sekmede açılır göz atın **hizmet ayarları**.
1. Altında **doğrulama seçenekleri**, tüm kullanıcıların yararlanabileceği yöntemler için kutuları işaretleyin.

   ![Multi-Factor Authentication hizmeti Ayarlar sekmesinde doğrulama yöntemlerini yapılandırma](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

1. **Kaydet**'e tıklayın.
1. Kapat **hizmet ayarları** sekmesi.

## <a name="plan-registration-policy"></a>Kayıt İlkesi planlama

Yöneticiler, kullanıcılar kendi yöntemlerini nasıl kaydedeceksiniz belirlemeniz gerekir. Kuruluşların gereken [yeni bir birleşik kayıt deneyimini etkinleştirmek](howto-registration-mfa-sspr-combined.md) Azure MFA ve Self Servis parola sıfırlama (SSPR). SSPR çok faktörlü kimlik doğrulaması için kullandıkları aynı yöntemleri kullanarak güvenli bir şekilde parolalarını sıfırlamalarına olanak tanır. Her iki hizmet için bir kez kaydetme olanağı ile kullanıcılar için harika bir deneyim olduğundan bu kayıt, şu anda genel önizlemede birleştirilmiş öneririz. SSPR ve Azure MFA için aynı yöntemleri etkinleştirilmesi, her iki özelliği kullanmak için kayıtlı olması kullanıcılarınızın izin verir.

### <a name="registration-with-identity-protection"></a>Kimlik koruması ile kayıt

Azure Active Directory kimlik koruması, kuruluşunuzun kullanıyorsa [MFA kayıt ilkesini yapılandırma](../identity-protection/howto-mfa-policy.md) bunlar içinde etkileşimli olarak bir sonraki oturum açışınızda kaydetmek için kullanıcılarınızın istemek için.

### <a name="registration-without-identity-protection"></a>Kayıt olmadan kimlik koruması

Kuruluşunuz kimlik korumasını etkinleştirmek lisansları yoksa, kullanıcılar kayıt mfa'yı oturum açma işleminde gerekli olan sonraki zamanı istenir. MFA ile korunan uygulamaları kullanmıyorsanız, kullanıcılar için mfa'yı kayıtlı değil. Etkili bir şekilde hesap denetimini üstlenerek kötü aktörleri'nın bir kullanıcı parolasını tahmin ve kendi adınıza MFA'ya kaydetmeniz kayıtlı tüm kullanıcıların önemlidir.

#### <a name="enforcing-registration"></a>Kayıt zorlama

Koşullu erişim aşağıdaki adımları kullanarak ilke çok faktörlü kimlik doğrulamasına kaydolacak şekilde kullanıcılar zorlayabilir

1. Şu anda kayıtlı olan tüm kullanıcılar ekleyin, bir grup oluşturun.
2. Koşullu erişim kullanarak, bu grubun tüm kaynaklara erişim için multi-Factor authentication uygular.
3. Düzenli aralıklarla, grup üyeliklerini yeniden değerlendir ve kayıtlı kullanıcılar gruptan kaldırın.

Dayanan PowerShell komutları ile kayıtlı ve kayıtlı olmayan Azure MFA kullanıcıları tanımlayabilir [MSOnline PowerShell modülünde](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).

#### <a name="identify-registered-users"></a>Kayıtlı kullanıcıları belirleyin

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

#### <a name="identify-non-registered-users"></a>Kayıtlı olmayan kullanıcıları belirleyin

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>MFA kullanıcı başına MFA dönüştürme kullanıcıların koşullu erişim için temel

Kullanıcılarınızın, kullanıcı başına etkin kullanarak etkinleştirilen ve zorlanan Azure multi-Factor Authentication aşağıdaki PowerShell koşullu erişim dönüştürme yaparken size yardımcı olabilir, Azure multi-Factor Authentication temel.

```PowerShell
# Disable MFA for all users, keeping their MFA methods intact
Get-MsolUser -All | Disable-MFA -KeepMethods

# Enforce MFA for all users
Get-MsolUser -All | Set-MfaState -State Enforced

# Wrapper to disable MFA with the option to keep the MFA
# methods (to avoid having to proof-up again later)
function Disable-Mfa {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipeline=$True)]
        $User,
        [switch] $KeepMethods
    )

    Process {

        Write-Verbose ("Disabling MFA for user '{0}'" -f $User.UserPrincipalName)
        $User | Set-MfaState -State Disabled

        if ($KeepMethods) {
            # Restore the MFA methods which got cleared when disabling MFA
            Set-MsolUser -ObjectId $User.ObjectId `
                         -StrongAuthenticationMethods $User.StrongAuthenticationMethods
        }
    }
}

# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

```

## <a name="plan-conditional-access-policies"></a>Koşullu erişim ilkelerini planlayın

MFA ve diğer denetimleri gerekli olduğunda belirler, koşullu erişim ilkesi stratejisi planlama başvurmak [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md).

Azure AD kiracınızın dışında yanlışlıkla kilitlenen önlemek önemlidir. Yönetici erişiminin yanlışlıkla bu eksikliği etkisini en aza indirebileceğiniz [kiracınızda iki veya daha fazla Acil Durum erişim hesapları oluşturma](../users-groups-roles/directory-emergency-access.md) ve bunları, koşullu erişim ilkesinden hariç tutulması.

### <a name="create-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

1. Oturum [Azure portalında](https://portal.azure.com) bir genel yönetici hesabını kullanarak.
1. Gözat **Azure Active Directory**, **koşullu erişim**.
1. Seçin **yeni ilke**.
1. İlkeniz için anlamlı bir ad sağlayın.
1. Altında **kullanıcılar ve gruplar**:
   * Üzerinde **INCLUDE** sekmesinde **tüm kullanıcılar** radyo düğmesi
   * Üzerinde **hariç** sekmesinde, onay kutusunu için **kullanıcılar ve gruplar** ve, Acil Durum erişim hesapları'nı seçin.
   * **Bitti**’ye tıklayın.
1. Altında **bulut uygulamaları**seçin **tüm bulut uygulamaları** radyo düğmesi.
   * İSTEĞE BAĞLI OLARAK: Üzerinde **hariç** sekmesinde, kuruluşunuz için MFA gerektirmeyen bir bulut uygulamaları seçin.
   * **Bitti**’ye tıklayın.
1. Altında **koşullar** bölümü:
   * İSTEĞE BAĞLI OLARAK: Azure kimlik koruması etkinleştirilirse, oturum açma riski İlkesi bir parçası olarak değerlendirilecek seçebilirsiniz.
   * İSTEĞE BAĞLI OLARAK: Güvenilen konumları yapılandırılmış veya adlandırılmış konumlar, dahil etmek veya ilkeden konumların dışlamak için belirtebilirsiniz.
1. Altında **Grant**, emin **erişim ver** radyo düğmesi seçili.
    * İçin kutuyu **çok faktörlü kimlik doğrulaması gerektiren**.
    * **Seç**'e tıklayın.
1. Skip **oturumu** bölümü.
1. Ayarlama **ilkesini etkinleştir** geç **üzerinde**.
1. **Oluştur**’a tıklayın.

![Pilot grubundaki Azure portalı kullanıcıları için mfa'yı etkinleştirmek için bir koşullu erişim ilkesi oluşturma](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

## <a name="plan-integration-with-on-premises-systems"></a>Şirket içi sistemlerle tümleştirme planlama

Doğrudan Azure AD'de kimlik doğrulaması yapmaz bazı eski ve şirket içi uygulamaları MFA dahil olmak üzere kullanmak için ek adımlar gerektirir:

* Eski uygulama ara sunucusu kullanmanız gerekir uygulamalar, şirket içi.
* Böylece, şirket içi RADIUS uygulamalar, MFA bağdaştırıcısı ile NPS sunucusu kullanmanız gerekir.
* Böylece, şirket içi AD FS uygulamalar, AD FS 2016 ile MFA bağdaştırıcı kullanmanız gerekir.

Doğrudan Azure AD kimlik doğrulaması ve modern kimlik doğrulaması (WS-Federasyon, SAML, OAuth, Openıd Connect) sahip uygulamalar koşullu erişimi kullanın yapabilir ilkeleri doğrudan.

### <a name="use-azure-mfa-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile Azure mfa'yı kullanın

Şirket içi kaynaklarınızda bulunan Azure AD'nize yayımlanan uygulamalarla Kiracı aracılığıyla [Azure AD uygulama proxy'si](../manage-apps/application-proxy.md) ve yararlanabilirsiniz Azure multi-Factor Authentication'ın Azure AD'yi kullanacak şekilde yapılandırılmışsa ön kimlik doğrulama.

Bu uygulamalar, Azure multi-Factor Authentication, diğer tüm Azure AD ile tümleşik uygulamalar gibi zorlayan koşullu erişim ilkeleri tabidir.

Benzer şekilde, Azure multi-Factor Authentication için tüm kullanıcı oturum açma işlemleri zorunlu kışınmışsa Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar korunan şirket içi.

### <a name="integrating-azure-multi-factor-authentication-with-network-policy-server"></a>Azure multi-Factor Authentication ağ ilkesi sunucusu ile tümleştirme

Azure MFA için ağ ilkesi sunucusu (NPS) uzantısı, var olan sunucuları kullanarak kimlik doğrulaması altyapınız için bulut tabanlı MFA özellikleri ekler. NPS uzantısı ile mevcut kimlik doğrulama akışınıza telefon araması, SMS mesajı ve telefon uygulaması doğrulama ekleyebilirsiniz. Bu tümleştirme aşağıdaki sınırlamalara sahiptir:

* Yalnızca kimlik doğrulayıcısı uygulaması anında iletme bildirimleri ve sesli arama CHAPv2 protokolü ile desteklenir.
* Koşullu erişim ilkeleri uygulanamaz.

Bir bağdaştırıcı arasında RADIUS ve korumak için Azure MFA bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için NPS uzantısı görür [VPN](howto-mfa-nps-extension-vpn.md), [Uzak Masaüstü Ağ Geçidi bağlantıları](howto-mfa-nps-extension-rdg.md), ya da diğer RADIUS uyumlu uygulamalar. Azure mfa kaydı bu ortamdaki tüm kimlik doğrulama girişimleri, koşullu erişim ilkeleri olmaması için zorluğu olan kullanıcılar, mfa'yı her zaman gerekli olan anlamına gelir.

#### <a name="implementing-your-nps-server"></a>NPS sunucunuzu uygulama

Dağıtılan bir NPS örneğiniz varsa ve kullanımda zaten başvuru [mevcut NPS altyapınızı Azure multi-Factor Authentication ile tümleştirme](howto-mfa-nps-extension.md). NPS ' için ilk kez ayarlıyorsanız, başvurmak [ağ ilkesi sunucusu (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) yönergeler için. Sorun giderme rehberi makalesinde bulunabilir [Azure multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md).

#### <a name="prepare-nps-for-users-that-arent-enrolled-for-mfa"></a>NPS'yi MFA için kayıtlı olmayan kullanıcılar için hazırlama

MFA ile kayıtlı olmayan kullanıcı kimlik doğrulaması çalıştığında ne olacağını seçin. Kayıt defteri ayarını kullanın `REQUIRE_USER_MATCH` kayıt defteri yolunda `HKLM\Software\Microsoft\AzureMFA` özellik davranışını denetlemek için. Bu ayar bir yapılandırma seçeneği vardır.

| Anahtar | Değer | Varsayılan |
| --- | --- | --- |
| `REQUIRE_USER_MATCH` | TRUE / FALSE | (TRUE eşdeğer) ayarlanmadı |

Bu ayar amacı bir kullanıcı için mfa'yı kayıtlı ne yapılacağını belirlemektir. Bu ayarı değiştirmenin etkileri aşağıdaki tabloda listelenmiştir.

| Ayarlar | Kullanıcı MFA durumu | Efektleri |
| --- | --- | --- |
| Anahtar yok | Kayıtlı değil | MFA testini başarısız oluyor |
| Değeri true olarak ayarlanmış / ayarlı değil | Kayıtlı değil | MFA testini başarısız oluyor |
| Anahtar False olarak ayarlayın. | Kayıtlı değil | Kimlik doğrulaması olmadan MFA |
| Yanlış veya doğru anahtarı ayarlayın | Kayıtlı | MFA ile kimlik doğrulaması yapması gereken |

### <a name="integrate-with-active-directory-federation-services"></a>Active Directory Federasyon Hizmetleri ile tümleştirme

Kuruluşunuz Azure AD ile birleştirildiyse kullanabileceğiniz [Azure multi-Factor Authentication'ı, AD FS kaynakları güvenli hale getirmek için](multi-factor-authentication-get-started-adfs.md), hem şirket içi ve bulut. Azure mfa'yı parolalar azaltır ve kimlik doğrulaması için daha güvenli bir şekilde sağlamanıza olanak sağlar. Windows Server 2016'dan itibaren Azure mfa'yı birincil kimlik doğrulaması için artık yapılandırabilirsiniz.

Farklı Windows Server 2012 R2'de AD FS ile AD FS 2016 Azure MFA bağdaştırıcısı doğrudan Azure AD ile tümleştirir ve bir şirket içi Azure MFA sunucusu gerekli değildir. Azure MFA bağdaştırıcısı Windows Server 2016 içinde yerleşik olarak bulunur ve ek yükleme için gerek yoktur.

AD FS 2016 ile Azure MFA ve hedef uygulamayı kullanarak koşullu erişim ilkesine olduğunda, ek hususlar vardır:

* Koşullu erişim, uygulamayı Azure AD'ye bağlı olan taraf AD FS 2016 ile Federasyon olduğunda kullanılabilir.
* Koşullu erişim uygulama bir bağlı olan taraf AD FS 2016 ve yönetilen veya AD FS 2016 ile Federasyon olduğunda kullanılamaz.
* AD FS 2016, Azure mfa'yı birincil kimlik doğrulama yöntemi olarak kullanmak için yapılandırıldığında, koşullu erişim de kullanılamaz.

#### <a name="ad-fs-logging"></a>AD FS oturum açma

Standart Windows Güvenlik günlüğü ve AD FS yönetim günlüğü, AD FS 2016 günlük kimlik doğrulama isteklerini ve başarısı veya başarısızlığı hakkında bilgi içerir. İçinde bu olayları olay günlüğü verilerini Azure mfa'yı kullanılıp kullanılmadığını belirtir. Örneğin, AD FS Auditing olay kimliği 1200 içerebilir:

```
<MfaPerformed>true</MfaPerformed>
<MfaMethod>MFA</MfaMethod>
```

#### <a name="renew-and-manage-certificates"></a>Yenileme ve sertifikaları yönetme

Her AD FS sunucusunda yerel bilgisayara My Store olacaktır otomatik olarak imzalanan bir Azure mfa'yı başlıklı OU sertifika sertifika sona erme tarihi içeren Microsoft AD FS Azure MFA =. Bu sertifika sona erme tarihi belirlemek için her AD FS sunucusunda geçerlilik süresini denetleyin.

Sertifikaların geçerlilik süresi, süresi dolmak üzere olup olmadığını [oluşturmak ve her AD FS sunucusunda yeni MFA sertifika doğrulamak](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa#configure-the-ad-fs-servers).

Aşağıdaki yönergeler, AD FS sunucularınızda Azure MFA sertifikalarını yönetme işlemi açıklanmaktadır. AD FS ile Azure mfa'yı yapılandırdığınızda, sertifika aracılığıyla oluşturulan `New-AdfsAzureMfaTenantCertificate` PowerShell cmdlet'i 2 yıl boyunca geçerlidir. Yenileyin ve MFA hizmeti ovoid bağlantılarında kesintiler için süre sonundan önce yenilenen sertifikaları yükleyin.

## <a name="implement-your-plan"></a>Planınızı uygulama

Çözümünüzü planladıktan sonra aşağıdaki adımları izleyerek uygulayabilirsiniz:

1. Gerekli önkoşulları karşılaması
   1. Dağıtma [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) herhangi bir karma senaryoları
   1. Dağıtma [Azure AD uygulama proxy'si](../manage-apps/application-proxy.md) için tüm şirket içi uygulamalara bulut erişimi için yayımlanan
   1. Dağıtma [NPS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) herhangi bir RADIUS kimlik doğrulaması için
   1. Kullanıcılar etkin modern kimlik doğrulaması ile desteklenen Microsoft Office sürümleri için yükseltilmiş emin olun.
1. Seçilen yapılandırma [kimlik doğrulama yöntemleri](#choose-verification-options)
1. Tanımlayın, [adlı ağ konumları](../conditional-access/location-condition.md#named-locations)
1. Mfa'yı alınıyor. başlamak için grupları seçin.
1. Yapılandırma, [koşullu erişim ilkeleri](#create-conditional-access-policy)
1. MFA kayıt ilkenizi yapılandırma
   1. [Birleşik MFA ve SSPR](howto-registration-mfa-sspr-combined.md)
   1. İle [kimlik koruması](../identity-protection/howto-mfa-policy.md)
1. Kullanıcı iletişimleri gönderin ve adresindeki kaydetmelerini Al [https://aka.ms/mfasetup](https://aka.ms/mfasetup)
1. [Kimin kayıtlı izler](#identify-non-registered-users)

## <a name="manage-your-solution"></a>Çözümünüzü yönetme

Azure mfa raporları

Azure multi-Factor Authentication, Azure portalı üzerinden raporları sağlar:

| Rapor | Location | Açıklama |
| --- | --- | --- |
| Kullanım ve dolandırıcılık uyarıları | Azure AD > oturum açma işlemleri | Bilgi genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları sağlar. yanı sıra belirtilen tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini. |

## <a name="troubleshoot-mfa-issues"></a>MFA sorunlarını giderme

Çözümleri Azure MFA ile ilgili yaygın sorunları bulmak [Azure multi-Factor Authentication'ı sorun giderme makalesi](https://support.microsoft.com/help/2937344/troubleshooting-azure-multi-factor-authentication-issues) Microsoft Support Center.

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik doğrulama yöntemleri nelerdir?](concept-authentication-methods.md)
* [Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için yakınsanmış kaydını etkinleştirin](concept-registration-mfa-sspr-converged.md)
* Neden bir kullanıcı istemi veya MFA gerçekleştirmek için istenmedi? Bölümüne bakın [Azure AD oturum açma işlemleri raporu Azure multi-Factor Authentication belge raporlardaki](howto-mfa-reporting.md#azure-ad-sign-ins-report).
