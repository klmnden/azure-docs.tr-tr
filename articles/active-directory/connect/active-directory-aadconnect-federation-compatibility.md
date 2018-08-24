---
title: Azure AD federasyonu uyumluluk listesi
description: Bu sayfada, çoklu oturum açmayı uygulamak için kullanılan Microsoft olmayan kimlik sağlayıcılarını alır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 3b2313b79c57a95af40d29bca3d7522c83e10e4c
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818723"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD federasyonu uyumluluk listesi
Azure Active Directory çoklu oturum sağlar ve herhangi bir üçüncü taraf çözümü gerek kalmadan Office 365 ve diğer Microsoft Online Hizmetleri için karma ve yalnızca bulutta yer alan uygulamalar için uygulama erişimi güvenliği artırılmış. Office 365 gibi Microsoft Çevrimiçi Hizmetleri, çoğu Dizin Hizmetleri, kimlik doğrulama ve yetkilendirme için Azure Active Directory ile tümleştirilmiştir. Azure Active Directory çoklu oturum açma için binlerce SaaS uygulamasına da sağlar ve şirket içi web uygulamaları. Azure Active Directory bkz [uygulama Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) desteklenen SaaS uygulamaları için. 

## <a name="idp-validation"></a>IDP doğrulama
Kuruluşunuz bir üçüncü taraf Federasyon çözümü kullanıyorsa, üçüncü taraf Federasyon çözümü Azure ile uyumlu olması şartıyla, çoklu oturum açma, Office 365 gibi Microsoft Online services ile şirket içi Active Directory kullanıcılarınız için yapılandırabilirsiniz Active Directory.  Uyumluluk ile ilgili sorularınız için lütfen kimlik sağlayıcınıza başvurun.  Microsoft, Azure AD ile uyumluluk tıklatın önceden sınanmış kimlik sağlayıcıları listesini görmek isterseniz [burada](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft artık Azure Active Directory ile uyumluluk için bağımsız kimlik sağlayıcıları için doğrulama sınaması sağlar. Ürününüzü birlikte çalışabilirlik için test etmek istiyorsanız, lütfen başvurun [yönergeleri](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Sonraki Adımlar

- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
- [Azure AD Connect ve federasyon](active-directory-aadconnectfed-whatis.md)
