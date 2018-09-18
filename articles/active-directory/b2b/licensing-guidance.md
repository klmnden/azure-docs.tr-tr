---
title: Azure Active Directory B2B işbirliği lisanslama Kılavuzu | Microsoft Docs
description: Azure AD lisansı işbirliği gerektirmez Azure Active Directory B2B Ücretli, ancak, ayrıca özellikleri B2B Konuk kullanıcılar için ödeme
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 08/09/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 1d92f68bbb5e8c001594e4f78f90cb10496aaf29
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984501"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Azure Active Directory B2B işbirliği lisanslama kılavuzu

Azure AD Hizmetleri ve kuruluşunuzdaki diğer kaynaklara erişmesine izin vermek için Azure AD kiracısına Konuk kullanıcıları davet etmek için Azure AD B2B işbirliği özelliklerini kullanabilirsiniz. B2B işbirliği Konuk kullanıcıları ile lisanslanması gerekir, erişim sağlamak istiyorsanız, ücretli Azure AD özellikleri, Azure AD lisansı uygun. 

Bu avantajlar şunlardır:
* Azure AD ücretsiz özellikleri ek lisans olmadan Konuk kullanıcılar için kullanılabilir.
* B2B kullanıcıları için sunulan Ücretli Azure AD özellikleri erişim sağlamak istiyorsanız, bu B2B Konuk kullanıcıları desteklemek için yeterli sayıda lisans olmalıdır.
* B2B işbirliği kullanım hakları kiracısına davet bir ek beş B2B Konuk kullanıcı davet eden bir lisans Ücretli bir Azure AD kiracısıyla sahiptir.
* Kullanıcıların kaç B2B işbirliği Ücretli Azure AD özellikleri belirlemek için bir davet eden kiracının sahibi müşteri olmalıdır. Ücretli Azure AD bağlı olarak, aynı 5:1 oranını B2B işbirliği kullanıcıları kapsayacak şekilde lisansları kadar Azure AD olmalıdır, Konuk kullanıcılar için istediğiniz özellikleri Ücretli.

B2B işbirliği Konuk kullanıcı, bir kullanıcı bir iş ortağı şirketi, değil, kuruluşunuzun bir çalışan veya yan kuruluşu çalışanı, conglomerate içinde farklı bir iş olarak eklenir. B2B Konuk kullanıcı, dış kimlik bilgilerini veya bu makalede anlatıldığı gibi kuruluşunuza ait kimlik bilgilerinizle oturum açabilirsiniz. 

Diğer bir deyişle, B2B lisansı kullanıcı kimliğini nasıl tarafından değil, ancak bunun yerine kuruluşunuz için kullanıcı arasındaki ilişkiyi tarafından ayarlanır. Bu kullanıcılar iş ortakları emin değilseniz, farklı olarak, lisanslama koşulları kabul edilir. B2B işbirliği kullanıcısı kendi UserType "Konuk" işaretlenmiş olsa bile amacıyla lisanslama olması olarak değerlendirilmez Bunlar genellikle, kullanıcı başına bir lisansı lisanslanmalıdır. Bu kullanıcılar şunları içerir:
* Çalışanlarınızın
* Dış kimlik kullanarak ekibi
* Bir çalışan, conglomerate farklı bir işletmenin


