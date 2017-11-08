---
title: "İlke: Azure AD SSPR'yi | Microsoft Docs"
description: "Azure AD Self Servis parola sıfırlama ilkesi seçenekleri"
services: active-directory
keywords: "Active directory parola yönetimi, Azure AD parola yönetimi self servis parola sıfırlama"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9d61f46070e6956c60f1135b98a9ebe71011b922
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Parola ilkeleri ve Azure Active Directory'de kısıtlamaları

Bu makalede parola ilkeleri ve Azure AD kiracınızda depolanan kullanıcı hesaplarıyla ilişkili karmaşıklık gereksinimlerini açıklar.

## <a name="administrator-password-policy-differences"></a>Yöneticinin parola ilkesi farklar

Microsoft, tüm Azure yöneticisi rolleri (Örnek: Genel Yönetici, Yardım Masası Yöneticisi, Parola Yöneticisi, vb.) için varsayılan olarak güçlü bir **iki kapılı** parola sıfırlama ilkesi uygular

Bu, yöneticilerin güvenlik soruları kullanarak devre dışı bırakır ve aşağıdaki uygular.

İki ilke, kimlik doğrulama verileri iki parça gerektiren kapı (e-posta adresi **ve** telefon numarası), aşağıdaki durumlarda uygular

* Tüm Azure yönetici rolleri
  * Yardım Masası Yöneticisi
  * Hizmet desteği Yöneticisi
  * Faturalama Yöneticisi
  * İş ortağı Tier1 desteği
  * İş ortağı Tier2 desteği
  * Exchange Hizmet Yöneticisi
  * Lync Hizmet Yöneticisi
  * Kullanıcı Hesap Yöneticisi
  * Directory yazıcılarının
  * Genel yönetici/şirket Yöneticisi
  * SharePoint Hizmet Yöneticisi
  * Uyumluluk Yöneticisi
  * Uygulama Yöneticisi
  * Güvenlik Yöneticisi
  * Ayrıcalıklı Rol Yöneticisi
  * Intune Hizmet Yöneticisi
  * Uygulama Proxy Hizmet Yöneticisi
  * CRM Hizmet Yöneticisi
  * Power BI Hizmet Yöneticisi
  
* 30 gün içinde bir deneme geçen **veya**
* Gösterim etki alanında bulunduğundan (contoso.com) **veya**
* Azure AD Connect, şirket içi dizindeki kimlikleri eşitleme

### <a name="exceptions"></a>Özel durumlar
Kimlik doğrulama verileri bir parçasının gerektiren bir ağ geçidi İlkesi (e-posta adresi **veya** telefon numarası), aşağıdaki durumlarda uygular

* Bir deneme ilk 30 gün **veya**
* Gösterim etki alanında mevcut değil (*. onmicrosoft.com) **ve** kimlikleri Azure AD Connect eşitleme değil


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Tüm kullanıcı hesaplarına uygulanan UserPrincipalName ilkeleri

Azure AD ile oturum açmak için gereken her bir kullanıcı hesabını hesaplarıyla ilişkili benzersiz kullanıcı asıl adı (UPN) öznitelik değeri olması gerekir. Aşağıdaki tabloda ana hatlarını ilkeler her iki Active Directory kullanıcı hesaplarınızı eşitlenen Bulut ve yalnızca bulut kullanıcı hesapları şirket içi Uygula.

| Özellik | UserPrincipalName gereksinimleri |
| --- | --- |
| İzin verilen karakter |<ul> <li>A-Z</li> <li>a - z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Karakterlere izin verilmez |<ul> <li>Bir ' @' kullanıcı adı etki alanından ayırarak değil karakter.</li> <li>Nokta karakteri içeremez '.' hemen önceki ' @' simgesi</li></ul> |
| Uzunluk kısıtlamaları |<ul> <li>Toplam uzunluğu 113 karakterden uzun olamaz</li><li>önce 64 karakteri ' @' simgesi</li><li>48 karakterden sonra ' @' simgesi</li></ul> |

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Yalnızca bulut kullanıcı hesapları için geçerli parola ilkeleri

Aşağıdaki tabloda, oluşturulan ve Azure AD'de yönetilen kullanıcı hesaplarına uygulanabilir kullanılabilir parola ilkesi ayarları açıklanır.

