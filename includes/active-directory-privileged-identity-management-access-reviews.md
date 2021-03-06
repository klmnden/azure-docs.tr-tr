---
title: include dosyası
description: include dosyası
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: d791c4ba46587ac5709d72cb31bc76f087118b03
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476245"
---
## <a name="create-one-or-more-access-reviews"></a>Bir veya daha fazla erişim gözden geçirmesi oluşturma

1. Tıklayın **yeni** yeni bir erişim gözden geçirmesi oluşturma.

1. Erişim gözden geçirmesi adı. İsteğe bağlı olarak, gözden geçirme bir açıklama girin. Ad ve açıklama gözden geçirenlere gösterilmektedir.

    ![Erişim gözden geçirmesi - gözden geçirme adı ve açıklaması oluştur](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. Ayarlama **başlangıç tarihi**. Varsayılan olarak, erişim gözden geçirmesi bir kez gerçekleşir, aynı oluşturulduğunda başlar ve bir ay içinde sonlandırır. Başlangıç değiştirebilirsiniz ve istediğiniz sayıda gün ancak erişim sağlamak için bitiş tarihi başlangıç gelecekte ve son gözden geçirin.

    ![Tarih sıklığı, süresi sonu, kaç kez başlangıç ve bitiş tarihi](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. Erişim gözden geçirme yineleme yapmak için değiştirme **sıklığı** ayarını **bir kez** için **haftalık**, **aylık**,  **Üç aylık**, **yıllık**, veya **yarı annually**. Kullanım **süresi** kaç gün yinelenen serisinin her incelenmesi gereken gözden geçirenler girişten açık tanımlamak için kaydırıcı veya metin kutusu. Örneğin, bir aylık gözden geçirilmek üzere ayarlayabileceğiniz en uzun süre 27 incelemeleri çakışan önlemek için gündür.

1. Kullanım **son** yinelenen erişim sonlandırma belirtmek için ayarı serisini gözden geçirin. Serinin üç şekilde sonlandırabilirsiniz: süresiz olarak, belirli bir tarihe kadar veya tanımlanan sayıda yineleme tamamlandıktan sonra incelemeleri başlatmak için sürekli olarak çalışmasını. Size, başka bir kullanıcının yönetici veya başka bir genel yönetici serisi oluşturulduktan sonra tarih değiştirerek durdurabilirsiniz **ayarları**, böylece bu tarihte sonlandırır.

1. İçinde **kullanıcılar** bölümünde, üyeliğini gözden geçirmek istediğiniz bir veya daha fazla rol seçin.

    ![Kullanıcılar kapsamının rol üyeliğini gözden geçirmek için](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > Birden fazla rolü seçerek birden çok erişim gözden geçirmeleri oluşturacaksınız. Örneğin, beş rolleri seçme beş ayrı bir erişim gözden geçirmeleri oluşturacaksınız.

    Erişim gözden geçirmesi Azure AD rolleri oluşturuyorsanız, aşağıdaki gözden geçirme üyeliği listesinin bir örneğini gösterir.

    ![Seçebileceğiniz üyelik bölmesindeki liste Azure AD rollerini İncele](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    Erişim gözden geçirmesi Azure kaynağı rolleri oluşturuyorsanız, aşağıdaki gözden geçirme üyeliği listesinin bir örneğini gösterir.

    ![Gözden geçirme üyeliği bölmesinde Azure kaynak rolleri listeleme seçebilirsiniz](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. İçinde **gözden geçirenler** bölümünde, tüm kullanıcıları gözden geçirmek için bir veya daha fazla kişi seçin. Veya kendi erişim gözden üyelere sahip seçebilirsiniz.

    ![Gözden geçirenler listesinde seçilen kullanıcılarla veya üyeler (kendi)](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **Seçili kullanıcıları** -erişim gerek duyan bilmiyorsanız bu seçeneği kullanın. Bu seçenek belirtilmişse, gözden geçirmeyi tamamlamak için bir kaynak sahibi veya grup yöneticisi atayabilirsiniz.
    - **Üyeler (kendi)** -kullanıcılar kendi rol atamalarını gözden geçirmek için bu seçeneği kullanın.

### <a name="upon-completion-settings"></a>Tamamlanma ayarları hakkında

1. Bir gözden geçirme tamamlandıktan sonra ne olacağını belirlemek için Genişlet **tamamlama ayarlarını bağlı** bölümü.

    ![Tamamlanmasından sonra ayarlar otomatik olarak uygulanır ve gözden geçirme değil yanıt vermesi gerekir](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. Erişimi reddedildi kullanıcılar için otomatik olarak Kaldır istiyorsanız **otomatik uygulama sonuçları kaynağa** için **etkinleştirme**. Gözden Geçirme tamamlandığında sonuçları el ile uygulamak istiyorsanız, anahtar kümesine **devre dışı**.

1. Kullanım **Gözden Geçiren değil yanıt** Gözden Geçiren tarafından gözden geçirme süresi içinde geçirilmedi kullanıcıların ne olacağını belirlemek için liste. Bu ayar, gözden geçirenler tarafından el ile gözden geçirdikten kullanıcılar etkilemez. Son gözden geçirenin karar verme ise, kullanıcının erişim kaldırılacak.

    - **Hiçbir değişiklik** -kullanıcının erişimini değiştirmeden bırakın
    - **Erişimi Kaldır** -kullanıcının erişimini Kaldır
    - **Erişimi onayla** -kullanıcının erişimini onaylama
    - **Öneriler alın** - reddetme üzerinde sistemin öneri alın veya kullanıcı onaylama erişim devam

### <a name="advanced-settings"></a>Gelişmiş ayarlar

1. Ek ayarları belirtmek için genişletin **Gelişmiş ayarlar** bölümü.

    ![Gelişmiş ayarları göster önerileri için neden onay, posta bildirimleri ve anımsatıcılar gerektirir](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. Ayarlama **önerileri göster** için **etkinleştirme** önerileri gözden geçirenler sistem göstermek için kullanıcının erişim bilgilerini temel.

1. Ayarlama **onayda nedeni gerekli kıl** için **etkinleştirme** onay gerekçesi sağlamak gözden geçireni gerektirmek için.

1. Ayarlama **posta bildirimleri** için **etkinleştirme** bir gözden geçirme tamamlandığında erişim gözden geçirmesi başlatıldığında gözden geçirenlere ve yöneticilere e-posta bildirimleri gönderme Azure AD'ye sahip olması.

1. Ayarlama **anımsatıcılar** için **etkinleştirme** sahip Azure AD göndermek erişim gözden geçirmesi anımsatıcı sürüyor, gözden geçirme tamamlamayan gözden geçirenlere.
