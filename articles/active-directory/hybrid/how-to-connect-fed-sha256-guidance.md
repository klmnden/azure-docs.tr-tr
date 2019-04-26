---
title: Office 365 bağlı olan taraf güveni için değişiklik imzası karma algoritması | Microsoft Docs
description: Bu sayfa Office 365 ile federasyon güveni için SHA algoritması değiştirmeye yönelik yönergeler sağlar.
keywords: SHA1, SHA256, O365, Federasyon, aadconnect, adfs, ad fs, değişiklik sha, federasyon güven, bağlı olan taraf güveni
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9aa597c8b458305946aa298631726df3da317534
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60244416"
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Office 365 bağlı olan taraf güveni için değişiklik imzası karma algoritması
## <a name="overview"></a>Genel Bakış
Active Directory Federasyon Hizmetleri (AD FS), Microsoft Azure Active Directory'ye bunlar ile değiştirilmemesi emin olmak için belirteçlerini imzalar. Bu imza SHA1 veya SHA256 temel alabilir. Azure Active Directory, artık bir SHA256 algoritmasını ile imzalanmış belirteçler destekler ve belirteç imzalama algoritması yüksek düzeyde güvenlik için SHA256 ayarlanması önerilir. Bu makalede, belirteç imzalama algoritması için daha güvenli SHA256 düzeyi ayarlamak için gerekli olan adımları açıklanmaktadır.

>[!NOTE]
>Microsoft olarak SHA1 daha güvenlidir, ancak desteklenen bir seçenek SHA1 kalıyor belirteçleri imzalama algoritması SHA256 kullanımını önerir.

## <a name="change-the-token-signing-algorithm"></a>Belirteç imzalama algoritması değiştirme
İmza algoritması bir aşağıdaki iki işlem ile ayarladıktan sonra AD FS belirteçleri için Office 365 bağlı olan taraf güveni SHA256 ile imzalar. Ek yapılandırma değişiklikleri yapmanız gerekmez ve bu değişiklik, Office 365 veya diğer Azure AD uygulamalarına erişmek için yeteneğinizi herhangi bir etkisi yoktur.

### <a name="ad-fs-management-console"></a>AD FS Yönetim Konsolu
1. Birincil AD FS sunucusunda AD FS yönetim konsolunu açın.
2. AD FS düğümünü genişletin ve tıklayın **bağlı olan taraf güvenleri**.
3. Office 365/Azure bağlı olan taraf güvenine sağ tıklayıp **özellikleri**.
4. Seçin **Gelişmiş** sekme ve güvenli karma algoritması SHA256'ı seçin.
5. **Tamam** düğmesine tıklayın.

![İmzalama Algoritması SHA256--MMC](./media/how-to-connect-fed-sha256-guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell cmdlet'leri
1. Tüm AD FS sunucusunda PowerShell'i yönetici ayrıcalıklarıyla açın.
2. Güvenli Karma algoritması kullanarak ayarlamak **Set-AdfsRelyingPartyTrust** cmdlet'i.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'https://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Ayrıca okuyun
* [Azure AD Connect ile Office 365 güveni onarın](how-to-connect-fed-management.md#repairthetrust)

