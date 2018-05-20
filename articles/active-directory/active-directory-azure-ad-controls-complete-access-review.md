---
title: Bir erişim gözden geçirme veya bir uygulamaya Azure AD ile kullanıcıların erişim grubu üyelerinin tamamlamak | Microsoft Docs
description: Bir grubu üyeleri veya Azure Active Directory'de bir uygulamaya erişimi olan kullanıcılar için bir erişim gözden geçirme tamamlamak öğrenin.
services: active-directory
documentationcenter: ''
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: billmath
ms.openlocfilehash: 7998d69a079c4858c54bea22dbd24e4e84c8c793
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Bir grup veya kullanıcıların Azure AD'de bir uygulamaya erişmeye üyeleri erişim incelenmesi tamamlayın

Yöneticiler için Azure Active Directory (Azure AD) kullanabilirsiniz [erişim gözden geçirme oluşturmak](active-directory-azure-ad-controls-create-access-review.md) Grup üyeleri veya bir uygulamaya atanan kullanıcılar için. Azure AD, gözden geçirenler erişim incelemek için onları isteyen bir e-posta otomatik olarak gönderir. Bir kullanıcı bir e-posta gelmedi varsa, bunları yönergeleri gönderebilirsiniz [erişiminizi gözden](active-directory-azure-ad-controls-perform-access-review.md). (Gözden geçirme önce bir davet ilk kabul etmelisiniz gibi gözden geçirenler atanmış, ancak daveti kabul edilmedi konuklar bir e-posta erişimi incelemeler almaz unutmayın.) Erişim gözden geçirme süresi bittikten sonra veya bir yönetici erişim gözden geçirme durursa, bakın ve sonuçları uygulamak için bu makaledeki adımları izleyin.

## <a name="view-an-access-review-in-the-azure-portal"></a>Azure portalında bir erişim gözden geçirme görüntüleyin

1. Git [erişim incelemeleri sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)seçin **programları**ve erişim gözden geçirme denetimi içeren programı seçin.

2. Seçin **Yönet**ve erişim gözden geçirme denetimi seçin. Programa birçok denetimleri varsa, belirli türde ve sıralama denetimleri için durumlarına göre filtre uygulayabilirsiniz. Ayrıca erişim gözden geçirme denetimi adını veya oluşturan sahibi görünen adını göre arama yapabilirsiniz. 

## <a name="stop-a-review-that-hasnt-finished"></a>Tamamlanmadı bir gözden geçirme Durdur

Gözden geçirme zamanlanan bitiş tarihinden ulaştıysanız, bu kurmadı yönetici seçebilirsiniz **durdurmak** gözden erken sona erdirmek için. Gözden geçirme durdurduktan sonra kullanıcılar artık incelenebilir. Bunu durdurulduktan sonra bir gözden geçirme yeniden başlatılamıyor.

## <a name="apply-the-changes"></a>Değişiklikleri uygulayın 

Bir erişim gözden geçirme işlemi tamamlandıktan sonra ya da bitiş tarihi sınıra veya bir yönetici el ile durduruldu ve otomatik uygulama olduğundan değildi yapılandırılmış gözden geçirilmek üzere seçtiğiniz **Uygula** el ile değişiklikleri uygulamak için. Gözden geçirme sonucunu grup veya uygulama güncelleştirerek uygulanır. Bir yönetici bu seçenek seçtiğinde incelemede bir kullanıcının erişimi reddediliyorsa Azure AD kendi üyelik veya uygulama atama kaldırır. 

Bir erişim gözden geçirme tamamlandı ve otomatik uygulama sonra İnceleme durumunu tamamlandı Ara durumları arasında değişir ve son olarak uygulanan durumuna değiştirilir yapılandırıldı. Varsa, kaynak grubundan kaldırılıyor grubu üyeliği veya uygulama atama birkaç dakika içinde değiştirilirse reddedilen kullanıcıları görmek beklemeniz gerekir.

Gözden geçirme uygulama veya seçerek yapılandırılmış bir otomatik **Uygula** bir şirket içi dizin kaynaklanan bir grup veya dinamik bir grup üzerinde bir etkisi yoktur. Şirket içi kaynaklanan bir grubu değiştirmek istiyorsanız, sonuçları indirmek ve bu değişiklikleri bu dizine grubunda gösterimini uygulanır.

## <a name="download-the-results-of-the-review"></a>İnceleme sonuçlarını indirin

İnceleme sonuçları almak için seçin **onayları** ve ardından **karşıdan**. Sonuçta elde edilen CSV dosyasını Excel veya UTF-8 açmak diğer programları görüntülenebilir kodlanmış CSV dosyaları.

## <a name="optional-delete-a-review"></a>İsteğe bağlı: gözden geçirmeyi Sil
Artık incelemede düşünüyorsanız silebilirsiniz. Seçin **silmek** gözden Azure AD'den kaldırmak için.

> [!IMPORTANT]
> Silme işlemi gerçekleştirilmeden önce hiçbir uyarı yok, bu nedenle gözden silmek istediğinizden emin olun.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleri için programları ve denetimleri yönetme](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Bir grubun üyeleri veya bir uygulamaya erişim için erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](active-directory-privileged-identity-management-how-to-start-security-review.md)
