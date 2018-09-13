---
title: Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi nasıl | Microsoft Docs
description: Belirli bir özel API erişmek için ihtiyaç duyduğunuz izinleri yapılandırmak nasıl Azure AD uygulaması geliştirdi
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 8d0b9f219d7a0bc61e3d12acfaae6015963401f9
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724226"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi bulma

Erişim kapsamları ve rol yapılandırma API'leri erişmesi. İstemci uygulamaları için kaynak uygulaması web API'lerini kullanıma sunmak istiyorsanız, erişim kapsamları ve rol API için yapılandırmanız gerekir. Bir web API'sine erişmek için bir istemci uygulaması isterseniz, uygulama kaydında API'sine erişim izni yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Web API'leri kullanıma sunmak için bir kaynak uygulaması yapılandırma

Web API, kullanıma sunduğunuzda API görüntülenmesi **bir API seçin** listesinde bir uygulama kaydı için izinler eklerken. Erişim kapsamları eklemek için özetlenen adımları izleyin. [erişim kapsamları kaynak uygulamanıza ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'leri erişmek için bir istemci uygulaması yapılandırma

Uygulama kaydınızı birini eklediğinizde, bu izinleri yapabilecekleriniz **API erişimi Ekle** sunulan web API'leri için. Web API'leri erişmek için özetlenen adımları izleyin. [kimlik bilgileri veya web API'lerine erişim izni ekleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Azure Active Directory Uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


