---
title: Azure Active Directory kullanarak bir grubu silme işlemini | Microsoft Docs
description: Azure Active Directory kullanarak bir grup silmeyi öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 014fe487d23a6c75e94ca2708ed15044bd6cf53b
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574343"
---
# <a name="how-to-delete-a-group-using-azure-active-directory"></a>Nasıl yapılır: Azure Active Directory'yi kullanarak bir grubu silme
Birkaç nedenden için bir grubu silebilirsiniz ancak genellikle çünkü olur:

- Hatalı **grup türü** yanlış seçeneği

- Kimlik bilgileri yanlış veya yinelenen bir grup yanlışlıkla oluşturdum 

- Artık gerek Grup

## <a name="to-delete-a-group"></a>Bir grubu silmek için
1. Oturum [Azure portalında](https://portal.azure.com) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

3. Gelen **gruplar - tüm gruplar** sayfasında aramak ve silmek istediğiniz grubu seçin. Bu adımlar için kullanacağız **MDM İlkesi - Doğu**.

    ![Tüm grupları grupları sayfasında grubu adı vurgulanmış](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Üzerinde **MDM İlkesi - Doğu genel bakış** sayfasında ve ardından **Sil**.

    Grubu Azure Active Directory kiracınızdan silindi.

    ![MDM İlkesi - Doğu genel bakış sayfasında, Sil seçeneği vurgulanmış](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir grubu yanlışlıkla silerseniz, yeniden oluşturabilirsiniz. Daha fazla bilgi için [temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

- Bir Office 365 grubunu yanlışlıkla silerseniz, bu geri yüklenmesi mümkün olabilir. Daha fazla bilgi için [silinen bir Office 365 grubunu geri](../users-groups-roles/groups-restore-deleted.md).
