---
title: Azure Active Directory Kullanım Koşulları| Microsoft Docs
description: Azure AD Kullanım Koşulları, size ve şirketinize Azure AD hizmetleri kullanıcıları için kullanım koşulları sağlama olanağı sunar.
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/29/2018
ms.author: billmath
ms.openlocfilehash: 208a65c09b13acad62c9b6d8e55b6050041c9f5d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Azure Active Directory Kullanım Koşulları özelliği
Azure AD Kullanım Koşulları, kuruluşların son kullanıcılara bilgi sağlamak için kullanabileceği basit bir yöntem sunar.  Bu sunum, kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlar.

Azure AD Kullanım Koşulları, içerik sunmak için PDF biçimini kullanır.   Bu PDF, kullanıcıların oturum açtığı sırada son kullanıcı sözleşmelerini toplamanıza olanak sağlayan herhangi bir içerik (örneğin mevcut sözleşme belgeleri) olabilir.  Kullanım koşullarını uygulamalar veya kullanıcı grupları için ya da farklı amaçlarla birden çok kullanım koşulları belgeniz olduğunda kullanabilirsiniz.

Bu belgenin geri kalanında Azure AD Kullanım Koşulları’nı nasıl kullanmaya başlayabileceğiniz açıklanmaktadır.  

## <a name="why-use-azure-ad-terms-of-use"></a>Neden Azure AD Kullanım Koşulları’nı kullanmalısınız?
Çalışanların veya ziyaretçilerin erişim sağlamadan önce koşullarınızı kabul etmesi konusunda zorluklarla mı karşılaşıyorsunuz? Şirket kullanım koşullarınızı kabul eden ve etmeyen kişileri belirlemek için yardıma mı ihtiyacınız var?  Azure AD Kullanım Koşulları, kuruluşların son kullanıcılara bilgi sağlamak için kullanabileceği basit bir yöntem sunar.  Bu sunum, yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmelerini sağlar.

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
![Kullanım Koşullarını Ekleme](media/active-directory-tou/tou12.png)
3. Kullanım Koşulları için bir **Ad** girin
4. **Görünen Ad** girin.  Kullanıcılar oturum açtıklarında üst bilgiyi görür.
5. Kullanım Koşullarınızın son halinin bulunduğu PDF’ye **gözatın** ve bunu seçin.  Önerilen yazı tipi boyutu 24’tür.
6. Kullanım koşulları için bir dil **seçin**.  Dil seçeneğini kullanarak her biri farklı dilde olan birden fazla kullanım koşulunu karşıya yükleyebilirsiniz.  Bir son kullanıcının göreceği kullanım koşulları sürümü, kullanıcının tarayıcı tercihlerine bağlıdır.
7. **Kullanıcıların kullanım koşullarını genişletmesini gerekli kıl** için açık veya kapalı seçeneğini belirleyin.  Bu seçenek açık olarak ayarlanırsa, son kullanıcıların kullanım şartlarını kabul etmeden önce görüntülemesi gerekir.
8. **Koşullu Erişim** bölümünde, bir özel koşullu erişim ilkesi veya açılır listeden bir şablon seçerek karşıya yüklenen kullanım koşullarını **Zorunlu Kılabilirsiniz**.  Özel koşullu erişim ilkeleri, belirli bulut uygulamaları veya kullanıcı gruplarına kadar ayrıntılı kullanım koşulları uygulamanıza olanak sağlar.  Daha fazla bilgi için bkz. [Koşullu erişim ilkelerini yapılandırma](active-directory-conditional-access-best-practices.md)
9. **Oluştur**’a tıklayın.
10. Özel bir koşullu erişim şablonu seçtiyseniz, CA ilkesini özelleştirmenize olanak sağlayan yeni bir ekran görüntülenir.
11. Şimdi yeni Kullanım Koşullarınızı görürsünüz.</br>

