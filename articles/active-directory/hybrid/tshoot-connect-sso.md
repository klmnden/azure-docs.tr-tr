---
title: "Azure Active Directory Connect'i: Sorunsuz çoklu oturum açma sorunlarını giderme | Microsoft Docs"
description: Bu konu, Azure Active Directory sorunsuz çoklu oturum açma sorunlarını gidermek açıklar
services: active-directory
author: billmath
ms.reviewer: swkrish
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 09/24/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: abfdad1db655c102dbfb300434eac952fe2154dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60381901"
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma sorunlarını giderme

Bu makale size yardımcı olur. sorun giderme bilgilerini ilgili sık karşılaşılan sorunları ile ilgili Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma (sorunsuz çoklu oturum açma) bulun.

## <a name="known-issues"></a>Bilinen sorunlar

- Bazı durumlarda, sorunsuz çoklu oturum açmayı etkinleştirme 30 dakikaya kadar sürebilir.
- Kendi önbelleğe alınmış Kerberos biletleri, 10 saat, genellikle geçerli süresi dolmuş kadar devre dışı bırakın ve sorunsuz çoklu oturum açma kiracınızda yeniden etkinleştirirseniz, kullanıcılar çoklu oturum açma deneyimini alamayacaksınız.
- Microsoft Edge tarayıcısı desteği kullanılamıyor.
- Sorunsuz çoklu oturum açma işlemi başarılı olursa, kullanıcının seçme fırsatı yok **Oturumumu açık bırak**. Bu davranış nedeniyle [SharePoint ve OneDrive eşleme senaryoları](https://support.microsoft.com/help/2616712/how-to-configure-and-to-troubleshoot-mapped-network-drives-that-connec) çalışmaz.
- Etkileşimli olmayan bir akış kullanarak Office 365 Win32 istemcileri (Outlook, Word, Excel ve diğerleri) ve üstü sürümleri 16.0.8730.xxxx ile desteklenir. Diğer sürümleri desteklenmez; Bu sürümlerde, kullanıcılar, kullanıcı adları, ancak oturum açma için parola, girin. OneDrive için etkinleştirmeniz gerekir [OneDrive sessiz Yapılandırma özelliği](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) sessiz bir oturum açma deneyimi.
- Sorunsuz çoklu oturum açma, Firefox özel tarama modunda çalışmıyor.
- Geliştirilmiş korumalı mod etkin olduğunda sorunsuz çoklu oturum açma Internet Explorer'da çalışmaz.
- Sorunsuz çoklu oturum açma, iOS ve Android mobil tarayıcılarda çalışmaz.
- Bir kullanıcı Active Directory içinde çok fazla gruplarının bir parçası ise, kullanıcının Kerberos anahtarı büyük olasılıkla işlemek için çok büyük olacaktır ve bu sorunsuz SSO başarısız olmasına neden olur. Azure AD HTTPS istekleri üstbilgileri en fazla 50 KB boyutlu olabilir; Kerberos biletleri tanımlama bilgileri gibi diğer Azure AD yapıları (genellikle 2-5 KB) uyum sağlamak için bu sınırdan küçük olması gerekir. Bizim önerimiz, kullanıcının grup üyeliklerini azaltın ve yeniden deneyin sağlamaktır.
- 30 veya daha fazla Active Directory ormanları eşitliyorsanız, Azure AD Connect ile sorunsuz SSO etkinleştirilemiyor. Geçici bir çözüm olarak yapabilecekleriniz [el ile etkinleştirmeniz](#manual-reset-of-the-feature) kiracınız özelliği.
- Azure AD hizmet URL'si ekleme (https://autologon.microsoftazuread-sso.com) Güvenilen siteler bölgesine yerel intranet bölgesine yerine *oturum açarken kullanıcıların engeller*.
- Sorunsuz çoklu oturum açma kullanan **RC4_HMAC_MD5** Kerberos için şifreleme türü. Kullanımını devre dışı bırakma **RC4_HMAC_MD5** şifreleme türü Active Directory ayarlarınızdaki sorunsuz çoklu oturum açma bozar. Grup İlkesi Yönetimi Düzenleyicisi aracınız için ilke değeri emin **RC4_HMAC_MD5** altında **bilgisayar yapılandırması -> Windows Ayarları -> Güvenlik Ayarları -> yerel ilkeler -> güvenlik seçenekleri - > "Ağ güvenliği: Kerberos için izin verilen şifreleme türlerini Yapılandır"** olduğu **etkin**. Sorunsuz çoklu oturum açma ek olarak, olamaz, diğer şifreleme türlerini kullanmak kadar olduklarından emin olun. **devre dışı**.

## <a name="check-status-of-feature"></a>Özelliğin durumunu denetleyin

Sorunsuz çoklu oturum açma özelliği hala olduğundan emin olun **etkin** kiracınıza. Durumu giderek denetleyebilirsiniz **Azure AD Connect** bölmesinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi: Azure AD Connect bölmesi](./media/tshoot-connect-sso/sso10.png)

Sorunsuz çoklu oturum açma için etkinleştirilen tüm AD ormanına görmek için tıklayın.

![Azure Active Directory Yönetim Merkezi: Sorunsuz SSO bölmesi](./media/tshoot-connect-sso/sso13.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Azure Active Directory Yönetim Merkezi'nde oturum açma hatası nedeniyle (bir Premium lisansı gerekir)

Kiracınızın ilişkili bir Azure AD Premium lisansınız varsa, ayrıca bakabilirsiniz [oturum açma etkinliği raporunu](../reports-monitoring/concept-sign-ins.md) içinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi: Oturum açma işlemleri raporu](./media/tshoot-connect-sso/sso9.png)

Gözat **Azure Active Directory** > **oturum açma işlemleri** içinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/)ve ardından belirli bir kullanıcının oturum açma etkinliği seçin. Aranacak **oturum açma hata kodu** alan. Bu alanın değerini, aşağıdaki tabloda kullanarak, bir hata nedeni ve çözümü eşleme:

