---
title: Azure Stack için kimlik mimarisi | Microsoft Docs
description: Azure Stack ile kullanabileceğiniz kimlik mimarisi hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/07/2018
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: a16a6596d6bc33200f87a1dfd3b2ea5b02628e10
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277826"
---
# <a name="identity-architecture-for-azure-stack"></a>Azure Stack için kimlik mimarisi
Azure Stack ile kullanmak için bir kimlik sağlayıcısı seçmeden önce Azure Active Directory (Azure AD) seçeneklerini ve Active Directory Federasyon Hizmetleri (AD FS) arasındaki önemli farklar anlayın. 

## <a name="capabilities-and-limitations"></a>Özellikler ve sınırlamalar 
Seçtiğiniz kimlik sağlayıcısı, çok kiracılı desteği dahil olmak üzere seçeneklerinizi sınırlayabilirsiniz. 

  

|Özellik veya senaryo        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|İnternet'e bağlı     |Evet       |İsteğe bağlı|
|Çok kiracılı desteği     |Evet       |Hayır      |
|Market'te öğeler sunar |Evet       |Evet. Kullanılmasını gerektirir [çevrimdışı Market dağıtım](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) aracı.|
|Active Directory kimlik doğrulama kitaplığı (ADAL) desteği |Evet |Evet|
|Azure CLI, Visual Studio ve PowerShell gibi araçlar desteği  |Evet |Evet|
|Azure Portalı aracılığıyla hizmet sorumlusu oluşturma     |Evet |Hayır|
|Sertifikalar ile hizmet sorumlusu oluşturma      |Evet |Evet|
|Gizli anahtarları (anahtarlar) ile hizmet sorumlusu oluşturma    |Evet |Hayır|
|Graf hizmeti uygulamaları kullanabilir           |Evet |Hayır|
|Uygulamaları, kimlik sağlayıcısı oturum açmak için kullanabilir |Evet |Evet. Şirket içi ile federasyona eklemek için uygulamalar oluşturabilmek için AD FS örneği. |

## <a name="topologies"></a>Topolojileri
Aşağıdaki bölümlerde, kullanabileceğiniz çeşitli kimlik topolojileri açıklanmaktadır.

### <a name="azure-ad-single-tenant-topology"></a>Azure AD: tek kiracılı topolojisi 
Azure yığını'nı yükleme ve Azure AD, varsayılan olarak, Azure Stack tek kiracılı topolojisi kullanır. 

Tek kiracılı topolojisi durumlarda yararlı olur:
- Tüm kullanıcıların aynı kiracıda bir parçasıdır.
- Bir hizmet sağlayıcısı, bir kuruluş için Azure Stack örneği barındırır. 

![Azure AD ile Azure Stack tek kiracılı topolojisi](media/azure-stack-identity-architecture/single-tenant.png)

Bu topoloji, aşağıdaki özelliklere sahiptir:
- Tüm uygulamaları Azure Stack kaydeder ve Dizin Hizmetleri aynı Azure ad Kiracı. 
- Azure Stack kullanıcıları ve belirteçleri de dahil olmak üzere bu dizine, uygulamalardan yalnızca kimliğini doğrular. 
- Yöneticiler (bulut operatörlerinin) ve Kiracı kullanıcılar aynı dizin kiracısında Kimlikleridir. 
- Bu Azure Stack ortamınıza erişmek bir kullanıcı başka bir dizinden etkinleştirmek için şunları yapmanız gerekir [kullanıcıyı konuk olarak davet](azure-stack-identity-overview.md#guest-users) Kiracı dizinine. 

### <a name="azure-ad-multi-tenant-topology"></a>Azure AD: çok kiracılı topolojisi
Bulut operatörleri, Azure Stack, bir veya daha fazla kuruluşlardan erişim kiracılar tarafından uygulamalara izin verecek şekilde yapılandırabilirsiniz. Kullanıcıların, uygulamaların Kullanıcı Portalı ile erişirsiniz. Bu yapılandırmada, Yönetim Portalı (bulut işleci tarafından kullanılır) kullanıcılar için tek bir dizinden sınırlıdır. 

Çok kiracılı topolojisi durumlarda yararlı olur:
- Bir hizmet sağlayıcısı, birden çok kuruluşlardan kullanıcıların Azure Stack erişmesine izin vermek istiyor.

![Azure AD ile Azure Stack çok kiracılı topolojisi](media/azure-stack-identity-architecture/multi-tenant.png)

Bu topoloji, aşağıdaki özelliklere sahiptir:
- Bir kuruluş başına temelinde kaynaklarına erişimi olmalıdır. 
- Bir kuruluşun kullanıcıları kuruluş kullanıcılar kaynaklara erişimi vermek olmalıdır. 
- Yöneticiler (bulut operatörlerinin) için kimlikleri, kullanıcıların kimliklerini bir ayrı dizin kiracısı olabilir. Bu ayrım kimlik sağlayıcısı düzeyinde hesabı yalıtım sağlar. 
 
### <a name="ad-fs"></a>AD FS  
AD FS topoloji gereklidir aşağıdaki koşullardan biri doğruysa:
- Azure Stack, internet'e bağlanmaz.
- Azure Stack, internet'e bağlanabilir, ancak kimlik sağlayıcınız için AD FS kullanmak üzere seçin.
  
![AD FS kullanarak azure Stack topolojisi](media/azure-stack-identity-architecture/adfs.png)

Bu topoloji, aşağıdaki özelliklere sahiptir:
- Üretim ortamında bu topoloji kullanımını desteklemek için yerleşik bir Azure Stack AD FS örneği bir federasyon güveni üzerinden Active Directory tarafından desteklenen mevcut AD FS örneği ile tümleştirmeniz gerekir. 
- Azure stack'teki graf hizmeti mevcut Active Directory örneğinizle tümleştirebilirsiniz. Azure AD Graph API ile tutarlı API'lerini destekleyen OData tabanlı Graph API hizmetini de kullanabilirsiniz. 

  Active Directory örneğinizle etkileşim kurmak için salt okunur izinlere sahip kullanıcı kimlik bilgileri, Active Directory örneğinden Graph API'sini gerektirir. 
  - Yerleşik AD FS örneği, Windows Server 2016'da temel alır. 
  - AD FS ve Active Directory örneklerinizi, Windows Server 2012 veya sonraki sürümlerde temel alması gerekir. 
  
  Active Directory örneğinizi ve yerleşik bir AD FS örneği arasında etkileşimler Openıd Connect için sınırlı değildir ve bunlar birbirini destekleyen herhangi bir protokolünü kullanabilirsiniz. 
  - Kullanıcı hesapları oluşturulur ve şirket içi Active Directory Örneğinizde yönetilir.
  - Hizmet sorumluları ve uygulamalar için kayıtları yerleşik Active Directory örneğinde yönetilir.



## <a name="next-steps"></a>Sonraki adımlar
- [Kimliğe genel bakış](azure-stack-identity-overview.md)   
- [Veri Merkezi tümleştirmesi - kimlik](azure-stack-integrate-identity.md)
