---
title: Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi nasıl | Microsoft Docs
description: Belirli bir özel API erişmek için ihtiyaç duyduğunuz izinleri yapılandırmak nasıl Azure AD uygulaması geliştirdi
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2492a9346585698132e7fd9cfcde068ffd60ebc5
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476160"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API'yi bulma

Erişim kapsamları ve rol yapılandırma API'leri erişmesi. İstemci uygulamaları için kaynak uygulaması web API'lerini kullanıma sunmak istiyorsanız, erişim kapsamları ve rol API için yapılandırmanız gerekir. Bir web API'sine erişmek için bir istemci uygulaması isterseniz, uygulama kaydında API'sine erişim izni yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API, kullanıma sunduğunuzda API görüntülenmesi **bir API seçin** listesinde bir uygulama kaydı için izinler eklerken. Erişim kapsamları eklemek için özetlenen adımları izleyin. [web API'lerini kullanıma sunmak için uygulama yapılandırma](quickstart-configure-app-expose-web-apis.md).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'leri erişmek için bir istemci uygulaması yapılandırma

Uygulama kaydınızı birini eklediğinizde, bu izinleri yapabilecekleriniz **API erişimi Ekle** sunulan web API'leri için. Web API'leri erişmek için özetlenen adımları izleyin. [web API'lerine erişmek için bir istemci uygulamasını yapılandırmak](quickstart-configure-app-access-web-apis.md).

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure Active Directory Uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


