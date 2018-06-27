---
title: Bir grubu üyeleri veya Azure AD ile bir uygulamaya erişimi olan kullanıcılar için bir erişim gözden geçirme oluşturma | Microsoft Docs
description: Bir grubu üyeleri veya bir uygulamaya erişimi olan kullanıcılar için bir erişim gözden geçirme oluşturmayı öğrenin.
services: active-directory
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: compliance-reports
ms.date: 06/21/2018
ms.author: rolyon
ms.openlocfilehash: 2c4e26bb6f2cd144d00d9e4ada92d756fe68418b
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37020415"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Azure AD ile bir erişim gözden geçirme Grup üyeleri veya uygulama erişimi oluşturma

Kullanıcılar artık gerekmeyen erişim sahibi, erişim atamalarını "eski" olur. Eski erişim atamalarını ile ilişkili riskini azaltmak için Yöneticiler Azure Active Directory (Azure AD) Grup üyeleri veya bir uygulamaya atanan kullanıcılar için bir erişim gözden geçirme oluşturmak için kullanabilirsiniz. Yinelenen erişim incelemeler oluşturma zaman kazandıran olabilir. Uygulama erişimi veya bir grubun üyesi olan kullanıcıları düzenli olarak gözden ihtiyacınız varsa, bu incelemeler sıklığını tanımlayabilirsiniz. Bu senaryolar hakkında daha fazla bilgi için bkz: [kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) ve [Konuk erişimi yönetme](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Erişim gözden geçirmesi oluştur

1. Bir genel yönetici veya kullanıcı hesabı yönetici Git [erişim incelemeleri sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)seçip **programlar**.

2. Oluşturmak istediğiniz erişim gözden geçirme denetimi tutan programı seçin. **Varsayılan Program** her zaman mevcut değil veya farklı bir program oluşturabilirsiniz. Örneğin, bir program her uyumluluk girişimi sahip olmayı seçebilirsiniz veya İş hedefi.

3. Programında seçin **denetimleri**ve ardından **Ekle** bir denetim eklemek için.

4. Adı erişim gözden geçirme. İsteğe bağlı olarak, gözden geçirme bir açıklama girin. Ad ve açıklama gözden geçirenlere gösterilir.

5. Başlangıç tarihini ayarlayın. Varsayılan olarak, bir erişim gözden geçirme bir kez meydana gelir, aynı oluşturulduğunda başlar ve bir ay içinde sona erer. Başlangıç değiştirebilir ve çok sayıda gün istiyor ancak bir erişim sağlamak için bitiş tarihi başlangıç gelecekte ve son gözden geçirin.

6. Erişim gözden geçirme yinelenen yapmak için haftalık, aylık, üç aylık veya yıllık olarak bir kez sıklığını değiştirmek ve kaç gün her gözden geçirme yinelenen serisinin geçirenlerin girişi için açık olacaktır tanımlamak için kaydırıcıyı veya metin kutusunu kullanın. Örneğin, için ayarlayabileceğiniz için en uzun süre incelemeler çakışan önlemek için 27 gün aylık bir gözden geçirme gerekir. 

7.  Yinelenen erişim gözden geçirme serisinin 3 şekilde bitebilir: sürekli incelemeler süresiz olarak, belirli bir tarih kadar veya tanımlı bir yineleme sayısı tamamlandıktan sonra başlatmak için çalışır. Belirli bir tarihte sona ermesini sağlayacak biçimde seri Ayarları'nda tarih değiştirerek, başka bir kullanıcı hesabının yönetici veya başka bir genel yönetici oluşturulduktan sonra durdurabilirsiniz.

8. Erişim incelemeleri, uygulamaya atanan kullanıcılar veya bir grubun üyeleri için olabilir. Daha fazla kimin üyeleridir (veya uygulamaya atanan), üyeleri olan tüm kullanıcıların gözden geçirme yerine veya uygulamaya kimlerin erişimi gözden geçirme gözden yalnızca konuk kullanıcıların erişimini kapsamını belirleyebilirsiniz.

9. Kapsamdaki tüm kullanıcılar gözden geçirmek için bir veya daha fazla kişileri seçin. Ya da kendi access gözden üyeleri olan seçebilirsiniz. Kaynak grubu ise, Grup sahiplerinin gözden geçirmek isteyebilirsiniz. Ayrıca, erişim onayladığınızda gözden geçirenler nedeni sağlamak isteyebilirsiniz.

10. Gözden Geçirme tamamlandığında sonuçları el ile uygulamak isterseniz tıklayın **Başlat**.  Aksi halde, sonraki bölümde otomatik olarak gözden yapılandırılmasını açıklamaktadır uygulayın.

### <a name="configuring-an-access-review-with-auto-apply"></a>Bir erişim gözden geçirme auto-apply ile yapılandırma

1.  Tamamlandıktan sonra ayarları ve etkinleştirme otomatik sonuçları kaynağa uygulamak için menüsünü genişletin. 

2.  Burada kullanıcılar değil gözden geçirenin gözden geçirme süresi içinde durumlarda engelleme/kullanıcının sürekli erişim onaylama üzerinde sistemin öneri (etkinse) alın, erişimleri değiştirmeden bırakın ya da kaldırma erişim gözden geçirme sağlayabilirsiniz, erişim. Kullanıcının erişimini kaldırılacak son gözden geçirenin karar verme, ise bu gözden geçirenler tarafından el ile – incelendi kullanıcılar etkilemez.

3.  Önerileri gözden geçirenler yanıt yapılacak seçeneğini etkinleştirmek için Gelişmiş Ayarları'nı genişletin ve Göster önerileri etkinleştirin.
 
4.  Son olarak, tıklatın **Başlat**.

Seçimlerinizi tamamlama ayarlarını bağlı bağlı olarak, otomatik olacak uygulama yürütülebilir gözden 's bitiş tarihi veya gözden geçirme el ile durdurun, sonra. Gözden geçirme durumunu ara durumları uygulanıyor gibi üzerinden tamamlandı değiştirecek ve son olarak uygulanan durumuna. Varsa, Grup üyeliğini veya uygulama ataması birkaç dakika içinde kaldırılmakta olan reddedilen kullanıcıları görmek beklemeniz gerekir.


## <a name="manage-the-access-review"></a>Erişim gözden geçirme yönetme

Kısa süre içinde gözden başladıktan sonra varsayılan olarak, Azure AD gözden geçirenlere bir e-posta gönderir. Azure AD e-posta Gönder biçimlendirmemesini seçerseniz, bir erişim gözden geçirme tamamlanmalarını bekliyor gözden geçirenlere bildirmek emin olun. Nasıl yapılır yönergeleri Göster [erişim gözden](active-directory-azure-ad-controls-perform-access-review.md). Gözden geçirilecek kendi access gözden geçirmek konuklar için ise, bunları nasıl yönergelerini Göster [kendi access gözden](active-directory-azure-ad-controls-perform-access-review.md).

Gözden geçirenler konuklar bazıları, yalnızca bunlar zaten kendi daveti kabul ettiğiniz, konukların e-posta aracılığıyla bildirilir.

Bir dizi erişim incelemeler yönetmek için erişim gözden geçirme sürecinden gidin **denetimleri**, ve yaklaşan oluşum zamanlanmış incelemeler içinde bulmak ve bitiş tarihi düzenlemek veya kaldıracak ekleyin veya gözden geçirenler uygun şekilde Kaldır. 

Gözden geçirenler kendi incelemeleri Azure AD Panosu'nda tamamlarken ilerleme durumunu izleyebilirsiniz **erişim incelemeleri** bölümü. Erişim hakları yok kadar dizinde değiştirilir [gözden tamamlandı](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir erişim gözden geçirme başladığında, Azure AD erişim incelemek için onları isteyen bir e-posta gözden geçirenler otomatik olarak gönderir. Bir kullanıcı bir e-posta gelmedi, bunları nasıl yönergelerini gönderebilirsiniz [erişim gözden](active-directory-azure-ad-controls-perform-access-review.md). 

Bu tek seferlik bir gözden geçirme ise, sonra erişim değerlendirme süresi sona erer veya yönetici erişim gözden geçirme durdurur sonra adımları [erişim gözden geçirme tamamlamak](active-directory-azure-ad-controls-complete-access-review.md) bakın ve sonuçları uygulamak için.  

Bu gözden geçirme serisi ise, ardından gidin **geçmişini gözden** tamamlanmış erişim gözden geçirme seçmek için erişim gözden geçirme serisi sayfasında.  Yaklaşan incelemeler altında listelenir **zamanlanmış gözden**, burada süre, Düzen ve ekleyebilir veya bireysel incelemeler için gözden geçirenleri Kaldır.
