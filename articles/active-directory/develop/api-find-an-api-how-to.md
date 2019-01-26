---
title: Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi nasıl | Microsoft Docs
description: Belirli bir özel API erişmek için ihtiyaç duyduğunuz izinleri yapılandırmak nasıl Azure AD uygulaması geliştirdi
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: b8c0f6a72d3b48c8e93a21fe3ad68e5799b39a5a
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078415"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi bulma

Erişim kapsamları ve rol yapılandırma API'leri erişmesi. İstemci uygulamaları için kaynak uygulaması web API'lerini kullanıma sunmak istiyorsanız, erişim kapsamları ve rol API için yapılandırmanız gerekir. Bir web API'sine erişmek için bir istemci uygulaması isterseniz, uygulama kaydında API'sine erişim izni yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API, kullanıma sunduğunuzda API görüntülenmesi **bir API seçin** listesinde bir uygulama kaydı için izinler eklerken. Erişim kapsamları eklemek için özetlenen adımları izleyin. [erişim kapsamları kaynak uygulamanıza ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'leri erişmek için bir istemci uygulaması yapılandırma

Uygulama kaydınızı birini eklediğinizde, bu izinleri yapabilecekleriniz **API erişimi Ekle** sunulan web API'leri için. Web API'leri erişmek için özetlenen adımları izleyin. [kimlik bilgileri veya web API'lerine erişim izni ekleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Azure Active Directory Uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


