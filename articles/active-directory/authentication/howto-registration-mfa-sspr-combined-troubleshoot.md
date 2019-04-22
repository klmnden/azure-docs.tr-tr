---
title: Azure AD SSPR ve multi-Factor Authentication (Önizleme) - Azure Active Directory için birleşik kayıt sorunlarını giderme
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
ms.openlocfilehash: 40918493071fe0dd694c43e2b087a2bf7eb197d8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59489202"
---
# <a name="troubleshooting-combined-security-information-registration-preview"></a>Sorun giderme birleştirilmiş güvenlik bilgileri kayıt (Önizleme)

Bu makaledeki bilgiler, birleşik kayıt deneyimi kullanıcıları tarafından bildirilen sorunları giderirken yöneticileri kılavuza yöneliktir.

|     |
| --- |
| Azure multi-Factor Authentication ve Azure Active Directory (Azure AD) Self Servis parola sıfırlama için birleşik güvenlik bilgileri kayıt bir Azure AD genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="audit-logs"></a>Denetim günlükleri

Birleşik kayıt için kaydedilen olayları, Azure AD'de kimlik doğrulama yöntemleri kategorisinde olan denetim günlükleri.

![Arabirim gösteren kayıt olaylarını Azure AD denetim günlükleri](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

Aşağıdaki tablo, birleştirilen kaydı tarafından oluşturulan tüm denetim olaylarını listeler:

| Etkinlik | Durum | Neden | Açıklama |
| --- | --- | --- | --- |
| Kullanıcı kayıtlı tüm gerekli güvenlik bilgileri | Başarılı | Kullanıcı, tüm gerekli güvenlik bilgileri kayıtlı. | Bu olay, bir kullanıcı kaydı başarıyla tamamlandığında gerçekleşir.|
| Kullanıcı kayıtlı tüm gerekli güvenlik bilgileri | Hata | Kullanıcı güvenlik bilgileri kayıt iptal edildi. | Bu olay, bir kullanıcı kaydı kesmeyi moddan iptal ettiğinde gerçekleşir.|
| Kayıtlı kullanıcı güvenlik bilgileri | Başarılı | Kayıtlı kullanıcı *yöntemi*. | Bu olay, bir kullanıcı, tek bir yöntem kaydeder oluşur. *Yöntemi* Authenticator uygulama, telefon, e-posta, güvenlik soruları, uygulama parola, telefon vb. olabilir.| 
| Gözden geçirilen kullanıcı güvenlik bilgileri | Başarılı | Kullanıcı başarıyla güvenlik bilgilerinizi gözden geçirdi. | Bu olay, bir kullanıcı seçtiğinde gerçekleşir **iyi görünüyor** güvenlik bilgileri gözden geçir sayfasında.|
| Gözden geçirilen kullanıcı güvenlik bilgileri | Hata | Kullanıcı, güvenlik bilgilerinizi gözden geçirmek başarısız oldu. | Bu olay, bir kullanıcı seçtiğinde gerçekleşir **iyi görünüyor** sayfasında güvenlik bilgilerini gözden ancak bir şey arka uçta başarısız olur.|
| Silinen kullanıcı güvenlik bilgileri | Başarılı | Silinen kullanıcı *yöntemi*. | Bu olay bir kullanıcı, tek bir yöntem sildiğinde gerçekleşir. *Yöntemi* Authenticator uygulama, telefon, e-posta, güvenlik soruları, uygulama parola, telefon vb. olabilir.|
| Silinen kullanıcı güvenlik bilgileri | Hata | Kullanıcı silme başarısız oldu *yöntemi*. | Bu olay, bir kullanıcı bir yöntem silmeye çalışması ancak deneme herhangi bir nedenle başarısız olduğunda gerçekleşir. *Yöntemi* Authenticator uygulama, telefon, e-posta, güvenlik soruları, uygulama parola, telefon vb. olabilir.|
| Değiştirilen kullanıcı varsayılan güvenlik bilgisi | Başarılı | Kullanıcı varsayılan güvenlik bilgilerini değiştirdi *yöntemi*. | Bu olay, bir kullanıcının varsayılan yöntemini değiştiğinde gerçekleşir. *Yöntemi* Authenticator uygulama bildirimi, bir kod benim authenticator uygulaması veya belirteç, çağrı + X XXXXXXXXXX, metin, kod + X XXXXXXXXX olması ve benzeri.|
| Değiştirilen kullanıcı varsayılan güvenlik bilgisi | Hata | Varsayılan güvenlik bilgilerini değiştirmek kullanıcı başarısız *yöntemi*. | Bu olay, bir kullanıcının varsayılan yöntemini değiştirmek çalışır ancak deneme herhangi bir nedenle başarısız olduğunda oluşur. *Yöntemi* Authenticator uygulama bildirimi, bir kod benim authenticator uygulaması veya belirteç, çağrı + X XXXXXXXXXX, metin, kod + X XXXXXXXXX olması ve benzeri.|

## <a name="troubleshooting-interrupt-mode"></a>Kesme modu sorunlarını giderme

| Belirti | Sorun giderme adımları |
| --- | --- |
| Görmeyi beklediğiniz yöntemleri görüyorum değil. | 1. Kullanıcı Azure AD Yönetici rolüne sahip olup olmadığını denetleyin. Yanıt Evet ise, SSPR Yönetici İlkesi farkları görüntüleyin. <br> 2. Kullanıcı multi-Factor Authentication kayıt zorlama veya SSPR kayıt zorlama nedeniyle kesintiye olup olmadığını belirler. Bkz: [akış](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md#combined-registration-modes) "Birleştirilmiş kayıt modları" altında hangi yöntemlerin gösterileceğini belirlemek için. <br> 3. Çok faktörlü kimlik doğrulaması veya SSPR İlkesi nasıl kısa bir süre önce değiştirildi belirleyin. Son değişiklik olduysa, güncelleştirilmiş İlke yayılması biraz zaman alabilir.|

## <a name="troubleshooting-manage-mode"></a>Sorun giderme modunu yönetin

| Belirti | Sorun giderme adımları |
| --- | --- |
| Belirli bir yöntemi ekleme seçeneğine sahibim yok. | 1. Yöntem SSPR veya çok faktörlü kimlik doğrulaması için etkin olup olmadığını belirler. <br> 2. Yöntem etkinleştirilirse, ilkeleri yeniden kaydedin ve daha önce 1-2 saat bekleyin. <br> 3. Yöntem etkin olduğunda kullanıcı sayısını ayarlamak için izin verilmeden bu yöntem zaten ayarlamanızı dolmadığından emin olun.|

## <a name="disable-combined-registration"></a>Birleşik kayıt devre dışı bırak

Bir kullanıcı bir telefon numarası ve/veya mobil uygulama yeni kaydedildiğinde deneyimi, bizim hizmet Damgalar bayrakları (StrongAuthenticationMethods) söz konusu kullanıcının bu yöntemleri için bir dizi birleştirilmiş. Bu işlev çok faktörlü kimlik doğrulaması gerekli olduğunda bu yöntemleriyle çok faktörlü kimlik doğrulaması gerçekleştirmesine izin verir.

Bir yönetici Önizleme sağlar, kullanıcıların yeni deneyimde kaydedin ve önizleme yönetici devre dışı bırakır, kullanıcılar bilmeden için multi-Factor Authentication da kaydedilmiş olabilir.

Birleşik kayıt dolduran bir kullanıcı geçerli Self Servis parola sıfırlama (SSPR) kayıt sayfasına gider, [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), kullanıcı erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmek üzere istenir Bu sayfa. Bu adım teknik açıdan bekleniyordu ancak SSPR için yalnızca daha önce kaydolan kullanıcılar için yeni olan. Başka bir güvenlik düzeyini sağlayarak, bu ek adım kullanıcının güvenlik duruşunu iyileştirmek de, yöneticiler artık çok faktörlü kimlik doğrulamasını yapamaz böylece kullanıcılar geri isteyebilirsiniz.  

### <a name="how-to-roll-back-users"></a>Kullanıcılar geri almak nasıl

Bir yönetici olarak, bir kullanıcının çok faktörlü kimlik doğrulaması ayarlarını sıfırlamak istiyorsanız, sonraki bölümde verilen PowerShell betiğini kullanabilirsiniz. Betik StrongAuthenticationMethods özelliği bir kullanıcının mobil uygulamasına ve/veya telefon numarası için temizler. Kullanıcılarınız için bu betiği çalıştırırsanız, çok faktörlü kimlik doğrulamasına ihtiyaç duyarlarsa yeniden kaydetmeniz gerekir. Test geri alma ile bir veya iki kullanıcı tüm etkilenen kullanıcılar geri önce öneririz.

Sonraki adımlarda, bir kullanıcı veya kullanıcı grubuna geri yardımcı olur.

#### <a name="prerequisites"></a>Önkoşullar

1. Uygun Azure AD PowerShell modüllerini yükleyin. Bir PowerShell penceresinde modüllerini yüklemek için şu komutları çalıştırın:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Etkilenen kullanıcı nesne kimlikleri listesini bilgisayarınıza her satırda bir Kimliğe sahip bir metin dosyası olarak kaydedin. Dosya konumunu not edin.
1. Aşağıdaki betiği bilgisayarınıza kaydedin ve komut dosyası konumunu not edin:

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

Bir PowerShell penceresinde betik ve kullanıcı dosya konumları sağlayan aşağıdaki komutu çalıştırın. İstendiğinde genel yönetici kimlik bilgilerini girin. Betik, her kullanıcı güncelleştirme işleminin sonucu çıkarır.

`<script location> -path <user file location>`

### <a name="disable-the-preview-experience"></a>Önizleme deneyimini devre dışı bırak

Kullanıcılarınız için Önizleme deneyimini devre dışı bırakmak için aşağıdaki adımları tamamlayın:

1. Azure portalında bir kullanıcının yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **kullanıcı ayarları** > **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilir**, seçici kümesine **hiçbiri**ve ardından **Kaydet**.

Kullanıcılar artık Önizleme deneyimini kullanarak kaydetmek için istenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure multi-Factor Authentication ve Self Servis parola sıfırlama için birleşik kayıt genel önizleme hakkında daha fazla bilgi edinin](concept-registration-mfa-sspr-combined.md)
