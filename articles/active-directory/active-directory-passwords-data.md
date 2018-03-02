---
title: Azure AD SSPR'yi veri gereksinimleri | Microsoft Docs
description: "Veri gereksinimleri için Azure AD Self Servis parola sıfırlama ve bunları karşılamak nasıl"
services: active-directory
keywords: 
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
ms.openlocfilehash: 929843825d19c003b5a97363a03ffdd3ae2a2f7d
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Parola sıfırlama son kullanıcı kayıt gerektirmeden dağıtma

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) dağıtmak için kimlik doğrulama verileri mevcut olması gerekir. Bazı kuruluşlar kendi kimlik doğrulama verileri girin, kullanıcılar sahiptir. Ancak, Active Directory'de zaten var. veri ile eşitlemek birçok kuruluş tercih eder. Azure AD ile eşitlenmiş veriler kullanılabilir hale getirilir ve SSPR ise kullanıcı etkileşimi gerektirmeden:
   * Düzgün bir şekilde şirket içi dizininizdeki verileri biçimlendirin.
   * Yapılandırma [hızlı ayarları kullanarak Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md).

Düzgün çalışması için telefon numaraları biçiminde olmalıdır *+ CountryCode PhoneNumber*, örneğin, + 1 4255551234.

> [!NOTE]
> Var. ülke kodunu ve telefon numarası arasında bir boşluk olması gerekir.
>
> Parola sıfırlama telefon uzantıları desteklemez. Kurulmadan önce bile + 1 4255551234 X 12345 biçiminde uzantıları kaldırılır.

## <a name="fields-populated"></a>Doldurulmuş alanları

Azure AD Connect varsayılan ayarları kullanırsanız, aşağıdaki eşlemelerini oluşturulur:

| Şirket içi Active Directory | Azure AD | Azure AD kimlik doğrulama kişi bilgisi |
| --- | --- | --- |
| telephoneNumber | Ofis telefonu | Alternatif telefon |
| Mobil | Cep telefonu | Telefon |

Bir kullanıcı kimlik doğrulama verilerini onaylayıncaya kadar bu alanları boş görünebilir.

Genel yönetici, aşağıdaki ekran görüntüsünde gösterildiği kullanıcı için kimlik doğrulama kişi bilgilerini el ile ayarlayabilirsiniz.

![lgili kişi][Contact]

## <a name="security-questions-and-answers"></a>Güvenlik sorularını ve yanıtlarını

Güvenlik sorularını ve yanıtlarını Azure AD kiracınızda güvenli bir şekilde depolanır ve yalnızca üzerinden kullanıcılara erişilebilir [SSPR kayıt portalı](https://aka.ms/ssprsetup). Yöneticiler bakın veya başka kullanıcıların sorularını ve yanıtlarını içeriğini değiştirin.

### <a name="what-happens-when-a-user-registers"></a>Kullanıcı kayıtları ne olur

Bir kullanıcı kaydettiğinde kayıt sayfası aşağıdaki alanları ayarlar:

* **Kimlik doğrulama telefon**
* **Kimlik doğrulama e-posta**
* **Güvenlik sorularını ve yanıtlarını**

İçin bir değer sağladıysanız **cep telefonu** veya **alternatif e-posta**, kullanıcıların hemen kullanabilir bu değerleri kendi parolalarını sıfırlamak için hizmet için kaydolmadıysanız bile. Ayrıca, kullanıcılar ilk kez kaydetmek ve etmek istiyorsanız, bunları değiştirebilir bu değerleri bakın. İçinde başarıyla kaydettikten sonra bu değerler kalıcıdır **kimlik doğrulama telefon** ve **kimlik doğrulama e-posta** alanlar, sırasıyla.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Ayarlama ve PowerShell aracılığıyla kimlik doğrulama verileri okuma

Aşağıdaki alanları PowerShell aracılığıyla ayarlanabilir:

* **Alternatif e-posta**
* **Cep telefonu**
* **Ofis telefonu**: yalnızca, bir şirket içi diziniyle değil eşitliyorsanız ayarlanabilir

### <a name="use-powershell-version-1"></a>PowerShell sürüm 1 kullanma

Başlamak için yapmanız [Azure AD PowerShell modülü yükleyip](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Yüklemediyseniz sonra her bir alan yapılandırmak için izlediği adımları kullanabilirsiniz.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulaması veri kümesi

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verileri okuma

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Kimlik doğrulama telefon ve kimlik doğrulama e-posta seçeneklerini okuyun

Okunacak **kimlik doğrulama telefon** ve **kimlik doğrulama e-posta** PowerShell sürüm 1 kullandığınızda, aşağıdaki komutları kullanın:

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>PowerShell sürüm 2 kullanın

Başlamak için yapmanız [Azure AD sürüm 2 PowerShell modülünü yükleyip](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0). Yüklemediyseniz sonra her bir alan yapılandırmak için izlediği adımları kullanabilirsiniz.

Install-Module destekleyen en son sürümlerinden PowerShell hızlı bir şekilde yüklemek için aşağıdaki komutları çalıştırın. (İlk satırı modül zaten yüklü olup olmadığını denetler.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulaması veri kümesi

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verileri okuma

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-data/user-authentication-contact-info.png "Genel Yöneticiler bir kullanıcının kimlik doğrulaması kişi bilgilerini değiştirebilirsiniz"
