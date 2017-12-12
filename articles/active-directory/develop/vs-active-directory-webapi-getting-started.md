---
title: "Visual Studio Webapı projelerinde Azure AD ile çalışmaya başlama | Microsoft Docs"
description: "Azure Active Directory Webapı projelerinde bağlanma veya Visual Studio kullanarak Azure AD oluşturduktan sonra kullanmaya başlamak nasıl bağlı Hizmetleri"
services: active-directory
documentationcenter: 
author: kraigb
manager: mtillman
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 3d510c193ab8c7e65340275017cb2dcd4c76def0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Azure Active Directory ve Visual Studio bağlı hizmetler (Webapı projeleri) ile Başlarken
> [!div class="op_single_selector"]
> * [Başlarken](vs-active-directory-webapi-getting-started.md)
> * [Ne oldu](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicileri kimlik doğrulaması gerektiren
Projenizdeki tüm denetleyicileri ile donatılan **Authorize** özniteliği. Bu öznitelik kullanıcıya bu denetleyicileri tarafından tanımlanan API'leri erişmeden önce kimlik doğrulaması gerektirir. Denetleyici anonim erişime izin vermek için bu öznitelik denetleyicisinden kaldırın. Daha ayrıntılı bir düzeyde izinleri ayarlamak istiyorsanız, denetleyici sınıfı için uygulama yerine yetkilendirme gerektiren her yöntemi için özniteliğini uygulayın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory/)