|Oturum açma hata kodu|Oturum açma hata nedeni|Çözüm
| --- | --- | ---
| 81001 | Kullanıcının Kerberos anahtarı fazla büyük. | Kullanıcının grup üyeliklerini azaltın ve yeniden deneyin.
| 81002 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [sorun giderme denetim listesi](#troubleshooting-checklist).
| 81003 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [sorun giderme denetim listesi](#troubleshooting-checklist).
| 81004 | Kerberos kimlik doğrulaması girişimi başarısız oldu. | Bkz: [sorun giderme denetim listesi](#troubleshooting-checklist).
| 81008 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [sorun giderme denetim listesi](#troubleshooting-checklist).
| 81009 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [sorun giderme denetim listesi](#troubleshooting-checklist).
| 81010 | Kullanıcının Kerberos anahtarının süresi dolduğu veya anahtar geçersiz olduğu için sorunsuz SSO başarısız oldu. | Kullanıcı, etki alanına katılmış bir CİHAZDAN Kurumsal ağınızdaki oturum açması gerekiyor.
| 81011 | Kullanıcının Kerberos anahtarı içindeki bilgileri temel alan kullanıcı nesneyi bulmak yüklenemiyor. | Azure AD Connect, Azure AD'de kullanıcının bilgilerini eşitlemek için kullanın.
| 81012 | Azure AD'de oturum açmaya çalışan kullanıcı, cihaza oturum açmış olan kullanıcının farklıdır. | Kullanıcı, farklı bir CİHAZDAN oturum açması gerekiyor.
| 81013 | Kullanıcının Kerberos anahtarı içindeki bilgileri temel alan kullanıcı nesneyi bulmak yüklenemiyor. |Azure AD Connect, Azure AD'de kullanıcının bilgilerini eşitlemek için kullanın. 

## <a name="troubleshooting-checklist"></a>Sorun giderme denetim listesi

Sorunsuz çoklu oturum açma sorunlarını gidermek için aşağıdaki denetim listesini kullanın:

- Sorunsuz çoklu oturum açma özelliği Azure AD Connect etkinleştirildiğinden emin olun. Özelliği (örneğin, nedeniyle engellenen bir bağlantı noktası) etkinleştiremezsiniz, tüm sahip olduğundan emin olun [önkoşulları](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) yerde.
- Her ikisi de etkinleştirdiyseniz [Azure AD'ye katılımı](../active-directory-azureadjoin-overview.md) ve sorunsuz çoklu oturum açma, kiracınıza sorunu Azure AD'ye katılımı ile olduğundan değil emin olun. Azure AD ile kaydedilen hem de etki alanına katılmış cihaz, Azure AD'ye katılım'nden SSO sorunsuz çoklu oturum açma öncelik kazanır. Azure AD'ye katılım'nden SSO ile "Windows bağlı" ifadesini içeren bir oturum açma kutucuğuna kullanıcı görür.
- Azure AD URL'sini emin olun (https://autologon.microsoftazuread-sso.com) kullanıcının Intranet bölgesi ayarlarını bir parçasıdır.
- Kurumsal cihaz Active Directory etki alanına katıldığından emin olun. Cihaz _değil_ olmasına gerek [Azure AD katıldı](../active-directory-azureadjoin-overview.md) çalışmak sorunsuz SSO için.
- Kullanıcının cihaza bir Active Directory etki alanı hesabıyla oturum açmasını sağlayın.
- Kullanıcı hesabının sorunsuz çoklu oturum açma yeri olan bir Active Directory ormanından kurulduğundan emin olun.
- Cihazın şirket ağına bağlı olduğundan emin olun.
- Cihazın saat saat Active Directory hem de etki alanı denetleyicileri ile eşitlenir ve beş dakika içinde birbiriyle olduklarından emin olun.
- Emin `AZUREADSSOACC` bilgisayar hesabı, sorunsuz SSO etkin istediğiniz her AD ormanında mevcut ve etkin. Bilgisayar hesabı silindi veya eksik, kullanabileceğiniz [PowerShell cmdlet'leri](#manual-reset-of-the-feature) yeniden oluşturulacak.
- Cihazda mevcut Kerberos anahtarları listelemeniz `klist` bir komut isteminden komutu. Emin olmak için verilen anahtarların `AZUREADSSOACC` bilgisayar hesabı. Kullanıcıların Kerberos biletleri için 10 saat genellikle geçerlidir. Active Directory'de farklı ayarlara sahip olabilir.
- Devre dışı ve sorunsuz çoklu oturum açma, kiracınızda yeniden etkinleştirildi, önbelleğe alınan, Kerberos biletleri süresi dolmuş kadar kullanıcılar çoklu oturum açma deneyimini alamayacaksınız.
- Kullanarak mevcut Kerberos biletleri CİHAZDAN Temizleme `klist purge` komutunu ve yeniden deneyin.
- JavaScript ile ilgili sorunlar olup olmadığını belirlemek için tarayıcının yönlendirilen konsol günlüklerini gözden geçirin (altında **Geliştirici Araçları**).
- Gözden geçirme [etki alanı denetleyicisi günlükleri](#domain-controller-logs).

### <a name="domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri

Etki alanı denetleyicinizde sonra sorunsuz çoklu oturum açma bir kullanıcı oturum açtığı her zaman başarılı denetimi etkinleştirirseniz, bir güvenlik girişi olay günlüğüne kaydedilir. Bu güvenlik olayları, aşağıdaki sorguyu kullanarak bulabilirsiniz. (Olay **4769** bilgisayar hesabı ile ilişkili **AzureADSSOAcc$**.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Elle sıfırlama özelliği

Sorun giderme yaramazsa, kiracınızda özelliğini el ile de sıfırlayabilirsiniz. Bu adımları, Azure AD Connect burada çalıştırdığınız şirket içi sunucuda izleyin.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>1. Adım: Sorunsuz SSO PowerShell modülünü içeri aktarın

1. İlk olarak, indirme ve yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Gözat `%programfiles%\Microsoft Azure Active Directory Connect` klasör.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>2. Adım: Active Directory ormanlarını sorunsuz çoklu oturum açma etkinleştirildi'nın listesini alın

1. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini girin.
2. Çağrı `Get-AzureADSSOStatus`. Bu komut, Active Directory ormanına ("Etki alanları" listesinde bakın) listesini bu özelliğin etkinleştirildiği üzerinde sağlar.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>3. Adım: Sorunsuz çoklu oturum açma özelliği burada ayarladığınız her bir Active Directory ormanı için devre dışı bırak

1. Çağrı `$creds = Get-Credential`. İstendiğinde, hedeflenen Active Directory orman için etki alanı yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Etki alanı yöneticisinin kullanıcı adı, kullanıcı asıl adı (UPN) içinde sağlanan kullanırız (johndoe@contoso.com) biçimi veya hedeflenen AD ormanı bulmak için etki alanı tam sam hesabı adı (contoso\johndoe veya contoso.com\johndoe) biçimi. Etki alanı tam sam hesabı adı kullanırsanız, etki alanı bölümü için kullanıcı adını kullanıyoruz [, DNS kullanarak olan etki alanı yöneticisi etki alanı denetleyicisinin yerini](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Bunun yerine, UPN kullanırsanız, biz [bir etki alanı tam sam hesabı adı için çevir](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) uygun etki alanı denetleyicisi bulunurken önce.

2. Çağrı `Disable-AzureADSSOForest -OnPremCredentials $creds`. Bu komut kaldırır `AZUREADSSOACC` bu belirli Active Directory ormanı için şirket içi etki alanı denetleyicisi bilgisayar hesabı.
3. Özelliği burada ayarladığınız her bir Active Directory ormanı için önceki adımları yineleyin.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>4. Adım: Her Active Directory ormanı için sorunsuz SSO etkinleştirme

1. Çağrı `Enable-AzureADSSOForest`. İstendiğinde, hedeflenen Active Directory orman için etki alanı yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Etki alanı yöneticisinin kullanıcı adı, kullanıcı asıl adı (UPN) içinde sağlanan kullanırız (johndoe@contoso.com) biçimi veya hedeflenen AD ormanı bulmak için etki alanı tam sam hesabı adı (contoso\johndoe veya contoso.com\johndoe) biçimi. Etki alanı tam sam hesabı adı kullanırsanız, etki alanı bölümü için kullanıcı adını kullanıyoruz [, DNS kullanarak olan etki alanı yöneticisi etki alanı denetleyicisinin yerini](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Bunun yerine, UPN kullanırsanız, biz [bir etki alanı tam sam hesabı adı için çevir](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) uygun etki alanı denetleyicisi bulunurken önce.

2. Bir özelliği ayarlamak istediğiniz her bir Active Directory ormanı için önceki adımı yineleyin.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>5. Adım. Kiracı özelliğini etkinleştirme

Kiracınızın özelliğini etkinleştirmek için çağrı `Enable-AzureADSSO -Enable $true`.
