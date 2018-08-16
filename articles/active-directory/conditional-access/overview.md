---
title: Azure Active Directory'de koşullu erişim nedir? | Microsoft Docs
description: Azure Active Directory'deki koşullu erişimin yalnızca kaynağa erişmeye çalışan kişiyi değil aynı zamanda kaynağa erişim şeklini temel alan otomatik erişim kararlarını uygulamaya almanıza nasıl yardımcı olabileceğini öğrenin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b191e041d219ad629c2f3ac6a0ac689551187eca
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630217"
---
# <a name="what-is-conditional-access-in-azure-active-directory"></a>Azure Active Directory'de koşullu erişim nedir?

Bulutu kullanan kuruluşlar için en önemli konu güvenliktir. Bulut kaynaklarınızı yönetmek söz konusu olduğunda, bulut güvenliğinin temel etmenlerinden biri güvenlik ve kimliktir. Mobil ve bulut öncelikli bir dünyada kullanıcılar, kuruluşunuzun kaynaklarına erişmek için farklı konumlardan farklı cihazlar ve uygulamalar kullanabilir. Bunun sonucunda artık yalnızca bir kaynağa erişim sağlayabilecek kullanıcılara odaklanmak yeterli değildir. Güvenlik ve üretkenlik arasındaki dengeyi kurmak için, erişim denetimi kararı sırasında bir kaynağa erişim şeklini de dikkate almanız gerekir. Azure AD'nin koşullu erişim özelliği sayesinde bu gereksinimi karşılayabilirsiniz. Koşullu erişim, Azure Active Directory tarafından sunulan bir özelliktir. Koşullu erişim sayesinde, bulut uygulamalarınıza erişim sağlamak için koşullara bağlı otomatik erişim denetimi kararları uygulayabilirsiniz. 

![Denetim](./media/overview/81.png)

Bu makalede Azure AD'deki koşullu erişim özelliklerine kavramsal genel bakış sağlanır.



## <a name="common-scenarios"></a>Genel senaryolar

Mobil ve bulut öncelikli bir dünyada Azure Active Directory cihazlarda, uygulamalarda ve hizmetlerde tüm konumlardan çoklu oturum açma özelliğinin kullanılmasını sağlar. Cihaz kullanımı (KCG dahil), kuruluş ağı dışında çalışma ve üçüncü taraf SaaS uygulamaları arttıkça iki hedefle karşılaşırsınız:

- Kullanıcıların her yerde ve her zaman üretken olmasını sağlama
- Kuruluş varlıklarını her zaman koruma altında tutma

Koşullu erişim ilkelerini kullanarak gerekli koşullarda doğru erişim denetimlerini uygulayabilirsiniz. Azure AD koşullu erişimi, gerekli olduğunda ek güvenlik katmanı sağlar ve gerekli olmadığında kullanıcınızın işini zorlaştırmaz. 

Erişim konusunda sıkça akla takılan ve koşullu erişimin yardımcı olabileceği konulardan bazıları şunlardır:



