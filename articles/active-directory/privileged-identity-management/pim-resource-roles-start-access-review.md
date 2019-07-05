---
title: PIM - Azure Active Directory erişim gözden geçirmesi Azure kaynağı rolleri oluşturun | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) erişim gözden geçirmesi Azure kaynağı rolleri oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0067bd6dc2f47c5460220295d486910d9195782d
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476261"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-pim"></a>PIM'de erişim gözden geçirmesi Azure kaynağı rolleri oluşturma

Çalışanlar için ayrıcalıklı bir Azure kaynak rolleri için erişim, zaman içinde değişir. Eski rol atamaları ile ilişkili riski azaltmak için düzenli olarak erişim gözden geçirmelisiniz. Azure kaynağı rolleri ayrıcalıklı erişim gözden geçirmeleri oluşturmak için Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullanabilirsiniz. Ayrıca, otomatik olarak gerçekleşen yinelenen bir erişim gözden geçirmeleri yapılandırabilirsiniz.

Bu makalede, bir veya daha fazla erişim gözden geçirmeleri ayrıcalıklı Azure kaynak rolleri için oluşturmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar

- [Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Açık erişim gözden geçirmeleri

1. Oturum [Azure portalında](https://portal.azure.com/) ayrıcalıklı rol yöneticisi rolünün bir üyesi olan bir kullanıcı ile.

1. Açık **Azure AD Privileged Identity Management**.

1. Sol menüde **Azure kaynaklarını**.

1. Bir abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağa tıklayın.

1. Yönet altında **erişim gözden geçirmeleriyle**.

    ![Azure kaynakları - tüm incelemeleri durumunu gösteren listesini erişim gözden geçirmeleri](./media/pim-resource-roles-start-access-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Erişim değerlendirmesi başlatma

Erişim gözden geçirmesi ayarları belirttikten sonra tıklayın **Başlat**. Erişim gözden geçirmesi listenizi durumu göstergesi görünür.

![Erişim gözden geçirmeleri kullanmaya başlama gözden geçirme durumu gösteren listesi](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

İnceleme kısa bir süre içinde başladıktan sonra varsayılan olarak, Azure AD için gözden geçirenler bir e-posta gönderir. Erişim gözden geçirmesi tamamlanmalarını bekliyor gözden geçirenlere bildirmek e-posta gönderin, Azure AD almamayı tercih ederseniz unutmayın. Nasıl yapılır yönergeleri Göster [Azure kaynak rolleri için erişim gözden geçirme](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesi yönetme

Gözden geçirenler, üzerinde kendi incelemeler tamamlandı olarak ilerleme durumunu izleyebilir **genel bakış** erişim gözden geçirmesi sayfası. Erişim hakları dizine kadar değişen [gözden geçirme tamamlandığında](pim-resource-roles-complete-access-review.md).

![Erişim gözden geçirmeleri incelenmesi ayrıntılarını gösteren genel bakış sayfası](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Bu tek seferlik bir gözden geçirme ise, ardından yönetici erişim gözden geçirmesi durdurur veya erişim incelemesi süresi bittikten sonra adımları [Azure kaynak rolleri erişim değerlendirmesi tamamlama](pim-resource-roles-complete-access-review.md) bakın ve sonuçları uygulamak için.  

Bir dizi erişim yönetmek için gözden geçirmeleri erişim gözden gidin ve zamanlanmış incelemelerde yaklaşan yinelemesi Bul ve bitiş tarihi Düzenle veya kaldıracak ekleme/gözden geçirenler uygun şekilde kaldırma.

Yaptığınız seçimlere göre **tamamlama ayarlarını bağlı**, otomatik olarak Uygula incelemesinin bitiş tarihi veya el ile ne zaman gözden geçirmeyi durdurmak sonra yürütülür. Gözden geçirme durumu değiştirilecek **tamamlandı** gibi ara durumları arasında **uygulama** ve son durumuna **uygulanan**. Reddedilen kullanıcılar varsa birkaç dakika içinde rollerinden kaldırılıyor görmeyi beklemelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynak rolleri için erişim gözden geçirin](pim-resource-roles-perform-access-review.md)
- [Azure kaynak rolleri erişim değerlendirmesi tamamlama](pim-resource-roles-complete-access-review.md)
- [Azure AD rolleri, erişim gözden geçirmesi oluştur](pim-how-to-start-security-review.md)
