---
title: Bir uygulama için oturum açarken beklenmedik bir onay istemi | Microsoft Docs
description: Bir kullanıcı girmek istemediğiniz Azure AD ile tümleşik bir uygulama için bir onay istemi gördüğünde sorunlarını giderme
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
ms.openlocfilehash: 83d043ecef152f977437e21e2caec40d1c40ce0d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65781144"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Bir uygulama için oturum açarken beklenmedik bir onay istemi

Azure Active Directory ile tümleştirme, birçok uygulama çeşitli kaynaklarıyla çalıştırmak için ilgili izinleri gerektirir. Bu kaynakları Azure Active Directory ile bunlara erişmek için izinleri de zaman tümleşik Azure AD'ye onay çerçevesine kullanarak istenir. 

Bu genellikle tek seferlik bir işlem olduğu uygulamanın ilk kullanılışında gösterilen bir onay istemi olduğu sonuçlanır. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Hangi kullanıcıların gördüğü senaryoları onay istemleri

Ek istemleri, çeşitli senaryolarda beklenebilir:

* Uygulama için gereken izinler kümesini değişti.

* İlk olarak uygulamaya onay veren kullanıcıya yönetici değildi ve artık farklı (yönetici olmayan) kullanıcı uygulamayı ilk kez kullanıyor.

* Yönetici ilk uygulamaya onay veren kullanıcıya ait olduğu, ancak adına tüm kuruluş izin vermedi.

* Uygulamayı kullanarak [artımlı ve dinamik onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) onay başlangıçta verildikten sonra ilave izin istemek için. Bu, genellikle isteğe bağlı ek uygulama özelliklerini temel işlevselliği için gerekli olanlar dışında izinleri gerektiğinde kullanılır.

* Başlangıçta verilen sonra onay iptal edildi.

* Geliştirici uygulamayı her kullanıldığında bir onay istemi gerektirecek şekilde yapılandırılmış (Not: Bu en iyi yöntem değildir).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamalar, izinler ve onay Azure Active Directory'de (v1.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Kapsamlar, izinler ve onay Azure Active Directory'de (v2.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


