---
title: Bir grup veya kullanıcıları Azure AD ile bir uygulamaya erişimi olan üyeleri için erişim gözden geçirmesi oluştur | Microsoft Docs
description: Üyeleri bir grup veya uygulamaya erişimi olan kullanıcılar için erişim gözden geçirmesi oluşturmayı öğrenin.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: e52686a8cfd691a7f82c6e64968cd94634fd596e
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607797"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Azure AD ile uygulama erişimi ve Grup üyeleri erişim gözden geçirmesi oluşturma

Erişim atamalarını kullanıcıların erişimi artık gerekmeyen "eski" olur. Eski erişim atamalarını ile ilişkili riski azaltmak için Yöneticiler Azure Active Directory (Azure AD) grubu üyeleri veya bir uygulamaya atanan kullanıcılar için erişim gözden geçirmesi oluşturmak için kullanabilirsiniz. Yinelenen erişim gözden geçirmesi oluşturma zamandan tasarruf olabilir. Düzenli olarak bir uygulama erişimi veya bir grubun üyesi olan kullanıcıları gözden geçirmek gerekiyorsa, bu incelemeleri sıklığını da tanımlayabilirsiniz. Bu senaryolar hakkında daha fazla bilgi için bkz. [kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md) ve [konuk erişimini yönetme](manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Erişim gözden geçirmesi oluştur

1. Bir genel yönetici veya kullanıcı hesabı yöneticisi, Git [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)seçip **programlar**.

2. Oluşturmak istediğiniz erişim gözden geçirme denetim tutan programı seçin. **Varsayılan Program** her zaman mevcut değil ya da başka bir program oluşturabilirsiniz. Örneğin, bir program için her uyumluluk girişim sahip olmayı seçebilirsiniz veya İş hedefi.

3. Bir program içindeki seçin **denetimleri**ve ardından **Ekle** bir denetim eklemek için.

4. Erişim gözden geçirmesi adı. İsteğe bağlı olarak, gözden geçirme bir açıklama girin. Ad ve açıklama gözden geçirenlere gösterilmektedir.

5. Başlangıç tarihini ayarlayın. Varsayılan olarak, erişim gözden geçirmesi bir kez gerçekleşir, aynı oluşturulduğunda başlar ve bir ay içinde sonlandırır. Başlangıç değiştirebilirsiniz ve istediğiniz sayıda gün ancak erişim sağlamak için bitiş tarihi başlangıç gelecekte ve son gözden geçirin.

6. Erişim gözden geçirme yineleme yapmak için haftalık, aylık, üç aylık veya yıllık bir kez gelen sıklığını değiştirmek ve kaç gün yinelenen serisinin her gözden geçirenlerin girişi için açık olacaktır tanımlamak için kaydırıcı veya metin kutusunu kullanın. İçin kümesi için örneğin, en uzun süre aylık gözden geçirme 27 gün incelemeleri çakışan kaçınmaktır. 

7.  Yinelenen erişim gözden geçirme serisini 3 şekilde sonlandırabilirsiniz: süresiz olarak, belirli bir tarihe kadar veya tanımlanan sayıda yineleme tamamlandıktan sonra incelemeleri başlatmak için sürekli olarak çalışmasını. Böylece, belirli bir tarihte sona serisi tarih ayarları değiştirerek, başka bir kullanıcı hesabı yöneticisi veya başka bir genel yönetici oluşturulduktan sonra durdurabilirsiniz.

8. Erişim gözden geçirmeleri, bir grubun üyesi veya bir uygulamaya atanmış kullanıcılar olabilir. Daha fazla kimin üyeleri (veya uygulamaya atanmış), gözden geçirme üyeleri olan tüm kullanıcılar yerine veya uygulamaya olan erişimi gözden geçirme gözden yalnızca konuk kullanıcılar erişim kapsamını belirleyebilirsiniz.

9. Kapsamdaki tüm kullanıcıları gözden geçirmek için bir veya daha fazla kişi seçin. Veya kendi erişim gözden üyelere sahip seçebilirsiniz. Kaynak grubuysa Grup sahiplerinin gözden geçirmek isteyebilirsiniz. Ayrıca bunlar erişim onayladığınızda gözden geçirenler bir neden sağlamanız gerekebilir.

10. Gözden Geçirme tamamlandığında sonuçları el ile uygulamak isterseniz tıklayın **Başlat**.  Aksi halde, sonraki bölümde otomatik olarak gözden geçirmeyi yapılandırma açıklanmaktadır uygulayın.

### <a name="configuring-an-access-review-with-auto-apply"></a>Erişim gözden geçirmesi auto-apply ile yapılandırma

1.  Tamamlanmasından sonra ayarları ve etkinleştirme otomatik sonuçları kaynağına uygulamak için menüsünü genişletin. 

2.  Burada kullanıcıları değil incelenen Gözden Geçiren tarafından gözden geçirme süresi içinde durumlarda, reddetme/kullanıcının sürekli erişim onaylama üzerinde (etkinse) sistemin öneri alın, erişimleri değiştirmeden bırakın veya kaldırın erişim gözden geçirmesi olabilir, erişim. Kullanıcının erişim kaldırılacak son gözden geçirenin karar verme, gözden geçirenler tarafından el ile – incelendi kullanıcılar etkilemez.

3.  Gözden Geçiren yanıt öneriler alın seçeneğini etkinleştirmek için Gelişmiş Ayarları'nı genişletin ve Show önerilerini etkinleştirmek.
 
4.  Son olarak, tıklayın **Başlat**.

Tamamlama ayarlarını üzerinde yaptığınız seçimlere bağlı olarak, otomatik olacak uygulama incelemesinin bitiş tarihi veya el ile ne zaman gözden geçirmeyi durdurmak sonra yürütülür. Gözden geçirme durumu tamamlandı uygulama gibi ara durumları arasında değişir ve son durumuna uygulandı. Reddedilen kullanıcılar varsa, Grup üyeliğini veya uygulama ataması birkaç dakika içinde kaldırılmakta olan görmeyi beklemelisiniz.


## <a name="manage-the-access-review"></a>Erişim gözden geçirmesi yönetme

İnceleme kısa bir süre içinde başladıktan sonra varsayılan olarak, Azure AD için gözden geçirenler bir e-posta gönderir. Erişim gözden geçirmesi tamamlanmalarını bekliyor gözden geçirenlere bildirmek e-posta gönderin, Azure AD almamayı tercih ederseniz unutmayın. Nasıl yapılır yönergeleri Göster [erişimi incele](perform-access-review.md). Gözden geçirme kendi erişimini gözden geçirmek, konuklar için ise, bunları nasıl yapılır yönergeleri Göster [kendi erişim gözden geçirme](perform-access-review.md).

Yalnızca bunlar zaten davetini kabul ettiğiniz, Konuklar, gözden geçirenlerin bazıları Konukları varsa, e-posta aracılığıyla bildirilir.

Erişim gözden geçirmeleri, bir dizi yönetmek için erişim gözden geçirmesinden gidin **denetimleri**, ve zamanlanmış incelemelerde yaklaşan yinelemesi Bul ve bitiş tarihi Düzenle veya kaldıracak ekleme/gözden geçirenler uygun şekilde kaldırma. 

Gözden geçirenler Azure AD'ye panosunda bulunan kendi incelemeler tamamlandı olarak ilerleme durumunu izleyebilir **erişim gözden geçirmeleri** bölümü. Erişim hakları dizine kadar değişen [gözden geçirme tamamlandığında](complete-access-review.md).

## <a name="next-steps"></a>Sonraki adımlar

Erişim gözden geçirmesi başlatıldığında, Azure AD erişim gözden geçirmek için isteyen bir e-posta gözden geçirenler otomatik olarak gönderir. Bir kullanıcı bir e-posta almadıysanız, bunları nasıl yapılır yönergeleri gönderebilirsiniz [erişimi incele](perform-access-review.md). 

Bu tek seferlik bir gözden geçirme ise, ardından yönetici erişim gözden geçirmesi durdurur veya erişim incelemesi süresi bittikten sonra adımları [erişim değerlendirmesi tamamlama](complete-access-review.md) bakın ve sonuçları uygulamak için.  

Bu gözden geçirme serisini ise, ardından gidin **gözden geçirme geçmişi** tamamlanmış erişim incelemesi seçmek için erişim gözden geçirme serisini sayfasında,.  Yaklaşan incelemeleri, altında listelenir **zamanlanmış gözden geçirme**, burada süresini Düzenle ve ekleyebilir veya bireysel incelemeleri için gözden geçirenler kaldırın.
