---
title: "İlkeleri - Azure Active Directory Self Servis parola sıfırlama"
description: "Azure AD Self Servis parola sıfırlama ilkesi seçenekleri"
services: active-directory
keywords: "Active directory parola yönetimi, Azure AD parola yönetimi self servis parola sıfırlama"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 51eb7c594c02310c083b42b6c0d964b4af239d76
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Parola ilkeleri ve Azure Active Directory'de kısıtlamaları

Bu makalede parola ilkeleri ve Azure Active Directory (Azure AD) kiracınızda depolanan kullanıcı hesaplarıyla ilişkili karmaşıklık gereksinimlerini açıklar.

## <a name="administrator-password-policy-differences"></a>Yöneticinin parola ilkesi farklar

Microsoft güçlü bir varsayılan zorlar *iki ağ geçidi* parola sıfırlama için herhangi bir Azure Yönetici rolü ilkesi. 

İki ağ geçidi İlkesi ile Yöneticiler güvenlik soruları kullanmanıza olanak yoktur.

 Bir e-posta adresi gibi kimlik doğrulama verileri iki parça iki ağ geçidi İlkesi gerektiriyor *ve* bir telefon numarası. İki ağ geçidi ilke aşağıdaki durumlarda uygulanır:

* Aşağıdaki tüm Azure yönetici rolleri etkilenir:
  * Yardım Masası Yöneticisi
  * Hizmet desteği Yöneticisi
  * Faturalama yöneticisi
  * Partner Tier1 Desteği
  * Partner Tier2 Desteği
  * Exchange hizmeti yöneticisi
  * Lync hizmeti yöneticisi
  * Kullanıcı Hesap Yöneticisi
  * Dizin yazıcılar
  * Genel yönetici veya şirket Yöneticisi
  * SharePoint Hizmet Yöneticisi
  * Uyumluluk yöneticisi
  * Uygulama yöneticisi
  * Güvenlik yöneticisi
  * Ayrıcalıklı rol yöneticisi
  * Microsoft Intune Hizmet Yöneticisi
  * Uygulama proxy Hizmet Yöneticisi
  * CRM Hizmet Yöneticisi
  * Power BI Hizmet Yöneticisi
  
* 30 gün içinde bir deneme aboneliği geçtiyse

  or

* Contoso.com gibi bir gösterim etki alanında yok

  or

* Azure AD Connect, şirket içi dizindeki kimlikleri eşitleme

### <a name="exceptions"></a>Özel durumlar
Bir ağ geçidi ilkesi tek bir e-posta adresi gibi kimlik doğrulama veri parçası gerektiriyor *veya* telefon numarası. Bir ağ geçidi ilke aşağıdaki durumlarda uygulanır:

* Bir deneme aboneliği, ilk 30 gün içinde olduğu

  or

* Bir gösterim etki alanında yer almayan (*. onmicrosoft.com) 

  ve 

  Azure AD Connect kimlikleri eşitlemeyi değil


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Tüm kullanıcı hesaplarına uygulanan UserPrincipalName ilkeleri

Azure AD ile oturum açmak için gereken her bir kullanıcı hesabını hesaplarıyla ilişkili benzersiz kullanıcı asıl adı (UPN) öznitelik değeri olması gerekir. Aşağıdaki tabloda ana hatlarını ilkeler buluta ve yalnızca bulut kullanıcı hesaplarına eşitlenir hem şirket içi Active Directory kullanıcı hesapları için geçerlidir:

| Özellik | UserPrincipalName gereksinimleri |
| --- | --- |
| İzin verilen karakter |<ul> <li>A-Z</li> <li>a - z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Karakterlere izin verilmez |<ul> <li>Tüm "@" kullanıcı etki alanı adından ayırarak olmayan karakter.</li> <li>Nokta karakteri içeremez "." hemen önceki "@" simgesi</li></ul> |
| Uzunluk kısıtlamaları |<ul> <li>Toplam uzunluğu 113 karakterden uzun olamaz</li><li>Önce en fazla 64 karakter olabilir "@" simgesi</li><li>Sonra en fazla 48 karakter olabilir "@" simgesi</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Yalnızca bulut kullanıcı hesapları için geçerli parola ilkeleri

Aşağıdaki tabloda, oluşturulan ve Azure AD'de yönetilen kullanıcı hesaplarına uygulanabilir kullanılabilir parola ilkesi ayarları açıklanır:

