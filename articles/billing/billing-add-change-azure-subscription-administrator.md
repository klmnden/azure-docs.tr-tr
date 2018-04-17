---
title: Ekleme veya Azure yönetici abonelik rolleri değiştirme | Microsoft Docs
description: Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi ekleme veya değiştirme açıklar
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/04/2018
ms.author: genli
ms.openlocfilehash: ecee98e9b74613a4176d20d231b32e4cb99a721e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="add-or-change-azure-subscription-administrators"></a>Ekleme veya Azure aboneliği yöneticileri değiştirme

Azure Klasik abonelik yöneticileri ve Azure [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) Azure kaynaklarına erişimi yönetmek için iki sistemleri:

* Klasik abonelik yöneticisi rollerini teklif temel erişim yönetimi ve Hesap Yöneticisi, Hizmet Yöneticisi ve ortak Yöneticiler içerir.
    * Yeni bir Azure aboneliği için kaydolduğunuzda, hesabınızın varsayılan olarak Hesap Yöneticisi ve Hizmet Yöneticisi ayarlanır.
    * Ortak yönetici işaretinden sonra eklenebilir.
* RBAC ayrıntılı erişim yönetimi birçok yerleşik roller, kapsam ve özel roller esnekliğini sunar, daha yeni bir sistemdir.
    * Ancak, yalnızca RBAC rolleri ve hiçbir Klasik abonelik yönetici rollerine sahip kullanıcılar Azure Klasik dağıtımlar yönetemezsiniz.

Daha iyi denetim sağlamak ve erişim yönetimini basitleştirmek için tüm erişim yönetimi ihtiyaçları için RBAC kullanmanızı öneririz. Mümkünse, varolan erişim ilkeleri RBAC kullanarak yeniden yapılandırmanız önerilir. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Azure portalında bir abonelik için RBAC sahip yönetici ekleme 

Birisi Azure aboneliği hizmet yönetimi için yönetici olarak eklemek için bunları RBAC sahip rolünü aboneliğine verin. Sahip rolü atanmış ve diğer aboneliklere erişim ayrıcalığına sahip değil Bu Abonelikteki kaynakları yönetebilir.

