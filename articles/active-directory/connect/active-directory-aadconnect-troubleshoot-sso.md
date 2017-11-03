---
title: "Azure AD Connect: Sorunsuz çoklu oturum açma sorunlarını giderme | Microsoft Docs"
description: "Bu konuda, Azure Active Directory sorunsuz çoklu oturum açma (Azure AD sorunsuz SSO) ile ilgili sorunları giderme açıklanmaktadır."
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
ms.openlocfilehash: b383a21500c753d8d2fe6747756541a3ff94ef02
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma sorunlarını giderme

Bu makale size yardımcı olacak sorun giderme ilişkin Azure AD sorunsuz çoklu oturum açma ortak sorunlar hakkında bilgileri bulabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

- Bazı durumlarda, sorunsuz SSO etkinleştirme 30 dakika kadar sürebilir.
- Edge tarayıcı desteği kullanılamıyor.
- Office istemcileri başlatılıyor, özellikle paylaşılan bir bilgisayar senaryolarında kullanıcılar için fazladan oturum açma ister neden. Kullanıcılar kendi kullanıcı adları, ancak değil parolaları sık girmeniz gerekir.
- Sorunsuz SSO başarılı olursa, kullanıcı "Oturumumu açık bırak" seçme imkanınız verilmedi. Bu davranış nedeniyle, SharePoint ve OneDrive eşlemesi senaryoları çalışmıyor.
- Sorunsuz SSO Firefox özel gözatma modunda çalışmıyor.
- Geliştirilmiş koruma modu etkinken sorunsuz SSO Internet Explorer'da işe yaramaz.
- Sorunsuz SSO iOS ve Android mobil tarayıcılar işe yaramaz.
- 30 veya daha fazla AD ormanına eşitleme, Azure AD Connect'i kullanarak sorunsuz SSO etkinleştiremezsiniz. Geçici bir çözüm olarak, şunları yapabilirsiniz [el ile etkinleştirmeniz](#manual-reset-of-azure-ad-seamless-sso) kiracınız özelliği.
- Azure AD hizmeti URL'leri (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) yerine "Yerel intranet" bölgesine "Güvenilen siteler" bölgesine ekleyerek **kullanıcıların açmasını engelleyen**.

## <a name="check-status-of-the-feature"></a>Özellik durumunu denetleme

Sorunsuz SSO özelliği hala olduğundan emin olun **etkin** kiracınız üzerinde. Giderek durumunu denetleyebilirsiniz **Azure AD Connect** dikey penceresinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi - Azure AD Connect dikey penceresi](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Oturum açma hatası nedeniyle Azure Active Directory Yönetim Merkezi (Premium lisansı gerekir)

Kiracı kendisiyle ilişkili bir Azure AD Premium lisansı varsa, aynı zamanda bakabilirsiniz [oturum açma etkinliği raporu](../active-directory-reporting-activity-sign-ins.md) üzerinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi - oturum açma işlemleri raporu](./media/active-directory-aadconnect-sso/sso9.png)

Gidin **Azure Active Directory** -> **oturum açma işlemleri** üzerinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) ve belirli bir kullanıcının oturum açma etkinliği tıklatın. Ara **oturum açmayı hata kodu** alan. Bu alanın değeri bir başarısızlık nedeniyle ve aşağıdaki tabloda kullanılarak çözümleme eşleyin:

|Oturum açma hata kodu|Oturum açma hatası nedeni|Çözüm
| --- | --- | ---
| 81001 | Kullanıcının Kerberos anahtarı fazla büyük. | Kullanıcı Grup üyeliklerini azaltın ve yeniden deneyin.
| 81002 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81003 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81004 | Kerberos kimlik doğrulaması girişimi başarısız oldu. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81008 | Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81009 | "Kullanıcının Kerberos anahtarı doğrulanamadı. | Bkz: [denetim listesi sorun giderme](#troubleshooting-checklist).
| 81010 | Kullanıcının Kerberos anahtarının süresi dolduğu veya anahtar geçersiz olduğu için sorunsuz SSO başarısız oldu. | Kullanıcı etki alanına katılmış bir CİHAZDAN Kurumsal ağınızdaki oturum açması gerekiyor.
| 81011 | Kullanıcının Kerberos anahtarındaki bilgiler temel alınarak kullanıcı nesnesi bulunamadı. | Azure AD Connect, Azure AD kullanıcı bilgilerini eşitlemek için kullanın.
| 81012 | Azure AD'de oturum açmaya çalışan kullanıcı, cihazda oturum açmış olan kullanıcıdan farklıdır. | Farklı bir CİHAZDAN oturum açın.
| 81013 | Kullanıcının Kerberos anahtarındaki bilgiler temel alınarak kullanıcı nesnesi bulunamadı. |Azure AD Connect, Azure AD kullanıcı bilgilerini eşitlemek için kullanın. 

