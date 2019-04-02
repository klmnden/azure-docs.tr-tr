---
title: 'Azure AD Connect: Sorunsuz çoklu oturum sık sorulan sorular açma - | Microsoft Docs'
description: İlgili Azure Active Directory sorunsuz çoklu oturum açma sık sorulan soruların yanıtları.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/14/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 709fb3be37850be37d6378652921ce26f4ff15fe
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804386"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory sorunsuz çoklu oturum açma: Sık sorulan sorular

Bu makalede, biz sık sorulan sorular hakkında Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz çoklu oturum açma) adresi. Yeni içerik için geri kontrol etmeyi unutmayın.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Oturum açma yöntemleri ile sorunsuz çoklu oturum açma çalışır?

Sorunsuz çoklu oturum açma ile birleştirilebilir [parola karması eşitleme](how-to-connect-password-hash-synchronization.md) veya [geçişli kimlik doğrulaması](how-to-connect-pta.md) oturum açma yöntemleri. Ancak bu özellik Active Directory Federasyon Hizmetleri (ADFS) ile kullanılamaz.

## <a name="is-seamless-sso-a-free-feature"></a>Sorunsuz çoklu oturum açma, ücretsiz bir özellik mi var?

Sorunsuz çoklu oturum açma ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri gerekmez.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpswwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Sorunsuz çoklu oturum açma kullanılabilir [Microsoft Azure Almanya bulut](https://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/)?

Hayır. Sorunsuz çoklu oturum açma, yalnızca dünya çapındaki örneğini Azure AD içinde kullanılabilir.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Hangi uygulamaların `domain_hint` veya `login_hint` parametresi sorunsuz çoklu oturum açma yeteneğini?

Aşağıda listelenen, bu parametreleri Azure AD'ye gönderebilir ve bu nedenle kullanıcıların sorunsuz çoklu oturum açma (yani, kullanıcılarınızın, kullanıcı adlarını veya parolaları giriş gerek) kullanarak sessiz oturum açma deneyimi sağlayan uygulamalar kapsamlı olmayan bir listesidir:

| Uygulama adı | Kullanılacak uygulama URL'si |
| -- | -- |
| Erişim paneli | https:\//myapps.microsoft.com/contoso.com |
| Web üzerinde Outlook'u | https:\//outlook.office365.com/contoso.com |
| Office 365 portalı | https:\//portal.office.com?domain_hint=contoso.com, https:\//www.office.com?domain_hint=contoso.com |

Uygulamanın oturum açma istekleri için Azure AD'nin uç noktaları ayarlama - kiracıları olarak diğer bir deyişle, https gönderirse kullanıcılar sessiz bir oturum açma deneyimi ayrıca Al:\//login.microsoftonline.com/contoso.com/ <... > ya da https:\//login.microsoftonline.com/ < Kiracı > / <... > - Azure AD'nin ortak uç nokta - diğer bir deyişle, https yerine:\//login.microsoftonline.com/common/ <... >. Aşağıda listelenen, oturum açma istekleri bu tür uygulamalar, kapsamlı olmayan bir listesidir.

| Uygulama adı | Kullanılacak uygulama URL'si |
| -- | -- |
| SharePoint Online | https:\//contoso.sharepoint.com |
| Azure portal | https:\//portal.azure.com/contoso.com |

Yukarıdaki tablolarda, "contoso.com" etki alanı adınızın kiracınız için doğru uygulama URL'lere almak için değiştirin.

Sessiz oturum açma deneyimimizi kullanarak diğer uygulamaları istiyorsanız geri bildirim bölümünde bize bildirin.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Sorunsuz çoklu oturum açmayı destekliyor mu `Alternate ID` kullanıcı adı olarak yerine `userPrincipalName`?

Evet. Sorunsuz çoklu oturum açmayı destekleyen `Alternate ID` gösterildiği gibi Azure AD Connect yapılandırılmış kullanıcı adı olarak [burada](how-to-connect-install-custom.md). Tüm Office 365 uygulamaları desteklemez `Alternate ID`. Destek bildirimi için belirli bir uygulamanın belgelerine bakın.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>Hangi çoklu oturum açma deneyimini arasındaki farkı tarafından sağlanan [Azure AD'ye katılımı](../active-directory-azureadjoin-overview.md) ve sorunsuz çoklu oturum açma?

[Azure AD'ye katılım](../active-directory-azureadjoin-overview.md) cihazlarını Azure AD'ye kaydedildiyse, kullanıcılara SSO sağlar. Bu cihazlar mutlaka etki alanına katılmış olması gerekmez. SSO kullanarak sağlanan *birincil yenileme belirteçleri* veya *PRTs*ve Kerberos değil. Windows 10 cihazlarda en iyi kullanıcı deneyimi. SSO, Microsoft Edge tarayıcısında otomatik olarak gerçekleşir. Bir tarayıcı uzantısı kullanımı ile Chrome üzerinde de çalışır.

Kiracınızda Azure AD Join ve sorunsuz çoklu oturum açma kullanabilirsiniz. Bu iki özellik tamamlayıcıdır. Her iki özelliği açıksa, Azure AD'ye katılım'nden SSO sorunsuz çoklu oturum açma öncelik kazanır.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>AD FS kullanmadan Windows 10 cihazlarını Azure AD'ye kaydetme istiyorsunuz. Sorunsuz çoklu oturum açma yerine kullanabilir miyim?

Evet, bu senaryo 2.1 veya sonraki sürümü gerekir [çalışma alanına katılma istemcisi](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Nasıl miyim geri Kerberos şifre çözme anahtarı `AZUREADSSOACC` bilgisayar hesabı?

Kerberos şifre çözme anahtarı sık geri önemlidir `AZUREADSSOACC` şirket içinde oluşturulan (Azure AD temsil eder) bilgisayar hesabını AD ormanı.

>[!IMPORTANT]
>En azından her 30 günde Kerberos şifre çözme anahtarını başa döndürmek önemle öneririz.

Azure AD Connect çalıştırdığınız şirket içi sunucusunda bu adımları izleyin:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. Adım Sorunsuz çoklu oturum açma yeri etkinleştirilmiş AD ormanına listesini alın

1. İlk olarak, indirme ve yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
4. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girmek için bir açılan pencere vermeniz gerekir.
5. Çağrı `Get-AzureADSSOStatus | ConvertFrom-Json`. Bu komut, bu özelliğin etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesinde bakın) listesini sağlar.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>2. Adım Bu, üzerinde ayarlanmış her AD ormanında Kerberos şifre çözme anahtarı güncelleştirme

1. Çağrı `$creds = Get-Credential`. İstendiğinde, hedeflenen AD ormanı için etki alanı yöneticisi kimlik bilgilerini girin.

   > [!NOTE]
   > Etki alanı yöneticisinin kullanıcı adı, kullanıcı asıl adı (UPN) içinde sağlanan kullanırız (johndoe@contoso.com) biçimi veya hedeflenen AD ormanı bulmak için etki alanı tam sam hesabı adı (contoso\johndoe veya contoso.com\johndoe) biçimi. Etki alanı tam sam hesabı adı kullanırsanız, etki alanı bölümü için kullanıcı adını kullanıyoruz [, DNS kullanarak olan etki alanı yöneticisi etki alanı denetleyicisinin yerini](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Bunun yerine, UPN kullanırsanız, biz [bir etki alanı tam sam hesabı adı için çevir](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) uygun etki alanı denetleyicisi bulunurken önce.

2. Çağrı `Update-AzureADSSOForest -OnPremCredentials $creds`. Bu komut için Kerberos şifre çözme anahtarı güncelleştirir `AZUREADSSOACC` bu belirli AD ormanında bilgisayar hesabı ve Azure AD'de güncelleştirir.
3. Özelliği ayarladığınız her AD ormanı için önceki adımları yineleyin.

>[!IMPORTANT]
>Emin olun, _yoksa_ çalıştırma `Update-AzureADSSOForest` birden çok kez komutu. Aksi takdirde, bu özellik, kullanıcılarınızın Kerberos biletleri süresinin dolmasını ve şirket içi Active Directory tarafından yeniden zamana kadar çalışmayı durduruyor.

## <a name="how-can-i-disable-seamless-sso"></a>Sorunsuz çoklu oturum açma nasıl devre dışı bırakabilirim?

### <a name="step-1-disable-the-feature-on-your-tenant"></a>1. Adım Kiracınızın özelliği devre dışı bırak

#### <a name="option-a-disable-using-azure-ad-connect"></a>Seçenek A: Azure AD Connect kullanarak devre dışı bırak

1. Azure AD Connect'i çalıştırın öğesini **değişiklik kullanıcı oturum açma sayfası** tıklatıp **sonraki**.
2. Onay kutusunu temizleyin **etkinleştirme çoklu oturum açma** seçeneği. Sihirbaza devam edin.

Sihirbazı tamamladıktan sonra sorunsuz çoklu oturum açma kiracınızda devre dışı bırakılır. Ancak, aşağıdaki ekranda bir ileti görürsünüz:

"Çoklu oturum açma devre dışı bırakıldı, ancak temizliği tamamlamak için gereken ek el ile adımlar vardır. Daha fazla bilgi edinin"

Temizleme işlemi tamamlamak için Azure AD Connect çalıştırdığınız şirket içi sunucuda 2 ve 3. adımları izleyin.

#### <a name="option-b-disable-using-powershell"></a>Seçenek B: PowerShell kullanarak devre dışı bırak

Aşağıdaki adımlar, Azure AD Connect çalıştırdığınız şirket içi sunucusunda çalıştırın:

1. İlk olarak, indirme ve yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
4. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girmek için bir açılan pencere vermeniz gerekir.
5. Çağrı `Enable-AzureADSSO -Enable $false`.

>[!IMPORTANT]
>Sorunsuz çoklu oturum açma devre dışı bırakma PowerShell kullanarak Azure AD CONNECT'te durumu değiştirmez. Sorunsuz çoklu oturum açma etkin olarak gösterilir **değiştirme kullanıcı oturum açma** sayfası.

### <a name="step-2-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>2. Adım Sorunsuz çoklu oturum açma yeri etkinleştirilmiş AD ormanına listesini alın

Sorunsuz bir Azure AD Connect kullanarak çoklu oturum açma devre dışı bıraktıysanız, 1 ile 4 aşağıdaki görevleri izleyin. Bunun yerine PowerShell kullanarak sorunsuz SSO devre dışı bıraktıysanız, görev 5 atlamayın.

1. İlk olarak, indirme ve yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
4. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girmek için bir açılan pencere vermeniz gerekir.
5. Çağrı `Get-AzureADSSOStatus | ConvertFrom-Json`. Bu komut, bu özelliğin etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesinde bakın) listesini sağlar.

### <a name="step-3-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>3. Adım El ile silmeniz `AZUREADSSOACCT` listelenen gördüğünüz her AD ormanında bilgisayar hesabı.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](how-to-connect-sso-quick-start.md) - getirmek ve Azure AD sorunsuz çoklu oturum açma çalışıyor.
- [**Teknik yakından bakışın** ](how-to-connect-sso-how-it-works.md) -bu özelliği nasıl çalıştığını anlayın.
- [**Sorun giderme** ](tshoot-connect-sso.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.
