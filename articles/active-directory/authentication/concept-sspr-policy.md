---
title: Azure AD Self Servis parola sıfırlama ilkeleri - Azure Active Directory
description: Azure AD Self Servis parola sıfırlama ilkesi seçenekleri yapılandırma
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d9055ef11bc5c117efc6d4de87d4ca8ec73a661
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60359033"
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Parola ilkeleri ve Azure Active Directory'de kısıtlamaları

Bu makalede, Azure Active Directory (Azure AD) kiracınız içindeki kullanıcı hesapları ile ilişkili karmaşıklık gereksinimleri ve parola ilkelerini açıklar.

## <a name="administrator-reset-policy-differences"></a>Yönetici İlkesi farklar Sıfırla

**Microsoft, varsayılan olarak güçlü bir zorlar *iki ağ geçidi* parola sıfırlama İlkesi tüm Azure yöneticisi rolleri için** Bu ilke, kullanıcılarınız için tanımladığınız ve değiştirilemez parçacıktan farklı olabilir. Her zaman bir kullanıcı olarak parola sıfırlama işlevselliği test etmelisiniz atanmış tüm Azure yöneticisi rollerini olmadan.

İki ağ geçidi İlkesi ile **Yöneticiler, güvenlik sorularını kullan olanağı yok**.

İki ağ geçidi ilkesi kimlik doğrulama verilerini iki parça gibi gerektiren bir **e-posta adresi**, **authenticator uygulamasını**, veya bir **telefon numarası**. İki ağ geçidi İlkesi, aşağıdaki durumlarda geçerlidir:

* Aşağıdaki Azure yöneticisi rollerini etkilenir:
  * Yardım Masası Yöneticisi
  * Hizmet desteği Yöneticisi
  * Faturalama yöneticisi
  * Partner Tier1 Desteği
  * Partner Tier2 Desteği
  * Exchange yöneticisi
  * Skype Kurumsal yöneticisi
  * Kullanıcı yöneticisi
  * Dizin yazıcıları
  * Genel yönetici veya şirket Yöneticisi
  * SharePoint yöneticisi
  * Uyumluluk yöneticisi
  * Uygulama yöneticisi
  * Güvenlik yöneticisi
  * Ayrıcalıklı rol yöneticisi
  * Intune yöneticisi
  * Uygulama Ara sunucusu Hizmet Yöneticisi
  * Dynamics 365 yöneticisi
  * Power BI Hizmet Yöneticisi
  * Kimlik doğrulaması yöneticisi
  * Ayrıcalıklı kimlik yöneticisi

* 30 günlük bir deneme aboneliği geçtiyse; veya
* Gösterim etki alanı contoso.com gibi mevcut değil; veya
* Azure AD Connect, şirket içi dizininizden kimlikler eşitleme

### <a name="exceptions"></a>Özel durumlar

Tek parça bir e-posta adresi gibi kimlik doğrulama verilerinin bir ağ geçidi İlkesi gerektirir *veya* telefon numarası. Bir ağ geçidi İlkesi, aşağıdaki durumlarda geçerlidir:

* Bu, ilk 30 gün içinde bir deneme aboneliği olur; veya
* Gösterim etki alanı mevcut değil (*. onmicrosoft.com); ve
* Azure AD Connect kimlik eşitleme değil

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Tüm kullanıcı hesaplarına uygulanan UserPrincipalName ilkeleri

Azure AD'de oturum açmak için gereken her bir kullanıcı hesabını hesaplarıyla ilişkili benzersiz kullanıcı asıl adı (UPN) öznitelik değeri olması gerekir. Aşağıdaki tabloda, bulutta ve yalnızca bulut kullanıcı hesapları için eşitlenir hem şirket içi Active Directory kullanıcı hesaplarına uygulanan tüm ilkeler özetlenmektedir:

