---
title: Dinamik bir grup oluşturun ve durum - Azure Active Directory denetleme | Microsoft Docs
description: Azure portalında bir grup üyeliği kuralı oluşturmak durumunu denetlemek nasıl.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f828ff83e6b9c60eb08edef7f47e88185fb5aef8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60472179"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Dinamik bir grup oluşturun ve durumunu denetle

Azure Active Directory'de (Azure AD), kuralları, kullanıcı veya cihaz özelliğe göre grup üyeliği belirlemek için kullanabilirsiniz. Bu makalede, Azure portalında dinamik bir grup için bir kural ayarlamak anlatır.
Dinamik üyelik güvenlik grupları veya Office 365 grupları için desteklenir. Bir grup üyeliği kuralı uygulandığında, kullanıcı ve cihaz öznitelikleri ile üyelik kuralı bir eşleşme için değerlendirilir. Öznitelik, bir kullanıcı veya cihaz için değiştiğinde, kuruluştaki tüm dinamik Grup kurallarını üyelik değişiklikleri için işlenir. Kullanıcılara ve cihazlara eklenir veya bunlar bir grup koşulları karşılıyorsa kaldırılır.

Söz dizimi, desteklenen özellikler, işleçler ve değerleri için bir üyelik kuralı örnekleri için bkz. [Azure Active Directory'de gruplar için dinamik Üyelik kuralları](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Bir grup üyeliği kuralı oluşturmak için

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) genel yönetici, Intune yönetici veya Kiracı Yöneticisi rolünde kullanıcı olan bir hesapla.
2. Seçin **grupları**.
3. Seçin **tüm grupları**seçip **yeni grup**.

   ![Yeni grubu eklemek için komutu seçin](./media/groups-create-rule/new-group-creation.png)

4. Üzerinde **grubu** sayfasında, bir ad ve yeni grup için bir açıklama girin. Seçin bir **üyelik türü** için kullanıcılara veya cihazlara tıklayın ve ardından **dinamik sorgu Ekle**. Basit bir kural oluşturmak için kural Oluşturucu'yu kullanabilirsiniz veya [kendiniz bir üyelik kuralı yazma](groups-dynamic-membership.md).

   ![Dinamik Grup Üyeliği Kuralı Ekle](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Üyelik sorgunuz için kullanılabilen özel uzantı özellikleri görmek için
   1. Seçin **özel uzantı özelliklerini alma**
   2. Uygulama Kimliğini girin ve ardından **yenileme özellikleri**. 
6. Bir kural oluşturduktan sonra seçin **Sorgu Ekle** dikey pencerenin alt kısmındaki.
7. Seçin **Oluştur** üzerinde **grubu** grubu oluşturmak için dikey pencere.

Girdiğiniz kuralı geçerli değilse, kural neden işlenemedi açıklama portalının sağ üst köşesinde görüntülenir. Bu hatayı düzeltmek nasıl dikkatli bir şekilde anlamak için okuyun.

## <a name="turn-on-or-off-welcome-email"></a>Hoş Geldiniz e-posta açma veya kapatma

Yeni bir Office 365 grubu oluşturulduğunda, gruba eklenen kullanıcılar Hoş Geldiniz bir bildirim gönderilir. Daha sonra bir kullanıcı veya cihaz herhangi bir özniteliği değiştirirseniz, kuruluştaki tüm dinamik Grup kurallarını üyelik değişiklikleri için işlenir. Eklenen kullanıcılar da ardından Hoş Geldiniz bildirimleri alır. Bu davranışı devre dışı bırakabilirsiniz [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps). 

## <a name="check-processing-status-for-a-rule"></a>Bir kural işleme durumunu denetleme

Durumu ve son güncelleştirme tarihi işleme üyelik gördüğünüz **genel bakış** grup için sayfa.
  
  ![dinamik grup durumunu görüntüleme](./media/groups-create-rule/group-status.png)

Aşağıdaki durum iletileri için gösterilen **üyelik işleme** durumu:

* **Değerlendirme**:  Grup değişikliğinin alındı ve güncelleştirmeler değerlendirilir.
* **İşleme**: Güncelleştirmeleri işlenmekte olan.
* **Güncelleştirme tamamlandı**: İşleme tamamlandı ve geçerli tüm güncelleştirmeleri yapıldı.
* **İşleme hatası**:  Üyelik Kuralı değerlendirilirken bir hata nedeniyle işlem tamamlanamadı.
* **Güncelleştirme duraklatıldı**: Dinamik üyelik kuralı güncelleştirmeleri yönetici tarafından duraklatıldı. MembershipRuleProcessingState "Paused" olarak ayarlanır.

Aşağıdaki durum iletileri için gösterilen **son güncelleştirme üyelik** durumu:

* &lt;**Tarih ve saat**&gt;: Son üyelik güncelleştirildi.
* **Devam eden**: Şu anda devam eden güncelleştirmelerin.
* **Bilinmeyen**: Son güncelleştirme zamanı alınamıyor. Grubun yeni olabilir.

Belirli bir grup üyeliği kuralı işlenirken bir hata meydana gelirse, üzerindeki bir uyarı gösterilir **genel bakış sayfasında** grubu için. Hayır dinamik üyelik güncelleştirmeleri Kiracı içindeki tüm gruplar için daha sonra 24 saat için işlenebilir varsa bir uyarı üzerindeki gösterilir **tüm grupları**.

![hata iletisi uyarıları işleme](./media/groups-create-rule/processing-error.png)

Bu makaleler, Azure Active Directory içinde grupları hakkında ek bilgi sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