1. Ziyaret [ **abonelikleri** Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Erişim vermek istediğiniz aboneliği seçin.
3. Seçin **Ekle**  
   (Ekle düğmesini eksikse, izinleri eklemek için izniniz yok.)
4. Seçin **erişim denetimi (IAM)** menüde.
5. İçinde **rol** kutusunda **sahibi**. 
6. İçinde **atamak için erişim** kutusunda **Azure AD kullanıcı, Grup veya uygulama**. 
7. İçinde **seçin** sahibi olarak eklemek istediğiniz kullanıcının e-posta adresini yazın. Kullanıcıyı seçin ve ardından **kaydetmek**.

    ![Seçili sahip rolünü gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Bu temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara kullanıcı tam erişim sağlar. Bir kaynak grubu gibi farklı bir kapsamda erişim vermek için bu kapsamı için IAM menü ziyaret edin. 

## <a name="add-or-change-co-administrator"></a>Ekleme veya ortak yöneticiyi değiştirme

Yalnızca bir sahip bir ortak yönetici eklenebilir. Rolleri katkıda bulunan ve okuyucu gibi diğer kullanıcılarla ortak yönetici olarak eklenemez.

> [!TIP]
> Yalnızca kullanıcının Azure Klasik dağıtımları yönetmek gerekirse "Sahip" hesabı ortak yönetici olarak eklemeniz gerekir. Diğer amaçlar için RBAC kullanmanızı öneririz.

1. Henüz yapmadıysanız, birisi üstten yönergeleri izleyerek sahibi olarak ekleyin.
2. **Sağ** sahibi kullanıcı, yeni eklediğiniz ve ardından **ortak yönetici olarak Ekle**. Görmüyorsanız, **ortak yönetici olarak Ekle** seçeneği, sayfayı yenileyin veya başka bir Internet tarayıcısı deneyin. 

    ![Ortak yönetici ekler ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Ortak yönetici izni kaldırmak için **sağ** seçin ve "Ortak Yöneticisi" kullanıcı **kaldırmak ortak yönetici**.

    ![Ortak yönetici kaldırır ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Bir Azure aboneliği için Hizmet Yöneticisi değiştirme

Yalnızca Hesap Yöneticisi bir aboneliği için Hizmet Yöneticisi değiştirebilirsiniz. Oturum açtığınızda varsayılan olarak, Hizmet Yöneticisi olarak Hesap Yöneticisi aynıdır. Hizmet Yöneticisi başka bir kullanıcı tarafından değiştirilirse, Hesap Yöneticisi Azure portalına erişim kaybeder. Ancak, Hesap Yöneticisi hizmet Yöneticisi geri kendilerini değiştirmek için her zaman hesap Merkezi'ni kullanabilirsiniz.

1. Senaryonuz denetleyerek desteklendiğinden emin olun [hizmet yöneticileri değiştirmek için sınırları](#limits).
1. Oturum [hesap Merkezi'nde](https://account.windowsazure.com/subscriptions) hesap yöneticisi olarak.
1. Bir abonelik seçin.
1. Sağ tarafta seçin **Düzenle abonelik ayrıntıları**.

    ![Hesap Merkezi'nde Düzenle abonelik düğmesini gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. İçinde **Hizmet Yöneticisi** kutusuna, yeni Hizmet Yöneticisi'nin e-posta adresi girin.

    ![Hizmet Yöneticisi e-posta değiştirmek için kutusunu gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Hizmet yöneticileri değiştirmek için sınırlamalar

* Her abonelik, Azure AD dizini ile ilişkilidir. Abonelik ile ilişkili dizine bulmak için Git [ **abonelikleri**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), dizin görmek için bir abonelik seçin.
* Bir iş veya Okul hesabıyla oturum açmış durumdaysanız, Hizmet Yöneticisi olarak, kuruluşunuzdaki diğer hesapları ekleyebilirsiniz. Örneğin, abby@contoso.com ekleyebilirsiniz bob@contoso.com olarak Hizmet Yöneticisi ekleyemezsiniz, ancak john@notcontoso.com sürece john@notcontoso.com contoso.com Directory'de durum vardır. İş veya Okul hesaplarıyla oturum imzalı kullanıcılar Hizmet Yöneticisi olarak Microsoft Account kullanıcıları eklemeye devam edebilirsiniz.

  | Oturum açma yöntemi | Microsoft Account user SA olarak eklensin mi? | SA ile aynı kuruluşta bir iş veya Okul hesabı eklensin mi? | SA olarak farklı bir kuruluşta bir iş veya Okul hesabı eklensin mi? |
  | --- | --- | --- | --- |
  |  Microsoft Hesabı |Evet |Hayır |Hayır |
  |  İş veya Okul hesabı |Evet |Evet |Hayır |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Bir Azure aboneliği için Hesap Yöneticisi değiştirme

Hesap Yöneticisi başlangıçta Azure aboneliği için kaydolup ve abonelik faturalama sahibi olarak sorumlu olan kullanıcıdır. Bir aboneliğin Hesap Yöneticisi değiştirmek için bkz: [başka bir hesap için bir Azure aboneliği sahipliğini aktarma](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Emin değil hesap yöneticisi olan?** Şu adımları uygulayın:

1. Ziyaret [ **abonelikleri** Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Denetleyin ve altında aramak istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.  

## <a name="types-of-classic-subscription-admins"></a>Klasik abonelik yöneticileri türleri

 Hesap Yöneticisi, Hizmet Yöneticisi ve ortak yönetici üç azure'da Klasik Abonelik Yöneticisi rolleri türleridir. Azure için kaydolmak için kullandığınız hesap Hesap Yöneticisi ve Hizmet Yöneticisi otomatik olarak ayarlanır. Sonra ek ortak Yöneticiler eklenebilir. Aşağıdaki tabloda, bu üç yönetici rolleri arasındaki tam farklar açıklanmaktadır. 

> [!TIP]
> Daha iyi denetim ve ayrıntılı erişim yönetimi için Azure rol tabanlı erişim denetimi (birden çok role eklenecek kullanıcıların veren RBAC), kullanmanızı öneririz. Daha fazla bilgi için bkz: [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

| Klasik abonelik yöneticisi | Sınır | Açıklama |
| --- | --- | --- |
| Hesap Yöneticisi (AA) |Azure hesabı başına 1 |Azure aboneliği için kaydolup ve erişimi için yetkilendirilmiş kullanıcı budur [hesap Merkezi'nde](https://account.azure.com/Subscriptions) ve çeşitli yönetim görevlerini gerçekleştirebilirsiniz. Bunlar, yeni abonelikler oluşturun, aboneliklerinizi iptal edin, bir abonelik için faturalama değiştirin ve Hizmet Yöneticisi değiştirmek çalışabilme içerir. Kavramsal olarak, Hesap Yöneticisi abonelik faturalama sahibidir. RBAC, hesap yöneticinize bir rolü atanmamış.|
| Hizmet Yöneticisi (SA) |Azure abonelik başına 1 |Bu rol hizmetleri yönetmeye yetkilidir [Azure portal](https://portal.azure.com). Varsayılan olarak, yeni bir abonelik için Hesap Yöneticisi ayrıca hizmet yöneticisidir. RBAC, hizmet yöneticisine abonelik kapsamda sahip rolünü verilir.|
| Ortak yönetici (CA) |Abonelik başına 200 |Bu rol Hizmet Yöneticisi ile aynı erişim ayrıcalıklarına sahiptir, ancak aboneliklerin Azure dizinleriyle ilişkisini değiştiremez. RBAC, abonelik kapsamında ortak yönetici için sahip rolünü verilir.|

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Kaynak erişim denetimi ve Active Directory hakkında daha fazla bilgi edinin

* Microsoft Azure'da kaynak erişiminin nasıl denetlendiğini hakkında daha fazla bilgi için bkz: [azure'da kaynak erişimini anlama](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Azure Active Directory hakkında daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](../active-directory/active-directory-how-subscriptions-associated-directory.md) ve [Azure Active Directory'de yönetici rolleri atama](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
