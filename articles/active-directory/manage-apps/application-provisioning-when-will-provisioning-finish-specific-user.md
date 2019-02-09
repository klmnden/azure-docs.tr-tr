---
title: Belirli bir kullanıcı bir uygulamaya erişmeye çalıştığında ne zaman sunulacaktır kullanıma bulma | Microsoft Docs
description: Kritik düzeyde önemli bir kullanıcı Azure AD ile kullanıcı sağlama için yapılandırmış olduğunuz bir uygulamaya erişmeye çalıştığında, bulma
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.openlocfilehash: 2f6ebd6d9024c912e1af427ac5a5b62a817651ab
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964381"
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a>Belirli bir kullanıcı bir uygulamaya erişmeye çalıştığında ne zaman öğrenin
Bir uygulama ile otomatik kullanıcı hazırlama kullanırken, Azure AD göre otomatik olarak bir uygulamada kullanıcı hesaplarını sağlama ve güncelleştirme gibi şeyleri [kullanıcı ve Grup atamasına](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) bir düzenli olarak zamanlanan saat aralığı, genellikle her 10 dakika.

## <a name="how-long-does-it-take"></a>Ne kadar sürer?

Belirli bir kullanıcının sağlanması gereken zaman, çoğunlukla olsun veya olmasın "tam" ilk eşitlemeden gerçekleşmiş üzerinde bağlıdır.

Azure AD arasında ilk eşitleme ve uygulama herhangi bir Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat 20 dakika sürebilir. 

İlk eşitleme sonrasında sonraki eşitlemeler sağlama hizmeti sonraki eşitlemeler performansını iyileştirme ilk eşitlemeden sonra her iki sistem durumunu temsil eden filigranlar depoları olarak (örn: 10 dakika içinde), daha hızlı.

## <a name="how-to-check-the-status-of-a-user"></a>Bir kullanıcının durumunu denetleme

Seçilen kullanıcı için sağlama durumunu görmek için Azure AD'de denetim günlüklerini inceleyin.

Sağlama denetim günlüklerinin Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlükleri** sekmesi. Günlükleri filtreleyin **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori. "İçinde öznitelik eşlemelerini kendileri için yapılandırılan eşleşen ID" göre kullanıcılar için arama yapabilirsiniz. 

Örneğin "kullanıcı asıl adı" veya "Azure AD tarafında eşleşen öznitelik olarak e-posta adresi" yapılandırılmış ve değerini değil sağlama kullanıcı varsa, "audrey@contoso.com", ardından Denetim günlüklerini arama "audrey@contoso.com" ve ardından girişlerini gözden geçirin döndürdü.

Sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri sağlama denetim günlüklerini kaydetme dahil olmak üzere:

* Azure AD sağlama kapsamında olan atanan kullanıcılar için sorgulama
* Hedef uygulama bu kullanıcıların varlığı için sorgulama
* Sistem arasındaki kullanıcı nesneleri karşılaştırma
* Ekleme, güncelleştirme veya karşılaştırma üzerine dayalı hedef sistemde kullanıcı hesabı devre dışı bırakma

## <a name="next-steps"></a>Sonraki adımlar
[Sağlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına kullanıcı otomatikleştirmek](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''
