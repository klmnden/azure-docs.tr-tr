---
title: Azure AD galeri uygulaması için hiçbir kullanıcı sağlanmıyor | Microsoft Docs
description: Kullanıcıların Azure AD galeri uygulaması, Azure AD ile kullanıcı sağlama için yapılandırdığınız görüntülenmesini görmüyor genişlettiklerinde karşılaştığı yaygın sorunları giderme
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
ms.date: 09/20/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: eaeb97f88c2482cb9d091afb1c205e9b09a85ce0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65784581"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulaması için hiçbir kullanıcı sağlanmıyor
Otomatik sağlama (uygulamaya bağlanmak için Azure AD'ye sağlanan uygulama kimlik bilgilerinin geçerli olduğunu doğrulama dahil) bir uygulama için yapılandırıldıktan sonra uygulamaya kullanıcı ve/veya grupları sağlanır. Sağlama aşağıdakiler tarafından belirlenir:

-   Hangi kullanıcılar ve gruplar olmuştur **atanan** uygulama. Atama hakkında daha fazla bilgi için bkz. [kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamak](assign-user-or-group-access-portal.md).
-   Olup olmadığını **öznitelik eşlemelerini** etkinleştirildiğinden ve geçerli Azure AD öznitelikleri uygulamaya eşitlemek için yapılandırılmış. Öznitelik eşlemeleri hakkında daha fazla bilgi için bkz. [özelleştirme kullanıcı sağlama öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için](customize-application-attributes.md).
-   Var olup olmadığını bir **kapsam belirleme filtresi** kullanıcıları filtreleme mevcut alan özel öznitelik değerleri. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam filtreleri ile öznitelik tabanlı uygulama sağlama](define-conditional-rules-for-provisioning-user-accounts.md).
  
Kullanıcıların sağlanan değil gözlemlerseniz, Azure AD'de denetim günlüklerine bakın. Belirli bir kullanıcı için günlük girişlerini arayın.

Sağlama denetim günlüklerinin Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlükleri** sekmesi. Günlükleri filtreleyin **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori. "İçinde öznitelik eşlemelerini kendileri için yapılandırılan eşleşen ID" göre kullanıcılar için arama yapabilirsiniz. Örneğin "kullanıcı asıl adı" veya "Azure AD tarafında eşleşen öznitelik olarak e-posta adresi" yapılandırılmış ve değerini değil sağlama kullanıcı varsa, "audrey@contoso.com", ardından Denetim günlüklerini arama "audrey@contoso.com" ve girişlerini gözden geçirin döndürdü.

Sağlama hizmeti, Azure AD sağlama, hedef uygulamayı kullanıcılarıyla varlığı için sorgulama ve kullanıcı nesneleri karşılaştırma için kapsamda olan atanan kullanıcılar için sorgulama dahil olmak üzere tüm işlemleri gerçekleştirmiş kaydı sağlama denetim günlükleri Sistem arasında. Ardından ekleme, güncelleştirme veya hedef sistem üzerinde karşılaştırma tabanlı kullanıcı hesabı devre dışı bırak.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Genel sorun alanlarından dikkate alınması gereken sağlama
Nereden başlayacağınızı hakkında bir fikir varsa, ayrıntılarına ulaşabilirsiniz genel sorunlu alanları listesi aşağıda verilmiştir.

- [Sağlama hizmeti başlatmak için görünmüyor.](#provisioning-service-does-not-appear-to-start)
- [Atanmış oldukları olsa bile kullanıcılar atlandı ve sağlanmadı, Denetim günlükleri söyleyin](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatmak için görünmüyor.
Ayarlarsanız **sağlama durumu** olmasını **üzerinde** içinde **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt;Sağlama** Azure portal'ın bölümü. Ancak başka durumu Ayrıntılar gösterilir bu sayfada sonraki yeniden yükler sonra hizmet çalışıyor ancak bir ilk eşitleme henüz tamamlanmadı olasıdır. Denetleme **denetim günlükleri** şu hizmet gerçekleştiriyor, hangi işlemleri belirlemek için yukarıda açıklanan ve herhangi bir hata varsa.

>[!NOTE]
>Bir ilk eşitleme 20 dakika veya herhangi bir Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat sürebilir. Sağlama hizmeti ilk eşitleme sonrasında her iki sistem durumunu temsil eden filigranlar depoları olarak ilk eşitleme sonrasında sonraki eşitlemeler hızlıdır. İlk eşitleme, sonraki eşitlemeler performansını artırır.
>


## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Denetim günlükleri kullanıcılar atlandı ve atanmış oldukları olsa bile sağlanmadı varsayalım.

Kullanıcı "Denetim günlüklerinde atlandı gibi" gösterilir, günlük iletisinde nedenini belirlemek için genişletilmiş ayrıntılarını okumak önemlidir. Sık karşılaşılan nedenleri ve çözümleri aşağıda verilmiştir:

- **Kapsam belirleme filtresi yapılandırılmış** **, filtre uygulayarak kullanıcının bir öznitelik değerine göre**. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
- **Kullanıcı, "etkili bir şekilde yetkili".** Bu belirli hata iletisini görürseniz, Azure AD'de depolanan kullanıcı atama kaydı ile ilgili bir sorun olduğundan olur. Bu sorunu gidermek için kullanıcı (veya grup) uygulamadan atamasını kaldırmak ve yeniden atayabilirsiniz. Atama hakkında daha fazla bilgi için bkz. [kullanıcı veya grup erişimi atama](assign-user-or-group-access-portal.md).
- **Gerekli bir özniteliği eksik veya doldurulmuş bir kullanıcı değil.** Sağlamayı ayarlama ayarlanırken dikkate alınması gereken önemli bir şeyi gözden geçirin ve hangi kullanıcı (veya grup) özellikleri akış Azure ad uygulama tanımlayan iş akışları ve öznitelik eşlemelerini yapılandırmaktır. Bu yapılandırma ayarı "benzersiz biçimde tanımlayan ve kullanıcılar/gruplar iki sistem arasındaki eşleştirmek için kullanılan eşleştirme özelliği" içerir. Bu önemli işlem hakkında daha fazla bilgi için bkz. [özelleştirme kullanıcı sağlama öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için](customize-application-attributes.md).
- **Öznitelik eşlemeleri için grupları:** Grup adını ve üyelerinin yanı sıra grubu ayrıntıları bazı uygulamalar için destekleniyorsa sağlama. Etkinleştirme veya etkinleştirme veya devre dışı bırakarak bu işlevi devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. Grupları sağlama etkinse, "Eşleşen kimliği" için uygun bir alanı kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Eşleşen kimlik, görünen ad veya e-posta diğer adı olabilir. Eşleşen özellik boş ya da doldurulmuş bir grup için Azure AD'de ise, Grup ve üyelerini sağlanmayan.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Connect eşitlemesi: Bildirim Temelli Sağlamayı Anlama](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
