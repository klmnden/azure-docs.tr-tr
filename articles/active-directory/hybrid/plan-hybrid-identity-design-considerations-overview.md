---
title: Azure Active Directory karma kimlik tasarım konuları - genel bakış | Microsoft Docs
description: Genel bakış ve içerik haritasını karma kimlik tasarım konuları Kılavuzu
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7f8dd49f3668b8f68753681123a04d21edac46c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60381488"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory Karma Kimlik Tasarımı ile İlgili Dikkat Edilmesi Gerekenler
Tüketici tabanlı cihazların Kurumsal dünyasında proliferating ve bulut tabanlı hizmet olarak yazılım (SaaS) uygulamaları benimsemek kolaydır. Sonuç olarak, kullanıcıların uygulama erişimi denetimini iç veri merkezlerinden ve bulut platformları arasında koruma zorludur.  

Microsoft’un kimlik çözümleri şirket içi ve bulut tabanlı çözümleri birleştirerek konumdan bağımsız olarak tüm kaynaklara kimlik doğrulaması ve yetkilendirme sağlamak üzere tek bir kullanıcı kimliği oluşturur. Bu kavram, karma kimlik bilinir. Bazı durumda, hangi birleşimin en iyi saptamak zor olabilir ve Microsoft çözümleri kullanarak karma kimlik için yapılandırma seçenekleri, kuruluşunuzun gereksinimlerini karşılamak ve farklı tasarım vardır. 

Bu karma kimlik tasarım konuları Kılavuzu nasıl karma kimlik çözümü tasarlamak için iş gereksinimlerine ve teknoloji, kuruluşunuzun gereksinimlerini anlamanıza yardımcı olur.  Bu kılavuzda, bir dizi adım ve kuruluşunuzun benzersiz gereksinimlerini karşılayan bir karma kimlik çözümü tasarlamanıza yardımcı olması için izleyebileceğiniz ayrıntıları verilmektedir. Adımlar ve görevler boyunca Kılavuzu ilgili teknolojileri sunmak ve işlevsel ve hizmet kalitesi (kullanılabilirlik, ölçeklenebilirlik, performans, yönetilebilirlik ve güvenlik gibi) karşılamak için kuruluşların kullanabileceği seçenekleri özellik düzeyi gereksinimleri. 

Özellikle, aşağıdaki soruları yanıtlamak için karma kimlik tasarım konuları Kılavuzu amaçları şunlardır: 

* Hangi soruları isteyin ve en iyi gereksinimlerime bir karma kimlik özel tasarım bir teknoloji veya sorun alanı için yanıtlamam gerekiyor?
* Teknoloji veya sorun alanı için bir karma kimlik çözümü tasarlamak için hangi etkinlikler dizisini tamamlamanız gerekir? 
* Karşılayacak yardımcı olmak hangi karma kimlik teknolojisi ve yapılandırma seçenekleri mevcuttur? My iş için en iyi seçenek seçebilirsiniz gereksinimlerimi karşılamama Bu seçenekler nelerdir?

## <a name="who-is-this-guide-intended-for"></a>Bu kılavuz kimin için tasarlanmıştır?
 CIO, CITO, baş kimlik mimarları, Kurumsal mimarlar ve orta ölçekli veya büyük kuruluşlarda bir karma kimlik çözümü tasarlamaktan sorumlu olan BT mimarları.

## <a name="how-can-this-guide-help-you"></a>Bu kılavuz size nasıl yardımcı olabilir?
Bir bulut tabanlı kimlik yönetimi sistemi geçerli şirket içi kimlik çözümünüzle tümleştirerek bir karma kimlik çözümünün nasıl tasarlanacağını anlamak için bu kılavuzu kullanabilirsiniz. 

Aşağıdaki grafikte, BT yöneticileri, kullanıcıların geçerli Windows Server Active Directory bulunan şirket içi çözüm Microsoft Azure Active kullanın (çoklu oturum açma olanağı dizini ile tümleştirme yönetmenizi sağlayan bir karma kimlik çözümü bir örnek gösterilmektedir. SSO) Bulut ve şirket içinde bulunan uygulamalar arasında.

![Örnek](media/plan-hybrid-identity-design-considerations/hybridID-example.png)

Yukarıdaki çizimde, son kullanıcı kimlik doğrulama işlemi için tek bir deneyim sunmak için ve kolaylaştırmak için şirket içi özelliklerle tümleştirilen bulut Hizmetleri yararlanan karma kimlik çözümü örneğidir BT bu yönetme kaynaklar. Bu örnekte yaygın bir senaryo olsa da, her kuruluşun karma kimlik tasarımı farklı gereksinimler nedeniyle Şekil 1'de gösterilen örnekten farklı olabilir. 

Bu kılavuz, bir dizi adım ve kuruluşunuzun benzersiz gereksinimlerini karşılayan bir karma kimlik çözümü tasarlamak için izleyebileceğiniz sağlar. Aşağıdaki adımlar ve görevler boyunca kılavuzda, işlevsel ve kuruluşunuz için hizmet kalite düzeyi gereksinimlerini karşılamak için ilgili teknolojiler ve özellik seçenekleri sunulmaktadır.

**Varsayımlar**: Windows Server, Active Directory Domain Services ve Azure Active Directory ile ilgili biraz deneyim var. Bu belgede, bu çözümleri kendi başına veya tümleşik bir çözüm içinde iş ihtiyaçlarınızı nasıl karşılayabileceğini aradığınız varsayılır.

## <a name="design-considerations-overview"></a>Tasarım konularına genel bakış
Bu belge, bir dizi adım ve gereksinimlerinize en uygun karma kimlik çözümü tasarlamak için izleyebileceğiniz sağlar. Adımlar sıralı halde verilmiştir. Sonraki adımlarda öğreneceğiniz tasarım konuları, önceki adımlarda, Bununla birlikte, çakışan tasarım seçimlerine nedeniyle kararları değiştirmenizi gerektirebilir. Her girişimde, belge boyunca olası tasarım çakışmaları konusunda sizi uyarmak için. 

Tüm belge konuları için gereken sayıda adım yalnızca yineleme sonra en iyi gereksinimlerinizi karşılayan tasarıma ulaşırsınız. 

| Karma kimlik aşaması | Konu listesi |
| --- | --- |
| Kimlik gereksinimlerini belirleme |[İş gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-business-needs.md)<br> [Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Karma kimlik benimseme stratejinizi tanımlayın](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Aracılığıyla güçlü kimlik çözümü veri güvenliği iyileştirmeyi planlama |[Veri koruma gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [İçerik Yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Erişim denetimi gereksinimleri belirleme](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Veri koruma stratejisini tanımlayın](plan-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Hibrit kimlik yaşam döngüsünü planlama |[Karma kimlik yönetimi görevleri belirleme](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Eşitleme Yönetimi](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Karma kimlik yönetimini benimseme stratejinizi belirleme](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="next-steps"></a>Sonraki Adımlar
[Kimlik gereksinimleri belirleme](plan-hybrid-identity-design-considerations-business-needs.md)

