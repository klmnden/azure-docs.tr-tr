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
ms.topic: conceptual
ms.component: compliance
ms.date: 09/04/2018
ms.author: rolyon
ms.openlocfilehash: 9fa966999e220ea4357d5b5c37f0038c75fe2339
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607930"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Azure Active Directory Kullanım Koşulları özelliği
Azure AD Kullanım Koşulları, kuruluşların son kullanıcılara bilgi sağlamak için kullanabileceği basit bir yöntem sunar. Bu sunum, kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlar. Bu makalede Azure AD Kullanım Koşullarını kullanmaya nasıl başlayacağınız açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-can-i-do-with-terms-of-use"></a>Kullanım Koşulları ile ne yapabilirim?
Azure AD Kullanım Koşulları aşağıdakileri yapmanızı sağlar:
- Çalışanların veya ziyaretçilerin erişim sağlamadan önce Kullanım Koşullarınızı kabul etmesini zorunlu tutun.
- Genel Kullanım Koşullarınızı kuruluşunuzdaki tüm kullanıcılarla paylaşın.
- Kullanıcı özniteliklerine dayalı belirli Kullanım Koşulları (örneğin, doktorlarla hemşirelere ya da yurtiçi ve uluslararası çalışanlara, [dinamik grupları](../users-groups-roles/groups-dynamic-membership.md) kullanarak) sunun.
- Yüksek iş etkisine sahip uygulamalara (Salesforce gibi) erişim sırasında geçerli belirli Kullanım Koşulları sunun.
- Kullanım Koşullarını farklı dillerde sunun.
- Kullanım Koşullarınızı kabul etmemiş olan kullanıcıları listeleyin.
- Kullanım Koşulları etkinlikleriyle ilgili bir denetim günlüğü görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar
Azure AD Kullanım Koşullarını kullanmak ve yapılandırmak için şunlara sahip olmalısınız:

