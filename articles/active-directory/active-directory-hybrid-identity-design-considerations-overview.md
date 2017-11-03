---
title: "Azure Active Directory karma kimlik tasarımı hakkında dikkat edilecek noktalar - genel bakış | Microsoft Docs"
description: "Genel bakış ve içerik haritasını karma kimlik tasarımı hakkında dikkat edilecek noktalar Kılavuzu"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e2a70f2474298618dd8ee11c583f8f445d7eba7d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory Karma Kimlik Tasarımı ile İlgili Dikkat Edilmesi Gerekenler
Tüketici tabanlı cihazlar şirket world proliferating ve bulut tabanlı hizmet olarak yazılım (SaaS) uygulamaları benimsemeye kolaydır. Sonuç olarak, bir iç veri merkezleri ile bulut platformlarda kullanıcıların uygulama erişim denetimini korumada görevdir.  

Microsoft'un kimlik çözümleri şirket içi ve bulut tabanlı özellikleri, kimlik doğrulama ve yetkilendirme konum bağımsız olarak tüm kaynaklar için tek bir kullanıcı kimliğini oluşturma span. Bu karma kimlik diyoruz. Yapılandırma seçenekleri için Microsoft Çözüm kullanarak karma kimlik ve hangi birleşimin en iyi belirlemek zor olabilir bazı durumda, kuruluşunuzun gereksinimlerini karşılamak ve farklı tasarım vardır. 

Bu karma kimlik tasarımı hakkında dikkat edilecek noktalar Kılavuzu, nasıl bir karma kimlik çözümü tasarlamak için en iyi iş uyan ve teknoloji, kuruluşunuzun gereksinimlerini anlamanıza yardımcı olur.  Bu kılavuzda, bir dizi adım ve kuruluşunuzun özel gereksinimleri karşılayan bir karma kimlik çözümü tasarlamanıza yardımcı olması için izleyebileceğiniz ayrıntıları verilmektedir. Adımlar ve görevler boyunca kılavuzda düzeyi gereksinimlerini ilgili teknolojiler ve özellik seçenekleri kuruluşların kullanabileceği karşılamak işlev ve hizmet kalitesi (kullanılabilirlik, ölçeklenebilirlik, performans, yönetilebilirlik ve güvenlik gibi) sunar. 

Özellikle, aşağıdaki soruları yanıtlamak için karma kimlik tasarımı hakkında dikkat edilecek noktalar Kılavuzu hedefleri şunlardır: 

* İsteyin ve en iyi my gereksinimlerini karşıladığını bir karma kimlik özgü tasarım teknoloji veya sorun alanı için sürücü yanıtlamak hangi soruları gerekiyor?
* Teknoloji veya sorun alanı için bir karma kimlik çözümü tasarlamak için hangi etkinlikler dizisini tamamlamanız gerekir? 
* Bana gereksinimlerimi yardımcı olmak hangi karma kimlik teknolojisi ve yapılandırma seçenekleri kullanılabilir? İşimi için en iyi seçenek seçebilmeniz dengelemeler Bu seçenekler arasında nelerdir?

## <a name="who-is-this-guide-intended-for"></a>Bu kılavuz için Kime yöneliktir?
 CIO, CITO, baş kimlik mimarları, Kurumsal mimarlar ve orta veya büyük ölçekli kuruluşlarda karma kimlik çözümü tasarlamaktan sorumlu BT mimarları.

## <a name="how-can-this-guide-help-you"></a>Bu kılavuz size nasıl yardımcı olabilir?
Geçerli şirket içi kimlik çözümünüzle birlikte bir bulut tabanlı kimlik yönetimi sistemi tümleştirebilir bir karma kimlik çözümünün nasıl tasarlanacağını anlamak için bu kılavuzu kullanın. 

Microsoft Azure kullanıcıların Bulut ve şirket içi bulunan uygulamalar boyunca çoklu oturum açma (SSO) kullanmalarını sağlamak için Active Directory ile şirket içi BT yöneticileri, kendi geçerli Windows Server Active Directory çözümünü tümleştirmek yönetmenizi sağlayan bir karma kimlik çözümü örnek bulunan aşağıdaki grafik gösterir.

![](./media/hybrid-id-design-considerations/hybridID-example.png)

Yukarıdaki çizimde, son kullanıcı kimlik doğrulama işlemi için tek bir deneyim sağlamak amacıyla ve kolaylaştırmak için şirket içi özelliklerle tümleştirilen bulut Hizmetleri yararlanan bir karma kimlik çözümü örneğidir BT bu kaynakları yönetme. Bu çok yaygın bir senaryo olsa da, her kuruluşun karma kimlik tasarımı farklı gereksinimler nedeniyle Şekil 1'de gösterilen örnekte farklı olması olasıdır. 

Bu kılavuz, bir dizi adım ve kuruluşunuzun özel gereksinimleri karşılayan bir karma kimlik çözümü tasarlamak için izleyebileceğiniz sağlar. Aşağıdaki adımlar ve görevler boyunca Kılavuzu size işlevsel ve kuruluşunuz için hizmet kalite düzeyi gereksinimlerini karşılamak için sağlanan ilgili teknolojileri ve özellik seçenekleri sunar.

**Varsayımlar**: Windows Server, Active Directory etki alanı Hizmetleri ve Azure Active Directory ile biraz deneyim sahibi. Bu belgede, bu çözümlerin kendi başına veya tümleşik bir çözüm içinde iş gereksinimlerinizi nasıl karşılayabileceğini görmek istediğinizi varsayarız.

## <a name="design-considerations-overview"></a>Tasarım konularına genel bakış
Bu belge adımları ve gereksinimlerinize en uygun karma kimlik çözümü tasarlamak için izleyebileceğiniz bir dizi sağlar. Adımlar sıralı halde verilmiştir. Sonraki adımlarda öğreneceğiniz tasarım konuları, önceki adımlarda nedeniyle çakışan tasarım seçimlerine ancak, verdiğiniz kararları değiştirmenizi gerektirebilir. Her girişimde, belge boyunca olası tasarım çakışmaları konusunda sizi uyarmak için. 

En iyi yalnızca tüm belge konuları kavramak gereken sayıda tekrar adımları uygulama işlemini, gereksinimlerinize uygun tasarım ulaşırsınız. 

| Karma kimlik aşaması | Konu listesi |
| --- | --- |
| Kimlik gereksinimleri belirleme |[İş gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Dizin eşitleme gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Çok faktörlü kimlik doğrulama gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Bir karma kimlik benimseme stratejinizi tanımlayın](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Güçlü kimlik çözümü ile veri güvenliği iyileştirmeyi planlama |[Veri koruma gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [İçerik Yönetimi gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Erişim denetimi gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Olay yanıtlama gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Veri koruma stratejisini tanımlayın](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Karma kimlik yaşam döngüsü planlaması |[Karma kimlik yönetimi görevleri belirleme](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Eşitleme Yönetimi](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Karma kimlik yönetimini benimseme stratejinizi belirleme](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="download-this-guide"></a>Bu Kılavuzu'nu indirin
Karma kimlik tasarımı hakkında dikkat edilecek noktalar Kılavuzu'ndaki bir pdf sürümünü indirebilirsiniz [Technet Galerisi](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

