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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 306d16dd306fc569181e0334e6674a9c935aac1f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299915"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi bulma

Erişim kapsamları ve rol yapılandırma API'leri erişmesi. İstemci uygulamaları için kaynak uygulaması web API'lerini kullanıma sunmak istiyorsanız, erişim kapsamları ve rol API için yapılandırmanız gerekir. Bir web API'sine erişmek için bir istemci uygulaması isterseniz, uygulama kaydında API'sine erişim izni yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API, kullanıma sunduğunuzda API görüntülenmesi **bir API seçin** listesinde bir uygulama kaydı için izinler eklerken. Erişim kapsamları eklemek için özetlenen adımları izleyin. [erişim kapsamları kaynak uygulamanıza ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'leri erişmek için bir istemci uygulaması yapılandırma

Uygulama kaydınızı birini eklediğinizde, bu izinleri yapabilecekleriniz **API erişimi Ekle** sunulan web API'leri için. Web API'leri erişmek için özetlenen adımları izleyin. [kimlik bilgileri veya web API'lerine erişim izni ekleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Azure Active Directory Uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


