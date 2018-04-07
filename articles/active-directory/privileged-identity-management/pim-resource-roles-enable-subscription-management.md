---
title: Privileged Identity Management Azure kaynaklarını - abonelik yönetimini etkinleştirmeniz için | Microsoft Docs
description: Genel Yöneticiler Kiracı Aboneliklerini yönetmek hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/27/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4c6ae3da34fe5157314b8ea422591f7ecbd2a667
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="enable-subscription-management"></a>Abonelik yönetimini etkinleştirme

Dizininizin genel Yöneticisi olarak, tüm abonelik kaynaklarına varsayılan erişim kiracınızda olmayabilir. Bu makalede kendiniz kiracınız ve erişim aldıktan sonra kuruluşunuza tüm güvenlik denetimleri ile uyumlu kalan bir önerilen yaklaşım tüm abonelikleri erişmenizi adımlarını özetler.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Kimin Dizinim Aboneliklerde yönetim etkinleştirebilir?

Genel yönetici rolüne atanan her bir kullanıcı abonelik yönetimini etkinleştirmek için aşağıdaki adımları izlemelisiniz. Kendiniz için abonelik yönetimi etkinleştirdikten sonra kaynak erişimi gerekebilir başka genel Yöneticiler ekleyebilirsiniz. Genel yönetici rolüne tüm üyeleri için erişim sağlayan hiçbir dizin ayarı yoktur.

## <a name="log-on-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalına genel Yönetici rolüne uygun veya etkin bir üyesi olan bir hesapla oturum açarak başlar. Uygun bir genel yönetici hesabı ise, sonraki adıma geçmeden önce ilk olarak rolü etkinleştirmeniz gerekir.

## <a name="access-the-azure-ad-admin-center"></a>Azure AD Yönetim Merkezi'ne erişme

Azure portalına genel yönetici olarak oturum artık Azure abonelikleri erişim sağlayacak mı ayarları düzenleyebilirsiniz. Azure AD Yönetim Merkezi'ne gidin, bulun ve sol gezinti bölmesinde Özellikler sekmesini seçin.

![](media/azure-pim-resource-rbac/aad_properties.png)

Özellikler listesinde "Genel yönetici Azure Aboneliklerini yönetmek" başlıklı "Evet" olarak değiştirin.

![](media/azure-pim-resource-rbac/aad_properties_save.png)

## <a name="navigate-to-azure-ad-pim"></a>Azure AD PIM gidin

Bu seçeneği etkinleştirdiğinizde, hesabınız için her Kiracı aboneliği kaynağında "Kullanıcı erişimi Yöneticisi" rolüne otomatik olarak eklenir. Buradan, Azure AD PIM gidin ve sol gezinti bölmesinin Yönet bölümünde Azure kaynaklarını seçin.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Kaynakları bulmak ve yönetme

Kuruluşunuz Azure Active Directory yöneticileri korumak üzere zaten Azure AD PIM kullanıyorsa, dikey yüklediğinde Aboneliklerin listesini görürsünüz.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Herhangi bir kaynağa görmüyorsanız denetleyin:
>- Genel yönetici rolü'nüz süresi yok 
>- Kuruluşunuzda bir Azure aboneliğiniz varsa

![](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Atamaları yapılandırın

Listeden bir abonelik seçin ve uygun bir kaynağın sahibi olarak kendiniz (veya bir üyesi olan bir grup) atayın. 
[Rol atama hakkında daha fazla okuma](pim-resource-roles-assign-roles.md).

Sonraki adıma geçmeden önce her bir kaynak için bu işlemi yineleyin.

## <a name="clean-up-standing-access"></a>Durumu erişimi Temizle

Önemli abonelikler için uygun atamaları, kuruluşunuzda sahip olduğunuza göre dizin özelliklerini seçeneği devre dışı bırakarak durumu erişimi temizleyebilirsiniz:

![](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Sonraki adımlar

[Kaynakları bulmak](pim-resource-roles-discover-resources.md)

[Rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)








