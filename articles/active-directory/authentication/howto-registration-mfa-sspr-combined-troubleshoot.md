---
title: Azure AD SSPR ve mfa'yı (Önizleme) - Azure Active Directory için birleşik kayıt sorunlarını giderme
description: Azure AD multi-Factor Authentication için sorun giderme ve Self Servis parola sıfırlama birleşik kaydı (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d926f7312b62e788289939dfd81c236a33503b43
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370474"
---
# <a name="troubleshooting-combined-security-information-registration-preview"></a>Sorun giderme birleştirilmiş güvenlik bilgileri kayıt (Önizleme)

Bu makalede sağlanan bilgiler, son kullanıcılar tarafından bildirilen birleşik kayıt deneyimi ile sorunlarını giderme yöneticileri için rehberlik sağlayabilir.

|     |
| --- |
| Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için birleşik güvenlik bilgileri kayıt Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="audit-logs"></a>Denetim günlükleri

Birleşik kayıt için kaydedilen olayları, Azure AD'de "Kimlik doğrulama yöntemleri" kategorisi altında olan denetim günlükleri.

![Arabirim gösteren kayıt olaylarını Azure AD denetim günlükleri](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

Birleşik kayıt tarafından oluşturulan tüm denetim olaylarını listeler:

| Etkinlik | Durum | Neden | Açıklama |
| --- | --- | --- | --- |
| Kullanıcı kayıtlı tüm gerekli güvenlik bilgileri | Başarılı | Kullanıcı, tüm gerekli güvenlik bilgileri kayıtlı. | Bu olay, bir kullanıcı kaydı başarıyla tamamlandığında gerçekleşir.|
| Kullanıcı kayıtlı tüm gerekli güvenlik bilgileri | Hata | Kullanıcı güvenlik bilgileri kayıt iptal edildi. | Bu olay, bir kullanıcı kaydı kesmeyi moddan iptal ettiğinde gerçekleşir.|
| Kayıtlı kullanıcı güvenlik bilgileri | Başarılı | Kullanıcı "method" kayıtlı. | Bu olay, bir kullanıcı, tek bir yöntem kaydeder oluşur. Authenticator uygulaması, telefon, e-posta, "Method" olabilir güvenlik soruları, uygulama parolası, telefon vb.| 
| Gözden geçirilen kullanıcı güvenlik bilgileri | Başarılı | Kullanıcı başarıyla güvenlik bilgilerinizi gözden geçirdi. | Bu olay, bir kullanıcı güvenlik bilgileri gözden geçir sayfasında "İyi görünüyor" tıkladığında gerçekleşir.|
| Gözden geçirilen kullanıcı güvenlik bilgileri | Hata | Kullanıcı, güvenlik bilgilerinizi gözden geçirmek başarısız oldu. | Kullanıcı tıkladığında bu olay, "güvenlik bilgilerini iyi görünüyor" sayfasını gözden geçirin, ancak arka uçtaki bir şey başarısız olursa oluşur.|
| Silinen kullanıcı güvenlik bilgileri | Başarılı | Kullanıcı "method" silindi. | Bu olay bir kullanıcı, tek bir yöntem sildiğinde gerçekleşir. Authenticator uygulaması, telefon, e-posta, "Method" olabilir güvenlik soruları, uygulama parolası, telefon vb.|
| Silinen kullanıcı güvenlik bilgileri | Hata | Kullanıcı, "method" silinemedi. | Bu olay, bir kullanıcı bir yöntem silmeye çalışması ancak herhangi bir nedenle başarısız olduğunda gerçekleşir. Authenticator uygulaması, telefon, e-posta, "Method" olabilir güvenlik soruları, uygulama parolası, telefon vb.|
| Değiştirilen kullanıcı varsayılan güvenlik bilgisi | Başarılı | Kullanıcı "yöntemi için" varsayılan güvenlik bilgileri değiştirildi. | Bu olay, bir kullanıcı kendi varsayılan yöntemini değiştiğinde gerçekleşir. "Yöntemi", kimlik doğrulayıcı uygulama bildirim, my authenticator uygulamadaki veya belirteçteki, çağrı + X XXXXXXXXXX, metin bir kod + X XXXXXXXXX, vb. kod.|
| Değiştirilen kullanıcı varsayılan güvenlik bilgisi | Hata | Kullanıcı, "yöntemi için" varsayılan güvenlik bilgileri değiştirilemedi. | Bu olay, bir kullanıcı kendi varsayılan yöntemini değiştirmek çalışır, ancak herhangi bir nedenle başarısız olduğunda oluşur. "Yöntem" Authenticator uygulama bildirimi, bir kod benim authenticator uygulaması veya belirteç, çağrı + X XXXXXXXXXX, metin bir kod + X XXXXXXXXX, vb. olabilir.|

## <a name="troubleshooting-interrupt-mode"></a>Kesme modu sorunlarını giderme

| Belirti | Sorun giderme adımları |
| --- | --- |
| Görmeyi beklediğiniz yöntemleri görüyorum değil. | 1. Kullanıcı Azure AD Yönetici rolüne sahip olup olmadığını denetleyin. Yanıt Evet ise, SSPR Yönetici İlkesi farklar gözden geçirin. <br> 2. Kullanıcı MFA kaydı zorlama veya SSPR kayıt zorlama nedeniyle kesintiye olup olmadığını belirler. Hangi yöntemlerin gösterileceğini belirlemek için birleşik kayıt modu altında akış gözden geçirin. <br> 3. Mfa'yı veya SSPR İlkesi nasıl kısa bir süre önce değiştirildi belirleyin. Son değişiklik olduysa, güncelleştirilmiş İlke yayılması biraz zaman alabilir.|

## <a name="troubleshooting-manage-mode"></a>Sorun giderme modunu yönetin

| Belirti | Sorun giderme adımları |
| --- | --- |
| Belirli bir yöntemi ekleme seçeneğine sahibim yok. | 1. Yöntem veya SSPR MFA için etkin olup olmadığını belirler. <br> 2. Yöntem etkinleştirilirse, ilkeleri kaydedin ve daha önce 1-2 saat bekleyin. <br> 3. Yöntem etkin olduğunda kullanıcı sayısını ayarlamak için izin verilmeden bu yöntem zaten ayarlamanızı dolmadığından emin olun.|

## <a name="disable-combined-registration"></a>Birleşik kayıt devre dışı bırak

Bir kullanıcı kendi telefon numarasını ve/veya mobil uygulama yeni kaydedildiğinde deneyimi, bizim hizmet Damgalar bayrakları (StrongAuthenticationMethods) söz konusu kullanıcının bu yöntemleri için bir dizi birleştirilmiş. Bu işlev, mfa'yı gerekli olduğunda bu yöntemleri ile Azure multi-Factor Authentication (MFA) gerçekleştirmesine izin verir.

Yeni deneyimde kullanıcıların kayıt yöntemleri Ayarla StrongAuthenticationMethods özelliğine sahip. Bir yönetici Önizleme sağlar, kullanıcıların yeni deneyimde kaydedin ve önizleme yönetici devre dışı bırakır, kullanıcılar farkında olmadan için mfa'yı da kaydedilmiş olabilir.

Tamamlandı bir kullanıcı kaydı birleştirildiğinde, geçerli Self Servis parola sıfırlama (SSPR) kayıt sayfasına gider [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), bu sayfayı erişebilmeniz için önce MFA gerçekleştirmek için istenir. Bu adım, beklenen bir davranış teknik açısından olmakla birlikte SSPR için yalnızca daha önce kaydolan kullanıcılar için bu adım yeni bir davranıştır. Bu ek adım kullanıcının güvenlik ek bir güvenlik düzeyi sağlayarak duruşunu olsa da, yöneticiler artık MFA gerçekleştirebilir, böylece kullanıcılar geri isteyebilirsiniz.  

### <a name="how-to-roll-back-users"></a>Kullanıcılar geri almak nasıl

Yönetici olarak bir kullanıcının MFA ayarlarını sıfırlamak istiyorsanız, bir kullanıcının mobil uygulamasına ve/veya telefon numarası için StrongAuthenticationMethods özelliği temizleyecek bir PowerShell Betiği oluşturduk. Kullanıcılarınız için bu betiğin çalıştırılması, bunlar gerekirse MFA için yeniden kaydetmeniz gerekir anlamına gelir. Etkilenen tüm kullanıcıları geri önce geri alma bir veya iki kullanıcı ile test etmenizi öneririz.

Aşağıdaki adımları, kullanıcı veya kullanıcı grubuna geri yardımcı olur:

#### <a name="prerequisites"></a>Önkoşullar

1. Uygun Azure AD PowerShell modüllerine yüklemeniz gerekir. Bir PowerShell penceresinde modüllerini yüklemek için şu komutları çalıştırın:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Etkilenen kullanıcı nesne kimliği/kimlikleri listesini makinenize her satırda bir Kimliğe sahip bir metin dosyası olarak kaydedin. Dosya konumunu not edin.
1. Aşağıdaki komut dosyasını makinenize kaydedin ve komut dosyası konumunu not edin:

```powershell
<# 
//********************************************************
//*                                                      *
//*   Copyright (C) Microsoft. All rights reserved.      *
//*                                                      *
//********************************************************
#>

param($path)

# Define Remediation Fn
function RemediateUser {

    param  
    (
        $ObjectId
    )

    $user = Get-MsolUser -ObjectId $ObjectId

    Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

    $hasMfaRelyingParty = $false
    foreach($p in $user.StrongAuthenticationRequirements)
    {
        if ($p.RelyingParty -eq "*")
        {
            $hasMfaRelyingParty = $true
            Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
        }
    }

    if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
    {
        Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
        Write-Host "Rolling back user ..." -ForegroundColor Yellow
        Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
        Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
    }
    else
    {
        Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
    }

    Write-Host ""
    Start-Sleep -Milliseconds 750
}

# Connect
Import-Module MSOnline
Connect-MsolService

foreach($line in Get-Content $path)
{
    RemediateUser -ObjectId $line
}
```

#### <a name="rollback"></a>Geri alma

Vurgulanan konumları güncelleştirdikten sonra bir PowerShell penceresinde aşağıdaki komutu çalıştırın. İstendiğinde genel yönetici kimlik bilgilerini girin. Betik, her kullanıcı güncelleştirme işleminin sonucu çıkarır.

`<script location> -path <user file location>`

### <a name="disable-preview-experience"></a>Önizleme deneyimini devre dışı bırak

Kullanıcılarınız için Önizleme deneyimini devre dışı bırakmak için aşağıdaki adımları tamamlayın:

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.
2. Gözat **Azure Active Directory**, **kullanıcı ayarları**, **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilir**, seçici kümesine **hiçbiri** tıklatıp **Kaydet**.

Kullanıcılar artık Önizleme deneyimini kullanarak kaydolmanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure multi-Factor Authentication ve Self Servis parola sıfırlama için birleşik kayıt genel önizleme hakkında daha fazla bilgi edinin](concept-registration-mfa-sspr-combined.md)
