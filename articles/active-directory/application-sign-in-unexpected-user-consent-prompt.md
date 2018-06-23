---
title: Bir uygulama için oturum açarken beklenmeyen onay istemi | Microsoft Docs
description: Bir kullanıcı değil beklediğiniz neydi Azure AD ile tümleşik bir uygulama için bir onay istemi gördüğünde ile ilgili sorunları giderme
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
ms.reviewer: asteen
ms.openlocfilehash: 096f099c7fc44078cc6c6329b7022613ef09a0d2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331069"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Bir uygulama için oturum açarken beklenmeyen onay istemi

Azure Active Directory ile tümleştirme birçok uygulamaları çalıştırmak için çeşitli kaynaklara izinleri gerektirir. Ne zaman bu kaynakları Azure Active Directory ile bunlara erişmek için izinleri de tümleştirilmiştir Azure AD onay framework kullanılarak istenir. 

Bu tek seferlik bir işlem olduğu çoğunlukla ilk kez bir uygulama kullanıldığında, gösterilen bir onay istemi olan içinde sonuçlanır. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Hangi kullanıcıların gördüğü senaryoları istemleri onayı

Ek istekler çeşitli senaryolarda beklenebilir:

* Uygulama için gereken izinler kümesi değişti.

* İlk olarak uygulamaya rıza kullanıcı yönetici değil ve farklı (yönetici olmayan) bir kullanıcı ilk kez uygulama artık kullanıyor.

* İlk olarak uygulamaya rıza kullanıcı için bir yönetici olsa da, bunlar tüm kuruluş ın adına izin değil.

* Uygulamayı kullanarak [artımlı ve dinamik izin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) onay başlangıçta verildi sonra ilave izin istemek için. Ek bir uygulamanın isteğe bağlı özellikler ötesinde temel işlevleri için gereken izinleri gerektirir, bu genellikle kullanılır.

* Onay, başlangıçta verilmeden sonra iptal edildi.

* Geliştirici uygulamayı her kullanılışında bir onay istemi gerektirecek şekilde yapılandırılmış olduğundan (Not: Bu en iyi yöntem değildir).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamalar, izinler ve onay Azure Active Directory'de (v1.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Kapsam, izinleri ve onay Azure Active Directory'de (v2.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


