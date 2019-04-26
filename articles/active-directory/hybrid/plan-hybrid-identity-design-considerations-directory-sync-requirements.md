---
title: Karma kimlik tasarımı - dizin eşitleme gereksinimleri Azure | Microsoft Docs
description: Hangi gereksinimlerin tüm kullanıcılar arasında eşitlemek için gerekli olan tanımlamak içinde ve bulutta Kurumsal on =.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21558c4eccf0cd1f4e9e1d630f0e89dbb6f01c51
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60381170"
---
# <a name="determine-directory-synchronization-requirements"></a>Dizin eşitleme gereksinimlerini belirleme
Eşitleme, kullanıcıların bulutta, şirket içi kimliğine dayalı bir kimlik tüm sağlamakla ilgilidir. Kimlik doğrulama veya Federasyon kimlik doğrulaması için eşitlenmiş hesabı kullanacakları olsun veya olmasın, kullanıcıların bulutta bir kimlik sağlamak yine de gerekir.  Bu kimlik bakımı yapılan ve düzenli aralıklarla güncelleştirilen gerekecektir.  Güncelleştirmeleri, parola değişiklikleri için başlık değişikliklerden birçok biçimde olabilir.  

Kuruluşlar şirket içi kimlik çözümü ve kullanıcı gereksinimleri değerlendirerek başlatın. Bu değerlendirme nasıl kullanıcı kimliklerini oluşturulur ve bulutta saklanır teknik gereksinimleri tanımlamak önemlidir.  Kuruluşların çoğu, şirket içi Active Directory, ve bu kullanıcılar tarafından şirket içi dizin, ancak bazı durumlarda bu durum olmayacaktır, eşitlenecektir.  

Aşağıdaki soruları yanıtlamak emin olun:

* Bir AD ormanında, birden çok veya hiçbiri var mı?
  
  * Nasıl birden çok Azure AD dizini için eşitleme?
    
    1. Filtreleme kullanıyorsunuz?
    2. Birden çok Azure AD Connect sunucuları planlı var mı?
* Eşitleme şu anda izniniz aracı şirket içi?
  
  * Kullanıcıların kimliklerinin sanal bir dizin/tümleştirme varsa Evet ise, kullanıcılarınızın mu?
* Herhangi diğer dizin (örneğin LDAP dizini, ik veritabanını, vb.) eşitlemek istediğiniz şirket içinde var mı?
  * Tüm GALSync çıkarıyor saldıracak mı?
  * UPN, kuruluşunuzdaki geçerli durumu nedir? 
  * Kullanıcıların kimlik doğrulaması için farklı bir dizin var mı?
  * Şirketiniz Microsoft Exchange kullanıyor mu?
    * Exchange karma dağıtımı kilitlenmelerinden planlıyor musunuz?

Eşitleme gereksinimleriniz hakkında fikir olduğuna göre bu gereksinimleri karşılamak için doğru bir araçtır belirlemeniz gerekir.  Microsoft, dizin tümleştirme ve eşitleme gerçekleştirmek için çeşitli araçlar sağlar.  Bkz: [karma kimlik dizini tümleştirme araçları karşılaştırması tablo](plan-hybrid-identity-design-considerations-tools-comparison.md) daha fazla bilgi için. 

Eşitleme gereksinimlerinizi ve bu durum, şirketiniz için yerine getirmiş olacaksınız aracı edindikten sonra bu dizin hizmetleri kullanan uygulamalar değerlendirilecek gerekir. Bu değerlendirme bu uygulamaları bulutta tümleştirmek için teknik gereksinimlerini tanımlamak önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* Bu uygulamaların buluta taşınması ve dizinini?
* Bu uygulamalar başarıyla kullanabilmesi için buluta eşitlenmesi gereken özel öznitelikleri vardır?
* Bu uygulamaların bulut kimlik doğrulaması yararlanmak için yeniden yazılması gerekiyor mu?
* Bu uygulamalar, kullanıcıların bulut kimliği kullanarak bunları erişirken şirket içi Canlı devam edecek mi?

Ayrıca güvenlik gereksinimleri ve kısıtlamaları dizin eşitlemesini belirlemek gerekir. Bu değerlendirme oluşturmak ve bulutta kullanıcı kimlikleri sürdürmek için gerekli gereksinimleri listesini almak önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* Eşitleme sunucusu konumlandırılacağı?
* Bu, etki alanına katılmış olacak mı?
* Sunucu bir DMZ gibi bir güvenlik duvarının arkasındaki kısıtlı bir ağ üzerinde yer alır?
  * Eşitlemeyi desteklemek için gerekli güvenlik duvarı bağlantı noktalarını açmak olacak mı?
* Eşitleme sunucusu için bir olağanüstü durum kurtarma planı var mı?
* Bir hesap ile eşitleme yapmak istediğiniz tüm ormanlar için doğru izinlere sahip gerekiyor?
  * Şirketiniz, bu soru için yanıt bilmez, ' % s'bölümü "Parola Eşitleme için izinler" makalesinde gözden geçirin [Azure Active Directory Sync hizmetini yükleme](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) ve bir hesap zaten yüklü olup olmadığını belirleme Bu izinlere sahip olan veya oluşturmanız gerekir.
* Varsa aktarılacaksa orman eşitleme eşitleme sunucusu almak için her bir orman mi?

> [!NOTE]
> Her yanıtı Not ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md) seçeneklerin geçilir. En iyi seçeneği işletmenizi uygun seçersiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

