---
title: Veya Azure AD ile bir uygulama kullanıcıların erişimini grubu üyelerinin erişim değerlendirmesi tamamlama | Microsoft Docs
description: Bir grubu üyeleri veya Azure Active Directory'de bir uygulamaya erişimi olan kullanıcılar için erişim gözden geçirmesi tamamlama hakkında bilgi edinin.
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
ms.component: compliance-reports
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: e4199f9c201f80cac3df1b7e3af687e507b9fe9a
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448586"
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Uygulamanın Azure AD'de kullanıcı erişimi veya grubu üyelerinin erişim değerlendirmesi tamamlama

Yöneticiler, bir uygulamaya atanmış grup üyeleri veya kullanıcılar için [bir erişim gözden geçirmesi oluşturmak](active-directory-azure-ad-controls-create-access-review.md) üzere Azure Active Directory’yi (Azure AD) kullanabilir. Azure AD, gözden geçirenler otomatik olarak erişim gözden geçirmek için isteyen bir e-posta gönderir. Bir kullanıcı bir e-posta almadıysanız, bunları yönergeleri gönderebilirsiniz [erişiminizi gözden](active-directory-azure-ad-controls-perform-access-review.md). (Davet gözden geçirme önce ilk kabul etmelidir gibi daveti kabul ancak gözden geçirenler olarak atanan konukların erişim gözden geçirmeleri, e-posta almaz unutmayın.) Erişim gözden geçirmesi dönemi bittikten sonra veya bir yönetici erişim gözden geçirmesi durduğunda bakın ve sonuçları uygulamak için bu makaledeki adımları izleyin.

## <a name="view-an-access-review-in-the-azure-portal"></a>Erişim gözden geçirmesi Azure portalında görüntüleme

1. Git [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)seçin **programlar**, erişim gözden geçirme denetim içeren programı seçin.

2. Seçin **Yönet**, erişim gözden geçirme denetim seçin. Programda çok sayıda denetim varsa, belirli bir türdeki denetimleri filtreleyebilir ve durumlarına göre sıralayabilirsiniz. Ayrıca, erişim gözden geçirmesi denetiminin adına ya da onu oluşturan sahibin görünen adına göre arama yapabilirsiniz. 

## <a name="stop-a-review-that-hasnt-finished"></a>Bir gözden geçirme işlemi tamamlanmadı Durdur

Gözden geçirmeyi zamanlanan bitiş tarihinden ulaştıysanız, bu henüz yönetici seçebilirsiniz **Durdur** gözden erken sona erdirmek için. Gözden geçirmeyi durdurduktan sonra kullanıcılar artık incelenebilir. Bunu durdurulduktan sonra bir gözden geçirme yeniden başlatılamıyor.

## <a name="apply-the-changes"></a>Değişiklikleri Uygula 

Erişim gözden geçirmesi sona erdikten sonra ya da son tarihe veya bir yönetici el ile durduruldu ve otomatik olarak Uygula değildi yapılandırılmış gözden geçirilmek üzere seçtiğiniz **Uygula** el ile değişiklikleri uygulamak için. Gözden geçirme sonucunu, Grup veya uygulama güncelleştirerek uygulanır. Bu seçenek yönetici seçtiğinde incelemede, bir kullanıcının erişim reddedildi, Azure AD kullanıcıların üyeliğini veya uygulama ataması kaldırır. 

Erişim gözden geçirmesi tamamlandı ve otomatik olarak Uygula sonra gözden geçirme durumu tamamlandı Ara durumları arasında değişir ve son olarak uygulanan bir duruma değiştirir yapılandırıldı. Varsa, kaynak grubundan kaldırılıyor grup üyeliği ve uygulama atama birkaç dakika içinde reddedilen kullanıcıları görmek beklemeniz gerekir.

Gözden geçirme uygulamak veya seçerek yapılandırılmış otomatik **Uygula** bir şirket içi dizininde kaynaklanan bir grup veya dinamik bir grup üzerinde bir etkisi yoktur. Şirket içi kaynaklanan bir grubu değiştirmek istiyorsanız, sonuçları indir ve bu değişiklikleri bu dizine grubunda gösterimini uygulanır.

## <a name="download-the-results-of-the-review"></a>Gözden geçirme sonuçlarını indir

Gözden geçirme sonuçlarını almak için seçin **onaylar** seçip **indirme**. Sonuçta elde edilen CSV dosyasını Excel'de veya UTF-8 açın. diğer programları görüntülenebilir kodlanmış CSV dosyaları.

## <a name="optional-delete-a-review"></a>İsteğe bağlı: gözden geçirmeyi Sil
Artık incelemesindeki ilginizi çeken, silebilirsiniz. Seçin **Sil** gözden Azure AD'den kaldırılamadı.

> [!IMPORTANT]
> Silme işlemi gerçekleştirilmeden önce hiçbir uyarı yok, bu nedenle gözden geçirmeyi silmek istediğinizden emin olun.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleri için programları ve denetimleri yönetme](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Bir grubun üyeleri veya bir uygulamaya erişim için erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](active-directory-privileged-identity-management-how-to-start-security-review.md)
