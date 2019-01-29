---
title: Özel olarak geliştirilmiş bir uygulamada oturum açarken sorun | Microsoft Docs
description: Bir uygulamaya oturum açabilir olmaması için neden olabilecek yaygın rrors Azure AD ile geliştirilmiştir.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: b776fab035a169132a18866c100370da12b3febb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197543"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada oturum açarken sorun

Bir uygulamada oturum açması mümkün olmaması için neden olabilecek çeşitli hatalar var. Bu sorun büyük neden değinilmektedir uygulamaları yanlış.

## <a name="errors-related-to--misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalarıyla ilgili hataları

* Uygulamanızda sahip olduğunuz yapılandırmaları portalında eşleştiğini doğrulayın. Özellikle, istemci/uygulama kimliği, yanıt URL'leri, istemci gizli dizileri/anahtarları ve uygulama kimliği URI'si karşılaştırın.

* Kod içinde yapılandırılan izinlerle erişim isteyen kaynak karşılaştırma **gerekli kaynakları** yapılandırdığınız kaynaklar yalnızca istek emin olmak için sekmesinde.

* Bkz: [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) benzer hatalar veya sorunlar için.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Onay ve Azure ad uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Uygulama onay ve Azure AD v2.0 için kullanarak yakınsanmış](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
