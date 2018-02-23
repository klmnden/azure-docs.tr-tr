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
ms.date: 02/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 16f9179b6cbaee00a2afbe2efe090cb3eb8b204a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Azure Active Directory'de koşullu erişim için en iyi yöntemler

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bu makalede hakkında bilgi sağlar:

- Bilmeniz gerekenler 
- Nedir koşullu erişim ilkeleri yapılandırırken yapılması kaçınmalısınız. 

Bu makale size tanıdık kavramları ve terminolojiyi özetlenen olduğunu varsayar [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>Hangi iş bir ilke yapmak için gerekli?

Yeni bir ilke oluşturduğunuzda, hiçbir kullanıcıları, grupları, uygulamalar veya seçili erişim denetimleri vardır.

![Bulut uygulamaları](./media/active-directory-conditional-access-best-practices/02.png)


İlkenizle çalışmak için yapılandırmanız gerekir:


|Ne           | Nasıl                                  | Neden|
|:--            | :--                                  | :-- |
|**Bulut uygulamaları** |Bir veya daha fazla uygulama seçmeniz gerekir.  | Bir koşullu erişim ilkesi nasıl yetkili kullanıcıların denetim sağlamak için hedefidir bulut uygulamalarını erişebilirsiniz.|
| **Kullanıcılar ve gruplar** | En az bir kullanıcı veya seçili bulut uygulamalarınız erişim yetkisi grubu seçmeniz gerekir. | Hiçbir zaman yok kullanıcılar ve gruplar atanan bir koşullu erişim ilkesi tetiklenir. |
| **Erişim denetimleri** | En az bir erişim denetimi seçmeniz gerekir. | Koşullarınızı sağlanırsa, ilke işlemcinizin ne yapacağını bilmesi gerekir.|




## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="how-are-assignments-evaluated"></a>Atamaları nasıl değerlendirildiği?

Tüm atamaları mantıksal olarak olan **and işleciyle**. Yapılandırılmış birden fazla atama varsa, bir ilke tetiklemek için tüm atamaları sağlanmalıdır.  

Kuruluşunuzun ağ dışında yapılan tüm bağlantılar için geçerli bir konum koşulu yapılandırmak gerekirse:

- Dahil **tüm konumları**
- Dışlama **tüm güvenilen IP'leri**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Yapılacaklar dışında Azure AD yönetim portalında kilitliyse?

Koşullu Erişim İlkesi'nde yanlış bir ayar nedeniyle Azure AD portalı dışında kilitliyse:

- Henüz engellenmeyen diğer yöneticiler, kuruluşunuzda olup olmadığını doğrulayın. Azure portalına erişime sahip bir yönetici oturum açma etkileyen ilke devre dışı bırakabilirsiniz. 

- Kuruluşunuzdaki Yöneticiler hiçbiri ilkesini güncelleştirebilir, bir destek isteği göndermek gerekir. Microsoft destek gözden geçirin ve erişimi engelleme koşullu erişim ilkeleri güncelleştirin.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Klasik Azure portalı ve Azure portal yapılandırılmış ilkelerinde varsa ne olur?  

Her iki ilkeleri Azure Active Directory tarafından uygulanır ve yalnızca tüm gereksinimleri karşılandıktan sonra kullanıcı erişim alır.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Intune Silverlight portalı ve Azure portalı ilkelerinde varsa ne olur?

Her iki ilkeleri Azure Active Directory tarafından uygulanır ve yalnızca tüm gereksinimleri karşılandıktan sonra kullanıcı erişim alır.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Yapılandırılmış aynı kullanıcı için birden çok ilke varsa ne olur?  

Her oturum açma için Azure Active Directory tüm ilkeler değerlendirir ve kullanıcıya erişim izni verilsin önce tüm gereksinimlerin karşılandığını sağlar.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Koşullu erişim, Exchange ActiveSync ile çalışır mı?

Evet, Exchange ActiveSync bir koşullu erişim ilkesini kullanabilirsiniz.






## <a name="what-you-should-avoid-doing"></a>Bunu yaptığınızda kaçınmanız gerekir

Koşullu erişim framework harika yapılandırma esnekliği sağlar. Ancak, büyük esneklik de dikkatle her yapılandırma İlkesi, istenmeyen sonuçları oluşturmamak için serbest bırakmadan önce gözden geçirmeniz gereken olduğunu anlamına gelir. Bu bağlamda, özel tam kümeleri gibi etkileyen atamaları dikkat edilmesi **tüm kullanıcıları / grupları / bulut uygulamaları**.

Ortamınızda, aşağıdaki yapılandırmaları kaçınmanız gerekir:


**Tüm kullanıcılar için tüm bulut uygulamalarından:**

- **Erişimi engelleme** -bu yapılandırmayı kesinlikle iyi bir fikir değil, tüm kuruluş engeller.

- **Uyumlu aygıt gerektiren** - kullanıcılar için yok sahip kayıtlı cihazlarını henüz, bu ilke erişim Intune portalı dahil olmak üzere tüm erişimini engeller. Kayıtlı bir cihazın olmadan bir yöneticiyseniz Bu ilke ilkesini değiştirmek için Geri'yi Azure portalına bulaşmasından engeller.

- **Etki alanına katılmayı gerektirecek** - Bu ilke bloğu erişimi de etki alanına katılmış bir cihaz henüz yoksa, kuruluşunuzdaki tüm kullanıcıların erişimini engellemek için olası.


**Tüm kullanıcılar, tüm bulut uygulamaları, tüm cihaz platformları için:**

- **Erişimi engelleme** -bu yapılandırmayı kesinlikle iyi bir fikir değil, tüm kuruluş engeller.


## <a name="how-should-you-deploy-a-new-policy"></a>Nasıl yeni bir ilke dağıtmanız gerekir?

İlk adım olarak, ilke kullanarak değerlendirmelidir [ne aracı](active-directory-conditional-access-whatif.md).

Yeni bir ilke ortamınıza dağıtmaya hazır olduğunuzda bu aşamada yapmanız gerekir:

1. Küçük bir kullanıcı kümesi için bir ilke uygulanır ve beklendiği gibi davranır doğrulayın. 

2.  Daha fazla kullanıcı eklemek için bir ilke genişlettiğinizde, tüm yöneticiler ilkenin dışında tutmak için devam edin. Bu Yöneticiler hala erişiminiz ve bir değişiklik olursa bir ilke güncelleştirebilirsiniz sağlar.

3. Yalnızca bu gerçekten gerekliyse bir ilke tüm kullanıcılara uygulanır. 

En iyi uygulama, olan bir kullanıcı hesabı oluşturun:

- İlke yönetimi için ayrılmış 
- Tüm ilkelerinizi dışlanan


## <a name="policy-migration"></a>İlke geçişi

Nedeniyle Azure portalında oluşturmadınız ilkeleri geçirmeyi düşünün:

- Şimdi önce işleyemedi senaryolarını ele alabilir.

- Bunları birleştirerek yönetmek zorunda ilkeleri sayısını azaltabilirsiniz.   

- Tek bir merkezi konumda tüm koşullu erişim ilkelerini yönetebilirsiniz.

- Klasik Azure portalı kullanımdan kaldırılmıştır.   


Daha fazla bilgi için bkz: [Azure portalında Klasik ilkelerine](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).