- Azure AD Premium P1, P2, EMS E3 veya EMS E5 aboneliği.
    - Bu aboneliklerden birine sahip değilseniz [Azure AD Premium'u alabilir](../fundamentals/active-directory-get-started-premium.md) veya [Azure AD Premium deneme sürümünü etkinleştirebilirsiniz](https://azure.microsoft.com/trial/get-started-active-directory/).
- Yapılandırmak istediğiniz dizin için aşağıdaki yönetici hesaplarından biri:
    - Genel yönetici
    - Güvenlik yöneticisi
    - Koşullu erişim yöneticisi

## <a name="terms-of-use-document"></a>Kullanım koşulları belgesi

Azure AD Kullanım Koşulları, içerik sunmak için PDF biçimini kullanır. PDF dosyasının, örneğin mevcut sözleşme belgeleri, son kullanıcı oturum sırasında kullanıcı sözleşmelerini toplamanıza olanak sağlayan herhangi bir içerik olabilir. PDF'te önerilen yazı tipi boyutu 24'tür.

## <a name="add-terms-of-use"></a>Kullanım Koşulları ekleme
Kullanım Koşulları belgenize son şeklini verdikten sonra, bunları eklemek için aşağıdaki yordamı kullanın.

1. Azure'da Genel yönetici, Güvenlik yöneticisi veya Koşullu erişim yöneticisi olarak oturum açın.

1. **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

    ![Kullanım koşulları dikey penceresi](./media/active-directory-tou/tou-blade.png)

1. **Yeni koşullar**'a tıklayın.

    ![Kullanım Koşullarını ekleme](./media/active-directory-tou/new-tou.png)

1. Kullanım Koşulları için bir **Ad** girin

2. **Görünen ad** girin.  Kullanıcılar oturum açtıklarında bu üst bilgiyi görür.

3. Kullanım Koşullarınızın son halinin bulunduğu PDF belgesine **göz atın** ve bunu seçin.

4. Kullanım Koşulları için bir dil **seçin**.  Dil seçeneğini kullanarak her biri farklı dilde olan birden fazla Kullanım Koşulları belgesini yükleyebilirsiniz.  Bir son kullanıcının göreceği Kullanım Koşulları sürümü, kullanıcının tarayıcı tercihlerine bağlıdır.

5. **Kullanıcıların kullanım koşullarını genişletmesini gerekli kıl** için Açık veya Kapalı seçeneğini belirleyin.  Bu ayar Açık olarak belirlenirse, son kullanıcıların Kullanım Koşullarını kabul etmeden önce görüntülemesi gerekir.

6. **Koşullu Erişim** bölümünde, bir özel koşullu erişim ilkesi veya açılır listeden bir şablon seçerek karşıya yüklenen Kullanım Koşullarını **Zorunlu Kılabilirsiniz**.  Özel koşullu erişim ilkeleri, belirli bulut uygulamaları veya kullanıcı gruplarına kadar ayrıntılı Kullanım Koşulları uygulamanıza olanak sağlar.  Daha fazla bilgi için bkz. [Koşullu erişim ilkelerini yapılandırma](../../cognitive-services/qnamaker/concepts/best-practices.md).

    >[!IMPORTANT]
    >Koşullu erişim ilkesi denetimleri (Kullanım Koşulları dahil), hizmet hesaplarında uygulamayı desteklemez.  Tüm hizmet hesaplarının koşullu erişim ilkesinden hariç tutulması önerilir.

7. **Oluştur**’a tıklayın.

8. Özel bir koşullu erişim şablonu seçtiyseniz, koşullu erişim ilkesini özelleştirmenize olanak sağlayan yeni bir ekran görüntülenir.

    Şimdi yeni Kullanım Koşullarınızı görürsünüz.

    ![Kullanım Koşullarını ekleme](./media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Kimin kabul ve reddedilen, raporu görüntüle
Kullanım Koşulları dikey penceresinin kabul eden ve reddeden kullanıcı sayısını gösterdiğini fark edeceksiniz. Bu sayılar ve Kullanım Koşullarını kabul eden/reddeden kullanıcılar, Kullanım Koşulları özelliğini kullandığınız süre boyunca depolanır.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

    ![Denetim Olayı](./media/active-directory-tou/view-tou.png)

1. Kullanıcıların geçerli durumunu görüntülemek için **Kabul edilenler** veya **Reddedilenler** bölümündeki sayılara tıklayın.

    ![Denetim Olayı](./media/active-directory-tou/accepted-tou.png)

## <a name="view-azure-ad-audit-logs"></a>Görünüm Azure AD denetim günlükleri
Daha fazla etkinlik görüntülemek isterseniz Azure AD Kullanım Koşulları denetim günlüklerini inceleyebilirsiniz. Her kullanıcı onayı 30 gün saklanan bir olay denetim günlüklerinde tetikler. Bu günlükleri portalda görüntüleyebilir veya .csv dosyası olarak indirebilirsiniz.

Azure AD ile kullanmaya başlamak için Denetim günlükleri, aşağıdaki yordamı kullanın:

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

1. **Denetim günlüklerini görüntüle**'ye tıklayın.

    ![Denetim Olayı](./media/active-directory-tou/audit-tou.png)

1. Azure AD denetim günlükleri ekranında sağlanan açılan listeleri kullanarak belirli denetim günlüğü bilgilerini hedeflemek için bilgileri filtreleyebilirsiniz.

    ![Denetim Olayı](./media/active-directory-tou/audit-logs-tou.png)

1. Ayrıca **İndir**'e tıklayarak bilgileri yerel olarak kullanmak üzere bir .csv dosyasında indirebilirsiniz.

## <a name="what-terms-of-use-looks-like-for-users"></a>Kullanım Koşullarının kullanıcılara görünüşü
Bir kullanım koşulları belgesi oluşturulup uygulandığında sonra kapsam dahilindeki kullanıcılar, oturum açma sırasında aşağıdaki ekranı görürsünüz.

![Denetim Olayı](./media/active-directory-tou/user-tou.png)

Aşağıdaki ekranda Kullanım Koşulları belgesinin mobil cihazlarda nasıl göründüğü gösterilmiştir.

![Denetim Olayı](./media/active-directory-tou/mobile-tou.png)

Kullanıcılar yalnızca kullanım koşulları bir kez kabul etmesi gerekir ve oturum açmalar yeniden üzerinde sonraki kullanım koşullarını görmezsiniz.

### <a name="how-users-can-review-their-terms-of-use"></a>Kullanıcılar kendi Kullanım Koşullarını nasıl gözden geçirebilir?
Kullanıcılar, kabul ettikleri kullanım koşullarını gözden geçirip incelemek için aşağıdaki yordamı kullanabilir.

1. Oturum [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

1. Sağ üst köşede adınıza tıklayın ve açılır menüden **Profil**'i seçin.

    ![Profil](./media/active-directory-tou/tou14.png)

1. Profil sayfanızda **Kullanım koşullarını gözden geçir**'e tıklayın.

    ![Denetim Olayı](./media/active-directory-tou/tou13a.png)

1. Buradan, kabul ettiğiniz Kullanım Koşullarını gözden geçirebilirsiniz. 

## <a name="edit-terms-of-use-details"></a>Kullanım koşulları ayrıntılarını düzenleyin
Kullanım Koşulları'nın bazı ayrıntıları düzenleyebilirsiniz ancak var olan bir belgeyi değiştiremez. Aşağıdaki yordam ayrıntılarının nasıl düzenleneceğini açıklar.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

1. Düzenlemek istediğiniz kullanım koşullarını seçin.

1. Tıklayın **koşulları Düzenle**.

1. Kullanım bölmesi düzenleme koşullarını adı, görünen ad veya kullanıcıların değerleri genişletmesini gerekli kıl.

    ![Kullanım Koşullarını ekleme](./media/active-directory-tou/edit-tou.png)

1. Tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.

    Değişiklikleri kaydettikten sonra kullanıcıların yeni koşulları yeniden kabul etmesini gerektirmek gerekir.

## <a name="add-a-terms-of-use-language"></a>Dil kullanım koşullarını ekleme
Aşağıdaki yordam, dil kullanım koşullarını eklemeyi açıklar.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

1. Düzenlemek istediğiniz kullanım koşullarını seçin.

1. Ayrıntılar bölmesinden **dilleri** sekmesi.

    ![Kullanım Koşullarını ekleme](./media/active-directory-tou/languages-tou.png)

1. Tıklayın **dil Ekle**.

1. Kullanım dil bölmesi ekleme koşullarını yerelleştirilmiş PDF'niz karşıya yükleme ve dil seçin.

    ![Kullanım Koşullarını ekleme](./media/active-directory-tou/language-add-tou.png)

1. Tıklayın **Ekle** dil eklemek için.

## <a name="delete-terms-of-use"></a>Kullanım Koşullarını silme
Aşağıdaki yordamı kullanarak eski Kullanım Koşullarını silebilirsiniz:

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

1. Kaldırmak istediğiniz Kullanım Koşullarını seçin.

1. **Koşulları sil**'e tıklayın.

1. Devam etmek isteyip istemediğinizi soran iletide **Evet**'e tıklayın.

    ![Kullanım Koşullarını ekleme](./media/active-directory-tou/delete-tou.png)

    Bu işlemden sonra Kullanım Koşullarınızı görmezsiniz.

## <a name="deleted-users-and-active-terms-of-use"></a>Silinen kullanıcılar ve etkin Kullanım Koşulları
Varsayılan olarak, silinmiş bir kullanıcı Azure AD'de 30 gün boyunca silinmiş durumda kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir.  30 gün sonra bu kullanıcı kalıcı olarak silinir.  Ayrıca, bir Genel yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md).  Bir kullanıcı kalıcı olarak silindikten sonra, bu kullanıcıya ilişkin sonraki veriler etkin Kullanım Koşullarından kaldırılır.  Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="policy-changes"></a>İlke değişiklikleri
Koşullu erişim ilkeleri hemen etkili olur. Bu durumda, yönetici "Üzgün Bulutlar" veya "Azure AD belirteç sorunları" görmek başlatılır. Yönetici, oturumu kapatın ve yeni ilkeyi karşılamak için yeniden oturum açın.

>[!IMPORTANT]
> Aşağıdaki durumlarda kapsam dahilindeki kullanıcıların yeni bir ilkeyi karşılamak için oturumu kapatıp yeniden oturum açmaları gerekir:
> - Kullanım Koşullarında bir koşullu erişim ilkesi etkinleştirildiğinde
> - veya ikinci bir Kullanım Koşulları belgesi oluşturulduğunda

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Kullanıcının Kullanım Koşullarını kabul edip etmediğini veya ne zaman kabul ettiğini nasıl görebilirim?**</br>
Y: üzerinde koşulları kullanım dikey penceresinin altındaki sayıya tıklayın **kabul edilen**. Ayrıca görüntüleyebilir veya Azure AD'de arama kabul etkinliğine denetim günlükleri. Daha fazla bilgi için [kimin kabul ve reddedilen, raporu görüntüle](#view-who-has-accepted-and-declined) ve [görünümü Azure AD denetim günlüklerini](#view-azure-ad-audit-logs).
 
**S: Bilgiler ne kadar süreyle depolanır?**</br>
C: kullanıcının kullanım raporu ve kimin kabul ve reddedilen kullanım koşulları süresince depolanan şartları sayar. Azure AD denetim günlükleri, 30 gün boyunca saklanır.

**S: neden Azure AD karşılaştırması kullanım raporunun Koşulları'nda bir onayları farklı sayıda denetim günlükleri görüyorum?**</br>
C: kullanım raporu koşullarını ömrü boyunca, Azure AD denetim günlükleri 30 gün boyunca saklanır, kullanım koşullarını depolanır. Ayrıca, kullanım raporu koşulları yalnızca görüntüler kullanıcıların geçerli onayı durumu. Örneğin, bir kullanıcı azalma ve ardından kabul eder, kullanım raporu koşulları yalnızca bu kullanıcının gösterir kabul edin. Geçmişini görmek gerekirse, Azure AD kullanabilirsiniz. Denetim günlükleri.

**S: Ayrıntılar için kullanım koşullarını düzenleyebilirim, kullanıcıların yeniden kabul etmesini gerektiriyor mu?**</br>
C: Evet, bir yönetici kullanım koşulları için ayrıntıları düzenlerse, kullanıcıların yeni koşulları yeniden kabul etmesini gerektirmek olmasını gerektirir.

**Ben bir var olan kullanım koşulları belgesi güncelleştirebilir miyim?**</br>
Y: şu anda bir var olan kullanım koşulları belgesi güncelleştirilemiyor. Bir kullanım koşulları belgesi değiştirmek için yeni bir kullanım örneği koşullarını oluşturmanız gerekir.

**S: köprüler kullanım koşulları PDF belgesi içinde ise son kullanıcılar bunları'ye erişebilirler mi?**</br>
Y: köprüler tıklanabilir olmadıklarından PDF bir JPEG, varsayılan olarak işlenir. Kullanıcıların tercih yapma seçeneğine sahip **görüntüleme konusunda sorun mu yaşıyorsunuz? Buraya**, işleyen PDF dosyasını yerel olarak köprüler burada desteklenir.

**S: Bir Kullanım Koşulları belgesi birden çok dili destekleyebilir mi?**</br>
C: Evet. Şu anda bir yöneticinin tek bir yapılandırabilir 108 farklı dillerde mevcuttur kullanım koşulları.

**S: Kullanım Koşulları ne zaman tetiklenir?**</br>
C: Kullanım Koşulları oturum açma deneyimi sırasında tetiklenir.

**S: Kullanım Koşullarını hangi uygulamalara hedefleyebilirim?**</br>
C: Modern kimlik doğrulaması kullanarak kurumsal uygulamalar üzerinde bir koşullu erişim ilkesi oluşturabilirsiniz.  Daha fazla bilgi için bkz. [Kurumsal uygulamalar](./../manage-apps/view-applications-portal.md).

**S: Belirli bir kullanıcı veya uygulamaya birden çok Kullanım Koşulları belgesi ekleyebilir miyim?**</br>
C: Evet, bu grup veya uygulamaları hedefleyen birden çok koşullu erişim ilkesi oluşturarak bunu gerçekleştirebilirsiniz. Birden çok Kullanım Koşulları belgesinin kapsamında olan bir kullanıcı, bunları tek tek kabul eder.
 
**S: Bir kullanıcı Kullanım Koşullarını reddederse ne olur?**</br>
C: Kullanıcının uygulamaya erişimi engellenir. Kullanıcı yeniden oturum açın ve erişmek için koşulları kabul etmesi gerekir.
 
**S: kullanım koşulları daha önce kabul edildi unaccept mümkün mü?**</br>
Y: yapabilecekleriniz [gözden geçirme, daha önce kullanım koşullarını kabul](#how-users-can-review-their-terms-of-use), ancak şu anda unaccept bir yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory’de koşullu erişim en iyi uygulamaları](../../cognitive-services/qnamaker/concepts/best-practices.md)