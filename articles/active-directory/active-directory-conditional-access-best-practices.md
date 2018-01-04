---
title: "Azure Active Directory'de koşullu erişim için en iyi uygulamaları | Microsoft Docs"
description: "Bilmeniz gerekenler ve koşullu erişim ilkeleri yapılandırırken bunun kaçınmalısınız nedir hakkında bilgi edinin."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 8c6707505a6331b53e06b1de60575dd3637ea477
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Azure Active Directory'de koşullu erişim için en iyi yöntemler

Bu konu, bilmeniz gerekenler ve koşullu erişim ilkeleri yapılandırırken bunun kaçınmalısınız nedir hakkında bilgi sağlar. Bu konu okumadan önce kavramları öğrenmeniz ve terminolojiyi özetlenen [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="whats-required-to-make-a-policy-work"></a>Hangi iş bir ilke yapmak için gerekli?

Yeni bir ilke oluşturduğunuzda, hiçbir kullanıcıları, grupları, uygulamalar veya seçili erişim denetimleri vardır.

![Bulut uygulamaları](./media/active-directory-conditional-access-best-practices/02.png)


İlkenizle çalışmak için aşağıdakileri yapılandırmanız gerekir:


|Ne           | Nasıl                                  | Neden|
|:--            | :--                                  | :-- |
|**Bulut uygulamaları** |Bir veya daha fazla uygulama seçmeniz gerekir.  | Bir koşullu erişim ilkesi ince ayar sağlamak için hedefidir nasıl yetkili kullanıcılar, uygulamalara erişebilir.|
| **Kullanıcılar ve gruplar** | En az bir kullanıcı veya seçtiğiniz bulut uygulamalarını erişim yetkisi grubu seçmeniz gerekir. | Hiçbir zaman yok kullanıcılar ve gruplar atanan bir koşullu erişim ilkesi tetiklenir. |
| **Erişim denetimleri** | En az bir erişim denetimi seçmeniz gerekir. | İlke işlemcinizin koşullarınızı sağlanırsa yapmanız gerekenler bilmek ister.|


Bu temel gereksinimlerine ek olarak, çoğu durumda, aynı zamanda bir koşul yapılandırmalısınız. Bir ilke yapılandırılmış bir koşulu işe yaramayacaktır olsa da, uygulamalarınıza ince ayar yapmak için yönlendirmeli faktörü koşullardır.


![Bulut uygulamaları](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Atamaları nasıl değerlendirildiği?

Tüm atamaları mantıksal olarak olan **and işleciyle**. Yapılandırılmış birden fazla atama varsa, bir ilke tetiklemek için tüm atamaları karşılanması gerekir.  

Kuruluşunuzun ağ dışında yapılan tüm bağlantılar için geçerli bir konum koşulu yapılandırmak gerekirse, bu görevleri gerçekleştirebilirsiniz:

- Dahil olmak üzere **tüm konumları**
- Hariç **tüm güvenilen IP'leri**

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Klasik Azure portalı ve Azure portal yapılandırılmış ilkelerinde varsa ne olur?  
Her iki ilkeleri Azure Active Directory tarafından uygulanır ve yalnızca tüm gereksinimleri karşılandıktan sonra kullanıcı erişim alır.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Intune Silverlight portalı ve Azure portalı ilkelerinde varsa ne olur?
Her iki ilkeleri Azure Active Directory tarafından uygulanır ve yalnızca tüm gereksinimleri karşılandıktan sonra kullanıcı erişim alır.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Yapılandırılmış aynı kullanıcı için birden çok ilke varsa ne olur?  
Her oturum açma için Azure Active Directory tüm ilkeler değerlendirir ve kullanıcıya erişim izni verilsin önce tüm gereksinimlerin karşılandığını sağlar.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Koşullu erişim, Exchange ActiveSync ile çalışır mı?

Evet, Exchange ActiveSync bir koşullu erişim ilkesini kullanabilirsiniz.


## <a name="what-you-should-avoid-doing"></a>Bunu yaptığınızda kaçınmanız gerekir

Koşullu erişim framework harika yapılandırma esnekliği sağlar. Ancak, büyük esneklik ayrıca her bir yapılandırma İlkesi, istenmeyen sonuçları oluşturmamak için serbest önce dikkatle gözden geçirmelidir anlamına gelir. Bu bağlamda, özel tam kümeleri gibi etkileyen atamaları dikkat edilmesi **tüm kullanıcıları / grupları / bulut uygulamaları**.

Ortamınızda, aşağıdaki yapılandırmaları kaçınmanız gerekir:


**Tüm kullanıcılar için tüm bulut uygulamalarından:**

- **Erişimi engelleme** -bu yapılandırmayı kesinlikle iyi bir fikir değil, tüm kuruluş engeller.

- **Uyumlu aygıt gerektiren** - kullanıcılar için yok sahip kayıtlı cihazlarını henüz, bu ilke erişim Intune portalı dahil olmak üzere tüm erişimini engeller. Kayıtlı bir cihazın olmadan bir yöneticiyseniz Bu ilke ilkesini değiştirmek için Geri'yi Azure portalına bulaşmasından engeller.

- **Etki alanına katılmayı gerektirecek** - Bu ilke bloğu erişimi de etki alanına katılmış bir cihaz henüz yoksa, kuruluşunuzdaki tüm kullanıcıların erişimini engellemek için olası.


**Tüm kullanıcılar, tüm bulut uygulamaları, tüm cihaz platformları için:**

- **Erişimi engelleme** -bu yapılandırmayı kesinlikle iyi bir fikir değil, tüm kuruluş engeller.



## <a name="policy-migration"></a>İlke geçişi

Nedeniyle Azure portalında oluşturmadınız ilkeleri geçirme dikkate almanız gerekir:

- Şimdi önce işleyemedi senaryolarını ele alabilir.

- Bunları birleştirerek yönetmek zorunda ilkeleri sayısını azaltabilirsiniz.   

- Tek bir merkezi konumda tüm koşullu erişim ilkelerini yönetebilirsiniz.

- Klasik Azure portalı kullanımdan kaldırılacaktır.   


Daha fazla bilgi için bkz: [Azure portalında Klasik ilkelerine](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).
