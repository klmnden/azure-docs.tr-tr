---
title: Visual Studio Webapı projelerinde Azure AD ile çalışmaya başlama | Microsoft Docs
description: Azure Active Directory Webapı projelerinde bağlanma veya Visual Studio kullanarak Azure AD oluşturduktan sonra kullanmaya başlamak nasıl bağlı Hizmetleri
services: active-directory
documentationcenter: ''
author: ghogen
manager: douge
editor: ''
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: f44c60cc02ef93fb8bd08954d8b059889eb82df1
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Azure Active Directory (Webapı projeleri) ile çalışmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bir ASP.NET Webapı projesine Active Directory ekledikten sonra bu makalede, ek yönergeler sağlanmaktadır. **Proje > Bağlantılı Hizmetler** Visual Studio komutu. Projeniz için hizmet henüz eklediyseniz, herhangi bir zamanda bunu yapabilirsiniz.

Bkz: [my Webapı projeye ne?](vs-active-directory-webapi-what-happened.md) projenize bağlı hizmet eklerken değişikliklerinin.

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicileri kimlik doğrulaması gerektiren

Projenizdeki tüm denetleyicileri ile donatılan `[Authorize]` özniteliği. Bu öznitelik kullanıcıya bu denetleyicileri tarafından tanımlanan API'leri erişmeden önce kimlik doğrulaması gerektirir. Denetleyici anonim erişime izin vermek için bu öznitelik denetleyicisinden kaldırın. Daha ayrıntılı bir düzeyde izinleri ayarlamak istiyorsanız, denetleyici sınıfı için uygulama yerine yetkilendirme gerektiren her yöntemi için özniteliğini uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](guidedsetups/active-directory-aspnetwebapp-v1.md)
