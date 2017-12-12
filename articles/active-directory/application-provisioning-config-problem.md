---
title: "Azure AD galeri uygulamaya kullanıcı sağlamayı yapılandırma sorunu | Microsoft Docs"
description: "Azure AD uygulama galerisinde listelenen yapılandırma kullanıcı bir uygulama zaten sağlama genişlettiklerinde karşılaştığı yaygın sorunlar nasıl giderilir"
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
ms.openlocfilehash: 3a19169effad54e26cd2061bffae369cd31e9a9e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulamaya kullanıcı sağlamayı yapılandırma sorunu

Yapılandırma [otomatik kullanıcı sağlamayı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) bir uygulama için (desteklendiği yerlerde), özel yönergeler otomatik sağlama uygulama hazırlamak için izlenmesi gerekir. Ardından, uygulama için kullanıcı hesaplarını eşitlemek için sağlama hizmeti yapılandırmak için Azure portalını kullanabilirsiniz.

Her zaman Kurulum Öğreticisi, uygulamanız için sağlama yukarı ayarına belirli bularak başlamanız gerekir. Ardından uygulama ve sağlama bağlantı oluşturmak için Azure AD yapılandırmak için bu adımları izleyin. Uygulama öğreticilerin bir listesi bulunabilir [SaaS uygulamaları Azure Active Directory ile tümleştirme için nasıl öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-to-see-if-provisioning-is-working"></a>Sağlama çalışıp çalışmadığını nasıl 

Hizmet yapılandırıldıktan sonra hizmet işlemi çoğu Öngörüler iki yerde çizilebilir:

-   **Denetim günlükleri** – sağlama denetim günlüklerini kaydı için Azure AD Sorgulama dahil olmak üzere sağlama hizmeti tarafından gerçekleştirilen tüm işlemler atanan sağlama kapsamında olan kullanıcılar. Hedef uygulama sistem arasında kullanıcı nesneleri karşılaştırma kullanıcılarla varlığı için sorgu. Ardından ekleme, güncelleştirme veya karşılaştırma üzerine dayalı hedef sistem kullanıcı hesabı devre dışı bırakın. Sağlama denetim günlüklerini Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlüklerini** sekmesi. Günlükleri filtre **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori.

-   **Sağlama durumu –** son sağlama Özet çalıştırmak için belirli bir uygulamanın görülebilir **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; Sağlama** kısmına ekranın alt kısmındaki hizmet ayarları altında. Bu bölümde kaç kullanıcıların (ve/veya grupları) şu anda iki sistem arasında eşitlendiğini özetler ve herhangi bir hata varsa. Hata ayrıntıları denetim günlüklerini olabilir. Azure AD arasında bir tam ilk eşitleme işlemi tamamlanana kadar sağlama durumu doldurulması değil olduğunu unutmayın ve uygulama.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Dikkate alınması gereken sağlama ile genel sorunlu alanları

Başlatılacağı konum hakkında bir fikir varsa, ayrıntılarına geçebilir genel sorunlu alanları listesi aşağıdadır.

* [Başlatmak için hizmet sağlama görünmüyor](#provisioning-service-does-not-appear-to-start)
* [Uygulama kimlik bilgileri çalışmıyor nedeniyle yapılandırma kaydedilemiyor.](#can’t-save-configuration-due-to-app-credentials-not-working)
* [Atanmış olan olsa bile kullanıcılar "atlanacak" ve sağlanan değil, denetim günlüklerini söyleyin](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Başlatmak için hizmet sağlama görünmüyor

Ayarlarsanız **sağlama durumu** olmasını **üzerinde** içinde **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt;sağlama** Azure portalının bölümü. Ancak sonraki yeniden yükler sonra diğer bir durum ayrıntıları bu sayfada gösterilir. Hizmet çalışıyor ancak bir ilk eşitleme henüz tamamlanmadı olasıdır. Denetleme **denetim günlüklerini** hizmet gerçekleştirip, hangi işlemleri belirlemek için yukarıda açıklanan ve herhangi bir hata varsa.

>[!NOTE]
>Bir başlangıç eşitlemesi 20 dakika arasında herhangi bir yere Azure AD dizini ve sayısı, kullanıcı sağlama kapsamında boyutuna bağlı olarak birkaç saat sürebilir. İlk eşitleme sonrasında sonraki eşitlemeler daha hızlı olması sağlama hizmeti, sonraki eşitlemeler performansını iyileştirme ilk eşitleme sonrasında her iki sistem durumunu temsil filigranlar depolar.
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a>Uygulama kimlik bilgileri çalışmıyor nedeniyle yapılandırma kaydedilemiyor.

Çalışmak için sağlama sırayla Azure AD kullanıcı yönetimi, uygulama tarafından sağlanan API bağlanmasına izin geçerli kimlik bilgileri gerektirir. Bu kimlik bilgileri çalışmıyor ya da bunlar wat bilmiyorsanız, daha önce açıklanan bu uygulamasını kurup Öğreticisi gözden geçirin.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Denetim günlüklerini kullanıcılar atlanacak ve atanmış olan olsa bile sağlanan değil söyleyin

Bir kullanıcı "denetim günlüklerini atlandı"olarak görünür, genişletilmiş ayrıntıları nedenini belirlemek için günlük iletisi okumak çok önemlidir. Ortak nedenleri ve çözümleri aşağıda verilmiştir:

-   **Bir kapsam filtresi yapılandırılmış** **filtrelemesi kullanıcı bir öznitelik değerini temel**. Kapsam belirleme filtreleri ile ilgili daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **Kullanıcı "etkili bir şekilde alınarak" değil.** Bu belirli bir hata iletisi görürseniz, Azure AD'de depolanan kullanıcı atama kaydı ile ilgili bir sorun olduğundan değildir. Bu sorun, atamayı kullanıcı (veya grubu) uygulamadan düzeltin ve yeniden yeniden atamak için. Atama hakkında daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Gerekli bir öznitelik eksik veya bir kullanıcı için doldurulan değil.** Gözden geçirin ve hangi kullanıcı (veya grubu) özellikleri akışı Azure AD'den uygulamaya tanımlamak iş akışları ve öznitelik eşlemelerini yapılandırmak için sağlama yukarı ayarlanırken dikkate alınması gereken önemli bir şey olabilir. Bu içerir "eşleşen özellik" ayarı benzersiz olarak tanımlamak ve kullanıcıları/grupları iki sistem arasında eşleştirmek için kullanılabilir. Bu önemli işlemi hakkında daha fazla bilgi için bkz: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

   * **Öznitelik grupları için eşlemeleri:** grup adı ve bazı uygulamalar için destekleniyorsa üyelerin yanı sıra Grup ayrıntılarını sağlama. Etkinleştirmek veya bu işlevi etkinleştirmek veya devre dışı bırakarak devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. Grupları sağlama etkinse, uygun bir alan "Eşleşen kimliği" kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Bu olabilir görünen ad veya e-posta diğer adı), boş veya bir grup için doldurulan eşleşen özellik Azure AD içinde ise, Grup ve üyelerini sağlanması değil olarak.

#<a name="next-steps"></a>Sonraki adımlar
[Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme](active-directory-saas-app-provisioning.md)
