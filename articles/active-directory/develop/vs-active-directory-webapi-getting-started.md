---
title: Azure AD'de Visual Studio .NET Webapı projelerini kullanmaya başlama
description: Bağlı hizmetler nasıl bağlanmak veya Visual Studio kullanarak Azure AD'yi oluşturduktan sonra Azure Active Directory içinde .NET Webapı projelerini kullanmaya başlama
services: active-directory
author: ghogen
manager: douge
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
origin.date: 03/12/2018
ms.date: 09/03/2018
ms.author: v-junlch
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 662d958fc7ef6fe5c9d0e61a1d8e48983d10196f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60353814"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Azure Active Directory (.NET Webapı projelerini) ile çalışmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Active Directory bir ASP.NET Webapı projeye ekledikten sonra bu makalede ek yönergeler sunulmuştur **Proje > bağlı hizmetler** Visual Studio'nun komutu. Hizmet projenize henüz eklediyseniz, herhangi bir zamanda bunu yapabilirsiniz.

Bkz: [Webapı projeme ne oldu?](vs-active-directory-webapi-what-happened.md) projenize bağlı hizmet ekleme sırasında yapılan değişiklikler için.

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicileri kimlik doğrulaması gerektiren

Projenizdeki tüm denetleyicileri ile donatılmış `[Authorize]` özniteliği. Bu öznitelik bu denetleyicileri tarafından tanımlanan API'leri erişmeden önce doğrulanmasını gerektirir. Anonim olarak erişim için denetleyici izin vermek için bu öznitelik denetleyicisinden kaldırın. Daha ayrıntılı bir düzeyde izinleri ayarlamak istiyorsanız, denetleyici sınıfı için uygulama yerine yetkilendirme gerektiren her yöntem için özniteliğini uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](quickstart-v1-aspnet-webapp.md)

<!-- Update_Description: link update -->