![Kullanım Koşullarını ekleme](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Kullanım Koşullarını silme
Aşağıdaki yordamı kullanarak eski kullanım koşullarını kaldırabilir veya silebilirsiniz:

### <a name="to-delete-terms-of-use"></a>Kullanım Koşullarını silmek için
1. [https://aka.ms/catou](https://aka.ms/catou) adresindeki panoya gidin
2. Kaldırmak istediğiniz kullanım koşullarını seçin.
3. **Sil**'e tıklayın.
4. Bu işlemden sonra yeni Kullanım Koşullarınızı görmezsiniz.


## <a name="viewing-current-user-status"></a>Geçerli kullanıcı durumunu görüntüleme
Kullanım koşullarınızın, kabul eden ve reddeden kullanıcı sayısını gösterdiğini fark edeceksiniz.

![Denetim Olayı](media/active-directory-tou/tou15.png)

Kullanıcıların geçerli durumunu görüntülemek için, **kabul edilenler** veya **reddedilenler** bölümündeki sayılara tıklayabilirsiniz.

![Denetim Olayı](media/active-directory-tou/tou16.png)

## <a name="audit-terms-of-use"></a>Kullanım Koşullarını denetleme
Yalnızca mevcut durumu değil, geçmişteki kabul edenleri ve reddedenleri de görüntülemek istiyorsanız Azure AD Kullanım Koşulları, kullanımı kolay denetim sağlar.  Bu denetim, kullanım koşullarınızı kimlerin ne zaman kabul ettiğini görmenize olanak sağlar.  

Şu anda ne yapmaya çalıştığınıza bağlı olarak, denetimi kullanmanızın iki yolu vardır.  


Denetimi başlatmak için aşağıdaki yordamı kullanın:

### <a name="to-audit-terms-of-use"></a>Kullanım Koşullarını denetlemek için
1. [https://aka.ms/catou](https://aka.ms/catou) adresindeki panoya gidin
2. Denetim günlüklerini görüntüle’ye tıklayın.</br>
![Denetim Olayı](media/active-directory-tou/tou8.png)
3.  Azure AD denetim günlükleri ekranında sağlanan açılır kutuları kullanarak belirli denetim günlüğü bilgilerini hedeflemek için bilgileri filtreleyebilirsiniz.
![Denetim Olayı](media/active-directory-tou/tou9.png)
4.  Ayrıca bilgileri yerel olarak kullanmak için bir .csv dosyasında indirebilirsiniz.

## 

## <a name="what-users-see"></a>Kullanıcıların gördükleri
Kapsam dahilindeki kullanıcılar bir kullanım koşulları belgesi oluşturulup uygulandığında aşağıdakileri görür.  Oturum açma sırasında bu ekranları görürler.
-   PDF içinde 24 yazı tipi boyutunun kullanılması önerilir.
![Denetim Olayı](media/active-directory-tou/tou10.png)
-   Mobil cihazlarda bu ekrandaki gibi görünür</br></br>
![Denetim Olayı](media/active-directory-tou/tou11.png)

### <a name="review-terms-of-use"></a>Kullanım koşullarını gözden geçirme
Kullanıcılar, kabul ettikleri kullanım koşullarını gözden geçirip inceleyebilir.  Kullanım koşullarını gözden geçirmek için aşağıdaki yordamı kullanın:

1. [https://myapps.microsoft.com](https://myapps.microsoft.com) adresine gidip burada oturum açın.
2. Sağ üst köşede adınıza tıklayın ve açılır menüden **Profil**’i seçin.
![Profil](media/active-directory-tou/tou14.png)

3. Profilinizde **Kullanım koşullarını gözden geçir**’e tıklayın.
![Denetim Olayı](media/active-directory-tou/tou13a.png)

4.  Buradan, kabul ettiğiniz kullanım koşullarını gözden geçirebilirsiniz. 


## <a name="additional-information"></a>Ek bilgiler
Aşağıdaki bilgiler göz önünde bulundurulmalıdır; bunlar kullanım koşullarının kullanılmasında faydalı olabilir.

>[!IMPORTANT]
> Aşağıdaki durumlarda kapsam dahilindeki kullanıcıların yeni bir ilkeyi karşılamak için oturumu kapatıp yeniden oturum açmaları gerekir:
> - Kullanım koşullarında bir koşullu erişim ilkesi etkinleştirildiğinde
> - veya ikinci bir kullanım koşulları belgesi oluşturulduğunda
>
>Koşullu erişim ilkeleri hemen etkili olur. Bu durum gerçekleştiğinde, yönetici “üzgün bulutlar” veya “Azure AD belirteç sorunları” ile karşılaşmaya başlar. Yöneticinin yeni ilkeyi karşılamak için oturumu kapatıp yeniden oturum açması gerekir.





## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Kullanıcının kullanım koşullarını kabul edip etmediğini veya ne zaman kabul ettiğini nasıl görebilirim?**</br>
C: Kullanım koşullarınızın yanında kabul edilenler bölümündeki sayıya tıklayabilirsiniz.  Daha fazla bilgi için bkz. [Geçerli kullanıcı durumunu görüntüleme](#viewing-current-user-status).  Ayrıca, kullanım koşullarını kabul eden bir kullanıcı da denetim günlüğüne yazılır. Sonuçları görmek için Azure AD denetim günlüğünde arama yapabilirsiniz.  

**S: Kullanım koşulları değiştirilirse kullanıcıların tekrar kabul etmesi gerekir mi?**</br>
C: Evet, bir yönetici kullanım koşullarının hükümlerini değiştirebilir ve bu durumda yeni hükümlerin yeniden kabul edilmesi gerekir.

**S: Bir kullanım koşulları belgesi birden çok dili destekleyebilir mi?**</br>
C: Evet.  Şu anda bir yöneticinin tek bir kullanım koşulları belgesi için yapılandırabileceği 18 farklı dil mevcuttur. 

**S: Kullanım koşulları ne zaman tetiklenir?**</br>
C: Kullanım koşulları oturum açma deneyimi sırasında tetiklenir.

**S: Kullanım koşulları ile hangi uygulamaları hedefleyebilirim?**</br>
C: Modern kimlik doğrulaması kullanarak kurumsal uygulamalar üzerinde bir koşullu erişim ilkesi oluşturabilirsiniz.  Daha fazla bilgi için bkz. [Kurumsal uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**S: Belirli bir kullanıcı veya uygulamaya birden çok kullanım koşulları belgesi ekleyebilir miyim?**</br>
C: Evet, bu grup veya uygulamaları hedefleyen birden çok koşullu erişim ilkesi oluşturarak bunu gerçekleştirebilirsiniz. Birden çok kullanım koşulları belgesinin kapsamında olan bir kullanıcı, bunları tek tek kabul eder.
 
**S: Bir kullanıcı kullanım koşullarını reddederse ne olur?**</br>
C: Kullanıcının uygulamaya erişimi engellenir. Kullanıcının erişim sağlamak için tekrar oturum açıp kullanım koşullarını kabul etmesi gerekir.