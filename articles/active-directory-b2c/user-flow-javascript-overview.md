---
title: JavaScript ve sayfa sözleşme sürümler - Azure Active Directory B2C | Microsoft Docs
description: JavaScript'i etkinleştirin ve Azure Active Directory B2C'de sayfa sözleşme sürümlerini kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ef474bec71a9015209b5748b6947816002bd4a5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66511971"
---
# <a name="javascript-and-page-contract-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C sürümlerinde sözleşme, JavaScript ve sayfa

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C kullanıcı akışları ve özel ilkeler, kullanıcı arabirimi öğeleri için HTML, CSS ve JavaScript içeren paket İçerik kümesi sağlar. Uygulamalarınız için JavaScript'i etkinleştirmek için bir öğeye ekleyin, [özel ilke](active-directory-b2c-overview-custom.md) veya portalı kullanıcı Akışları'nda etkinleştirmek, bir sayfa sözleşme seçin ve kullanmak [b2clogin.com](b2clogin.md) isteklerinizdeki.

Etkinleştirmek istiyorsanız [JavaScript](javascript-samples.md) istemci tarafı kod, istemeniz emin olmak için sizin alma, JavaScript üzerinde öğeleri sabittir. Aksi takdirde, herhangi bir değişiklik kullanıcı sayfalarınızda beklenmeyen davranışlara neden olabilir. Bu sorunları önlemek için bir sayfa sözleşme kullanımını zorunlu kılmak ve bir sayfa sözleşme sürümü belirtin. Bunun yapılması sağlar, JavaScript tabanlı içerik tanımlarını sabittir. JavaScript etkinleştirmek düşünmüyorsanız bile, bir sayfa sözleşme sürümü sayfalarınız için belirtebilirsiniz.

## <a name="user-flows"></a>Kullanıcı akışları

Kullanıcı akışı özellikleri, JavaScript, ayrıca bir sayfa sözleşme kullanımını zorunlu kılar etkinleştirebilirsiniz. Ardından, sonraki bölümde açıklandığı gibi sayfa sözleşme sürümü ayarlayabilirsiniz.

![JavaScript ayarını etkinleştirin](media/user-flow-javascript-overview/javascript-settings.png)

Kullanıcı akışınızın özelliklerinde JavaScript'i etkinleştirin olsun veya olmasın, kullanıcı akışı sayfalarınız için bir sayfa sözleşme sürümü belirtebilirsiniz. Kullanıcı akışı açın ve seçin **sayfa düzenleri**. Altında **Düzen adı**, bir kullanıcı Akış sayfası seçip **sayfa sözleşme sürümü**.

![JavaScript ayarını etkinleştirin](media/user-flow-javascript-overview/page-contract-version.png)

## <a name="custom-policies"></a>Özel ilkeler

Özel ilkeleri JavaScript'i etkinleştirmek için eklediğiniz **ScriptExecution** öğesine **RelyingParty** özel ilke dosyanızdaki öğesi. Daha fazla bilgi için [JavaScript örnekleri kullanılmak üzere Azure Active Directory B2C](javascript-samples.md).

Özel ilkelerinizi JavaScript'i etkinleştirin olsun veya olmasın, bir sayfa sözleşme sürümü sayfalarınız için belirtebilirsiniz. Bir sayfa sözleşme belirtme hakkında daha fazla bilgi için bkz. [özel ilkeleri kullanarak, Azure Active Directory B2C, bir sayfa sözleşme seçin](page-contract.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [JavaScript örnekleri kullanılmak üzere Azure Active Directory B2C](javascript-samples.md).
