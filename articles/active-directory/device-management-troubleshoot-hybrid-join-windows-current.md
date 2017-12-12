---
title: "Karma Azure Active Directory sorun giderme alanına katılmış Windows 10 ve Windows Server 2016 cihazları | Microsoft Docs"
description: "Karma Azure Active Directory sorun giderme Windows 10 ve Windows Server 2016 cihazları katıldı."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3b98d31efcdbd61cf12e2c905f200c1e54f68f69
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Katılmış Windows 10 ve Windows Server 2016 cihazlarda karma Azure Active Directory sorun giderme 

Bu konu, aşağıdaki istemciler için geçerlidir:

-   Windows 10
-   Windows Server 2016

Diğer Windows istemcileri için bkz: [sorun giderme karma Azure Active Directory birleştirilmiş alt düzey aygıtları](device-management-troubleshoot-hybrid-join-windows-legacy.md).

Bu konu, sahibi olduğunuzu varsayar [yapılandırılmış karma Azure Active Directory'ye katılmış cihazları](device-management-hybrid-azuread-joined-devices-setup.md) aşağıdaki senaryoları desteklemek için:

- Cihaz temelli koşullu erişim

- [Kurumsal Dolaşım ayarları](active-directory-windows-enterprise-state-roaming-overview.md)

- [İş İçin Windows Hello](active-directory-azureadjoin-passport-deployment.md)


Bu belge hakkında olası sorunları gidermek sorun giderme kılavuzu sağlar. 


Windows 10 ve Windows Server 2016, karma Azure Active Directory katılım desteklediği için Windows 10 Kasım 2015 güncelleştirme ve üstü. Yıldönümü güncelleştirme kullanmanızı öneririz.

## <a name="step-1-retrieve-the-join-status"></a>1. adım: katılım durumunu alma 

**Birleşim durumunu almak için:**

1. Komut istemini yönetici olarak açın

2. Tür **dsregcmd/Status**



    +----------------------------------------------------------------------+
   | Cihaz durumu |+----------------------------------------------------------------------+
    
        AzureAdJoined: YES
     EnterpriseJoined: Cihaz kimliği yok: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 parmak izi: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform şifreleme sağlayıcısı TpmProtected: Evet KeySignTest:: test etmek için yükseltilmiş çalıştırmanız gerekir.
                  IDP: login.windows.net Tenantıd: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/Token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-Beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ JoinSrvVersion ==: 1.0 JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Evet DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Kullanıcı durumunu |+----------------------------------------------------------------------+
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    WamDefaultAuthority: kuruluşların WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (Azuread'i) AzureAdPrt: Evet



## <a name="step-2-evaluate-the-join-status"></a>2. adım: katılım durumunu değerlendirme 

Aşağıdaki alanları gözden geçirin ve beklenen değerleri sahip olduğunuzdan emin olun:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Evet  

Bu alan, Azure AD ile cihaz katıldığından olup olmadığını gösterir. Değer ise **Hayır**, Azure ad birleştirme henüz tamamlanmadı. 

**Olası nedenler:**

- Bilgisayar bir birleştirme için kimlik doğrulaması başarısız oldu.

- Bilgisayar tarafından bulunan kuruluştaki bir HTTP proxy yok

- Bilgisayar kimlik doğrulaması için Azure AD alanına erişilemiyor veya kayıt için Azure DRS

- Bilgisayar VPN veya kuruluşunuzun iç ağ üzerinde doğrudan görüş bir şirket içi ile olmayabilir AD etki alanı denetleyicisi.

- Bilgisayar bir TPM varsa, hatalı durumda olabilir.

- Olabilir yetersizliğini hizmetlerinde not ettiğiniz belgede daha önce yeniden doğrulamanız gerekir. Ortak örnekler şunlardır:

    - Federasyon sunucunuz etkin WS-Trust uç nokta yok

    - Federasyon sunucunuz bilgisayarlardan gelen kimlik doğrulama tümleşik Windows kimlik doğrulaması kullanarak ağınızda izin vermiyor.

    - Bilgisayar için ait olduğu AD ormanında Azure AD'de doğrulanmış etki alanı adınızı işaret hizmet bağlantı noktası nesnesi yok

---

### <a name="domainjoined--yes"></a>DomainJoined: Evet  

Bu alan, cihaz bir şirket içi Active Directory veya alanına katılıp katılmadığını gösterir. Değer ise **Hayır**, cihaz bir karma Azure AD birleştirme işlemi gerçekleştiremezsiniz.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Hayır  

Bu alan, bir kişisel cihaz olarak Azure AD ile cihazın kayıtlı olup olmadığını gösterir (olarak işaretlenmiş *çalışma alanına katılmış*). Bu değer olmalıdır **Hayır** de etki alanına katılmış bir bilgisayar için karma Azure AD alanına. Değer ise **Evet**, karma Azure AD birleştirme tamamlanmadan önce bir iş veya Okul hesabı eklendi. Bu durumda, hesap, Windows 10 (1607) Yıldönümü güncelleştirme sürümünü kullanırken dikkate alınmaz.

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Evet ve AzureADPrt: Evet
  
Bu alanlar kullanıcının Azure AD ile başarıyla kimliğinin olup olmadığını belirtmek için aygıt oturum açarken. Değerler ise **Hayır**, son olabilir:

- Hatalı depolama anahtar aygıt kaydı (denetimi yükseltilmiş çalışırken KeySignTest) ile ilişkili TPM (STK).

- Alternatif oturum açma kimliği

- HTTP Proxy bulunamadı

## <a name="next-steps"></a>Sonraki adımlar

Soruları için bkz: [aygıt yönetimi hakkında SSS](device-management-faq.md) 