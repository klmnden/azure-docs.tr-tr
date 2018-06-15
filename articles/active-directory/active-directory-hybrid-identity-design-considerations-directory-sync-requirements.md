---
title: Karma kimlik tasarımı - dizin eşitleme gereksinimlerini Azure | Microsoft Docs
description: Hangi gereksinimler tüm kullanıcılar arasında eşitlemek için gerekli olan tanımlamak şirket içi ve bulut kuruluş için on =.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: d6749960806e858909f42c6ecccd445ba8d5ec00
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801570"
---
# <a name="determine-directory-synchronization-requirements"></a>Dizin eşitleme gereksinimlerini belirleyin
Eşitleme, kullanıcıların kendi şirket içi kimliğine göre bulutta bir kimlik sağlama tüm hakkında olur. Kimlik doğrulaması veya şirket dışı kimlik doğrulaması için eşitlenmiş hesap kullanacakları olup olmadığına bakılmaksızın, kullanıcıların hala bulutta bir kimliğe sahip gerekir.  Bu kimlik korunur ve düzenli aralıklarla güncelleştirilen gerekecektir.  Güncelleştirmeleri başlık değişikliklerden parola değişiklikleri için birçok biçimde olabilir.  

Kuruluşlar şirket içi kimlik çözümü ve kullanıcı gereksinimleri değerlendirerek başlatın. Bu değerlendirme nasıl kullanıcı kimlikleri oluşturulacak ve bulutta saklanır teknik gereksinimlerini tanımlamak önemlidir.  Kuruluşların çoğunluğunun, şirket içi Active Directory verilir ve kullanıcılar tarafından olacak şirket içi dizin, ancak bazı durumlarda bu durumda olmayacaktır, eşitlenir.  

Aşağıdaki soruları yanıtlayın emin olun:

* Bir AD ormanında, birden çok veya hiçbiri var mı?
  
  * Kaç tane Azure AD dizinler için eşitleme?
    
    1. Filtreleme kullanıyor musunuz?
    2. Planlanan birden çok Azure AD Connect sunucuları var mı?
* Şu anda bir eşitleme elinizde aracı şirket içi?
  
  * Kullanıcıların kimliklerinin sanal bir dizin/tümleştirme varsa Evet ise, kullanıcılarınızın mu?
* Tüm diğer dizin (örn. LDAP dizini, ik veritabanını, vb.) eşitlemek istediğiniz şirket içi var mı?
  * Tüm GALSync yaparak mısınız?
  * UPN'ler geçerli durumu, kuruluşunuzda nedir? 
  * Kullanıcıların kimlik doğrulaması farklı bir dizin var mı?
  * Şirketiniz Microsoft Exchange kullanıyor mu?
    * Bunlar, karma bir exchange dağıtım sahip planlıyor musunuz?

Eşitleme gereksinimleri hakkında bir fikir sahip olduğunuza göre bu gereksinimleri karşılamak için doğru bir araçtır belirlemeniz gerekir.  Microsoft directory tümleştirme ve eşitleme gerçekleştirmek için çeşitli araçlar sağlar.  Bkz: [karma kimlik dizini tümleştirme araçları karşılaştırması tablo](active-directory-hybrid-identity-design-considerations-tools-comparison.md) daha fazla bilgi için. 

Eşitleme gereksinimlerinizi ve bu, şirketiniz için yapabiliriz aracı sahip olduğunuza göre bu dizin hizmetleri kullanan uygulamalar değerlendirmeniz gerekir. Bu değerlendirme, bulut için bu uygulamaları tümleştirmek için teknik gereksinimleri tanımlamak önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Bu uygulamalar buluta taşınır ve dizini kullanın?
* Bu uygulamalar başarıyla kullanabilmeniz için bulut için eşitlenmesi gereken özel öznitelikler var mı?
* Bu uygulamalar, bulut kimlik doğrulama yararlanmak için yeniden yazılması gerekecek mi?
* Bu uygulamaları kullanıcıların bulut kimliği kullanarak bunları erişirken şirket içi Canlı devam eder?

Ayrıca, güvenlik gereksinimleri ve kısıtlamaları dizin eşitleme belirlemeniz gerekir. Bu değerlendirme oluşturmak ve kullanıcının kimlikleri bulutta korumak için gerekli olan gereksinimleri listesini almak önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Burada eşitleme sunucusu bulunur?
* Etki alanına katılmış olacak?
* Sunucu, kısıtlanmış bir ağda DMZ gibi bir güvenlik duvarının arkasında bulunur?
  * Eşitlemeyi desteklemek için gerekli güvenlik duvarı bağlantı noktalarını açmak erişebilecek mi?
* Eşitleme sunucusu için bir olağanüstü durum kurtarma planı var mı?
* İle eşitleme yapmak istediğiniz tüm orman için doğru izinlere sahip bir hesaba var mı?
  * Şirketiniz bu soruya yanıt bilmiyor makaleyi "Parola Eşitleme izinlerini" bölümünde inceleyin [Azure Active Directory Eşitleme Hizmeti'ni yükleme](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) ve bir hesap zaten yüklü olup olmadığını belirleme Bu izinlere sahip olan veya oluşturmanız gerekir.
* Varsa belirleyebiliriz orman eşitleme eşitleme sunucusu her orman için almak mi?

> [!NOTE]
> Her yanıtı not alın ve yanıtın yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıtlama gereksinimlerini belirlemek](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) seçeneklerin geçilir. Seçeneği, iş gereksinimlerinize en uygun belirleyeceksiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Çok faktörlü kimlik doğrulama gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

