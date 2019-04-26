---
title: Hibrit Azure Active Directory sorun giderme alanına katılmış cihazlar Windows 10 ve Windows Server 2016 | Microsoft Docs
description: Hibrit Azure Active Directory sorun giderme alanına katılmış Windows 10 ve Windows Server 2016 cihazları.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcb7dc356c8101c1b0907818b45618ef6372c691
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60250889"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Hibrit Azure Active Directory sorun giderme alanına katılmış Windows 10 ve Windows Server 2016 cihazları 

Bu makalede, aşağıdaki istemciler için geçerlidir:

-   Windows 10
-   Windows Server 2016

Diğer Windows istemcileri için bkz: [sorun giderme hibrit Azure Active Directory'ye katılmış alt düzey cihazları](troubleshoot-hybrid-join-windows-legacy.md).

Bu makalede, sahibi olduğunuzu varsayar [yapılandırılmış hibrit Azure Active Directory alanına katılmış cihazlar](hybrid-azuread-join-plan.md) aşağıdaki senaryoları desteklemek için:

- Cihaz tabanlı koşullu erişim

- [Kurumsal Dolaşım ayarları](../active-directory-windows-enterprise-state-roaming-overview.md)

- [İş İçin Windows Hello](../active-directory-azureadjoin-passport-deployment.md)


Bu belge hakkında olası sorunları çözmek sorun giderme kılavuzu verilmiştir. 


Windows 10 ve Windows Server 2016, hibrit Azure Active Directory join destekler için Windows 10 Kasım 2015 güncelleştirmesi ve üzeri. Yıldönümü Güncelleştirmesi'ni kullanmanızı öneririz.

## <a name="step-1-retrieve-the-join-status"></a>1. Adım: Birleştirme durumu alma 

**Birleştirme durumunu almak için:**

1. Komut istemini yönetici olarak açın.

2. Tür **dsregcmd/Status**



```
+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+

    AzureAdJoined: YES
 EnterpriseJoined: NO
         DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7
       Thumbprint: B753A6679CE720451921302CA873794D94C6204A
   KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9
      KeyProvider: Microsoft Platform Crypto Provider
     TpmProtected: YES
     KeySignTest: : MUST Run elevated to test.
              Idp: login.windows.net
         TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47
       TenantName: Contoso
      AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize
   AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token
           MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc
        MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx
  dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance
      SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ==
   JoinSrvVersion: 1.0
       JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/
        JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net
    KeySrvVersion: 1.0
        KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/
         KeySrvId: urn:ms-drs:enterpriseregistration.windows.net
     DomainJoined: YES
       DomainName: CONTOSO

+----------------------------------------------------------------------+
| User State                                                           |
+----------------------------------------------------------------------+

             NgcSet: YES
           NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
    WorkplaceJoined: NO
      WamDefaultSet: YES
WamDefaultAuthority: organizations
       WamDefaultId: https://login.microsoft.com
     WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)
         AzureAdPrt: YES
```



## <a name="step-2-evaluate-the-join-status"></a>2. Adım: Birleştirme durumu değerlendirin 

Aşağıdaki alanlar gözden geçirin ve beklenen değerleri sahip olduğunuzdan emin olun:

### <a name="azureadjoined--yes"></a>AzureAdJoined: EVET  

Bu alan Azure AD ile cihazı alanına katılıp katılmadığını gösterir. Değer ise **Hayır**, Azure AD'ye Katıl henüz tamamlanmadı. 

**Olası nedenler:**

- Bir birleştirme için bilgisayarın kimlik doğrulaması başarısız oldu.

- Bilgisayar tarafından bulunamıyorsa kuruluştaki bir HTTP Ara sunucusu yok

- Azure AD kimlik doğrulaması için bilgisayar erişilemiyor veya kayıt için Azure DRS

- Bilgisayar kuruluşunuzun iç ağ veya VPN ile doğrudan görebilmesi için şirket içi olmayabilir AD etki alanı denetleyicisi.

- Bilgisayarda TPM varsa, hatalı durumda olabilir.

- Olabilir bir yanlış yapılandırma Hizmetleri belirtildiği belgede daha önce yeniden doğrulamanız gerekir. Sık karşılaşılan örnekler:

    - WS-Trust uç noktaları etkinleştirilmiş Federasyon sunucunuz yok.

    - Federasyon sunucunuz bilgisayarlardan gelen kimlik doğrulama, tümleşik Windows kimlik doğrulaması kullanarak ağınızda izin vermez.

    - Bilgisayar için ait olduğu AD ormanında Azure AD'de doğrulanmış etki alanı adınızı işaret eden bir hizmet bağlantı noktası nesne yok

---

### <a name="domainjoined--yes"></a>DomainJoined: EVET  

Bu alan veya cihazın bir şirket içi Active Directory'ye katılmış olup olmadığını gösterir. Değer ise **Hayır**, cihaz hibrit Azure AD'ye katılma gerçekleştirilemiyor.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: NO  

Bu alan, kişisel cihaz olarak Azure AD ile cihazın kayıtlı olup olmadığını gösterir (olarak işaretlenmiş *çalışma alanına katılmış*). Bu değer olmalıdır **Hayır** aynı zamanda etki alanına katılmış bir bilgisayar için hibrit Azure AD'ye katıldı. Değer ise **Evet**, bir iş veya Okul hesabı hibrit Azure AD'ye katılma tamamlanmadan eklendi. Bu durumda, hesap, Windows 10 (1607) Yıldönümü güncelleştirmesi sürümünü kullanırken dikkate alınmaz.

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Evet ve AzureADPrt: EVET
  
Bu alanlar, kullanıcının Azure AD'ye başarıyla yapıldığını olup olmadığını belirtmek için cihaz oturum açarken. Değerler **Hayır**, son olabilir:

- Hatalı depolama anahtarı TPM cihazı kaydı (denetimi KeySignTest yükseltilmiş çalışırken) ile ilişkili (STK).

- Alternatif oturum açma kimliği

- HTTP Proxy bulunamadı

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız varsa bkz [cihaz yönetimi hakkında SSS](faq.md) 
