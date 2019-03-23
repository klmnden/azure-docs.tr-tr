---
title: Azure AD SSPR veri gereksinimleri - Azure Active Directory
description: Veri gereksinimleri için Azure AD Self Servis parola sıfırlama ve bunları karşılamak nasıl
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a0d7edb6c7faafcad55e827c2d9e3d2eeea40f5
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371375"
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Son kullanıcı kaydı istemeden parola sıfırlamayı dağıtma

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) dağıtmak için kimlik doğrulama verilerini mevcut olması gerekir. Bazı kuruluşların kullanıcıları kendi kimlik doğrulama verilerini girin vardır. Ancak birçok kuruluşun Active Directory'de zaten var. veri ile eşitlemek tercih edin. Azure AD'ye eşitlenmiş veriler kullanılabilir hale gelir ve SSPR, kullanıcı etkileşimi gerektirmeden:

* Şirket içi dizininizdeki verileri düzgün bir şekilde biçimlendirin.
* Yapılandırma [hızlı ayarları kullanarak Azure AD Connect](../hybrid/how-to-connect-install-express.md).

Düzgün çalışması için telefon numaraları biçiminde olmalıdır *+ CountryCode PhoneNumber*, örneğin, + 1 4255551234.

> [!NOTE]
> Orada ülke kodunu ve telefon numarası arasına bir boşluk olması gerekir.
>
> Parola sıfırlama telefon dahili numaralarına desteklemez. Kurulmadan önce bile X + 1 4255551234 12345 biçiminde uzantılar kaldırılır.

## <a name="fields-populated"></a>Doldurulmuş alanları

Azure AD Connect varsayılan ayarları kullanıyorsanız, aşağıdaki eşlemeler oluşturulur:

| Şirket içi Active Directory | Azure AD |
| --- | --- |
| telephoneNumber | Ofis telefonu |
| Mobil | Cep telefonu |

Bir kullanıcı kullanıcıdan birincil telefonu numarasını doğruladıktan sonra Azure AD'de kimlik doğrulaması iletişim bilgileri altında telefon alan ayrıca numaralı doldurulur.

## <a name="authentication-contact-info"></a>Kimlik doğrulaması iletişim bilgileri

Genel yönetici, aşağıdaki ekran görüntüsünde gösterilen şekilde bir kullanıcı için kimlik doğrulaması iletişim bilgileri el ile ayarlayabilirsiniz.

![Kimlik doğrulaması, Azure AD'de bir kullanıcı hakkında bilgiler başvurun][Contact]

Telefon alan doldurulur ve cep telefonu SSPR ilkesinde etkin kullanıcı parola sıfırlama kayıt sayfasında ve sırasında parola sıfırlama iş akışı görürsünüz.

Alternatif telefon alan parola sıfırlama için kullanılmaz.

E-posta alanı doldurulur ve e-posta SSPR ilkesinde etkinleştirilirse, kullanıcının e-posta parola sıfırlama kayıt sayfasında ve sırasında parola sıfırlama iş akışı görürsünüz.

Alternatif e-posta alanı doldurulur ve e-posta SSPR ilkesinde etkin ise kullanıcı olacak **değil** e-parola sıfırlama kayıt sayfasına, ancak bunu sırasında parola sıfırlama iş akışı görürler bakın.

## <a name="security-questions-and-answers"></a>Güvenlik sorularını ve yanıtlarını

Güvenlik sorularını ve yanıtlarını, Azure AD kiracınızda güvenli bir şekilde depolanır ve yalnızca kullanıcılara erişilebilir [SSPR kayıt portalı](https://aka.ms/ssprsetup). Yöneticiler göremez, ayarlama veya başka kullanıcıların sorularını ve yanıtlarını içeriğini değiştirin.

## <a name="what-happens-when-a-user-registers"></a>Bir kullanıcı kayıt olurkenki ne olur

Bir kullanıcı kayıt olurkenki, aşağıdaki alanlar kayıt sayfasına ayarlar:

* **Kimlik doğrulama telefonu**
* **Kimlik doğrulama e-posta**
* **Güvenlik sorularını ve yanıtlarını**

İçin bir değer sağlanmışsa **cep telefonu** veya **alternatif e-posta**, kullanıcıların hemen kullanabilir bu değerleri kendi parolalarını sıfırlamak için bile için hizmet kayıtlı olmayabilirsiniz. Ayrıca, kullanıcılar ilk kez kaydetmek ve isterlerse bunlar değiştirebilmek bu değerleri kullanıcılar görür. Başarıyla kaydettikten sonra bu değerler, kalıcı **kimlik doğrulama telefonu** ve **kimlik doğrulama e-posta** alanlar, sırasıyla.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Ayarlayın ve PowerShell aracılığıyla kimlik doğrulama verilerini okuma

Aşağıdaki alanlar, PowerShell aracılığıyla ayarlanabilir:

* **Alternatif e-posta**
* **Cep telefonu**
* **Ofis telefonu**: Bir şirket içi dizininizle eşitleme değil, yalnızca ayarlanabilir

### <a name="use-powershell-version-1"></a>PowerShell sürüm 1'i kullanın

Başlamak için yapmanız [Azure AD PowerShell modülünü indirip](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Yüklü sonra her bir alan yapılandırmak için aşağıdaki adımları kullanabilirsiniz.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini okuyun

```PowerShell
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Kimlik doğrulama telefon numarasını ve kimlik doğrulama e-posta seçeneklerini okuyun

Okunacak **kimlik doğrulama telefonu** ve **kimlik doğrulama e-posta** PowerShell sürüm 1'i kullandığınızda, aşağıdaki komutları kullanın:

```PowerShell
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>PowerShell sürüm 2 kullanın

Başlamak için yapmanız [Azure AD sürüm 2 PowerShell modülünü indirip](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0). Yüklü sonra her bir alan yapılandırmak için aşağıdaki adımları kullanabilirsiniz.

Install-Module destekleyen yeni sürümlerinden PowerShell hızlı bir şekilde yüklemek için aşağıdaki komutları çalıştırın. (İlk satır modül zaten yüklü olup olmadığını denetler.)

```PowerShell
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verilerini okuyun

```PowerShell
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "Genel Yöneticiler bir kullanıcının kimlik doğrulaması iletişim bilgileri değiştirebilirsiniz."
