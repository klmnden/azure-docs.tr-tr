---
title: Azure yönetici abonelik rollerini ekleme veya değiştirme | Microsoft Docs
description: Ekleme veya Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi değiştirme açıklar
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
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: genli
ms.openlocfilehash: 97210c0d9dba9c4130b1da9ad17a257ff1d81b42
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450277"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Ekleme veya Azure aboneliği yöneticileri değiştirme

Azure kaynaklarına erişimi yönetmek için uygun yönetici rolüne sahip olmalıdır. Bu makalede, ekleme veya değiştirme abonelik düzeyinde bir kullanıcı için yönetici rolünü açıklar.

## <a name="what-administrator-role-do-i-use"></a>Yönetici rolü kullanabilirim?

Azure, birçok farklı rol yok. Kaynaklara erişimi yönetmek için Klasik Abonelik Yöneticisi rolleri, Hizmet Yöneticisi ve ortak yönetici veya rol tabanlı erişim denetimi (RBAC) adı verilen yeni bir yetkilendirme sistemine gibi kullanabilirsiniz. Daha iyi denetim sağlamak ve erişim yönetimini basitleştirmek için tüm erişim yönetimi gereksinimleriniz için RBAC kullanmanızı öneririz. Mümkünse, RBAC kullanarak var olan erişim ilkelerini yeniden yapılandırmanız önerilir. Daha fazla bilgi için [rol tabanlı erişim denetimi (RBAC) nedir](../role-based-access-control/overview.md) ve [farklı rolleri Azure anlamak](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Azure portalında RBAC sahibi aboneliği ekleme 

Birisi Azure aboneliğiniz için yönetici olarak eklemek için bunları atayın [sahibi](../role-based-access-control/built-in-roles.md#owner) abonelik kapsamında rol (RBAC rolü). Sahip rolüne atanan ve diğer aboneliklere erişimi ayrıcalığına sahip değil Bu Abonelikteki kaynakları yönetebilir.

1. Ziyaret [ **abonelikleri** Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Erişim vermek istediğiniz aboneliği seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
   (Ekle düğmesini eksikse, izinler eklemek için izniniz yok.)
4. Seçin **erişim denetimi (IAM)** listesinde.
5. İçinde **rol** kutusunda **sahibi**. 
6. İçinde **erişim Ata** kutusunda **Azure AD kullanıcı, Grup veya uygulama**. 
7. İçinde **seçin** sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın. Kullanıcıyı seçin ve ardından **Kaydet**.

    ![Seçili sahip rolü gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Bu, tüm kaynaklara temsilci erişimi başkalarına hakkı dahil olmak üzere kullanıcı tam erişim sağlar. Bir kaynak grubu gibi farklı bir kapsamda erişmenizi sağlayacak ziyaret **erişim denetimi (IAM)** dikey penceresinde bu kapsamı için.

## <a name="add-or-change-co-administrator"></a>Ekleme veya ortak yönetici değiştirme

Yalnızca bir [sahibi](../role-based-access-control/built-in-roles.md#owner) ortak yönetici olarak eklenebilir. Rolleri gibi diğer kullanıcılarla [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) ve [okuyucu](../role-based-access-control/built-in-roles.md#reader) ortak Yöneticiler olarak eklenemez.

> [!TIP]
> Yalnızca Azure Klasik dağıtımlarını yönetmek kullanıcının erişmesi gerekiyorsa sahibi ortak yönetici olarak eklemeniz gerekir. Diğer amaçlar için RBAC kullanmanızı öneririz.

1. Henüz yapmadıysanız, birisi yukarıdaki yönergeleri izleyerek bir sahibi olarak ekleyin.
2. **Sağ** sahip kullanıcı, yeni eklediğiniz ve ardından **ortak yönetici olarak Ekle**. Görmüyorsanız, **ortak yönetici olarak Ekle** seçeneği, sayfayı yenileyin veya başka bir Internet tarayıcısı deneyin. 

    ![Ortak yönetici ekler ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Ortak yönetici izni kaldırmak için **sağ** seçin ve ortak yönetici kullanıcı **ortak yöneticiyi kaldırmak**.

    ![Ortak yönetici kaldırır ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Bir Azure aboneliğine yönelik hizmet yöneticisini değiştiremez

Yalnızca Hesap Yöneticisi, bir aboneliği için Hizmet Yöneticisi olarak değiştirebilirsiniz. Oturum açtığınızda varsayılan olarak, Hizmet Yöneticisi hesap yöneticisi olarak aynıdır. Hizmet Yöneticisi başka bir kullanıcı tarafından değiştirilirse, Hesap Yöneticisi Azure Portalı'na erişimi kaybeder. Ancak, Hesap Yöneticisi hizmet Yöneticisi geri kendilerini değiştirmek için her zaman hesap merkezi kullanabilirsiniz.

1. Senaryonuz denetimi tarafından desteklenen emin [hizmet yöneticileri değiştirmek için sınırları](#limits).
1. Oturum [hesap Merkezi](https://account.windowsazure.com/subscriptions) hesap yöneticisi olarak.
1. Bir abonelik seçin.
1. Sağ tarafta seçin **Abonelik Ayrıntıları Düzenle**.

    ![Hesap Merkezi'nde abonelik Düzenle düğmesini gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. İçinde **Hizmet Yöneticisi** kutusunda, yeni bir Hizmet Yöneticisi e-posta adresini girin.

    ![Hizmet Yöneticisi e-posta değiştirmek için kutusunu gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Hizmet yöneticileri değiştirmeye ilişkin sınırlamalar

* Her abonelik bir Azure AD dizini ile ilişkilidir. Aboneliği ile ilişkili dizini bulunacak Git [ **abonelikleri**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), ardından dizini görmek için bir abonelik seçin.
* Bir iş veya Okul hesabıyla oturum açtıysanız, Hizmet Yöneticisi olarak kuruluşunuzdaki diğer hesapları ekleyebilirsiniz. Örneğin, abby@contoso.com ekleyebilirsiniz bob@contoso.com olarak Hizmet Yöneticisi, ancak ekleyemez john@notcontoso.com sürece john@notcontoso.com contoso.com dizinde faaliyet. İş veya Okul hesaplarıyla oturum açmış kullanıcıların, Microsoft Account kullanıcılarını Hizmet Yöneticisi olarak eklemeye devam edebilirsiniz.

  | Oturum açma yöntemi | Microsoft Account user Hizmet Yöneticisi olarak eklensin mi? | İş veya Okul hesabı aynı kuruluşta Hizmet Yöneticisi olarak eklensin mi? | İş veya Okul hesabı, farklı bir kuruluşun Hizmet Yöneticisi olarak eklensin mi? |
  | --- | --- | --- | --- |
  |  Microsoft Hesabı |Evet |Hayır |Hayır |
  |  İş veya Okul hesabı |Evet |Evet |Hayır |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Bir Azure aboneliğine yönelik hesap yöneticisi değiştirme

Hesap Yöneticisi başlangıçta Azure aboneliğine kaydolduğu ve faturalandırma abonelik sahibi olarak sorumlu olan kullanıcıdır. Bir aboneliğin Hesap Yöneticisi değiştirmek için bkz [bir Azure aboneliğinin sahipliğini başka bir hesaba](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Emin değil hesap yöneticisi olan?** Şu adımları uygulayın:

1. Ziyaret [ **abonelikleri** Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Kontrol edin ve ardından altına bakın istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Kaynak erişim denetimi ve Active Directory hakkında daha fazla bilgi edinin

* RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
* Azure'da tüm rolleri hakkında daha fazla bilgi edinmek için [farklı rolleri Azure anlamak](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Azure Active Directory hakkında daha fazla bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../active-directory/active-directory-how-subscriptions-associated-directory.md) ve [Azure Active Directory'de yönetici rolleri atama](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