| Özellik | Gereksinimler |
| --- | --- |
| İzin verilen karakter |<ul><li>A-Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Karakterlere izin verilmez |<ul><li>Unicode karakterler</li><li>Alanları</li><li> **Güçlü parolalar**: bir nokta karakterini içeremez '.' hemen önceki ' @' simgesi</li></ul> |
| Parola kısıtlamaları |<ul><li>en az 8 karakter ve en fazla 16 karakter</li><li>**Güçlü parolalar**: 4 aşağıdaki dışında 3 gerektirir:<ul><li>Küçük harf karakterler</li><li>Büyük harf karakterler</li><li>Sayılar (0-9)</li><li>Simgeler (parola kısıtlamaları yukarıdaki bakın)</li></ul></li></ul> |
| Parola geçerlilik süresi |<ul><li>Varsayılan değer: **90** gün </li><li>Azure Active Directory için Windows PowerShell Modülü'ndan Set-MsolPasswordPolicy cmdlet'ini kullanarak yapılandırılabilir değerdir.</li></ul> |
| Parola sona erme bildirimi |<ul><li>Varsayılan değer: **14** (parolasının süresi dolmadan)</li><li>Set-MsolPasswordPolicy cmdlet'ini kullanarak yapılandırılabilir değerdir.</li></ul> |
| Parola süre sonu |<ul><li>Varsayılan değer: **false** gün (Bu parola süre sonu etkinleştirildiğini gösteren) </li><li>Değer Set-MsolUser cmdlet'ini kullanarak bireysel kullanıcı hesapları için yapılandırılabilir. </li></ul> |
| Parola **değiştirmek** geçmişi |Son parola **olamaz** kullanılabilir yeniden ne zaman **değiştirme** bir parola. |
| Parola **sıfırlama** geçmişi | Son parola **olabilir** kullanılabilir yeniden ne zaman **sıfırlama** Unutulan parolayı. |
| Hesap kilitleme |10 başarısız oturum açma girişiminden sonra (yanlış parola), kullanıcı için bir dakika kilitlenir. Daha fazla yanlış oturum açma kullanıcı kilitlenme süreleri artırmak için çalışır. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Parola süre sonu ilkeleri Azure Active Directory'de ayarlayın.

Bir Microsoft bulut hizmeti için genel yönetici kullanıcı parolaları değil süresi dolacak şekilde ayarlamak için Microsoft Azure Active Directory için Windows PowerShell modülü kullanabilirsiniz. Kaldırmak için Windows PowerShell cmdlet'lerini kullanabilirsiniz-yapılandırma süresi asla ya da kullanıcı görmek için parolalar değil ayarlanır süresi dolacak. Bu kılavuz, ayrıca kimlik ve Dizin Hizmetleri için Microsoft Azure Active Directory'ye bağlı Microsoft Intune ve Office 365 gibi diğer sağlayıcıları uygular. Bu, değiştirilebilir ilkenin yalnızca bölümüdür.

> [!NOTE]
> Dizin Eşitleme ile eşitlenmemiş kullanıcı hesapları için yalnızca parola son kullanma tarihi için yapılandırılabilir. Dizin eşitleme hakkında daha fazla bilgi için bkz:[Bağlan Azure AD ile](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Ayarlama veya PowerShell kullanarak parola ilkeleri denetleyin

Başlamak için yapmanız [Azure AD PowerShell modülü yükleyip](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Yüklemediyseniz sonra her bir alan yapılandırmak için aşağıdaki adımları izleyebilirsiniz.

### <a name="how-to-check-expiration-policy-for-a-password"></a>Parola süre sonu ilkesi denetleme
1. Windows PowerShell şirket Yöneticisi kimlik bilgilerinizi kullanarak bağlanın.
2. Aşağıdaki komutlardan birini yürütün:

   * Tek bir kullanıcının parolasını ermeyecek şekilde ayarlanmış olup olmadığını görmek için kullanıcı asıl adı (UPN) kullanarak aşağıdaki cmdlet'i çalıştırın (örneğin, aprilr@contoso.onmicrosoft.com) ya da denetlemek istediğiniz kullanıcının kullanıcı kimliği:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Tüm kullanıcılar için "Parola her zaman geçerli olsun" ayarı görmek için aşağıdaki cmdlet'i çalıştırın:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Parola süresi dolacak şekilde ayarlama

1. Windows PowerShell şirket Yöneticisi kimlik bilgilerinizi kullanarak bağlanın.
2. Aşağıdaki komutlardan birini yürütün:

   * Parola süresinin dolduğu böylece bir kullanıcının parolasını ayarlamak için kullanıcı asıl adı (UPN) kullanarak aşağıdaki cmdlet'i çalıştırın ya da kullanıcının kullanıcı kimliği:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Süreleri doluncaya kuruluşunuzdaki tüm kullanıcıların parolalarının ayarlamak için aşağıdaki cmdlet'i kullanın:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Süresi dolmayacak için bir parola ayarlama

1. Windows PowerShell şirket Yöneticisi kimlik bilgilerinizi kullanarak bağlanın.
2. Aşağıdaki komutlardan birini yürütün:

   * Süresi dolmayacak bir kullanıcının parolasını ayarlamak için kullanıcı asıl adı (UPN) kullanarak aşağıdaki cmdlet'i çalıştırın ya da kullanıcının kullanıcı kimliği:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Tüm kullanıcıların parolalarının süresi dolmayacak kuruluştaki ayarlamak için aşağıdaki cmdlet'i çalıştırın:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

   > [!WARNING]
   > Ayarlarsanız `-PasswordNeverExpires $true` parola hala yaş temel alarak `pwdLastSet` özniteliği. Bu, parolaların süresi dolmayacak ayarlayın ve sonra 90 gün temel alınarak anlamına gelir `pwdLastSet` ve değiştirirseniz `-PasswordNeverExpires $false` sahip tüm parolaları bir `pwdLastSet` 90 gün sonraki oturum açılışında değiştirmeniz gerekecektir daha eski. Bu değişiklik, çok sayıda kullanıcı etkileyebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisans ile ilgili sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
