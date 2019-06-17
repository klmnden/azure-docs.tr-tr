---
title: Azure dijital İkizlerini API kimlik doğrulaması anlama | Microsoft Docs
description: Azure dijital İkizlerini bağlanıp API'leri için kimlik doğrulaması için kullanma
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: lyrana
ms.openlocfilehash: 4ea4479d77e06940bed50859341952ffbcbbda46
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60533841"
---
# <a name="connect-and-authenticate-to-apis"></a>Bağlanıp API'lerine kimlikleri

Azure dijital İkizlerini, kullanıcıların kimliklerini doğrulamak ve uygulamaları korumak için Azure Active Directory (Azure AD) kullanır. Azure AD, çeşitli modern mimarileri için kimlik doğrulamasını destekler. Bunların tümünde, OAuth 2.0 veya Openıd Connect endüstri standardı protokollerine dayalıdır. Ayrıca, geliştiriciler, Azure AD'ye tek kiracılı ve satır iş kolu (LOB) uygulamaları oluşturmak için kullanabilirsiniz. Geliştiriciler, Azure AD çok kiracılı uygulamalar geliştirmek için de kullanabilirsiniz.

Azure AD genel bakış için ziyaret [temelleri sayfa](https://docs.microsoft.com/azure/active-directory/fundamentals/index) için adım adım kılavuzlar, kavramlar ve hızlı başlangıçları.

Bir uygulama veya hizmeti Azure AD ile tümleştirmek için geliştiricilerin önce uygulamayı Azure AD'ye kaydetmesi gerekir. Ayrıntılı yönergeler ve ekran görüntüleri için bkz. [Bu hızlı başlangıçta](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

[Beş birincil uygulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) Azure AD tarafından desteklenir:

* Tek sayfalı uygulama (SPA): Bir kullanıcının Azure AD tarafından güvenliği sağlanan bir tek sayfalı uygulama için oturum açmanız gerekir.
* Web uygulamasına Web tarayıcısı: Bir kullanıcının Azure AD tarafından güvenliği sağlanan bir web uygulaması için oturum açmanız gerekir.
* Web API'si yerel uygulamaya: Telefon, tablet veya PC çalıştıran yerel bir uygulama, Azure AD tarafından güvenliği sağlanan bir web API'sini kaynakları almak için bir kullanıcının kimliğini doğrulaması gerekir.
* Web uygulaması web API'si için: Bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir web uygulaması gerekir.
* Web API arka plan programı ya da sunucu uygulamasında: Daemon uygulamasının veya bir sunucu uygulaması web ile Azure AD tarafından güvenliği sağlanan bir web API'den kaynakları almak kullanıcı Arabirimi gerekir.

Windows Azure kimlik doğrulama kitaplığı, Active Directory belirteçlerini almak için birçok yol sunar. Kitaplık ve kod örnekleri hakkında daha fazla bilgi için bkz: [bu makalede](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Dijital İkizlerini bir orta katman web API'si çağırma

Geliştiriciler dijital İkizlerini çözümleri tasarlama, bunlar genellikle bir orta katman uygulama veya API oluşturun. Daha sonra dijital İkizlerini API uygulaması veya API aşağı yönde çağırır. Bu standart web çözümü mimari desteklemek için emin kullanıcıların ilk:

1. Orta katman uygulama ile kimlik doğrulaması

1. Bir OAuth 2.0 On-Behalf-Of belirteci kimlik doğrulaması sırasında alınır

1. Alınan belirteç kimlik doğrulaması veya daha fazla On-Behalf-Of akışı aşağı yönde kullandığınız API'leri çağırmak için kullanılır

On-behalf-of akışı düzenlemek nasıl hakkında yönergeler için bkz: [OAuth 2.0 On-Behalf-Of akış](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Kod örnekleri de görüntüleyebilirsiniz [Aşağı Akış web API'si çağırma](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma ve Azure dijital OAuth 2.0 örtülü izin akışı kullanarak çiftlerini sınamak için okuma [Postman yapılandırma](./how-to-configure-postman.md).

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).