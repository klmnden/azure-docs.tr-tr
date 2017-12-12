---
title: "Bir grubu üyeleri veya Azure AD ile bir uygulamaya erişimi olan kullanıcılar için bir erişim gözden geçirme oluşturma | Microsoft Docs"
description: "Bir grubu üyeleri veya bir uygulamaya erişimi olan kullanıcılar için bir erişim gözden geçirme oluşturmayı öğrenin."
services: active-directory
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: b2f8985f12e17ac69543cfb3a33725f796eedde8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Azure AD ile bir erişim gözden geçirme Grup üyeleri veya uygulama erişimi oluşturma

Kullanıcılar artık gerekmeyen erişim sahibi, erişim atamalarını "eski" olur. Eski erişim atamalarını ile ilişkili riskini azaltmak için Yöneticiler Azure Active Directory (Azure AD) Grup üyeleri veya bir uygulamaya atanan kullanıcılar için bir erişim gözden geçirme oluşturmak için kullanabilirsiniz. Bu senaryolar hakkında daha fazla bilgi için bkz: [kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) ve [Konuk erişimi yönetme](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Bir erişim gözden geçirme oluşturma

1. Genel yönetici olarak, Git [erişim incelemeleri sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)seçip **programlar**.

2. Oluşturmak istediğiniz erişim gözden geçirme denetimi tutan programı seçin. **Varsayılan Program** her zaman mevcut değil veya farklı bir program oluşturabilirsiniz. Örneğin, bir program her uyumluluk girişimi sahip olmayı seçebilirsiniz veya İş hedefi.

3. Programında seçin **denetimleri**ve ardından **Ekle** bir denetim eklemek için.

4. Her erişim gözden geçirme olarak adlandırın. İsteğe bağlı olarak, her gözden geçirme bir açıklama girin. Ad, gözden geçirenlere gösterilir.

5. Başlangıç ve bitiş tarihlerini belirleyin. Varsayılan olarak, bir erişim oluşturulduktan aynı gün ve biten bir ay içinde başlatır gözden geçirin. Başlangıç değiştirebilir ve çok sayıda gün istiyor ancak bir erişim sağlamak için bitiş tarihi başlangıç gelecekte ve son gözden geçirin.

6. Erişim incelemeleri, uygulamaya atanan kullanıcılar veya bir grubun üyeleri için olabilir. Daha fazla kimin üyeleridir (veya uygulamaya atanan), üyeleri olan tüm kullanıcıların gözden geçirme yerine veya uygulamaya kimlerin erişimi gözden geçirme gözden yalnızca konuk kullanıcıların erişimini kapsamını belirleyebilirsiniz.

7. Kapsamdaki tüm kullanıcılar gözden geçirmek için bir veya daha fazla kişileri seçin. Ya da kendi access gözden üyeleri olan seçebilirsiniz. Kaynak grubu ise, Grup sahiplerinin gözden geçirmek isteyebilirsiniz. Ayrıca, erişim onayladığınızda gözden geçirenler nedeni sağlamak isteyebilirsiniz.

8. Son olarak, seçin **Başlat**.


## <a name="manage-the-access-review"></a>Erişim gözden geçirme yönetme

Kısa süre içinde gözden başladıktan sonra varsayılan olarak, Azure AD gözden geçirenlere bir e-posta gönderir. Azure AD e-posta Gönder biçimlendirmemesini seçerseniz, bir erişim gözden geçirme tamamlanmalarını bekliyor gözden geçirenlere bildirmek emin olun. Nasıl yapılır yönergeleri Göster [erişim gözden](active-directory-azure-ad-controls-perform-access-review.md). Gözden geçirilecek kendi access gözden geçirmek konuklar için ise, bunları nasıl yönergelerini Göster [kendi access gözden](active-directory-azure-ad-controls-perform-access-review.md).

Gözden geçirenler konuklar bazıları, yalnızca bunlar zaten kendi daveti kabul ettiğiniz, konukların e-posta aracılığıyla bildirilir.


Gözden geçirenler kendi incelemeleri Azure AD Panosu'nda tamamlarken ilerleme durumunu izleyebilirsiniz **erişim incelemeleri** bölümü. Erişim hakları yok kadar dizinde değiştirilir [gözden tamamlandı](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir erişim gözden geçirme başladığında, Azure AD erişim incelemek için onları isteyen bir e-posta gözden geçirenler otomatik olarak gönderir. Bir kullanıcı bir e-posta gelmedi, bunları nasıl yönergelerini gönderebilirsiniz [erişim gözden](active-directory-azure-ad-controls-perform-access-review.md). 

Erişim değerlendirme süresi sona erer veya yönetici erişim gözden geçirme durdurur sonra adımları [erişim gözden geçirme tamamlamak](active-directory-azure-ad-controls-complete-access-review.md) bakın ve sonuçları uygulamak için.


