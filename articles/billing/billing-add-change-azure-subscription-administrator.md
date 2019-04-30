---
title: Ekleme veya Azure aboneliği yöneticileri değiştirme | Microsoft Docs
description: Ekleme veya değiştirme rol tabanlı erişim denetimi (RBAC) kullanarak bir Azure aboneliği Yöneticisi açıklar.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 6cc965f8e775e02e9dec9f610516739a9a2c1936
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127726"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Ekleme veya Azure aboneliği yöneticileri değiştirme

Azure kaynaklarına erişimi yönetmek için uygun yönetici rolüne sahip olmanız gerekir. Azure, aralarından seçim yapabileceğiniz birkaç yerleşik rol ile rol tabanlı erişim denetimi (RBAC) olarak adlandırılan bir yetkilendirme sistemine sahiptir. Yönetim grubu, abonelik veya kaynak grubu gibi farklı kapsamlarda bu roller atayabilirsiniz.

Microsoft, RBAC kullanarak kaynaklara erişimi yönetme önerir. Bununla birlikte, yine de klasik dağıtım modelini kullanarak ve kullanarak Klasik kaynakları yönetmek [Azure Hizmet Yönetimi PowerShell Modülü](https://docs.microsoft.com/en-us/powershell/module/servicemanagement/azure), Klasik yönetici kullanmanız gerekecektir. 

> [!TIP]
> Yalnızca klasik kaynakları yönetmek için Azure portalını kullanıyorsanız, Klasik yönetici kullanmanız gerekmez.

Daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md) ve [Azure Klasik abonelik yöneticileri](../role-based-access-control/classic-administrators.md).

Bu makalede nasıl veya abonelik kapsamında RBAC kullanarak bir kullanıcı için Yönetici rolü değiştirin.

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Bir abonelik Yöneticisi olarak kullanıcı atama

Bir kullanıcının bir Azure aboneliğinin bir yöneticisi olmak için bunları atayın [sahibi](../role-based-access-control/built-in-roles.md#owner) abonelik kapsamında rol (RBAC rolü). Sahip rolü, diğerleri erişim hakkı dahil olmak üzere, Abonelikteki tüm kaynaklara kullanıcı tam erişim sağlar. Bu adımları herhangi bir rol ataması ile aynıdır.

1. Azure portalında açın [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Erişim vermek istediğiniz aboneliğe tıklayın.

1. Tıklayın **erişim denetimi (IAM)**.

1. Tıklayın **rol atamaları** Bu abonelik için tüm rol atamalarını görüntülemek için sekmesinde.

    ![Rol atamaları gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/role-assignments.png)

1. Tıklayın **Ekle** > **rol ataması Ekle** açmak için **rol ataması Ekle** bölmesi.

    Rol atama izinleri yoksa, seçenek devre dışı bırakılır.

1. İçinde **rol** aşağı açılan listesinden **sahibi** rol.

1. İçinde **seçin** listesinde, bir kullanıcı seçin. Kullanıcı listede görmüyorsanız, yazabilirsiniz **seçin** kutusunu dizin görünen adları için arama yapın ve e-posta adresleri.

    ![Seçili sahip rolü gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-role.png)

1. Tıklayın **Kaydet** rol atamak için.

    Birkaç dakika sonra kullanıcıya abonelik kapsamında bir sahip rolü atanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
* [Azure'daki farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Nasıl yapılır: Azure Active Directory’ye bir Azure aboneliğini ekleme veya ilişkilendirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Azure Active Directory'deki yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
