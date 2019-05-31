---
title: Kullanımı - Azure Active Directory kullanım koşulları | Microsoft Docs
description: Azure Active Directory erişim almadan önce çalışan veya konuklar için bilgi sunmak için Kullanım Koşulları'nı kullanmaya başlayın.
services: active-directory
ms.service: active-directory
ms.subservice: compliance
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1abae0a454e17e8f633f68bc5853bfb4a4b24d14
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383169"
---
# <a name="azure-active-directory-terms-of-use"></a>Azure Active Directory kullanım koşulları

Azure AD kullanım koşulları, kuruluşların son kullanıcılara bilgi sunmak için kullanabileceği basit bir yöntem sağlar. Bu sunum, kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlar. Bu makalede kullanım koşulları ile çalışmaya başlama işlemini açıklamaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Genel Bakış videoları

Aşağıdaki video, kullanım koşullarını hızlı bir genel bakış sağlar.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Videolar için bkz:
- [Azure Active Directory Kullanım Koşulları'nı dağıtma](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Azure Active Directory kullanım koşulları kullanıma sunma](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Kullanım Koşulları ile ne yapabilirim?

Azure AD kullanım koşulları aşağıdaki özelliklere sahiptir:

- Çalışanların veya konuklar kullanım erişim sağlamadan önce koşullarınızı kabul etmek gereklidir.
- Çalışanların veya konuklar, her cihazda erişim sağlamadan önce kullanım koşullarını kabul etmek gerektirir.
- Çalışanların veya konuklar, yinelenen bir zamanlamaya göre kullanım koşullarını kabul etmek gerektirir.
- Çalışanların veya konuklar, Azure multi-Factor Authentication (MFA) güvenlik bilgilerini kaydetmeden önce kullanım koşullarını kabul etmek gerektirir.
- Azure AD Self Servis parola sıfırlama (SSPR) güvenlik bilgilerini kaydetmeden önce kullanım koşullarınızı kabul etmek çalışanların gerektirir.
- Kuruluşunuzdaki tüm kullanıcılar için genel koşulları sunar.
- Belirli bir kullanıcı özniteliklerine (ör. dayalı kullanım koşulları sunar doktorlarla hemşirelere ya da yurtiçi ve uluslararası çalışanlara, [dinamik grupları](../users-groups-roles/groups-dynamic-membership.md) kullanarak) sunun.
- Belirli kullanım koşulları, Salesforce gibi yüksek iş etkisi uygulamalarına erişirken sunar.
- Kullanım koşulları farklı dillerde sunar.
- Sahip veya bu, kullanım koşullarını kabul edilmemiş listesi.
- Gizlilik düzenlemeleriyle Karşılama konusunda yardımcı olur.
- Uyumluluk ve denetim için kullanım etkinlik koşulları günlüğünü görüntüleyin.
- Oluşturma ve yönetme kullanarak kullanım koşullarını [Microsoft Graph API'lerini](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (şu anda önizlemede).

## <a name="prerequisites"></a>Önkoşullar

Azure AD kullanım koşulları yapılandırmak ve kullanmak için şunlara sahip olmalısınız:

- Azure AD Premium P1, P2, EMS E3 veya EMS E5 aboneliği.
   - Bu aboneliklerden birine sahip değilseniz [Azure AD Premium'u alabilir](../fundamentals/active-directory-get-started-premium.md) veya [Azure AD Premium deneme sürümünü etkinleştirebilirsiniz](https://azure.microsoft.com/trial/get-started-active-directory/).
- Yapılandırmak istediğiniz dizin için aşağıdaki yönetici hesaplarından biri:
   - Genel Yönetici
   - Güvenlik Yöneticisi
   - Koşullu Erişim Yöneticisi

## <a name="terms-of-use-document"></a>Kullanım koşulları belgesi

Azure AD kullanım koşulları, içerik sunmak için PDF biçimini kullanır. Bu PDF dosyası, kullanıcıların oturum açtığı sırada son kullanıcı sözleşmelerini toplamanıza olanak sağlayan herhangi bir içerik (örneğin mevcut sözleşme belgeleri) olabilir. Kullanıcıların mobil aygıtları desteklemek için PDF olarak önerilen yazı tipi boyutu 24 noktasıdır.

## <a name="add-terms-of-use"></a>Kullanım koşulları ekleme

Kullanım koşulları belgesi koşullarınıza son şeklini verdikten sonra bunu eklemek için aşağıdaki yordamı kullanın.

1. Azure'da oturum aç bir genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi.
1. **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

   ![Kullanım koşulları dikey penceresi](./media/terms-of-use/tou-blade.png)

1. **Yeni koşullar**'a tıklayın.

   ![Kullanım Koşullarını ekleme](./media/terms-of-use/new-tou.png)

1. İçinde **adı** kutusuna, Azure portalında kullanılacak kullanım koşulları için bir ad girin.
1. İçinde **görünen ad** kutusunda, oturum kullanıcıların gördüğü başlık girin.
1. İçin **kullanım koşulları belgesi**gösterilmediğinden, kullanım koşulları için gözatın ve seçin.
1. Kullanım koşulları belgesi için dili seçin. Dil seçeneğini kullanarak her biri farklı dilde olan birden fazla kullanım koşulunu karşıya yükleyebilirsiniz. Bir son kullanıcının göreceği kullanım koşulları sürümü, kullanıcının tarayıcı tercihlerine bağlıdır.
1. Son kabul etmeden önce kullanım koşullarını görüntülemek kullanıcılar gerektirecek şekilde **kullanıcıların kullanım koşullarını genişletmesini gerekli kıl** için **üzerinde**.
1. Bunlar erişmesini her bir cihazdaki kullanım koşullarınızı kabul etmek son kullanıcıların gerektirecek şekilde **kullanıcıların her cihazda kabul etmesini zorunlu tut** için **üzerinde**. Daha fazla bilgi için [cihaz başına kullanım koşullarını](#per-device-terms-of-use).
1. Kullanım koşulları onayları bir zamanlamaya göre süresini doldurmak isteyip verilirse **onayları sona** için **üzerinde**. Üzerinde ayarlandığında, iki ek zamanlama ayarları görüntülenir.

   ![Onayların süresi dolsun](./media/terms-of-use/expire-consents.png)

1. Kullanma **sona erme başlangıç** ve **sıklığı** koşulları için bir zamanlama belirtmek için ayarları süresinin sona ermesinin kullanın. Aşağıdaki tabloda birkaç örnek ayarlara yönelik sonuçları gösterir:

   | Süre sonu başlangıcı: | Sıklık | Sonuç |
   | --- | --- | --- |
   | Bugünün tarihi  | Aylık | Bugünden itibaren kullanıcılar gerekir kullanım koşullarını kabul edin ve sonra her ay artırmasını. |
   | Tarih gelecekte  | Aylık | Bugünden itibaren kullanıcıların kullanım koşullarını kabul etmeniz gerekir. Gelecekteki bir tarihi gerçekleştiğinde onayları sona erer ve kullanıcılar her ay sonra artırmasını gerekir.  |

   Örneğin, sona erme tarihi başlangıç ayarlarsanız **Oca 1** ve sıklığını **aylık**, işte süresinin sona ermesinin iki kullanıcı için nasıl oluşabilir:

   | Kullanıcı | İlk tarih kabul edin | Önce sona eren tarihi | İkinci sona tarihi | Üçüncü sona tarihi |
   | --- | --- | --- | --- | --- |
   | Alice | Ocak 1 | 1 Şubat ' | 1 Mart'ta | Apr 1 |
   | Bob | Dönemlerinizin 15 Ocak | 1 Şubat ' | 1 Mart'ta | Apr 1 |

1. Kullanma **süreyi (gün) yeniden kabul gerektirir** ayarı önce kullanıcı kullanım koşullarını artırmasını gereken gün sayısını belirtin. Bu, kullanıcıların kendi zamanlamalarında izleyin olanak tanır. Örneğin, süresi ayarlayın **30** gün, işte süresinin sona ermesinin iki kullanıcı için nasıl oluşabilir:

   | Kullanıcı | İlk tarih kabul edin | Önce sona eren tarihi | İkinci sona tarihi | Üçüncü sona tarihi |
   | --- | --- | --- | --- | --- |
   | Alice | Ocak 1 | 31 Ocak'a kadar | Mar 2 | Apr 1 |
   | Bob | Dönemlerinizin 15 Ocak | 14 Şubat | 16 Mart | Apr 15 |

   Kullanmak mümkün mü **sona onayları** ve **süresi (gün) yeniden kabulü gerektiren önce** ayarları birlikte, ancak genellikle birini kullanın.

1. Altında **koşullu erişim**, kullanın **koşullu erişim ilkesi şablonu ile zorla** kullanım koşullarını uygulamak için şablonu seçin.

   ![Koşullu erişim şablonları](./media/terms-of-use/conditional-access-templates.png)

   | Şablon | Açıklama |
   | --- | --- |
   | **Tüm konuklar için bulut uygulamalarına erişim** | Tüm konuklar ve tüm koşullu erişim ilkesi oluşturulacak bulut uygulamaları. Bu ilke, Azure portalını etkiler. Bu oluşturulduktan sonra oturum kapatma ve oturum açma için gerekli olabilir. |
   | **Bulut uygulamalarında tüm kullanıcılar için erişim** | Koşullu erişim ilkesi oluşturulur, tüm kullanıcılar ve tüm bulut uygulamaları. Bu ilke, Azure portalını etkiler. Bu oluşturulduktan sonra oturum kapatma ve oturum açma için gerekli olacaktır. |
   | **Özel ilke** | Kullanıcılar, gruplar ve bu kullanım koşullarını uygulanacağı uygulamaları seçin. |
   | **Koşullu erişim ilkesini sonra oluştur** | Bu kullanım koşulları, koşullu erişim ilkesi oluşturulurken denetim verme listesinde görünür. |

   >[!IMPORTANT]
   >Koşullu erişim ilkesi denetimleri (kullanım koşulları dahil), hizmet hesaplarında uygulamayı desteklemez. Tüm hizmet hesaplarının koşullu erişim ilkesinden hariç tutulması önerilir.

    Ayrıntılı kullanım koşulları, belirli bulut uygulamaları veya kullanıcı grubu aşağı özel koşullu erişim ilkelerini etkinleştirin. Daha fazla bilgi için [hızlı başlangıç: Bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektiren](require-tou.md).

1. **Oluştur**’a tıklayın.

   Bir özel koşullu erişim şablonu seçtiyseniz, yeni bir ekran özel koşullu erişim ilkesi oluşturmak izin veren görüntülenir.

   ![Özel ilke](./media/terms-of-use/custom-policy.png)

   Şimdi yeni kullanım koşullarınızı görürsünüz.

   ![Kullanım Koşullarını ekleme](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Kimin kabul ve reddedilen, raporu görüntüle

Kullanım Koşulları dikey penceresinin kabul eden ve reddeden kullanıcı sayısını gösterdiğini fark edeceksiniz. Kullanım koşullarını süresince, bu sayıları ve kimin kabul/reddedilen depolanır.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

   ![Kullanım koşulları dikey penceresi](./media/terms-of-use/view-tou.png)

1. Kullanım koşulları için bölümündeki sayılara tıklayın **kabul edilen** veya **reddedildi** kullanıcılar için geçerli durumu görüntülemek için.

   ![Kullanım koşulları onayları](./media/terms-of-use/accepted-tou.png)

1. Bireysel kullanıcı geçmişini görüntülemek için üç nokta simgesine tıklayın ( **...** ) ve ardından **geçmişi görüntüleyebilir**.

   ![Görünüm geçmişi menüsü](./media/terms-of-use/view-history-menu.png)

   Tüm geçmiş görünümü geçmişi bölmesinde gördüğünüz kabul eder, reddeder ve süre sonu.

   ![Geçmiş bölmesini görüntüleyin](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Görünüm Azure AD denetim günlükleri

Ek etkinliğini görüntülemek istiyorsanız, Azure AD kullanım koşulları denetim günlükleri içerir. Her kullanıcı onayı tetikleyen bir olay için depolanan denetim günlüklerinde **30 gün**. Bu günlükleri portalda görüntüleyebilir veya .csv dosyası olarak indirebilirsiniz.

Azure AD ile kullanmaya başlamak için Denetim günlükleri, aşağıdaki yordamı kullanın:

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Kullanım Koşulları'nı seçin.
1. **Denetim günlüklerini görüntüle**'ye tıklayın.

   ![Kullanım koşulları dikey penceresi](./media/terms-of-use/audit-tou.png)

1. Azure AD denetim günlükleri ekranında, sağlanan hedef belirli denetim günlüğü bilgilerini listelerine kullanarak bilgileri filtreleyebilirsiniz.

   Ayrıca **İndir**'e tıklayarak bilgileri yerel olarak kullanmak üzere bir .csv dosyasında indirebilirsiniz.

   ![Denetim günlükleri](./media/terms-of-use/audit-logs-tou.png)

   Bir günlük tıklarsanız, bir bölmesi ile ek Etkinlik ayrıntıları görüntülenir.

   ![Etkinlik ayrıntıları](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Hangi kullanım koşullarına şekilde kullanıcılar için görünür.

Bir kullanım koşulları belgesi oluşturulup uygulandığında sonra kapsam dahilindeki kullanıcılar, oturum açma sırasında aşağıdaki ekranı görürsünüz.

![Kullanıcı web oturumu açma](./media/terms-of-use/user-tou.png)

Kullanıcıların kullanım koşullarını görüntüleyin ve gerekirse, yakınlaştırma düğmelerini.

![Yakınlaştırma düğmeleri kullanım koşullarını görüntüle](./media/terms-of-use/zoom-buttons.png)

Aşağıdaki ekranda, kullanım koşullarını mobil cihazlarda nasıl göründüğünü gösterir.

![Kullanıcı mobil oturum açma](./media/terms-of-use/mobile-tou.png)

Kullanıcılar yalnızca kullanım koşulları bir kez kabul etmesi gerekir ve bunların kullanım koşullarını sonraki oturum açma işlemleri üzerinde görmezsiniz.

### <a name="how-users-can-review-their-terms-of-use"></a>Kullanıcıların nasıl kendi kullanım koşullarını gözden geçirebilirsiniz

Kullanıcılar, gözden geçirin ve aşağıdaki yordamı kullanarak kabul ettikleri kullanım koşullarını bakın.

1. [https://myapps.microsoft.com](https://myapps.microsoft.com) adresinde oturum açın.
1. Sağ üst köşede adınıza tıklayın ve seçin **profili**.

   ![Profil](./media/terms-of-use/tou14.png)

1. Profil sayfanızda **Kullanım koşullarını gözden geçir**'e tıklayın.

   ![Profil - kullanım koşullarını gözden geçirin](./media/terms-of-use/tou13a.png)

1. Buradan, kabul ettiğiniz kullanım koşullarını gözden geçirebilirsiniz.

## <a name="edit-terms-of-use-details"></a>Kullanım koşulları ayrıntılarını düzenleyin

Kullanım Koşulları'nın bazı ayrıntıları düzenleyebilirsiniz ancak var olan bir belgeyi değiştiremez. Aşağıdaki yordam ayrıntılarının nasıl düzenleneceğini açıklar.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Düzenlemek istediğiniz kullanım koşullarını seçin.
1. Tıklayın **koşulları Düzenle**.
1. Kullanım bölmesi düzenleme koşullarını adı, görünen ad veya kullanıcıların değerleri genişletmesini gerekli kıl.

   PDF belgesi gibi değiştirmek istediğiniz diğer ayarları varsa, kullanıcıların her cihazda onay, onayları, sona zorunlu süresi reacceptance veya koşullu erişim ilkesi önce yeni bir kullanım koşulları belgesi oluşturmanız gerekir.

   ![Kullanım koşullarını düzenle](./media/terms-of-use/edit-tou.png)

1. Tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.

   Değişiklikleri kaydettikten sonra kullanıcılar bu düzenlemeler artırmasını yoktur.

## <a name="add-a-terms-of-use-language"></a>Dil kullanım koşullarını ekleme

Aşağıdaki yordam, dil kullanım koşullarını eklemeyi açıklar.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Düzenlemek istediğiniz kullanım koşullarını seçin.
1. Ayrıntılar bölmesinden **dilleri** sekmesi.

   ![Kullanım Koşullarını ekleme](./media/terms-of-use/languages-tou.png)

1. Tıklayın **dil Ekle**.
1. Kullanım dil bölmesi ekleme koşullarını yerelleştirilmiş PDF'niz karşıya yükleme ve dil seçin.

   ![Kullanım Koşullarını ekleme](./media/terms-of-use/language-add-tou.png)

1. Tıklayın **Ekle** dil eklemek için.

## <a name="per-device-terms-of-use"></a>Cihaz başına kullanım koşulları

**Kullanıcıların her cihazda kabul etmesini zorunlu tut** ayar, son kullanıcıların bunlar erişmesini her bir cihazdaki kullanım koşullarınızı kabul etmesini gerektirmek etkinleştirir. Son kullanıcı cihazlarını Azure AD'ye katılmak için gerekli olacaktır. Cihazı alanına katıldığında, cihaz kimliği her cihazda kullanım koşullarını uygulamak için kullanılır.

Yazılım ve desteklenen platformlar listesi aşağıda verilmiştir.

> [!div class="mx-tableFixed"]
> |  | iOS | Android | Windows 10 | Diğer |
> | --- | --- | --- | --- | --- |
> | **Yerel uygulama** | Evet | Evet | Evet |  |
> | **Microsoft Edge** | Evet | Evet | Evet |  |
> | **Internet Explorer** | Evet | Evet | Evet |  |
> | **Chrome (uzantısı ile)** | Evet | Evet | Evet |  |

Cihaz başına kullanım koşulları aşağıdaki sınırlamalara sahiptir:

- Bir cihaz, yalnızca tek bir kiracı katılabilir.
- Bir kullanıcının kendi cihazı alanına katma izinleri olması gerekir.
- Intune kaydı uygulaması desteklenmez.
- Azure AD B2B kullanıcıları desteklenmez.

Kullanıcının cihazı alanına katılmamışsa, bunların cihazlarını katılmak için ihtiyaç duydukları bir ileti alırsınız. Deneyimlerini platform ve yazılım bağımlı olacaktır.

### <a name="join-a-windows-10-device"></a>Windows 10 cihazını ekleme

Bir kullanıcı Windows 10 ve Microsoft Edge kullanıyorsanız, aşağıdakine benzer bir ileti alırsınız [cihazını katılın](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device).

![Windows 10 ve Microsoft Edge - birleştirme cihaz istemi](./media/terms-of-use/per-device-win10-edge.png)

Chrome kullanıyorsanız, yüklemeniz istenir [Windows 10 hesapları uzantısı](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="browsers"></a>Tarayıcılar

Bir kullanıcı desteklenmeyen bir tarayıcı kullanıyorsanız, farklı bir tarayıcı kullanın istenir.

![Desteklenmeyen tarayıcı](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Kullanım koşullarını silme

Eski aşağıdaki yordamı kullanarak kullanım koşullarını silebilirsiniz.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Kaldırmak istediğiniz kullanım koşullarını seçin.
1. **Koşulları sil**'e tıklayın.
1. Devam etmek isteyip istemediğinizi soran iletide **Evet**'e tıklayın.

   ![Kullanım koşullarını silme](./media/terms-of-use/delete-tou.png)

   Artık, kullanım koşullarınızı görürsünüz.

## <a name="deleted-users-and-active-terms-of-use"></a>Silinmiş kullanıcılar ve etkin kullanım koşulları

Varsayılan olarak, silinmiş bir kullanıcı Azure AD'de 30 gün boyunca silinmiş durumda kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir. 30 gün sonra bu kullanıcı kalıcı olarak silinir. Ayrıca, bir Genel Yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md). Bir kullanıcının kalıcı olarak silindi, bu kullanıcı ile ilgili sonraki veri etkin kullanım koşullarını kaldırılacak. Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="policy-changes"></a>İlke değişiklikleri

Koşullu erişim ilkeleri hemen etkili olur. Bu durumda, yönetici "Üzgün Bulutlar" veya "Azure AD belirteç sorunları" görmek başlatılır. Yönetici, oturumu kapatın ve yeni ilkeyi karşılamak için yeniden oturum açın.

> [!IMPORTANT]
> Aşağıdaki durumlarda kapsam dahilindeki kullanıcıların yeni bir ilkeyi karşılamak için oturumu kapatıp yeniden oturum açmaları gerekir:
>
> - Kullanım koşullarında bir koşullu erişim ilkesi etkinleştirildiğinde
> - veya ikinci bir kullanım koşulları belgesi oluşturulduğunda

## <a name="b2b-guests-preview"></a>B2B Konukları (Önizleme)

Çoğu kuruluş, çalışanlarının, kuruluşlarının koşullarını kullanımı ve gizlilik bildirimlerini kabul yürürlükte olan bir işlem var. Ancak nasıl zorunlu aynı bir onayları için Azure AD iş işletmeden işletmeye (B2B) konukların, SharePoint veya Teams eklenen? Koşullu erişim ve kullanım koşullarını kullanarak, B2B Konuk kullanıcıları doğrudan doğru bir ilke uygulayabilir. Davet kullanım akışı sırasında kullanıcı kullanım koşullarını sunulur. Bu destek, şu anda Önizleme aşamasındadır.

Kullanım koşulları, yalnızca kullanıcının Azure AD'de bir Konuk hesabı olduğunda görüntülenir. SharePoint Online şu anda sahip bir [geçici dış paylaşım alıcı deneyimi](/sharepoint/what-s-new-in-sharing-in-targeted-release) bir belge veya kullanıcının bir Konuk hesabı gerektirmeyen bir klasör paylaşma. Bu durumda, kullanım koşulları görüntülenmez.

![Tüm konuk kullanıcılar](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps-preview"></a>Bulut uygulamaları (Önizleme) desteği

Kullanım koşulları, Azure Information Protection ve Microsoft Intune gibi farklı bulut uygulamaları için kullanılabilir. Bu destek, şu anda Önizleme aşamasındadır.

### <a name="azure-information-protection"></a>Azure Information Protection

Azure Information Protection uygulaması için bir koşullu erişim ilkesi yapılandırın ve bir kullanıcı, korumalı bir belge eriştiğinde bir kullanım koşulları belgesi gerektirir. Bu, bir kullanıcı ilk kez korumalı bir belge erişme önce kullanım koşulları tetikler.

![Azure Information Protection bulut uygulaması](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Microsoft Intune kaydı

Microsoft Intune kaydı uygulama için bir koşullu erişim ilkesini yapılandırma ve ıntune'da bir cihaz kaydetmeden önce kullanım koşulları gerektirir. Daha fazla bilgi için bkz: Okuma [koşulları, kuruluş blog gönderisi için çözüm seçme hakkını](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

![Microsoft Intune bulut uygulaması](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> Intune kaydı uygulama için desteklenmiyor [cihaz başına kullanım koşullarını](#per-device-terms-of-use).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Nasıl görebilirim ne zaman / kullanıcı kullanım koşullarını kabul ettiğini?**<br />
Y: Kullanım dikey koşullarınızda altındaki sayıya tıklayın **kabul edilen**. Ayrıca görüntüleyebilir veya Azure AD'de arama kabul etkinliğine denetim günlükleri. Daha fazla bilgi için bkz: görüntüleme kimin kabul ve reddedilen rapor ve [görünümü Azure AD denetim günlüklerini](#view-azure-ad-audit-logs).

**S: Ne kadar süreyle depolanan bilgilerin mi?**<br />
Y: Kullanım raporu ve kimin kabul ve reddedilen kullanım koşulları süresince depolanan şartları kullanıcı sayar. Azure AD denetim günlükleri, 30 gün boyunca saklanır.

**S: Denetim günlükleri koşullarını ve Azure AD kullanım raporunun bir onayları farklı sayıda neden görüyorum?**<br />
Y: Rapor kullanım koşullarını ömrü boyunca, Azure AD denetim günlükleri 30 gün boyunca saklanır, kullanım koşullarını depolanır. Ayrıca, kullanım raporu koşulları yalnızca görüntüler kullanıcıların geçerli onayı durumu. Örneğin, bir kullanıcı azalma ve ardından kabul eder, kullanım raporu koşulları yalnızca bu kullanıcının gösterir kabul edin. Geçmişini görmek gerekirse, Azure AD kullanabilirsiniz. Denetim günlükleri.

**S: Ayrıntılar için kullanım koşullarını düzenlerseniz, kullanıcıların yeniden kabul etmesini gerektiriyor mu?**<br />
Y: Hayır, bir yönetici kullanım koşulları için ayrıntıları düzenlerse (adı, görünen ad, kullanıcıların genişletmesini gerekli kıl veya bir dil Ekle), kullanıcıların yeni koşulları yeniden kabul etmesini gerektirmek gerektirmez.

**S: Bir mevcut kullanım koşulları belgesi güncelleştirebilirim?**<br />
Y: Şu anda bir var olan kullanım koşulları belgesi güncelleştirilemiyor. Bir kullanım koşulları belgesi değiştirmek için yeni bir kullanım örneği koşullarını oluşturmanız gerekir.

**S: Köprüler kullanım koşulları PDF belgesi içinde ise son kullanıcılar bunları'ye erişebilirler mi?**<br />
Y: Köprüler tıklanabilir olmadıklarından PDF bir JPEG, varsayılan olarak işlenir. Kullanıcıların tercih yapma seçeneğine sahip **görüntüleme konusunda sorun mu yaşıyorsunuz? Buraya**, işleyen PDF dosyasını yerel olarak köprüler burada desteklenir.

**S: Bir kullanım koşulları belgesi birden çok dili destekleyebilir mi?**<br />
Y: Evet. Şu anda bir yöneticinin tek bir yapılandırabilir 108 farklı dillerde mevcuttur kullanım koşulları. Bir yönetici, birden çok PDF belgeleri karşıya yüklemesine ve bu belgelerle (en fazla 108) karşılık gelen bir dil etiketi. Son kullanıcılar oturum açtığında, biz kendi tarayıcı dil tercihi arayın ve eşleşen belge görüntüler. Eşleşme yoksa, biz karşıya yüklenen belge ilk varsayılan belgeyi görüntüler.

**S: Tetiklenen kullanım koşulları ne zaman uygulanır?**<br />
Y: Kullanım koşulları oturum açma deneyimi sırasında tetiklenir.

**S: Kullanım koşullarını hangi uygulamalara hedefleyebilirim?**<br />
Y: Modern kimlik doğrulaması kullanarak kurumsal uygulamalar üzerinde bir koşullu erişim ilkesi oluşturabilirsiniz. Daha fazla bilgi için bkz. [Kurumsal uygulamalar](./../manage-apps/view-applications-portal.md).

**S: Birden çok kullanım koşulları belirli bir kullanıcı veya uygulama için ekleyebilir miyim?**<br />
Y: Evet, bu grup veya uygulamaları hedefleyen birden çok koşullu erişim ilkesi oluşturarak. Bir kullanıcı birden çok kullanım koşulları kapsamında denk gelirse, tek bir zaman kullanım koşullarını kabul edin.

**S: Bir kullanıcı kullanım koşullarını reddederse ne olur?**<br />
Y: Kullanıcının uygulamaya erişimi engellenir. Kullanıcı yeniden oturum açın ve erişmek için koşulları kabul etmesi gerekir.

**S: Daha önce kabul edildi kullanım koşullarını unaccept mümkündür?**<br />
Y: Yapabilecekleriniz [gözden geçirme, daha önce kullanım koşullarını kabul](#how-users-can-review-their-terms-of-use), ancak şu anda unaccept bir yolu yoktur.

**S: Intune hüküm ve koşulları kullanma da ne olur?**<br />
Y: Her iki Azure AD Kullanım Koşulları'nı yapılandırdıysanız ve [Intune hüküm ve koşulları](/intune/terms-and-conditions-create), kullanıcının her ikisini de kabul etmek için gerekli. Daha fazla bilgi için [koşulları, kuruluş blog gönderisi için çözüm seçme hakkını](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

**S: Hangi uç noktaları, kimlik doğrulaması için hizmet kullanım koşullarını kullanıyor mu?**<br />
Y: Kullanım koşulları aşağıdaki uç noktaların kimlik doğrulaması için kullanır: https://tokenprovider.termsofuse.identitygovernance.azure.com ve https://account.activedirectory.windowsazure.com. Kuruluşunuzun URL'leri kayıt için bir izin verilenler varsa, bu uç noktalar Azure AD'ye uç noktaları ile birlikte izin verilenler listenize oturum eklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektirir](require-tou.md)
- [Azure Active Directory’de koşullu erişim en iyi uygulamaları](best-practices.md)
