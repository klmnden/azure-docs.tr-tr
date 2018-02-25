---
title: "Azure yığın kimliği mimarisi | Microsoft Docs"
description: "Azure yığın ile kullanabileceğiniz kimlik mimarisi hakkında bilgi edinin."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 0f3e28f7726afab02211902b5ba2e478ae8065df
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Azure yığın kimliği mimarisi
Azure yığın ile kullanmak için bir kimlik sağlayıcısı seçmeden önce Active Directory Federasyon Hizmetleri (AD FS) ile Azure Active Directory (Azure AD) seçenekleri arasındaki önemli farklılıkları anlayın. 

## <a name="capabilities-and-limitations"></a>Özellikleri ve sınırlamaları 
Seçtiğiniz kimlik sağlayıcısı çoklu kiracı için destek dahil olmak üzere seçeneklerinizi sınırlayabilirsiniz. 

  

|Yetenek veya senaryosu        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|İnternet'e bağlı     |Evet       |İsteğe bağlı|
|Çoklu kiracı için destek     |Evet       |Hayır      |
|Market dağıtım       |Evet       |Evet - kullanılmasını gerektiren [çevrimdışı Market dağıtım](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) aracı.|
|Active Directory Authentication Library (ADAL) desteği |Evet |Evet|
|Azure komut satırı arabirimi (CLI), Visual Studio (VS) ve PowerShell gibi araçlar desteği  |Evet |Evet|
|Hizmet sorumluları Portalı aracılığıyla oluşturma     |Evet |Hayır|
|Hizmet asıl adı ile sertifikaları oluşturma      |Evet |Evet|
|Hizmet asıl adı (anahtarlar) parolaları ile oluşturma    |Evet |Hayır|
|Uygulamalar için grafik hizmetini kullanabilirsiniz           |Evet |Hayır|
|Uygulamaları oturum açmak için kimlik sağlayıcısı kullanın |Evet |Evet - şirket içi ile birleştirmek için uygulamaları gerektirir AD FS. |

## <a name="topologies"></a>Topolojileri
Aşağıdaki bölümlerde kullanabileceğiniz kimlik topolojileri hakkında bilgi sağlar.

### <a name="azure-ad--single-tenant"></a>Azure AD – tek Kiracı 
Azure yığın yüklediğinizde ve Azure AD, varsayılan olarak, bir tek Kiracı topolojisi Azure yığın kullanır. 

Bir tek Kiracı topolojisi ne zaman yararlı olur:
- Tüm kullanıcılar aynı Kiracı'nın bir parçasıdır.
- Bir hizmet sağlayıcısı, bir kuruluş için bir Azure yığın örneği barındırır.  

![Azure AD ile tek bir kiracı topolojisi kullanarak azure yığın topolojisi](media/azure-stack-identity-architecture/single-tenant.png)

Bu topoloji ile:
- Tüm uygulamaları Azure yığın kaydeder ve Dizin Hizmetleri aynı Azure ad Kiracı. 
- Yalnızca kullanıcıları ve belirteçleri de dahil olmak üzere bu dizine uygulamalardan Azure yığın kimliğini doğrular. 
- Yöneticiler (bulut operatörleri) ve Kiracı kullanıcılar için aynı directory kiracısında kimlik hesaplardır. 
- Bu Azure yığın ortamını erişmek bir kullanıcı başka bir dizindeki etkinleştirmek için şunları yapmalısınız [kullanıcı konuk olarak davet](azure-stack-identity-overview.md#guest-users) Kiracı dizinine.  

### <a name="azure-ad--multi-tenant"></a>Azure AD – çok kiracılı
Bulut operatörleri, bir veya daha fazla kuruluşlardan uygulamaları kiracılar tarafından erişim sağlamak için Azure yığın yapılandırabilirsiniz. Kullanıcılar, kullanıcı portalı üzerinden uygulamaları erişin. Bu yapılandırmada (bulut operatörü tarafından kullanılır) Yönetici portalı kullanıcılara tek bir dizinden sınırlıdır. 

Bir çok kiracılı topolojisi ne zaman yararlı olur:
- Bir hizmet sağlayıcısı, birden çok kuruluşlardan kullanıcıların Azure yığın erişmesine izin vermek istiyor.

![Azure AD ile bir çok kiracılı topolojisi kullanarak azure yığın topolojisi](media/azure-stack-identity-architecture/multi-tenant.png)

Bu topoloji ile:
- Bir kuruluş başına temelinde kaynaklara erişimi olması gerekir. 
- Bir kuruluştaki kullanıcılar kendi kuruluş dışında olan kullanıcılar için kaynaklara erişim izni olmamalıdır.  
- Kimlikleri Yöneticiler (bulut operatörleri) için ayrı bir dizini Kiracı kullanıcılar için kimlikleri daha olabilir. Bu ayrım kimlik sağlayıcısı düzeyinde hesap yalıtım sağlar. 
 
### <a name="ad-fs"></a>AD FS  
AD FS topolojisi gereklidir aşağıdaki koşullardan herhangi biri doğruysa:
- Azure yığın Internet'e bağlanmaz.
- Azure yığın Internet'e bağlanabiliyor, ancak AD FS, kimlik sağlayıcısı için kullanmak üzere seçin.
  
![AD FS kullanarak azure yığın topolojisi](media/azure-stack-identity-architecture/adfs.png)

Bu topoloji ile:
- Kullanımını desteklemek için üretimde, Active Directory (AD) ile bir federasyon güveni üzerinden yedeklenen var olan bir AD FS örneği ile yerleşik Azure yığın AD FS örneğini tümleştirmeniz gerekir. 
- Azure yığın grafik hizmetinde var olan AD ile tümleştirebilirsiniz.  OData de kullanabilirsiniz, Azure AD grafik API'si ile tutarlı olan API'lerine destekleyen grafik API'si hizmet tabanlı.  

  AD ile etkileşim kurmak için salt okunur izni olan kullanıcı kimlik bilgilerini AD grafik API'si gerektirir AD. 
  - Yerleşik AD FS sunucu 2016 tarihinde temel alır. 
  - AD FS ile AD Server 2012 veya önceki sürümlerde temel gerekir. 
  
  AD ve yerleşik AD FS arasında etkileşimler Openıd Connect için sınırlı değildir ve herhangi bir birbirini desteklenen protokol kullanabilirsiniz.  
  - Kullanıcı hesapları oluşturulur ve şirket içinde yönetilir AD.
  - Hizmet sorumluları ve kayıtlar uygulamalar için yerleşik AD içinde yönetilir.



## <a name="next-steps"></a>Sonraki adımlar
- [Kimliği'ne genel bakış](azure-stack-identity-overview.md)   
- [Veri Merkezi tümleştirmesi - kimliği](azure-stack-integrate-identity.md)