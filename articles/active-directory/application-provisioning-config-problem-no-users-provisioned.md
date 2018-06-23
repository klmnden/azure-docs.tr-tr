---
title: Azure AD galeri uygulamaya hiçbir kullanıcı sağlanan | Microsoft Docs
description: Sık rastlanan sorunları giderme konusunda karşılaştığı görüntülenmesini kullanıcılar görmüyorsanız, Azure AD bir kullanıcı Azure AD ile sağlamak için yapılandırdığınız uygulama Galerisi
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
ms.date: 05/04/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 394e8642c177312c8990ea211f77fb802d4228fd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332418"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulamaya hiçbir kullanıcı sağlandı

Bir kez otomatik sağlama (uygulamaya bağlanmak için Azure AD için sağlanan uygulama kimlik bilgilerinin geçerli olduğunu doğrulama dahil) bir uygulama için yapılandırıldı. Kullanıcılar ve/veya grupları uygulamaya sağlanır ve şunları tarafından belirlenir:

-   Hangi kullanıcıların ve grupların olmuştur **atanan** uygulama. Atama hakkında daha fazla bilgi için bkz: [bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Desteklemediğini **öznitelik eşlemelerini** etkinleştirilir ve uygulamaya Azure AD'den geçerli öznitelikleri eşitlemek için yapılandırılmış. Öznitelik eşlemelerini hakkında daha fazla bilgi için bkz: [özelleştirme kullanıcı sağlama öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Olsun veya olmasın bir **kapsam filtresi** kullanıcıları filtreleme mevcut dayalı belirli öznitelik değerleri. Kapsam belirleme filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam belirleme filtreleri ile öznitelik tabanlı uygulama sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Kullanıcıların sağlanan değil Gözlemleme, belirli bir kullanıcı için günlük girdilerini arayın ve Azure AD'de denetim günlüklerine bakın.

Sağlama denetim günlüklerini Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlüklerini** sekmesi. Günlükleri filtre **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori. "İçinde öznitelik eşlemelerini kendileri için yapılandırılan eşleşen ID" temel alarak kullanıcılara arayabilirsiniz. Örneğin "kullanıcı asıl adı" veya "Azure AD tarafında eşleşen öznitelik olarak e-posta adresi" yapılandırılmış ve değil sağlama kullanıcı değerine sahip "audrey@contoso.com". Denetim günlüklerini arama "audrey@contoso.com" ve gözden geçirin, ardından girdi döndü.

Sağlama, hedef uygulama kullanıcılarla varlığı için sorgulama, kullanıcı nesneleri karşılaştırma kapsamında atanan kullanıcılar için Azure AD Sorgulama dahil olmak üzere sağlama hizmeti tarafından gerçekleştirilen tüm işlemler kayıt sağlama denetim günlüklerini Sistem arasında. Ardından ekleme, güncelleştirme veya karşılaştırma üzerine dayalı hedef sistem kullanıcı hesabı devre dışı bırakın.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Genel sorun alanlarından dikkate alınması gereken hazırlama

Başlatılacağı konum hakkında bir fikir varsa, ayrıntılarına geçebilir genel sorunlu alanları listesi aşağıdadır.

* [Başlatmak için hizmet sağlama görünmüyor](#provisioning-service-does-not-appear-to-start)
* [Atanmış olan olsa bile kullanıcılar atlanacak ve sağlanan değil, denetim günlüklerini söyleyin](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Başlatmak için hizmet sağlama görünmüyor

Ayarlarsanız **sağlama durumu** olmasını **üzerinde** içinde **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt;Sağlama** Azure portalının bölümü. Ancak başka durumunu Ayrıntılar gösterilir bu sayfada sonraki yeniden yükler sonra hizmeti çalışıyor ancak bir ilk eşitleme henüz tamamlanmadı olasıdır. Denetleme **denetim günlüklerini** hizmet gerçekleştirip, hangi işlemleri belirlemek için yukarıda açıklanan ve herhangi bir hata varsa.

>[!NOTE]
>Bir başlangıç eşitlemesi 20 dakika arasında herhangi bir yere Azure AD dizini ve sayısı, kullanıcı sağlama kapsamında boyutuna bağlı olarak birkaç saat sürebilir. İlk eşitleme sonrasında sonraki eşitlemeler daha hızlı olduğunu sağlama hizmeti ilk eşitleme sonrasında her iki sistem durumunu temsil filigranlar depolar. Bu, sonraki eşitlemeler performansını artırır.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Denetim günlüklerini kullanıcılar atlanacak ve atanmış olan olsa bile sağlanan değil söyleyin

Bir kullanıcı "denetim günlüklerini atlandı"olarak görünür, genişletilmiş ayrıntıları nedenini belirlemek için günlük iletisi okumak çok önemlidir. Ortak nedenleri ve çözümleri aşağıda verilmiştir:

-   **Bir kapsam filtresi yapılandırılmış** **filtrelemesi kullanıcı bir öznitelik değerini temel**. Kapsam belirleme filtreleri ile ilgili daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **Kullanıcı "etkili bir şekilde alınarak" değil.** Bu belirli bir hata iletisi görürseniz, Azure AD'de depolanan kullanıcı atama kaydı ile ilgili bir sorun olduğundan değildir. Bu sorun, atamayı kullanıcı (veya grubu) uygulamadan düzeltin ve yeniden yeniden atamak için. Atama hakkında daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Gerekli bir öznitelik eksik veya bir kullanıcı için doldurulan değil.** Gözden geçirin ve hangi kullanıcı (veya grubu) özellikleri akışı Azure AD'den uygulamaya tanımlamak iş akışları ve öznitelik eşlemelerini yapılandırmak için sağlama yukarı ayarlanırken dikkate alınması gereken önemli bir şey olabilir. Bu içerir "eşleşen özellik" ayarı benzersiz olarak tanımlamak ve kullanıcıları/grupları iki sistem arasında eşleştirmek için kullanılabilir. Bu önemli işlemi hakkında daha fazla bilgi için bkz: [özelleştirme kullanıcı sağlama öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Öznitelik grupları için eşlemeleri:** grup adı ve bazı uygulamalar için destekleniyorsa üyelerin yanı sıra Grup ayrıntılarını sağlama. Etkinleştirmek veya bu işlevi etkinleştirmek veya devre dışı bırakarak devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. Grupları sağlama etkinse, uygun bir alan "Eşleşen kimliği" kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Bu olabilir görünen ad veya e-posta diğer adı), boş veya bir grup için doldurulan eşleşen özellik Azure AD içinde ise, Grup ve üyelerini sağlanması değil olarak.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Connect eşitleme: bildirim temelli hazırlama anlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

