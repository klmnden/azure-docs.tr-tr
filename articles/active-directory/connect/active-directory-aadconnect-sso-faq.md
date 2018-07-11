---
title: 'Azure AD Connect: Sorunsuz çoklu oturum sık sorulan sorular açma - | Microsoft Docs'
description: İlgili Azure Active Directory sorunsuz çoklu oturum açma sık sorulan soruların yanıtları.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 8e4cc67af4276bc244d402258a90dfec01d61add
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919028"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory sorunsuz çoklu oturum açma: sık sorulan sorular

Bu makalede, biz sık sorulan sorular hakkında Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz çoklu oturum açma) adresi. Yeni içerik için geri kontrol etmeyi unutmayın.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Oturum açma yöntemleri ile sorunsuz çoklu oturum açma çalışır?

Sorunsuz çoklu oturum açma ile birleştirilebilir [parola karması eşitleme](active-directory-aadconnectsync-implement-password-hash-synchronization.md) veya [geçişli kimlik doğrulaması](active-directory-aadconnect-pass-through-authentication.md) oturum açma yöntemleri. Ancak bu özellik Active Directory Federasyon Hizmetleri (ADFS) ile kullanılamaz.

## <a name="is-seamless-sso-a-free-feature"></a>Sorunsuz çoklu oturum açma, ücretsiz bir özellik mi var?

