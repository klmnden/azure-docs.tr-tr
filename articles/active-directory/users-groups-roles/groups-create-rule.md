---
title: Dinamik grup oluşturma ve Azure Active Directory'de durumunu kontrol edin | Microsoft Docs
description: Azure portalında oluşturma bir grup üyeliği kuralları, durumunu kontrol edin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: d521406e37920dcd76c0078d2fdf54c16b7a0461
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209998"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Dinamik bir grup oluşturun ve durumunu denetle

Azure Active Directory'de (Azure AD), kullanıcı veya cihaz özelliklerine bağlı üyelik belirlemek için bir kural uygulayarak grupları oluşturabilirsiniz. Azure AD kullanıcı veya cihaz değişiklikleri özniteliklerini değerlendirirken, tüm dinamik Grup kurallarını Azure AD kiracısında ve herhangi gerçekleştirir ekler veya kaldırır. Bir grup için bir kural kullanıcı veya cihazın karşılayıp karşılamadığını bir üyesi olarak eklenir ve bunlar artık kural karşılamak, bunlar kaldırılır.

Bu makalede, Azure portalında güvenlik grupları veya Office 365 gruplarında dinamik üyelik için bir kural ayarlama işlemi açıklanmaktadır. Kuralın söz dizimi örneklerini ve desteklenen özellikleri, işleçler ve değerleri bir üyelik kuralı için tam bir listesi için bkz: [Azure Active Directory'de gruplar için dinamik Üyelik kuralları](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Bir grup üyeliği kuralı oluşturmak için

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) kiracısında genel yönetici, Intune Hizmet Yöneticisi veya kullanıcı hesabı yöneticisi rolü olan bir hesapla.
2. Seçin **grupları**.
3. Seçin **tüm grupları**seçip **yeni grup**.

   ![Yeni grubu eklemek](./media/groups-create-rule/new-group-creation.png)

4. Üzerinde **grubu** dikey penceresinde, bir ad ve yeni grup için bir açıklama girin. Seçin bir **üyelik türü** herhangi birinin **dinamik kullanıcı** veya **dinamik cihaz**kullanıcı veya cihaz için bir kural oluşturmak ve ardındanisteyipistemediğinizibağlıolarak**Dinamik sorgu Ekle**. Basit bir kural oluşturmak için kural Oluşturucusu'nu kullanın veya kendiniz bir üyelik kuralı yazma. Bu makale, üyelik kuralları örnekleri yanı sıra mevcut kullanıcı ve cihaz öznitelikleri hakkında daha fazla bilgi içerir.

   ![Dinamik üyelik kuralı ekle](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Üyelik sorgunuzu ekleyebilirsiniz müşteri uzantı özelliklerinin tam listesini görmek için seçin **Get müşteri uzantı özellikleri**benzersiz uygulama Kimliğini girin ve ardından **yenileme özellikleri**. Özelliklerin tam listesi şimdi seçmek kullanılabilir.
6. Bir kural oluşturduktan sonra seçin **Sorgu Ekle** dikey pencerenin alt kısmındaki.
7. Seçin **Oluştur** üzerinde **grubu** grubu oluşturmak için dikey pencere.

> [!TIP]
> Hatalı oluşturulmuş ya da geçerli girdiğiniz kural grubu oluşturma başarısız olur. Sağ üst köşede portalının kural neden işlenemedi açıklama içeren bir bildirim görüntülenir. Bunu nasıl geçerli hale getirmek için kural ayarlamak gerekir dikkatli bir şekilde anlamak için okuyun.

## <a name="check-processing-status-for-a-membership-rule"></a>Bir üyelik kuralı işleme durumunu denetleme

Durumu ve son güncelleştirme tarihi işleme üyelik gördüğünüz **genel bakış** grup için sayfa.
  
  ![dinamik grup durumunu görüntüleme](./media/groups-create-rule/group-status.png)

Aşağıdaki durum iletileri için gösterilen **üyelik işleme** durumu:

* **Değerlendirme**: Grup değişikliği alındı ve güncelleştirmeler değerlendirilir.
* **İşleme**: güncelleştirmeleri işleniyor.
* **Güncelleştirme tamamlandı**: işleme tamamlandı ve geçerli tüm güncelleştirmeleri yapıldı.
* **İşleme hatası**: Üyelik Kuralı değerlendirilirken bir hata oluştu ve işlem tamamlanamadı.
* **Güncelleştirme duraklatıldı**: dinamik üyelik kuralı güncelleştirmeleri yönetici tarafından duraklatıldı. MembershipRuleProcessingState "Paused" olarak ayarlanır.

Aşağıdaki durum iletileri için gösterilen **son güncelleştirme üyelik** durumu:

* &lt;**Tarih ve saat**&gt;: en son ne zaman üyelik güncelleştirildi.
* **Devam eden**: güncelleştirmeleri şu anda sürüyor.
* **Bilinmeyen**: son güncelleştirme zamanı alınamıyor. Yeni oluşturulan grubu nedeniyle olabilir.

Belirli bir grup üyeliği kuralı işlenirken bir hata meydana gelirse, üzerindeki bir uyarı gösterilir **genel bakış sayfasında** grubu için. Hayır dinamik üyelik güncelleştirmeleri Kiracı içindeki tüm gruplar için daha sonra 24 saat için işlenebilir varsa bir uyarı üzerindeki gösterilir **tüm grupları**.

![işleme hata iletisi](./media/groups-create-rule/processing-error.png)

Bu makaleler, Azure Active Directory içinde grupları hakkında ek bilgi sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
