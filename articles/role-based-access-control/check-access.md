---
title: Hızlı Başlangıç - görünümü kullanıcı erişimi olan Azure kaynaklarına | Microsoft Docs
description: Bir kullanıcı veya diğer güvenlik sorumlusu, rol tabanlı erişim denetimi (RBAC) ve Azure portalını kullanarak Azure kaynaklarına sahip erişimi görüntülemeyi öğrenin.
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
ms.openlocfilehash: f388215b2829066906ee7faf41abb17307bf3fff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60782348"
---
# <a name="quickstart-view-the-access-a-user-has-to-azure-resources"></a>Hızlı Başlangıç: Azure kaynaklarına kullanıcının erişimi görüntüleme

Kullanabileceğiniz **erişim denetimi (IAM)** dikey penceresinde [rol tabanlı erişim denetimi (RBAC)](overview.md) bir kullanıcı veya başka bir güvenlik sorumlusu sahip olduğu Azure kaynaklarına erişim görüntülemek için. Ancak, bazen hızlı erişim için tek bir kullanıcı veya başka bir güvenlik sorumlusu görüntülemek yeterlidir. Bunu yapmanın en kolay yolu kullanmaktır **denetleyin erişim** Azure portalında özelliği.

## <a name="view-role-assignments"></a>Rol atamalarını görüntüle

 Bir kullanıcı için erişim görüntüleme biçiminizi rol atamaları listelemektir. Tek bir kullanıcı, Grup, hizmet sorumlusu veya abonelik kapsamında yönetilen kimlik için rol atamalarını görüntülemek için aşağıdaki adımları izleyin.

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
> [Öğretici: RBAC ve Azure portalını kullanarak Azure kaynaklarına kullanıcı erişimi](quickstart-assign-role-user-portal.md)
