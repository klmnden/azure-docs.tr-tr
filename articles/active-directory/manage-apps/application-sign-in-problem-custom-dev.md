---
title: Özel olarak geliştirilmiş bir uygulamada oturum açarken sorun | Microsoft Docs
description: Azure AD ile geliştirilmiş bir uygulamaya oturum açabilir olmaması için neden olabilecek yaygın hataları
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8ad2499aea8bf4e41ca00d6c78d76e112f0493e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825232"
---
# <a name="problems-signing-in-to-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada oturum açma sorunları

Bir uygulamada oturum açması mümkün olmaması için neden olabilecek çeşitli hatalar var. Bu sorun büyük neden değinilmektedir uygulamaları yanlış.

## <a name="errors-related-to--misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalarıyla ilgili hataları

* Uygulamanızda sahip olduğunuz yapılandırmaları portalında eşleştiğini doğrulayın. Özellikle, istemci/uygulama kimliği, yanıt URL'leri, istemci gizli dizileri/anahtarları ve uygulama kimliği URI'si karşılaştırın.

* Kod içinde yapılandırılan izinlerle erişim isteyen kaynak karşılaştırma **gerekli kaynakları** yapılandırdığınız kaynaklar yalnızca istek emin olmak için sekmesinde.

* Bkz: [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) benzer hatalar veya sorunlar için.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Onay ve Azure ad uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Uygulama onay ve Azure AD v2.0 için kullanarak yakınsanmış](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
