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
ms.openlocfilehash: 5c33f08e54d522e0eea13a3e267f14f407fc59b6
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
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

Bir Microsoft bulut hizmeti için genel yönetici kullanıcı parolaları değil süresi dolacak şekilde ayarlamak için Microsoft Azure Active Directory için Windows PowerShell modülü kullanabilirsiniz. Kaldırmak için Windows PowerShell cmdlet'lerini kullanabilirsiniz-yapılandırma süresi asla ya da kullanıcı görmek için parolalar değil ayarlanır süresi dolacak. Bu kılavuz, ayrıca kimlik ve Dizin Hizmetleri için Microsoft Azure Active Directory'ye bağlı Microsoft Intune ve Office 365 gibi diğer sağlayıcıları uygular.

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

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [SSPR başarılı bir sunum nasıl tamamlamak?](active-directory-passwords-best-practices.md)
* [Sıfırlama veya parolanızı değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md).
* [Bir lisans soru var mı?](active-directory-passwords-licensing.md)
* [Hangi verilerin SSPR tarafından kullanılır ve hangi verilerin, kullanıcılarınız için doldurmanız gerekir?](active-directory-passwords-data.md)
* [Hangi kimlik doğrulama yöntemlerinin kullanıcıların var mı?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Parola geri yazma nedir ve neden t hakkında önemli?](active-directory-passwords-writeback.md)
* [SSPR etkinliğinde üzerinde nasıl rapor edebilirim?](active-directory-passwords-reporting.md)
* [Tüm SSPR seçeneklerinde nedir ve ne anlama geldiklerini?](active-directory-passwords-how-it-works.md)
* [Bir şey bozuk düşünüyorum. SSPR nasıl sorun giderme?](active-directory-passwords-troubleshoot.md)
* [Herhangi bir yerde else kapsanmayan bir soru sahip](active-directory-passwords-faq.md)