## <a name="licensing-examples"></a>Lisanslama örnekleri
- Bir müşteri, 100 kendi Azure AD kiracınıza B2B işbirliği kullanıcıları davet etmek istemektedir. Erişim denetimi ve sağlaması tüm kullanıcılar için müşteri atar ancak 50 kullanıcı MFA ve koşullu erişim de gerektirir. Bu müşteri 10 Azure AD temel lisansı ve doğru şekilde bu B2B kullanıcıları kapsayacak şekilde 10 Azure AD Premium P1 lisansı satın almanız gerekir. Müşteri, kimlik koruması ile B2B kullanıcıları özelliklerine planlıyorsa, davet edilen kullanıcılar aynı 5:1 oranını karşılamak için Azure AD Premium P2 lisansı olması gerekir.
- Bir müşteri şu anda tüm Azure AD Premium P1 ile lisanslanan 10 kişiden sahiptir. Şirket artık ihtiyaç duyan tüm çok faktörlü kimlik doğrulaması (MFA) 60 B2B kullanıcıları davet etmek istemektedir. 5:1 lisans kuralı altında müşteri 60 B2B işbirliği kullanıcıları kapsayacak şekilde en az 12 Azure AD Premium P1 lisansı olması gerekir. 10 Premium P1 lisansı 10 çalışanlar için sahip oldukları için bunlar MFA gibi Premium P1 özelliklerle 50 B2B kullanıcıları davet etme hakkına sahiptir. Bu örnekte, daha sonra kalan 10 B2B işbirliği kullanıcıları kapsayacak şekilde 2 ek Premium P1 lisansı satın almanız gerekir.

> [!NOTE]
> Henüz bu B2B işbirliği kullanıcı hakları etkinleştirmek için doğrudan B2B kullanıcılara lisans atamak için hiçbir yolu yoktur.

Kullanıcıların kaç B2B işbirliği Ücretli Azure AD özellikleri belirlemek için bir davet eden kiracının sahibi müşteri olmalıdır. Azure AD özellikleri için konuk kullanıcılarınızın ödediğiniz miktar bağlı olarak, 5:1 oranını B2B işbirliği kullanıcıları kapsayacak yeterli Azure AD Ücretli lisansı olması gerekir. 

## <a name="additional-licensing-details"></a>Ek lisans ayrıntıları
- Aslında B2B kullanıcı hesaplarına lisansları atamak için gerek yoktur. 5:1 oranını bağlı olarak, lisans otomatik olarak hesaplanan bildirilen ve.
- Ücretli Hayır ise, Azure AD lisansınızın kiracısında mevcut Azure AD ücretsiz sürüm sunan hakları her davet edilen kullanıcının alır.
- Kendi kuruluştan bir B2B işbirliği kullanıcısı zaten bir Ücretli Azure AD lisansı, davet eden kiracının B2B işbirliği lisanstan kullandıkları değil.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Tartışma Gelişmiş: ediyoruz "Apı'lerinizi kullanan üyelerin" olarak kümeleme bir kuruluşun kullanıcı eklerken lisans dikkat edilecek noktalar nelerdir?
B2B Konuk kullanıcı bir iş ortağı kuruluştan ana kuruluşla çalışmak üzere davet edildiği biridir. Genellikle, diğer bir olay B2B bile, kullanan B2B özellikleri uygun değil. İki durumlara özellikle göz atalım:

1. Bir tüketici adresi kullanarak bir çalışan bir konağa davet ederse
  * Bu senaryo, bizim lisans ilkeleri ile uyumlu değildir ve önerilmez.

2. Bir konak kuruluş başka bir kümeleme kuruluştan bir kullanıcı eklerse
  1. Bu durumda, B2B API'lerini kullanarak kullanıcı davet, ancak bu durumda geleneksel B2B değil. İdeal olarak, ki bu kuruluşlardaki diğer kuruluşlar (API'mizi sağlar) üye olarak davet olması gerekir. Bu durumda, davet eden kuruluşunda bulunan kaynaklara erişmek için bu üye için bunları atanan lisanslara sahip.

  2. Bazı kuruluşlar, bir ilke olarak "Konuk" eklenecek kullanıcıları diğer kuruluş 's eklemek isteyebilirsiniz. Burada iki durum vardır:
      * Kümeleme kuruluş zaten Azure AD'yi kullanarak ve davet edilen kullanıcılar diğer kuruluştaki lisanslanır: Bu durumda, biz bu belgenin önceki bölümlerinde düzenlendiği 1:5 formül izlemeniz gereken davet edilen kullanıcıların beklemiyoruz. 

      * Kümeleme kuruluş Azure AD'ye kullanmıyor veya uygun lisansları yok: Bu durumda, bu belgenin önceki bölümlerinde düzenlendiği 1:5 formül izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](faq.md)
