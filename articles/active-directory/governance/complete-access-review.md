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
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4265a7e08eab079e55ce91b27142ec3e55b3f3e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579606"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Erişim gözden geçirmesi gruplarının tamamlamak ya da uygulamaları Azure ad erişim gözden geçirmeleri

Yöneticiler, bir uygulamaya atanmış grup üyeleri veya kullanıcılar için [bir erişim gözden geçirmesi oluşturmak](create-access-review.md) üzere Azure Active Directory’yi (Azure AD) kullanabilir. Azure AD, gözden geçirenler otomatik olarak erişim gözden geçirmek için isteyen bir e-posta gönderir. Bir kullanıcı bir e-posta almadıysanız, bunları yönergeleri gönderebilirsiniz [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md). (Davet gözden geçirme önce ilk kabul etmelidir gibi daveti kabul ancak gözden geçirenler olarak atanan konukların erişim gözden geçirmeleri, e-posta almaz unutmayın.) Erişim gözden geçirmesi dönemi bittikten sonra veya bir yönetici erişim gözden geçirmesi durduğunda bakın ve sonuçları uygulamak için bu makaledeki adımları izleyin.

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

## <a name="optional-delete-a-review"></a>İsteğe bağlı: Gözden geçirmeyi Sil
Artık incelemesindeki ilginizi çeken, silebilirsiniz. Seçin **Sil** gözden Azure AD'den kaldırılamadı.

> [!IMPORTANT]
> Silme işlemi gerçekleştirilmeden önce hiçbir uyarı yok, bu nedenle gözden geçirmeyi silmek istediğinizden emin olun.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](manage-guest-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleri için programları ve denetimleri yönetme](manage-programs-controls.md)
- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md)
