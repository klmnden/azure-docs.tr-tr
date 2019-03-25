---
title: JavaScript ve sayfa sözleşme sürümleri için Azure Active Directory B2C kullanıcı akışlarında | Microsoft Docs
description: JavaScript'i etkinleştirin ve Azure Active Directory B2C kullanıcı akışı özelleştirmek için sayfa sözleşme sürümlerini kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5102755c9e830f43fa92e8546e5125960e0a2f9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401551"
---
# <a name="about-using-javascript-and-page-contract-versions-in-a-user-flow"></a>Kullanıcı akışı JavaScript ve sayfa sözleşme sürümlerini kullanma hakkında

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C, paket içeriği için kullanıcı arabirimi öğeleri, kullanıcı Akışları'nda, HTML, CSS ve JavaScript içeren bir dizi sağlar. Etkinleştirmek istiyorsanız [JavaScript](javascript-samples.md) istemci tarafı kod, kullanıcı akışları, sizin alma, JavaScript üzerinde öğeleri sabittir emin olmak istersiniz. Aksi takdirde, herhangi bir değişiklik beklenmeyen davranışı, kullanıcı akışı sayfaları neden olabilir. Bu sorunları önlemek için bir kullanıcı akışı için bir sayfa sözleşme kullanımını zorunlu kılmak ve bir sayfa sözleşme sürümü belirtin. Bunun yapılması emin olmanızı Javascript'inizi bağlı içerik tanımlarını sabittir. JavaScript için bir kullanıcı akışı etkinleştirmek düşünmüyorsanız bile, kullanıcı akışı sayfalarınız için bir sayfa sözleşme sürümü belirtebilirsiniz.

> [!NOTE]
> Kullanıcı akışları için JavaScript anlatılmaktadır, ancak Ayrıca JavaScript kullanın ve kullanırken sayfa sözleşme sürümlerini seçin [özel ilkeler](page-contract.md).

## <a name="enable-javascript"></a>JavaScript'i etkinleştir

Kullanıcı akışı özellikleri, JavaScript, ayrıca bir sayfa sözleşme kullanımını zorunlu kılar etkinleştirebilirsiniz. Ardından, sonraki bölümde açıklandığı gibi sayfa sözleşme sürümü ayarlayabilirsiniz.

![JavaScript ayarını etkinleştirin](media/user-flow-javascript-overview/javascript-settings.PNG)

## <a name="specify-a-page-contract-version"></a>Bir sayfa sözleşme sürümü belirtin

Kullanıcı akışınızın özelliklerinde JavaScript'i etkinleştirin olsun veya olmasın, kullanıcı akışı sayfalarınız için bir sayfa sözleşme sürümü belirtebilirsiniz. Kullanıcı akışı açın ve seçin **sayfa düzenleri**. Altında **Düzen adı**, bir kullanıcı Akış sayfası seçip **sayfa sözleşme sürümü**.

![JavaScript ayarını etkinleştirin](media/user-flow-javascript-overview/page-contract-version.PNG)

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [JavaScript örnekleri kullanılmak üzere Azure Active Directory B2C](javascript-samples.md).