Sorunsuz çoklu oturum açma ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri gerekmez.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Sorunsuz çoklu oturum açma kullanılabilir [Microsoft Azure Almanya bulut](http://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/)?

Hayır. Sorunsuz çoklu oturum açma, yalnızca dünya çapındaki örneğini Azure AD içinde kullanılabilir.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Hangi uygulamaların `domain_hint` veya `login_hint` parametresi sorunsuz çoklu oturum açma yeteneğini?

Aşağıda listelenen uygulamaları Azure AD'ye bu parametreleri gönderin ve bu nedenle kullanıcıların sorunsuz çoklu oturum açma (yani, kullanıcılarınızın, kullanıcı adlarını giriş gerek) kullanarak sessiz oturum açma deneyimi sağlar, kapsamlı olmayan bir listesidir:

| Uygulama adı | Kullanılacak uygulama URL'si |
| -- | -- |
| Erişim paneli | myapps.microsoft.com/contoso.com |
| Web üzerinde Outlook'u | Outlook.Office365.com/contoso.com |

Ayrıca, diğer bir deyişle, Azure AD'nin kiralanan uç noktalar için - uygulamanın oturum açma isteği gönderirse, sessiz bir oturum açma deneyimi kullanıcıların alma https://login.microsoftonline.com/contoso.com/<..> veya https://login.microsoftonline.com/<tenant_ID>/<..> - Azure AD'nin ortak uç nokta - diğer bir deyişle, yerine https://login.microsoftonline.com/common/<...>. Aşağıda listelenen, oturum açma istekleri bu tür uygulamalar, kapsamlı olmayan bir listesidir.

| Uygulama adı | Kullanılacak uygulama URL'si |
| -- | -- |
| SharePoint Online | contoso.SharePoint.com |
| Azure portalına | Portal.Azure.com/contoso.com |

Yukarıdaki tablolarda, "contoso.com" etki alanı adınızın kiracınız için doğru uygulama URL'lere almak için değiştirin.

Sessiz oturum açma deneyimimizi kullanarak diğer uygulamaları istiyorsanız geri bildirim bölümünde bize bildirin.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Sorunsuz çoklu oturum açmayı destekliyor mu `Alternate ID` kullanıcı adı olarak yerine `userPrincipalName`?

Evet. Sorunsuz çoklu oturum açmayı destekleyen `Alternate ID` gösterildiği gibi Azure AD Connect yapılandırılmış kullanıcı adı olarak [burada](active-directory-aadconnect-get-started-custom.md). Tüm Office 365 uygulamaları desteklemez `Alternate ID`. Destek bildirimi için belirli bir uygulamanın belgelerine bakın.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>Hangi çoklu oturum açma deneyimini arasındaki farkı tarafından sağlanan [Azure AD'ye katılımı](../active-directory-azureadjoin-overview.md) ve sorunsuz çoklu oturum açma?

[Azure AD'ye katılım](../active-directory-azureadjoin-overview.md) cihazlarını Azure AD'ye kaydedildiyse, kullanıcılara SSO sağlar. Bu cihazlar mutlaka etki alanına katılmış olması gerekmez. SSO kullanarak sağlanan *birincil yenileme belirteçleri* veya *PRTs*ve Kerberos değil. Windows 10 cihazlarda en iyi kullanıcı deneyimi. SSO, Edge tarayıcının otomatik olarak gerçekleşir. Bir tarayıcı uzantısı kullanımı ile Chrome üzerinde de çalışır.

Kiracınızda Azure AD Join ve sorunsuz çoklu oturum açma kullanabilirsiniz. Bu iki özellik tamamlayıcıdır. Her iki özelliği açıksa, Azure AD'ye katılım'nden SSO sorunsuz çoklu oturum açma öncelik kazanır.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>AD FS kullanmadan Windows 10 cihazlarını Azure AD'ye kaydetme istiyorsunuz. Sorunsuz çoklu oturum açma yerine kullanabilir miyim?

Evet, bu senaryo 2.1 veya sonraki sürümü gerekir [çalışma alanına katılma istemcisi](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Nasıl miyim geri Kerberos şifre çözme anahtarı `AZUREADSSOACC` bilgisayar hesabı?

Kerberos şifre çözme anahtarı sık geri önemlidir `AZUREADSSOACC` şirket içinde oluşturulan (Azure AD temsil eder) bilgisayar hesabını AD ormanı.

>[!IMPORTANT]
>En azından her 30 günde Kerberos şifre çözme anahtarını başa döndürmek önemle öneririz.

Azure AD Connect çalıştırdığınız şirket içi sunucusunda bu adımları izleyin:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. Adım Sorunsuz çoklu oturum açma yeri etkinleştirilmiş AD ormanına listesini alın

1. İlk olarak, indirme ve yükleme [Microsoft Online Services oturum açma Yardımcısı](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Ardından indirme ve yükleme [64 bit Windows PowerShell için Azure Active Directory Modülü](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).
3. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
4. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
5. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girmek için bir açılan pencere vermeniz gerekir.
6. Çağrı `Get-AzureADSSOStatus`. Bu komut, bu özelliğin etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesinde bakın) listesini sağlar.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>2. Adım Bu, üzerinde ayarlanmış her AD ormanında Kerberos şifre çözme anahtarı güncelleştirme

1. Çağrı `$creds = Get-Credential`. İstendiğinde, hedeflenen AD ormanı için etki alanı yöneticisi kimlik bilgilerini girin.
2. Çağrı `Update-AzureADSSOForest -OnPremCredentials $creds`. Bu komut için Kerberos şifre çözme anahtarı güncelleştirir `AZUREADSSOACC` bu belirli AD ormanında bilgisayar hesabı ve Azure AD'de güncelleştirir.
3. Özelliği ayarladığınız her AD ormanı için önceki adımları yineleyin.

>[!IMPORTANT]
>Emin olun, _yoksa_ çalıştırma `Update-AzureADSSOForest` birden çok kez komutu. Aksi takdirde, bu özellik, kullanıcılarınızın Kerberos biletleri süresinin dolmasını ve şirket içi Active Directory tarafından yeniden zamana kadar çalışmayı durduruyor.

## <a name="how-can-i-disable-seamless-sso"></a>Sorunsuz çoklu oturum açma nasıl devre dışı bırakabilirim?

Azure AD Connect kullanarak sorunsuz çoklu oturum açma devre dışı bırakılabilir.

Azure AD Connect'i çalıştırın, "Değişiklik kullanıcı oturum açma sayfasında" öğesini seçin ve "İleri" ye tıklayın. Ardından "çoklu oturum açmayı etkinleştir" seçeneğinin işaretini kaldırın. Sihirbaza devam edin. Sihirbaz tamamlandıktan sonra kiracınızda sorunsuz çoklu oturum açma devre dışı bırakıldı.

Ancak, aşağıdaki ekranda bir ileti görürsünüz:

"Çoklu oturum açma devre dışı bırakıldı, ancak temizliği tamamlamak için gereken ek el ile adımlar vardır. Daha fazla bilgi edinin"

İşlemi tamamlamak için Azure AD Connect çalıştırdığınız adımları el ile şirket içi sunucu üzerinde izleyin:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. Adım Sorunsuz çoklu oturum açma yeri etkinleştirilmiş AD ormanına listesini alın

1. İlk olarak, indirme ve yükleme [Microsoft Online Services oturum açma Yardımcısı](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Ardından indirme ve yükleme [64 bit Windows PowerShell için Azure Active Directory Modülü](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
4. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
5. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girmek için bir açılan pencere vermeniz gerekir.
6. Çağrı `Get-AzureADSSOStatus`. Bu komut, bu özelliğin etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesinde bakın) listesini sağlar.

### <a name="step-2-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>2. Adım El ile silmeniz `AZUREADSSOACCT` listelenen gördüğünüz her AD ormanında bilgisayar hesabı.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-sso-quick-start.md) - getirmek ve Azure AD sorunsuz çoklu oturum açma çalışıyor.
- [**Teknik yakından bakışın** ](active-directory-aadconnect-sso-how-it-works.md) -bu özelliği nasıl çalıştığını anlayın.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.
