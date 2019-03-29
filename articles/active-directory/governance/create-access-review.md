---
title: Gruplar veya uygulamalar - Azure Active Directory erişim gözden geçirmesi oluştur | Microsoft Docs
description: Erişim gözden geçirmesi grubu üyeleri veya uygulama erişimi Azure Active Directory erişim gözden geçirmelerine oluşturmayı öğrenin.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e25af938d09a254abd5d28ca3a5eecca2d3f8f1
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576216"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Grupları bir erişim gözden geçirmesi oluşturma veya uygulamaları Azure ad erişim gözden geçirmeleri

Gruplara ve uygulamalara çalışanlar ve konuklar için erişim, zaman içinde değişir. Eski erişim atamalarını ile ilişkili riski azaltmak için Yöneticiler Azure Active Directory (Azure AD) erişim gözden geçirmeleri grubu üyeleri veya uygulama erişimi için oluşturmak için kullanabilirsiniz. Düzenli bir şekilde erişim gözden geçirmek gerekiyorsa, yinelenen erişim gözden geçirmeleri oluşturabilirsiniz. Bu senaryolar hakkında daha fazla bilgi için bkz: [kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md) ve [konuk erişimini yönetme](manage-guest-access-with-access-reviews.md).

Bu makalede, grubu üyeleri veya uygulama erişimi için bir veya daha fazla erişim gözden geçirmeleri oluşturmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar

- [Erişim gözden geçirmeleri etkin](access-reviews-overview.md)
- Genel yönetici veya Kullanıcı Yöneticisi

## <a name="create-one-or-more-access-reviews"></a>Bir veya daha fazla erişim gözden geçirmesi oluşturma

