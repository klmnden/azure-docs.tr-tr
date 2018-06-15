---
title: Özel geliştirilmiş uygulamaya oturumu açmada sorun | Microsoft Docs
description: Azure AD ile geliştirilen bir uygulamayı imzalayabilirsiniz olmaması için neden olabilecek yaygın rrors
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 57b620f45d1985351064020e122c088584bcdcf5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26614146"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Özel geliştirilmiş uygulama oturum açma sorunları

Uygulamaya imzalayabilirsiniz olmaması için neden olabilecek birçok hata vardır. Bu sorun büyük neden değinilmektedir uygulamaları yanlış.

## <a name="errors-related-to--misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalara ilgili hatalar

* Uygulamanızda sahip yapılandırmaları portalında eşleşen doğrulayın. Özellikle, istemci/uygulama kimliği, yanıt URL'leri, istemci parolaları/anahtarları ve uygulama kimliği URI'si karşılaştırın.

* Yapılandırılan izne sahip Code erişim isteyen kaynak karşılaştırmak **gerekli kaynakları** sekmesinde yapılandırdığınız kaynaklar yalnızca istek emin olun.

* Bkz: [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) benzer hatalar veya sorunlar için.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[İzin ve uygulamaları Azure ad ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Onay ve Azure AD v2.0 için adının uygulamaları Yakınsanan](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
