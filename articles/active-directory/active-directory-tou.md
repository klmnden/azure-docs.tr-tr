---
title: Azure Active Directory Kullanım Koşulları| Microsoft Docs
description: Azure AD Kullanım Koşulları, size ve şirketinize Azure AD hizmetleri kullanıcıları için Kullanım Koşulları sağlama olanağı sunar.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.component: compliance-reports
ms.date: 06/18/2018
ms.author: rolyon
ms.openlocfilehash: 2919ce1d7c57b7a92420ac11b61503caa1fdd3b0
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36267566"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Azure Active Directory Kullanım Koşulları özelliği
Azure AD Kullanım Koşulları, kuruluşların son kullanıcılara bilgi sağlamak için kullanabileceği basit bir yöntem sunar. Bu sunum, kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlar. Bu makalede Azure AD Kullanım Koşullarını kullanmaya nasıl başlayacağınız açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-can-i-do-with-terms-of-use"></a>Kullanım Koşulları ile ne yapabilirim?
Azure AD Kullanım Koşulları aşağıdakileri yapmanızı sağlar:
- Çalışanların veya ziyaretçilerin erişim sağlamadan önce Kullanım Koşullarınızı kabul etmesini zorunlu tutun.
- Genel Kullanım Koşullarınızı kuruluşunuzdaki tüm kullanıcılarla paylaşın.
- Kullanıcı özniteliklerine dayalı belirli Kullanım Koşulları (örneğin, doktorlarla hemşirelere ya da yurtiçi ve uluslararası çalışanlara, [dinamik grupları](active-directory-groups-dynamic-membership-azure-portal.md) kullanarak) sunun.
- Yüksek iş etkisine sahip uygulamalara (Salesforce gibi) erişim sırasında geçerli belirli Kullanım Koşulları sunun.
- Kullanım Koşullarını farklı dillerde sunun.
- Kullanım Koşullarınızı kabul etmemiş olan kullanıcıları listeleyin.
- Kullanım Koşulları etkinlikleriyle ilgili bir denetim günlüğü görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar
Azure AD Kullanım Koşullarını kullanmak ve yapılandırmak için şunlara sahip olmalısınız:

