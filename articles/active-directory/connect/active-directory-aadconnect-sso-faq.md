---
title: "Azure AD Connect: Sorunsuz çoklu oturum açma sık sorulan sorular özelliğini - | Microsoft Docs"
description: "İlgili Azure Active Directory sorunsuz çoklu oturum açma sık sorulan soruların yanıtları."
services: active-directory
keywords: "Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: billmath
ms.openlocfilehash: a91e46d3aa0b04015b60837ae4aef020acd42279
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory sorunsuz çoklu oturum açma: sık sorulan sorular

Bu makalede, ilgili Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz SSO) sık sorulan sorular adres. Yeni içerik için geri denetleniyor tutun.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Oturum açma yöntemleri ile sorunsuz SSO çalışıyor mu?

Sorunsuz SSO ile birleştirilebilir [parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-synchronization.md) veya [doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md) oturum açma yöntemleri. Ancak bu özellik Active Directory Federasyon Hizmetleri (ADFS) ile kullanılamaz.

## <a name="is-seamless-sso-a-free-feature"></a>Sorunsuz SSO boş bir özellik mi var?

Sorunsuz SSO boş bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri olması gerekmez.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Sorunsuz SSO kullanılabilir [Microsoft Azure Almanya bulut](http://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/)?

Hayır. Sorunsuz SSO yalnızca Azure AD dünya çapındaki örneğini içinde kullanılabilir.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Hangi uygulamaların yararlanmak `domain_hint` veya `login_hint` sorunsuz SSO parametre yeteneğini?

Bu parametreler ve verme olanları Gönder uygulamaların listesini derleme sürecinde duyuyoruz. İlginizi çekiyor mu uygulamalarınız varsa, açıklamalar bölümünde bize bildirin.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Sorunsuz SSO desteklemiyor `Alternate ID` kullanıcı adı olarak yerine `userPrincipalName`?

Evet. Sorunsuz SSO destekleyen `Alternate ID` içinde Azure AD Connect gösterildiği gibi yapılandırıldığında kullanıcı adı olarak [burada](active-directory-aadconnect-get-started-custom.md). Tüm Office 365 uygulamaları desteklemez `Alternate ID`. Destek deyimi için belirli uygulama belgelerine bakın.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>AD FS kullanarak olmadan Azure AD ile Windows 10 cihazları kaydetmek istediğiniz. Sorunsuz SSO yerine kullanabilir miyim?

Evet, bu senaryo 2.1 veya sonraki sürümü gerekir [çalışma alanına katılma istemcisi](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Nasıl miyim alma Kerberos şifre çözme anahtarı `AZUREADSSOACC` bilgisayar hesabı?

Sık Kerberos şifre çözme anahtarı alma önemlidir `AZUREADSSOACC` şirket içinde oluşturulan (Azure AD temsil eder) bilgisayar hesabını AD orman.

>[!IMPORTANT]
>En az 30 günde Kerberos şifre çözme anahtarı alma öneririz.

Azure AD Connect çalıştırdığınız şirket içi sunucu üzerinde aşağıdaki adımları izleyin:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. Adım AD ormanına sorunsuz SSO etkinleştirdiğiniz listesini al

1. İlk olarak, indirme ve yükleme [Microsoft Online Services oturum açma Yardımcısı](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Ardından karşıdan yükleyip [64-bit Windows PowerShell için Azure Active Directory Modülü](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
4. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
5. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracının genel Yöneticisi kimlik bilgilerini girmek için açılan pencere vermesi gerekir.
6. Çağrı `Get-AzureADSSOStatus`. Bu komut bu özellik etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesi bakın) listesini sağlar.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>2. Adım Bunu üzerinde oluşturulduğuna her bir AD ormanda Kerberos şifre çözme anahtarını güncelleştir

1. Çağrı `$creds = Get-Credential`. İstendiğinde, hedeflenen AD orman için etki alanı yönetici kimlik bilgilerini girin.
2. Çağrı `Update-AzureADSSOForest -OnPremCredentials $creds`. Bu komut için Kerberos şifre çözme anahtarı güncelleştirir `AZUREADSSOACC` bu belirli AD ormanında bilgisayar hesabını ve Azure AD içinde güncelleştirir.
3. Özelliği ayarladığınız her bir AD orman için önceki adımları yineleyin.

>[!IMPORTANT]
>Sağlamak sizin _yok_ çalıştırmak `Update-AzureADSSOForest` birden çok kez komutu. Aksi takdirde, bu özellik, kullanıcılarınızın Kerberos biletleri süresinin dolmasını ve şirket içi Active Directory tarafından yeniden zamana kadar çalışmayı durdurur.

## <a name="how-can-i-disable-seamless-sso"></a>Sorunsuz SSO nasıl devre dışı bırakabilirim?

Azure AD Connect'i kullanarak sorunsuz SSO devre dışı bırakılabilir.

Azure AD Connect çalıştırın, "Değiştirme kullanıcı oturum açma sayfası" seçin ve "İleri" yi tıklatın. Ardından "çoklu oturum açmayı etkinleştir" seçeneğinin işaretini kaldırın. Sihirbaza devam edin. Sihirbaz tamamlandıktan sonra Kiracı'sorunsuz SSO devre dışı bırakılır.

Ancak, aşağıdaki gibi okur ekranda bir ileti görür:

"Çoklu oturum açma artık devre dışıdır, ancak temizleyin tamamlamak üzere gerçekleştirmeniz gereken ek el ile adım vardır. Daha fazla bilgi edinin"

İşlemi tamamlamak için Azure AD Connect çalıştırdığınız adımları el ile şirket içi sunucuda izleyin:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. Adım AD ormanına sorunsuz SSO etkinleştirdiğiniz listesini al

1. İlk olarak, indirme ve yükleme [Microsoft Online Services oturum açma Yardımcısı](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Ardından karşıdan yükleyip [64-bit Windows PowerShell için Azure Active Directory Modülü](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
4. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
5. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracının genel Yöneticisi kimlik bilgilerini girmek için açılan pencere vermesi gerekir.
6. Çağrı `Get-AzureADSSOStatus`. Bu komut bu özellik etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesi bakın) listesini sağlar.

### <a name="step-2-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>2. Adım El ile silmeniz `AZUREADSSOACCT` listelenen gördüğünüz her AD ormanındaki bilgisayar hesabı.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-sso-quick-start.md) - hale getirmek ve Azure AD sorunsuz SSO çalışıyor.
- [**Teknik derinlemesine** ](active-directory-aadconnect-sso-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
