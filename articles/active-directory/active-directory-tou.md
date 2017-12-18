---
title: "Azure Active Directory Kullanım Koşulları| Microsoft Docs"
description: "Azure AD Kullanım Koşulları, size ve şirketinize Azure AD hizmetleri kullanıcıları için kullanım koşulları sağlama olanağı sunar."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/17/2017
ms.author: billmath
ms.openlocfilehash: a935c3a7a5eeead8eaac5d8d0980c289b17f3289
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-terms-of-use-feature-preview"></a>Azure Active Directory Kullanım Koşulları özelliği (Önizleme)
Azure AD Kullanım Koşulları, kuruluşların son kullanıcılara bilgi sağlamak için kullanabileceği basit bir yöntem sunar.  Böylece kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlanır.

Azure AD Kullanım Koşulları, içerik sunmak için PDF biçimini kullanır.   Bu PDF, kullanıcıların oturum açtığı sırada son kullanıcı sözleşmelerini toplamanıza olanak sağlayan herhangi bir içerik (örneğin mevcut sözleşme belgeleri) olabilir.  Kullanım koşullarını uygulamalar veya kullanıcı grupları için ya da farklı amaçlarla birden çok kullanım koşulları belgeniz olduğunda kullanabilirsiniz.

Bu belgenin geri kalanında Azure AD Kullanım Koşulları’nı nasıl kullanmaya başlayabileceğiniz açıklanmaktadır.  

## <a name="why-use-azure-ad-terms-of-use"></a>Neden Azure AD Kullanım Koşulları’nı kullanmalısınız?
Çalışanların veya ziyaretçilerin erişim sağlamadan önce koşullarınızı kabul etmesi konusunda zorluklarla mı karşılaşıyorsunuz? Şirket kullanım koşullarınızı kabul eden ve etmeyen kişileri belirlemek için yardıma mı ihtiyacınız var?  Azure AD Kullanım Koşulları, kuruluşların son kullanıcılara bilgi sağlamak için kullanabileceği basit bir yöntem sunar.  Böylece kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlanır.

Azure AD Kullanım Koşulları aşağıdaki senaryolarda kullanılabilir:
-   Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşulları.
-   Kullanıcı özniteliklerine dayalı belirli kullanım koşulları (örneğin, doktorlarla hemşirelere ya da yurtiçi ve uluslararası çalışanlara, [dinamik gruplara](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups) göre farklı koşullar sunulması).
-   Yüksek iş etkisine sahip uygulamalara (Salesforce gibi) erişim durumuna dayalı belirli kullanım koşulları.


## <a name="prerequisites"></a>Ön koşullar
Azure AD Kullanım Koşullarını yapılandırmak için aşağıdaki adımları kullanın:

