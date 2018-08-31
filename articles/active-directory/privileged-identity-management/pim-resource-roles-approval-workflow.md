---
title: Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri | Microsoft Docs
description: Onaylayın veya reddedin Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için istekleri öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e5cda31af5ac95e7ebe2b858b1d7355ea3f2a6bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189813"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri

Onaylayın veya bir isteği reddetmek için onaylayan listesinin bir üyesi olmalıdır. 

1. PIM içinde seçin **istekleri onaylama** sol menüde sekmesinden ve istek bulun.

   !["İstekleri onaylama" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. İstek, bir kullanıcının bir gerekçe kararı seçip **Onayla** veya **Reddet**. İstek ardından çözülür.

   ![Seçilen istek ayrıntılı bilgileri](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>İş akışı bildirimlerini

İş akışı bildirimlerini hakkında bazı bilgiler şunlardır:

- Bir rol için bir isteği, gözden geçirme olduğunda tüm üyelerinin onaylayan listesini e-posta ile bildirilir. E-posta bildirimleri onaylayan burada onaylama veya reddetme isteğini doğrudan bir bağlantı içerir.
- İstekleri onaylar veya reddeder listenin ilk üyesi tarafından çözümlenir. 
- Bir onaylayan isteği yanıtladığında onaylayan listenin tüm üyelerini eylemi bildirilir. 
- Onaylanan bir üyenin içindeki rollerine etkin olduğunda, kaynak yöneticileri bildirilir. 

>[!Note]
>Onaylanan bir üyenin etkin olmamalıdır düşündüğü bir kaynak yöneticisi PIM'de etkin bir rol atamasını kaldırabilir. Onaylayan listesini üyesi olduğu sürece kaynak yöneticileri, bekleyen istek bildirilmez olsa da, görüntüleyebilir ve bekleyen istekler PIM görüntüleyerek bekleyen tüm kullanıcıların istekleri iptal et. 

## <a name="next-steps"></a>Sonraki adımlar

- [Onaylayın veya Azure AD dizin rollerini PIM isteklerini reddedin](azure-ad-pim-approval-workflow.md)
- [PIM e-posta bildirimleri](pim-email-notifications.md)
