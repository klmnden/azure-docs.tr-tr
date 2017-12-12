---
title: "Azure AD galeri uygulamaya kullanıcı sağlamayı olduğu alma saat veya daha fazla bilgi | Microsoft Docs"
description: "Uygulamanıza sağlama neden öğrenmek nasıl beklediğinizden daha uzun sürüyor"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c9ed12569c0adc5ed8625a8d9fc81c9bee874cd4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Alma saat veya daha fazla olduğundan, Azure AD galeri uygulamaya kullanıcı hazırlama

İlk otomatik bir uygulama için sağlama olanağı, ilk eşitleme herhangi bir yere 20 dakika arasında Azure AD dizini ve sayısı, kullanıcı sağlama kapsamında boyutuna bağlı olarak birkaç saat sürebilir. 

İlk eşitleme sonrasında sonraki eşitlemeler daha hızlı olması sağlama hizmeti, sonraki eşitlemeler performansını iyileştirme ilk eşitleme sonrasında her iki sistem durumunu temsil filigranlar depolar.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansı nasıl

İlk eşitleme birkaç saat sürüyorsa, performansı iyileştirmek için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsam filtreleri.** Kapsam filtreleri için ince ayar sağlama hizmet kullanıcıları belirli öznitelik değerlerine göre filtreleyerek Azure AD'den ayıklar veri sağlar. Kapsam belirleme filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam belirleme filtreleri ile öznitelik tabanlı uygulama sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme](active-directory-saas-app-provisioning.md)

