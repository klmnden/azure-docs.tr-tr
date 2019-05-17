---
title: Azure AD galeri uygulaması için kullanıcı sağlama yapılandırma sorunu | Microsoft Docs
description: Azure AD uygulama galerisinde bulunan yapılandırma kullanıcı uygulamaya zaten sağlama listelenen genişlettiklerinde karşılaştığı yaygın sorunları giderme
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
ms.openlocfilehash: 42bffdc1960a87c931e914896e8e36de45991bd4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784124"
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulaması için kullanıcı sağlama yapılandırma sorunu

Yapılandırma [otomatik kullanıcı hazırlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) bir uygulama için (destekleniyorsa), belirli yönergeler otomatik sağlama için uygulama hazırlama için izlenmesi gerekir. Ardından uygulamaya kullanıcı hesaplarını eşitlemek için sağlama hizmeti yapılandırmak için Azure portalını kullanabilirsiniz.

Her zaman, uygulamanız için sağlamayı ayarlama ayarlamak için özel kurulum öğretici bularak başlamanız gerekir. Ardından hem uygulama hem de sağlama bağlantısı oluşturmak için Azure AD'de yapılandırmak için bu adımları izleyin. Uygulama öğreticilerin bir listesi şu yolda bulunabilir: [nasıl Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-to-see-if-provisioning-is-working"></a>Sağlama çalışıp çalışmadığını nasıl 

Hizmet yapılandırıldıktan sonra hizmetin işlemi çoğu Öngörüler iki yerden kurulabilir:

-   **Denetim günlükleri** – sağlama denetim kaydı atanan sağlama kapsamında olan kullanıcılar Azure AD için sorgulama dahil olmak üzere sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri günlüğe kaydeder. Hedef uygulama için kullanıcı nesneleri arasında sistem karşılaştırma kullanıcılarla varlığını sorgulayın. Ardından ekleme, güncelleştirme veya hedef sistem üzerinde karşılaştırma tabanlı kullanıcı hesabı devre dışı bırak. Sağlama denetim günlüklerinin Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlükleri** sekmesi. Günlükleri filtreleyin **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori.

-   **Sağlama durumu –** son sağlama özeti çalıştırmak için belirli bir uygulamanın görülebilir **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt;Sağlama** kısmına ekranın altında hizmet ayarları. Bu bölümde, kaç kullanıcılara (ve/veya gruplar) şu anda iki sistem arasında eşitlendiğini özetler ve herhangi bir hata varsa. Hata ayrıntıları denetim günlüklerinde olabilir. Azure AD arasında bir tam ilk eşitleme tamamlanana kadar sağlama durumu doldurulmuyor gerektiğini unutmayın ve uygulama.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Dikkate alınması gereken sağlama ile genel sorunlu alanlar

Nereden başlayacağınızı hakkında bir fikir varsa, ayrıntılarına ulaşabilirsiniz genel sorunlu alanları listesi aşağıda verilmiştir.

* [Sağlama hizmeti başlatmak için görünmüyor.](#provisioning-service-does-not-appear-to-start)
* Yapılandırma çalışmıyor uygulama kimlik bilgileri nedeniyle kaydedilemiyor.
* [Atanmış oldukları olsa bile kullanıcılar "atlandı" ve sağlanmadı, Denetim günlükleri söyleyin](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatmak için görünmüyor.

Ayarlarsanız **sağlama durumu** olmasını **üzerinde** içinde **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt;Sağlama** Azure portal'ın bölümü. Ancak sonraki yeniden yükler sonra diğer bir durum ayrıntıları bu sayfada gösterilir. Hizmet çalışıyor ancak bir ilk eşitleme henüz tamamlanmadı olma olasılığı yüksektir. Denetleme **denetim günlükleri** şu hizmet gerçekleştiriyor, hangi işlemleri belirlemek için yukarıda açıklanan ve herhangi bir hata varsa.

>[!NOTE]
>Bir ilk eşitleme 20 dakika veya herhangi bir Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat sürebilir. İlk eşitleme sonrasında sonraki eşitlemeler sağlama hizmeti sonraki eşitlemeler performansını iyileştirme ilk eşitlemeden sonra her iki sistem durumunu temsil eden filigranlar depolar daha hızlı olması.
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a>Yapılandırma çalışmıyor uygulama kimlik bilgileri nedeniyle kaydedilemiyor.

Çalışmak için sağlama sırada Azure AD'ye bağlanmak için kullanıcı yönetimi, uygulama tarafından sağlanan API izin geçerli kimlik bilgilerini gerektirir. Bu kimlik bilgileri çalışmıyor veya bunların ne olduğunu bilmiyorsanız, daha önce açıklanan bu uygulamayı hazırlama için öğreticiyi gözden geçirin.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Denetim günlükleri kullanıcılar atlandı ve atanmış oldukları olsa bile sağlanmadı varsayalım.

Kullanıcı "Denetim günlüklerinde atlandı gibi" gösterilir, günlük iletisinde nedenini belirlemek için genişletilmiş ayrıntılarını okumak çok önemlidir. Sık karşılaşılan nedenleri ve çözümleri aşağıda verilmiştir:

- **Kapsam belirleme filtresi yapılandırılmış** **, filtre uygulayarak kullanıcının bir öznitelik değerine göre**. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

- **Kullanıcı, "etkili bir şekilde yetkili".** Bu belirli hata iletisini görürseniz, Azure AD'de depolanan kullanıcı atama kaydı ile ilgili bir sorun olduğundan olur. Bu sorun, powerbı.com'u kullanıcı (veya grup) uygulamadan düzeltin ve yeniden yeniden atamak için. Atama hakkında daha fazla bilgi için bkz. <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

- **Gerekli bir özniteliği eksik veya doldurulmuş bir kullanıcı değil.** Gözden geçirin ve hangi kullanıcı (veya grup) özellikleri akış Azure ad uygulama tanımlayan iş akışları ve öznitelik eşlemelerini yapılandırma sağlamayı ayarlama ayarlanırken dikkate alınması gereken önemli bir şey olabilir. Bu içerir "eşleştirme özelliği" ayarı benzersiz biçimde tanımlayan ve kullanıcılar/gruplar iki sistem arasındaki eşleştirmek için kullanılır. Bu önemli işlem hakkında daha fazla bilgi için bkz. <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

  * **Öznitelik eşlemeleri için grupları:** Grup adını ve üyelerinin yanı sıra grubu ayrıntıları bazı uygulamalar için destekleniyorsa sağlama. Etkinleştirme veya etkinleştirme veya devre dışı bırakarak bu işlevi devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. Grupları sağlama etkinse, "Eşleşen kimliği" için uygun bir alanı kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Bu olabilir görünen ad veya e-posta diğer adı), eşleşen özellik boş ya da doldurulmuş bir grup için Azure AD'de ise grup ve üyelerini sağlanacak değil olarak.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)
