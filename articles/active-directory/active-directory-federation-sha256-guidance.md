---
title: Office 365 bağlı olan taraf güveni için değişiklik imza karma algoritmasını | Microsoft Docs
description: Bu sayfa, Office 365 ile bir federasyon güveni için SHA algoritma değiştirmek için yönergeler sağlar
keywords: SHA1, SHA256, O365, Federasyon, aadconnect, adfs, ad fs, değişiklik sha, bağlı olan taraf güveni bir federasyon güveni
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: ec7eee36888d825d65335db590731103aabbf5c2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26598969"
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Office 365 bağlı olan taraf güveni için imza karma algoritması değiştirme
## <a name="overview"></a>Genel Bakış
Active Directory Federasyon Hizmetleri (AD FS) Microsoft Azure Active Directory'ye bunlar ile değiştirilmemesi emin olmak için belirteçlerini imzalar. Bu imza SHA1 veya SHA256 dayalı olabilir. Azure Active Directory şimdi SHA256 algoritmasını ile imzalanmış belirteçleri destekler ve belirteç imzalama algoritması yüksek düzeyde güvenlik için SHA256 ayarlanması önerilir. Bu makalede daha güvenli SHA256 belirteç imzalama algoritması düzeyi ayarlamak için gereken adımlar açıklanır.

>[!NOTE]
>Microsoft, SHA1'den daha güvenlidir, ancak desteklenen bir seçenek SHA1 kalıyor belirteç imzalama algoritması olarak SHA256 kullanımını önerir.

## <a name="change-the-token-signing-algorithm"></a>Belirteç imzalama algoritması değiştirme
Aşağıdaki iki işlemlerden biri ile imza algoritması ayarladıktan sonra AD FS belirteçleri Office 365 bağlı olan taraf güveni SHA256 ile imzalar. Ek yapılandırma değişiklikleri yapmanıza gerek yoktur ve bu değişiklik, Office 365 veya diğer Azure AD uygulamalarına erişmek için yeteneğinizi üzerinde hiçbir etkisi olmaz.

### <a name="ad-fs-management-console"></a>AD FS Yönetim Konsolu
1. Birincil AD FS sunucusunda AD FS Yönetimi konsolunu açın.
2. AD FS düğümünü genişletin ve tıklatın **bağlı olan taraf güvenleri**.
3. Office 365/Azure bağlı olan taraf güveniniz sağ tıklatıp **özellikleri**.
4. Seçin **Gelişmiş** sekmesinde ve güvenli karma algoritması SHA256 seçin.
5. **Tamam** düğmesine tıklayın.

![SHA256 imzalama algoritmasını--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell cmdlet'leri
1. Herhangi bir AD FS sunucusu üzerinde PowerShell'i yönetici ayrıcalıklarıyla açın.
2. Güvenli karma algoritmasını kullanarak ayarlamak **Set-AdfsRelyingPartyTrust** cmdlet'i.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Ayrıca okuyun
* [Azure AD Connect ile Office 365 güven onarın](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

