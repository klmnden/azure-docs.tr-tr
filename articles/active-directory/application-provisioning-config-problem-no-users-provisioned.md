---
title: Azure AD galeri uygulaması için hiçbir kullanıcı sağlanmıyor | Microsoft Docs
description: Sık karşılaşılan sorunları giderme karşılaştığı görüntülenmesini kullanıcılar görmüyorsanız, bir Azure ad galeri Azure AD ile kullanıcı sağlama için yapılandırdığınız uygulama
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
ms.topic: conceptual
ms.date: 05/04/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: e3017612fe27ba5efbcdbf66c04d4d15641f216b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364666"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulaması için hiçbir kullanıcı sağlanmıyor

Otomatik sağlama (uygulamaya bağlanmak için Azure AD'ye sağlanan uygulama kimlik bilgilerinin geçerli olduğunu doğrulama dahil) bir uygulama için yapılandırıldıktan sonra. Kullanıcılar ve/veya grupları uygulamaya sağlanır ve şunları tarafından belirlenir:

-   Hangi kullanıcılar ve gruplar olmuştur **atanan** uygulama. Atama hakkında daha fazla bilgi için bkz. [kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Olup olmadığını **öznitelik eşlemelerini** etkinleştirildiğinden ve geçerli Azure AD öznitelikleri uygulamaya eşitlemek için yapılandırılmış. Öznitelik eşlemeleri hakkında daha fazla bilgi için bkz. [özelleştirme kullanıcı sağlama öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Var olup olmadığını bir **kapsam belirleme filtresi** kullanıcıları filtreleme mevcut alan özel öznitelik değerleri. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam filtreleri ile öznitelik tabanlı uygulama sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Kullanıcılar sağlanan değil gözleme, Azure AD'de denetim günlüklerine bakın ve belirli bir kullanıcı için günlük girişlerini arayın.

Sağlama denetim günlüklerinin Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlükleri** sekmesi. Günlükleri filtreleyin **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori. "İçinde öznitelik eşlemelerini kendileri için yapılandırılan eşleşen ID" göre kullanıcılar için arama yapabilirsiniz. Örneğin "kullanıcı asıl adı" veya "Azure AD tarafında eşleşen öznitelik olarak e-posta adresi" yapılandırılmış ve değil sağlama kullanıcı değeri olan "audrey@contoso.com". Ardından Denetim günlüklerini arama "audrey@contoso.com" ve gözden geçirin, ardından döndürülen girdileri.

Sağlama hizmeti, Azure AD sağlama, hedef uygulamayı kullanıcılarıyla varlığı için sorgulama ve kullanıcı nesneleri karşılaştırma için kapsamda olan atanan kullanıcılar için sorgulama dahil olmak üzere tüm işlemleri gerçekleştirmiş kaydı sağlama denetim günlükleri Sistem arasında. Ardından ekleme, güncelleştirme veya hedef sistem üzerinde karşılaştırma tabanlı kullanıcı hesabı devre dışı bırak.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Genel sorun alanlarından dikkate alınması gereken sağlama

Nereden başlayacağınızı hakkında bir fikir varsa, ayrıntılarına ulaşabilirsiniz genel sorunlu alanları listesi aşağıda verilmiştir.

* [Sağlama hizmeti başlatmak için görünmüyor.](#provisioning-service-does-not-appear-to-start)
* [Atanmış oldukları olsa bile kullanıcılar atlandı ve sağlanmadı, Denetim günlükleri söyleyin](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatmak için görünmüyor.

Ayarlarsanız **sağlama durumu** olmasını **üzerinde** içinde **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt;Sağlama** Azure portal'ın bölümü. Ancak başka durumu Ayrıntılar gösterilir bu sayfada sonraki yeniden yükler sonra hizmet çalışıyor ancak bir ilk eşitleme henüz tamamlanmadı olasıdır. Denetleme **denetim günlükleri** şu hizmet gerçekleştiriyor, hangi işlemleri belirlemek için yukarıda açıklanan ve herhangi bir hata varsa.

>[!NOTE]
>Bir ilk eşitleme 20 dakika veya herhangi bir Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat sürebilir. Sağlama hizmeti ilk eşitleme sonrasında her iki sistem durumunu temsil eden filigranlar depoları olarak ilk eşitleme sonrasında sonraki eşitlemeler hızlıdır. Bu, sonraki eşitlemeler performansını artırır.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Denetim günlükleri kullanıcılar atlandı ve atanmış oldukları olsa bile sağlanmadı varsayalım.

Kullanıcı "Denetim günlüklerinde atlandı gibi" gösterilir, günlük iletisinde nedenini belirlemek için genişletilmiş ayrıntılarını okumak çok önemlidir. Sık karşılaşılan nedenleri ve çözümleri aşağıda verilmiştir:

-   **Kapsam belirleme filtresi yapılandırılmış** **, filtre uygulayarak kullanıcının bir öznitelik değerine göre**. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **Kullanıcı, "etkili bir şekilde yetkili".** Bu belirli hata iletisini görürseniz, Azure AD'de depolanan kullanıcı atama kaydı ile ilgili bir sorun olduğundan olur. Bu sorun, powerbı.com'u kullanıcı (veya grup) uygulamadan düzeltin ve yeniden yeniden atamak için. Atama hakkında daha fazla bilgi için bkz. <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Gerekli bir özniteliği eksik veya doldurulmuş bir kullanıcı değil.** Gözden geçirin ve hangi kullanıcı (veya grup) özellikleri akış Azure ad uygulama tanımlayan iş akışları ve öznitelik eşlemelerini yapılandırma sağlamayı ayarlama ayarlanırken dikkate alınması gereken önemli bir şey olabilir. Bu içerir "eşleştirme özelliği" ayarı benzersiz biçimde tanımlayan ve kullanıcılar/gruplar iki sistem arasındaki eşleştirmek için kullanılır. Bu önemli işlem hakkında daha fazla bilgi için bkz. [özelleştirme kullanıcı sağlama öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Öznitelik eşlemeleri için gruplar:** grubu adını ve bazı uygulamalar için destekleniyorsa üyelerin ek olarak grubu ayrıntıları sağlama. Etkinleştirme veya etkinleştirme veya devre dışı bırakarak bu işlevi devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. Grupları sağlama etkinse, "Eşleşen kimliği" için uygun bir alanı kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Bu olabilir görünen ad veya e-posta diğer adı), eşleşen özellik boş ya da doldurulmuş bir grup için Azure AD'de ise grup ve üyelerini sağlanacak değil olarak.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Connect eşitleme: anlama, bildirim temelli sağlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