## <a name="troubleshooting-checklist"></a>Sorun giderme denetim listesi

Sorunsuz SSO sorunları gidermek için aşağıdaki denetim listesini kullanın:

- Sorunsuz SSO Özelliği Azure AD Connect etkin olup olmadığını denetleyin. Özelliği (örneğin, nedeniyle engellenen bir bağlantı noktası) etkinleştiremezsiniz tümüne sahip olun [ön koşullar](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) yerinde.
- Her iki bu Azure AD URL'leri (https://autologon.microsoftazuread-sso.com ve https://aadg.windows.net.nsatc.net) kullanıcının Intranet bölgesi ayarlarının bir parçası olup olmadığını denetleyin.
- Kurumsal cihaz AD etki alanına katılmış emin olun.
- Kullanıcının aygıtına bir AD etki alanı hesabı kullanarak oturum emin olun.
- Kullanıcı hesabının sorunsuz SSO burada bırakıldı bir AD ormandan kurulduğundan emin olun.
- Cihaz şirket ağına bağlı olduğundan olun.
- Cihazın zaman Active Directory'nin ve etki alanı denetleyicileri süresiyle eşitlenir ve beş dakikalık olduğundan emin olun.
- Aygıt kullanarak mevcut Kerberos anahtarları listesinde **klist** bir komut isteminden komutu. Biletleri için verilen varsa denetleyin `AZUREADSSOACCT` bilgisayar hesabı. Kullanıcıların Kerberos biletleri için 12 saat genellikle geçerlidir. Active Directory'de farklı ayarlara sahip olabilir.
- Cihazı kullanarak mevcut Kerberos anahtarları Temizleme **klist Temizleme** komut ve yeniden deneyin.
- JavaScript ile ilgili bir sorun varsa belirlemek için (altında "Geliştirici Araçları") tarayıcının konsol günlüklerini gözden geçirin.
- Gözden geçirme [etki alanı denetleyicisi](#domain-controller-logs) de.

### <a name="domain-controller-logs"></a>Etki alanı denetleyicisi

Etki alanı denetleyicinizde, ardından sorunsuz SSO kullanarak kullanıcının oturum açtığı her zaman başarı denetimi etkinse, güvenlik girişi olay günlüğüne kaydedilir. Aşağıdaki sorguyu kullanarak bu güvenlik olaylarını Bul (olayını arayın **4769** bilgisayar hesabıyla ilişkilendirilmiş **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Özelliğin elle sıfırlama

Sorun giderme işe yaramazsa, Kiracı ' özelliğini el ile sıfırlayabilirsiniz. Azure AD Connect çalıştırdığınız şirket içi sunucu üzerinde aşağıdaki adımları izleyin:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>1. adım: sorunsuz SSO PowerShell modülünü içeri aktarın

1. İlk olarak, indirme ve yükleme [Microsoft Online Services oturum açma Yardımcısı](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Ardından karşıdan yükleyip [64-bit Windows PowerShell için Azure Active Directory Modülü](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Gidin `%programfiles%\Microsoft Azure Active Directory Connect` klasör.
4. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>2. adım: AD ormanına sorunsuz SSO etkinleştirildiği listesini al

1. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, çağrı `New-AzureADSSOAuthenticationContext`. İstendiğinde, kiracının genel Yöneticisi kimlik bilgilerini girin.
2. Çağrı `Get-AzureADSSOStatus`. Bu komut bu özellik etkinleştirildiği üzerinde AD ormanına ("Etki alanları" listesi bakın) listesini sağlar.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>3. adım: Bunu üzerinde oluşturulduğuna her AD ormanı için sorunsuz SSO devre dışı bırak

1. Çağrı `$creds = Get-Credential`. İstendiğinde, hedeflenen AD orman için etki alanı yönetici kimlik bilgilerini girin.
2. Çağrı `Disable-AzureADSSOForest -OnPremCredentials $creds`. Bu komut kaldırır `AZUREADSSOACCT` bu belirli AD ormanı için şirket içi etki alanı denetleyicisinden bilgisayar hesabı.
3. Özelliği ayarladığınız her bir AD orman için önceki adımları yineleyin.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>4. adım: Her bir AD orman için sorunsuz SSO etkinleştir

1. Çağrı `Enable-AzureADSSOForest`. İstendiğinde, hedeflenen AD orman için etki alanı yönetici kimlik bilgilerini girin.
2. Üzerinde özelliği ayarlamak istediğiniz her bir AD orman için önceki adımları yineleyin.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>5. Adım. Kiracı özelliğini etkinleştir

Çağrı `Enable-AzureADSSO` "true" yazın `Enable: ` kiracınızda özelliğini etkinleştirmek komut istemi.