- Azure AD Premium P1, P2, EMS E3 veya EMS E5 aboneliği.
    - Bu aboneliklerden birine sahip değilseniz [Azure AD Premium'u alabilir](fundamentals/active-directory-get-started-premium.md) veya [Azure AD Premium deneme sürümünü etkinleştirebilirsiniz](https://azure.microsoft.com/trial/get-started-active-directory/).
- Yapılandırmak istediğiniz dizin için aşağıdaki yönetici hesaplarından biri:
    - Genel yönetici
    - Güvenlik yöneticisi
    - Koşullu erişim yöneticisi

## <a name="terms-of-use-document"></a>Kullanım koşulları belgesi

Azure AD Kullanım Koşulları, içerik sunmak için PDF biçimini kullanır. Bu PDF dosyası, kullanıcıların oturum açtığı sırada son kullanıcı sözleşmelerini toplamanıza olanak sağlayan herhangi bir içerik (örneğin mevcut sözleşme belgeleri) olabilir. PDF'te önerilen yazı tipi boyutu 24'tür.

## <a name="add-terms-of-use"></a>Kullanım Koşulları ekleme
Kullanım Koşulları belgenize son şeklini verdikten sonra, bunları eklemek için aşağıdaki yordamı kullanın.

1. Azure'da Genel yönetici, Güvenlik yöneticisi veya Koşullu erişim yöneticisi olarak oturum açın.

1. **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

    ![Kullanım koşulları dikey penceresi](media/active-directory-tou/tou-blade.png)

1. **Yeni koşullar**'a tıklayın.

    ![Kullanım Koşullarını ekleme](media/active-directory-tou/new-tou.png)

1. Kullanım Koşulları için bir **Ad** girin

2. **Görünen ad** girin.  Kullanıcılar oturum açtıklarında bu üst bilgiyi görür.

3. Kullanım Koşullarınızın son halinin bulunduğu PDF belgesine **göz atın** ve bunu seçin.

4. Kullanım Koşulları için bir dil **seçin**.  Dil seçeneğini kullanarak her biri farklı dilde olan birden fazla Kullanım Koşulları belgesini yükleyebilirsiniz.  Bir son kullanıcının göreceği Kullanım Koşulları sürümü, kullanıcının tarayıcı tercihlerine bağlıdır.

5. **Kullanıcıların kullanım koşullarını genişletmesini gerekli kıl** için Açık veya Kapalı seçeneğini belirleyin.  Bu ayar Açık olarak belirlenirse, son kullanıcıların Kullanım Koşullarını kabul etmeden önce görüntülemesi gerekir.

6. **Koşullu Erişim** bölümünde, bir özel koşullu erişim ilkesi veya açılır listeden bir şablon seçerek karşıya yüklenen Kullanım Koşullarını **Zorunlu Kılabilirsiniz**.  Özel koşullu erişim ilkeleri, belirli bulut uygulamaları veya kullanıcı gruplarına kadar ayrıntılı Kullanım Koşulları uygulamanıza olanak sağlar.  Daha fazla bilgi için bkz. [Koşullu erişim ilkelerini yapılandırma](active-directory-conditional-access-best-practices.md).

    >[!IMPORTANT]
    >Koşullu erişim ilkesi denetimleri (Kullanım Koşulları dahil), hizmet hesaplarında uygulamayı desteklemez.  Tüm hizmet hesaplarının koşullu erişim ilkesinden hariç tutulması önerilir.

7. **Oluştur**’a tıklayın.

8. Özel bir koşullu erişim şablonu seçtiyseniz, koşullu erişim ilkesini özelleştirmenize olanak sağlayan yeni bir ekran görüntülenir.

    Şimdi yeni Kullanım Koşullarınızı görürsünüz.

    ![Kullanım Koşullarını ekleme](media/active-directory-tou/create-tou.png)

## <a name="view-who-has-accepted-and-declined"></a>Kabul edenleri ve reddedenleri görüntüleme
Kullanım Koşulları dikey penceresinin kabul eden ve reddeden kullanıcı sayısını gösterdiğini fark edeceksiniz. Bu sayılar ve Kullanım Koşullarını kabul eden/reddeden kullanıcılar, Kullanım Koşulları özelliğini kullandığınız süre boyunca depolanır.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

    ![Denetim Olayı](media/active-directory-tou/view-tou.png)

1. Kullanıcıların geçerli durumunu görüntülemek için **Kabul edilenler** veya **Reddedilenler** bölümündeki sayılara tıklayın.

    ![Denetim Olayı](media/active-directory-tou/accepted-tou.png)

## <a name="view-audit-logs"></a>Denetim günlüklerini görüntüleme
Daha fazla etkinlik görüntülemek isterseniz Azure AD Kullanım Koşulları denetim günlüklerini inceleyebilirsiniz. Her kullanıcı izni denetim günlüklerinde 30 gün boyunca depolanan bir etkinliği tetikler. Bu günlükleri portalda görüntüleyebilir veya .csv dosyası olarak indirebilirsiniz.

Denetim günlüklerini kullanmaya başlamak için aşağıdaki yordamı kullanın:

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

1. **Denetim günlüklerini görüntüle**'ye tıklayın.

    ![Denetim Olayı](media/active-directory-tou/audit-tou.png)

1. Azure AD denetim günlükleri ekranında sağlanan açılan listeleri kullanarak belirli denetim günlüğü bilgilerini hedeflemek için bilgileri filtreleyebilirsiniz.

    ![Denetim Olayı](media/active-directory-tou/audit-logs-tou.png)

1. Ayrıca **İndir**'e tıklayarak bilgileri yerel olarak kullanmak üzere bir .csv dosyasında indirebilirsiniz.

## <a name="what-terms-of-use-looks-like-for-users"></a>Kullanım Koşullarının kullanıcılara görünüşü
Bir Kullanım Koşulları belgesi oluşturup uygulandığında kapsam dahilindeki kullanıcılar oturum açma sırasında aşağıdaki ekranı görür.

![Denetim Olayı](media/active-directory-tou/user-tou.png)

Aşağıdaki ekranda Kullanım Koşulları belgesinin mobil cihazlarda nasıl göründüğü gösterilmiştir.

![Denetim Olayı](media/active-directory-tou/mobile-tou.png)

### <a name="how-users-can-review-their-terms-of-use"></a>Kullanıcılar kendi Kullanım Koşullarını nasıl gözden geçirebilir?
Kullanıcılar, kabul ettikleri kullanım koşullarını gözden geçirip incelemek için aşağıdaki yordamı kullanabilir.

1. [https://myapps.microsoft.com](https://myapps.microsoft.com) adresinde oturum açın.

1. Sağ üst köşede adınıza tıklayın ve açılır menüden **Profil**'i seçin.

    ![Profil](media/active-directory-tou/tou14.png)

1. Profil sayfanızda **Kullanım koşullarını gözden geçir**'e tıklayın.

    ![Denetim Olayı](media/active-directory-tou/tou13a.png)

1. Buradan, kabul ettiğiniz Kullanım Koşullarını gözden geçirebilirsiniz. 

## <a name="delete-terms-of-use"></a>Kullanım Koşullarını silme
Aşağıdaki yordamı kullanarak eski Kullanım Koşullarını silebilirsiniz:

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

1. Kaldırmak istediğiniz Kullanım Koşullarını seçin.

1. **Koşulları sil**'e tıklayın.

1. Devam etmek isteyip istemediğinizi soran iletide **Evet**'e tıklayın.

    ![Kullanım Koşullarını ekleme](media/active-directory-tou/delete-tou.png)

    Bu işlemden sonra Kullanım Koşullarınızı görmezsiniz.

## <a name="deleted-users-and-active-terms-of-use"></a>Silinen kullanıcılar ve etkin Kullanım Koşulları
Varsayılan olarak, silinmiş bir kullanıcı Azure AD'de 30 gün boyunca silinmiş durumda kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir.  30 gün sonra bu kullanıcı kalıcı olarak silinir.  Ayrıca, bir Genel yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](fundamentals/active-directory-users-restore.md).  Bir kullanıcı kalıcı olarak silindikten sonra, bu kullanıcıya ilişkin sonraki veriler etkin Kullanım Koşullarından kaldırılır.  Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="policy-changes"></a>İlke değişiklikleri
Koşullu erişim ilkeleri hemen etkili olur. Bu durum gerçekleştiğinde, yönetici "üzgün bulutlar" veya "Azure AD belirteç sorunları" ile karşılaşmaya başlar. Yöneticinin yeni ilkeyi karşılamak için oturumu kapatıp yeniden oturum açması gerekir.

>[!IMPORTANT]
> Aşağıdaki durumlarda kapsam dahilindeki kullanıcıların yeni bir ilkeyi karşılamak için oturumu kapatıp yeniden oturum açmaları gerekir:
> - Kullanım Koşullarında bir koşullu erişim ilkesi etkinleştirildiğinde
> - veya ikinci bir Kullanım Koşulları belgesi oluşturulduğunda

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Kullanıcının Kullanım Koşullarını kabul edip etmediğini veya ne zaman kabul ettiğini nasıl görebilirim?**</br>
C: Kullanım Koşullarınızın yanında kabul edilenler bölümündeki sayıya tıklayabilirsiniz.  Daha fazla bilgi için bkz. [Kabul edenleri ve reddedenleri görüntüleme](#view-who-has-accepted-and-declined).  Ayrıca, Kullanım Koşullarını kabul eden bir kullanıcı da denetim günlüğüne yazılır. Sonuçları görmek için Azure AD denetim günlüğünde arama yapabilirsiniz.  

**S: Kullanım Koşulları değiştirilirse kullanıcıların tekrar kabul etmesi gerekir mi?**</br>
C: Evet, bir yönetici Kullanım Koşullarının hükümlerini değiştirebilir ve bu durumda yeni hükümlerin yeniden kabul edilmesi gerekir.

**S: Bir Kullanım Koşulları belgesi birden çok dili destekleyebilir mi?**</br>
C: Evet.  Şu anda bir yöneticinin tek bir Kullanım Koşulları belgesi için yapılandırabileceği 18 farklı dil mevcuttur. 

**S: Kullanım Koşulları ne zaman tetiklenir?**</br>
C: Kullanım Koşulları oturum açma deneyimi sırasında tetiklenir.

**S: Kullanım Koşullarını hangi uygulamalara hedefleyebilirim?**</br>
C: Modern kimlik doğrulaması kullanarak kurumsal uygulamalar üzerinde bir koşullu erişim ilkesi oluşturabilirsiniz.  Daha fazla bilgi için bkz. [Kurumsal uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**S: Belirli bir kullanıcı veya uygulamaya birden çok Kullanım Koşulları belgesi ekleyebilir miyim?**</br>
C: Evet, bu grup veya uygulamaları hedefleyen birden çok koşullu erişim ilkesi oluşturarak bunu gerçekleştirebilirsiniz. Birden çok Kullanım Koşulları belgesinin kapsamında olan bir kullanıcı, bunları tek tek kabul eder.
 
**S: Bir kullanıcı Kullanım Koşullarını reddederse ne olur?**</br>
C: Kullanıcının uygulamaya erişimi engellenir. Kullanıcının erişim sağlamak için tekrar oturum açıp kullanım koşullarını kabul etmesi gerekir.
 
**S: Bilgiler ne kadar süreyle depolanır?**</br>
C: Kullanıcı sayıları ve Kullanım Koşullarını kabul eden/reddeden kullanıcılar, Kullanım Koşulları özelliğini kullandığınız süre boyunca depolanır. Denetim günlükleri 30 gün süreyle depolanır.