1. Oturum açma için Azure portalını ve açık [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Tıklayın **denetimleri**.

1. Tıklayın **yeni erişim gözden geçirmesi** yeni bir erişim gözden geçirmesi oluşturma.

    ![Erişim gözden geçirmesi - denetimler](./media/create-access-review/controls.png)

1. Erişim gözden geçirmesi adı. İsteğe bağlı olarak, gözden geçirme bir açıklama girin. Ad ve açıklama gözden geçirenlere gösterilmektedir.

    ![Erişim gözden geçirmesi - gözden geçirme adı ve açıklaması oluştur](./media/create-access-review/name-description.png)

1. Ayarlama **başlangıç tarihi**. Varsayılan olarak, erişim gözden geçirmesi bir kez gerçekleşir, aynı oluşturulduğunda başlar ve bir ay içinde sonlandırır. Başlangıç değiştirebilirsiniz ve istediğiniz sayıda gün ancak erişim sağlamak için bitiş tarihi başlangıç gelecekte ve son gözden geçirin.

    ![Bir erişim gözden geçirmesi - oluşturma başlangıç ve bitiş tarihleri](./media/create-access-review/start-end-dates.png)

1. Erişim gözden geçirme yineleme yapmak için değiştirme **sıklığı** ayarını **bir kez** için **haftalık**, **aylık**,  **Üç aylık** veya **yıllık**ve **süresi** kaç gün yinelenen serisinin her incelenmesi gereken gözden geçirenler girişten açık tanımlamak için kaydırıcı veya metin kutusu. Örneğin, bir aylık gözden geçirilmek üzere ayarlayabileceğiniz en uzun süre 27 incelemeleri çakışan önlemek için gündür.

1. Kullanım **son** yinelenen erişim sonlandırma belirtmek için ayarı serisini gözden geçirin. Serinin üç şekilde sonlandırabilirsiniz: süresiz olarak, belirli bir tarihe kadar veya tanımlanan sayıda yineleme tamamlandıktan sonra incelemeleri başlatmak için sürekli olarak çalışmasını. Size, başka bir kullanıcının yönetici veya başka bir genel yönetici serisi oluşturulduktan sonra tarih değiştirerek durdurabilirsiniz **ayarları**, böylece bu tarihte sonlandırır.

1. İçinde **kullanıcılar** bölümünde, gözden geçirme erişen kullanıcıların uygulayacağını belirtin. Erişim gözden geçirmeleri, bir grubun üyesi veya bir uygulamaya atanmış kullanıcılar olabilir. Daha fazla kimin üyeleri (veya uygulamaya atanan), gözden geçirme üyeleri olan tüm kullanıcılar yerine veya uygulamaya olan erişimi gözden geçirme gözden yalnızca konuk kullanıcılar erişim kapsamını belirleyebilirsiniz.

    ![Kullanıcıların erişim gözden geçirmesi - oluştur](./media/create-access-review/users.png)

1. İçinde **grupları** bölümünde, üyeliğini gözden geçirmek istediğiniz bir veya daha fazla grup seçin.

    > [!NOTE]
    > Birden fazla Grup seçme, birden çok erişim gözden geçirmeleri oluşturacaksınız. Örneğin, beş grupları seçmek, beş ayrı bir erişim gözden geçirmeleri oluşturacaksınız.
    
    ![Erişim gözden geçirmesi - Select grubu oluştur](./media/create-access-review/select-group.png)

1. İçinde **uygulamaları** bölümü (seçtiyseniz **bir uygulamaya atanan** 8. adımda), erişimi gözden geçirmek istediğiniz uygulamaları seçin.

    > [!NOTE]
    > Birden fazla uygulama seçerek birden çok erişim gözden geçirmeleri oluşturacaksınız. Örneğin, beş uygulamalarını seçerek beş ayrı bir erişim gözden geçirmeleri oluşturacaksınız.
    
    ![Erişim gözden geçirmesi - uygulama seçme oluştur](./media/create-access-review/select-application.png)

1. İçinde **gözden geçirenler** bölümünde, kapsamdaki tüm kullanıcıları gözden geçirmek için bir veya daha fazla kişi seçin. Veya kendi erişim gözden üyelere sahip seçebilirsiniz. Kaynak grubuysa Grup sahiplerinin gözden geçirmek isteyebilirsiniz. Ayrıca bunlar erişim onayladığınızda gözden geçirenler bir neden sağlamanız gerekebilir.

    ![Gözden Geçiren - erişim gözden geçirmesi oluşturma](./media/create-access-review/reviewers.png)

1. İçinde **programlar** bölümünde, kullanmak istediğiniz programı seçin. Programlara düzenleyerek farklı amaçlara yönelik erişim gözden geçirmeleri toplamak ve izlemek nasıl basitleştirebilir. **Varsayılan Program** her zaman mevcut değil ya da başka bir program oluşturabilirsiniz. Örneğin, bir program için her uyumluluk girişim sahip olmayı seçebilirsiniz veya İş hedefi.

    ![Erişim gözden geçirmesi - program oluştur](./media/create-access-review/programs.png)

### <a name="upon-completion-settings"></a>Tamamlanma ayarları hakkında

1. Bir gözden geçirme tamamlandıktan sonra ne olacağını belirlemek için Genişlet **tamamlama ayarlarını bağlı** bölümü.

    ![Tamamlanma ayarları hakkında](./media/create-access-review/upon-completion-settings.png)

1. Erişimi reddedildi kullanıcılar için otomatik olarak Kaldır istiyorsanız **otomatik uygulama sonuçları kaynağa** için **etkinleştirme**. Gözden Geçirme tamamlandığında sonuçları el ile uygulamak istiyorsanız, anahtar kümesine **devre dışı**.

1. Kullanım **Gözden Geçiren değil yanıt** Gözden Geçiren tarafından gözden geçirme süresi içinde geçirilmedi kullanıcıların ne olacağını belirlemek için liste. Bu ayar, gözden geçirenler tarafından el ile gözden geçirdikten kullanıcılar etkilemez. Son gözden geçirenin karar verme ise, kullanıcının erişim kaldırılacak.

    - **Hiçbir değişiklik** -kullanıcının erişimini değiştirmeden bırakın
    - **Erişimi Kaldır** -kullanıcının erişimini Kaldır
    - **Erişimi onayla** -kullanıcının erişimini onaylama
    - **Öneriler alın** - reddetme üzerinde sistemin öneri alın veya kullanıcı onaylama erişim devam

### <a name="advanced-settings"></a>Gelişmiş ayarlar

1. Ek ayarları belirtmek için genişletin **Gelişmiş ayarlar** bölümü.

    ![Gelişmiş ayarlar](./media/create-access-review/advanced-settings.png)

1. Ayarlama **önerileri göster** için **etkinleştirme** önerileri gözden geçirenler sistem göstermek için kullanıcının erişim bilgilerini temel.

1. Ayarlama **onayda nedeni gerekli kıl** için **etkinleştirme** onay gerekçesi sağlamak gözden geçireni gerektirmek için.

1. Ayarlama **posta bildirimleri** için **etkinleştirme** bir gözden geçirme tamamlandığında erişim gözden geçirmesi başlatıldığında gözden geçirenlere ve yöneticilere e-posta bildirimleri gönderme Azure AD'ye sahip olması.

1. Ayarlama **anımsatıcılar** için **etkinleştirme** sahip Azure AD göndermek erişim gözden geçirmesi anımsatıcı sürüyor, gözden geçirme tamamlamayan gözden geçirenlere.

## <a name="start-the-access-review"></a>Erişim değerlendirmesi başlatma

Erişim gözden geçirmesi ayarları belirttikten sonra tıklayın **Başlat**.

İnceleme kısa bir süre içinde başladıktan sonra varsayılan olarak, Azure AD için gözden geçirenler bir e-posta gönderir. Erişim gözden geçirmesi tamamlanmalarını bekliyor gözden geçirenlere bildirmek e-posta gönderin, Azure AD almamayı tercih ederseniz unutmayın. Nasıl yapılır yönergeleri Göster [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md). Gözden geçirme kendi erişimini gözden geçirmek, konuklar için ise, bunları nasıl yapılır yönergeleri Göster [grupları ve uygulamaları için erişimi kendiniz için incele](review-your-access.md).

Yalnızca bunlar zaten davetini kabul ettiğiniz, Konuklar, gözden geçirenlerin bazıları Konukları varsa, e-posta aracılığıyla bildirilir.

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesi yönetme

Gözden geçirenler Azure AD'ye panosunda bulunan kendi incelemeler tamamlandı olarak ilerleme durumunu izleyebilir **erişim gözden geçirmeleriyle** bölümü. Erişim hakları dizine kadar değişen [gözden geçirme tamamlandığında](complete-access-review.md).

Bu tek seferlik bir gözden geçirme ise, ardından yönetici erişim gözden geçirmesi durdurur veya erişim incelemesi süresi bittikten sonra adımları [grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md) bakın ve sonuçları uygulamak için.  

Erişim gözden geçirmeleri, bir dizi yönetmek için erişim gözden geçirmesinden gidin **denetimleri**, ve zamanlanmış incelemelerde yaklaşan yinelemesi Bul ve bitiş tarihi Düzenle veya kaldıracak ekleme/gözden geçirenler uygun şekilde kaldırma. 

Tamamlama ayarlarını üzerinde yaptığınız seçimlere bağlı olarak, otomatik olacak uygulama incelemesinin bitiş tarihi veya el ile ne zaman gözden geçirmeyi durdurmak sonra yürütülür. Gözden geçirme durumu tamamlandı uygulama gibi ara durumları arasında değişir ve son durumuna uygulandı. Reddedilen kullanıcılar varsa, Grup üyeliğini veya uygulama ataması birkaç dakika içinde kaldırılmakta olan görmeyi beklemelisiniz.

## <a name="create-reviews-via-apis"></a>Gözden geçirmeler API'leri aracılığıyla oluşturma

Erişim gözden geçirmeleri API'lerini kullanarak da oluşturabilirsiniz. Erişimi yönetmek için bunu gruplarını gözden geçirir ve uygulama kullanıcıları Azure portalında da yapılabilir Microsoft Graph API'leri kullanılarak. Daha fazla bilgi için [Azure AD erişim gözden geçirmeleri, API Başvurusu](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Bir kod örneği için bkz. [örnek alma Azure AD erişim gözden geçirmeleri, Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplar veya uygulamalar için erişim gözden geçirin](perform-access-review.md)
- [Gruplar veya uygulamalar için erişimi kendiniz için İncele](review-your-access.md)
- [Grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md)
