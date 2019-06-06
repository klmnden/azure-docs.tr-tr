---
title: Gruplar veya uygulamalar - Azure Active Directory erişim gözden geçirmesi tamamlama | Microsoft Docs
description: Grup üyelerini Azure Active Directory erişim gözden geçirmeleri uygulama erişimi ve erişim değerlendirmesi tamamlama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/22/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec3909ffbb624284f999360140b7454098643062
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66473357"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Erişim gözden geçirmesi gruplarının tamamlamak ya da uygulamaları Azure ad erişim gözden geçirmeleri

Bir yönetici olarak, [grupları ve uygulamaları, erişim gözden geçirmesi Oluştur](create-access-review.md) ve gözden geçirenler [erişim gözden geçirmesi gerçekleştirme](perform-access-review.md). Bu makalede, erişim gözden geçirmesi sonuçlarını görmek ve sonuçları uygulama açıklar.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure AD Premium P2
- Genel yönetici, yönetici kullanıcı, güvenlik yöneticisi veya güvenlik okuyucusu

Daha fazla bilgi için [hangi kullanıcıların lisansına sahip olması gerekir?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="view-an-access-review"></a>Erişim gözden geçirmesi görüntüleyin

Gözden geçirenler, incelemeler tamamlandı olarak ilerleme durumunu izleyebilirsiniz.

1. Azure portal ve açık oturum [Kimlik Yönetimi sayfası](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Sol menüde **erişim gözden geçirmeleriyle**.

1. Listede, erişim gözden geçirmesi tıklayın.

    Bir dizi erişim görüntülemek için gözden geçirmeleri, erişim gözden geçirmesi için gidin ve zamanlanmış incelemelerde yaklaşan örnekleri bulabilirsiniz.

    Üzerinde **genel bakış** sayfasında, ilerleme durumunu görebilirsiniz. Gözden geçirmeyi tamamlanana kadar erişim hakları dizin olarak değiştirilir.

    ![Erişim gözden geçirmeleri ilerleme durumu](./media/complete-access-review/overview-progress.png)

1. Erişim gözden geçirmesi durdurmak istiyorsanız, önce ulaşıldı zamanlanmış bitiş tarihi, tıklayın **Durdur** düğmesi.

    Ne zaman bir gözden geçirmeyi durdurmak, gözden geçirenler artık yanıt vermek mümkün olacaktır. Bunu durdurulduktan sonra bir gözden geçirme yeniden başlatılamıyor.

1. Artık erişim gözden geçirmesine ilginizi çeken, tıklayarak silebilirsiniz **Sil** düğmesi.

## <a name="apply-the-changes"></a>Değişiklikleri Uygula

Varsa **otomatik uygulama sonuçları kaynağa** etkin ve yaptığınız seçimlere dayanarak **tamamlama ayarlarını bağlı**, otomatik-uygulamak incelemesinin bitiş tarihi veya el ile ne zaman gözden geçirmeyi durdurmak sonra yürütülür.

Varsa **otomatik uygulama sonuçları kaynağa** değildi gözden geçirmeniz için etkin, tıklayın **Uygula** el ile değişiklikleri uygulamak için. ' A tıkladığınızda incelemede, bir kullanıcının erişim reddedildi, **Uygula**, Azure AD, üyeliği veya uygulama ataması kaldırır.

![Erişim incelemesi Değişiklikleri Uygula](./media/complete-access-review/apply-changes.png)

Gözden geçirme durumu değiştirilecek **tamamlandı** gibi ara durumları arasında **uygulama** ve son durumuna **sonuç uygulandı**. Reddedilen kullanıcılar varsa, Grup üyeliğini veya uygulama ataması birkaç dakika içinde kaldırılmakta olan görmeyi beklemelisiniz.

Gözden geçirme uygulamak veya seçerek yapılandırılmış otomatik **Uygula** bir şirket içi dizininde kaynaklanan bir grup veya dinamik bir grup üzerinde bir etkisi yoktur. Şirket içi kaynaklanan bir grubu değiştirmek istiyorsanız, sonuçları indir ve bu değişiklikleri bu dizine grubunda gösterimini uygulanır.

## <a name="retrieve-the-results"></a>Sonuçları alma

Bir kerelik erişim gözden geçirmesi sonuçlarını görüntülemek için tıklayın **sonuçları** sayfası. Arama kutusuna yalnızca kullanıcının erişim görüntülemek için erişim gözden geçirildi bir kullanıcının kullanıcı asıl adını ve görünen ad yazın.

![Erişim gözden geçirmesi sonuçlarını Al](./media/complete-access-review/retrieve-results.png)

Etkin erişim gözden geçirmesi yinelenendir ilerlemesini görüntülemek için tıklayın **sonuçları** sayfası.

Erişim gözden geçirmesi yinelenendir tamamlanmış bir örneğini sonuçlarını görüntülemek için tıklayın **geçmişi gözden**seçin tamamlanmış erişim gözden geçirme örneği, belirli örneğini listeden tabanlı örneğinin başlangıç ve bitiş tarihi. Bu örnek sonuçları örneğinden alınabilen **sonuçları** sayfası.

Erişim gözden geçirmesi sonuçlarını almak için tıklayın **indirme** düğmesi. Sonuçta elde edilen CSV dosyasını Excel'de veya UTF-8 açın. diğer programları görüntülenebilir kodlanmış CSV dosyaları.

## <a name="remove-users-from-an-access-review"></a>Erişim gözden geçirmesi Kullanıcıları Kaldır

 Varsayılan olarak, silinmiş bir kullanıcı Azure AD’de 30 gün boyunca silinmiş olarak kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir.  30 gün sonra bu kullanıcı kalıcı olarak silinir.  Ayrıca, bir Genel Yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md).  Bir kullanıcı kalıcı olarak silindikten sonra, bu kullanıcıya ilişkin sonraki veriler etkin erişim gözden geçirmelerinden kaldırılır.  Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](manage-guest-access-with-access-reviews.md)
- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md)