| Özellik | Gereksinimler |
| --- | --- |
| İzin verilen karakter |<ul><li>A-Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Karakterlere izin verilmez |<ul><li>Unicode karakterler.</li><li>Alanları.</li><li> Güçlü parolalar: bir nokta karakterini içeremez "." hemen önceki "@" simgesi.</li></ul> |
| Parola kısıtlamaları |<ul><li>En az 8 karakter ve en fazla 16 karakter.</li><li>Güçlü parolalar: dışı üç dört birini gerektirir:<ul><li>Küçük harf karakterler.</li><li>Büyük harf karakterler.</li><li>Sayılar (0-9).</li><li>Simgeler (önceki parola kısıtlamaları bakın).</li></ul></li></ul> |
| Parola geçerlilik süresi |<ul><li>Varsayılan değer: **90** gün.</li><li>Kullanarak yapılandırılabilir bir değerdir `Set-MsolPasswordPolicy` Azure Active Directory modülü için Windows PowerShell cmdlet'i.</li></ul> |
| Parola sona erme bildirimi |<ul><li>Varsayılan değer: **14** (parolasının süresi dolmadan).</li><li>Kullanarak yapılandırılabilir bir değerdir `Set-MsolPasswordPolicy` cmdlet'i.</li></ul> |
| Parola süre sonu |<ul><li>Varsayılan değer: **false** gün (Bu parola süre sonu etkinleştirildiğini gösteren).</li><li>Değeri kullanarak bireysel kullanıcı hesapları için yapılandırılabilir `Set-MsolUser` cmdlet'i.</li></ul> |
| Parola değişiklik geçmişi |Son parola *olamaz* kullanılabilir yeniden kullanıcının parola değiştiğinde. |
| Parola geçmişini Sıfırla | Son parola *için* yeniden kullanıcı Unutulan parolayı sıfırlar sağladığınızda kullanılır. |
| Hesap kilitleme |10 başarısız oturum açma girişiminden sonra yanlış parola ile kullanıcı için bir dakika kilitlidir. Daha fazla yanlış oturum açma kullanıcı kilitlenme süreleri artırmak için çalışır. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Parola süre sonu ilkelerini Azure AD'de ayarlayın.

Bir Microsoft bulut hizmeti için genel yönetici kullanıcı parolalarını değil süresi dolacak şekilde ayarlamak için Windows PowerShell için Microsoft Azure AD modülü kullanabilirsiniz. Kaldırmak için Windows PowerShell cmdlet'lerini kullanabilirsiniz-yapılandırma süresi asla veya hangi kullanıcısını görmek için parola hiç süresi dolacak şekilde ayarlanmıştır. 

Bu kılavuz, Azure AD kimlik ve Dizin Hizmetleri için de Bel Intune ve Office 365 gibi diğer sağlayıcıları için geçerlidir. Parola geçerlilik süresi değiştirilebilir ilkesi yalnızca parçasıdır.

> [!NOTE]
> Dizin Eşitleme ile eşitlenmemiş kullanıcı hesapları için yalnızca parola son kullanma tarihi için yapılandırılabilir. Dizin eşitleme hakkında daha fazla bilgi için bkz: [Bağlan Azure AD ile](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>Ayarlama veya PowerShell kullanarak parola ilkeleri denetleyin

Başlamak için yapmanız [Azure AD PowerShell modülü yükleyip](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Yüklemediyseniz sonra her bir alan yapılandırmak için aşağıdaki adımları kullanabilirsiniz.

### <a name="how-to-check-the-expiration-policy-for-a-password"></a>Parola süre sonu ilkesi denetleme
1. Windows PowerShell, şirket Yöneticisi kimlik bilgilerinizi kullanarak bağlanın.
2. Aşağıdaki komutlardan birini yürütün:

   * Tek bir kullanıcının parolasını ermeyecek şekilde ayarlanmış olup olmadığını görmek için UPN kullanarak aşağıdaki cmdlet'i çalıştırın (örneğin,  *aprilr@contoso.onmicrosoft.com* ) ya da denetlemek istediğiniz kullanıcının kullanıcı kimliği:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Görmek için **parola her zaman geçerli olsun** tüm kullanıcılar için ayarlama, aşağıdaki cmdlet'i çalıştırın:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Parola süresi dolacak şekilde ayarlama

1. Windows PowerShell, şirket Yöneticisi kimlik bilgilerinizi kullanarak bağlanın.
2. Aşağıdaki komutlardan birini yürütün:

   * Parola süresinin dolduğu şekilde bir kullanıcının parolasını ayarlamak için UPN veya kullanıcının kullanıcı Kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Süreleri doluncaya kuruluşunuzdaki tüm kullanıcıların parolalarının ayarlamak için aşağıdaki cmdlet'i kullanın:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Süresi dolmayacak için bir parola ayarlama

1. Windows PowerShell, şirket Yöneticisi kimlik bilgilerinizi kullanarak bağlanın.
2. Aşağıdaki komutlardan birini yürütün:

   * Süresi dolmayacak bir kullanıcının parolasını ayarlamak için UPN veya kullanıcının kullanıcı Kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Tüm kullanıcıların parolalarının süresi dolmayacak kuruluştaki ayarlamak için aşağıdaki cmdlet'i çalıştırın:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

   > [!WARNING]
   > Parolalar ayarlamak `-PasswordNeverExpires $true` hala bağlı yaş `pwdLastSet` özniteliği. Süresi dolmayacak kullanıcı parolaları ayarlamanız ve ardından 90 gün gidin, parolaların süreleri. Temel `pwdLastSet` bitiş tarihine değiştirirseniz, öznitelik `-PasswordNeverExpires $false`, gereken tüm parolaların bir `pwdLastSet` 90 gün oturum açtığında değiştirmek kullanıcının gerektiren daha eski. Bu değişiklik, çok sayıda kullanıcı etkileyebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler parola Azure AD sıfırlama hakkında ek bilgi sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
