---
title: Hızlı Başlangıç - Azure portalını kullanarak bir kullanıcıya atanmış görünümü rolleri | Microsoft Docs
description: Bir kullanıcı, Grup, hizmet sorumlusu veya Azure portalını kullanarak yönetilen kimlik için atanan rol tabanlı erişim denetimi (RBAC) izinleri görüntülemeyi öğrenin.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b755dd6223c21084cafea82a1c8857f9f54b03b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52638654"
---
# <a name="quickstart-view-roles-assigned-to-a-user-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir kullanıcıya atanan rollerin görüntüleyin

Kullanabileceğiniz **erişim denetimi (IAM)** dikey penceresinde [rol tabanlı erişim denetimi (RBAC)](overview.md) birden çok kullanıcı için rol atamalarını görüntülemek için gruplar, hizmet sorumluları ve yönetilen kimlikleri, ancak bazen yalnızca tek bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik için rol atamalarını hızla görüntülemeniz gerekir. Bunu yapmanın en kolay yolu kullanmaktır **denetleyin erişim** Azure portalında özelliği.

## <a name="view-role-assignments"></a>Rol atamalarını görüntüle

Tek bir kullanıcı, Grup, hizmet sorumlusu veya abonelik kapsamında yönetilen kimlik için rol atamalarını görüntülemek için aşağıdaki adımları izleyin.

1. Azure portalında **tüm hizmetleri** ardından **abonelikleri**.

1. Aboneliğinize tıklayın.

1. Tıklayın **erişim denetimi (IAM)**.

1. Tıklayın **denetleyin erişim** sekmesi.

    ![Erişim denetimi - onay erişim sekmesi](./media/check-access/access-control-check-access.png)

1. İçinde **Bul** listesinde, istediğiniz erişimi denetlemek için güvenlik sorumlusu türü seçin.

1. Arama kutusunda görünen adları, e-posta adresi veya nesne tanımlayıcıları dizinini aramak için bir dize girin.

    ![Seçim listesine erişimi denetleyin](./media/check-access/check-access-select.png)

1. Açmak için güvenlik sorumlusunu **atamaları** bölmesi.

    ![Atamalar bölmesinin](./media/check-access/check-access-assignments.png)

    Bu bölmede, seçili güvenlik sorumlusu ve kapsamına atanan rollerini görebilirsiniz. Varsa tüm atamalarını bu kapsamda reddetme ya da bu kapsama devralınan, bunlar listelenir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure portalını kullanarak bir kullanıcı için erişim verin.](quickstart-assign-role-user-portal.md)
