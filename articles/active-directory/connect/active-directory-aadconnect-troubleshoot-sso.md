---
title: "Azure Active Directory Connect: Sorunsuz çoklu oturum açma sorunlarını giderme | Microsoft Docs"
description: "Bu konu, Azure Active Directory sorunsuz çoklu oturum açma giderileceğini açıklar"
services: active-directory
keywords: "Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: b4efb457a58d8b54c9ebb126a8d84fdef01b3847
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma sorunlarını giderme

Bu makale size yardımcı olacak sorun giderme bilgileri ilgili sık karşılaşılan sorunları ile ilgili Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma (sorunsuz SSO) bulun.

## <a name="known-problems"></a>Bilinen sorunlar

- Bazı durumlarda, sorunsuz SSO etkinleştirme 30 dakika kadar sürebilir.
- Edge tarayıcı desteği kullanılamıyor.
- Office istemcileri, özellikle paylaşılan bir bilgisayar senaryolarda başlangıç kullanıcılar için fazladan oturum açma ister neden olur. Kullanıcıların kendi kullanıcı adları sık, ancak kullanıcıların parolalarını girmeleri gerekir.
- Sorunsuz SSO başarılı olursa, kullanıcının seçmek için Fırsat yok **Oturumumu açık bırak**. Bu davranış nedeniyle, SharePoint ve OneDrive eşleme senaryolar çalışmaz.
- Sorunsuz SSO Firefox özel gözatma modunda çalışmıyor.
- Geliştirilmiş korumalı mod açıldığında sorunsuz SSO Internet Explorer'da işe yaramaz.
- Sorunsuz SSO iOS ve Android mobil tarayıcılar işe yaramaz.
- 30 veya daha fazla Active Directory ormanları eşitliyorsanız, Azure AD Connect ile sorunsuz SSO etkinleştiremezsiniz. Geçici bir çözüm olarak, şunları yapabilirsiniz [el ile etkinleştirmeniz](#manual-reset-of-azure-ad-seamless-sso) kiracınız özelliği.
- Azure AD hizmeti URL'leri (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) yerel intranet bölgesine yerine Güvenilen siteler bölgesine ekleme *kullanıcıların açmasını engelleyen*.

## <a name="check-the-status-of-the-feature"></a>Özellik durumunu denetleme

Sorunsuz SSO özelliği hala olduğundan emin olun **etkin** kiracınız üzerinde. Giderek durumunu denetleyebilirsiniz **Azure AD Connect** bölmesinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi: Azure AD Connect bölmesi](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Oturum açma hatası nedeniyle Azure Active Directory Yönetim Merkezi'nden (Premium lisansı gerekir)

Kiracı kendisiyle ilişkili bir Azure AD Premium lisansı varsa, aynı zamanda bakabilirsiniz [oturum açma etkinliği raporu](../active-directory-reporting-activity-sign-ins.md) içinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi: oturum açma işlemleri raporu](./media/active-directory-aadconnect-sso/sso9.png)

Gözat **Azure Active Directory** > **oturum açma işlemleri** içinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/)ve ardından belirli bir kullanıcının oturum açma etkinliği seçin. Ara **oturum açmayı hata kodu** alan. Bu alanın değeri bir hata nedeni ve çözümü için aşağıdaki tabloyu kullanarak eşleyin:

|Oturum açma hata kodu|Oturum açma hatası nedeni|Çözüm
| --- | --- | ---
| 81001 | Kullanıcının Kerberos anahtarı fazla büyük. | Kullanıcı Grup üyeliklerini azaltın ve yeniden deneyin.
| 81002 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81003 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81004 | Kerberos kimlik doğrulaması girişimi başarısız oldu. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81008 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81009 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81010 | Kullanıcının Kerberos anahtarının süresi dolduğu veya anahtar geçersiz olduğu için sorunsuz SSO başarısız oldu. | Kullanıcı etki alanına katılmış bir CİHAZDAN Kurumsal ağınızdaki oturum açması gerekiyor.
| 81011 | Kullanıcının Kerberos bileti içindeki bilgileri temel kullanıcı nesnesi bulunamıyor. | Azure AD Connect, Azure AD kullanıcı bilgilerini eşitlemek için kullanın.
| 81012 | Azure AD ile oturum açmaya çalışan kullanıcıya, cihaza açtığınız kullanıcı farklıdır. | Kullanıcı, farklı bir CİHAZDAN oturum açması gerekiyor.
| 81013 | Kullanıcının Kerberos bileti içindeki bilgileri temel kullanıcı nesnesi bulunamıyor. |Azure AD Connect, Azure AD kullanıcı bilgilerini eşitlemek için kullanın. 

## <a name="troubleshooting-checklist"></a>Sorun giderme denetim listesi

