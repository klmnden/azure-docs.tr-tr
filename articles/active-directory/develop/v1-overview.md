---
title: Azure Active Directory geliştiricilerin (v1.0) genel bakış
description: Bu makalede genel bir bakış sağlar. Microsoft imzalama iş ve Okul hesaplarında platform ve Azure Active Directory v1.0 uç nokta kullanarak.
services: active-directory
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13cc5c7ae428f74f2892e6066dfdcd7efb73efbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545227"
---
# <a name="azure-active-directory-for-developers-v10-overview"></a>Azure Active Directory geliştiricilerin (v1.0) genel bakış

Azure Active Directory (Azure AD), geliştiricilerin Microsoft iş veya okul hesabına sahip kullanıcıların güvenli şekilde oturumunu açan uygulamalar derlemesine olanak sağlayan bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, iş kolu (LOB) uygulamalarını derleyen geliştiricileri ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricileri destekler. Temel oturum açmaya ek olarak Azure AD, uygulamaların hem [Microsoft Graph](https://docs.microsoft.com/graph/overview) gibi Microsoft API’lerini hem de Azure AD platformunda derlenen özel API’leri çağırmasını sağlar. Bu belgeler, OAuth2.0 ve OpenID Connect gibi sektör standardı protokolleri kullanarak Azure AD desteğini uygulamanıza nasıl ekleyeceğinizi göstermektedir.

> [!NOTE]
> Bu sayfadaki içeriğin büyük bir v1.0 üzerinde odaklanır uç noktası ve yalnızca Microsoft destekleyen platformu, iş veya Okul hesapları. Tüketici veya kişisel Microsoft hesaplarının oturum açmak istiyorsanız, bilgilere bakın [v2.0 uç noktası ve platform](v2-overview.md). V2.0 uç noktası, tüm Microsoft kimliklerini oturum açmak istediğiniz uygulamalar için merkezi bir geliştirici deneyimi sunar.

| | |
| --- | --- |
|[Kimlik doğrulaması temel bilgileri](authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş. |
|[Uygulama türleri](app-types.md) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış. |
| | |

## <a name="get-started"></a>başlarken

V1.0 kılavuzlarımız ve öğreticilerimizden yararlanarak Azure AD Authentication Library (ADAL) SDK'sını kullanarak tercih ettiğiniz platformda uygulama derleme konusunu inceleyin. Bkz: **v1.0 hızlı Başlangıçlar** ve **v1.0 öğreticiler** içinde [Microsoft kimlik Platformu (geliştiriciler için Azure Active Directory)](index.yml) kullanmaya başlamak için.

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

Bkz: **nasıl yapılır kılavuzları v1.0** ayrıntılı bilgi ve yönergeler Azure AD'de en yaygın görevleri için.

## <a name="reference-topics"></a>Başvuru konuları

Aşağıdaki makaleler, Azure AD’de kullanılan API'ler, protokol iletileri ve terimler hakkında ayrıntılı bilgi sağlar.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Kimlik Doğrulama Kitaplıkları (ADAL)](active-directory-authentication-libraries.md)   | Azure AD tarafından sağlanan kitaplıklara ve SDK’lara genel bakış. |
| [Kod örnekleri](sample-v1-code.md)                                  | Tüm Azure AD kod örneklerinin listesi. |
| [Sözlük](developer-glossary.md)                                      | Bu belgelerde kullanılan terminoloji ve sözcük tanımları. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
