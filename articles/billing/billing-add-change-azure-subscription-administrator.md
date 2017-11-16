---
title: "Ekleme veya Azure yönetici abonelik rolleri değiştirme | Microsoft Docs"
description: "Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi ekleme veya değiştirme açıklar"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/19/2017
ms.author: genli
ms.openlocfilehash: 091fe208783a2fe5d5c91abe4ec498bf760a3eb3
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Ekleme veya abonelik ya da hizmetleri yönetmek Azure yönetici rollerini değiştirme

Azure aboneliğiniz yönetir veya aboneliğinizde kullanılan Azure hizmetlerini yöneten Azure yönetici değiştirebilirsiniz. Azure faturalama bilgilerini görüntülemek ve Aboneliklerini yönetmek için hesap merkezi hesabı yönetici olarak oturum gerekir. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Azure portalında bir abonelik için RBAC sahip yönetici ekleme 

Birisi bir abonelik Azure portalında yönetici olarak eklemek için onların öneririz bir [RBAC](../active-directory/role-based-access-control-configure.md) sahip rolü. Sahip rolü atanmış ve diğer aboneliklere erişim ayrıcalığına sahip değil Bu Abonelikteki kaynakları yönetebilir. Eklediğiniz aracılığıyla sahipleri [Azure portal](https://portal.azure.com) kaynağında yönetemez [Klasik Azure portalı](https://manage.windowsazure.com).

1. Oturum [abonelikleri görmek Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Erişim için yönetici istediğiniz aboneliği seçin.
1. Seçin **erişim denetimi (IAM)** menüde.
1. Seçin **ekleme** > **rol** > **sahibi**. Sahibi olarak eklemek istediğiniz kullanıcıyı seçin ve ardından istediğiniz kullanıcının e-posta adresini yazın **kaydetmek**.

    ![Seçili sahip rolünü gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-role.png)

## <a name="add-or-change-co-administrator"></a>Ekleme veya ortak yöneticiyi değiştirme

Yalnızca bir sahip bir ortak yönetici eklenebilir. Rolleri katkıda bulunan ve okuyucu gibi diğer kullanıcılarla ortak yönetici olarak eklenemez.

1. Henüz yapmadıysanız, birisi üstten yönergeleri izleyerek sahibi olarak ekleyin.
2. **Sağ** sahibi kullanıcı, yeni eklediğiniz ve ardından **ortak yönetici olarak Ekle**. Görmüyorsanız, **ortak yönetici olarak Ekle** seçeneğini yeni sayfa veya başka bir Internet tarayıcısı deneyin. 

     ![Ortak yönetici ekler ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Kullanıcının Azure hizmetleri yönetmek gerekirse "Sahip" hesabı ortak yönetici eklemek gereken [Klasik Azure portalı](https://manage.windowsazure.com/).

    Ortak yönetici izni kaldırmak için **sağ** seçin ve "Ortak Yöneticisi" kullanıcı **kaldırmak ortak yönetici**.

    ![Ortak yönetici kaldırır ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Bir Azure aboneliği için Hizmet Yöneticisi değiştirme

Yalnızca Hesap Yöneticisi bir aboneliği için Hizmet Yöneticisi değiştirebilirsiniz. Oturum açtığınızda varsayılan olarak, Hizmet Yöneticisi olarak Hesap Yöneticisi aynıdır.

1. Senaryonuz denetleyerek desteklendiğinden emin olun [hizmet yöneticileri değiştirmek için sınırları](#limits).
1. Oturum [hesap Merkezi'nde](https://account.windowsazure.com/subscriptions) hesap yöneticisi olarak.
1. Bir abonelik seçin.
1. Sağ tarafta seçin **Düzenle abonelik ayrıntıları**.

    ![Hesap Merkezi'nde Düzenle abonelik düğmesini gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. İçinde **Hizmet Yöneticisi** kutusuna, yeni Hizmet Yöneticisi'nin e-posta adresi girin.

    ![Hizmet Yöneticisi e-posta değiştirmek için kutusunu gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Hizmet yöneticileri değiştirmek için sınırlamalar

* Her abonelik, Azure AD dizini ile ilişkilidir. Abonelik ile ilişkili dizine bulmak için Git [Klasik Azure portalı](https://manage.windowsazure.com/)seçin **ayarları** > **abonelikleri**. Dizini bulmak için abonelik Kimliğini denetleyin.
* Bir iş veya Okul hesabıyla oturum açmış durumdaysanız, Hizmet Yöneticisi olarak, kuruluşunuzdaki diğer hesapları ekleyebilirsiniz. Örneğin, abby@contoso.com ekleyebilirsiniz bob@contoso.com olarak Hizmet Yöneticisi ekleyemezsiniz, ancak john@notcontoso.com sürece john@notcontoso.com contoso.com Directory'de durum vardır. İş veya Okul hesaplarıyla oturum imzalı kullanıcılar Hizmet Yöneticisi olarak Microsoft Account kullanıcıları eklemeye devam edebilirsiniz.

  | Oturum açma yöntemi | Microsoft Account user SA olarak eklensin mi? | SA ile aynı kuruluşta bir iş veya Okul hesabı eklensin mi? | SA olarak farklı bir kuruluşta bir iş veya Okul hesabı eklensin mi? |
  | --- | --- | --- | --- |
  |  Microsoft Hesabı |Evet |Hayır |Hayır |
  |  İş veya Okul hesabı |Evet |Evet |Hayır |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Bir Azure aboneliği için Hesap Yöneticisi değiştirme

Bir aboneliğin Hesap Yöneticisi değiştirmek için bkz: [başka bir hesap için bir Azure aboneliği sahipliğini aktarma](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Emin değil hesap yöneticisi olan?** Şu adımları uygulayın:

1. Oturum [abonelikleri görmek Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Denetleyin ve altında aramak istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.  

## <a name="types-of-azure-admin-accounts"></a>Azure Yönetici hesap türleri

 Hesap Yöneticisi, Hizmet Yöneticisi ve ortak yönetici üç Microsoft Azure yönetici rolleri türleridir. Aşağıdaki tabloda, bu üç yönetici rolleri arasındaki farkı açıklar.

| Yönetim rolü | Sınır | Açıklama |
| --- | --- | --- |
| Hesap Yöneticisi (AA) |Azure hesabı başına 1 |Bu kaydolup veya Azure aboneliklerinin satın aldınız ve erişim yetkisi kişidir [hesap Merkezi'nde](https://account.azure.com/Subscriptions) ve çeşitli yönetim görevlerini gerçekleştirebilirsiniz. Bunlar, abonelikleri oluşturma, aboneliklerinizi iptal edin, bir abonelik için faturalama değiştirin ve Hizmet Yöneticisi değiştirmek çalışabilme içerir. |
| Hizmet Yöneticisi (SA) |Azure abonelik başına 1 |Bu rol hizmetleri yönetmeye yetkilidir [Azure portal](https://portal.azure.com). Varsayılan olarak, yeni bir abonelik için Hesap Yöneticisi ayrıca hizmet yöneticisidir. |
| Ortak yönetici (CA) [Klasik Azure portalı](https://manage.windowsazure.com) |Abonelik başına 200 |Bu rol Hizmet Yöneticisi ile aynı erişim ayrıcalıklarına sahiptir, ancak aboneliklerin Azure dizinleriyle ilişkisini değiştiremez. |

Azure Active Directory rol tabanlı erişim denetimi (RBAC), kullanıcıların birden çok rol eklenmesine izin verir. Daha fazla bilgi için bkz: [Azure Active Directory rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).


## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Kaynak erişim denetimi ve Active Directory hakkında daha fazla bilgi edinin

* Microsoft Azure'da kaynak erişiminin nasıl denetlendiğini hakkında daha fazla bilgi için bkz: [azure'da kaynak erişimini anlama](../active-directory/active-directory-understanding-resource-access.md).
* Azure Active Directory hakkında daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](../active-directory/active-directory-how-subscriptions-associated-directory.md) ve [Azure Active Directory'de yönetici rolleri atama](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
