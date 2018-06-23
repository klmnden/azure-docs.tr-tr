---
title: Özel geliştirilmiş bir uygulama için gereken belirli bir API bulma | Microsoft Docs
description: Azure AD uygulaması, belirli bir API, özel erişim için gereken izinleri nasıl yapılandıracağınızı geliştirilmiş
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 94f3c68e9a1433d2a433e2a3bbe557b487e7bf75
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332067"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel geliştirilmiş bir uygulama için gereken belirli bir API bulma

Erişim API'lerine erişim kapsamları ve rol yapılandırmasını gerektirir. İstemci uygulamaları için kaynak uygulama web API'lerini kullanıma sunmak istiyorsanız, erişim kapsamları ve rol API için yapılandırmanız gerekir. Bir istemci uygulaması web API'si erişmek isterseniz, app kaydında API'sine erişim izni yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Web API'leri kullanıma sunmak için bir kaynak uygulamasını yapılandırma

Web API, kullanıma zaman API görüntülenmesi **bir API seçin** izinleri bir uygulama kaydı eklerken listesi. Erişim kapsamları eklemek için özetlenen adımları izleyin [kaynak uygulamanıza erişim kapsamları ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'leri erişmek için bir istemci uygulamasını yapılandırma

Uygulama kaydınızı izinleri eklediğinizde, şunları yapabilirsiniz **API erişim Ekle** sunulan Web API'leri. Web API'leri erişmek için özetlenen adımları izleyin [kimlik bilgileri veya web API'leri erişim izinleri eklemek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Azure Active Directory Uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