1. Azure AD Kullanım Koşulları’nı yapılandırmak istediğiniz dizin için Azure AD’de bir genel yönetici, güvenlik yöneticisi veya koşullu erişim yöneticisi hesabını kullanarak oturum açın.
2. Dizinde bir Azure AD Premium P1, P2, EMS E3 veya EMS E5 aboneliği olduğundan emin olun.  Yoksa, [Azure AD Premium’u edinin](active-directory-get-started-premium.md) veya [bir deneme sürümü başlatın](https://azure.microsoft.com/trial/get-started-active-directory/).
3. [https://aka.ms/catou](https://aka.ms/catou) adresinde yer alan Azure AD Kullanıcı Koşulları panosunu görüntüleyin.

>[!IMPORTANT]
>Koşullu erişim ilkesi denetimleri (kullanım koşulları dahil), hizmet hesaplarında uygulamayı desteklemez.  Tüm hizmet hesaplarının koşullu erişim ilkesinden hariç tutulması önerilir.

## <a name="add-company-terms-of-use"></a>Şirket Kullanım Koşulları ekleme
Kullanım Koşullarınıza son şeklini verdikten sonra, bunları eklemek için aşağıdaki yordamı kullanın.

### <a name="to-add-terms-of-use"></a>Kullanım Koşulları eklemek için
1. [https://aka.ms/catou](https://aka.ms/catou) adresindeki panoya gidin
2. Ekle'ye tıklayın.</br>
![Kullanım Koşullarını Ekleme](media/active-directory-tou/tou2.png)
3. Kullanım Koşulları için bir **Ad** girin
4. **Görünen Ad** girin.  Kullanıcılar oturum açtıklarında bu üst bilgiyi görür.
5. Kullanım Koşullarınızın son halinin bulunduğu PDF’ye **gözatın** ve bunu seçin.  Önerilen yazı tipi boyutu 24’tür.
6. Bir şablon veya özel koşullu erişim ilkesi kullanarak karşıya yüklenen kullanım koşullarını **Zorunlu kılabilirsiniz**.  Özel koşullu erişim ilkeleri, belirli bulut uygulamaları veya kullanıcı gruplarına kadar ayrıntılı kullanım koşulları uygulamanıza olanak sağlar.  Daha fazla bilgi için bkz. [Koşullu erişim ilkelerini yapılandırma](active-directory-conditional-access-best-practices.md)
7. **Oluştur**'a tıklayın.
8. Özel bir koşullu erişim şablonu seçtiyseniz, CA ilkesini özelleştirmenize olanak sağlayan yeni bir ekran görüntülenir.
7. Şimdi yeni Kullanım Koşullarınızı görürsünüz.</br>

![Kullanım Koşullarını ekleme](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Kullanım Koşullarını silme
Aşağıdaki yordamı kullanarak eski kullanım koşullarını kaldırabilir veya silebilirsiniz:

### <a name="to-delete-terms-of-use"></a>Kullanım Koşullarını silmek için
1. [https://aka.ms/catou](https://aka.ms/catou) adresindeki panoya gidin
2. Kaldırmak istediğiniz kullanım koşullarını seçin.
3. **Sil**'e tıklayın.
4. Bu işlemden sonra yeni Kullanım Koşullarınızı görmezsiniz.


## <a name="audit-terms-of-use"></a>Kullanım Koşullarını denetleme
Azure AD Kullanım Koşulları, kullanım koşullarınızın kimler tarafından ne zaman kabul edildiğini görmenize olanak sağlayan kullanımı kolay denetim sunar.  Denetimi başlatmak için aşağıdaki yordamı kullanın:

### <a name="to-audit-terms-of-use"></a>Kullanım Koşullarını denetlemek için
1. [https://aka.ms/catou](https://aka.ms/catou) adresindeki panoya gidin
2. Denetim Olayı’na tıklayın.</br>
![Denetim Olayı](media/active-directory-tou/tou8.png)
3.  Azure AD denetim günlükleri ekranında sağlanan açılır kutuları kullanarak belirli denetim günlüğü bilgilerini hedeflemek için bilgileri filtreleyebilirsiniz.
![Denetim Olayı](media/active-directory-tou/tou9.png)
4.  Ayrıca bilgileri yerel olarak kullanmak için bir .csv dosyasında indirebilirsiniz.

## <a name="what-users-see"></a>Kullanıcıların gördükleri
Kapsam dahilindeki kullanıcılar bir kullanım koşulları belgesi oluşturulup uygulandığında aşağıdakileri görür.  Oturum açma sırasında bu ekranları görürler.
-   PDF içinde 24 yazı tipi boyutunun kullanılması önerilir.
![Denetim Olayı](media/active-directory-tou/tou10.png)
-   Mobil cihazlarda bu ekrandaki gibi görünür</br></br>
![Denetim Olayı](media/active-directory-tou/tou11.png)

## <a name="additional-information"></a>Ek bilgiler
Aşağıdaki bilgiler göz önünde bulundurulmalıdır; bunlar kullanım koşullarının kullanılmasında faydalı olabilir.

Aşağıdaki durumlarda kapsam dahilindeki kullanıcıların yeni bir ilkeyi karşılamak için oturumu kapatıp yeniden oturum açmaları gerekir:
 - Kullanım koşullarında bir koşullu erişim ilkesi etkinleştirildiğinde
 - veya ikinci bir kullanım koşulları belgesi oluşturulduğunda

Bunun nedeni koşullu erişim ilkelerinin hemen etkili olmasıdır. Bu durum gerçekleştiğinde, yönetici “üzgün bulutlar” veya “Azure AD belirteç sorunları” ile karşılaşmaya başlar. Yöneticinin yeni ilkeyi karşılamak için oturumu kapatıp yeniden oturum açması gerekir.





## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Kullanıcının kullanım koşullarını kabul edip etmediğini veya ne zaman kabul ettiğini nasıl görebilirim?**</br>
C: Kullanım koşullarını kabul eden bir kullanıcı denetim günlüğüne yazılır. Sonuçları görmek için Azure AD denetim günlüğünde arama yapabilirsiniz.  

**S: Kullanım koşulları değiştirilirse kullanıcıların tekrar kabul etmesi gerekir mi?**</br>
C: Evet, bir yönetici kullanım koşullarının koşullarını değiştirebilir ve bu durumda yeni koşulların yeniden kabul edilmesi gerekir.

**S: Bir kullanım koşulları belgesi birden çok dili destekleyebilir mi?**</br>
C: Hayır, tek bir kullanım koşulları içinde birden çok dil kullanmak şu anda mümkün değildir.  Ancak bir grubun kapsamını belirleyebilirsiniz (örneğin, Fransa’ya yönelik kullanım koşulları Birleşik Krallık’tan farklı olabilir). 

**S: Kullanım koşulları ne zaman tetiklenir?**</br>
C: Kullanım koşulları oturum açma deneyimi sırasında tetiklenir.

**S: Kullanım koşulları ile hangi uygulamaları hedefleyebilirim?**</br>
C: Modern kimlik doğrulaması kullanarak kurumsal uygulamalar üzerinde bir koşullu erişim ilkesi oluşturabilirsiniz.  Daha fazla bilgi için bkz. [Kurumsal uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**S: Belirli bir kullanıcı veya uygulamaya birden çok kullanım koşulları belgesi ekleyebilir miyim?**</br>
C: Evet, bu grup veya uygulamaları hedefleyen birden çok koşullu erişim ilkesi oluşturarak bunu gerçekleştirebilirsiniz. Birden çok kullanım koşulları belgesinin kapsamında olan bir kullanıcı, bunları tek tek kabul eder.
 
**S: Bir kullanıcı kullanım koşullarını reddederse ne olur?**</br>
C: Kullanıcının uygulamaya erişimi engellenir. Kullanıcının erişim sağlamak için tekrar oturum açıp kullanım koşullarını kabul etmesi gerekir.