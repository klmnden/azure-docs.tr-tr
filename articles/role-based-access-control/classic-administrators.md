---
title: Azure Klasik abonelik yöneticileri | Microsoft Docs
description: Azure ortak yönetici ve hizmet yöneticisi rollerini ekleme veya değiştirme yapma ve Hesap Yöneticisi'ni görüntülemek nasıl açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/19/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9c3bd2480853f5c4134cd560c20a6007b044e138
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64924644"
---
# <a name="azure-classic-subscription-administrators"></a>Azure Klasik abonelik yöneticileri

Microsoft, rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetme önerir. Ancak, yine de klasik dağıtım modelini kullanıyorsanız, bir Klasik Abonelik Yöneticisi rolü kullanmak gerekir: Hizmet Yöneticisi ve ortak yönetici. Daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md).

Bu makalede, ortak yönetici ve hizmet yöneticisi rollerini ekleme veya değiştirme yapma ve Hesap Yöneticisi görüntüleme açıklanmaktadır.

## <a name="add-a-co-administrator"></a>Ortak yönetici Ekle

> [!TIP]
> Yalnızca Azure Klasik dağıtımlar kullanarak yönetmek kullanıcının erişmesi gerekiyorsa, ortak yönetici eklemek gereken [Azure Hizmet Yönetimi PowerShell Modülü](https://docs.microsoft.com/powershell/module/servicemanagement/azure). Kullanıcı, Azure portalında Klasik kaynakları yönetmek için yalnızca kullanıyorsa, Klasik yönetici kullanıcının eklemek gerekmez.

1. Oturum [Azure portalında](https://portal.azure.com) bir Hizmet Yöneticisi olarak.

1. Açık [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ve bir abonelik seçin.

    Ortak Yöneticiler abonelik kapsamında yalnızca atanabilir.

1. Tıklayın **erişim denetimi (IAM)** .

1. Tıklayın **Klasik yöneticileri** sekmesi.

    ![Klasik yöneticileri açılan ekran görüntüsü](./media/classic-administrators/classic-administrators.png)

1. Tıklayın **Ekle** > **ortak yönetici ekleme** Ekle ortak Yöneticiler bölmesini açmak için.

    Ortak yönetici Ekle seçeneğini devre dışı bırakılırsa, izinlere sahip değilsiniz.

1. ' I tıklatın ve eklemek istediğiniz kullanıcıyı seçin **Ekle**.

    ![Ortak yönetici ekler ekran görüntüsü](./media/classic-administrators/add-coadmin.png)

### <a name="adding-a-guest-user-as-a-co-administrator"></a>Ortak yönetici olarak bir Konuk kullanıcı ekleme

[Konuk kullanıcıları](../active-directory/b2b/b2b-quickstart-add-guest-users-portal.md) atanan ortak Yönetici rolü, ortak Yönetici rolüne üye kullanıcılarla karşılaştırıldığında bazı farklılıklar görebilirsiniz. Aşağıdaki senaryoyu göz önünde bulundurun:

- Kullanıcı A'ile bir Azure AD iş veya Okul hesabı bir Azure aboneliğine yönelik Hizmet Yöneticisi ' dir.
- B kullanıcısı, bir Microsoft hesabı vardır.
- Kullanıcı A, b kullanıcısına ortak Yönetici rolüne atar
- B kullanıcısı neredeyse her şeyi yapabilirsiniz, ancak uygulamaları veya Azure AD dizinindeki kullanıcıların arayın.

Kullanıcının B her şeyi yönetmenizi beklenir. Bu farkın nedeni, Microsoft hesabı yerine bir üye kullanıcı Konuk kullanıcı aboneliğe eklenir. Konuk kullanıcıların karşılaştırıldığında üye kullanıcıları Azure AD'de farklı varsayılan izinleri vardır. Örneğin, üye kullanıcılar diğer kullanıcılar Azure AD'de okuyabilir ve Konuk kullanıcılar olamaz. Üye kullanıcılar Azure AD'de yeni hizmet sorumluları kaydedebilir ve Konuk kullanıcılar olamaz. Konuk kullanıcı, bu görevleri gerçekleştirmek gerekiyorsa, söz konusu atamak için olası bir çözüm olan Azure AD yönetici rollerini Konuk kullanıcının olmalıdır. Örneğin, önceki senaryoda atayabilirsiniz [dizin okuyucular](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) diğer kullanıcıların okuma ve atamak için rol [uygulama geliştiricisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) hizmet sorumluları oluşturabilmek için rol. Üye ve Konuk kullanıcıları ve izinlerini hakkında daha fazla bilgi için bkz: [varsayılan kullanıcı izinleri Azure Active Directory nelerdir?](../active-directory/fundamentals/users-default-permissions.md).

Unutmayın [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md) farklıdır [Azure AD yönetici rollerini](../active-directory/users-groups-roles/directory-assign-admin-roles.md). Yerleşik roller Azure AD'ye hiçbir erişim izni yok. Daha fazla bilgi için [farklı rollerin anladığınızdan](../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="remove-a-co-administrator"></a>Ortak yöneticiyi Kaldır

1. Oturum [Azure portalında](https://portal.azure.com) bir Hizmet Yöneticisi olarak.

1. Açık [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ve bir abonelik seçin.

1. Tıklayın **erişim denetimi (IAM)** .

1. Tıklayın **Klasik yöneticileri** sekmesi.

1. Kaldırmak istediğiniz ortak yönetici yanında bir onay işareti ekleyin.

1. Tıklayın **Kaldır**.

1. Görüntülenen ileti kutusunda **Evet**.

    ![Ortak yönetici kaldırır ekran görüntüsü](./media/classic-administrators/remove-coadmin.png)

## <a name="change-the-service-administrator"></a>Hizmet Yöneticisini değiştirme

Yalnızca Hesap Yöneticisi, bir aboneliği için Hizmet Yöneticisi olarak değiştirebilirsiniz. Bir Azure aboneliği için oturum açtığınızda varsayılan olarak, Hizmet Yöneticisi olarak Hesap Yöneticisi aynıdır. Kullanıcı hesabı yönetici rolüne sahip Azure portalına erişim sağlayamıyor. Hizmet Yöneticisi rolüne sahip kullanıcının, Azure portalında tam erişimi vardır. Hizmet Yöneticisi ve hesap yöneticiliği aynı kullanıcı ve farklı bir kullanıcı olarak hizmet yöneticisini değiştiremez, Hesap Yöneticisi Azure portalına erişim kaybeder. Ancak, Hesap Yöneticisi hizmet Yöneticisi geri kendilerini değiştirmek için her zaman hesap merkezi kullanabilirsiniz.

Hizmet Yöneticisi’ni değiştirmenin iki yolu vardır. Değiştirebileceğiniz **Azure portalında** veya **hesap Merkezi**.

### <a name="azure-portal"></a>Azure portal

1. Hizmet yöneticileri değiştirmek için sınırlamalar denetleyerek senaryonuz desteklenen emin olun.

1. Oturum [Azure portalında](https://portal.azure.com) hesap yöneticisi olarak.

1. Açık [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ve bir abonelik seçin.

1. **Özellikler**'e tıklayın.

    ![Hesap Yöneticisi gösteren ekran görüntüsü](./media/classic-administrators/account-admin.png)

1. En üstünde tıklayın **Hizmet Yöneticisi** Hizmet Yöneticisi bölmesini açmak için.

    Hizmet Yöneticisi düğmesi devre dışıysa, izinlere sahip değilsiniz. Hesap Yöneticisi olan kullanıcı, Hizmet Yöneticisi değiştirebilir.

1. Yeni bir hizmet yöneticisini seçin ve ardından **Kaydet**.

### <a name="account-center"></a>Hesap Merkezi

1. Hizmet yöneticileri değiştirmek için sınırlamalar denetleyerek senaryonuz desteklenen emin olun.

1. Oturum [hesap Merkezi](https://account.windowsazure.com/subscriptions) hesap yöneticisi olarak.

1. Bir aboneliğe tıklayın.

1. Sağ taraftaki **Abonelik Ayrıntıları Düzenle**.

    ![Hesap Merkezi'nde abonelik Düzenle düğmesini gösteren ekran görüntüsü](./media/classic-administrators/editsub.png)

1. İçinde **Hizmet Yöneticisi** kutusuna, yeni Hizmet Yöneticisi e-posta adresi girin.

    ![Hizmet Yöneticisi e-posta değiştirmek için kutusunu gösteren ekran görüntüsü](./media/classic-administrators/change-service-admin.png)

1. Değişikliği kaydetmek için onay işaretine tıklayın.

### <a name="limitations-for-changing-the-service-administrator"></a>Hizmet Yöneticisi değiştirmek için kısıtlamalar

Her abonelik bir Azure AD dizini ile ilişkilidir. Aboneliği ile ilişkili dizini bulunacak açın **abonelikleri** Azure portalı ve dizin görmek için bir abonelik seçin.

Bir iş veya Okul hesabıyla oturum açtıysanız, Hizmet Yöneticisi olarak, kuruluşunuzdaki diğer hesapları ekleyebilirsiniz. Örneğin, abby@contoso.com ekleyebilirsiniz bob@contoso.com Hizmet Yöneticisi olarak eklenemez ancak john@notcontoso.com sürece john@notcontoso.com contoso.com dizinde faaliyet. Kullanıcılar imzalı iş veya Okul hesapları, Microsoft hesabı kullanıcılarını Hizmet Yöneticisi olarak eklemeye devam edebilirsiniz.

  | Oturum açma yöntemi | Microsoft hesabı kullanıcı, Hizmet Yöneticisi olarak eklensin mi? | İş veya Okul hesabı aynı kuruluşta bir Hizmet Yöneticisi olarak eklensin mi? | İş veya Okul hesabı, farklı kuruluştaki bir Hizmet Yöneticisi olarak eklensin mi? |
  | --- | --- | --- | --- |
  |  Microsoft hesabı |Evet |Hayır |Hayır |
  |  İş veya Okul hesabı |Evet |Evet |Hayır |

## <a name="view-the-account-administrator"></a>Hesap Yöneticisi görüntüleyebilir

Hesap Yöneticisi başlangıçta Azure aboneliğine kaydolduğu ve faturalandırma abonelik sahibi olarak sorumlu olan kullanıcıdır. Bir aboneliğin Hesap Yöneticisi değiştirmek için bkz [bir Azure aboneliğinin sahipliğini başka bir hesaba](../billing/billing-subscription-transfer.md).

Hesap Yöneticisi'ni görüntülemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Açık [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ve bir abonelik seçin.

1. **Özellikler**'e tıklayın.

    Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.

    ![Hesap Yöneticisi gösteren ekran görüntüsü](./media/classic-administrators/account-admin.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'daki farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](../role-based-access-control/role-assignments-portal.md)
* [Azure aboneliği yöneticileri ekleme veya değiştirme](../billing/billing-add-change-azure-subscription-administrator.md)
