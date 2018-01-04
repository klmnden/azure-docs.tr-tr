---
title: "Ne zaman belirli bir kullanıcı bir uygulamaya erişim kuramaz çıkışı Bul | Microsoft Docs"
description: "Kritik düzeyde önemli bir kullanıcı, kullanıcı Azure AD ile sağlamak için yapılandırdığınız uygulama erişebilmeleri zaman öğrenmek nasıl"
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
ms.openlocfilehash: 42c305ad48e6994b1d2c729b849acf665e575807
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a>Belirli bir kullanıcı bir uygulamaya erişim mümkün olduğunda öğrenin
Bir uygulama ile otomatik kullanıcı sağlamayı kullanırken, Azure AD uygulama sağlama ve güncelleştirme kullanıcı hesapları gibi şeyleri temel alınarak otomatik olarak [kullanıcı ve grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) genellikle her 10 dakikada bir düzenli olarak zamanlanan saat aralığında.

## <a name="how-long-does-it-take"></a>Ne kadar sürer?

Sağlanacak belirli bir kullanıcı için geçen süre, çoğunlukla desteklemediğini ilk bir "tam" eşitleme zaten oluştu üzerinde bağlıdır.

İlk eşitleme Azure AD arasında ve bir uygulama herhangi bir yere Azure AD dizini ve sayısı, kullanıcı sağlama kapsamında boyutuna bağlı olarak birkaç saat 20 dakika sürebilir. 

Sonraki eşitlemeler performansını iyileştirme ilk eşitleme sonrasında her iki sistem durumunu temsil filigranlar sağlama hizmeti depolar ilk eşitleme sonrasında sonraki eşitlemeler (örn. 10 dakika içinde), daha hızlı olması.

## <a name="how-to-check-the-status-of-a-user"></a>Kullanıcı durumunu denetleme

Seçilen kullanıcı için sağlama durumunu görmek için Azure AD'de denetim günlüklerine bakın.

Sağlama denetim günlüklerini Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlüklerini** sekmesi. Günlükleri filtre **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori. "İçinde öznitelik eşlemelerini kendileri için yapılandırılan eşleşen ID" temel alarak kullanıcılara arayabilirsiniz. 

Örneğin "kullanıcı asıl adı" veya "Azure AD tarafında eşleşen öznitelik olarak e-posta adresi" yapılandırılmış ve değil sağlama kullanıcı değerine sahip "audrey@contoso.com", denetim günlüklerini arama "audrey@contoso.com" ve gözden geçirin, ardından girdi döndü.

Sağlama denetim günlüklerini sağlama hizmeti tarafından gerçekleştirilen tüm işlemlerin kayıt dahil olmak üzere:

* Azure AD sağlama kapsamında atanan kullanıcılar için sorgulama
* Hedef uygulama kullanıcılarla varlığı için sorgulama
* Sistem arasında kullanıcı nesneleri karşılaştırma
* Ekleme, güncelleştirme ya da karşılaştırma üzerine dayalı hedef sistem kullanıcı hesabı devre dışı bırakma

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirmek](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''
