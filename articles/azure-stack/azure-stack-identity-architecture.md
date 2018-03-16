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
ms.date: 2/28/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 899e0fc0c1eb93d68c79c92c9cc042462ebc2fef
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Azure yığın kimliği mimarisi
Azure yığın ile kullanmak için bir kimlik sağlayıcısı seçmeden önce Active Directory Federasyon Hizmetleri (AD FS) ile Azure Active Directory (Azure AD) seçenekleri arasındaki önemli farklılıkları anlayın. 

## <a name="capabilities-and-limitations"></a>Özellikleri ve sınırlamaları 
Seçtiğiniz kimlik sağlayıcısı çoklu kiracı için destek dahil olmak üzere seçeneklerinizi sınırlayabilirsiniz. 

  

|Yetenek veya senaryosu        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|İnternet'e bağlı     |Evet       |İsteğe bağlı|
|Çoklu kiracı için destek     |Evet       |Hayır      |
|Market dağıtım       |Evet       |Evet. Kullanılmasını gerektiren [çevrimdışı Market dağıtım](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) aracı.|
|Active Directory Authentication Library (ADAL) desteği |Evet |Evet|
|Azure CLI, Visual Studio ve PowerShell gibi araçlar desteği  |Evet |Evet|
|Azure Portalı aracılığıyla hizmet asıl adı oluşturma     |Evet |Hayır|
|Hizmet asıl adı ile sertifikaları oluşturma      |Evet |Evet|
|Hizmet asıl adı (anahtarlar) parolaları ile oluşturma    |Evet |Hayır|
|Uygulamalar için grafik hizmetini kullanabilirsiniz           |Evet |Hayır|
|Uygulamaları oturum açmak için kimlik sağlayıcısı kullanın |Evet |Evet. Şirket içi birleştirmek için uygulamaları gerektirir AD FS örnekleri. |

## <a name="topologies"></a>Topolojileri
Aşağıdaki discus kullanabileceğiniz çeşitli kimlik topolojileri bölümler.

### <a name="azure-ad-single-tenant-topology"></a>Azure AD: tek Kiracı topolojisi 
Azure yığın yüklediğinizde ve Azure AD, varsayılan olarak, bir tek Kiracı topolojisi Azure yığın kullanır. 

Bir tek Kiracı topolojisi ne zaman yararlı olur:
- Tüm kullanıcılar aynı Kiracı'nın bir parçasıdır.
- Bir hizmet sağlayıcısı, bir kuruluş için bir Azure yığın örneği barındırır. 

![Azure AD ile Azure yığın tek Kiracı topolojisi](media/azure-stack-identity-architecture/single-tenant.png)

Bu topoloji, aşağıdaki özelliklere sahiptir:
- Tüm uygulamaları Azure yığın kaydeder ve Dizin Hizmetleri aynı Azure ad Kiracı. 
- Yalnızca kullanıcıları ve belirteçleri de dahil olmak üzere bu dizine uygulamalardan Azure yığın kimliğini doğrular. 
- Yöneticiler (bulut operatörleri) ve Kiracı kullanıcılar için aynı directory kiracısında kimlik hesaplardır. 
- Bu Azure yığın ortamını erişmek bir kullanıcı başka bir dizindeki etkinleştirmek için şunları yapmalısınız [kullanıcı konuk olarak davet](azure-stack-identity-overview.md#guest-users) Kiracı dizinine. 

### <a name="azure-ad-multi-tenant-topology"></a>Azure AD: çok kiracılı topolojisi
Bulut operatörleri, bir veya daha fazla kuruluşlardan uygulamaları kiracılar tarafından erişim sağlamak için Azure yığın yapılandırabilirsiniz. Kullanıcılar, kullanıcı portalı üzerinden uygulamaları erişin. Bu yapılandırmada (bulut operatörü tarafından kullanılır) Yönetici portalı kullanıcılara tek bir dizinden sınırlıdır. 

Bir çok kiracılı topolojisi ne zaman yararlı olur:
- Bir hizmet sağlayıcısı, birden çok kuruluşlardan kullanıcıların Azure yığın erişmesine izin vermek istiyor.

![Azure AD ile Azure yığını çok kiracılı topolojisi](media/azure-stack-identity-architecture/multi-tenant.png)

Bu topoloji, aşağıdaki özelliklere sahiptir:
- Bir kuruluş başına temelinde kaynaklara erişimi olması gerekir. 
- Bir kuruluştaki kullanıcılar kendi kuruluş dışında olan kullanıcılar için kaynaklara erişim izni alamadı olmalıdır. 
- Yöneticiler (bulut operatörleri) için kimlikleri kimlikleri kullanıcılar için ayrı bir dizini kiracısından olabilir. Bu ayrım kimlik sağlayıcısı düzeyinde hesap yalıtım sağlar. 
 
### <a name="ad-fs"></a>AD FS  
AD FS topolojisi gereklidir aşağıdaki koşullardan herhangi biri doğruysa:
- Azure yığın Internet'e bağlanmaz.
- Azure yığın internet'e bağlanabiliyor, ancak AD FS, kimlik sağlayıcısı için kullanmak üzere seçin.
  
![AD FS kullanarak azure yığın topolojisi](media/azure-stack-identity-architecture/adfs.png)

Bu topoloji, aşağıdaki özelliklere sahiptir:
- Üretimde bu topoloji kullanımını desteklemek için Active Directory tarafından bir federasyon güveni üzerinden yedeklenen var olan bir AD FS örneğini ile yerleşik Azure yığın AD FS örneğini tümleştirmeniz gerekir. 
- Var olan Active Directory örneğinizle Azure yığın grafik hizmetinde tümleştirebilirsiniz. Azure AD grafik API'si ile tutarlı API'lerini destekleyen OData tabanlı grafik API'si hizmetini de kullanabilirsiniz. 

  Active Directory örneğinizle etkileşim kurmak için salt okunur izni olan kullanıcı kimlik bilgilerini Active Directory örneğinizi grafik API'sini gerektirir. 
  - Yerleşik AD FS örneği üzerinde Windows Server 2016 temel alır. 
  - AD FS ve Active Directory örnekleri, Windows Server 2012 veya sonraki sürümlerde dayalı olması gerekir. 
  
  Active Directory örneğinizi yerleşik AD FS örneği arasında etkileşimleri Openıd Connect için sınırlı değildir ve herhangi bir birbirini desteklenen protokolünü kullanabilirsiniz. 
  - Kullanıcı hesapları oluşturulur ve şirket içi Active Directory örneğinizi yönetilir.
  - Hizmet sorumluları ve kayıtlar uygulamalar için yerleşik Active Directory örneğinde yönetilir.



## <a name="next-steps"></a>Sonraki adımlar
- [Kimliği'ne genel bakış](azure-stack-identity-overview.md)   
- [Veri Merkezi tümleştirmesi - kimliği](azure-stack-integrate-identity.md)