| Özellik | UserPrincipalName gereksinimleri |
| --- | --- |
| İzin verilen karakter |<ul> <li>A – Z</li> <li>a - z</li><li>0 – 9</li> <li> ' \. - \_ ! \# ^ \~</li></ul> |
| Karakterlere izin verilmez |<ul> <li>Tüm "\@ \" kullanıcı etki alanı adından ayırarak olmayan karakter.</li> <li>Nokta karakteri içeremez "." hemen "\@ \" simgesi</li></ul> |
| Uzunluk kısıtlamaları |<ul> <li>Toplam uzunluğu 113 karakterden uzun olmamalıdır</li><li>Önce en fazla 64 karakter olabilir "\@ \" simgesi</li><li>Sonra en fazla 48 karakter olabilir "\@ \" simgesi</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Yalnızca bulut kullanıcı hesapları için geçerli bir parola ilkeleri

Aşağıdaki tabloda, oluşturulan ve Azure AD'de yönetilen kullanıcı hesaplarına uygulanan parola ilkesi ayarları açıklanır:

| Özellik | Gereksinimler |
| --- | --- |
| İzin verilen karakter |<ul><li>A – Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / \` ~ " ( ) ;</li></ul> |
| Karakterlere izin verilmez |<ul><li>Unicode karakter.</li><li>Alanları.</li><li> Nokta karakteri içeremez "." hemen "\@ \" sembol".</li></ul> |
| Parola kısıtlamaları |<ul><li>En az 8 karakter ve en fazla 16 karakter.</li><li>Üç tanesi dört birini gerektirir:<ul><li>Küçük harf karakterler.</li><li>Büyük harf karakterler.</li><li>Sayılar (0-9).</li><li>Semboller (önceki parola kısıtlamalarını bakın).</li></ul></li></ul> |
| Parola sona erme süresi |<ul><li>Varsayılan değer: **90** gün.</li><li>Değeri kullanılarak yapılandırılabilir `Set-MsolPasswordPolicy` Azure Active Directory modülü için Windows PowerShell cmdlet'i.</li></ul> |
| Parola süre sonu bildirimi |<ul><li>Varsayılan değer: **14** (parola süresi dolmadan).</li><li>Değeri kullanılarak yapılandırılabilir `Set-MsolPasswordPolicy` cmdlet'i.</li></ul> |
| Parola kullanım süresi sonu |<ul><li>Varsayılan değer: **false** gün (Bu parola süre sonu etkin olduğunu gösterir).</li><li>Değeri kullanarak bireysel kullanıcı hesapları için yapılandırılabilir `Set-MsolUser` cmdlet'i.</li></ul> |
| Parola değişiklik geçmişi |Son parola *olamaz* kullanılabilir yeniden kullanıcı parola değiştirdiğinde. |
| Parola geçmişini Sıfırla | Son parola *olabilir* yeniden kullanıcı Unutulan parolayı sıfırlar sağladığınızda kullanılır. |
| Hesap kilitleme |10 başarısız oturum açma denemeden sonra yanlış parola ile kullanıcı için bir dakika kilitlidir. Daha fazla yanlış oturum açma kullanıcı kilitleme süreleri artırmaya yönelik çalışır. [Akıllı kilitleme](howto-password-smart-lockout.md) aynı parola kilidi açma sayacı artıyor olmasını önlemek için son üç hatalı parola karmaları izler. Birisi birden çok kez aynı yanlış parola girerse, bu davranış, hesap kilitleme için neden olmaz. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Parola, Azure AD'de süre sonu ilkelerini ayarlama

Bir genel yönetici veya Kullanıcı Yöneticisi bir Microsoft bulut hizmeti için Windows PowerShell için Microsoft Azure AD modülü kullanıcı parolalarını dolmayacak şekilde ayarlamak için kullanabilirsiniz. Kaldırmak için Windows PowerShell cmdlet'lerini kullanabilirsiniz-yapılandırma süresi asla veya kullanıcı görmek için parolaları süresiz olarak ayarlanır. 

Bu kılavuz, ayrıca Azure AD için kimlik ve dizin hizmetleri kullanan Intune ve Office 365 gibi diğer sağlayıcılar için geçerlidir. Parola kullanım süresi değiştirilebilir ilke yalnızca parçasıdır.

> [!NOTE]
> Yalnızca dizin eşitleme ile eşitlenmemiş kullanıcı hesaplarının parolalarını dolmayacak şekilde yapılandırılabilir. Dizin eşitleme hakkında daha fazla bilgi için bkz: [AD ile Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>Ayarlayın veya PowerShell kullanarak parola ilkelerini denetleyin

Başlamak için yapmanız [Azure AD PowerShell modülünü indirip](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Yüklü sonra her bir alan yapılandırmak için aşağıdaki adımları kullanabilirsiniz.

### <a name="check-the-expiration-policy-for-a-password"></a>Onay için bir parola süre sonu ilkesi

1. Windows PowerShell için kullanıcı yönetici veya şirket Yöneticisi kimlik bilgilerini kullanarak bağlanın.
1. Aşağıdaki komutlardan birini yürütün:

   * Tek bir kullanıcının parolasını ermeyecek şekilde ayarlanmış olup olmadığını görmek için UPN kullanarak aşağıdaki cmdlet'i çalıştırın (örneğin, *aprilr\@contoso.onmicrosoft.com*) ya da kontrol etmek istediğiniz kullanıcının kullanıcı kimliği: `Get-AzureADUser -ObjectId <user ID> | Select-Object @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}`
   * Görmek için **parola her zaman geçerli olsun** ayar tüm kullanıcılar için aşağıdaki cmdlet'i çalıştırın: `Get-AzureADUser -All $true | Select-Object UserPrincipalName, @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}`

### <a name="set-a-password-to-expire"></a>Parola süresi dolacak şekilde ayarlayın

1. Windows PowerShell için kullanıcı yönetici veya şirket Yöneticisi kimlik bilgilerini kullanarak bağlanın.
1. Aşağıdaki komutlardan birini yürütün:

   * Parolanın süresi dolarsa, bir kullanıcının parolasını ayarlamak için UPN veya kullanıcının kullanıcı Kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın: `Set-AzureADUser -ObjectId <user ID> -PasswordPolicies None`
   * Bu süre kuruluştaki tüm kullanıcıların parolalarının ayarlamak için aşağıdaki cmdlet'i kullanın: `Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies None`

### <a name="set-a-password-to-never-expire"></a>Parola süresi dolmayacak şekilde ayarlayın

1. Windows PowerShell için kullanıcı yönetici veya şirket Yöneticisi kimlik bilgilerini kullanarak bağlanın.
1. Aşağıdaki komutlardan birini yürütün:

   * Süresiz olarak bir kullanıcı parolasını ayarlamak için UPN veya kullanıcının kullanıcı Kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın: `Set-AzureADUser -ObjectId <user ID> -PasswordPolicies DisablePasswordExpiration`
   * Parolanın süresi dolmayacak kuruluştaki tüm kullanıcılar için aşağıdaki cmdlet'i çalıştırın: `Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies DisablePasswordExpiration`

   > [!WARNING]
   > Parolaları kümesine `-PasswordPolicies DisablePasswordExpiration` hala göre yaş `pwdLastSet` özniteliği. Süresi dolmayacak kullanıcı parolaları ayarlama ve sonra 90 gün gidin, parolalarının süresinin dolmasını. Temel `pwdLastSet` özniteliği için sona erme değiştirirseniz `-PasswordPolicies None`, gereken tüm parolaların bir `pwdLastSet` 90 günde bir sonraki oturum açışlarında değiştirmeye gerektirmek daha eski. Bu değişiklik, kullanıcıların çok sayıda etkileyebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler Azure AD aracılığıyla parola sıfırlama hakkında daha fazla bilgi sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md).
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