- **[Oturum açma riski](conditions.md#sign-in-risk)**: Azure AD Identity Protection, oturum açma risklerini algılar. Oturum açma riskinin kötü niyetli bir kullanıcıyı işaret ettiği durumda erişimi nasıl sınırlarsınız? Oturumun açma işleminin doğru kullanıcı tarafından gerçekleştirilmiş olduğuna dair daha kuvvetli bir kanıta ihtiyaç duyarsanız ne olur? Şüpheleriniz, belirli kullanıcıların bir uygulamaya erişimini engelleyecek kadar kuvvetliyse ne yapmalısınız? Yapılandırma ile 

- **[Ağ konumu](location-condition.md)**: Azure AD'ye her yerden erişim sağlanabilir. BT departmanınızın denetimi altında olmayan bir ağ konumundan erişim girişimi gerçekleştirilirse ne olur? Kullanıcı adı ve parola birleşimi, kuruluş ağınızın dışından kaynak erişimi girişimi için yeterli bir kimlik kanıtı olabilir. Peki dünya üzerindeki diğer beklenmeyen ülkelerden veya bölgelerden başlatılan erişim girişimleri için daha kuvvetli bir kimlik kanıtına ihtiyaç duyuyorsanız? Belirli konumlardan erişim girişimlerini tamamen engellemek isterseniz ne yapmalısınız?  

- **[Cihaz yönetimi](conditions.md#device-platforms)**: Azure AD'de kullanıcılar bulut uygulamalarına mobil ve kişisel cihazlar dahil olmak üzere çok çeşitli cihazlardan erişebilir. Erişim girişimlerinin yalnızca BT departmanınız tarafından yönetilen cihazlardan gerçekleştirilmesini isterseniz? Peki ya belirli cihaz türlerinin ortamınızdaki bulut uygulamalarına erişmesini engellemek isterseniz? 

- **[İstemci uygulaması](conditions.md#client-apps)**: Günümüzde web tabanlı uygulamalar, mobil uygulamalar veya masaüstü uygulamaları gibi farklı uygulama türü kullanan birçok bulut uygulamasına erişmek mümkün. Peki bilinen sorunlara yol açan türde bir istemci uygulaması kullanılarak erişim girişimi gerçekleştirilirse ne olur? Belirli uygulama türleri için BT departmanınız tarafından yönetilen bir cihazı şart koşmak istiyorsanız ne yapmalısınız? 

Bu sorular ve yanıtları, Azure AD koşullu erişim özelliğinin yaygın erişim senaryolarını oluşturmaktadır. Azure Active Directory'nin koşullu erişim özelliği, senaryoları ilke tabanlı bir yaklaşımla ele almanızı sağlar.


## <a name="conditional-access-policies"></a>Koşullu erişim ilkeleri

Koşullu erişim ilkesi, aşağıdaki modeli kullanan bir erişim senaryosunun tanımıdır:

![Denetim](./media/overview/10.png)

**Ardından bunu yap**, ilkenizin yanıtını tanımlar. Koşullu erişim ilkesinin amacının bir bulut uygulamasına erişim sağlamak olmadığını anlamanız önemlidir. Azure AD'de bulut uygulamalarına erişim izni verme işlemi, kullanıcı atamaları ile gerçekleştirilir. Koşullu erişim ilkesiyle yetkili kullanıcıların (bulut uygulamasına erişim izni verilmiş olan kullanıcıların) bulut uygulamalarına belirli koşullarda nasıl erişebileceğini denetlersiniz. Yanıt olarak çok faktörlü kimlik doğrulaması, yönetilen cihaz ve diğerleri gibi ek gereksinimlerin uygulanmasını sağlayabilirsiniz. Azure AD koşullu erişim bağlamında ilkenizin uyguladığı gereksinimler, erişim denetimleri olarak adlandırılır. İlkeniz, en kısıtlayıcı düzeyde erişimi engelleyebilir. Daha fazla bilgi için bkz. [Azure Active Directory koşullu erişim özelliğindeki erişim denetimleri](controls.md).
     

**Bu ortaya çıktığında**, ilkenizin tetiklenme nedenini tanımlar. Neden, karşılanan bir koşul grubu ile belirlenir. Azure AD koşullu erişim özelliğinde iki atama koşulu özel bir role sahiptir:

- **[Kullanıcılar](conditions.md#users-and-groups)**: Erişim girişiminde bulunan kullanıcılar (**Kim**). 

- **[Bulut uygulamaları](conditions.md#cloud-apps)**: Erişim girişiminin hedefi (**Ne**).    

Bu iki koşulun, koşullu erişim ilkesinde kullanılması zorunludur. Bu iki zorunlu koşula ek olarak, erişim girişiminin gerçekleştirilme şeklini açıklayan ek koşullar da kullanabilirsiniz. Mobil cihazların veya kuruluş ağınızın dışındaki konumların kullanılması, yaygın örnekler arasında sayılabilir. Daha fazla bilgi için bkz. [Azure Active Directory koşullu erişim özelliğindeki koşullar](conditions.md).   

Koşullar ile erişim denetimleriniz bir araya gelerek koşullu erişim ilkelerini oluşturur. 

![Denetim](./media/overview/51.png)

Azure AD koşullu erişim özelliğini kullanarak yetkili kullanıcıların bulut uygulamalarınıza nasıl erişeceğini denetleyebilirsiniz. Koşullu erişim ilkesinin amacı, erişim denetimlerini gerçekleştirilme şekline göre bir bulut uygulaması erişim girişimine uygulamaktır.

Bulut uygulamalarınıza erişimi korumak için ilke tabanlı yaklaşım kullanmanın avantajlarından biri, ortamınız için ilke gereksinimlerini oluşturmaya başlarken işin teknik boyutu konusunda bu makalede ana hatları belirlenen yapıyı kullanabilecek olmanızdır. 


## <a name="license-requirements-for-using-conditional-access"></a>Koşullu erişim kullanımı için lisans gereksinimleri

Koşullu erişimi kullanabilmek için Azure AD Premium lisansına ihtiyacınız vardır. Gereksinimlerinize uygun lisansı bulmak için bkz. [Ücretsiz, Temel ve Premium sürümlerinin genel olarak sağlanan özelliklerini karşılaştırma](https://azure.microsoft.com/pricing/details/active-directory/).


## <a name="next-steps"></a>Sonraki adımlar

- Aşağıdaki konularda daha fazla bilgi edinmek için ilgili sayfaları inceleyin:
    - Koşullar hakkında daha fazla bilgi için bkz. [Azure Active Directory koşullu erişim özelliğindeki koşullar](conditions.md).

    - Erişim denetimleri hakkında daha fazla bilgi için bkz. [Azure Active Directory koşullu erişim özelliğindeki erişim denetimleri](controls.md).

- Koşullu erişim ilkelerini yapılandırma konusunda deneyim edinmek istiyorsanız bkz. [Azure Active Directory koşullu erişim özelliğiyle belirli uygulamalar için MFA isteme](app-based-mfa.md).

- Ortamınızda koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz. [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 

- Önerilen ilkelere sahip ayrıntılı bir dağıtım planına ihtiyacınız varsa bkz. [koşullu erişim dağıtım planı](http://aka.ms/conditionalaccessdeploymentplan)
