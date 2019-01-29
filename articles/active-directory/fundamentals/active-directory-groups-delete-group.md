---
title: -Azure Active Directory grup silme | Microsoft Docs
description: Azure Active Directory'yi kullanarak grup silme hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.openlocfilehash: 61f6d68e873b6c8b8b901e5e065065ed42513572
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55100221"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak bir grubu silme
Bir Azure Active Directory (Azure AD) grubu için birkaç nedenden silebilirsiniz ancak genellikle çünkü olur:

- Hatalı **grup türü** yanlış seçeneği için.

- Kimlik bilgileri yanlış veya yinelenen bir grup yanlışlıkla oluşturdum. 

- Artık grubu gerekir.

## <a name="to-delete-a-group"></a>Bir grubu silmek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

3. Gelen **gruplar - tüm gruplar** sayfasında aramak ve silmek istediğiniz grubu seçin. Bu adımlar için kullanacağız **MDM İlkesi - Doğu**.

    ![Tüm grupları grupları sayfasında grubu adı vurgulanmış](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Üzerinde **MDM İlkesi - Doğu genel bakış** sayfasında ve ardından **Sil**.

    Grubu Azure Active Directory kiracınızdan silindi.

    ![MDM İlkesi - Doğu genel bakış sayfasında, Sil seçeneği vurgulanmış](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir grubu yanlışlıkla silerseniz, yeniden oluşturabilirsiniz. Daha fazla bilgi için [temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

- Bir Office 365 grubunu yanlışlıkla silerseniz, bu geri yüklenmesi mümkün olabilir. Daha fazla bilgi için [silinen bir Office 365 grubunu geri](../users-groups-roles/groups-restore-deleted.md).
