---
title: PIM - Azure Active Directory erişim gözden geçirmesi Azure AD rolleri oluşturun | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) erişim gözden geçirmesi Azure AD rolleri oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/27/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a0680ddf2c9e654455933bf09699ab81e8ab65d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141655"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-pim"></a>PIM'de erişim gözden geçirmesi Azure AD rolleri oluşturma

Erişim zamanla çalışanlar değişikliklerin Azure AD rolleri ayrıcalıklı. Eski rol atamaları ile ilişkili riski azaltmak için düzenli olarak erişim gözden geçirmelisiniz. Azure AD rolleri ayrıcalıklı erişim gözden geçirmeleri oluşturmak için Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullanabilirsiniz. Ayrıca, otomatik olarak gerçekleşen yinelenen bir erişim gözden geçirmeleri yapılandırabilirsiniz.

Bu makalede, Azure AD rolleri ayrıcalıklı için bir veya daha fazla erişim gözden geçirmeleri oluşturmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar

- [Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Açık erişim gözden geçirmeleri

1. Oturum [Azure portalında](https://portal.azure.com/) ayrıcalıklı rol yöneticisi rolünün bir üyesi olan bir kullanıcı ile.

1. Açık **Azure AD Privileged Identity Management**.

1. Sol menüde **Azure AD rolleri** ve ardından **erişim gözden geçirmeleriyle**.

1. Yönet altında **erişim gözden geçirmeleriyle**.

    ![Azure AD rolleri - erişim gözden geçirmeleri](./media/pim-how-to-start-security-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Erişim değerlendirmesi başlatma

Erişim gözden geçirmesi ayarları belirttikten sonra tıklayın **Başlat**. Erişim gözden geçirmesi listenizi durumu göstergesi görünür.

![Erişim incelemeleri listesi](./media/pim-how-to-start-security-review/access-reviews-list.png)

İnceleme kısa bir süre içinde başladıktan sonra varsayılan olarak, Azure AD için gözden geçirenler bir e-posta gönderir. Erişim gözden geçirmesi tamamlanmalarını bekliyor gözden geçirenlere bildirmek e-posta gönderin, Azure AD almamayı tercih ederseniz unutmayın. Nasıl yapılır yönergeleri Göster [Azure AD rolleri için erişim gözden geçirme](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesi yönetme

Gözden geçirenler, üzerinde kendi incelemeler tamamlandı olarak ilerleme durumunu izleyebilir **genel bakış** erişim gözden geçirmesi sayfası. Erişim hakları dizine kadar değişen [gözden geçirme tamamlandığında](pim-how-to-complete-review.md).

![Erişim gözden geçirmeleri ilerleme durumu](./media/pim-how-to-start-security-review/access-review-overview.png)

Bu tek seferlik bir gözden geçirme ise, ardından yönetici erişim gözden geçirmesi durdurur veya erişim incelemesi süresi bittikten sonra adımları [rollerin Azure AD erişim gözden geçirmesi tamamlama](pim-how-to-complete-review.md) bakın ve sonuçları uygulamak için.  

Bir dizi erişim yönetmek için gözden geçirmeleri erişim gözden gidin ve zamanlanmış incelemelerde yaklaşan yinelemesi Bul ve bitiş tarihi Düzenle veya kaldıracak ekleme/gözden geçirenler uygun şekilde kaldırma.

Yaptığınız seçimlere göre **tamamlama ayarlarını bağlı**, otomatik olarak Uygula incelemesinin bitiş tarihi veya el ile ne zaman gözden geçirmeyi durdurmak sonra yürütülür. Gözden geçirme durumu değiştirilecek **tamamlandı** gibi ara durumları arasında **uygulama** ve son durumuna **uygulanan**. Reddedilen kullanıcılar varsa birkaç dakika içinde rollerinden kaldırılıyor görmeyi beklemelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD rolleri için erişim gözden geçirin](pim-how-to-perform-security-review.md)
- [Azure AD rolleri erişim değerlendirmesi tamamlama](pim-how-to-complete-review.md)
- [Azure kaynak rolleri erişim gözden geçirmesi oluştur](pim-resource-roles-start-access-review.md)
