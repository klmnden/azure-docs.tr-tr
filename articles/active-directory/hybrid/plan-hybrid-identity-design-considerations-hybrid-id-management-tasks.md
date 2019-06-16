---
title: Karma kimlik tasarımı - yönetim görevlerini Azure | Microsoft Docs
description: Koşullu erişim denetimi ile Azure Active Directory kullanıcı kimlik doğrulaması yapılırken ve uygulamaya erişime izin vermeden önce çekme belirli koşullara bakar. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a829d39ff21a1abeafd3b4362747894d196d9d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109377"
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Karma kimlik yaşam döngüsü planlaması
Kimlik, Kurumsal mobilite ve uygulama erişim stratejinizin girişiminin temellerinden biridir. Mobil cihaz veya SaaS uygulaması için imzalama olsun, kimlik bilgilerinizi her şeyi erişmesini önemlidir. En yüksek düzeyde otomatik hale getirme ve kaynaklar sağlama işleminin merkezileştirerek içeren birleştirme ve kimlik depolarınızı arasında eşitleme bir kimlik yönetimi çözümü kapsar. Kimlik çözümü, şirket içi ve bulut arasında bir merkezi kimlik olabilir ve Kimlik Federasyonu çeşit merkezi kimlik doğrulamaya korumak ve güvenli bir şekilde paylaşın ve dış kullanıcıların ve işletmelerin işbirliği yapmak için de gerekir. İşletim sistemlerini ve uygulamaları kişilere arasında veya bir kuruluşla ilişkili kaynaklar. Kuruluş yapısı sağlama ilkelerini ve yordamlarını uyum sağlayacak şekilde değiştirilebilir.

Verimli olmasını sağlamak için Self Servis deneyimleri sunarak kullanıcılarınıza geliştirilmiş bir kimlik çözümü sağlamak önemlidir. Erişim duydukları tüm kaynakları kullanıcılar için çoklu oturum açmayı etkinleştirir, daha sağlam, kimlik çözümüdür. Tüm düzeylerde Yöneticiler, kullanıcı kimlik bilgilerini yönetmek için standartlaştırılmış yordamları kullanabilirsiniz. Bazı yönetim düzeyde azaltılmış veya ortadan kalkar çeşitliliğini sağlama yönetimi çözümü bağlı olarak. Ayrıca, güvenli bir şekilde yönetim yetenekleri el ile veya otomatik olarak, çeşitli kuruluş arasında dağıtabilirsiniz. Örneğin, bir etki alanı yöneticisi yalnızca kişileri ve bu etki alanındaki kaynaklara görebilir. Bu kullanıcı, yönetim ve sağlama görevlerini gerçekleştirebilir, ancak iş akışları oluşturma gibi yapılandırma görevleri gerçekleştirmek için yetkili değil.

## <a name="determine-hybrid-identity-management-tasks"></a>Karma kimlik yönetimi görevleri belirleme
Kuruluşunuzdaki yönetim görevleri dağıtma doğruluk ve yönetim verimliliğini artırır ve bir kuruluşun iş yükü dengesi artırır. Güçlü kimlik yönetimi sistemi tanımlayan özetleri aşağıda verilmiştir.

 ![kimlik yönetimi konuları](./media/plan-hybrid-identity-design-considerations/Identity_management_considerations.png)

Karma kimlik yönetimi görevleri tanımlamak için karma kimlik benimseyen bir kuruluşun temel özelliklerinden bazıları anlamanız gerekir. Kimlik kaynakları için kullanılan geçerli depoları anlamak önemlidir. Bu temel öğeleri bilerek tarafından temel gereksinimleri vardır ve daha iyi bir tasarım kararı kimlik çözümünüz için neden daha ayrıntılı soru gerekecektir dayanır.  

Bu gereksinimleri tanımlarken, en az emin aşağıdaki soruları yanıtlanır

* Sağlama seçenekleri: 
  
  * Karma kimlik çözümü, güçlü bir hesap erişim yönetimi ve sağlama sistem destekliyor mu?
  * Kullanıcılar, gruplar ve yönetilecek parolaları nasıl misiniz?
  * Kimlik yaşam döngüsü yönetimi, esnek mi? 
    * Parola güncelleştirmeleri hesabı askıya ne kadar sürer?
* Lisans Yönetimi: 
  
  * Karma kimlik çözümü tanıtıcıları Lisans Yönetimi mu?
    * Yanıt Evet ise, hangi özellikler sağlanıyor?
  * Grup tabanlı lisans yönetimi çözümü işliyor? 
  
    * Yanıt Evet ise, bir güvenlik grubu atamak mümkün mü? 
    * Evet ise, bulut dizini lisans grubunun tüm üyelerine otomatik olarak atayabilirim? 
    * Bir kullanıcı daha sonra eklenen veya gruptan, bir lisans otomatik olarak atanan veya kaldırılacak uygun şekilde kaldırıldı ne olacak? 
* Diğer üçüncü taraf kimlik sağlayıcıları ile tümleştirme:
  * Bu karma çözümü, çoklu oturum açmayı uygulamak için üçüncü taraf kimlik sağlayıcıları ile tümleştirilebilir?
  * Tüm farklı kimlik sağlayıcıları cohesive kimlik sisteme buluşturulan mümkündür?
  * Yanıt Evet ise nasıl ve bunlar ve hangi özelliklerin kullanılabilir olduğu?

## <a name="synchronization-management"></a>Eşitleme Yönetimi
Tüm kimlik sağlayıcıları getirin ve saklamak için bir kimlik Yöneticisi'nin hedeflerinden eşitlendi. Eşitlenen verileri tutmak bir yetkili ana kimlik sağlayıcısını temel. Eşitlenmiş yönetim modeliyle bir karma kimlik senaryosunda bir şirket içi sunucusundaki tüm kullanıcı ve cihaz kimliklerini yönetme ve hesapları ve isteğe bağlı, buluta parolaları eşitleyin. Kullanıcı, bulutta yaradıkları ve oturum açma işleminde, parola kimlik çözümü tarafından doğrulanmış olarak aynı şirket içi parolayı girer. Bu model, bir dizin eşitleme aracı kullanır.

![Dizin eşitleme](./media/plan-hybrid-identity-design-considerations/Directory_synchronization.png) uygun tasarıma, karma kimlik çözümü eşitlenmesini sağlamak aşağıdaki soruları yanıtlanır:
*    Karma kimlik çözümü için kullanılabilir eşitleme çözümleri nelerdir?
*    Çoklu oturum açma özellikleri nelerdir?
*    B2B ve B2C arasında Kimlik Federasyonu için seçenekleri nelerdir?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimini benimseme stratejinizi belirleme](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

