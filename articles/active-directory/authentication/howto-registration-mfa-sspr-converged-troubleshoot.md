---
title: Azure AD SSPR ve mfa'yı (genel Önizleme) için yakınsanmış kaydını devre dışı bırak
description: Azure AD multi-Factor Authentication devre dışı bırakın ve Self Servis parola sıfırlama kaydı (genel Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: c7cdd2e62c76011f2ff4ada33353b85dcf564ef5
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076664"
---
# <a name="disable-azure-ad-converged-registration-public-preview"></a>Azure AD devre dışı bırakma yakınsanmış kayıt (genel Önizleme)

Bir kullanıcı kendi telefon numarasını ve/veya mobil uygulama yeni kaydedildiğinde deneyimi, bizim hizmet Damgalar bayrakları (StrongAuthenticationMethods) söz konusu kullanıcının bu yöntemleri için bir dizi yakınsanmış. Bu işlev, mfa'yı gerekli olduğunda bu yöntemleri ile Azure multi-Factor Authentication (MFA) gerçekleştirmesine izin verir.

Yeni deneyimde kullanıcıların kayıt yöntemleri Ayarla StrongAuthenticationMethods özelliğine sahip. Genel Önizleme kullanıma hazır olduğunda bu davranış de gerçekleşir. Bir yönetici Önizleme sağlar, kullanıcıların yeni deneyimde kaydedin ve önizleme yönetici devre dışı bırakır, kullanıcılar farkında olmadan için mfa'yı da kaydedilmiş olabilir.

Kayıt tamamlandı bir kullanıcıyı yakınsanmış varsa, geçerli SSPR kayıt sayfasına gider [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), bu sayfayı erişebilmeniz için önce MFA gerçekleştirmek için istenir. Bu adım, beklenen bir davranış teknik açısından olmakla birlikte SSPR için yalnızca daha önce kaydolan kullanıcılar için bu adım yeni bir davranıştır. Bu ek adım kullanıcının güvenlik ek bir güvenlik düzeyi sağlayarak duruşunu olsa da, yöneticiler artık MFA gerçekleştirebilir, böylece kullanıcılar geri isteyebilirsiniz.  

## <a name="how-to-roll-back-users"></a>Kullanıcılar geri almak nasıl

Yönetici olarak bir kullanıcının MFA ayarlarını sıfırlamak istiyorsanız, bir kullanıcının mobil uygulamasına ve/veya telefon numarası için StrongAuthenticationMethods özelliği temizleyecek bir PowerShell Betiği oluşturduk. Kullanıcılarınız için bu betiğin çalıştırılması, bunlar gerekirse MFA için yeniden kaydetmeniz gerekir anlamına gelir. Etkilenen tüm kullanıcıları geri önce geri alma bir veya iki kullanıcı ile test etmenizi öneririz.

Aşağıdaki adımları, kullanıcı veya kullanıcı grubuna geri yardımcı olur:

### <a name="pre-requisites"></a>Ön koşullar

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

### <a name="rollback"></a>Geri alma

Vurgulanan konumları güncelleştirdikten sonra bir PowerShell penceresinde aşağıdaki komutu çalıştırın. İstendiğinde genel yönetici kimlik bilgilerini girin. Betik, her kullanıcı güncelleştirme işleminin sonucu çıkarır.

`<script location> -path <user file location>`

## <a name="disable-preview-experience"></a>Önizleme deneyimini devre dışı bırak

Kullanıcılarınız için Önizleme deneyimini devre dışı bırakmak için aşağıdaki adımları tamamlayın:

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.
2. Gözat **Azure Active Directory**, **kullanıcı ayarları**, **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilir**, seçici kümesine **hiçbiri** tıklatıp **Kaydet**.

Kullanıcılar artık Önizleme deneyimini kullanarak kaydolmanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

[Self Servis parola sıfırlama ve Azure multi-Factor Authentication için yakınsanmış kaydının genel önizleme hakkında daha fazla bilgi edinin](concept-registration-mfa-sspr-converged.md)