Sorunsuz SSO sorunlarını gidermek için aşağıdaki denetim listesini kullanın:

- Azure AD Connect sorunsuz SSO özelliği etkin olduğundan emin olun. Özelliği (örneğin, nedeniyle engellenen bir bağlantı noktası) etkinleştiremezsiniz tümüne sahip olun [Önkoşullar](active-directory-aadconnect-sso-quick-start.md#step-1-check-the-prerequisites) yerinde.
- Bu Azure AD URL'ler (https://autologon.microsoftazuread-sso.com ve https://aadg.windows.net.nsatc.net) kullanıcının Intranet bölgesi ayarlarının bir parçası olduğundan emin olun.
- Kurumsal cihaz Active Directory etki alanına katılmış emin olun.
- Kullanıcı aygıt bir Active Directory etki alanı hesabıyla oturum açmış emin olun.
- Kullanıcı hesabının sorunsuz SSO burada bırakıldı bir Active Directory ormanından kurulduğundan emin olun.
- Cihazın şirket ağına bağlı olduğundan emin olun.
- Cihazın zaman zaman Active Directory ve etki alanı denetleyicileri ile eşitlenir ve beş dakikalık olduklarından emin olun.
- Cihazda mevcut Kerberos anahtarları kullanarak listesinde `klist` bir komut isteminden komutu. Çıkarılan biletleri emin `AZUREADSSOACCT` bilgisayar hesabı. Kullanıcıların Kerberos biletleri için 12 saat genellikle geçerlidir. Active Directory'de farklı ayarlara sahip olabilir.
- Mevcut Kerberos anahtarları kullanarak aygıttan Temizleme `klist purge` komut ve yeniden deneyin.
- JavaScript ile ilgili sorunlar olup olmadığını belirlemek için tarayıcının konsol günlüklerini gözden geçirin (altında **Geliştirici Araçları**).
- Gözden geçirme [etki alanı denetleyicisi günlükleri](#domain-controller-logs).

### <a name="domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri

Etki alanı denetleyicinizde, ardından sorunsuz SSO ile bir kullanıcı oturum açtığında her zaman Başarı Denetimi etkinleştirirseniz, güvenlik girişi olay günlüğüne kaydedilir. Bu güvenlik olayları aşağıdaki sorguyu kullanarak bulabilirsiniz. (Olayını arayın **4769** bilgisayar hesabıyla ilişkilendirilmiş **AzureADSSOAcc$**.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Özelliğin elle sıfırlama

Sorun giderme işe yaramazsa, Kiracı ' özelliğini el ile sıfırlayabilirsiniz. Azure AD Connect kullanmakta olduğunuz nerede şirket içi sunucu üzerinde aşağıdaki adımları izleyin.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>1. adım: sorunsuz SSO PowerShell modülünü içeri aktarın

1. İndirme ve yükleme [Microsoft Online Services oturum açma Yardımcısı](http://go.microsoft.com/fwlink/?LinkID=286152).
2. İndirme ve yükleme [64-bit Windows PowerShell için Azure Active Directory Modülü](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Gözat `%programfiles%\Microsoft Azure Active Directory Connect` klasör.
4. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>2. adım: Active Directory ormanları sorunsuz SSO etkinleştirildiği listesini al

1. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. İstendiğinde, kiracının genel Yöneticisi kimlik bilgilerini girin.
2. Çağrı `Get-AzureADSSOStatus`. Bu komut, Active Directory ormanına ("Etki alanları" listesi bakın) listesini bu özellik etkinleştirildiği üzerinde sağlar.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>3. adım: Özelliği burada ayarladığınız her Active Directory ormanı için sorunsuz SSO devre dışı bırak

1. Çağrı `$creds = Get-Credential`. İstendiğinde, hedeflenen Active Directory orman için etki alanı yönetici kimlik bilgilerini girin.
2. Çağrı `Disable-AzureADSSOForest -OnPremCredentials $creds`. Bu komut kaldırır `AZUREADSSOACCT` bu belirli Active Directory ormanı için şirket içi etki alanı denetleyicisinden bilgisayar hesabı.
3. Özelliği burada ayarladığınız her Active Directory ormanı için önceki adımları yineleyin.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>4. adım: Her Active Directory ormanı için sorunsuz SSO etkinleştirme

1. Çağrı `Enable-AzureADSSOForest`. İstendiğinde, hedeflenen Active Directory orman için etki alanı yönetici kimlik bilgilerini girin.
2. Özelliği ayarlamak istediğiniz her bir Active Directory ormanı için önceki adımı yineleyin.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>5. Adım. Kiracı özelliğini etkinleştir

Kiracı özelliğini etkinleştirmek için arama `Enable-AzureADSSO` ve girin **true** adresindeki `Enable:` istemi.
